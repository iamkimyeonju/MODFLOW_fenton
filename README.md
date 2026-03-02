# UConn MODFLOW Lab — ERTH 4750

**Author:** Lijing Wang (lijing.wang@uconn.edu) — University of Connecticut

Co-authored with Claude ai. 

This repository contains a progressive series of Jupyter notebooks that build a data-driven MODFLOW 6 groundwater model for the UConn Forest catchment, from raw data download through steady-state and transient simulation.

---

## Repository Structure

```
UConn_MODFLOW_Lab/
├── 00_MODFLOW_Installation.ipynb       # Install and compile MODFLOW 6
├── 01_Run_Steady_State_Model.ipynb     # Introductory steady-state example
├── 02_Model_Calibration_and_Prediction.ipynb  # Calibration concepts
└── UConn_Forest/
    ├── 03_Model_Input_Data_Prep.ipynb  # Download all spatial inputs
    ├── 04_Recharge_from_Baseflow_Separation.ipynb  # Eckhardt baseflow filter
    ├── 05_Steady_State_MODFLOW.ipynb   # Data-driven steady-state model
    ├── 06_Transient_MODFLOW.ipynb      # Monthly transient model with UZF
    ├── UConn_forest.shp                # Watershed boundary (WGS84)
    ├── Pumping_well.shp                # Hypothetical pumping well location
    ├── HBV_inputs_UConn_Forest/        # Climate data (P, T, PET)
    │   ├── inputPrecipTemp.csv         # Daily precipitation and temperature
    │   └── inputMonthlyTempEvap.csv    # Monthly mean T and PET
    └── model_inputs/                   # Pre-processed 10 m rasters
        ├── dem_10m.tif                 # USGS 3DEP DEM (UTM Zone 18N)
        ├── soil_ksat.tif               # Saturated K [m/s] — SSURGO
        ├── soil_Sy.tif                 # Specific yield — SSURGO
        ├── soil_porosity.tif           # Total porosity — SSURGO
        ├── soil_depth_m.tif            # Soil profile depth [m] — SSURGO
        ├── depth_to_bedrock_m.tif      # Depth to bedrock [m] — SoilGrids BDTICM
        ├── regolith_glhymps.shp        # Regolith K and porosity — GLHYMPS
        ├── soil_ssurgo.shp             # SSURGO soil polygons
        └── streams_nhd.shp             # NHDPlus V2 stream network
```

---

## Notebooks
00-02 is for ERTH 4735 Introduction to Groundwater Hydrology
03-06 is for ERTH 4750 Machine Learning and Numerical Modeling for Hydrology

| # | Notebook | Key concepts |
|---|---|---|
| 00 | MODFLOW Installation | Compile mf6 from source with `mfpymake`; save to Google Drive |
| 01 | Steady-State Example | Structured grid, CHD, RCH, flopy basics |
| 02 | Model Calibration | Manual Calibration only |
| 03 | Model Input Data Prep | USGS 3DEP DEM, NHDPlus, SSURGO, GLHYMPS, SoilGrids BDTICM |
| 04 | Baseflow Separation | Eckhardt (2008) recursive filter, BFI, USGS NWIS download |
| 05 | Steady-State MODFLOW | Spatially variable K/Sy, DRN surface drain, Newton solver |
| 06 | Transient MODFLOW | STO, UZF unsaturated zone ET, monthly stress periods, ATS |

---

## Getting Started on Google Colab

### 1. Install MODFLOW 6 (one-time)
Run **`00_MODFLOW_Installation.ipynb`** to compile `mf6` and save it to your Google Drive at `/content/drive/MyDrive/mf6`.

### 2. Clone this repository into Google Drive
Open a Colab notebook, mount your Drive, then clone directly into it:

```python
from google.colab import drive
drive.mount('/content/drive')
```

```bash
!git clone https://github.com/lijingwang/UConn_MODFLOW_Lab \
    /content/drive/MyDrive/MODFLOW_Lab
```

This places all notebooks and small data files at:
```
/content/drive/MyDrive/MODFLOW_Lab/UConn_Forest/
```

Alternatively, download the repo as a ZIP from GitHub (`Code → Download ZIP`), unzip, and upload the folder to your Drive using the [Google Drive web interface](https://drive.google.com).

### 3. Download large data files separately (not in this repo)
Two datasets are too large for GitHub and must be downloaded and placed manually inside `UConn_Forest/`:

| Dataset | Target path (in Drive) | Size | Download |
|---|---|---|---|
| GLHYMPS 2.0 | `UConn_Forest/GLHYMPS/GLHYMPS.shp` | **~6.6 GB** | [Scholars Portal Dataverse](https://doi.org/10.5683/SP2/TTJNIU) |
| SoilGrids BDTICM | `UConn_Forest/SoilGrids/BDTICM_M_250m_ll.tif` | **~7.9 GB** | [BNU GlobalChange](http://globalchange.bnu.edu.cn/research/dtb.jsp) |

> **Tip:** These are global datasets. Download them once to your local machine, then upload to Google Drive if you use Google Colab to run MODFLOW(https://www.google.com/drive/download/). 


### 4. Run notebooks in order
Each notebook begins with a **Google Colab setup cell** that mounts your Drive and sets the working directory automatically. Run that cell first, then proceed sequentially from 00, 01, 02 for ERTH 4735, and 00, 03, 04, 05, 06 for ERTH 4750. 

---

## Dependencies

All notebooks install required packages at runtime. Key packages:

```
flopy          # MODFLOW 6 Python interface
rasterio       # raster I/O
rioxarray      # xarray + rasterio
geopandas      # vector I/O
sfrmaker       # streamflow routing package builder
py3dep         # USGS 3DEP DEM download
pynhd          # NHDPlus stream network download
dataretrieval  # USGS NWIS data download
baseflow       # Eckhardt baseflow filter
```

---

## What Is Excluded from This Repository

The following are **not** tracked by git (see `.gitignore`):

- `GLHYMPS/` and `SoilGrids/` — large raw data files (download separately)
- `ERTH4750_SS_DataDriven/` and `ERTH4750_TR_DataDriven/` — binary model output (`.hds`, `.cbc`, etc.)
- `mf6` executable — compiled separately in notebook 00

---

## References

- Gleeson et al. (2014); Huscroft et al. (2018) — GLHYMPS 2.0 global hydrogeology
- Eckhardt (2008) — recursive digital filter for baseflow separation
- Niswonger et al. (2011) — MODFLOW-NWT unsaturated zone flow package (UZF)
- Langevin et al. (2017) — MODFLOW 6 documentation
- USGS 3DEP — [https://www.usgs.gov/3d-elevation-program](https://www.usgs.gov/3d-elevation-program)
- SSURGO — [https://www.nrcs.usda.gov/resources/data-and-reports/ssurgo](https://www.nrcs.usda.gov/resources/data-and-reports/ssurgo)
