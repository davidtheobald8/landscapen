 # LandSCaPeN v1.1-beta, November 4, 2024.
A Google Earth Engine toolbox to support analyses of landscape structure, composition, process, connectivity, and networks.
 Please cite as: Theobald, David M. 2023. *LandSCaPeN v1.0: A Google Earth Engine toolbox 
 to support analyses of landscape structure, composition, process, connectivity, and networks.* [www.davidmtheobald.com](https:davidmtheobald.com).
 Licensed under CC BY-SA 4.0
 To load example scripts that illustrate various tools, go to: https://code.earthengine.google.com/?accept_repo=users/DavidTheobald8/libs

 The tools are organized into landscape [composition](#comp), [structure](#stru), [process](#proc), [networks](#netw), [utilities](#util), and [summaries](#sum).
 To call LandSCaPeN functions, first load the module into your script through the *require()* function, and then call the function
 using *lse*.
 var lse = require('users/DavidTheobald8/libs:lse')
 For example:
 ```javascript
 var lse = require('users/DavidTheobald8/libs:lse')
 // from the LandSCaPeN library (module), call a function
 var compositionSummary = lse.composition(fcPolys, propertyGroup, propertyValue, precision)
 ```
 Please join the Google Group for LandSCaPeN for announcements, updates and community help at: https://groups.google.com/g/gee-landscapen
 Also note there a number of worked examples are provided to facilitate learning of landscape ecology concepts and the tools provided: https://code.earthengine.google.com/?accept_repo=users/DavidTheobald8/libs

 Technical notes:
 + parameters to functions must be in proper order -- dictionary format (using {}) is *not* supported.
 ```mermaid
   graph LR;
       A(LandSCaPeN)
       A-->B;
       A-->C1;
       A-->C2;
       B(Ecosystem attributes)
       B-->B1;
       B-->B2;
       B-->B3;
       B-->B4;
       B1(Composition)
       B1-->B11
       B1-->B12
       B1-->B13
       B11(FC summary)
 %%      click B11 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.compositionFC()"
       B12(Image summary)
 %%      click B12 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.compositionImage()"
       B13(FC stats)
 %%      click B13 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.compositionStats()"
       B2(Structure)
       B21(Complexity)
       B2-->B21
 %%      click B21 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralComplexity()"
       B22(Distance)
       B2-->B22
 %%      click B22 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralDistance()"
       B23(Proximity)
       B2-->B23
 %%      click B23 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralProximity()"
       B3(Process)
       B31(Connectivity)
       B3-->B31
 %%      click B31 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivity()"
       B32(Connectivity all)
       B3-->B32
       B3-->B34
 %%      click B34 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivityAll()"
       B4(Network)
       C1(Summarize)
       C11(Components)
       C1-->C11
       C12(Groups)
       C1-->C12
       C13(Samples)
       C1-->C13
       C14(Regions)
       C1-->C14
       C2(Utilities)
       C21(Add shape properties)
       C2-->C21
       C22(Rank values)
       C2-->C22
%%      click C2 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.rankValues()%20add%20shape%20properties"
 ```
 ## <a name="comp"></a> Composition tools
 ### lse.compositionFC(fc, propertyGroup, propertyValue, precision)
 Summarizes the values of a given property (*propertyValue*) for features from a Feature Collection using a property (*propertyGroup*) that contains nominal/class data.
+ *fc*: feature collection, ee.FeatureCollection()
+ *propertyGroup*: name of the "group" property (aka class) in *fc* to summarize on, ee.String()
+ *propertyValue*: name of the "value" property in *fc* that describes the values to summarize. Must contain numerical values, ee.String()
+ *precision*: number of decimal places to report summarized value, defaults to 2, ee.Number()
+ returns: a feature collection with summarized statistics for each unique value in *propertyGroup*, and can be exported as a CSV in the Tasks tab.
 Results are approximately equivalent to the Class Area Proportion and Patch Richness metrics ([Leitao et al. 2006](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Measuring+Landscapes&btnG=)).
 ### lse.compositionFCStats(fc, propertyGroup, propertyValue, propertyWeight)
 Summarizes an attribute (*propertyValue*) for features from a Feature Collection using a property (*propertyGroup*) that contains nominal/class data.
+ *fc*: feature collection with polygons, ee.FeatureCollection()
+ *propertyGroup*: name of the "class" property in *fc* to summarize on, ee.String()
+ *propertyValue*: name of the "value" property in *fc* that describes the values to summarize. Must contain numerical values, ee.String()
+ *propertyWeight*: name of the property in *fc* used to calculate weighted statistics. Defaults to the *propertyValue*. Must contain numerical values, ee.String()
+ returns: a feature collection with summarized statistics for each unique value in *propertyGroup* to Export.table.toDrive()
 Note: when quantifying a summary measure of patch size, it is recommended to use the "meanQ" (mean quadratic) or "meanW" (mean weighted) statistic (rather than arithmetic "mean" or "median"), which is known as Weighted Mean Patch Size ([Li and Archer 1997](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=weighted+mean+patch+size+li+and+archer&btnG=&oq=weight)).
 Compositional statistics are also known as Patch Richness and Class Area Proportion ([Leitao et al. 2006](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Measuring+Landscapes&btnG=)).
 ### lse.compositionImage(image, resolution, regions, printToConsole)
 Calculates the area of classes for an image (raster), assuming image contains nominal/class image values.
 These are also known as Class Area Proportion and Patch Richness ([Leitao et al. 2006](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Measuring+Landscapes&btnG=)).
 Future plans to include summarizes of patches in each class.
 Thanks to the blog post: https://spatialthoughts.com/2020/06/19/calculating-area-gee/
+ *image*: image with integer values representing nominal values, type = ee.Image()
+ *resolution*: size of cells in meters used for sampling image, defaults to 1000 m, type ee.Number()
+ *regions*: features to calculate, type ee.FeatureCollection()
+ *printToConsole*: a flag to prevent printing to console when false, but exports CSV file, boolean (default is false, does NOT print)
+ returns: a feature collection with summarized statistics for each class for Export.table.toDrive, ee.FeatureCollection().
 ## <a name="stru"></a> Structural tools
 ### lse.structuralComplexity(values, lstRadii, kernelType, region, resolution, edgeOnly, printToConsole)
 This tool characterizes the structure of landscape patches, including edge complexity, represented as nominal classes. Area-weighting is not implemented currently.
 Masked (no data) values are recognized.
+ *values*: integer values represent groups (classes), ee.Image()
+ *lstRadii*: a list of radii of moving window (kernel) in number of pixels, ee.Number
+ *kernelType*: the type of kernel to use to calculate complexity: "circle", "square", ee.String()
+ *region*: the analytical area used to calculate, ee.Geometry()
+ *resolution*: the size of pixels in meters, ee.Number()
+ *edgeOnly*: use only the edge pixels and exclude the interior cells, ee.Boolean()
+ *printToConsole*: a flag to prevent printing to console when false, but exports CSV file, boolean (default is false, does NOT print)
returns: image with the mean number of different groups (classes) within the radii
 ### lse.structuralDistance(patches, maxPixels, printToConsole)
 This function characterizes the structure and configuration among patches
 by measuring Euclidean distance outside a patch (from the patch edge) to other nearby patches.
 Note that the pixel values returned are expressed as number of pixels to closest "patch" pixel,
 so to get meters you must multiply times the resolution.
 Patch pixels are defined by any non-zero value and matrix pixels should equal 0.
 Also, by inverting the patches (i.e. patches become the matrix, the matrix becomes the "patches")
 provides a structural measure of the shape and size of patches as between patches (landscape level), following [Theobald (2003)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Theobald%2C+DM+GIS+Concepts+and+ARCGIS+Methods&btnG=)
+ *patches*: a binary representation of habitat (0=matrix, >0 is "patch"), type: ee.Image()
+ *maxPixels*: the maximum number of pixels to calculate distance from the nearest patch, by default set to 1024, ee.Number()
+ *printToConsole*: a flag to prevent printing to console when false, but exports CSV file, boolean (default is false, does NOT print)
returns: image with two bands: "dInto", the distance into patch ("core") and "dFrom", away from (into "matrix")'
+ Note: this is a broader application based on the "GISFrag" metric: [Ripple et al. (1991)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=Measuring+forest+landscape+patterns+in+the+cascade+range+of+Oregon%2C+USA&btnG=).
 ### lse.structuralDistanceFDT(patches, maxPixels, printToConsole)
 This function uses FastDistanceTransform to characterizes the structure and configuration among patches
 by measuring Euclidean distance outside a patch (from the patch edge) to other nearby patches.
 Note that the pixel values returned are expressed as number of pixels to closest "patch" pixel,
 so to get meters you must multiply times the resolution.
 Patch pixels are defined by any non-zero value and matrix pixels should equal 0.
 Also, by inverting the patches (i.e. patches become the matrix, the matrix becomes the "patches")
 provides a structural measure of the shape and size of patches as between patches (landscape level), following [Theobald (2003)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Theobald%2C+DM+GIS+Concepts+and+ARCGIS+Methods&btnG=)
+ *patches*: a binary representation of habitat (0=matrix, >0 is "patch"), type: ee.Image()
+ *maxPixels*: the maximum number of pixels to calculate distance from the nearest patch, by default set to 1024, ee.Number()
+ *printToConsole*: a flag to prevent printing to console when false, but exports CSV file, boolean (default is false, does NOT print)
returns: image with distance into patch ("core") and away from (into "matrix")'
+ Note: this is a broader application based on the "GISFrag" metric: [Ripple et al. (1991)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=Measuring+forest+landscape+patterns+in+the+cascade+range+of+Oregon%2C+USA&btnG=).
 ### lse.structuralProximity(values, kernel, statistic, lstRadii )
 Calculates proximity using a moving window analysis, often considered to be a structural connectivity metric and are ocassionally called "intactness" or "integrity".
+ If the radius of the kernel exceeds 31 pixels, then the values are upscaled by a factor of 2 (e.g., 30 m is reprojected (using EPSG:4326) to 60 m, and so on.)
+ *values*: the values in an image to be analyzed, treated as real data type, ee.Image().
+ *kernel*: the desired kernel type. Currently supports: 'circle', 'gaussian', 'rectangle' (if multiple radii then xRadius = yRadius), 'square' windows); type ee.String().
+ *statistic*: used to summarize values within the moving window (essentially the 'reducer'): "mean" (default), "max", "min", 'meanQ', "sum", "count"; type ee.String().
+ *printToConsole*: a flag to prevent printing to console when false, but exports CSV file, boolean (default is false, does NOT print)
+ returns the proximity image, ee.Image() with float type.
 ### lse.structuralFragmentation(values, kernel, statistic, lstRadii )
 Calculate core-edge-periphery-isolated patches, for estimating fragmentation patterns, e.g. for forests.
 The general approach is as follows:
   1. identify forest;
   2. find "cores" -- in the interior within *edgeDistance* from edge;
   3. find the edge and periphery surrounding cores, within distance maxPeripheryDistance
