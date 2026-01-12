# Data Sources

## SEER Melanoma Data (2000-2022)

**Source:** Surveillance, Epidemiology, and End Results (SEER) Program  
**Database:** SEER Research Data, 17 Registries, Nov 2024 Submission (2000-2022)  
**Cases:** 234,818 cutaneous melanoma patients

### Citation
Surveillance, Epidemiology, and End Results (SEER) Program (www.seer.cancer.gov) SEER.Stat Database: Incidence - SEER Research Data, 17 Registries, Nov 2024 Sub (2000-2022) - Linked To County Attributes - Time Dependent (1990-2023) Income/Rurality, 1969-2023 Counties, National Cancer Institute, DCCPS, Surveillance Research Program, released April 2025, based on the November 2024 submission.

### Individual Patient-level Data Not Included
**Individual patient-level data cannot be shared publicly per SEER Research Data Agreement.**
<br>Aggregate statistics (not patient-level) are available in summary CSV files.

### How to Obtain Data
1. Request access at [https://seer.cancer.gov/data/access.html](https://seer.cancer.gov/data/access.html)
2. Download SEER 17 Registries, Nov 2024 Sub (2000-2022) and Seer*Stats software
3. Create a Case Listing in Seer*Stats

**Selection Criteria (Filters):**
- Malignant behavior
- Known age
- Melanoma histology codes: 8720-8790 (exclude 8728 meningeal, 8746 mucosal)
- Primary site: C44.0-C44.9 (skin sites only)
- Microscopy confirmed (positive histology, cytology, or immunophenotyping)
- Known stage: Localized, Regional, or Distant (exclude in situ, unknown)
- Sequence number: 00 or 01 (first primary only)

**Variables to Extract:**
See Data Dictionary below for complete list of 13 variables

4. Export as CSV for analysis

5. Clean and filter the data using `notebooks/01_data_cleaning.ipynb`

### Data Dictionary

**Demographics:**
- `age_group` - Age at diagnosis (categorical)
- `sex` - Male/Female
- `race` - Race/ethnicity (NHW, NHB, NHAIAN, NHAPI, Hispanic)
- `marital_status` - Marital status at diagnosis
- `year_diag` - Year of diagnosis (2000-2022)

**Disease Characteristics:**
- `stage` - Summary stage (Localized, Regional, Distant)
- `histology` - ICD-O-3 histology code
- `primary_site` - Anatomical site

**Outcomes:**
- `vital_status` - Alive/Dead at last follow-up
- `cause_death` - SEER cause-specific death classification
- `survival_months` - Months from diagnosis to death/last contact

**Socioeconomic:**
- `median_income` - County-level median household income (2023 inflation-adjusted)
- `rural_urban` - Rural-Urban Continuum Code


