# Interpretable Machine Learning for CMIP6 Multi-Model Ensembles

This repository contains the analysis code, notebooks, and scripts used to produce all results
and figures in the paper:

> **Interpretable Machine Learning for CMIP6 Multi-Model Ensembles**  
> Siyi Wu, Steve Easterbrook, University of Toronto
> *Climate Informatics*, 2026 

---

## Overview

We develop an interpretable ensemble learning framework for CMIP6 climate model post-processing.
The framework compares four ensemble strategies against ERA5 reanalysis as the observational
reference:

- **AMME** — arithmetic multi-model ensemble (baseline)
- **Ridge** — global linear weighting with L2 regularisation
- **Random Forest (RF)** — nonlinear ensemble mapping
- **Gating** — continuous mixture of Ridge and RF, learned on validation data

Analyses cover near-surface temperature (`tas`) and precipitation (`pr`) at monthly resolution
over the historical period 1948–2014, with model evaluation on a held-out test period (2006–2014).

---

## Repository structure

```
.
├── mme.ipynb                  # Main analysis notebook (all sections, all figures)
├── requirements.txt           # Python dependencies with exact versions
├── README.md                  # This file
├── LICENSE                    # MIT licence
```

The notebook is self-contained and reproduces all tables and figures in the paper when run
sequentially. 

---

## Data

This study uses two publicly available datasets. **The raw data are not included in this
repository** due to their size (~5 GB). Download instructions are provided below.

### CMIP6 model outputs

Monthly near-surface temperature (`tas`) and precipitation (`pr`) from 25 CMIP6 models,
regridded to a common 1° × 1° grid, covering 1948–2014.

- **Source:** Earth System Grid Federation (ESGF)  
  https://esgf-node.llnl.gov/
- **Citation:** Eyring et al. (2016), *Geoscientific Model Development*,
  https://doi.org/10.5194/gmd-9-1937-2016
- **Expected file:** `cmip6.nc` — dimensions `(model, time, lat, lon)`

### ERA5 reanalysis

Monthly 2-metre temperature and total precipitation, regridded to 1° × 1°, covering 1948–2014.

- **Source:** Copernicus Climate Data Store (CDS)  
  https://cds.climate.copernicus.eu/
- **Citation:** Hersbach et al. (2020), *Quarterly Journal of the Royal Meteorological Society*,
  https://doi.org/10.1002/qj.3803
- **Expected files:** `era_1res.nc` (temperature), `era5pr_1res.nc` (precipitation)

### Setting data paths

Update the file paths in **cell 1** of the notebook to point to your local copies:

```python
ds_cmip   = xr.open_dataset('/path/to/cmip6.nc',      chunks={'time': 12})
ds_era    = xr.open_dataset('/path/to/era_1res.nc',    chunks={'time': 12})
ds_era5pr = xr.open_dataset('/path/to/era5pr_1res.nc', chunks={'time': 12})
```

---

## Installation

### Requirements

- Python 3.11 (tested; Python 3.10–3.12 should also work)
- All dependencies listed in `requirements.txt`

### Setup

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME

# Create and activate a virtual environment (recommended)
python3.11 -m venv venv
source venv/bin/activate        # macOS/Linux
# venv\Scripts\activate         # Windows

# Install dependencies
pip install -r requirements.txt

# Launch the notebook
jupyter notebook mme.ipynb
```

### Cartopy note

Cartopy requires GEOS and PROJ system libraries. On macOS with Homebrew:

```bash
brew install geos proj
pip install cartopy==0.25.0
```

On Linux (Ubuntu/Debian):

```bash
sudo apt-get install libgeos-dev libproj-dev
pip install cartopy==0.25.0
```

---


### Output files

Running the notebook produces the following figure files in the working directory:

| File | Figure in paper |
|------|----------------|
| `figure1_clustering.png` | Figure 1 — CMIP6 clustering |
| `dominant_cluster.png` / `dominant_model.png` | Figure 2 — Dominant cluster/model maps |
| `spatial_skill.png` | Figure 3 — Spatial skill improvement |
| `figure4_trust_entropy.png` | Figure 4 — Trust entropy and N_eff |
| `figure5_timeseries_weights.png` | Figure 5 — Time series with weight shading |
| `figure6a_gate_timeseries.png` | Figure 6a — Seasonal gate analysis |
| `figure6b_gate_map.png` | Figure 6b — Spatial gate distribution |
| `figure7_comparison_maps.png` | Figure 7 — Ridge vs RF vs Gating comparison |
| `figure8_ridge_vs_rf_weights.png` | Figure 8 — Per-model weight comparison |



---

## Licence

This code is released under the [MIT Licence](LICENSE).

The CMIP6 and ERA5 data are subject to their respective provider terms of use (ESGF and
Copernicus CDS). Processed datasets derived from those sources follow the same terms.
