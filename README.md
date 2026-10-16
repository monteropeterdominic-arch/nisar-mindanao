# NISAR Mindanao (Region XII + Davao Region)

Key outputs from the QGIS project `nasa2` (NASA NISAR L2 GCOV, SAR-based change monitoring). All files are in **EPSG:4326**.

## geotiff/
| File | Meaning |
|---|---|
| `NISAR_Potential_Water_Change_CORRECTED_EPSG4326.tif` | Potential water change classes (SAR low-backscatter based; uint8, nodata 255) |
| `NISAR_Vegetation_Change_CORRECTED_EPSG4326.tif` | Change in SAR vegetation response (RVI proxy, same look direction; float32) |
| `NISAR_SAR_Vegetation_Response_Class_CORRECTED_EPSG4326.tif` | Classified SAR vegetation response (uint8, nodata 255) |
| `NISAR_Surface_Change_Magnitude_CORRECTED_EPSG4326.tif` | Robust Mahalanobis surface change magnitude (float32) |

Blank pixels are genuine coverage gaps. The RVI proxy is a SAR vegetation indicator, not NDVI.

## boundary/
`R12_Davao_Regions_EPSG4326.shp` : Region XII (SOCCSKSARGEN) and Region XI (Davao Region).