+ *values*: the values in an image to analyze the pattern of fragmentation, typically binary (e.g., forest/non-forest), ee.Image().
+ *edgeDistance*: the desired distance from the edge; type ee.Number().
+ *maxPeripheryDistance*: the desired distance from the edge; type ee.Number().
+ *printToConsole*: a flag to prevent printing to console when false, but exports CSV file, boolean (default is false, does NOT print)
+ returns the proximity image, ee.Image() of integer type (i.e. byte()).
 ## <a name="proc"></a>Process tools
 ### lse.connectivity(nodes, resistance, statistic, distance, distanceProbDisp, nodeWeightProperty)
 Calculates landscape connectivity by calculating least-cost distance from *nodes*
 across a *resistance* surface.
+ *nodes*: the source locations or "nodes" to measure distance from (typically polygons, but can be lines or points), ee.FeatureCollection()
+ *resistance*: resistance surface used to calculate cost-distance, ee.Image(). Use a value of 1 for Euclidean distance, resistance values (cost weights) typically are >=1 (up to 10-1000)
+ *summaryStat*: the statistic to summarize the cost distance surfaces: "mean" (default), "max", "min", "sum", "count", "meanW" (value-weighted mean), ee.String().
+ *distance*: the distance at which the prob of dispersal occurs in meters. Default = 10000 m. The maximum Euclidean distance in cumulativeCost() is set to 3 x the distance used to calculate cost-distance; ee.Number().
+ NOTE: to calculate the dispersal
+   kernel, the distance at which the dispersal probability is set to *distanceProbDispersal% probability at *distance* (m),
+   but applied to the ecological (cumulative cost) distance (e.g., distanceProbDispersal=0.01 at distance=100 means a 1% probability of "dispersal" at 100 meters reaching 100 km, theta=0.0000461).
+ *distanceProbDisp*: the probability parameter to calculate the dispersal kernel; ee.Number() from 0.0 to 1.0, default=0.01.
+ *nodeWeightProperty*: a weight, as an integer value, specified in a property that is applied to the cost distance surface calculated for each node, ee.Number(). By default, the value is 1 but can be any positive value. !!!Not currently implemented!!!
+ returns connectivity image named "Dispersal<<statistic>>", ee.Image().
 ### lse.connectivityAll(nodes, resistance, maxDistance, nodeWeightProperty)
 Calculates landscape connectivity by calculating least-cost distance from all *nodes* simultaneously, not for each node,
 across a *resistance* surface. This generates a 'planar' surface.
