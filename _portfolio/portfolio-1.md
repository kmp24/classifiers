---
title: "Calculating the area of NH Bedrock"
excerpt: "Using the USGS's bedrock geology shapefiles, bedrock types in New Hampshire were summarized in a plot by area.<br/><img src='/images/Rock_types.png' style='width: 350px;'>"
collection: portfolio
---


``` python
import numpy as np
import geopandas as gpd
import pandas as pd
import folium
from matplotlib.colors import to_hex
import seaborn as sns
```

```python
geology = gpd.read_file(r'C:\Users\Kaitlyn\Desktop\GIS\bedrock_geology\nhgeol_poly_dd.shp')
```
### Take a selection of the columns to simplify the dataset
```python
geology = geology.loc[:, ['geometry', 'ROCKTYPE1', 'ROCKTYPE2', 'UNIT_LINK']]
```
### Use the USGS litho symbology text file for color styling
```python
colors = pd.read_csv(r'C:\Users\Kaitlyn\Desktop\GeoPython\GeologyPython\lithrgb.txt', delimiter='\t')
colors.index = colors.text.str.lower()
colors.replace('-', '')
colors.head()
```
### Use matplotlib's coloring and lambda function in python to join rgb values
```python
colors['rgba'] = colors.apply(lambda x:'rgba(%s,%s,%s,%s)' % (x['r'],x['g'],x['b'], 255), axis=1)
colors['hex'] = colors.apply(lambda x: to_hex(np.asarray([x['r'],x['g'],x['b'],255]) / 255.0), axis=1)
```
### Group the polygons by rock type
```python
rock_group = geology.groupby('ROCKTYPE1')
```

### Calculate area
```python
geology['AREA_sqkm'] = geology.to_crs({'init': 'epsg:32616'}).area / 10**6
```
### Get sum of all rock types
```python
rock_group_sum = rock_group.sum()
rock_group_sum['ROCK'] = rock_group_sum.index
rock_group_sum.head()
print(rock_group_sum.head())
                     AREA_sqkm                ROCK
ROCKTYPE1                                         
amphibolite          18.507552         amphibolite
basalt               24.006376              basalt
bimodal suite       723.229612       bimodal suite
calc-silicate rock   47.731576  calc-silicate rock
conglomerate         35.352720        conglomerate
```
### The resulting figure is created:

<img src='/images/Rock_types.png' style='height: 500px;'>"  
