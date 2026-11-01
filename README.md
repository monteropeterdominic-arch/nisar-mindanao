# NISAR Mindanao (Region XII + Davao Region), 30 m

Key outputs from the QGIS project `nasa2` (NASA NISAR L2 GCOV, SAR-based monitoring). All files are **EPSG:4326, ~30 m (0.00027 deg)** and clipped to `boundary/R12_Davao_Regions_EPSG4326.shp`.
They were rebuilt from the original 10 m scenes (3x3 multilook to 30 m), not resampled from the earlier 100 m layers. Blank pixels are genuine coverage gaps. RVI-based layers are a SAR vegetation indicator, not NDVI.

## Web map: Sarangani and Davao Occidental (docs/)
An interactive Mapbox GL JS map on the Mapbox Standard style, with an earthquake view (possible landslides after the M7.8 of 8 June 2026), El Niño time series and a 12-month outlook with play sliders (monthly MODIS NDVI and land temperature, quarterly Landsat HLS EVI, NDMI and NDWI, and 2026-vs-2025 anomalies), 3D terrain and day/dusk/night lighting.

To publish it: Settings > Pages > Deploy from a branch > `main` / `/docs`. The map needs a public Mapbox token: put it in `docs/config.js`, or paste it when the page asks. Restrict the token to your Pages URL in your Mapbox account.

### Machine learning (NISAR at the centre)
1. **Radar to vegetation**: gradient-boosted trees predict Landsat HLS EVI and NDMI from NISAR HH/HV (plus texture) and SRTM terrain, trained on 168k pixels where both sensors saw the ground within 8 days. 5 km spatial block cross-validation: EVI R2 0.37 (NISAR only 0.24, terrain only 0.12); NDMI R2 0.32 (NISAR only 0.24, terrain only 0.01). Used to map vegetation and moisture on every NISAR date under cloud.
2. **Land-cover change**: a classifier learns the 2020 land-cover map (8 classes) from June-October NISAR statistics plus terrain. Block CV: 63% (8 classes, kappa 0.57), 88% when grouped into forest / other vegetation / built-up-bare / water, 93% forest vs non-forest (89% NISAR only). Confident disagreement with 2020 is mapped as possible change; optical landslide pixels are flagged as radar change at twice the background rate (28% vs 14%).

### 12-month outlook (Nov 2026 - Oct 2027, strong El Nino scenario)
NOAA CPC (8 Oct 2026) expects a strong to very strong El Nino through early 2027. The **Outlook** tab, plus `Outlook_Nov2026_Oct2027_ElNino_scenario.csv`, gives month-by-month expectations for rainfall, greenness (MODIS EVI/NDVI, 250 m) and daytime land temperature (MODIS, 1 km).
- Rainfall: GPM IMERG V07 over 124.3-125.8E, 5.3-6.7N (monthly Final Run 1998-2025 plus daily Late Run to 30 Oct 2026). El Nino strength (ONI, Nov-Jan) explains Nov-Apr rain: r = -0.81, and leave-one-year-out r = 0.83. With ONI +2.5 C, Nov-Apr is expected at about 41% of normal (341 vs 825 mm; 10-90% range per month in the CSV). In 2015-16 the actual figure was 324 mm.
- Vegetation and heat: for each pixel and month ahead, anomaly = a + b*ONI + c*(Sep-Oct anomaly), after removing that pixel's seasonal cycle and trend (2000-2026). The area-mean EVI dip peaks in Apr 2027 (-0.073; Sarangani -0.084, Davao Occidental -0.055). About 71% of the land has at least a 2-in-3 chance of below-normal greenness by Apr 2027. Daytime ground temperature is about +2.2 C above normal in Apr 2027.
- Hindcasts (each El Nino hidden in turn): the model got the timing and roughly the size of the dips in 2002-03, 2009-10 and 2023-24. It underestimated 2015-16, the strongest event (EVI -0.063 expected vs -0.119 actual; ground +2.4 vs +3.9 C), so a very strong event may hit harder. Pixel-level patterns were only predictable in strong events.
- This is a scenario conditioned on El Nino, not a weather forecast. It cannot anticipate typhoons, fires or land clearing.

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
