# melanoma-project
A personal project examining racial disparities in melanoma survival outcomes using SEER cancer registry data.

### Purpose
This notebook cleans and processes raw SEER data to prepare it for survival analysis.

### Dataset

**Source:** SEER Research Data, 17 Registries, Nov 2024 Sub (2000-2022)  
**Original Size:** 234,836 cutaneous melanoma patients

The data was pre-filtered in SEER*Stat to include only:
- Microscopy-confirmed malignant cutaneous melanoma
- Known stage at diagnosis
- First primary tumors only

**Note:** Individual patient-level data cannot be shared publicly per SEER Research Data Agreement. 
<br>Instructions for requesting access and recreating this dataset can be found in the [data README](../data/README.md).

### Research Question

Are melanoma survival disparities by race explained by later stage at diagnosis and socioeconomic factors, or do disparities persist independent of these factors?

### Analysis Workflow

This is the first notebook in a three-part series:

1. **01_data_cleaning.ipynb** (this notebook) - Data cleaning and filtering
2. **02_exploratory_analysis.ipynb** - Exploratory data analysis and visualization
3. **03_survival_analysis.ipynb** - Kaplan-Meier curves and Cox regression models