+ *nodes*: the source locations or "nodes" to measure distance from (typically polygons, but can be lines or points), ee.FeatureCollection()
+ *resistance*: resistance surface used to calculate cost-distance, ee.Image(). Use a value of 1 for Euclidean distance, resistance values (cost weights) typically are >=1 (up to 10-1000)
+ *distance*: the maximum Euclidean distance (meters) used to calculate cost-distance; ee.Number().
+ NOTE: to calculate the dispersal
+   kernel, the distance at which the dispersal probability is set to *distanceProbDisp* probability at the maxDistance,
+ *summaryStat*: the statistic to summarize the cost distance surfaces: "mean" (default), "max", "min", "sum", "count", ee.String().
+ *nodeWeightProperty*: a weight, as an integer value, specified in a property that is applied to the cost distance surface calculated for each node, ee.Number(). By default, the value is 1 but can be any positive value. !!!Not currently implemented!!!
+ returns connectivity image named "DispersalMean", ee.Image().
 ### lse.connectivityResistantKernel(nodes, resistance, kernelType, sigma, numSD, nodeWeightProperty, maxResistance, resolution, printToConsole)
 Calculates landscape connectivity using cumulative resistant kernels generated using least-cost distance from *nodes*
 across a *resistance* surface and applying a Gaussian kernel.
