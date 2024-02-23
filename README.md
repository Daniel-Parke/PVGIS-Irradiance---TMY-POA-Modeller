# PVGIS TMY POA Irradiance Model
## Description:
This repo contains the code required to obtain TMY data from PVGIS for any location in the world. This is then used to calculate from the total plane of array (POA) irradiance in kWh/m2 striking a modelled surface. 

This is useful when conducting any modelling which requires incident radiation values, and calculates values within a 1 hour resolution. This script can be run by importing the packages into an appropriate IDE, or by running the `PVGIS_irradiance.py` file from wtihin this directory. The parameters that can be adjusted are as follow:

Currently once intialised the Sites class will collect annual TMY data from the EU JRC PVGIS API, it will then perform calculations to determine POA radiation, which will then be summarised and stored within the class object. The modelled radiation data can then be accessed directly in timeseries format with 8760 (1 hour) entries, or alternatively the grouped and aggreagated data can be accessed.

## **Modelling Parameters:**
- latitude (float): Latitude of the site.
- longitude (float): Longitude of the site.
- surface_pitch (float): Pitch of the modelled plane in degrees.
- surface_azimuth (float): Azimuth of the modelled plane in degrees.
- albedo (float): Albedo of the modelled plane.
- refraction_index (float): Refraction index of the modelled plane.
- tmz_hrs_east (int): Timezone hours east of GMT, UTC=0.
- timestep (int): Timestep of weather data in minutes
- tmy_data (pd.DataFrame): DataFrame containing TMY data for the site.
- irrad_model (pd.DataFrame): DataFrame containing POA irradiance values for the site.
- rad_data (pd.DataFrame): DataFrame containing statistical grouping of POA irradiance values for the site.


## ** Example Command Line Interface commands to run PVGIS_Irradiance.py:**

**Full Command**
```bash
python PVGIS_irradiance.py --latitude 54.60452 --longitude -5.92860 --surface_pitch 35 --surface_azimuth 0 --albedo 0.2 --refraction_index 0.1
```
**Simple Command**
```bash
python PVGIS_irradiance.py --latitude 54.60452 --longitude -5.92860
```

## **Current implementation of model:**

The model is created by initializing a Site class object when the program is run, which then conducts the appropriate modelling. The TMY and Irradiance data simulated can then be accessed using `.xxx` notation. These models are returned as DataFrames, with the library being built with a focus on using it offline for personal data analysis.
<br>


### **Example Jupyter Notebook Implementation**
```python
from meteo.Site import Site

latitude = 54.60452
longitude = -5.92860
surface_pitch = 35
surface_azimuth = 0
albedo = 0.2
refraction_index = 0.1
tmz_hrs_east = 0

site = Site(latitude, longitude, surface_pitch, surface_azimuth, 
            albedo, refraction_index, tmz_hrs_east)
```

### **Accessing model results:**
```python
# Example on how to access summary of model results
site_tmy_data = site.tmy_data
site_irrad_data = site.irrad_model

site_irrad_data_hourly = site.rad_data.hourly
site_irrad_data_daily = site.rad_data.daily
site_irrad_data_weekly = site.rad_data.weekly
site_irrad_data_monthly = site.rad_data.monthly
site_irrad_data_quarterly = site.rad_data.quarterly
```

### **Saving model results:**
```python
# Example on how to access summary of model results
import pandas as pd

site_irrad_data_daily = site.rad_data.daily

site_irrad_data_daily.to_csv("Testy_Test_Name.csv")
```

This is an example implementation using basic inputs, however there are many more options for customising the model by adding additional inputs. All of the data is stored within pandas dataframes, and as such allow for the usual methods of indexing and accessing data within these structures.
<br><br>

### **Current Model Classes & Methods:**

**Site**
```python
site = Site()
site.latitude
site.longitude
site.surface_pitch
site.surface_azimuth
site.albedo
site.refraction_index
site.tmz_hrs_east
site.timestep

site.tmy_data
site.irrad_model

site.rad_data.hourly
site.rad_data.daily
site.rad_data.weekly
site.rad_data.monthly
site.rad_data.quarterly
```
<br><br>

![WiseWattage](https://i.imgur.com/Y7oMz2Y.png)