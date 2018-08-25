---
title: second post
description: the second post will go here
---


# Points to Shapefile
In this tutorial we use geopandas library to convert points in CSV file into an ESRI shapefile


```python
import os
import shutil
import pandas as pd
import geopandas as gpd
from shapely.geometry import Point
```

First, the points in the csv file are read into pandas dataframe


```python
datafile = "./Dataset/Points.csv"
df = pd.read_table(datafile, sep=',')
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Lon</th>
      <th>Lat</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-88.864</td>
      <td>40.444</td>
      <td>A</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-88.924</td>
      <td>40.603</td>
      <td>B</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-88.592</td>
      <td>40.120</td>
      <td>C</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-89.174</td>
      <td>40.302</td>
      <td>D</td>
    </tr>
  </tbody>
</table>
</div>



- After that, a list of point geometry is created from the Latitude and Longitude column of the dataframe. 
- An appropriate CRS is taken. 
- And a geodataframe is created from the dataframe, crs and geometry. 


```python
geometry = [Point(xy) for xy in zip(df.Lon, df.Lat)]
crs = {'init': 'epsg:4326'}
gdf = gpd.GeoDataFrame(df, crs=crs, geometry=geometry)
gdf.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Lon</th>
      <th>Lat</th>
      <th>Value</th>
      <th>geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-88.864</td>
      <td>40.444</td>
      <td>A</td>
      <td>POINT (-88.86399999999999 40.444)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-88.924</td>
      <td>40.603</td>
      <td>B</td>
      <td>POINT (-88.92399999999999 40.603)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-88.592</td>
      <td>40.120</td>
      <td>C</td>
      <td>POINT (-88.59200000000001 40.12)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-89.174</td>
      <td>40.302</td>
      <td>D</td>
      <td>POINT (-89.17399999999999 40.302)</td>
    </tr>
  </tbody>
</table>
</div>



Now the geodataframe is saved into a shapefile using a simple method from geopandas. 


```python
outdir = './Dataset/point_to_shapefile'
if os.path.exists(outdir):
    shutil.rmtree(outdir)
os.makedirs(outdir)
outfile = os.path.join(outdir, "mypoints.shp")
gdf.to_file(outfile, driver='ESRI Shapefile')
```
