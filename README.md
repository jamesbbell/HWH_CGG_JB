# 4D Seismic Carbon Storage Tracking

## Sleipner CCS Project

Since 1996, Equinor, the Norwegian state-owned energy company, has been injecting CO2 into the Earth's subsurface in offshore Norway as part of their Sleipner carbon capture and storage project. Similar to how subsurface geological structures can trap oil and gas for thousands or millions of years, the injected CO2 is stored in a porous layer of sandstone and is prevented from migrating upwards by a sealing layer of shale. Sleipner was the world's first offshore carbon capture and storage plant and has seen approximately 1 million tonnes of CO2 injected each year reducing Norway's "business-as-usual" CO2 emissions by 3%. <br><br>

<img src="map.png" alt="Sleipner_map" width="800"><br>
<a href="www.sccs.org.uk/map">Global CCS Map</a>
<br><br>

## Seismic imaging

Seismic imaging is a technique in which seismic waves are bounced off different layers of material in the subsurface, recorded and processed to create an interpretable image. In the case of the Sleipner site, repeated seismic imaging surveys have been performed to monitor the accumulation and migration of CO2 across time. Equinor and their partners have made these seismic images freely available on the co2datashare.org website and are the subject of this hackathon challenge.<br><br>

## Data Acknowledgement

Many thanks to the Sleipner group for providing the seismic data used in the cells below.<br>
https://co2datashare.org/dataset/sleipner-4d-seismic-dataset<br>
https://co2datashare.org/sleipner-2019-benchmark-model/static/license.pdf
<br><br>

## Data loading and visualisation

First, we will load the data and familiarise ourselves with it by displaying a slice:

## import libraries


```python
import pandas as pd
from matplotlib import pyplot as plt
import numpy as np
```

## read base (1994), monitor 1 (2001) and monitor 2 (2006) surveys' data


```python
seismic = pd.read_parquet("Sleipner_4D_Seismic.parquet.gzip", engine = "pyarrow")
display(seismic)
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
      <th>z</th>
      <th>x</th>
      <th>y</th>
      <th>94</th>
      <th>01</th>
      <th>06</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>956</td>
      <td>1716</td>
      <td>0.000026</td>
      <td>-0.006091</td>
      <td>-0.005390</td>
    </tr>
    <tr>
      <td>1</td>
      <td>0.0</td>
      <td>956</td>
      <td>1717</td>
      <td>0.002181</td>
      <td>-0.015860</td>
      <td>0.009069</td>
    </tr>
    <tr>
      <td>2</td>
      <td>0.0</td>
      <td>956</td>
      <td>1718</td>
      <td>0.000025</td>
      <td>-0.009283</td>
      <td>-0.006776</td>
    </tr>
    <tr>
      <td>3</td>
      <td>0.0</td>
      <td>956</td>
      <td>1719</td>
      <td>-0.003922</td>
      <td>-0.009160</td>
      <td>-0.004944</td>
    </tr>
    <tr>
      <td>4</td>
      <td>0.0</td>
      <td>956</td>
      <td>1720</td>
      <td>0.000031</td>
      <td>-0.010252</td>
      <td>-0.008233</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>116648527</td>
      <td>2000.0</td>
      <td>1423</td>
      <td>1960</td>
      <td>0.037578</td>
      <td>0.000948</td>
      <td>0.005703</td>
    </tr>
    <tr>
      <td>116648528</td>
      <td>2000.0</td>
      <td>1423</td>
      <td>1961</td>
      <td>0.026595</td>
      <td>-0.018875</td>
      <td>-0.006727</td>
    </tr>
    <tr>
      <td>116648529</td>
      <td>2000.0</td>
      <td>1423</td>
      <td>1962</td>
      <td>0.018116</td>
      <td>-0.032113</td>
      <td>0.005249</td>
    </tr>
    <tr>
      <td>116648530</td>
      <td>2000.0</td>
      <td>1423</td>
      <td>1963</td>
      <td>0.024587</td>
      <td>-0.012843</td>
      <td>0.032607</td>
    </tr>
    <tr>
      <td>116648531</td>
      <td>2000.0</td>
      <td>1423</td>
      <td>1964</td>
      <td>0.024657</td>
      <td>-0.002074</td>
      <td>0.039382</td>
    </tr>
  </tbody>
</table>
<p>116648532 rows × 6 columns</p>
</div>


Above you can see the data stored in a Pandas DataFrame. There are spatial z, x and y columns along with the seismic amplitude at those coordinates in 94 (1994), 01 (2001) and 06 (2006) seismic surveys. (for those with pre-existing seismic imaging experience z is twt, x is xline and y is iline)

## define a function to display slices of seismic data


```python
def show(data, const_axis, const_val, x_axis, y_axis, z_axis, colour = "gray", z_min = -2, z_max = 2):
    data = data.loc[data[const_axis] == const_val]
    data = data.pivot(columns = x_axis, index = y_axis, values = z_axis)
    fig, ax = plt.subplots(figsize = (10, 10))
    ax.imshow(data, cmap = colour, vmin = z_min, vmax = z_max)
    return plt.show(fig)
```

## display slice y=1800 of 1994 data


```python
show(seismic, "y", 1800, "x", "z", "94")
```


![png](output_9_0.png)


Above you can see a vertical slice of the 1994 survey. That bold white line near the top is the water bottom - the boundary between the ocean and the subsurface. All of the seismic amplitude beneath that water bottom is being reflected off the boundaries between different layers of rock, and the boundaries between the different materials that the porous rocks hold (CO2, natural gas, salt water, oil).

## The challenge:

Here's the hackathon challenge. <b>Create a function which automatically spatially selects the newly accumulated or migrated CO2 from one year's survey to another.</b> If you look at the change in seismic amplitude between one survey and the next you should be able to see a strong change in seismic amplitude in some places as a result of CO2 accumulating in the subsurface.<br><br>

Below is an image of the raw seismic amplitude showing a region of injected CO2:
<img src="seismic.png" alt="Raw seismic example" width="200"> 

And by performing k-means clustering with amplitude, x, y and z as vector dimensions we have been able to highlight the CO2:
 <img src="co2_selection.png" alt="CO2 selection example" width="200">
This is not perfect, however, the water bottom is being selected despite it not having accumulated any CO2.
<br><br>
## Over to you.
There are no constraints on how you attempt this challenge other than your function for selecting migrated CO2 should be automatic i.e. don't manually fit a polygon to the CO2. Please ask us as many questions as you like. We can provide ideas on how to start and can help you better understand the seismic data and the geology.


```python

```
