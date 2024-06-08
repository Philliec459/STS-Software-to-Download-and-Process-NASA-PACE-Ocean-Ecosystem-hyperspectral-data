![logo](PACE2.png)
![logo](sts_nasa.png)

# Downloading NASA PACE Image Data Using STS Customized Hyperspectral Software

## Overview
The Science and Technology Society (STS) of Sarasota-Manatee Counties, Florida, has developed a series of Jupyter Notebooks to facilitate the downloading, processing, and visualization of PACE hyperspectral data from NASA. These tools have a long term goal of addressing the significant challenge of Red Tide (Karenia brevis) in Florida, which has severe impacts on health, marine life, and the local economy.

## Background
Red Tide is an algal bloom that releases toxic vapors, affecting respiratory health, killing marine wildlife, and damaging the tourism industry. Although Red Tide has existed for millions of years, advancements in technology, specifically through NASA's PACE (Plankton, Aerosol, Cloud, Ocean Ecosystem) satellite, offer new opportunities to identify and potentially track these Harmful Algal Blooms (HABs). This is work in progress.

### PACE Satellite
NASA's [PACE](https://pace.gsfc.nasa.gov) satellite provides hyperspectral data focusing on ocean ecosystems and phytoplankton, including Red Tide species. Each pixel in a PACE image contains 184 channels of spectral wavelength data ranging from 339nm (UV) to 719nm (near red edge of the visible spectrum). These data is stored in netCDF4 format (.nc files) and are accessible through NASA's [EarthData](https://urs.earthdata.nasa.gov/) website. You will to create your own EarthData account to download data.

        Wavelengths: [315 316 318 320 322 325 327 329 331 334 337 339 341 344 346 348 351 353
         356 358 361 363 366 368 371 373 375 378 380 383 385 388 390 393 395 398
         400 403 405 408 410 413 415 418 420 422 425 427 430 432 435 437 440 442
         445 447 450 452 455 457 460 462 465 467 470 472 475 477 480 482 485 487
         490 492 495 497 500 502 505 507 510 512 515 517 520 522 525 527 530 532
         535 537 540 542 545 547 550 553 555 558 560 563 565 568 570 573 575 578
         580 583 586 588 591 593 596 598 601 603 605 608 610 613 615 618 620 623
         625 627 630 632 635 637 640 641 642 643 645 646 647 648 650 651 652 653
         655 656 657 658 660 661 662 663 665 666 667 668 670 671 672 673 675 676
         677 678 679 681 682 683 684 686 687 688 689 691 692 693 694 696 697 698
         699 701 702 703]

## STS Tools and Methodology
STS uses Geospatial Solution's HyperCoast software to download PACE data. We use HyperCoast because it does show the footprint of available data prior to downloading. The user can then choose the PACE data they want for their analysis from the list of available data.

```python
results, gdf = hypercoast.search_pace(
    bounding_box=(-83, 25, -81, 28),
    temporal=("2024-05-15", "2024-12-31"),
    count=-1,  # use -1 to return all datasets
    return_gdf=True,
)

hypercoast.download_pace(results[:20], out_dir="data")
```


>![choose](choose_file.png)

The user can then visualize individual channel data in maps at a particular wavelength or analyze the entire wavelength spectrum for any pixel on the map using Python. We have also developed custom indices, such as Chlorophyll a, to enhance our analysis capabilities. Our ultimate goal is to detect and track Red Tide algal blooms using PACE data. We have a lot to learn on this subject. 

### Jupyter Notebook
We have created a Jupyter Notebook that allow users to:
1. Download PACE data using HyperCoast.
2. Process and visualize the data in Python.

This notebook provide a guide for users to access and analyze hyperspectral data, and hopefully contributing to the early detection and tracking of Harmful Algal Blooms (HABs) in the near future.

## Example Outputs
Below are examples of the outputs you can generate using our notebooks:

### Waveleng Plot from one PACE Pixel:

```python
def plot_wavelength(target_latitude, target_longitude):
        # Find the closest grid point to the target latitude and longitude
        lat_idx, lon_idx = find_closest_grid_point(latitudes, longitudes, target_latitude, target_longitude)
        
        # Extract reflectance data for the grid point across all wavelengths
        reflectance_spectrum = reflectance_data[lat_idx, lon_idx, :]
        
        # Ensure the dimensions match
        print(f"Wavelengths shape: {wavelengths.shape}")
        print(f"Reflectance spectrum shape: {reflectance_spectrum.shape}")
        
        # Plot reflectance vs. wavelength
        plt.figure(figsize=(12, 6))
        #plt.plot(wavelengths, reflectance_spectrum, marker='o',color='red')
        plt.plot(wavelengths, reflectance_spectrum,color='red',linewidth=3)
        #plt.yscale('log')
        plt.ylim(0,0.02)
        plt.xlim(300,1000)
        
        plt.xlabel('Wavelength (nm)')
        plt.ylabel('Reflectance')
        plt.title(f'PACE Reflectance Spectrum at Lat: {latitudes[lat_idx, lon_idx]}, Lon: {longitudes[lat_idx, lon_idx]}')
        
        # Edge colors picked from image below and sent to Mike 
        plt.axvspan(300, 400, alpha=0.5, color='purple',label='Near UV from 300-400m')
        plt.axvspan(400, 450, alpha=0.5, color='violet',label='Violet 400-450m')
        plt.axvspan(450, 495, alpha=0.2, color='blue',label='Blue from 450-495nm')
        plt.axvspan(495, 550, alpha=0.2, color='green',label='Green from 495-550nm')
        plt.axvspan(550, 590, alpha=0.2, color='yellow',label='Yellow from 550-590nm')
        plt.axvspan(590, 630, alpha=0.2, color='orange',label='Orange from 590-630nm')
        plt.axvspan(630, 700, alpha=0.2, color='red',label='Red from 630-700nm')
        plt.axvspan(700, 1000, alpha=1, color='lavender',label='Near IR from 700-1,000nm')
        plt.legend(bbox_to_anchor=(1.05, 1), loc='upper left', borderaxespad=0.)
        
        plt.grid(True)
        plt.show()

plot_wavelength(target_latitude = 25.3280 , target_longitude = -83.9747)
```

>![Wavelength Spectrum](wavelength.png)
>*Example of a wavelength spectrum for a specific pixel.*
---




```python
def Map_Reflectance(target_wavelength):
        wavelength_index = np.argmin(np.abs(wavelengths - target_wavelength))
        reflectance_at_target = reflectance_data[:, :, wavelength_index]
        
        # Plot the reflectance
        plt.figure(figsize=(12, 8))    
        map_projection = ccrs.PlateCarree()
        ax = plt.axes(projection=map_projection)
        ax.set_extent([-98, -77, 20, 35], crs=ccrs.PlateCarree())  # Florida region
        
        # Plot the reflectance data using pcolormesh
        im = ax.pcolormesh(lon, lat, reflectance_at_target, cmap='rainbow')
        ax.coastlines(resolution='10m')
        ax.add_feature(cartopy.feature.STATES, linewidth=0.5)
        
        # Set ticks and format them
        ax.set_xticks(np.linspace(-91, -77, 5), crs=map_projection)
        ax.set_yticks(np.linspace(20, 35, 5), crs=map_projection)
        lon_formatter = LongitudeFormatter(zero_direction_label=True)
        lat_formatter = LatitudeFormatter()
        ax.xaxis.set_major_formatter(lon_formatter)
        ax.yaxis.set_major_formatter(lat_formatter)
        
        plt.colorbar(im, label=f'Reflectance at {target_wavelength} nm')
        plt.title(f'Reflectance at {target_wavelength} nm around Florida')
        plt.show()

Map_Reflectance(target_wavelength=430)
```


>![Wavelength_Map](430nm2.png)
>*Example of 430nm Reflectance map May 12, 2024.*
---

### This is our attempt to calculate Chlorophyll a using PACE data trying to follow the methods described in the source below:
```python

'''
We used the following source:

        Earth Data Tools Method Datad 2023 used for PACE Data

        Viewing Chlorophyll a — Algorithm Publication Tool.pdf

        Jeremy Werdell and John O'Reilly and Chuanmin Hu and Lian Feng and 
        Zhongping Lee and Bryan Franz and Sean Bailey and Christopher Proctor, 
        Chlorophyll a, Earth Data Publications Tool, November 6, 2023
'''


# Identify indices for the required bands
band_442 = np.argmin(np.abs(wavelengths - 442))
band_490 = np.argmin(np.abs(wavelengths - 490))
band_510 = np.argmin(np.abs(wavelengths - 510))
band_555 = np.argmin(np.abs(wavelengths - 555))
band_670 = np.argmin(np.abs(wavelengths - 670))

# Extract reflectance values at the required wavelengths
Rrs_442 = reflectance_data[:, :, band_442]
Rrs_490 = reflectance_data[:, :, band_490]
Rrs_510 = reflectance_data[:, :, band_510]
Rrs_555 = reflectance_data[:, :, band_555]
Rrs_670 = reflectance_data[:, :, band_670]

'''
1. chlor_a is first calculated using the CI algorithm, which is a three-band reflectance difference algorithm employing the difference between sensor specific Rrs in the green band and a reference formed linearly between Rrs in the blue and red bands (bands are instrument specific - see Table 1):
CI = Rrs(λgreen) − [Rrs(λblue) + (λgreen − λblue)/(λred − λblue) ∗ (Rrs(λred) − Rrs(λblue))] 
'''

# Calculate CI
CI = Rrs_555 - (Rrs_442 + (555 - 442) / (670 - 442) * (Rrs_670 - Rrs_442))

# A calculation of CI chlor_a is done using two coefficients (a0CI = -0.4287 and a1CI = 230.47) specified by Hu et al (2019), where:
# chlor_a = 10**(-0.4287 + 230.47 * CI)
# is this used for anything?

'''
2. chlor_a is then calculated following the OCx algorithm, which is a fourth-order polynomial relationship between a ratio of Rrs and chlor_a: 
'''
    
# Calculate chlor_a using CI
chlor_aCI = 10**(0.32814 + -3.20725*np.log10(Rrs_442 / Rrs_555)**1 + 
                            3.22969*np.log10(Rrs_442 / Rrs_555)**2 + 
                           -1.36769*np.log10(Rrs_442 / Rrs_555)**3 + 
                           -0.81739*np.log10(Rrs_442 / Rrs_555)**4)


'''
OCx Method (I think?)
'''

# Define OCx algorithm coefficients for OC4v6 as an example
a0, a1, a2, a3, a4 = -0.3704, -3.9622, 1.7441, 1.4487, -0.2874

# Calculate the OCx ratio
R = np.maximum.reduce([Rrs_442 / Rrs_555, Rrs_490 / Rrs_555, Rrs_510 / Rrs_555])


# Calculate chlor_a using OCx
log_chlor_a_OCx = a0 + a1 * np.log10(R) + a2 * np.log10(R)**2 + a3 * np.log10(R)**3 + a4 * np.log10(R)**4
chlor_a_OCx = 10**log_chlor_a_OCx

'''
3. For chlor_a retrievals below 0.25 mg m-3, the CI algorithm is used.

For chlor_a retrievals above 0.35 mg m-3, the OCx algorithm is used. 

In between these values, the CI and OCx algorithm are blended using a weighted approach:
'''

# Initialize the chlorophyll-a concentration array
chl_a_CI = np.zeros_like(CI)

# Define thresholds
t1 = 0.25
t2 = 0.35

# Apply the OCI algorithm to estimate chlorophyll concentration
positive_CI_mask = chlor_aCI > t2
negative_CI_mask = chlor_aCI < t1
blended_CI_mask = (~positive_CI_mask) & (~negative_CI_mask)

# Calculate chlorophyll-a for positive CI values
chl_a_CI[positive_CI_mask] = chlor_aCI[positive_CI_mask]

# Calculate chlorophyll-a for negative CI values
chl_a_CI[negative_CI_mask] = chlor_a_OCx[negative_CI_mask]

# Blend CI and OCx for values in between
chl_a_CI[blended_CI_mask] = ((chlor_aCI[blended_CI_mask] * (t2 - chlor_aCI[blended_CI_mask])) / (t2 - t1) +
                             (chlor_a_OCx[blended_CI_mask] * (chlor_aCI[blended_CI_mask] - t1)) / (t2 - t1))

```
>![Chlorophyll a Map](chlor_a_zoom2.png)
>*Example of a Chlorophyll a concentration map May 21, 2024.*
---
>![Chlorophyll a Map](chlor_a2.png)
>*Example of a Chlorophyll a concentration map May 21, 2024.*
---
>![Chlorophyll a Map](WorldView2.png)
>*WorldView Chlorophyll a map May 21, 2024.*
---

### Since our PACE Webinar on June 7, we are also useing [WorldView](https://worldview.earthdata.nasa.gov/?v=-99.09292764857383,18.857870096514613,-73.64517039265459,32.90845125461138&l=Reference_Labels_15m(hidden),Reference_Features_15m(hidden),Coastlines_15m,OCI_PACE_Chlorophyll_a,VIIRS_NOAA20_CorrectedReflectance_TrueColor(hidden),VIIRS_SNPP_CorrectedReflectance_TrueColor(hidden),MODIS_Aqua_CorrectedReflectance_TrueColor(hidden),MODIS_Terra_CorrectedReflectance_TrueColor&lg=true&t=2024-06-07-T14%3A54%3A38Z) to compare our calculated Chlorophyll_a to their products and tweak our code and displays as needed. WorldView is an impressive product from NASA. 
---


## Additional Resources
The repository also includes the original PACE Access Data notebook, as proposed by the Ocean Color Instrument (OCI), for users who wish to explore further:

    Authors: Anna Windle (NASA, SSAI), Ian Carroll (NASA, UMBC), Carina Poulin (NASA, SSAI)

This is the original Notebook supplied by NASA. 
---

By using these tools, STS and citizen scientists aim to be part of the solution in detecting, tracking and hopefully mitigating the impacts of Red Tide through advanced hyperspectral data analysis.



> **PREREQUISITES**
>
> This notebook has the following prerequisites:
> - An **<a href="https://urs.earthdata.nasa.gov/" target="_blank">Earthdata Login</a>**
>   account is required to access data from the NASA Earthdata system, including NASA ocean color data.
> - There are no prerequisite notebooks for this module.
