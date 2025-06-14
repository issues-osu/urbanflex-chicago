# Chicago Urbanization & Firearm Incidents Analysis

This repository implements a reproducible workflow for mapping and analyzing urban form, neighborhood deprivation, greenness, and firearm‐related shootings across Cook County, IL, using the Flexurba R package and a variety of open datasets.

---

## 📂 Repository Structure
```
├── data/                       # Raw input data (rasters, shapefiles)
│   ├── GHS_POP_*.tif           # Copernicus population tiles
│   ├── GHS_BUILT_S_*.tif       # Copernicus built-up tiles
│   ├── GHS_LAND_*.tif          # Copernicus land-fraction tiles
│   ├── VNL_*.tif               # VIIRS nighttime-lights composite
│   ├── LC09_*_B4.TIF           # Landsat red band
│   ├── LC09_*_B5.TIF           # Landsat NIR band
│   └── gun_violence_nonfatal.shp  # Chicago shooting incidents
│
├── scripts/                    # Analysis scripts
│   ├── 01_prepare_data.R       # Download & preprocess CBGs, rasters
│   ├── 02_compute_indices.R    # Urbanization, NDVI, ADI extraction
│   ├── 03_firearm_SIR.R        # Shooting counts & standardized incidence ratio
│   └── 04_flexurba_DoU.R       # Degree of Urbanisation grid & units classification
│
├── output/                     # Generated outputs
│   ├── cbg_chicago_metrics.shp # Block-group shapefile with all metrics
│   ├── cbg_SIR_2020_2024.shp    # SIR maps per year
│   └── plots/                  # PNG/PDF figures
│
├── README.md                   # This file
└── .gitignore                  # Ignore raw data, .Rproj.user, etc.
```
---
## 🛠️ Prerequisites

- **R ≥ 4.0** with the following packages:
  - `flexurba`, `flexurbaData`  
  - `tigris`, `sf`, `terra`, `dplyr`, `purrr`, `exactextractr`  
  - `sociome`, `tidycensus`, `lubridate`, `tidyr`  
- A valid **Census API key** (for ACS population).  
- Local copy of raw GeoTIFFs under `data/` (GHS-POP, GHS-BUILT-S, GHS-LAND, VIIRS, Landsat).  
- Shooting incident shapefile downloaded from Chicago Data Portal.

---

## 🚀 Installation

1. **Clone** this repository:
   ```bash
   git clone https://github.com/yourusername/urbanization-flexurba.git
   cd urbanization-flexurba

install.packages(c(
  "flexurba", "flexurbaData", "tigris", "sf", "terra", "dplyr",
  "purrr", "exactextractr", "sociome", "tidycensus",
  "lubridate", "tidyr"
))

library(tidycensus)
census_api_key("YOUR_KEY_HERE", install = TRUE)

## Data Sources

| Data Source                                    | Variable                                           | URL                                                                                                                                                      |
| ---------------------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **USGS EarthExplorer**                         | NDVI (Landsat 9 surface reflectance bands)         | [https://earthexplorer.usgs.gov/](https://earthexplorer.usgs.gov/)                                                                                       |
| **Copernicus GHS-POP**                         | Population density (inh/km²)                       | [https://human-settlement.emergency.copernicus.eu/download.php?ds=pop](https://human-settlement.emergency.copernicus.eu/download.php?ds=pop)             |
| **Copernicus GHS-BUILT-S**                     | Built-up area (m² built per km² cell)              | [https://human-settlement.emergency.copernicus.eu/download.php?ds=built-s](https://human-settlement.emergency.copernicus.eu/download.php?ds=built-s)     |
| **Earth Observation Group (Payne Institute)**  | Nighttime lights radiance (VIIRS DNB, nW/cm²/sr)   | [https://eogdata.mines.edu/products/vnl/](https://eogdata.mines.edu/products/vnl/)                                                                       |
| **University of Wisconsin Neighborhood Atlas** | Area Deprivation Index (ADI)                       | [https://www.neighborhoodatlas.medicine.wisc.edu/](https://www.neighborhoodatlas.medicine.wisc.edu/)                                                     |
| **U.S. Census Bureau (via tidycensus / API)**  | Block-group total population (Decennial & ACS5)    | [https://api.census.gov/data.html](https://api.census.gov/data.html)                                                                                     |
| **Chicago Data Portal (Socrata)**              | Non-fatal firearm incidents (People Shot, Chicago) | [Chicago Open Data](https://data.cityofchicago.org/)  |
