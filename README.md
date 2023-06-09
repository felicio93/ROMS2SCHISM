# ROMS2SCHISM
Tools for creating SCHISM boundary condition forcing and hotstart files from ROMS output.

Example:

```python
from datetime import datetime, timedelta
import numpy as np
import roms2schism as r2s

# set up dates corresponding to ROMS files to be read:
start_date = datetime(2017, 1, 1)
ndays = 30
dates = start_date + np.arange(ndays) * timedelta(days = 1)

roms_dir = '/path/to/roms/data/'
lonc, latc = 175., -37.
dcrit = 7e3

# read SCHISM grid:
schism = r2s.schism.schism_grid(lonc = lonc, latc = latc)

# create boundary forcing files:
template = "foo_his_%Y%m%d.nc"
r2s.boundary.make_boundary(schism, template, dates, dcrit, roms_dir,
                           lonc = lonc, latc = latc)

# create boundary nudging files for T, S:
template = "foo_avg_%Y%m%d.nc"
r2s.nudging.make_nudging(schism, template, dates, dcrit, roms_dir,
                         lonc = lonc, latc = latc)

# create hotstart.nc file:
roms_data_filename = "foo_his_20170101.nc"
r2s.hotstart.make_hotstart(schism, roms_data_filename, dcrit, roms_dir,
                           lonc = lonc, latc = latc)

```
