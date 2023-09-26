---
layout: single_column
---

## PDB Data Analysis

The following data was extracted from the Protein Data Bank:  

![Missing Data Table](/assets/img/PDB_missing_data_table.png "Missing Data Table")  

The first five rows of the dataframe created by the ccmodule.py module and which contains all of the extracted data is displayed below:  

_Click image to view larger version_

[![Extracted Conditions Dataframe](/assets/img/conditions_df_first5.png "Extracted Conditions Dataframe")](https://github.com/at58474/at58474.github.io/blob/master/assets/img/conditions_df_first5.png)  

To get a better visualization of the missing data, the Python library missingo was used to create a bar chart showing each column with the number of non Null values found in the conditions dataframe.  

```python
msno.bar(conditions_df)
```

[![PDB Missing Data](/assets/img/PDB_missing_data.png "PDB Missing Data")](https://github.com/at58474/at58474.github.io/blob/master/assets/img/PDB_missing_data.png)  

A new dataframe was then created that contained only the rows rows that had precipitate data. This excludes any row that contains Null values in both the Organic_Precipitates and Salt_Precipitates columns.  

```python
conditions_exist = conditions_df.loc[((conditions_df['Organic_Precipitates'].notnull()) & (conditions_df['Salt_Precipitates'].notnull()))
                                   | ((conditions_df['Organic_Precipitates'].notnull()) | (conditions_df['Salt_Precipitates'].notnull()))]
```

[![PDB Missing Data Excluding](/assets/img/PDB_missing_data_excluding.png "PDB Missing Data Excluding")](https://github.com/at58474/at58474.github.io/blob/master/assets/img/PDB_missing_data_excluding.png)

A heatmap was then created that shows nullity correlation between the columns in the original conditions_df. This represents how strongly each variable affects the presence of each other variable.  

- (-1): if 1 variable appears the other does not
- (0): the variables have no effect on each others appearance
- (1): if 1 variable appears, so does the other

From this the following can be inferred:  

1. If non Null data appears in the Crystallography_Type column, then data most likely also appears in the Vapor_Diffusion column, and vice versa.
2. If non Null data appears in the Organic_Precipitates column, then data most likely also appears in the Salt_Precipitates column, and vice versa.

```python
msno.heatmap(conditions_df, cmap='YlGnBu')
```

[![PDB Heatmap](/assets/img/nullity_correlation_heatmap.png "PDB Heatmap")](https://github.com/at58474/at58474.github.io/blob/master/assets/img/nullity_correlation_heatmap.png)  

Since the crystalliation cocktails consisted of a varying number of chemicals, they were stored as a list of dictionaries. This allows for multiple chemicals along with their concentrations to be stored in a single dataframe column.  
The following is an example of the list of dictionaries for the salt precipitates contained in the crystallization cocktail for the protein with an ID of 1A00:  

[{'10 MM': 'POTASSIUM PHOSPHATE'}, {'100 MM': 'POTASSIUM CHLORIDE'}, {'3 MM': 'SODIUM DITHIONITE'}]  

In order to use these values they needed to be unpacked. This was done by stacking the dictionaries into multiple rows, then splitting the concentration and chemical into different columns.  

```python
# stack the Organic_Precipitates column into organic_df_stacked and Salt_Precipitates column into salt_df_stacked
organic_df_stacked = chemicals_df.set_index('Protein_ID')['Organic_Precipitates'].str.split(', ', expand=True).stack()
salt_df_stacked = chemicals_df.set_index('Protein_ID')['Salt_Precipitates'].str.split(', ', expand=True).stack()

# reset the index and rename the columns
organic_df_stacked = organic_df_stacked.reset_index(level=1, drop=True).rename('Organic_Precipitates')
salt_df_stacked = salt_df_stacked.reset_index(level=1, drop=True).rename('Salt_Precipitates')
```

[![Stacking and Splitting](/assets/img/stacking_and_splitting.png "Stacking and Splitting")](https://github.com/at58474/at58474.github.io/blob/master/assets/img/stacking_and_splitting.png)



[comment]: # (HTML for Organic Precipitate TOP10 Pie Chart)

<div>                        <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script src="https://cdn.plot.ly/plotly-2.12.1.min.js"></script>                <div id="2f6865f0-9fb7-442b-a540-8a012b12f7b0" class="plotly-graph-div" style="height:100%; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("2f6865f0-9fb7-442b-a540-8a012b12f7b0")) {                    Plotly.newPlot(                        "2f6865f0-9fb7-442b-a540-8a012b12f7b0",                        [{"domain":{"x":[0.0,1.0],"y":[0.0,1.0]},"hole":0.3,"hovertemplate":"Organic_Precipitate=%{label}<br>Occurances=%{value}<extra></extra>","labels":["PEG 8000","PEG 4000","PEG 400","PEG 3350","GLYCEROL","PEG 6000","ISOPROPANOL","PEG 8K","TETRAETHYLENE GLYCOL","PEG 6K"],"legendgroup":"","name":"","showlegend":true,"values":[21,15,8,7,6,6,5,5,5,5],"type":"pie","textinfo":"percent+label","textposition":"inside"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"legend":{"tracegroupgap":0},"title":{"text":"Most Occuring Chemicals Found in the PDB"}},                        {"responsive": true}                    )                };                            </script>        </div>



[back](./)
