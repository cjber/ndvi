# Sentinel Data Processing

This project processes Sentinel data to calculate vegetation indices and compile statistics for Local Super Output Areas (LSOAs). The output is saved as a Parquet file containing NDVI and EVI statistics for each LSOA.

![](./img/nvdi_mean.png)

## Cloning the Repository

To clone the repository, use the following command:

```bash
git clone https://github.com/cjber/ndvi.git
cd ndvi
```

## Prerequisites

- Python 3.8 or higher
- Required Python packages: fastparquet, fiona, gdal, geopandas, ipdb, ipython, mapclassify, matplotlib, polars, pyarrow, pyqt6, rasterio, rtree, tqdm

## Setup

1. Clone the repository and navigate to the project directory.

2. Install the required Python packages using `uv` or `pip`:

   ```bash
   uv sync
   ```

   Alternatively, you can use pip:

   ```bash
   pip install .
   ```

3. Ensure the Sentinel raster data and shapefiles are placed in the appropriate directories as specified in `utils.py`.

## Data Files

- **Raster Data**: Place all Sentinel raster files in the `gisdata/13` directory.
- **Shapefiles**: Ensure the following shapefiles are available:
  - Northern Ireland Data Zones: `gisdata/geography-dz2021-esri-shapefile.zip`
  - Scottish Data Zones: `gisdata/SG_DataZoneBdry_2022.zip`
  - LSOA Boundaries: `gisdata/gov/LSOA2021/LSOA_2021_EW_BFC_V8.shp`

## Processing Steps

1. **Read LSOA Boundaries**: The script reads and combines LSOA boundary data from Northern Ireland, Scotland, and England/Wales.

2. **Create R-tree Index**: An R-tree index is created for efficient spatial querying of raster files.

3. **Find Overlapping Rasters**: For each LSOA, the script identifies raster files that overlap with the LSOA geometry.

4. **Process Rasters and Remove Noise**: The script processes each overlapping raster to extract NDVI and EVI values and remove noise:
   - NDVI is calculated using the red and NIR bands.
   - EVI is calculated using the NIR, red, and green bands.
   - Noise is reduced by detecting and removing outliers using Mahalanobis distance, and applying a median filter to smooth the NDVI values.

5. **Compile Statistics**: For each LSOA, the script compiles statistics such as mean, median, standard deviation, and vegetation fraction for NDVI and EVI.

6. **Save Results**: The compiled statistics are saved to a Parquet file (`gisdata/ndvi.parquet`).

## Running the Script

To run the script and generate the output file, execute:

```bash
python main.py
```

## Visualizing Results

After running the script, you can visualize the results using the following command:

```bash
python demo.py
```

This will display a plot of the mean NDVI values for each LSOA.
