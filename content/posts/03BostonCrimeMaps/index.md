+++
title = "Boston Crime Maps with Folium"
date = 2020-04-07T13:13:08-05:00
author = "Frank Ceballos"
tags = ["python", "folium", "geospatial", "geojson", "maps"]
categories = [""]
draft = true
+++

{{< image "images/boston.jpg" >}}


## Introduction
In this blog post, we will demonstrate the capabilities of Python to create web-based maps from geospatial data. We will be using Folium which is built upon Leaflet a JavaScript library for building interactive maps. What this means is that we can manipulate and visualize geospatial data without leaving the Python ecosystem.

In my search to recreate some of maps I've seen on reddit, I stumbled upon Analyze Boston -- a website that publishes datasets related to the city of Boston. I advise you to visit Analyze Boston and take all of their awesome data. You never know what you can do with it later.

In this notebook, we will be visualizing and creating crime maps for the city of Boston using some data I found in Kaggle.

```python
###############################################################################
#                          1. Importing Libraries                             #
###############################################################################
# For reading, visualizing, and preprocessing data
import pandas as pd
import geopandas as gpd
import numpy as np
import folium
from folium.plugins import HeatMap, HeatMapWithTime
from folium import FeatureGroup, LayerControl, Map, Marker, TileLayer, Choropleth
from shapely.geometry import Point, Polygon

# For inline figures
%matplotlib inline
```