+ *nodes*: the source locations or "nodes" to measure distance from (typically polygons, but can be lines or points), ee.FeatureCollection(). If nodeWeightProperty is specified, must contain that property name.
+ *resistance*: resistance surface used to calculate cost-distance, ee.Image(). Use a value of 1 for Euclidean distance, resistance values (cost weights) typically are >=1 (up to 10-1000)
+ *kernelType*: the shape of the kernel: 'Gaussian', ee.String(). 'linear' is implemented, but be cautious that linear doesn't get trimmed excessively because of the assumption of the *maxResistance* parameter.
+ *sigma*: the radius of the kernel at 1 SD, in meters. Default = 10000 m. The maximum Euclidean distance in cumulativeCost() is set to numSD x sigma, the distance used to calculate cost-distance; ee.Number().
+ *numSD*: the max number of SDs, default = 3, ee.Number()
+ *nodeWeightProperty*: the property name that contains a weight (ideally 0.0 to 1.0) which is used in cost distance surface calculated for each node, ee.String().
+ *maxResistance*: the value used to set a ceiling on the resistance value, used to prevent spurious (high) values from causing unforeseen errors, ee.Number().
+ *resolution*: the value used to set the resolution (scale) in the resulting image, ee.Number().
+ *printToConsole*: if true, information on tool will be printed in the console window, ee.Boolean().
+ returns connectivity image named "ResistantLinearKernel" or "ResistantGaussianKernel", ee.Image().
 ## <a name="sum"></a>Summary tools
 ### lse.summarizeComponents(values, components, statistic, resolution, region, eight)
 Summarizes the values from an image (*values*) within *components*.
 The values are weighted by pixel area to ensure proper calculations for all coordinate systems.
 *values* = values to summarize for each component, ee.Image()
 *components* = Unique labels of each component, represented by integer values, ee.Image()
 *statistic* = a statistic that can include: 'max', 'mean', 'meanA',  'meanQ' (assumes values range from 0.0 to 1.0), 'median', 'min', 'skew', 'sum'.
 These statistics are planned to be supported in the near future: 'deciles', 'meanG', 'MAD' (median absolute difference), 'percentiles', 'quartiles', 'stdDev', 'variance', 'vigintiles'.
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 *kernel* = describes the kernel or neighborhood, supporting "four", "eight" (default), or "twelve" when identifying component, ee.String()
 *printToConsole*: a flag to prevent printing to console if false, but allows of export to CSV file, boolean (default true, does print)
 returns ee.Image()
 ### lse.summarizeGroups(values, groups, statistic, resolution, extent)
 Summarizes the values from an image (*values*) using *statistic* within *groups* specified by the first band (index of 0) of *values* image. Group values must be represented by integer values.
 The values are weighted by pixel area to ensure proper calculations for all coordinate systems (currently, the image can only have 1 band).
 *values* = ee.Image()
 *groups* = ee.Image()
 *statistic* = a statistic that can include: 'max', 'mean', 'meanA',  'meanQ' (assumes values range from 0.0 to 1.0), 'median', 'min', 'skew', 'sum'.
 These statistics are planned to be supported in the near future: 'deciles','meanG', 'MAD' (median absolute difference), 'percentiles', 'quartiles', 'stdDev', 'variance', 'vigintiles'.
 *resolution* = ee.Number()
 *region* = ee.Geometry()
