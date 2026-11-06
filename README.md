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
  data_engineering_real_estate.ipynb    3  merge scraping runs, assign municipalities, join indicators
  data_cleaning_rentals.ipynb           4  clean listings (6 mandatory cleaning steps, net rent)
  rent_factors_analysis.ipynb           5  analysis RQ1–RQ3 (correlations, regressions, figures)
  report_figures.ipynb                  6  figures 1–4 of the documentation (English labels)
  flatfox_data_analysis.ipynb              first exploratory plots
data/
  flatfox_data/<run>/                   raw JSON and CSV per scraping run
  demographic_data/                     BFS municipal indicators (Swiss Stats Map Explorer CSVs)
                                        and urban/rural typology (raumgliederungen_bfs.xlsx)
  tax_data/                             ESTV direct federal tax statistics 2022
  geodata/                              swissBOUNDARIES3D (download separately, see below)
  data_processed/                       intermediate and final datasets
figures/                                PNG figures used in the documentation (from notebook 6)
```

Main datasets in `data/data_processed/`:

| File | Created by | Content |
|---|---|---|
| `gemeinde_indikatoren_zentralschweiz.csv` | notebook 2 | 157 municipalities, 12 indicators with reference years |
| `flatfox_rentals_zentralschweiz.csv` | notebook 3 | 513 rental listings with municipality and indicators |
| `flatfox_rentals_clean.csv` | notebook 4 | final sample: 332 apartments with net rent |
| `flatfox_rentals_excluded.csv` | notebook 4 | every excluded listing with step and reason |

## Setup and run order

```bash
pip install -r requirements.txt
```

Run the notebooks from inside `scripts/` in the order 1 → 6. All intermediate files are included in the repository, so each notebook can also be run on its own; notebooks 4–6 need neither the scraper nor the geodata.

- **Notebook 1** queries the live Flatfox website and creates a new timestamped run folder; it is only needed to collect new data.
- **Notebook 3** needs the municipal boundaries: download *swissBOUNDARIES3D* (GeoPackage, LV95) from <https://ogd.swisstopo.admin.ch/ch.swisstopo.swissboundaries3d>, unzip it and save the `.gpkg` file in `data/geodata/` (see `data/geodata/README.md`; the file is not versioned because of its size).

## Data sources

- Flatfox.ch rental listings, collected 18–24 October 2026 (used for this course project only)
- Federal Statistical Office (FSO/BFS): municipal indicators, Swiss Stats Map Explorer
- Federal Statistical Office (FSO/BFS): urban/rural typology (Gemeindetypologie 2020, municipality status 1.1.2026), Historisiertes Gemeindeverzeichnis
- Federal Tax Administration (FTA/ESTV): direct federal tax, natural persons, key figures by municipality 2022
- swisstopo: swissBOUNDARIES3D, version 2026-01

## Contributions

Member
Code contribution

Enrique Rüfenacht
`flatfox_scrape.ipynb`, `data_engineering_municipality.ipynb`, `data_engineering_real_estate.ipynb`, `flatfox_data_analysis.ipynb`; project and data folder structure

Jessica Herzog
`data_cleaning_rentals.ipynb` (cleaning, net rent, duplicates, value ranges, outliers, enrichment), `rent_factors_analysis.ipynb` (hypotheses, RQ1–RQ3 analysis and figures), `report_figures.ipynb`, `README.md`, `requirements.txt`

Kymberly Bader