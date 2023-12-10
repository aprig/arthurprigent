---
permalink: /BenguelaNiños/
title: "Benguela Niños?"
---


## What are Benguela Niños? 

Benguela Niños are extreme and accute events occuring in the Angola-Benguela-Area (ABA, 8˚E-20˚E; 20˚S-10˚S) off the coasts of Angola and Namibia. 

## Get a timeseries of the Benguela Niños
### Load OI-SST v2 data
```bash
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import xarray as xr
import matplotlib.dates as mdates
import matplotlib.ticker as mticker
import scipy.stats as stats
from datetime import *
import cartopy.crs as ccrs
import cartopy
import matplotlib.patches as mpatches
now = datetime.now()
print(now)
date_time = now.strftime("%d/%m/%Y")
import matplotlib
```
#### Load SST data
```bash
path_data = 'https://psl.noaa.gov/thredds/dodsC/Datasets/noaa.oisst.v2/'
ds = xr.open_dataset(path_data+'sst.mnmean.nc',engine='pydap')
mask = xr.open_dataset(path_data+'lsmask.nc',engine='pydap')
ds = ds.sst.where(mask.mask[0,:,:]==1)
```
#### Extract the SST between 1982 and present day over the tropical Atlantic
```bash

sst= ds.sel(time=slice(datetime(1982, 1, 1), now))
sst = xr.concat([sst[:, :, 180:], sst[:, :, :180]], dim='lon')
sst.coords['lon'] = (sst.coords['lon'] + 180) % 360 - 180  

sst_tropical_atlantic = sst.where((sst.lon>-50)&(sst.lon<25)&(sst.lat<30)&(sst.lat>-30),drop=True)
```