*printToConsole*: a flag to prevent printing to console but allow use of export to CSV file, boolean (default false, does not print)
 returns ee.FeatureCollection()
 ### lse.summarizeRegions(values, zones, lstStatistics, resolution, printToConsole)
 Summarizes the values from an image (*values*) using *statistic* within *regions* specified by a FeatureCollection.
 *values* = ee.Image(). The values are weighted by pixel area to ensure proper calculations for all coordinate systems (currently, the image can only have 1 band).
 *regions* = the regions used to summarize over, ee.FeatureCollection().
 *statistic* = a statistic that can include: 'max', 'mean', 'meanA',  'meanQ' (assumes values range from 0.0 to 1.0), 'median', 'min', 'skew', 'sum'.
 These statistics are planned to be supported in the near future: 'deciles','meanG', 'MAD' (median absolute difference), 'percentiles', 'quartiles', 'stdDev', 'variance', 'vigintiles'.
 *resolution* = ee.Number()
 *printToConsole*: a flag to prevent printing to console but allow use of export to CSV file, boolean (default false, does not print)
 returns ee.FeatureCollection()
 ### lse.summarizeSamples(values, properties, statistic, region, resolution, proportion)
 Summarizes the values from an image (*values*) at random sample locations, the number determined by the *proportion* of pixels.
 *values* = ee.Image(), with 1 or more bands.
 *properties* = a list of the properties to summarize on, the first is used as a weight, ee.String()
 *statistic* = a statistic that can include: 'max', 'mean', 'meanA',  'meanQ' (assumes values range from 0.0 to 1.0), 'median', 'min', 'skew', 'sum'.
 These statistics are planned to be supported in the near future: 'deciles','meanG', 'MAD' (median absolute difference), 'percentiles', 'quartiles', 'stdDev', 'variance', 'vigintiles'.
 *region* = ee.Geometry()
 *resolution* = ee.Number()
 *proportion* = the proportion of pixels (>0.0, ), defaults to 0.0001, ee.Number()
 *printToConsole*: a flag to prevent printing to console but allow use of export to CSV file, boolean (default false, does not print)
 returns ee.FeatureCollection()
 ## <a name="util"></a>Utility tools
 ### lse.addFID(fc)
 Adds a integer feature id value to a feature collection in a property called "FID", indexed starting at 1...
 + *fc*: ee.FeatureCollection()
 + returns: ee.FeatureCollection() with properties: fid, Area_m2, Length_m, CompactnessHull,
 ### lse.addFeatureProperties(fc)
 Adds properties to a feature collection based on feature geometries.
 + *fc*: ee.FeatureCollection()
 + *resolution*: optional value for geometric calculations (default=30), in meters, ee.Number()
 + returns: ee.FeatureCollection() with properties: fid, Area_m2, Length_m, CompactnessHull,
 +   ConvexHull_m2, Parts, Perimeter_m2: PerimeterSingle_m2
 ### lse.addLayer(asset)
 Adds layer to the map using information from the layer itself as default.
 This includes getting setting the layer name to the asset name (with the full directory path removed).
 For feature collection assets, the layer name will include the number of features ()
 For image assets, the layer name will include the nominal resolution.
 + returns: null
 ### lse.exportExtent(image, geometry)
 Export an image to GeoTIFF drive using CRS and CRS Transform for aligned images.
