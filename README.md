 # LandSCaPeN v1.0-alpha, August 2022.
 A Google Earth Engine toolbox to analyze and visualize landscape structure, composition, process, and networks.
 Please cite as: DM Theobald. 2022. *LandSCaPeN v1.0: A Google Earth Engine toolbox to analyze and visualize landscape structure, composition, process, and networks.* [www.davidmtheobald.com](https:davidmtheobald.com).
 The tools are organized into landscape [composition](#comp), [structure](#stru), [process](#proc), [networks](#netw), [utilities](#util), and [visualization](#visu).
 To call LandSCaPeN functions, first load the module into your script through the *require()* function, and then call the function 
 using *lse*. For example:
 var lse = require('users/DavidTheobald8/libsPri:lse')
 Technical notes:
 + parameters to functions must be in proper order and dictionary format (using {}) is *not* supported.
 ```mermaid
   graph LR;
       A(LandSCaPeN)
       A-->B;
       A-->C;
       %%(HM CA) users/DavidTheobald8/HM/HM_202204/HM_Y2Y_CA_2020_90_60sland%%
       B(Ecosystem attributes)
       B-->B1;
       B-->B2;
       B-->B3;
       B-->B4;
       %%(HM US) users/DavidTheobald8/HM/HM_202204/HM_Y2Y_US_2020_90_60sland%%
       B1(Composition)
       B1-->B11
       B1-->B12
       B1-->B13
       B1-->B14
       B11(FC summary)
 %%      click B11 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.compositionFC()"
       B12(FC scatterplot)
 %%      click B12 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.compositionFCscatter()"
       B13(FC stats)
 %%      click B13 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.compositionFCstats()"
       B14(FC shape)
 %%      click B14 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.compositionFCstats()%20add%20shape%20properties"
       B2(Structure)
       B21(Complexity)
       B2-->B21
 %%      click B21 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralComplexity()"
       B22(Intactness)
       B2-->B22
 %%      click B22 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralIntactness()"
       B23(RGB)
       B2-->B23
 %%      click B23 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.structuralRGB()"
       B3(Process)
       B31(Connectivity)
       B3-->B31
 %%      click B31 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivity()"
       B32(Connectivity all)
       B3-->B32
 %%      click B32 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivityAll()"
       B33(Connectivity nested)
       B3-->B33
 %%      click B33 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivityAll()"
       B34(Connectivity redundancy)
       B3-->B34
 %%      click B34 "https://code.earthengine.google.com/?scriptPath=users%2FDavidTheobald8%2Flibs%3Alse.connectivityAll()"
       B4(Network)
       C(Utilities)
 ```
 ## <a name="comp"></a> Composition functions
 ### lse.compositionFC(fc, propertyClass, propertyValue, propertyWeight, places)
 Summarizes an attribute (*propertyValue*) for features from a Feature Collection using a property (*propertyClass*) that contains nominal/class data.
+ *fc*: feature collection with polygons, ee.FeatureCollection()
+ *propertyClass*: name of the "class" property in *fc* to summarize on, ee.String()
+ *propertyValue*: name of the "value" property in *fc* that describes the values to summarize. Must contain numerical values, ee.String()
+ *propertyWeight*: name of the property in *fc* used to calculate weighted statistics. Defaults to the *propertyValue*. Must contain numerical values, ee.String()
+ *places*: number of decimal places when reporting summarized value, ee.Number(). Defaults to 2
+ returns: a feature collection with summarized statistics for each unique value in *propertyClass* to Export.table.toDrive()
 ### lse.compositionFCscatter(fc, propertyClass, propertyValue, propertyWeight, places)
 Displays a scattergram of a feature collection comparing properties for each feature, optionally grouped.
+ *propertyX*: property name containing of the values in *fc* to display on the x-axis, ee.Number()
+ *propertyX*: property name containing of the values in *fc* to display on the y-axis, ee.Number()
+ *propertyGroup*: name of the property to group features in *fc*, ee.String() or ee.Number() treated as nominal data type
+ returns: a scattergram plot in the console window
 ### lse.compositionFCstats(fc, propertyClass, propertyValue, propertyWeight)
 Summarizes an attribute (*propertyValue*) for features from a Feature Collection using a property (*propertyClass*) that contains nominal/class data.
+ *fc*: feature collection with polygons, ee.FeatureCollection()
+ *propertyClass*: name of the "class" property in *fc* to summarize on, ee.String()
+ *propertyValue*: name of the "value" property in *fc* that describes the values to summarize. Must contain numerical values, ee.String()
+ *propertyWeight*: name of the property in *fc* used to calculate weighted statistics. Defaults to the *propertyValue*. Must contain numerical values, ee.String()
+ returns: a feature collection with summarized statistics for each unique value in *propertyClass* to Export.table.toDrive()
 Note: when quantifying a summary measure of patch size, it is recommended to use the "meanQuadratic" or "meanWeighted" statistic (rather than arithmetic "mean" or "median"), which is known as Weighted Mean Patch Size ([Li and Archer 1997](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=weighted+mean+patch+size+li+and+archer&btnG=&oq=weight)).
 Compositional statistics are also known as Patch Richness and Class Area Proportion ([Leitao et al. 2006](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Measuring+Landscapes&btnG=)).
 ### lse.compositionImage(image, resolution, region)
 Calculates the area of classes for an image (raster), assuming nominal/class image values.
 These are also known as Class Area Proportion and Patch Richness ([Leitao et al. 2006](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Measuring+Landscapes&btnG=)).
 Future plans to include summarizes of patches in each class.
+ *image*: image with integer values representing nominal values, type = ee.Image()
+ *resolution*: size of cells in meters used for sampling image, type ee.Number()
+ *region*: area to calculate, type ee.Geometry()
+ returns: a feature collection with summarized statistics for each class for Export.table.toDrive, ee.FeatureCollection().
 ### lse.compositionImage(image, resolution, region)
 Calculates the area of classes for an image (raster), assuming nominal/class image values.
 These are also known as Class Area Proportion and Patch Richness ([Leitao et al. 2006](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Measuring+Landscapes&btnG=)).
 Future plans to include summarizes of patches in each class.
+ *image*: image with integer values representing nominal values, type = ee.Image()
+ *resolution*: size of cells in meters used for sampling image, type ee.Number()
+ *region*: area to calculate, type ee.Geometry()
+ returns: a feature collection with summarized statistics for each class for Export.table.toDrive, ee.FeatureCollection().
 ### lse.uniqueValuesCat(fc, properties)
 Summarizes a feature collection and provides a list of the unique values for a given property or multiple properties concatenated together.
+  *fc*: ee.FeatureCollection(), the feature collection with >0 features to summarize.
+  *property*: ee.List() of strings, the property (aka field) contained in the FeatureCollection with either integer or string values.
+  returns a list of the unique values in a given property, type ee.List().
 ### lse.uniqueValues(fc, property)
 Summarizes a feature collection and provides a list of the unique values for a given property.
+  *fc*: ee.FeatureCollection(), the feature collection with >0 features to summarize.
+  *properties*: a list of properties ee.String(), the property (aka field) contained in the FeatureCollection with either integer or string values.
+  returns a list of the unique values in a given property, type ee.List().
 ## <a name="stru"></a> Structural functions
 ### lse.structuralRGB(landCover, lstRemap, radius)
 Calculates and visualizes the landscape structural mosaic, as described by [Riitters et al. (2009)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Riitters%2C+K.+H.%2C+Wickham%2C+J.+D.%2C+%26+Wade%2C+T.+G.+%282009%29.+An+indicator+of+forest+dynamics+using+a+shifting+landscape+mosaic&btnG=)
 Note that water is considered as null.
+ *landCover*: the desired land cover dataset
+ *lstFrom*, a list with classes from the raw land cover map, ee.List()
+ *lstTo*, a list with classes from the raw land cover map, ee.List()
+ + For example, for NLCD: var lstFrom = [11,12,21,22,23,24,31,41,42,43,52,71,81,82,90,95]
+ +                        var lstTo =   [ 3, 3, 2, 2, 2, 2, 3, 3, 3, 3, 3, 3, 1, 1, 3, 3]
+ *lstRadii*: the radius of moving window in meters, type ee.Number()
+ returns mosaic image and draws the landscape mosaic in the map window.
 ### lse.structuralComplexity(image, lstRadii, region, resolution, edgeOnly)
 This function characterizes the structure of nominal classes. Masked (no data) values are recognized. TEST!!!!
+ *image*: integer values represent classe, ee.Image()
+ *lstRadii*: a list of radii of moving window circles in number of pixels, ee.Number()
+ *region*: the analytical area used to calculate, ee.Geometry()
+ *resolution*: the size of pixels in meters, ee.Number()
+ *edgeOnly*: use only the edge pixels and exclude the interior cells, ee.Boolean()
returns: image with the mean number of different values (types) within the radii
 ### lse.landscapeSignature(patches, resolution, maxDistance, geometry)
 This function characterizes the structure within patches as well as between patches (landscape level), following [Theobald (2003)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C6&q=Theobald%2C+DM+GIS+Concepts+and+ARCGIS+Methods&btnG=)
+ *habitat*: a binary representation of habitat (0=matrix, >0 is "patch"), type: ee.Image()
+ *resolution*: the size of pixels in meters, ee.Number()
+ *maxDistance*: the maximum distance to calculate distance from the nearest patch, ee.Number()
+ *AOI*: user-defined area of interest summarized by histogram, type: ee.Geometry()
returns: image with distance into patch ("core") and away from (into "matrix")'
+ Note: this is similar to "GISFrag" metric: [Ripple et al. (1991)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C5&q=Measuring+forest+landscape+patterns+in+the+cascade+range+of+Oregon%2C+USA&btnG=).
 ### lse.structuralIntactness(values, lstRadii, resolution, units)
 Calculates intactness using a moving window analysis (currently uses circular window)
+ *values*: the imagem ee,Image()
+ *lstRadii*, a list of the radii of the moving window
+ *resolution* in 'meters' to be used as the final output, ee.Number()
+ returns the intactness image.
 ### lse.exportExtent(image, geometry)
 Export an image to GeoTIFF drive using CRS and CRS Transform for aligned images. 
+ *image*: The image to export to GeoTIFF drive, type ee.Image(). 
+ *geometry*, NOT WORKING YET, get the extent of the geometry but snap it to the image piels, apply the CRS information from this to the image, type ee.FeatureCollection() or ee.Geometry(). Optional.
+ returns a nice message.
 ### lse.connectivity(nodes, resistance, summaryStat, distance, distanceProbDisp, nodeWeightProperty)
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
 ### lse.connectivityAll(nodes, resistance, maxDistance)
 Calculates landscape connectivity by calculating least-cost distance from all *nodes* simultaneously, not for each node,
 across a *resistance* surface. This generates a 'planar' surface.
+ *nodes*: the source locations or "nodes" to measure distance from (typically polygons, but can be lines or points), ee.FeatureCollection()
+ *resistance*: resistance surface used to calculate cost-distance, ee.Image(). Use a value of 1 for Euclidean distance, resistance values (cost weights) typically are >=1 (up to 10-1000)
+ *maxDistance*: the maximum Euclidean distance (meters) used to calculate cost-distance; ee.Number(). 
+ NOTE: to calculate the dispersal
+   kernel, the distance at which the dispersal probability is set to 1% chance at the maxDistance,
+   but applied to the ecological (cumulative cost) distance (e.g., 1% chance of reaching 100 km, theta=0.0000461). 
+ *summaryStat*: the statistic to summarize the cost distance surfaces: "mean" (default), "max", "min", "sum", "count", ee.String(). 
+ *nodeWeight*: a weight, as an integer value, specified in a property that is applied to the cost distance surface calculated for each node, ee.Number(). By default, the value is 1 but can be any positive value. !!!Not currently implemented!!!
+ returns connectivity image named "DispersalMean", ee.Image().
 ### lse.connectivityIntactness(fc, resistance, resolution, maxDistance, statistic, tileScale)
 Calculates landscape intactness from each feature in *fc* across all cells, calculated using cost-distance 
 across the *resistance* surface. Calculated as the sum of (Ni x Nj x Dij... Builds on ideas of ...
+ *fc*: the locations (usually points). ee.FeatureCollection()
+ *naturalness*: a surface of values ranging from 0 (not natural, developed) to 1.0 (natural); ee.Image().
+ *resistance*: resistance surface used to calculate cost-distance, assumes minimum value is 1.0; ee.Image().
+   Right now assumes that the degree of human modification (H) or naturalness (N=1-H) is the same as resistance surface.
+ *resolution*: the resolution of output image in meters; ee.Number()
+ *maxDistance*: the maximum Euclidean distance (meters) used to calculate cost-distance; ee.Number(). NOTE: to calculate the permeability or dispersal
+   kernel, the distance at which the dispersal probability is set to 1% chance at the maxDistance,
+   but applied to the ecological (cumulative cost) distance (e.g., 1% chance of reaching 100 km, theta=0.0000461). 
+   Therefore, the resulting dispersal probabilities need to be interpreted carefully, and typical in relative terms, 
+   because the dispersal kernel is applied to cost-distance units.
+ *tileScale*: typically a value of 1 (nominal scale), but use 2 or 4 if computational limits; ee.Number()
+ returns connectivity image named "DispersalMean", ee.Image().
### lse.connectivityNested(values, lstFCs, statistic, resolution)
  Estimates connectivity using nested, hierarchical zones, such as hydrological basins... both up and downstream connectivity by calculating a statistic on values that are within each watershed, at multiple hierarchical watershed levels.
+ *values*: image with values to summarize by analytical units, ee.Image().
+ *lstFCs*: a list of the Feature Collections that contain units at different levels, ee.List().
+ *statistic*: ee.String(), supported: 'max', 'mean', 'median', 'min', 'mode', 'stdDev', 'sum'. Defaults to 'mean'.
+ *resolution*: ee.Number()
 ## <a name="util"></a>Utility functions
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
 ### lse.zonalStats(values, zones, statistic, resolution, extent)
 Summarizes the values from an image (*values*) within *zones* specified by a FeatureCollection.
 Statistic to calculate for each zone at the *resolution* specified. 
 *values* = ee.Image()
 *zones* = the zones or regions used to summarize over. Can be either ee.FeatureCollection() or ee.Image()
 *statistic* = One of: 'deciles', 'MAD' (median absolute difference), 'max', 'mean', 'median', 'min', 'percentiles','quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintiles'
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 *returnStats* = boolean, default is true to return requested stats, if false then image of zones
 returns ee.Image()
 ### lse.summarizeZones(values, zones, lstStatistics, resolution, extent)
 Summarizes the values from an image (*values*) within *zones* specified by a FeatureCollection.
 Each statistic in the *lstStatistics* is calculated for each zone at the *resolution* specified. 
 *values* = ee.Image()
 *zones* = the zones or regions used to summarize over. Can be either ee.FeatureCollection() or ee.Image()
 *lstStatistics* = ee.List() of strings that can include: 'deciles', 'MAD' (median absolute difference), 'max', 'mean', 'median', 'min', 'percentiles','quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintile'
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 *label* = A label used to annotate the name(s) of exported summary tables, ee.String()
 returns ee.FeatureCollection()
 ### lse.summarizeZonesWeighted(values, zones, lstStatistics, resolution, extent)
 Summarizes the values from an image (*values*) within *zones* specified by an integer image.
 Values are weighted by pixelArea for proper area-based calculations -- no need to use equal-area projection!
 Each statistic in the *lstStatistics* is calculated for each zone at the *resolution* specified. 
 *values* = ee.Image()
 *zones* = the zones or regions used to summarize over... ee.Image() of integer type. Band must be renamed to 'zone'
 *lstStatistics* = ee.List() of strings that can include: 'deciles', 'max', 'mean', 'median', 'min', 'percentiles','quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintile'
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 *label* = A label used to annotate the name(s) of exported summary tables, ee.String()
 returns ee.FeatureCollection()
 ### lse.summarizeZonesGroup(values, zones, lstStatistics, resolution, extent)
 Summarizes the values from an image (*values*) within *zones* specified by a FeatureCollection.
 Each statistic in the *lstStatistics* is calculated for each zone at the *resolution* specified. 
 *values* = ee.Image()
 *zones* = the zones or regions used to summarize over... ee.Image() of integer type. Band must be renamed to 'zone'
 *lstStatistics* = ee.List() of strings that can include: 'deciles', 'max', 'mean', 'median', 'min', 'percentiles','quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintile'
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 *label* = A label used to annotate the name(s) of exported summary tables, ee.String()
 returns ee.FeatureCollection()
 ### lse.summarizeZones(values, zones, lstStatistics, resolution, extent)
 Summarizes the values from an image (*values*) within *zones* specified by a FeatureCollection.
 Each statistic in the *lstStatistics* is calculated for each zone at the *resolution* specified. 
 *values* = ee.Image()
 *zones* = the zones or regions used to summarize over, ee.FeatureCollection()
 *lstStatistics* = ee.List() of strings that can include: 'deciles', 'max', 'mean', 'median', 'min', 'percentiles','quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintile'
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 *label* = A label used to annotate the name(s) of exported summary tables, ee.String()
 returns ee.FeatureCollection()
 ### lse.summarizeZones(values, zones, lstStatistics, resolution, extent)
 Summarizes the values from an image (*values*) within *zones* specified by a FeatureCollection.
 Each statistic in the *lstStatistics* is calculated for each zone at the *resolution* specified. 
 *values* = ee.Image(), with 1 or more bands. 
 *zones* = the zones or regions used to summarize over, ee.FeatureCollection()
 *lstStatistics* = ee.List() of strings that can include: 'deciles', 'max', 'mean', 'median', 'min', 'percentiles','quartiles', 'skew', 'stdDev', 'sum', 'variance', 'vigintile'
 *resolution* = ee.Number()
 *extent* = ee.Geometry()
 *label* = A label used to annotate the name(s) of exported summary tables, ee.String()
 returns ee.FeatureCollection()
