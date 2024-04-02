# Topic 6: line simplification algorithms to joining spatial data

Spatial join of mobility and climate data, incorporating the line simplification algorithms (such as douglas-peuker algorithm) as a front stage to reduce the size of the polygon files, then we use the filter and refinement approach to join the data. 

So, the idea is to develop a new filter-and-refine based approach for spatial join, which takes advantage of douglas peuker algorithm to perform optimized spatial join on the integrated schema. 

Then we can feed the reduced data to a data science task of our selection (for example DBSCAN clustering, regression, recommendation etc.,) 

 
***Team members***:
- [Dr. Isam Al Jawarneh](https://isamaljawarneh.github.io/) (```supervisor```)
- [Fatima Alhammadi]() (```member```)
- [Haya AlSuwaidi]() (```member```) 
- [Shooq Alzarouni]() (```member```) 

Reference paper: 

[paper](https://www.mdpi.com/1424-8220/23/19/8178)
