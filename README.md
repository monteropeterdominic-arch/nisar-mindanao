# NISAR Mindanao (Region XII + Davao Region)

Key outputs from the QGIS project `nasa2` (NASA NISAR L2 GCOV, SAR-based monitoring). All files are **EPSG:4326**.
The RVI-based layers are a SAR vegetation response indicator, not NDVI. Blank pixels are genuine coverage gaps.

## geotiff/ : full coverage of Region XII + Davao Region (clipped to the boundary)
| File | Meaning |
|---|---|
| `NISAR_Vegetation_Response_FullCoverage_...tif` | SAR vegetation response (RVI proxy), 100% of the boundary covered |
| `NISAR_SAR_Vegetation_Response_Class_FullCoverage_...tif` | Classified SAR vegetation response |
| `NISAR_Water_Response_Terrain_Context_FullCoverage_...tif` | Low-backscatter water context with terrain: 1 flat sea level, 2 flat inland, 3 sloping terrain (possible radar shadow), 4 no DEM coverage |
| `NISAR_Vegetation_Response_Terrain_Context_FullCoverage_...tif` | Vegetation response z-score within elevation x slope strata (terrain-normalised) |
| `NISAR_Vegetation_Change_AllGeometry_FullCoverage_...tif` | Vegetation response change using all dates, ~98% covered. Mixes ascending and descending geometry, so treat as indicative |

## geotiff/same_direction_subset/ : partial coverage by design
Change products only use pairs with the same look direction (Asc 20-25 Sep, Desc 23-30 Sep) so they are not geometry-confounded. NISAR only repeated those footprints over a strip, so they cover only about 11-17% of the boundary.
- `NISAR_Potential_Water_Change_CORRECTED_EPSG4326.tif`
- `NISAR_Vegetation_Change_CORRECTED_EPSG4326.tif`
- `NISAR_Surface_Change_Magnitude_CORRECTED_EPSG4326.tif`
- `NISAR_SAR_Vegetation_Response_Class_CORRECTED_EPSG4326.tif`

## boundary/
`R12_Davao_Regions_EPSG4326.shp` : Region XII (SOCCSKSARGEN) and Region XI (Davao Region).
