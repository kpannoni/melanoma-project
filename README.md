# melanoma-project
A personal project examining racial disparities in melanoma survival outcomes using SEER cancer registry data.

### Research Question

Are racial disparities in melanoma survival explained by a later stage at diagnosis and socioeconomic factors, or do disparities persist even after controlling for these factors?

### Dataset

*Source:* SEER Research Data, 17 Registries, Nov 2024 Sub (2000-2022)  
*Dataset:* 226,696 cutaneous melanoma cases across 13 variables

The data was pre-filtered in SEER*Stat to include only:
- Microscopy-confirmed malignant cutaneous melanoma
- Known stage at diagnosis
- First primary tumors only

#### Key variables
- *Demographics:*  Age, Sex, Race
- *Disease outcomes:*  Year of diagnosis, Stage, Survial (months), Vital status, Cause of death
- *Socioeconomic:*  Household income, Rural-Urban continuum

**Note:** Individual patient-level data cannot be shared publicly per SEER Research Data Agreement. 
<br>Instructions for requesting access and recreating this dataset can be found in the [data README](../data/README.md).

### Analysis Workflow

1. **01_data_cleaning.ipynb** - Data cleaning and filtering
2. **02_exploratory_analysis.ipynb** - Exploratory data analysis and visualization
3. **03_survival_analysis.ipynb** - Kaplan-Meier curves and Cox regression models
