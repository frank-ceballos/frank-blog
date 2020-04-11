+++
title = "Gotta Graph 'Em All!"
date = 2020-04-10T11:42:27-05:00
tags = ["pokemon", "graph", "python", "mpld3"]
categories = ["visualizations"]
draft = false
+++
{{< image "images/charmander.gif" >}}

{{< iframe "images/pokemon_graph.html" >}}

**Figure 1 — Scatter plot of Health vs Pokemon Rank**: The Pokemon’s evolution stage
in shown by the different style of marker and color. The Pokemon’s speed
determines the size of the marker. Pokemon’s with larger speeds have larger markers.
Hover your mouse over a point to see the corresponding Pokemon.

## Things I do on a Friday night
**Friday 5:00PM**: I have nothing better to do and my new blog needs
content so I guess I will revisit a Pokemon dataset I got hold of last year.
Actually, it was with this dataset that I wrote my first [blog](https://medium.com/i-want-to-be-the-very-best/visualizing-the-pokemon-dataset-using-the-seaborn-module-a47c71a470fb) related
to data visualization. I spent like 3 days collecting the data, processing it,
making graphs, and writing the whole damn thing. It's been over a year and
the Pokemon blog I wrote in Medium has barely gained any views. However,
I've learned so many tricks since then so I figured why not create a cool
graph with this data.

**Friday 11:55PM**: Well that took much longer than I expected but here we go.
Go hover your mouse over a point in the figure above to see a cool trick.
To make this graph, I had to do a bunch of maneuvers. The main packages
that I used are `pandas`, `seaborn`, `matplotlib`, and `mpld3`. Making
`seaborn` and `mpld3` to work correctly was a bit of a challenge.
I will just paste the Python source code here and you can figure out the rest.
```python
###############################################################################
#                          1. Importing Libraries                             #
###############################################################################
import pandas as pd
import numpy as np
import seaborn as sns
from matplotlib import pyplot as plt
from mpld3 import fig_to_html, plugins
import mpld3

import io
import requests


###############################################################################
#                             2. Helper Functions                             #
###############################################################################
def reorder_data(data, rank, stage):
    """Sorts the data df based on the rank df"""
    ordered_pokemon = list(rank['Pokemon'].values)
    unordered_pokemon = list(data['name'].values)

    # Find order
    ordered_indices = [unordered_pokemon.index(pokemon) for pokemon in ordered_pokemon]

    # Reoder data
    df = data.reindex(ordered_indices)
    df['rank'] = rank['Rank'].values


    # Get stages
    name = [name.strip() for name in stage['name'].values]
    stage['name'] = name
    keep = [line in ordered_pokemon for line in stage['name']]
    stage = stage[keep]
    stage = stage.drop_duplicates()
    stage = stage.reset_index(drop = True)
    unordered_pokemon_stage = list(stage['name'].values)

    # Get stages sorted
    # Find order
    ordered_indices = [unordered_pokemon_stage.index(pokemon) for pokemon in ordered_pokemon]

    # Reset index
    stage = stage.reindex(ordered_indices)

    # Add stage
    df['stage'] = stage['stage'].values
    return df

def get_labels(pokemons, data):
    "connects labels to images"
    temp_data = pokemons[['name', 'rank']]
    ordered_pokemon = list(temp_data['name'].values)
    unordered_pokemon = list(data['name'].values)

    # Find order
    ordered_indices = [unordered_pokemon.index(pokemon)+1 for pokemon in ordered_pokemon]

    # Create empty list
    labels = []
    # Create tags
    for ii in ordered_indices:
        raw_path = f"https://raw.githubusercontent.com/frank-ceballos/frank-blog/master/content/posts/04PokemonGraph/images/main_sprites/{ii}.png"
        temp_tag = f'<img src="{raw_path}" alt="image name">'
        labels.append(temp_tag)

    return labels

###############################################################################
#                             3. Create Dataset                               #
###############################################################################
# Define urls
url_data = "https://raw.githubusercontent.com/frank-ceballos/frank-blog/master/content/posts/04PokemonGraph/data/pokemon_data.csv"
url_rank = "https://raw.githubusercontent.com/frank-ceballos/frank-blog/master/content/posts/04PokemonGraph/data/pokemon_rank.csv"
url_stage = "https://raw.githubusercontent.com/frank-ceballos/frank-blog/master/content/posts/04PokemonGraph/data/stages.csv"

# Get pokemon data
s=requests.get(url_data).content
data = pd.read_csv(io.StringIO(s.decode('utf-8')))

# Get rank data
s=requests.get(url_rank).content
rank = pd.read_csv(io.StringIO(s.decode('utf-8')))

# Get stage data
s=requests.get(url_stage).content
stage = pd.read_csv(io.StringIO(s.decode('utf-8')))

# Reorderdata based on rank
pokemons = reorder_data(data, rank, stage)

# Process data
pokemons = pokemons.loc[pokemons['generation'] == 1]

# Drop features
features_to_remove = ['japanese_name', 'percentage_male', 'height_m', 'weight_kg', 'classfication', 'abilities', 'type2']
pokemons = pokemons.drop(features_to_remove, axis = 1)


###############################################################################
#                               3. Create Graph                               #
###############################################################################
# Set seaborn enviroment and font size
sns.set(font_scale = 1.5)
sns.set_style({"axes.facecolor": "0.95", "axes.edgecolor": "1", "grid.color": "1",
               "grid.linestyle": "-", 'axes.labelcolor': '0', "xtick.color": "1",
               'ytick.color': '1', 'axes.spines.left': True,
 'axes.spines.bottom': True,
 'axes.spines.right': True,
 'axes.spines.top': True})

# Define color palette
color_palette = ['#e53d00', '#00cc66', '#ffb400']

# Create figure
fig, ax = plt.subplots(figsize=(12,9))

# Create scatterplot
sns.scatterplot(x = 'rank', y = 'hp' , hue = 'stage', style = 'stage',
                     label = None, size = 'speed', sizes=(50, 400),
                     palette = color_palette, legend = False, data = pokemons,
                     ax = ax)

# Change axis labels
plt.xlabel('Pokemon Rank')
plt.ylabel('Health (HP)')

# Change the x-axis range
plt.xlim(-1, 155)

# Manually create a legend since mpld3 cant render the sns.scatterplot legend
ax.plot([], [], "o", color=color_palette[0] , label="Basic")
ax.plot([], [], "x", color=color_palette[2] , label="Stage 1")
ax.plot([], [], "o", color=color_palette[1] , label="Stage 2")
ax.legend(title="Evolutionary Stage", loc="best", framealpha=1, fontsize = 'medium',
          markerscale = 2, facecolor = 'white')

# Tight layout
plt.tight_layout()

# Create labels for points
labels = get_labels(pokemons, data)

# Connect sns and mpld3
plugins.connect(fig, mpld3.plugins.PointHTMLTooltip(ax.get_children()[0], labels))

# Save figure
file_name = 'pokemon_graph.html'
mpld3.save_html(fig, file_name)
```

You are welcome to contact me on [LinkedIn](https://www.linkedin.com/in/frank-ceballos/).

Until next time, take care, and code everyday!

{{< buymeacoffee "" >}}
