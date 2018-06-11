# Seaborn Heatmap
Create Heatmaps with Seaborn

Supply a website source CSV file, a list of column names, and generate a correlation heat map of your features.

## Imports
```Python3
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

## Grab All The Data

```Python3
def getDF(data_url, columns):
    #retrieve data from url, create dataframe, return it
    data = pd.read_csv(data_url, names=columns)
    return data
```

Next, let's build our heat map! It's important to note that we will be creating our own color gradient map, with high correlations displayed in increasing red shades, and lower correlations trending into the blue hues.

We will also add our correlation values inside the graph itself, displaying floating numbers in each category.

## Seaborn Heat Map

```Python3
def heatMap(df):
    #Create Correlation df
    corr = df.corr()
    #Plot figsize
    fig, ax = plt.subplots(figsize=(10, 10))
    #Generate Color Map
    colormap = sns.diverging_palette(220, 10, as_cmap=True)
    #Generate Heat Map, allow annotations and place floats in map
    sns.heatmap(corr, cmap=colormap, annot=True, fmt=".2f")
    #Apply xticks
    plt.xticks(range(len(corr.columns)), corr.columns);
    #Apply yticks
    plt.yticks(range(len(corr.columns)), corr.columns)
    #show plot
    plt.show()
```

## Visualize It
<img src="https://github.com/ajh1143/ajh1143.github.io/blob/master/Images/Abalone/heatmap.png" class="inline"/><br>

Awesome! However, we can do better. The diagonal plane mirrors itself on either side, with both redundant results AND self to self correlations, which we should drop.

## Toggling Our Plot, Apply Mask Parameter

Let's alter our previous code a little, by adding in a 'mirror' parameter that will allow us to drop our redundant mappings.

If the user enters a second paramenter, 'mirror', as 'False', we will generate half the visualization as we did previously. 

```Python3
def heatMap(df, mirror):

   # Create Correlation df
   corr = df.corr()
   # Plot figsize
   fig, ax = plt.subplots(figsize=(10, 10))
   # Generate Color Map
   colormap = sns.diverging_palette(220, 10, as_cmap=True)
   
   if mirror == True:
      #Generate Heat Map, allow annotations and place floats in map
      sns.heatmap(corr, cmap=colormap, annot=True, fmt=".2f")
      #Apply xticks
      plt.xticks(range(len(corr.columns)), corr.columns);
      #Apply yticks
      plt.yticks(range(len(corr.columns)), corr.columns)
      #show plot

   else:
      # Drop self-correlations
      dropSelf = np.zeros_like(corr)
      dropSelf[np.triu_indices_from(dropSelf)] = True
      # Generate Color Map
      colormap = sns.diverging_palette(220, 10, as_cmap=True)
      # Generate Heat Map, allow annotations and place floats in map
      sns.heatmap(corr, cmap=colormap, annot=True, fmt=".2f", mask=dropSelf)
      # Apply xticks
      plt.xticks(range(len(corr.columns)), corr.columns);
      # Apply yticks
      plt.yticks(range(len(corr.columns)), corr.columns)
   # show plot
   plt.show()
   
```
<img src="https://github.com/ajh1143/ajh1143.github.io/blob/master/Images/Abalone/halfheat.png" class="inline"/><br>

Much better! We've dropped a substantial amount of redundant information that only served to act as distractive noise. Now we can focus on the relationships with ease. 

## Code Summary

### Heat Map
```Python3
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

def getDF(data_url, columns):
    #retrieve data from url, create dataframe, return it
    data = pd.read_csv(data_url, names=columns)
    return data
    
    def heatMap(df):
    #Create Correlation df
    corr = df.corr()
    #Plot figsize
    fig, ax = plt.subplots(figsize=(10, 10))
    #Generate Color Map
    colormap = sns.diverging_palette(220, 10, as_cmap=True)
    #Generate Heat Map, allow annotations and place floats in map
    sns.heatmap(corr, cmap=colormap, annot=True, fmt=".2f")
    #Apply xticks
    plt.xticks(range(len(corr.columns)), corr.columns);
    #Apply yticks
    plt.yticks(range(len(corr.columns)), corr.columns)
    #show plot
    plt.show()
```

### "Half" Heat Map

```Python3
    def halfHeatMap(df, mirror):

       # Create Correlation df
       corr = df.corr()
       # Plot figsize
       fig, ax = plt.subplots(figsize=(10, 10))
       # Generate Color Map
       colormap = sns.diverging_palette(220, 10, as_cmap=True)

       if mirror == True:
          #Generate Heat Map, allow annotations and place floats in map
          sns.heatmap(corr, cmap=colormap, annot=True, fmt=".2f")
          #Apply xticks
          plt.xticks(range(len(corr.columns)), corr.columns);
          #Apply yticks
          plt.yticks(range(len(corr.columns)), corr.columns)
          #show plot

       else:
          # Drop self-correlations
          dropSelf = np.zeros_like(corr)
          dropSelf[np.triu_indices_from(dropSelf)] = True
          # Generate Color Map
          colormap = sns.diverging_palette(220, 10, as_cmap=True)
          # Generate Heat Map, allow annotations and place floats in map
          sns.heatmap(corr, cmap=colormap, annot=True, fmt=".2f", mask=dropSelf)
          # Apply xticks
          plt.xticks(range(len(corr.columns)), corr.columns);
          # Apply yticks
          plt.yticks(range(len(corr.columns)), corr.columns)
       # show plot
       plt.show()
   ```
