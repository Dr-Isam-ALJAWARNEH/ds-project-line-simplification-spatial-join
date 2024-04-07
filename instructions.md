# instructions
## follow the following instructions
-----------------------------------
<!-- Task 2 -->
# [ ] Task 2! 
# `Update: April 6, 2024`
### `N.B.` references are available in the end of this instruction file!
# `Required OPTIMIZATION ==> IMPORTANT!`
## `TODO:` 
1. [ ] start by performing Exploratory Data Analytics (EDA) for the data > for example - historgrams to study the distribution of data (you have three sensors) - `Kernel Density (univariate, aspatial)` - get insights from the following: - [02_geovisualization](https://darribas.org/gds_scipy16/ipynb_md/02_geovisualization.html) - [Exploratory Spatial Data Analysis (ESDA)](https://darribas.org/gds_scipy16/ipynb_md/04_esda.html) - [NYC Data](https://github.com/PacktPublishing/Geospatial-Data-Science-Quick-Start-Guide/blob/master/Chapter02/NYC%20Data.ipynb) - [Performing Spatial operations like a Pro](https://github.com/PacktPublishing/Geospatial-Data-Science-Quick-Start-Guide/blob/master/Chapter03/Chapter3.ipynb) such as `Spatial join`
   > also,
   >
   > - Exploratory Spatial Data Analysis (ESDA) [example](https://darribas.org/gds_scipy16/ipynb_md/04_esda.html), such as `Spatial Autocorrelation`
   > - Exploratory Spatial and Temporal Data Analysis (ESTDA), [example](https://darribas.org/gds_scipy16/ipynb_md/05_spatial_dynamics.html), such as `Spaghetti Plot`, `Kernel Density (univariate, aspatial)`, `Markov Chains`, and `Spatial Markov`, [Spatial Autocorellation](https://github.com/PacktPublishing/Geospatial-Data-Science-Quick-Start-Guide/blob/master/Chapter04/Chapter4.ipynb), and [GLobal Spatial Autocorrelation](https://github.com/PacktPublishing/Geospatial-Data-Science-Quick-Start-Guide/blob/master/Chapter04/Chapter4.ipynb)
- read more how simplify in mapshaper works, add the implementation for `visvalingam` algorithm and do all the comparisons between `DP` and `visvalingam`. Have a look at  `Appendix A` by the end of this file for insights!
- After the join with the `simplified version`, perform each of the following queries for testing:
    - Geocode using H3, and S2
        - generate geo-cover (H3 coverer or S2 coverer from the polygon file, then use it as a prefiltering stage to sample only data resulting in the cover), compare cover file size in MB.
    - Stratified sampling (using H3 and S2)
    - Top-N
    - containment spatial query (regular and irrigular)
    - DBSCAN
    - Choropleth map generation
    - And compare with RMSE, MAPE, etc.,
    - to calculate any of these metrics to test the accuracy, you need to group data into two distributions, one for the original and one for the simplified, then calculate those metrics as distances between distributions. Here (in the notebook comments!), I am coding an example for RMSE, MAPE, Pearson, KL Divergence and other metrics for the count and Top-N respectively. That is to say, how much is the difference in count of PM readings between the original data and the simplified data.
    - you need to apply the same methods for the distribution of the averages, by how much the distribution of averages across neighborhoods in city-wide diverge for the simplified version data as compared to the original data

-----------------------------
<!-- Task 1 -->
# [ ] Task 1! 
1. [X] run the example starting code and familiarize yourself with some geosaptial processing techniques, including:
    - sampling
    - spatial join
    - geo-visualization

2. [ ] read about some famous line simplification algorithms, including
    > [Ramer–Douglas–Peucker RDP algorithm](https://en.wikipedia.org/wiki/Ramer%E2%80%93Douglas%E2%80%93Peucker_algorithm) and
    > [Visvalingam–Whyatt algorithm](https://en.wikipedia.org/wiki/Visvalingam%E2%80%93Whyatt_algorithm)

3. [ ] search appropriate open source implementation for those algorithms in Python!, for example:
    > [python tool for RDP algorithm](https://rdp.readthedocs.io/en/latest/)
but, more importantly, probably the following is "go-for it" one that you want to consider to simplify a geojson file consisting of polygons:
[Shapely geojson simplifier](https://github.com/anishdhakal15/geojson-simplifyer)
>> read more about how ```simplify``` method in Shapely works here:
[shapely simplify](https://shapely.readthedocs.io/en/stable/manual.html#object.simplify)

here is another implementation for the [visvalingamwyatt algorithm](https://pypi.org/project/visvalingamwyatt/)
> probably you need to compare the performance of ```visvalingamwyatt``` and ```RDP``` algorithms!

4. [ ] Your task is the following:
    - Read our paper titled: ***"Polygon Simplification for the Efficient Approximate Analytics of Georeferenced Big Data"***, available online here: [reference paper](https://www.mdpi.com/1424-8220/23/19/8178)
    - You need to design a novel spatial join algorithm that employs RDP algorithm (and probably other algorithms) to first simplify the polygons, then perform the spatial join and compare the results in terms of latency and accuracy quality of service (QoS) constraints!
-------------
Update 27 March 2024
For the paper writing and submission
- consider one of the following two conference
    - [MCNA - Spain](https://mcna-conference.org/2024/committee.php)
    - [IDSTA - Croatia](https://idsta-conference.org/2024/calls.php)
- Results:
    - parameters : tolerance in the x-axis , accuracy [RMSE](https://www.statisticshowto.com/probability-and-statistics/regression-analysis/rmse-root-mean-square-error/). Take a query, "what is the average pm10 value for each neighborhood", consider for calculating the RMSE (RMSD) that the expected value is the average pm10 for each neighborhood independently (theta-hat) after using the join with simplification, and the original average (theta) before using simplification with the original polygons. [RMSE equation](https://en.wikipedia.org/wiki/Root-mean-square_deviation)
    - the same thing x/y but with Mean Absoulte Percentage Error [MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
--------------------
# Appendix A
> ### -simplify
Mapshaper supports Douglas-Peucker simplification and two kinds of Visvalingam simplification.

Douglas-Peucker (a.k.a. Ramer-Douglas-Peucker) produces simplified lines that remain within a specified distance of the original line. It is effective for thinning dense vertices but tends to form spikes at high simplification.

Visvalingam simplification iteratively removes the least important point from a polyline. The importance of points is measured using a metric based on the geometry of the triangle formed by each non-endpoint vertex and the two neighboring vertices. The `visvalingam` option uses the "effective area" metric &mdash; points forming smaller-area triangles are removed first.

Mapshaper's default simplification method uses Visvalingam simplification but weights the effective area of each point so that smaller-angle vertices are preferentially removed, resulting in a smoother appearance.

When working with multiple polygon and polyline layers, the `-simplify` command is applied to all of the layers.

**Options**

`<percentage>` or `percentage=`  Percentage of removable points to retain. Accepts values in the range `0%-100%` or `0-1`.

`dp` `rdp`	Use Douglas-Peucker simplification.

`visvalingam`   Use Visvalingam simplification with the "effective area" metric.

`weighted`   Use weighted Visvalingam simplification (this is the default). Points located at the vertex of more acute angles are preferentially removed, for a smoother appearance.

`weighting=`  Coefficient for weighting Visvalingam simplification (default is 0.7). Higher values produce smoother output. `weighting=0` is equivalent to unweighted Visvalingam simplification.

`resolution=`  Use an output resolution (e.g. `1000x800`) to control the amount of simplification.

`interval=`	 Specify simplification amount in units of distance. Uses meters when simplifying unprojected datasets in 3D space (see `planar` option below), otherwise uses the same units as the source data.

`variable`  Apply a variable amount of simplification to the paths in a polygon or polygon layer. This flag changes the `interval=`, `percentage=` and `resolution=` options to accept JavaScript expressions instead of literal values. (See the `-each` command for information on mapshaper JS expressions).

`planar`  By default, mapshaper simplifies decimal degree coordinates in 3D space (using geocentric x,y,z coordinates). The `planar` option treats lng,lat coordinates as x,y coordinates on a Cartesian plane.

`keep-shapes`   Prevent polygon features from disappearing at high simplification. For multipart features, mapshaper preserves the part with the largest original bounding box.

`no-repair`	By default, mapshaper rolls back simplification along pairs of intersecting line segments by re-introducing removed points until either the intersection disappears or there are no more points to add. This option disables intersection repair.

`stats`  Display summary statistics relating to the geometry of simplified paths.

**Examples**
```bash
# Simplify counties.shp using the default algorithm, retaining 10% of removable vertices.
mapshaper counties.shp -simplify 10% -o simplified.shp

# Use Douglas-Peucker simplification with a 100 meter threshold.
mapshaper states.shp -simplify dp interval=100 -o simplified/
```
[mapshaper](https://github.com/mbloch/mapshaper/blob/master/REFERENCE.md)