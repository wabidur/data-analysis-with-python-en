# Data Analysis with Python — and Geospatial Data Analysis & Visualization

A complete, eleven-class course that takes an absolute beginner from `print('Hello World')`
to a publication-ready geospatial analysis — trend maps, spatial statistics and
journal-quality figures at 300 dpi.

Every notebook runs end to end on the data in this repository. Nothing has to be
downloaded, and nothing has to be invented.

**Keynote Speaker:** Md Wabidur Rahman, Researcher, Research and Integrated Thought (RIT), Dhaka, Bangladesh

> A Bengali edition of this course is at **[data-analysis-with-python-bn](https://github.com/USERNAME/data-analysis-with-python-bn)**.
> The code is identical; only the explanations differ.

---

## The classes

### Part 1 — Python for data analysis (Classes 01–07)

| # | Notebook | What it covers |
|---|----------|----------------|
| 01 | `Class_01_Introduction_Variables_DataTypes_Operators_001.ipynb` | Variables, data types, arithmetic and assignment operators, casting, f-strings, the three errors beginners meet |
| 02 | `Class_02_Operators_Comparison_Logical_Identity_Membership_Bitwise.ipynb` | Comparison, logical, identity, membership and bitwise operators; why `&` and not `and` in pandas |
| 03 | `Class_03_Lists_Tuples_Sets_Dictionaries.ipynb` | The four containers, `copy()` versus `=`, `zip()`, applied to a real rainfall series |
| 04 | `Class_04_Booleans_Conditionals_Loops.ipynb` | Booleans, `if`/`elif`/`else`, nested conditions, `for`, `while`, `break`, `continue` |
| 05 | `Class_05_Functions_Lambda_Strings_Classes_Modules.ipynb` | Functions, `*args`, defaults, recursion, lambda, string methods, classes, modules |
| 06 | `Class_06_NumPy_and_Pandas.ipynb` | ndarrays, reshaping, vectorised arithmetic, boolean indexing; Series, DataFrame, `loc`/`iloc`, reading Excel |
| 07 | `Class_07_Missing_Data_and_Statistical_Analysis.ipynb` | Sentinel values, `isnull`, `dropna`, `fillna`, `interpolate`, `describe`, `corr`, `resample` |

### Part 2 — Geospatial analysis (Classes 08–11)

These four notebooks form one continuous study, answering a single research question:

> **How did near-surface air temperature and relative humidity change over Bangladesh
> between 2000 and 2024, and what is the spatial structure of that change?**

| # | Notebook | What it covers |
|---|----------|----------------|
| 08 | `Class_08_Foundations_of_Spatial_Data.ipynb` | Vector versus raster, Shapefile/GeoJSON/GeoPackage/GeoParquet benchmarked, shapely geometry, NetCDF and ERA5, a first map |
| 09 | `Class_09_Vector_Data_Manipulation.ipynb` | CRS and the area disaster, centroids, spatial joins, the coarse-grid problem, dissolve/overlay/clip/buffer, geocoding |
| 10 | `Class_10_Raster_and_Remote_Sensing.ipynb` | rioxarray, affine transforms, reprojection, clipping with `all_touched`, zonal statistics, GeoTIFF and COG, **STAC — satellite imagery without downloading**, NDVI |
| 11 | `Class_11_Advanced_Analysis_and_Visualization.ipynb` | Area-weighted trends, vectorised per-pixel OLS, Mann-Kendall and Sen's slope, Moran's I, LISA, Getis-Ord Gi\*, K-means climate zones, networks, folium, **journal-ready Figures 1–3 and Tables 1–3** |

By the end of Class 11 the `outputs/` folder holds three figures at 300 dpi in PNG and
PDF, three CSV tables, a draft Results paragraph and a reproducibility record — the
Results section of a paper.

---

## Getting started

### 1. Create the environment

The geospatial libraries sit on top of GDAL, GEOS and PROJ, which are C/C++ libraries.
Installing them all from **conda-forge** keeps their versions matched; mixing channels,
or installing them with pip on Windows, is the usual cause of a broken setup.

```bash
conda env create -f environment.yml
conda activate geo
```

Or build it by hand:

```bash
conda create -n geo -c conda-forge python=3.12 numpy pandas xarray rioxarray rasterio \
  geopandas pyogrio shapely pyproj netcdf4 h5netcdf matplotlib scipy scikit-learn dask \
  libpysal esda splot mapclassify networkx folium contextily pyarrow geopy \
  jupyterlab notebook ipykernel -y

conda activate geo
python -m pip install pymannkendall pystac-client odc-stac planetary-computer
```

### 2. Start Jupyter **from inside the environment**

```bash
conda activate geo
jupyter lab
```

> **On Windows this matters more than it sounds.** If you launch Jupyter from `base` and
> then switch the kernel to `geo`, the kernel inherits `base`'s `PATH` and can load the
> wrong GDAL/PROJ DLLs — which kills the kernel with no error message. Launch Jupyter from
> the same environment the notebooks will run in.

### 3. Run

Open any notebook and use **Kernel → Restart Kernel and Run All Cells**. The geospatial
notebooks find and unpack the data archives by themselves on first run.

---

## The data

| File | What it is | Source | Licence |
|------|------------|--------|---------|
| `monthly_mean_data_5.3.25.zip` | ERA5 monthly means, temperature and relative humidity at 1000 hPa, 300 months (2000–2024), 0.25° over 20–27°N / 87–94°E | [Copernicus Climate Data Store](https://cds.climate.copernicus.eu) | Copernicus Licence — free reuse with attribution |
| `hourly_mean_data_5.3.25.zip` | ERA5 monthly means by hour of day (the diurnal cycle) | Copernicus CDS | Copernicus Licence |
| `bgd_adm_bbs_20201113_SHP.zip` | Official administrative boundaries of Bangladesh (BBS, 2020): 8 divisions, 64 districts, 544 upazilas | [HDX](https://data.humdata.org) / Bangladesh Bureau of Statistics | see the dataset page on HDX |
| `Data.xlsx`, `Shundorbon.xlsx`, `Khulna.csv` | Daily weather series derived from NASA POWER | [NASA POWER](https://power.larc.nasa.gov) | freely available |
| `sample1.csv`, `barishal_temperature.txt`, `bangladesh_seawater.txt` | Small teaching datasets | prepared for this course | MIT, with the code |
| `Boolean_Algebra.pdf` | Reference handout for Class 02 | prepared for this course | CC BY 4.0 |

### Getting the GADM file

Class 08 compares two boundary datasets — GADM 2015 (7 divisions, 65 districts) against
BBS 2020 (8 divisions, 64 districts) — to show why the source and version of a boundary
file must be stated in a paper's Methods.

**GADM is not included here**, because its licence says *"Redistribution or commercial use
is not allowed without prior permission."* To run that one comparison, download the
Bangladesh shapefile yourself from [gadm.org](https://gadm.org/download_country.html)
and place it next to the notebooks as `BGD_adm.zip`, with layers named
`BGD_adm0.shp` … `BGD_adm3.shp`.

Without it, Class 08 prints a short notice, shows the BBS boundaries alone, and carries on.
Nothing else in the course depends on GADM.

---

## What is in the repository

```
.
├── Class_01 … Class_11 .ipynb     the eleven notebooks
├── *.zip, *.xlsx, *.csv, *.txt    the data, read directly by the notebooks
├── environment.yml                the conda environment
├── requirements.txt               a pip fallback
├── LICENSE
└── outputs/                       created on the first run (git-ignored)
```

Everything sits at the top level on purpose: the notebooks read their data by bare
filename (`pd.read_csv('sample1.csv')`), so a `data/` subfolder would break them.

The notebooks are committed **without stored outputs**, which keeps the diffs readable and
the repository small. Run them once and every figure and table appears.

---

## Licence

The notebooks, the code and the teaching text are released under the **MIT Licence**
(see `LICENSE`). The datasets keep their own licences, listed in the table above —
GADM in particular is not redistributed here.

If this material is useful in your teaching or research, a citation is appreciated:

> Rahman, M. W. (2026). *Data Analysis with Python, and Geospatial Data Analysis &
> Visualization: an eleven-class course.* Research and Integrated Thought (RIT), Dhaka.

---

## Acknowledgements

ERA5 data were generated using Copernicus Climate Change Service information. Neither the
European Commission nor ECMWF is responsible for any use of the Copernicus information
contained here. Administrative boundaries come from the Bangladesh Bureau of Statistics
via the Humanitarian Data Exchange.
