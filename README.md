<img src="invaskog-logo.png" alt="InvaSkog moth logo" width="120">

# InvaSkog

A map-first R Shiny application for forest-owner preparedness in Sweden. Explore reported invasive forest-pest occurrences and experimental climate-suitability projections.

**[Open InvaSkog · Beta](https://01a163e0-ca46-aaf0-dcb9-f416353e9de4.share.connect.posit.cloud/)**

The beta is online for testing. Feedback from forest owners will help improve usability and interpretation.

Developed by **Alexandra Nagy**, with support from **LNU Innovation**.

## Explore the map

- Live GBIF human-observation records, with a refresh control.
- Emerald ash borer (*Agrilus planipennis*), oak processionary moth (*Thaumetopoea processionea*) and mountain pine beetle (*Dendroctonus ponderosae*).
- Baseline climate suitability for 2001–2020.
- Future suitability for 2041–2060, SSP2-4.5: cellwise median of three climate-model projections.
- Optional climate-model disagreement: maximum minus minimum suitability.
- Research comparison of 200 km and 500 km background definitions.
- Esri satellite imagery, adjustable suitability opacity and coordinate-based location lookup.
- Species photographs and EPPO links; detailed explanations under How it works.

## Run locally

Run commands from the repository root. R and an internet connection are required; the spatial packages sf and terra also require compatible system libraries.

Once renv has been initialized and its lockfile committed:

```r
install.packages("renv") # if not already installed
renv::restore()
shiny::runApp(".")
```

The app reads packaged prediction rasters. Running the app does not require Java MaxEnt, raw climate downloads or model refitting. Internet access is needed for GBIF retrieval and satellite tiles.

## Deployment files

| Path | Purpose |
| --- | --- |
| app.R | Shiny application, styling and embedded visual assets |
| data/model_catalogue.csv | Model metadata, relative raster paths and SHA-256 checksums |
| data/models/ | 18 prediction rasters |
| data/boundaries/sweden.geojson | Sweden boundary |
| renv.lock | Recorded R package versions; generated on the tested machine |
| renv/activate.R and .Rprofile | Project dependency activation |

The catalogue's **file** column contains runtime paths. Its **source_file** column records original modelling output locations; those source directories are not required to run the app.

This is the deployment repository. Raw climate data, training runs, backups and exploratory scripts remain in the separate research workspace. This repository alone does not reproduce model training.

## Models and interpretation

| Species | Occupied training cells | Predictors | MaxEnt features | Regularization |
| --- | ---: | --- | --- | --- |
| EAB | 2,131 | BIO6, BIO10 | Linear + quadratic | 1 |
| OPM | 2,383 | BIO5, BIO6, BIO16, BIO17 | Linear + quadratic | 1 |
| MPB | 26 | BIO6, BIO10 | Linear | 1 |

Suitability is displayed on a fixed 0–1 scale. It is **not the probability of pest arrival, establishment or damage**, and does not account for all factors affecting forest risk. The MPB model is exploratory and has weak performance in some held-out regions.

The provisional baseline is a custom 2001–2020 climatology at 2.5 arc minutes, built from CRU-TS 4.09 monthly data downscaled and bias corrected with WorldClim 2.1. The preparation scripts average each calendar month over 20 years, apply quality screening and derive bioclimatic variables.

Future inputs are WorldClim-downscaled CMIP6 data for EC-Earth3-Veg, MPI-ESM1-2-HR and MRI-ESM2-0. Saved models are projected with clamping; the displayed median and range are calculated across their suitability projections. Future inputs were not rebased to the custom baseline, so baseline–future differences can include differences between climate products.

The configured projection input paths have been checked against preparation scripts. End-to-end verification of the training climate-extraction chain remains outstanding; this is not a complete provenance audit.

The inland-water mask is a display-only mask based on generalized Natural Earth lakes. It does not change the original prediction files and does not cover every small water body.

GBIF observations reflect reporting effort and data quality as well as species distribution. They are not a systematic survey. EAB and MPB queries cover all years worldwide; OPM queries cover the last five calendar years within the app's European viewing region. The displayed subset is capped at 3,000 records. Disputed MPB record 5018499248 is excluded.

## Sources and reuse

See [CREDITS.md](CREDITS.md) for photographs, climate sources and scientific references. Third-party assets retain their respective terms. No project-wide software licence has been selected yet.

## Contact

Alexandra Nagy — [nagy.alexandra9519@gmail.com](mailto:nagy.alexandra9519@gmail.com)