+ *image*: The image to export to GeoTIFF drive, type ee.Image().
+ *geometry*, NOT WORKING YET, get the extent of the geometry but snap it to the image pixels, apply the CRS information from this to the image, type ee.FeatureCollection() or ee.Geometry(). Optional.
+ returns a nice message.
### lse.exportImageToDrive(image)
  Exports an asset image to the drive in TIF format, using the footprint as the geometry, using transform and resolution.
+ *image*: an image to be exported, ee.Image()
+ *shardSize*: an integer in increments of 256 to adapt tile size (defaults to 256), ee.Number().
  Resulting file size must not exceed 17,179,869,183 bytes (i.e. 17 GB!).
 ### lse.fc2table(fc)
 Converts a feature collection to a text-based table, useful to print summary tables to the console and then copy-paste into a spreadsheet.
+ *fc*: The feature collection to convert to a tab-delimited table.
+ returns a nice message.
 ### lse.getCDFs(statistic, distribution)
 Generates percentiles for theoretical distributions to test against.
 + *statistic*: provides the number of increments needed, "deciles" (10, default), "vigintiles" (20), "percentiles", ee.String()
 + *distribution*: the type of distribution to generate, "normal" (default), "pow(0.5)", "pow(2.0)", ee.String()
 + returns: ee.List()
### lse.rankValues(values, extent, resolution, start, end, increment)
  Converts values in an image to their rank order (1 to 100).
+ *values*: an image with continuous (real) values to be ranked, ee.Image()
+ *region*: geographic extent to analyze, ee.FeatureCollection().
+ *resolution*: width of a pixel, in meters. ee.Number()
 ### lse.uniqueValues(fc, property)
 Summarizes a feature collection and provides a list of the unique values for a given property.
+  *fc*: ee.FeatureCollection(), the feature collection with >0 features to summarize.
+  *properties*: a list of properties ee.String(), the property (aka field) contained in the FeatureCollection with either integer or string values.
+  returns a list of the unique values in a given property, type ee.List().

