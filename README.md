**English** | [中文](README_CN.md)

# A global rock glacier inventory dataset

Download and usage guide for the dataset hosted at the National Tibetan Plateau / Third Pole Environment Data Center (TPDC).

- **Dataset DOI:** [https://doi.org/10.11888/Cryos.tpdc.303562](https://doi.org/10.11888/Cryos.tpdc.303562)
- **Inventory:** 301,524 mapped polygons across 19 regions, approximately 38,849 km².
- **Format:** GeoPackage (`.gpkg`); WGS 84 geographic coordinates (**EPSG:4326**).
- **Package size:** 490.98 MB.
- **Guide updated:** 30 September 2026.

## 1 Direct download

1. Open the [dataset page](https://doi.org/10.11888/Cryos.tpdc.303562).
2. Click **Download** to open the **FTP account** dialog.
3. Click **Download without login** (**不登录下载**) to start the direct download.
4. Keep the page open while the loading indicator rotates. **No download progress is displayed.**
5. When the save dialog appears, save the compressed archive. Some browsers save it automatically to the default download folder.
6. Extract the archive and open the `global_rgs_epsg4326` folder.

## 2 Download through FTP

1. Open the dataset page and click **Download**.
2. Copy the host, port, username, and password from the **FTP account** dialog.
3. Connect with an FTP client using the displayed settings.
4. Select all files or the files you need: `global_rgs.gpkg` for the global inventory, `global_rgs_XX.gpkg` for regional subsets, and `global_rg_regions.gpkg` for regional boundaries. See the [regional file index](#4-regional-file-index) for region IDs and names.
5. Open the downloaded GeoPackage directly in your GIS software.

If FTP fails, use direct download.

## 3 Package contents

Extracted files:

```text
全球石冰川编目数据集/
└── global_rgs_epsg4326/
    ├── global_rgs.gpkg
    ├── global_rg_regions.gpkg
    ├── global_rgs_01.gpkg
    ├── ...
    └── global_rgs_19.gpkg
```

| File | Contents |
| --- | --- |
| `global_rgs.gpkg` | Complete global inventory of 301,524 polygons |
| `global_rgs_01.gpkg`–`global_rgs_19.gpkg` | 19 regional subsets with the same inventory fields |
| `global_rg_regions.gpkg` | 19 regional boundary features with `fid`, `geom`, and `name` fields |

Each file contains one feature layer, named after the filename without `.gpkg`. Regional subsets retain the original global `fid`, complete geometries, and attributes. Features crossing regional boundaries are assigned to the region with the largest intersection area rather than clipped at the boundary.

The 19 subsets partition the global inventory without duplicate feature IDs. Do not combine the global file with its regional subsets in the same analysis.

## 4 Regional file index

Region names match the boundary file; the stored name for region 15 is truncated. Sizes refer to extracted files (1 MB = 1,000,000 bytes).

| Region ID | Region name as stored | File | Polygons | Extracted size MB |
| --- | --- | --- | ---: | ---: |
| 01 | Alaska | global_rgs_01.gpkg | 30312 | 23.76 |
| 02 | Western Canada and USA | global_rgs_02.gpkg | 41019 | 29.06 |
| 03 | Iceland | global_rgs_03.gpkg | 462 | 0.52 |
| 04 | Svalbard Archipelago | global_rgs_04.gpkg | 109 | 0.2 |
| 05 | Scandinavia | global_rgs_05.gpkg | 1693 | 1.5 |
| 06 | Severny Island | global_rgs_06.gpkg | 147 | 0.22 |
| 07 | Central Europe | global_rgs_07.gpkg | 8903 | 6.65 |
| 08 | Caucasus and Middle East | global_rgs_08.gpkg | 4292 | 3.56 |
| 09 | Central Asia | global_rgs_09.gpkg | 83541 | 69.02 |
| 10 | South Asia West | global_rgs_10.gpkg | 30208 | 27.23 |
| 11 | South Asia East | global_rgs_11.gpkg | 38917 | 35.09 |
| 12 | Southern Andes | global_rgs_12.gpkg | 7136 | 6.59 |
| 13 | New Zealand | global_rgs_13.gpkg | 413 | 0.38 |
| 14 | Western Mediterranean | global_rgs_14.gpkg | 37 | 0.13 |
| 15 | South Georgia and the South Sand | global_rgs_15.gpkg | 74 | 0.16 |
| 16 | Northern Andes | global_rgs_16.gpkg | 2297 | 1.82 |
| 17 | South Greenland | global_rgs_17.gpkg | 865 | 0.81 |
| 18 | North Asia | global_rgs_18.gpkg | 49055 | 37.13 |
| 19 | Arctic Canada South | global_rgs_19.gpkg | 2044 | 1.75 |

## 5 Inventory attribute dictionary

| Field | Storage type | Unit | Meaning |
| --- | --- | --- | --- |
| fid | Integer | — | Feature identifier retained in regional subsets. |
| geom | MultiPolygon | WGS 84 coordinates | Rock glacier polygon geometry. |
| area | Real | km² | Polygon area calculated in the equal-area CRS EPSG:8857. |
| longitude | Real | decimal degrees | Centroid longitude in WGS 84. |
| latitude | Real | decimal degrees | Centroid latitude in WGS 84. |
| elevation | Real | m above sea level | Polygon mean elevation. |
| slope | Real | degrees | Polygon mean slope. |
| aspect | Real | directional class | Modal class of eight compass directions; categorical, not an angle in degrees. |
| maat | Real | °C | Polygon mean annual air temperature. |
| magt | Real | °C | Polygon mean annual ground temperature. |
| precipitation | Real | mm/year | Polygon mean annual precipitation. |
| pzi | Real | dimensionless | Polygon mean Permafrost Zonation Index. |

Missing values are **NULL**, not zero. `aspect` codes are categorical and should not be averaged.

Aspect codes start at east and proceed clockwise:

| Code | Direction |
| --- | --- |
| 1 | East (E) |
| 2 | Southeast (SE) |
| 3 | South (S) |
| 4 | Southwest (SW) |
| 5 | West (W) |
| 6 | Northwest (NW) |
| 7 | North (N) |
| 8 | Northeast (NE) |

For area calculations, use `area` or reproject to an equal-area CRS such as **EPSG:8857**. EPSG:4326 geometry coordinates are in degrees.

## 6 Open and use the files

**QGIS or ArcGIS Pro:** add the required `.gpkg` as a vector layer. Load `global_rg_regions.gpkg` for regional boundaries.

**Python with GeoPandas:**

```python
from pathlib import Path
import geopandas as gpd

folder = Path("path/to/global_rgs_epsg4326")
rgs = gpd.read_file(folder / "global_rgs_01.gpkg", layer="global_rgs_01")
print(rgs.crs)                       # EPSG:4326
print(len(rgs))                      # number of mapped polygons
print(rgs["area"].sum())              # mapped area in km²
print(rgs[["maat", "magt"]].isna().sum())
```

Some readers expose `fid` as an index rather than a column. Preserve it when exporting or joining records.

## 7 Data production and environmental attributes

The inventory was mapped primarily from high-resolution Esri World Imagery, supplemented by Google Satellite imagery. Candidate polygons were generated using an ensemble of UPerNet and Mask2Former semantic segmentation models and subsequently checked and refined through expert manual post-processing.

Environmental attributes are polygon-level raster statistics, with the following sources:

| Attributes | Source | Reference period or spatial resolution |
| --- | --- | --- |
| Elevation, slope, aspect | FABDEM; NASADEM supplementation in the Greater Caucasus | 30 m DEM inputs |
| MAAT, precipitation | CHELSA v2.1 climatology | 1981–2010; 30 arc-seconds |
| MAGT | UiO PEX–MAGT v5.0 | 2000–2016; 2000–2017 for Antarctica; 1 km |
| PZI | Global Permafrost Zonation Index | Based on MAAT for 1961–1990; 30 arc-seconds |

Attributes have different reference periods and spatial resolutions. The inventory is not a time series.

## 8 Quality and interpretation limits

Validation of 3,015 randomly sampled polygons yielded **90.9% precision** (Wilson 95% confidence interval: **89.8%–91.9%**). This measures the reliability of mapped detections, not inventory completeness or boundary accuracy.

The inventory supports occurrence, mapped-area, and environmental analyses. Activity state, velocity, ice content, and ice volume are not included and require additional observations.

## 9 Citation and data use

Cite the dataset using its TPDC record:

> Xu, J., Feng, M., Su, Y., Yan, D., Wu, Q., Zhao, P., Zhang, X., and Li, X. (2026). A global rock glacier inventory dataset. National Tibetan Plateau / Third Pole Environment Data Center. https://doi.org/10.11888/Cryos.tpdc.303562

Data-use terms and contact information are provided on the TPDC dataset page.
