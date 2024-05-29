# Downloading-of-NASA-PACE-Image-data-using-our-STS-Customized-HyperCoast-Software
The Science and Technology Society (STS) of Sarasota-Manatee Counties, Florida have created a few Jupyter Notebooks to be downloaded from this repository so that you too can learn how to download PACE data and then with Geospatial Solutions HyperCoast software view, process and display this new PACE hyperspectral data. 

# **Visualizing NASA PACE data interactively with [HyperCoast](https://github.com/opengeos/HyperCoast)**
---

This notebook demonstrates how to visualize PACE [Plankton, Aerosol, Cloud, ocean Ecosystem (PACE)](https://pace.gsfc.nasa.gov) data interactively with HyperCoast.

The Open Geospatial Solutions ([opengeos](https://github.com/opengeos)) GitHub organization hosts a collection of open-source geospatial software projects. The projects are developed by a community of geospatial software developers and researchers. The projects are maintained by the community and are free to use and modify. The projects are open-source and are licensed under the MIT license. If you are interested in hosting an open-source project with them, please submit a request to Geospatial Solutions on their Discussion Board. They always welcome new contributors and collaborators.

# **Open Geospatial Solutions HyperCoast**
---

The following is taken from the their [page](https://github.com/opengeos/HyperCoast?tab=readme-ov-file)


**HyperCoast is a Python package for visualizing and analyzing hyperspectral data in coastal regions**

    - Free software: MIT License
    - Documentation: https://hypercoast.org

## Features
---

    - Interactive visualization and analysis of hyperspectral data (e.g., EMIT, PACE)
    - Interactive extraction and visualization of spectral signatures
    - Saving spectral signatures as CSV files

![image1](HyperCoast_Map.png)

We are all very much appreciative of the fine work that goes on at Open Geospatial Solutions. 

Florida faces some stiff challenges in dealing the old problem of Red Tide (Karenia brevis). When this algal bloom drifts toward our shorelines, we have problems. Red Tide gives off a toxic vapor that attacks the lungs, kills the fish and other aquatic wildlife and have a huge negative impact on the economy and tourism. Yes, Red Tide has been around for millions of years, but what can we do to mitigate the impact now? Is it possible for us to at least identify and track these Harmful Algal Blooms (HAB) as an early warning system? STS and our citizen scientists would like to be a part of the solution.   

It just so happens that NASA has just launched a new satellite called PACE (Plankton, Aerosol, Cloud Ocean Ecosystem) with hyperspectral data that focuses on our ocean ecosystem and microscopic algae called phytoplankton including the Red Tide species. 

PACE data is hyperspectral data in that each pixel in a PACE image has 184 channels of spectral data from 339nm in the UV range to 719nm in the near red edge of the visible spectrum. This is a lot of data. However, our friends at Open Geospatial Solutions have a new project called HyperCoast that can be used to download the PACE data that you need with your geographic coordinates, requested dates and cloud cover. After the download, you can view individual channel data on interactive maps or even view the wavelength spectrum for each pixel that you request. HyperCoast works extremely well. In addition, we can then use then use the HyperCoast data structure to calculate your own indices such as Chlorophyll a and possibly Red Tide algal bloom locations for the requested PACE data. 

1) We have created a Jupyter Notebook so that you too can download PACE data use HyperCoast features to view and process the data. In addition, we have extracted the source code and run it directly in each Jupyter notebook cells to better understand how the actual source code is used to view the PACE data and then use these data to calculate Chlorophyll a and in time Red Tide blooms. 

![image1](chlor_a.png)

2) We have also created a Jupyter Notebook called 1_netCDF4_PACE_Earth_Data_Visualize_brie_ver6.ipynb that uses HyperCoast to download the data, but then uses the NetCDF .nc file structure that allows for a more straight forward, pythonic approach to working with PACE data. However, this lacks the plethora of tools being developed by Open Geospatial Solutions that will grow with time in the future, Still, give the netCDF4 method a try too. 

![image1](wavelength.png)

We also have included the original PACE Access Data notebook proposed from the Ocean Color Instrument (OCI).

**Authors:** Anna Windle (NASA, SSAI), Ian Carroll (NASA, UMBC), Carina Poulin (NASA, SSAI)

> **PREREQUISITES**
>
> This notebook has the following prerequisites:
> - An **<a href="https://urs.earthdata.nasa.gov/" target="_blank">Earthdata Login</a>**
>   account is required to access data from the NASA Earthdata system, including NASA ocean color data.
> - There are no prerequisite notebooks for this module.
