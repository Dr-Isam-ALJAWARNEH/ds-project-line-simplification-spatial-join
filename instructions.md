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
   > - Exploratory Spatial Data Analysis (ESDA) [example](https://darribas.org/gds_scipy16/ipynb_md/04_esda.html), such as `Spatial Autocorrelation`
   > - Exploratory Spatial and Temporal Data Analysis (ESTDA), [example](https://darribas.org/gds_scipy16/ipynb_md/05_spatial_dynamics.html), some of the following, such as `Spaghetti Plot`, `Kernel Density (univariate, aspatial)`, `Markov Chains`, and `Spatial Markov`, [Spatial Autocorellation](https://github.com/PacktPublishing/Geospatial-Data-Science-Quick-Start-Guide/blob/master/Chapter04/Chapter4.ipynb), and [GLobal Spatial Autocorrelation](https://github.com/PacktPublishing/Geospatial-Data-Science-Quick-Start-Guide/blob/master/Chapter04/Chapter4.ipynb)

2. [ ] After the join with the `simplified version`, perform each of the following queries for testing:
    - create a covering h3 for each polygon in the `simplified version` of the `geojson` file, the result is an array of H3 values covering each polygon,
        for example , given a polygon ``` python 
        coordinates: [[
              [-122.47485823276713, 37.85878356045377],
              [-122.47504834087829, 37.86196795698972],
              [-122.47845104316997, 37.86010614563313],
              [-122.47485823276713, 37.85878356045377]
            ]]```, applying
        ```const hexagons = geojson2h3.featureToH3Set(polygon, 10);```
        this will result in an array, `// -> ['8a2830855047fff', '8a2830855077fff', '8a283085505ffff', '8a283085506ffff']`, for each polygon in the geojson the same appllies, an array of covering H3 values.
        > you can get insights from here, [geojson2h3](https://github.com/uber/geojson2h3)
- we need to create a compact representation, specifically you need to  `explode` the arrays so that you have two columns, one column is `neighborhood` and the other is the `h3 value`, such that for each neighborhood we have several corresponding h3 values (those are the values appeared previously in the array).

    | neighborhood    | H3 |
    | -------- | ------- |
    | Bronx  | '8a2830855047fff'    |
    | Bronx | '8a2830855077fff'     |
    | Bronx    | '8a283085505ffff'    |
    | center  | '8a283085506ffff'    |
    | center | '8a283085504ffff'     |
    | center    | '8a283085502ffff'    |

> you can imagine then that one can perform a filter-refine spatial join (between a csv and geojson files) as  follows: 
- perform the equi-join first on the H3 values, such that you find neighborhood to which each long/lat pair represented by H3 from the csv (mobility, air quality data etc.,) belongs to. Same H3 values can have several neighborhoods because this is an approximate join known as MBR-join (MBR for Minimum Bounding Rectangle), this is a kind of equi-join. This will result in candidates for which you have to apply the exact geometrical operation to check whether a point (long/lat pair represented by H3 value) falls within the neighborhood (i.e., the polygon in the geojson file) or not in real geometries, which is normally achieved using ray-casting algorithm.
- Having generated the `H3 cover` from the polygon file, you develop a simple algorithm to resemble the filter-and-refinement join using the H3 as the encoding system. 
- Do the same for Google's S2, generate a cover for each polygon in the geojson file, the result should be a dataframe with a compact format (two columns, `neighborhood ` and `S2 value`), same as described for the H3. 
    - also implement a filter-and-refine spatial join using S2 this time.
    - check [S2 Region Coverer Online Viewer](https://igorgatis.github.io/ws2/), to see what it means by generating an S2 coverer!, You can also use the following [s2-geojson](https://github.com/pantrif/s2-geojson) as an S2 coverer viewer, and also read more about in the original work by google's [S2 geometry](http://s2geometry.io/) (`SIDE`: you can use those to write some parts of your paper!), also [S2 Covering Examples](http://s2geometry.io/devguide/examples/coverings.html)
    - probably you can use `s2sphere` framework to generate the coverer given a geohjson file. For example something similar to the following: [get a set of S2 cells covering a rectangle in](https://s2sphere.readthedocs.io/en/latest/quickstart.html)
    - here is the implementation of the [RegionCoverer class](https://s2sphere.readthedocs.io/en/latest/_modules/s2sphere/sphere.html#RegionCoverer) from s2shpere implementation!
- Do the same for geohash, generate geohash cover, then perform `filter-refine` spatial join, and compare (time-based and accuracy-based QoS requirements) with S2 and H3.
- read more about `filter-and-refine` spatial join in the following [presentation](https://isamaljawarneh.github.io/talks/FOSS4G2021.pdf)
    - also, a very good explanation (which you can use and cite when you write your paper) is our recent paper in [^1].
    - you specifically need to check the part that reads ```filter-and-refine algorithm [39]. It operates in three steps as follows....```
    [^1]: Al Jawarneh, I. M., Foschini, L., & Bellavista, P. (2023). Efficient Integration of Heterogeneous Mobility-Pollution Big Data for Joint Analytics at Scale with QoS Guarantees. Future Internet, 15(8), 263. [available online](https://www.mdpi.com/1999-5903/15/8/263)
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
- read more how simplify in mapshaper works, add the implementation for `visvalingam` algorithm and do all the comparisons between `DP` and `visvalingam`. Have a look at  `Appendix A` by the end of this file for insights!


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