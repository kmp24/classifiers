---
title: "Custom function - classifier"
excerpt: "Using a custom function to classify lakes by size, into two classes.<br/><img src='/images/2LakesClassifier.JPG' style='width: 350px;'>"
collection: portfolio
---

### Reclassifying NH Lakes with a custom function

### The object of this project was to create and use a custom function in Python. I created a function to classify lakes in New Hampshire in 2 classes: under 1 sq km, and those over 1 sq km.

```import matplotlib.pyplot as plt
import geopandas as gpd
```

### I'm going to use the lakes and ponds shapefile available on NH Granit.

```
fp = "C:/Users/Kaitlyn/Desktop/GeoPython/Reclass/data/NH_lakes_polygons.shp"
```
```
data = gpd.read_file(fp)
```

### Examining the columns: I'm going to get rid of irrelevant columns in the shapefile, selecting just the county, area, and Lake names for this analysis.
<img src='/images/data.JPG' style='width: 350px;'>"
```
data.head(2)
```
```
selected_cols = ['COUNTY', 'AREA', 'LAKE', 'geometry']
data = data[selected_cols]
```

#### A plot of lakes, symbolized by area

```
data.plot(column='AREA', linewidth=0.05)
```

#### And remove extra white space
```
plt.tight_layout()
```

### Now, to create a binary classifier - everything under 1 sq km in one class, everything over 1 sq mk is in another:

```
def binaryClassifier(row, source_col, output_col, threshold):
    if row[source_col] < threshold:
        row[output_col] = 0
    else:
        row[output_col] = 1
    return row
```

#### Calculating the area of lakes, and converting from meters into sq km

```
data['area_km2'] = data['AREA'] / 1000000
```
```
l_mean_size = data['area_km2'].mean()
```

### Create an empty column for the classifier to reside in
```
data['small_big'] = None
```

### Now apply the classifier to the data:

```
data = data.apply(binaryClassifier, source_col='area_km2', output_col='small_big', threshold=l_mean_size, axis=1)
```

### And here is the resulting plot:
```
data.plot(column='small_big', linewidth=0.05, cmap="seismic")
```

### Save the file as a new shapefile:
```
outfp_data = r"C:/Users/Kaitlyn/Desktop/GeoPython/Reclass/NH_lakes.shp"
data.to_file(outfp_data)
```
