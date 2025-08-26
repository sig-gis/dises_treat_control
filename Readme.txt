Project: Proximity / Treatment vs Control villages (Cambodia)

Overview
- This repository contains Jupyter notebooks and supporting code to identify treatment and control villages relative to Community Forests (CFs), Forested Areas (FAs) and Community Protected Areas (CPAs). Key workflows: nearest-village treatment selection, fuzzy matching CF-reported villages to georeferenced villages, and multiple control-selection approaches (buffers, FA-only, etc).

Notebooks (open in VS Code / Jupyter)
- [Controls.ipynb](Controls.ipynb) — end-to-end control-village selection examples and several approaches (A/B/C). Uses `calculate_distances`, `find_nearest` and buffer workflows.
  - referenced symbols: [`calculate_distances`](Controls.ipynb), [`find_nearest`](Controls.ipynb)
- [Proximity to CFs.ipynb](Proximity to CFs.ipynb) — raster filtering, forest-area shapefile creation, and initial nearest-village treatment selection. Implements `filter_raster_by_percentage_and_area` and raster → polygon extraction.
  - referenced symbols: [`filter_raster_by_percentage_and_area`](Proximity to CFs.ipynb), [`calculate_distances`](Proximity to CFs.ipynb), [`find_nearest`](Proximity to CFs.ipynb)
- [Villages Matching.ipynb](Villages Matching.ipynb) — fuzzy matching CF-reported village names to georeferenced village dataset (uses thefuzz / rapidfuzz), builds treatment set and falls back to nearest-village for non-matches.
  - referenced symbols: [`find_closest_phumcode`](Villages Matching.ipynb)
- [Villages Matching-NIS.ipynb](Villages Matching-NIS.ipynb) — variant that joins NIS village table into the matching workflow.

Key scripts and resources
- Data processing helper (MPI postprocessing): [data/STATA DHS/stata_code/code/MPI/postprocess_summarize_mpi.py](data/STATA DHS/stata_code/code/MPI/postprocess_summarize_mpi.py)
- WHO/IGROWUP STATA macros and reference tables: [data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/README.md](data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/README.md) and license [data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/LICENSE](data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/LICENSE)
  - STATA macros of interest: [data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/igrowup_standard.ado](data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/igrowup_standard.ado), [data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/igrowup_restricted.ado](data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/igrowup_restricted.ado)
  - WHO 2007 reference: [data/STATA DHS/stata_code/code/MPI/ado/who2007_stata/who2007.ado](data/STATA DHS/stata_code/code/MPI/ado/who2007_stata/who2007.ado)

Data folders (examples)
- Root data directory used in notebooks: [data/](data/) #https://drive.google.com/open?id=18ttY1vG89yW1TT4UNwBPimaUsl0nKvqt&usp=drive_fs
- Example shapefiles referenced by notebooks:
  - Updated CFs: [Camboda CF updated data - Feb 5, 2025/CF_KH_Updated_2.5.2025_EPSG32648.shp](data/Camboda CF updated data - Feb 5, 2025-20250217T220026Z-001.zip) (zipped in data/; original shapefile inside)
  - Villages: [KHM-Villages/Villages.shp](data/KHM-Villages/Villages.shp)
  - CPA shapefile: [Community_Protected_Areas_Cambodia/CPA_Shape_31_Aug_2022.shp](data/Community_Protected_Areas_Cambodia/CPA_Shape_31_Aug_2022.shp)

How to run (recommended)
1. Open the desired notebook in VS Code (Jupyter) and run cells interactively.
2. Ensure the notebooks' `root_path` variables point to the repository root (many cells use `root_path = 'DISES/Proximity/data'` or relative `data/` paths). Adjust if running from a different working directory.
3. Required Python packages (install via pip or conda):
   - geopandas, pandas, shapely, rasterio, numpy, scipy, matplotlib, contextily, rapidfuzz, thefuzz
4. For heavy spatial ops (buffers, sjoin), use a machine with sufficient memory; consider simplifying geometries before buffering (notebooks show .simplify usage).

Notes & pointers
- Fuzzy matching threshold is configurable (see variable `threshold` in [Villages Matching.ipynb](Villages Matching.ipynb)).
- The notebooks create GeoDataFrames that are exported as shapefiles under outputs paths (check `outputs_path` or hardcoded `to_file` calls).
- STATA macros and license files are kept under [data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/](data/STATA DHS/stata_code/code/MPI/ado/igrowup_stata/) — consult the README and LICENSE if you reuse those macros.
- If you need to find where a function is defined in the repo, search the notebook file; e.g. [`calculate_distances`](Proximity to CFs.ipynb) is defined inline in the notebooks.
