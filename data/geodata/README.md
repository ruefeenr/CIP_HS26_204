# Geodata

## swissBOUNDARIES3D (not in the repository)

Needed by `scripts/data_engineering_real_estate.ipynb` to assign each listing to a municipality. The file is not versioned because of its size (about 74 MB).

1. Download the GeoPackage (LV95) from <https://ogd.swisstopo.admin.ch/ch.swisstopo.swissboundaries3d>: `swissboundaries3d_2026-01_2056_5728.gpkg.zip`.
2. **Unzip** the ZIP file. The notebook cannot read the ZIP file directly.
3. Save the unzipped file `swissBOUNDARIES3D_1_5_LV95_LN02.gpkg` in this folder.

The notebook uses the only `.gpkg` file in this folder, regardless of its name. If there is no `.gpkg` file or more than one, it stops with an error message.

Version used in the project:

| | |
|---|---|
| Release | 2026-01 (data model 1.5, LV95 / LN02, EPSG:2056) |
| File | `swissBOUNDARIES3D_1_5_LV95_LN02.gpkg` (74,231,808 bytes) |
| SHA-256 | `1f122cb7a06f2d312a84b7c0a91116348ba907054d487f0a70b9d2302984e6fc` |

Check on macOS/Linux: `shasum -a 256 swissBOUNDARIES3D_1_5_LV95_LN02.gpkg`

## Postal code directory (in the repository)

`ortschaftenverzeichnis_plz.csv` is included in the repository. Source: official swisstopo postal code directory, <https://data.geo.admin.ch/ch.swisstopo-vd.ortschaftenverzeichnis_plz/ortschaftenverzeichnis_plz/ortschaftenverzeichnis_plz_2056.csv.zip> (file `AMTOVZ_CSV_LV95.csv`).
