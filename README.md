# TEFAAnalysis

`TEFAAnalysis` is a reproducible analysis pipeline for the Texas Education Freedom
Account school finder. It pulls the public Texas Comptroller vendor feed, normalizes
the dataset into analysis-friendly tables, enriches vendors with Texas Education
Agency boundary context, and publishes a workbook suitable for policy research and
public-interest review.

## What the Repository Produces

The project is designed to answer a practical question: what does the public Texas
EFA/TEFA school finder actually contain, and how can that information be reviewed
in a structured spreadsheet rather than a web UI alone?

The pipeline produces:

- a consolidated Excel workbook for analysts
- normalized CSV exports for downstream use
- TEA boundary enrichment such as ESC region, county, and district
- a published workbook copy for GitHub Pages or direct sharing

## Data Sources

- TEFA Finder (Texas Comptroller): <https://finder.educationfreedom.texas.gov/>
- Primary vendor feed: <https://finder.educationfreedom.texas.gov/data/tx/vendors.json>
- Config metadata: <https://finder.educationfreedom.texas.gov/data/tx/config.js>
- TEA boundary data: <https://schoolsdata2-tea-texas.opendata.arcgis.com/>

## Repository Layout

```text
.
├── data/tea/                     # Cached TEA boundary data
├── docs/                         # Published workbook and static download page
├── output/tx_efa_finder/         # Normalized CSV and workbook outputs
├── scripts/                      # Scraping, workbook build, and helper utilities
└── README.md
```

## Primary Outputs

- `output/tx_efa_finder/tx_efa_finder.xlsx`
  The main workbook used for review and analysis
- `output/tx_efa_finder/tx_vendors_enriched.csv`
  Vendor-level dataset with TEA boundary enrichment
- `docs/tx_efa_finder.xlsx`
  Published copy of the workbook for download or static hosting
- `output/tx_efa_finder/*.csv`
  Supporting normalized tables for vendor types, specialties, features, and metadata

Archive snapshots are also kept for comparison and recordkeeping.

## Quick Start

### 1. Create an environment

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
```

### 2. Refresh the raw TEFA data

```bash
python3 scripts/scrape_tx_efa_finder.py
```

### 3. Build the workbook and publishable outputs

```bash
.venv/bin/python scripts/build_tx_efa_workbook.py
```

To refresh cached TEA boundary files at the same time:

```bash
.venv/bin/python scripts/build_tx_efa_workbook.py --refresh-boundaries
```

## Workbook Structure

The workbook is intended to be readable by policy staff, researchers, and spreadsheet-first reviewers.

Typical sheets include:

- an overview sheet with generation metadata
- a vendor sheet with enrichment fields
- normalized supporting tables for features, specialties, and vendor types
- configuration and field-inventory tables for data interpretation

Sheets are formatted as Excel tables with frozen headers to support filtering and review.

## Why the Boundary Enrichment Matters

The Comptroller feed is useful on its own, but boundary enrichment makes it much
easier to answer questions such as:

- which ESC region a vendor falls into
- which school district or county a vendor is associated with
- how the vendor distribution looks across Texas geography

That turns the dataset from a simple finder export into something more suitable
for policy and oversight work.

## Reproducibility Notes

- The Comptroller feed is the authoritative source for listed vendors.
- TEA boundary data is cached locally under `data/tea/` so geographic joins are reproducible.
- Published outputs in `docs/` and `output/` are derived artifacts and can be regenerated from source data plus cached boundaries.

## Optional AskTED Mapping Helper

If you need a county-to-ESC mapping from AskTED exports, use:

```bash
python3 scripts/build_county_esc_mapping.py \
  --input /path/to/askted_district_and_site_directory.csv
```

## GitHub Pages

The `docs/` directory contains a static download surface for the latest workbook.
If GitHub Pages is enabled for the repository, point it at `/docs` to publish a
simple download page.
