# NISAR Mindanao (Region XII + Davao Region), 30 m

Key outputs from the QGIS project `nasa2` (NASA NISAR L2 GCOV, SAR-based monitoring). All files are **EPSG:4326, ~30 m (0.00027 deg)** and clipped to `boundary/R12_Davao_Regions_EPSG4326.shp`.
They were rebuilt from the original 10 m scenes (3x3 multilook to 30 m), not resampled from the earlier 100 m layers. Blank pixels are genuine coverage gaps. RVI-based layers are a SAR vegetation indicator, not NDVI.

## geotiff_30m/ : full coverage of the boundary
| File | Meaning |
|---|---|
| `NISAR_Vegetation_Response_30m_...tif` | SAR vegetation response (RVI proxy 4HV/(HH+HV), mean over valid dates) |
| `NISAR_SAR_Vegetation_Response_Class_30m_...tif` | 0 single observation, 1 stable, 2 increasing, 3 decreasing, 4 high variability, 5 indeterminate |
| `NISAR_Vegetation_Response_Terrain_Context_30m_...tif` | Vegetation response z-score within elevation x slope strata |
| `NISAR_Water_Response_Terrain_Context_30m_...tif` | Low-backscatter pixels: 1 flat sea level, 2 flat inland, 3 sloping (possible radar shadow), 4 no DEM |
| `NISAR_Vegetation_Change_AllGeometry_30m_...tif` | RVI change, latest minus earliest valid date; mixes ascending and descending geometry, so indicative only |

Three files are stored as 16-bit integers to stay under GitHub's 100 MB limit. QGIS and GDAL apply the stored scale factor automatically (1/10000 for vegetation response and change, 1/1000 for the terrain z-score). Nodata is -32768.

## geotiff_30m/same_direction_subset/ : partial coverage by design
Only pairs with the same look direction are used (Asc 20-25 Sep, Desc 23-30 Sep), so these cover a strip, about 10-17% of the boundary. Note: the two dates in each pair are on different tracks (39 vs 111, and 75 vs 3), so the viewing angle still differs. Treat these as indicative. A same-track pairing (12-day repeat) is planned.
- `NISAR_Vegetation_Change_SameDirection_30m_...tif`
- `NISAR_Potential_Water_Change_SameDirection_30m_...tif` (0 no change, 1 possible, 2 strong)
- `NISAR_Surface_Change_Magnitude_SameDirection_30m_...tif` (robust Mahalanobis distance; breaks 2.448 and 3.717)

## Method notes
Water thresholds are the scene-wide histogram-valley values from the 100 m run. At 30 m there is more speckle, so robust change sigmas were recomputed at 30 m and are larger than at 100 m. Edge-noise buffers are unchanged.

## legacy_100m_geotiff/
Earlier 100 m versions, kept for reference.

## boundary/
Region XII (SOCCSKSARGEN) and Region XI (Davao Region) in EPSG:4326.
