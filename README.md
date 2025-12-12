# EDS220 Final Project: Visualizing Biodiversity Decline in Phoenix

## Author
Nathalie Bonnet

## Instructors
Dr. Carmen Galaz Garcia 

Annie Adams

## About
This repository houses the work for [EDS220 final project task2](https://meds-eds-220.github.io/MEDS-eds-220-course/assignments/final-project.html) analyzing the change in biodiversity intactness in the Phoenix, AZ area from 2017 to 2020 in association with rapid urbanization. Microsoft Planetary Computer dataset items with BII (Biodiversity Intactness Index) data were retrieved to estimate the change in species richness and abundance, and geographic information was accessed from data.gov for the state of Arizona. Analysis involved manipulating rioxarray data to find the difference between years, and using matplotlib to visualize BII on a map.

## Repository structure
```
eds220-phoenix-biodiversity
│   README.md
|   phoenix-biodiversity.ipynb
|   .gitignore
    │
    └── data 
```
Within this repository is the Jupyter notebook used for analysis, visualization, and discussion (phoenix-biodiversity.ipynb), and the .gitignore, which contains shapefile used to access Arizona state boundary information.

## Data access
The Arizona TIGER shapefile for 2020 was accessed from data.gov, which is a publicly available database. BII data was sourced from Microsoft Planetary Computer STAC API, which is connected via `pystac_client`. This collection of catalog items is an open source collection of earth science data. The data were accessed using the following Python code:
```
import planetary_computer
from pystac_client import Client

catalog = Client.open(
    "https://planetarycomputer.microsoft.com/api/stac/v1",
    modifier=planetary_computer.sign_inplace,)

catalog.get_collections()


collections = list(catalog.get_collections())
# Phoenix bounding coordinates
bbox = [-112.826843, 32.974108, -111.184387, 33.863574]

# Time range desired
time_range = "2017-01-01/2020-01-01"

# Search with given parameters for BII data
search = catalog.search(
    collections = ["io-biodiversity"], 
    bbox = bbox, 
    datetime = time_range)

# Return items from search
items = search.item_collection()
```
The output of the code is a list of 4 items, 1 dataset for each year between 2017-2020 with BII data. 

## References
### Data
Data.gov. (2021). TIGER/Line Shapefile, 2020, State, Arizona, Census Tracts [Dataset]. https://catalog.data.gov/dataset/tiger-line-shapefile-2020-state-arizona-census-tracts

Microsoft Planetary Computer STAC API. (2017-2020).*io-bidiversity* (BII catalog data) [Catalog].

## Background literature
Z. Levitt and J. Eng, “Where America’s developed areas are growing: ‘Way off into the horizon’,” The Washington Post, Aug. 2021, Available: https://www.washingtonpost.com/nation/interactive/2021/land-development-urban-growth-maps/. [Accessed: Nov. 22, 2024]

F. Gassert, J. Mazzarello, and S. Hyde, “Global 100m Projections of Biodiversity Intactness for the years 2017-2020 [Technical Whitepaper].” Aug. 2022. Available: https://ai4edatasetspublicassets.blob.core.windows.net/assets/pdfs/io-biodiversity/Biodiversity_Intactness_whitepaper.pdf
