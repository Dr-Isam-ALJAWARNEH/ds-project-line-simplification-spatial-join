# instructions
## follow the following instructions
1. [ ] run the example starting code and familiarize yourself with some geosaptial processing techniques, including:
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