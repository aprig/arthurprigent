---
permalink: /Atools/
title: "Atools"
---

# Artools

## What is it about? 

Artools is just some usefull scripts to compute interannual anomalies of a timeserie and take subdomains of a large dataset.
## Examples
### Example 1: data_sub
```bash
import xarray as xr
import numpy as np
import Artools.my_functions as Atools
from pydap.client import open_url
import matplotlib.pyplot as plt
```
#### Load SST data
```bash
sst = xr.open_dataset('https://icdc.cen.uni-hamburg.de/thredds/dodsC/reynolds_sst_anomalies_1982_2001',
                      engine="pydap")
```
#### Take subdomain
```bash
sst_sub = Atools.data_sub(sst,0,90,-30,10)
```
#### Plot maps
```bash
f,ax = plt.subplots(1,2,figsize=[10,5])
cmap = plt.cm.RdYlBu_r
bounds = np.arange(-2,2.2,0.2)
ax=ax.ravel()

ax[0].set_title('All domain')
ax[0].contourf(sst.lon,sst.lat,sst.anomaly[0,:,:],cmap=cmap,levels=bounds)


ax[1].set_title('Subdomain')
ax[1].contourf(sst_sub.lon,sst_sub.lat,sst_sub.anomaly[0,:,:],cmap=cmap,levels=bounds)
```


### Example 2: ano_norm_t
#### Load SST data
```bash
sst_tmp = xr.open_dataset('https://icdc.cen.uni-hamburg.de/thredds/dodsC/reynolds_sst_all',
                      engine="pydap")
```

#### Take Nino3.4 index between 1982/01 and 2020/12
```bash
sst_nino34 = Atools.data_sub(sst_tmp.sst,190,240,-3,3)

sst_nino34 = sst_nino34.sel(
    time=slice(datetime(1982, 1, 1), datetime(2020, 12, 31)))
    
```

#### Compute SST anomalies
```bash
ssta_nino34, ssta_nino34_norm = Atools.ano_norm_t(sst_nino34.mean(dim='lon').mean(dim='lat'))

```

### Plot SSTa timeseries
```bash
f,ax = plt.subplots(1,1,figsize=[10,5])
ax.plot(ssta_nino34.time,ssta_nino34,label='SSTa',color='blue')
ax.plot(ssta_nino34.time,ssta_nino34_norm,label='SSTa normalized',color='red')
ax.set_ylabel('SST anomalies [K]')
ax.set_xlabel('Time')
ax.legend()
```

# Installation 
Create an environment :
```bash
conda create -n Artools_env
```


Then install Artools : 
```bash
source activate Artools_env
pip install git+https://github.com/aprig/Artools.git
```

