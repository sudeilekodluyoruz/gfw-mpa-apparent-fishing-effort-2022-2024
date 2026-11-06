# GFW MPA Apparent Fishing Effort 2022–2024

A reproducible analysis of Global Fishing Watch (GFW) AIS-based apparent fishing activity within marine protected areas (MPAs) from January 2022 through December 2024. The study focuses on the `CHN` and `UNKNOWN-CHN` flag categories and examines annual and seasonal patterns, spatial concentration, AIS presence, and overlapping MPA boundaries.

> **Interpretation:** “Apparent fishing hours” are model estimates derived from AIS data. They are not direct observations of catch, vessel nationality, illegal fishing, or violations of MPA rules.

## Key findings

- The analysis processed 36 monthly grid files containing 41,862,480 records.
- After correcting for duplicated grid observations in overlapping MPA geometries, the analysis covered 150 analytical units.
- CHN apparent fishing hours increased 6.16% from 2022 to 2024; UNKNOWN-CHN increased 319.62% from a smaller baseline.
- Across all units, combined CHN and UNKNOWN-CHN apparent fishing hours rose by 4,230.13 hours (2.54%) from 2023 to 2024. Approximately 83.7% of this net increase was associated with UNKNOWN-CHN.
- The 2024 data are provisional and may be revised as vessel classifications are updated.

See the Turkish and English manuscript files in [`outputs/`](outputs/).

## Repository contents

The project archive includes:

- Monthly GFW fleet CSV files for 2022–2024, organized in year folders.
- Analysis scripts, including `gfw_analiz.py`, `gfw_kalite.py`, `analyze_mpa_overlap.py`, `analyze_mpa_site_by_flag.py`, and `analyze_guangdong_monthly.py`.
- Generated tables and charts under `outputs/`.
- MPA geometry download and analysis utilities.

The archive also contains exploratory and backup scripts. Review each script’s header and outputs before using it as the source of a reported result. Do not commit the bundled virtual environment, temporary files, or large source datasets by default.

## Setup

Python 3.10 or later is recommended. From the repository root:

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
python -m pip install -r requirements.txt
```

The project dependencies are DuckDB, pandas, Matplotlib, GeoPandas, and Requests. The monthly CSV files should remain in the project tree, in folders matching the archive layout, so the scripts can discover them.

## Running the analyses

The general monthly-data summary and quality report can be generated with:

```bash
python gfw_analiz.py
python gfw_kalite.py
```

To refresh marine protected-area boundaries and run the inside/nearby comparison:

```bash
python download_mpas.py
python analyze_mpa_overlap.py
```

The site-level flag analysis uses the downloaded WDPCA geometry file:

```bash
python analyze_mpa_sitepid_by_flag.py
```

Other scripts provide additional MPA overlap, flag, gear, and Guangdong monthly analyses. Check their source comments for expected inputs and output paths. Large source files and the geometry download can require substantial disk space and processing time.

## Data and methods

- **Fishing-effort data:** GFW monthly fleet apparent fishing effort, version 3, at 0.1-degree grid resolution. Cell coordinates identify the lower-left corner of each grid cell.
- **Protected areas:** Marine protected-area geometries from the UNEP-WCMC/IUCN World Database on Protected and Conserved Areas (WDPCA). Record the download date, exact source URL, and dataset version when reproducing or publishing results.
- **Spatial assignment:** The project’s MPA comparison scripts assign grid cells using cell-center point-in-polygon matching. A cell center inside an MPA does not mean the entire cell lies within its boundary. Some analyses group intersecting MPA records and deduplicate shared cell observations to reduce double-counting.
- **Categories:** `UNKNOWN-CHN` is a GFW classification and does not establish Chinese vessel flag, ownership, or nationality.

For background on the GFW data product and its known issues, see the [GFW version 3 dataset record](https://zenodo.org/records/14982712) and [data caveats](https://api-doc.globalfishingwatch.org/our-apis/documentation/docs/v3/general-api-doc/data-caveats). For WDPCA terms, consult the [official data license](https://www.unep-wcmc.org/en/wdpa-data-license). Confirm current source terms before redistributing downloaded datasets or derived products.

## Limitations

AIS participation and reception are incomplete and vary across places and time. Vessel identity and flag classifications can be uncertain, model estimates can contain false positives or omissions, and 0.1-degree cells limit boundary precision. Results are descriptive and do not establish causal effects or MPA effectiveness. MPA regulations, zoning, and effective dates differ, so apparent activity inside a polygon is not by itself evidence of a legal violation.

## Manuscripts

- [Turkish manuscript (DOCX)](outputs/Sude_Nur_Kilic_GFW_MPA_Makale_Genisletilmis_Duzenlenmis.docx) · [PDF](outputs/Sude_Nur_Kilic_GFW_MPA_Turkce.pdf)
- [English manuscript (DOCX)](outputs/Sude_Nur_Kilic_GFW_MPA_Expanded_English.docx) · [PDF](outputs/Sude_Nur_Kilic_GFW_MPA_English.pdf)

## Suggested repository name

`gfw-mpa-apparent-fishing-effort-2022-2024`
