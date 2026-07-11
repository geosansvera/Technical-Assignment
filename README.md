# Technical-Assignment
Sentinel-2 Change Analysis

**Candidate:** Gonzalo Sanson  
**Date:** July 2026  
**Assessment:** Technical Assignment – Geospatial Analytics
---
## Approach
This repository contains a complete geospatial analytics pipeline for detecting land surface changes between two Sentinel-2 images (August 12, 2023 and September 2, 2023) over an open-pit mining area in Zambia.

The pipeline includes:

Part 1: Data preparation
- Load and stack: Load raster files and validates dimension match before stacking bands into a single file per scene
- Stack validation: Quality check of stacking process between scenes
- Mask Invalid Pixels: Remove invalid pixels out of range

Part 2: Change detection
- Spectral index: Calculates NDVI proxy, band ratios and stack
- Normalize Histogram: Applies a radiometric normalization approximation
- Point extract: Extract fields at given coordinates from supervised observation points
- Train classifier: Train a Random Forest model with supervised observations to predict classes in image
- Classify Image: Apply classification model to a stacked
- Change detection: Identifies changes at pixel level using PCA

Part 3: Change feature extraction and storage
- Save raster: Saves results into an array
- Vectorize map: Saves maps in polygon shapefile
- Store database: Saves results in a SQL ready database

Part 4: Visualization
- Visualize change binary: Plot change binary map
- Visualize change classes: Plot classes within detected change areas
- Create interactive map: create a interactive mao using Folium

Part 5: Analysis and interpretation
- Analyze change by class: Returns a dataframe of change area per class
- Generate report: Markdown report generation
---
## Project Structure

data/
- sentinel2_20230812/
- sentinel2_20230902/

output/
- change_analysis.db
- binary_change_map.png
- change_map.tif
- change_area_by_class.png
- change_overlay_rgb.png
- change_map_interactive.html
- report.md
- classification_map.tif
- sentinel2_20230812_stack.tif
- sentinel2_20230902_stack.tif

Points.geojson

aoi.geojson

Technical Assessment.ipynb

README.md

requirements.txt

---
##  How to Run

### 1. Environment Setup

Create a conda environment with Python 3.10+:

```bash
conda create -n geo_env python=3.10
conda activate geo_env
```
Install the required dependencies in ```requirements.txt``` file: 
```bash
numpy
pandas
rasterio
geopandas
matplotlib
seaborn
scikit-learn
shapely
folium
jupyter
```
### 2. Data Preparation
Place the Sentinel-2 data folders in the data/ directory. Ensure that ```Points.geojson``` (supervised points) and ``aoi.geojson`` (area of interest) are in the root directory.

### 3. Run the Pipeline
Open and run the ```Technical Assessment.ipynb``` Jupyter Notebook.

---
##  Results
An automatic report ```report.md``` is generated with the following sections:
1. Executive summary - Brief description of the assignment, statistics of total change area.
2. Methodology - Description of change detection and classification approaches.
3. Results - Analysis and interpretation developed for the assignment.
4. Limitations and recommendations - Identification of inaccuracy causes and mitigation strategies.
5. Conclusions

- Maps: ```Binary_change_map.png```, ```change_overlay_rgb.png```, ```change_map_interactive.html``` (open in browser)
- Database: ```change_analysis.db```

---
## Assumptions
- For area calculations, the CRS of all input data is EPSG:32735 (UTM zone 35S). 
- Pixel size is constant for area calculations.
- Change is consider when the spectral response at pixel level in the second image is high enough that differs significantly from the response in the initial image. 
