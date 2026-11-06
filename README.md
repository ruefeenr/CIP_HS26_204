# CIP HS26 – Group 204

**The Relationship Between Socioeconomic Factors and Net Asking Rents in Central Switzerland**

Course: Data Collection, Integration and Preprocessing (HSLU, HS26)
Group: Kymberly Bader, Enrique Rüfenacht, Jessica Herzog

## Project

We combine rental listings scraped from [Flatfox.ch](https://flatfox.ch) with official municipal indicators (FSO/BFS, FTA/ESTV) for the cantons of Lucerne, Uri, Schwyz, Obwalden, Nidwalden and Zug, and examine:

1. Which socioeconomic and regional variables show the strongest individual relationships with asking rents?
2. Which of them are most strongly associated with asking rents when housing characteristics (size, rooms) are also considered?
3. How stable are these relationships across urban and rural municipalities?

The target variable is the monthly **net rent per m²**. Results describe statistical associations, not causal effects.

## Repository structure

```
scripts/
  flatfox_scrape.ipynb                  1  collect Flatfox listings (map tiles + listing details)
  data_engineering_municipality.ipynb   2  build municipal indicator dataset (BFS, ESTV)
  data_engineering_real_estate.ipynb    3  merge scraping runs, assign municipalities, join indicators (no listing filters)
  data_cleaning_rentals.ipynb           4  all listing filters and cleaning (6 mandatory cleaning steps, net rent)
  rent_factors_analysis.ipynb           5  analysis RQ1–RQ3 (correlations, regressions, figures)
  report_figures.ipynb                  6  figures 1–4 of the documentation (English labels)

data/
  flatfox_data/<run>/                   raw JSON and CSV per scraping run
  demographic_data/                     BFS municipal indicators (Swiss Stats Map Explorer CSVs)
                                        and urban/rural typology (raumgliederungen_bfs.xlsx)
  tax_data/                             ESTV direct federal tax statistics 2022
  geodata/                              swissBOUNDARIES3D (download separately, see below)
                                        and swisstopo postal code directory (ortschaftenverzeichnis_plz.csv)
  data_processed/                       intermediate and final datasets
figures/                                PNG figures used in the documentation (from notebook 6)
```

Main datasets in `data/data_processed/`:

| File | Created by | Content |
|---|---|---|
| `gemeinde_indikatoren_zentralschweiz.csv` | notebook 2 | 157 municipalities, 12 indicators with reference years |
| `flatfox_rentals_zentralschweiz.csv` | notebook 3 | all 2640 listings in Central Switzerland (all offer types) with municipality and indicators |
| `flatfox_rentals_clean.csv` | notebook 4 | final sample: 510 unfurnished rental apartments with net rent |
| `flatfox_rentals_excluded.csv` | notebook 4 | every excluded listing with step and reason |

## Setup and run order

```bash
pip install -r requirements.txt
```

Run the notebooks from inside `scripts/` in the order 1 → 6. All intermediate files are included in the repository, so each notebook can also be run on its own; notebooks 4–6 need neither the scraper nor the GeoPackage.

- **Notebook 1** queries the live Flatfox website and creates a new timestamped run folder; it is only needed to collect new data.
- **Notebook 3** needs the municipal boundaries: download *swissBOUNDARIES3D* (GeoPackage, LV95, `swissboundaries3d_2026-01_2056_5728.gpkg.zip`) from <https://ogd.swisstopo.admin.ch/ch.swisstopo.swissboundaries3d>, **unzip** it and save the contained `swissBOUNDARIES3D_1_5_LV95_LN02.gpkg` in `data/geodata/`. The notebook finds the only `.gpkg` file in that folder automatically and stops with an error message if there is none or more than one. Version and SHA-256 checksum are documented in `data/geodata/README.md`; the file is not versioned because of its size.

## Data sources

- Flatfox.ch rental listings, collected 18 October – 6 November 2026 (used for this course project only)
- Federal Statistical Office (FSO/BFS): municipal indicators, Swiss Stats Map Explorer
- Federal Statistical Office (FSO/BFS): urban/rural typology (Gemeindetypologie 2020, municipality status 1.1.2026), Historisiertes Gemeindeverzeichnis
- Federal Tax Administration (FTA/ESTV): direct federal tax, natural persons, key figures by municipality 2022
- swisstopo: swissBOUNDARIES3D, release 2026-01 (data model 1.5, `swissBOUNDARIES3D_1_5_LV95_LN02.gpkg`)
- swisstopo: Amtliches Ortschaftenverzeichnis mit Postleitzahl (official postal code directory), used to check listing coordinates against the postal code

## Contributions

| Member | Code contribution |
|---|---|
| Enrique Rüfenacht | `flatfox_scrape.ipynb`, `data_engineering_municipality.ipynb`, `data_engineering_real_estate.ipynb`; project and data folder structure |
| Jessica Herzog | `data_cleaning_rentals.ipynb` (cleaning, net rent, duplicates, value ranges, outliers, enrichment), `rent_factors_analysis.ipynb` (hypotheses, RQ1–RQ3 analysis and figures), `report_figures.ipynb`, `README.md`, `requirements.txt` |
| Kymberly Bader | |