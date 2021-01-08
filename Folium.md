# 1. Basic

## 1.1 Definition

지도 데이터에 `leaflet.js` 를 이용해 위치정보를 시각화하는 라이브러리

자바스크립트 기반이기에 interactive함

- 참고자료

[Folium - Folium 0.12.0 documentation](https://python-visualization.github.io/folium/)

[How to: Folium for maps, heatmaps & time analysis](https://www.kaggle.com/daveianhickey/how-to-folium-for-maps-heatmaps-time-analysis)

[HyunSu-Jin/seoul_crime](https://github.com/HyunSu-Jin/seoul_crime/blob/master/seoul_crime.ipynb)

## 1.2 Package Loading

```python
import folium
from folium.plugins import MarkerCluster
import pandas as pd
```

# 2. Guide

## 2.1 Put center point

```python
#Define coordinates of where we want to center our map
boulder_coords = [40.015, -105.2705]

#Create the map
my_map = folium.Map(location = boulder_coords, zoom_start = 13)

#Display the map
my_map
```

## 2.2 Marker on the map

```python
#Define the coordinates we want our markers to be at
CU_Boulder_coords = [40.007153, -105.266930]
East_Campus_coords = [40.013501, -105.251889]
SEEC_coords = [40.009837, -105.241905]

#Add markers to the map
folium.Marker(CU_Boulder_coords, popup = 'CU Boulder').add_to(my_map)
folium.Marker(East_Campus_coords, popup = 'East Campus').add_to(my_map)
folium.Marker(SEEC_coords, popup = 'SEEC Building').add_to(my_map)

#Display the map
my_map
```

### 2.2.1 Circular marker

```python
# CircleMarker with radius 
folium.CircleMarker(location = [28.5011226, 77.4099794], 
                    radius = 50, popup = ' FRI ').add_to(my_map2)
```

### 2.2.2 marker as polygun

```python
#Using polygon markers with colors instead of default markers
polygon_map = folium.Map(location = boulder_coords, zoom_start = 13)

CU_Boulder_coords = [40.007153, -105.266930]
East_Campus_coords = [40.013501, -105.251889]
SEEC_coords = [40.009837, -105.241905]

#Add markers to the map
folium.RegularPolygonMarker(CU_Boulder_coords, popup = 'CU Boulder', fill_color = '#00ff40',
                            number_of_sides = 3, radius = 10).add_to(polygon_map)
folium.RegularPolygonMarker(East_Campus_coords, popup = 'EastCampus', fill_color = '#bf00ff',
                            number_of_sides = 5, radius = 10).add_to(polygon_map)
folium.RegularPolygonMarker(SEEC_coords, popup = 'SEEC Building', fill_color = '#ff0000',
                           number_of_sides = 8, radius = 10).add_to(polygon_map)

#Display the map
polygon_map
```

### 2.2.3 interactive marekrs

```python
markers = map_hooray.add_child(folium.ClickForMarker(popup="pop_up_name"))
# Literally add that code to your map and then users can click anywhere to add their own marker.

map_hooray = folium.Map(location=[51.5074, 0.1278],
                        tiles = "Stamen Toner",
                        zoom_start = 9)
# Simple marker
folium.Marker([51.5079, 0.0877],
              popup='London Bridge',
              icon=folium.Icon(color='green')
             ).add_to(map_hooray)

# Circle marker
folium.CircleMarker([51.4183, 0.2206],
                    radius=30,
                    popup='East London',
                    color='red',
                    ).add_to(map_hooray)

# Interactive marker
map_hooray.add_child(folium.ClickForMarker(popup="Dave is awesome"))

map_hooray
```

### 2.2.4 vincent/vega markers

### **You can also use Vincent/Vega markers**

These are clickable effects. So far, we've just seen text pop-ups. Vincent markers use additional JS to pull in graphical overlays, e.g. click on a pop-up to see the timeline of it's history. You can see an example of them in the Folium documentation, [https://folium.readthedocs.io/en/latest/quickstart.html#vincent-vega-markers](https://folium.readthedocs.io/en/latest/quickstart.html#vincent-vega-markers)

### 2.2.5 **Add icons from fontawesome.io**

- fontawesome.io

    Reference the "prefix='fa'" to pull icons from fontawesome.io

    Run help(folium.Icon) to get the full documentation on what you can do with icons

```python
tiles = "Stamen Toner",
                        zoom_start = 9)

folium.Marker([51.5079, 0.0877],
              popup='London Bridge',
              icon=folium.Icon(color='green')
             ).add_to(map_hooray)
folium.Marker([51.5183, 0.5206], 
              popup='East London',
              icon=folium.Icon(color='red',icon='university', prefix='fa') 
             ).add_to(map_hooray)

folium.Marker([51.5183, 0.3206], 
              popup='East London',
              icon=folium.Icon(color='blue',icon='bar-chart', prefix='fa') 
             ).add_to(map_hooray)
             # icon=folium.Icon(color='red',icon='bicycle', prefix='fa')

map_hooray.add_child(folium.ClickForMarker(popup="Dave is awesome"))

map_hooray
```

### 2.2.6 Add a line

```python
folium.PolyLine(locations = [(28.704059, 77.102490), (28.5011226, 77.4099794)], 
                line_opacity = 0.5).add_to(my_map4)
```

## 2.3 Map Style

### 2.3.1 Tileset

```python
# Adding a tileset to our map
t_list = ["Stamen Terrain", "Stamen Toner", "Mapbox Bright"]
map_with_tiles = folium.Map(location = boulder_coords, tiles = 'Stamen Toner')
map_with_tiles

# 사용가능 컬러체크
help(folium.Icon)
```

### 2.3.2 Heatmap

Definitely one of the best functions in Folium. This does not take Dataframes. You'll need to give it a list of lat, lons, i.e. a list of lists. It should be like this. NaNs will also trip it up,

> [[lat, lon],[lat, lon],[lat, lon],[lat, lon],[lat, lon]]

```python
from folium import plugins
from folium.plugins import HeatMap

map_hooray = folium.Map(location=[51.5074, 0.1278],
                    zoom_start = 13) 

# Ensure you're handing it floats
df_acc['Latitude'] = df_acc['Latitude'].astype(float)
df_acc['Longitude'] = df_acc['Longitude'].astype(float)

# Filter the DF for rows, then columns, then remove NaNs
heat_df = df_acc[df_acc['Speed_limit']=='30'] # Reducing data size so it runs faster
heat_df = heat_df[heat_df['Year']=='2007'] # Reducing data size so it runs faster
heat_df = heat_df[['Latitude', 'Longitude']]
heat_df = heat_df.dropna(axis=0, subset=['Latitude','Longitude'])

# List comprehension to make out list of lists
heat_data = [[row['Latitude'],row['Longitude']] for index, row in heat_df.iterrows()]

# Plot it on the map
HeatMap(heat_data).add_to(map_hooray)

# Display the map
map_hooray
```

### 2.3.3 Choropleth maps

[Choropleth Maps](https://plotly.com/python/choropleth-maps/)
