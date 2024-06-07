# Downloading NASA PACE Image Data Using STS Customized Hyperspectral Software

## Overview
The Science and Technology Society (STS) of Sarasota-Manatee Counties, Florida, has developed a series of Jupyter Notebooks to facilitate the downloading, processing, and visualization of PACE hyperspectral data from NASA. These tools are particularly aimed at addressing the significant challenge of Red Tide (Karenia brevis) in Florida, which has severe impacts on health, marine life, and the local economy.

## Background
Red Tide is an algal bloom that releases toxic vapors, affecting respiratory health, killing marine wildlife, and damaging the tourism industry. Although Red Tide has existed for millions of years, advancements in technology, specifically through NASA's PACE (Plankton, Aerosol, Cloud, Ocean Ecosystem) satellite, offer new opportunities to identify and potentially  track these Harmful Algal Blooms (HABs).

### PACE Satellite
NASA's PACE satellite provides hyperspectral data focusing on ocean ecosystems and phytoplankton, including Red Tide species. Each pixel in a PACE image contains 184 channels of spectral wavelength data ranging from 339nm (UV) to 719nm (near red edge of the visible spectrum). This data is stored in netCDF4 format (.nc files) and is accessible through NASA's [EarthData](https://urs.earthdata.nasa.gov/). You need to create your own EarthData account. 

## STS Tools and Methodology
STS uses Geospatial Solution's HyperCoast to download PACE data. After obtaining the data, users can visualize individual channel data in maps or analyze the wavelength spectrum for each pixel using Python. We have also developed custom indices, such as Chlorophyll a, to enhance our analysis capabilities. Our ultimate goal is to detect and track Red Tide algal blooms using PACE data.

### Jupyter Notebook
We have created a Jupyter Notebook that allow users to:
1. Download PACE data using HyperCoast.
2. Process and visualize the data in Python.

This notebook provide a guide for users to access and analyze hyperspectral data, contributing to the early detection and tracking of Harmful Algal Blooms (HABs).

## Example Outputs
Below are examples of the outputs you can generate using our notebooks:

![Chlorophyll a Map](chlor_a.png)
*Example of a Chlorophyll a concentration map.*

![Wavelength Spectrum](wavelength.png)
*Example of a wavelength spectrum for a specific pixel.*

## Additional Resources
The repository also includes the original PACE Access Data notebook, as proposed by the Ocean Color Instrument (OCI), for users who wish to explore further:

  Authors: Anna Windle (NASA, SSAI), Ian Carroll (NASA, UMBC), Carina Poulin (NASA, SSAI)
  
---

By using these tools, STS and citizen scientists aim to be part of the solution in mitigating the impacts of Red Tide through advanced hyperspectral data analysis.



> **PREREQUISITES**
>
> This notebook has the following prerequisites:
> - An **<a href="https://urs.earthdata.nasa.gov/" target="_blank">Earthdata Login</a>**
>   account is required to access data from the NASA Earthdata system, including NASA ocean color data.
> - There are no prerequisite notebooks for this module.
