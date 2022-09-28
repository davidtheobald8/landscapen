  # LandSCaPeN v1.0-alpha, August 2022.
 A Google Earth Engine toolbox to support analyses of landscape structure, composition, process, connectivity, and networks.
 Please cite as: DM Theobald. 2022. *LandSCaPeN v1.0: A Google Earth Engine toolbox to analyze and visualize landscape structure, composition, process, connectivity, and networks.* [www.davidmtheobald.com](https:davidmtheobald.com).
 The tools are organized into landscape [composition](#comp), [structure](#stru), [process](#proc), [networks](#netw), [utilities](#util), and [summaries](#sum).
 To call LandSCaPeN functions, first load the module into your script through the *require()* function, and then call the function 
 using *lse*. For example:
 ```javascript
 var lse = require('users/DavidTheobald8/libs:lse')

 ```
 *Note that the tools are not accessible currently while in alpha stage -- anticipated October 15, 2022 for beta release.*
 
Technical notes:
 + parameters to functions must be in proper order and dictionary format (using {}) is *not* supported.
 ```mermaid
   graph LR;
       A(LandSCaPeN)
       A-->B;
       A-->C1;
       A-->C2;
       %%(HM CA) users/DavidTheobald8/HM/HM_202204/HM_Y2Y_CA_2020_90_60sland%%
       B(Ecosystem attributes)
       B-->B1;
       B-->B2;
       B-->B3;
       B-->B4;
       %%(HM US) users/DavidTheobald8/HM/HM_202204/HM_Y2Y_US_2020_90_60sland%%
       B1(Composition)
       B1-->B11
       B1-->B13
       B1-->B14
       B11(FC summary)
 %%      click B11 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.composition()"
       B13(FC stats)
 %%      click B13 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.compositionStats()"
       B2(Structure)
       B21(Complexity)
       B2-->B21
 %%      click B21 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralComplexity()"
       B22(Distance)
       B2-->B22
 %%      click B22 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralDistance()"
       B23(Intactness)
       B2-->B23
 %%      click B23 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralIntactness()"
       B3(Process)
       B31(Connectivity)
       B3-->B31
 %%      click B31 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivity()"
       B32(Connectivity all)
       B3-->B32
 %%      click B32 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivityAll()"
       B34(Connectivity redundancy)
       B3-->B34
 %%      click B34 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivityAll()"
       B4(Network)
       C1(Summarize)
       C-->C1
       C11(Summarize by components)
       C1-->C11
       C12(Summarize by groups)
       C1-->C12
       C13(Summarize by samples)
       C1-->C13
       C2(Utilities)
       C21(Add shape properties)
       C2-->C21
       C22(Values to ranks)
       C2-->C22
 %%      click C2 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.valuesToRanks()%20add%20shape%20properties"
 ```
 ## <a name="comp"></a> Composition tools
 ### lse.composition(fc, propertyGroup, propertyValue, precision)
 Summarizes the values of a given property (*propertyValue*) for features from a Feature Collection using a property (*propertyGroup*) that contains nominal/class data.
+ *fc*: feature collection, ee.FeatureCollection()
+ *propertyGroup*: name of the "group" property (aka class) in *fc* to summarize on, ee.String()
+ *propertyValue*: name of the "value" property in *fc* that describes the values to summarize. Must contain numerical values, ee.String()
+ *precision*: number of decimal places to report summarized value, defaults to 2, ee.Number()
+ returns: a feature collection with summarized statistics for each unique value in *propertyGroup*, and can be exported as a CSV in the Tasks tab.
 Results are approximately equivalent to the Class Area Proportion and Patch Richness metrics ([Leitao et al. 2006](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Measuring+Landscapes&btnG=)).
 ### lse.compositionStats(fc, propertyGroup, propertyValue, propertyWeight)
 Summarizes an attribute (*propertyValue*) for features from a Feature Collection using a property (*propertyGroup*) that contains nominal/class data.
+ *fc*: feature collection with polygons, ee.FeatureCollection()
+ *propertyGroup*: name of the "class" property in *fc* to summarize on, ee.String()
+ *propertyValue*: name of the "value" property in *fc* that describes the values to summarize. Must contain numerical values, ee.String()
+ *propertyWeight*: name of the property in *fc* used to calculate weighted statistics. Defaults to the *propertyValue*. Must contain numerical values, ee.String()
+ returns: a feature collection with summarized statistics for each unique value in *propertyGroup* to Export.table.toDrive()
 Note: when quantifying a summary measure of patch size, it is recommended to use the "meanQuadratic" or "meanWeighted" statistic (rather than arithmetic "mean" or "median"), which is known as Weighted Mean Patch Size ([Li and Archer 1997](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=weighted+mean+patch+size+li+and+archer&btnG=&oq=weight)).
 Compositional statistics are also known as Patch Richness and Class Area Proportion ([Leitao et al. 2006](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Measuring+Landscapes&btnG=)).
 ## <a name="stru"></a> Structural tools
 ### lse.structuralComplexity(image, lstRadii, kernelType, region, resolution, edgeOnly)
 This tool characterizes the structure of landscape patches, including edge complexity, represented as nominal classes. 
 Masked (no data) values are recognized. TEST!!!!
+ *image*: integer values represent groups (classes), ee.Image()
+ *lstRadii*: a list of radii of moving window (kernel) in number of pixels, ee.Number
+ *kernelType*: the type of kernel to use to calculate complexity: "circle", "gaussian" (Gaussian kernel with 1 SD set to 1/3 of radius), ee.String()
+ *region*: the analytical area used to calculate, ee.Geometry()
+ *resolution*: the size of pixels in meters, ee.Number()
+ *edgeOnly*: use only the edge pixels and exclude the interior cells, ee.Boolean()
returns: image with the mean number of different groups (classes) within the radii
 ### lse.structuralDistance(patches, maxPixels)
 This function characterizes the structure and configuration among patches by measuring Euclidean distance outside a patch (from the patch edge) to other nearby patches.
 Patches are defined by any non-zero value, and matrix should equal 0.
 Also, by inverting the patches (i.e. patches become the matrix, the matrix becomes the "patches")
 provides a structural measure of the shape and size of patches as between patches (landscape level), following [Theobald (2003)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Theobald%2C+DM+GIS+Concepts+and+ARCGIS+Methods&btnG=)
+ *patches*: a binary representation of habitat (0=matrix, >0 is "patch"), type: ee.Image()
+ *maxPixels*: the maximum number of pixels to calculate distance from the nearest patch, by default set to 1024, ee.Number()
returns: image with distance into patch ("core") and away from (into "matrix")'
+ Note: this is a broader application based on the "GISFrag" metric: [Ripple et al. (1991)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=Measuring+forest+landscape+patterns+in+the+cascade+range+of+Oregon%2C+USA&btnG=).
 ### lse.structuralIntactness(values, lstRadii, statistic, radiusPixelsThreshold)
 Calculates intactness using a moving window analysis (currently uses circular window)
+ *values*: the image to be analyzed, ee.Image()
+ *lstRadii*, a list of the radii in meters of the moving window in ascending order.
+ DOUBLE CHECK SUM!!!!
+ *statistic*: the statistic to summarize values: "mean" (default), "max", "min", 'meanQ', "sum", "count", ee.String(). 
+ returns the intactness image.
 ## <a name="proc"></a>Process tools
 ### lse.connectivity(nodes, resistance, statistic, distance, distanceProbDisp, nodeWeightProperty)
 Calculates landscape connectivity by calculating least-cost distance from *nodes*
 across a *resistance* surface.
+ *nodes*: the source locations or "nodes" to measure distance from (typically polygons, but can be lines or points), ee.FeatureCollection()
+ *resistance*: resistance surface used to calculate cost-distance, ee.Image(). Use a value of 1 for Euclidean distance, resistance values (cost weights) typically are >=1 (up to 10-1000)
+ *distance*: the distance at which the prob of dispersal occurs in meters. Default=10000 m. The maximum Euclidean distance is set to be ... (meters) used to calculate cost-distance; ee.Number(). 
+ NOTE: to calculate the dispersal
+   kernel, the distance at which the dispersal probability is set to *distanceProbDispersal% probability at *distance* (m),
+   but applied to the ecological (cumulative cost) distance (e.g., distanceProbDispersal=0.01 at distance=100 means a 1% probability of "dispersal" at 100 meters reaching 100 km, theta=0.0000461). 
+ *summaryStat*: the statistic to summarize the cost distance surfaces: "mean" (default), "max", "min", "sum", "count", ee.String(). 
+ *distanceProbDisp*: the probability parameter to calculate the dispersal kernel; ee.Number() from 0.0 to 1.0, default=0.5. 
+ *nodeWeightProperty*: a weight, as an integer value, specified in a property that is applied to the cost distance surface calculated for each node, ee.Number(). By default, the value is 1 but can be any positive value. !!!Not currently implemented!!!
+ returns connectivity image named "DispersalMean", ee.Image().
 ### lse.connectivityAll(nodes, resistance, maxDistance, nodeWeightProperty)
 Calculates landscape connectivity by calculating least-cost distance from all *nodes* simultaneously, not for each node,
 across a *resistance* surface. This generates a 'planar' surface.
+ *nodes*: the source locations or "nodes" to measure distance from (typically polygons, but can be lines or points), ee.FeatureCollection()
+ *resistance*: resistance surface used to calculate cost-distance, ee.Image(). Use a value of 1 for Euclidean distance, resistance values (cost weights) typically are >=1 (up to 10-1000)
+ *distance*: the maximum Euclidean distance (meters) used to calculate cost-distance; ee.Number(). 
+ NOTE: to calculate the dispersal
+   kernel, the distance at which the dispersal probability is set to 1% chance at the maxDistance,
+   but applied to the ecological (cumulative cost) distance (e.g., 1% chance of reaching 100 km, theta=0.0000461). 
+ *summaryStat*: the statistic to summarize the cost distance surfaces: "mean" (default), "max", "min", "sum", "count", ee.String(). 
+ *nodeWeightProperty*: a weight, as an integer value, specified in a property that is applied to the cost distance surface calculated for each node, ee.Number(). By default, the value is 1 but can be any positive value. !!!Not currently implemented!!!
+ returns connectivity image named "DispersalMean", ee.Image().
 ## <a name="sum"></a>Summary tools
 ### lse.summarizeComponents(values, components, statistic, resolution, region, eight)
 Summarizes the values from an image (*values*) within *components*.
 The values are weighted by pixel area to ensure proper calculations for all coordinate systems.
 *values* = values to summarize for each component, ee.Image()
 *components* = Unique labels of each component, represented by integer values, ee.Image()
 *statistics* = the statistic type that can include: 'deciles', 'geometricMedian', 'kurtosis', 'max', 'mean', 'meanA', 'meanG', 'meanQ', 'median', 'min', 'percentiles', 'quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintile'
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 *kernel* = describes the kernel or neighborhood, supporting "four", "eight" (default), or "twelve" when identifying component, ee.String()
 returns ee.Image()
 ### lse.summarizeGroups(values, groups, statistic, resolution, extent)
 Summarizes the values from an image (*values*) using *statistic* within *groups* specified by the first band (index of 0) of *values* image. Group values must be represented by integer values.
 The values are weighted by pixel area to ensure proper calculations for all coordinate systems.
 *values* = ee.Image()
 *statistics* = the statistic type that can include: 'deciles', 'geometricMedian', 'kurtosis', 'max', 'mean', 'meanA', 'meanG', 'meanQ', 'median', 'min', 'percentiles', 'quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintile'
 *resolution* = ee.Number()
 *region* = ee.Geometry()
 returns ee.FeatureCollection()
 ### lse.summarizeRegions(values, zones, lstStatistics, resolution, extent)
 Summarizes the values from an image (*values*) using *statistic* within *regions* specified by a FeatureCollection.
 The values are weighted by pixel area to ensure proper calculations for all coordinate systems.
 *values* = ee.Image()
 *regions* = the regions used to summarize over, ee.FeatureCollection().
 *lstStatistics* = ee.List() of strings that can include: 'deciles', 'MAD' (median absolute difference), 'max', 'mean', 'meanA', 'meanG', 'meanQ', 'median', 'min', 'percentiles', 'quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintile'
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 returns ee.FeatureCollection()
 ### lse.summarizeSamples(values, properties, statistic, region, resolution, proportion)
 Summarizes the values from an image (*values*) at random sample locations, the number determined by the *proportion* of pixels.
 *values* = ee.Image(), with 1 or more bands. 
 *properties* = a list of the properties to summarize on, the first is used as a weight, ee.String()
 *statistic* = Summary statistic to calculate, that can include: 'max', 'mean' (default), 'median', 'min', 'skew', 'stdDev', 'sum', 'variance'
 Note: 'deciles', 'percentiles','quartiles', 'vigintile' not implemented yet.
 *region* = ee.Geometry()
 *resolution* = ee.Number()
 *proportion* = the proportion of pixels (>0.0, ), defaults to 0.0001, ee.Number()
 returns ee.FeatureCollection()
 ## <a name="util"></a>Utility tools
 ### lse.uniqueValues(fc, property)
 Summarizes a feature collection and provides a list of the unique values for a given property.
+  *fc*: ee.FeatureCollection(), the feature collection with >0 features to summarize.
+  *properties*: a list of properties ee.String(), the property (aka field) contained in the FeatureCollection with either integer or string values.
+  returns a list of the unique values in a given property, type ee.List().
 ### lse.uniqueValuesCat(fc, properties)
 Summarizes a feature collection and provides a list of the unique values for a given property or multiple properties concatenated together.
+  *fc*: ee.FeatureCollection(), the feature collection with >0 features to summarize.
+  *property*: ee.List() of strings, the property (aka field) contained in the FeatureCollection with either integer or string values.
+  returns a list of the unique values in a given property, type ee.List().
### lse.valuesToRanks(values, extent, resolution, start, end, increment)
  Converts values in an image to the rank order.
+ *values*: an image with continuous (real) values to be ranked, ee.Image()
+ *extent*: geographic extent to analyze, ee.String().
+ *resolution*: width of a pixel, in meters. ee.Number()
### lse.dissolveFeatures(fc, property, statistic)
  Combines features with unique values from *property* into a single feature, calculating values of other numerical properties using statistic.
+ *fc*: the feature collection with *property*, ee.FeatureCollection()
+ *property*: geographic extent to analyze, ee.String().
+ *resolution*: width of a pixel, in meters. ee.Number()
 ### lse.chiliEquinox(fc, dem, resolution)
 Calculates the Continuous Heat-Insolation Load Index using 
 elevation data from *dem* at the specified *resolution*. 
 The azimuth to model heatload is "folded" and set to 22.5 west of south (in northern hemisphere),
 and 22.5 east of north (in southern hemisphere).
 *dem* = ee.Image()
 *resolution* = ee.Number()
 returns ee.Image().byte() 
 ### lse.exportExtent(image, geometry)
 Export an image to GeoTIFF drive using CRS and CRS Transform for aligned images. 
+ *image*: The image to export to GeoTIFF drive, type ee.Image(). 
+ *geometry*, NOT WORKING YET, get the extent of the geometry but snap it to the image pixels, apply the CRS information from this to the image, type ee.FeatureCollection() or ee.Geometry(). Optional.
+ returns a nice message.
 ### lse.addFeatureProperties(fc)
 Adds properties to a feature collection based on feature geometries.
 + *fc*: ee.FeatureCollection()
 + *resolution*: optional value for geometric calculations (default=30), in meters, ee.Number()
 + returns: ee.FeatureCollection() with properties 
