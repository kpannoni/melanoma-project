# melanoma-project
A personal project examining racial disparities in melanoma survival outcomes using SEER cancer registry data.

### Research Question
Are racial disparities in melanoma survival explained by a later stage at diagnosis and socioeconomic factors, or do disparities persist even after controlling for these factors?

### Significance
Melanoma, a malignancy of melanocyte cells, is the 5th most common cancer in the United States and the most aggressive of the three major skin cancers (melanoma, basal cell, squamous cell). According to the [National Cancer Institute](https://seer.cancer.gov/statfacts/html/melan.html), approximately 2.2% of men and women will be diagnosed with melanoma of the skin at some point during their lifetime. In 2025, there were 104,960 new cases diagnosed and 8,430 deaths from melanoma in the United States, and the rate of new cases has increased over time since the NCI began tracking cases in 1992.

While melanoma has excellent prognosis when detected at an early stage (over 99% 5-year survival), survival drops to 35% for distant-stage disease. Early detection is therefore critical for survival.

Despite a higher incidence of cutaneous melanoma in White patients, Black patients experience significantly worse survival outcomes and increased mortality risk compared to White patients ([Dawes et al., 2016]( https://doi.org/10.1016/j.jaad.2016.06.006)). The factors driving these racial disparities are not fully understood. This analysis examines whether disparities can be explained by stage at diagnosis and socioeconomic access, or if significant differences persist after accounting for these factors.


## Dataset

*Source:* SEER Research Data, 17 Registries, Nov 2024 Sub (2000-2022)  
*Dataset:* 234,818 cutaneous melanoma cases across 13 variables

The data was pre-filtered in SEER*Stat to include only:
- Microscopy-confirmed malignant cutaneous melanoma
- Known stage at diagnosis
- First primary tumors only

#### Key variables
- **Demographics:**  Age, Sex, Race
- **Disease outcomes:**  Year of diagnosis, Stage, Survival (months), Vital status, Cause of death
- **Socioeconomic:**  Household income, Rural-Urban continuum

For critial variables, cases with *unknown values* were removed: Race, Survival time, Income, Rural-urban status
<br>**Total:** 8,249 cases were removed (3.5% of the data)

***Note:*** Individual patient-level data cannot be shared publicly per SEER Research Data Agreement. 
<br>Instructions for requesting access and recreating this dataset can be found in the [data README](../data/README.md).

### Final Cohort Summary
<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/cohort_summary_table.png" alt="Cohort Summary Table" width="375"/>

## Analysis Overview

This analysis examines racial disparities in melanoma survival through three stages:

### 1. Data Cleaning  ([01_data_cleaning.ipynb](notebooks/01_data_cleaning.ipynb))
- Standardize variable names
- Remove cases with unknown values for key variables (race, survival time, income, rural-urban status)
- **Final dataset:** 226,587 cases

### 2. Exploratory Analysis  ([02_exploratory_analysis.ipynb](notebooks/02_exploratory_analysis.ipynb))
- Examine distributions of patient demographics, survival outcomes and stage at diagnosis by race
- Create derived variables for survival analysis:

  - Stage categories *(Early vs Advanced)*
  - Age groups *(<50, 50-69, 70+)*
  - Income tiers *(Low, Medium, High)*
  - Metropolitan status *(Metro vs Non-Metro)*
  - Acral melanoma indicator

### 3. Survival Analysis  ([03_survival_analysis.ipynb](notebooks/03_survival_analysis.ipynb))
- **Kaplan-Meier curves:** Compare overall survival by race, then stratify by cancer stage
- **Log-rank tests:** Assess statistical significance of survival differences
- **Cox proportional hazards regression:** Quantify the association between risk factors and melanoma-specific mortality while controlling for multiple variables. Three progressive models test whether racial disparities persist after accounting for clinical factors and socioeconomic access:

  - **Model 1:**  Demographics (age, sex, year of diagnosis)
  - **Model 2:** + Clinical factors (cancer stage, acral melanoma)
  - **Model 3:** + Socioeconomic access (metropolitan residence)
 
# Summary of Results
## Overall Disparities in Survival Outcomes
Black patients have a median survival time that is **39 months less** than White patients (76 months compared to 115 months). Asian / Pacific Islander and Hispanic patients also show significant worse survival times compared to White Patients. Notably, American Indian / Alaska Native patients have a median survival time similar to White patients (111 months).
<br><br>

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/boxplot_survival_time_by_race.png" alt="Survival Time By Race" width="800"/>

Looking at melanoma-specific cause of death, Black patients died of melanoma at nearly **3×** the rate of White patients (32.5% vs 11.7%).
<br>

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/barplot_melanoma_deaths_by_race.png" alt="Melanoma Deaths By Race" width="425"/>

*API = Asian or Pacific Islander; AI/AN = American Indian/Alaska Native*

### Survival Curves by Race with Log-Rank Statistical Tests
To visualize the racial disparities, Kaplan-Meier survival curves were plotted by race.

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/km_curves_by_race.png" alt="K-M Curves By Race" width="600"/>

Log-Rank statistical tests confirm that melanoma survival differs significantly across racial groups (overall p < 0.0001). All minority groups have significantly worse survival probabilty than White patients (p < 0.03 for all comparisons), with Black patients showing the largest disparity (test statistic = 531.59, p < 0.0001).

**View the full statistical test results:**
<br>[logrank_overall_summary.csv](data/logrank_overall_summary.csv)
<br>[logrank_pairwise_summary.csv](data/logrank_pairwise_summary.csv)

*These findings confirm that there is a significant racial disparity in survival for melanoma patients.*

## Disparities in Clinical and Socioeconomic Risk Factors

Black patients are **3.7×** more likely to be diagnosed with distant melanoma (14.3% vs 3.9%), which has a worse prognosis, while White patients are predominantly diagnosed at the earlier localized stage (87.2%).

<br><img src="https://github.com/kpannoni/melanoma-project/blob/main/images/barplot_stage_by_race.png" alt="Cancer Stage By Race" width="450"/>

*API = Asian or Pacific Islander; AI/AN = American Indian/Alaska Native*

Due to sparse data at the distant stage for the smaller minority groups, regional and distant melanoma stages were combined as Advanced stage melanoma, relative to localized melanoma (Early stage). Black patients are diagnosed with Advanced stage melanoma at **3.1×** the rate of White patients (see summary table below).

Overall, minority groups are disproportionately diagnosed at advanced stages of melanoma, which likely accounts for much of the disparity in survival time across racial groups. We will test this more directly later with the series of COX regression models.

### Survival Curves by Race Stratified by Cancer Stage

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/km_curves_strat_by_stage_category.png" alt="K-M Curves Stratified by Cancer Stage" width="750"/>

The K-M survival curves show a similar pattern as we saw before, with Black patients showing the worst survival probabilities at both early and advanced stages, followed by Asian or Pacific Islander patients. Multivariate log-rank tests show a significant difference in survival time across race at both stages.

For *early stage diagnosis*, all minority groups except for American Indian / Alaska Native patients have significantly worse survival outcomes than White Patients. This is consistent with the previous finding that White patients and American Indian / Alaska Native patients have similar median survival times (115 vs 110 months).

At the *advanced stage*, both Black patients and Asian / Pacific Islander patients have significantly worse survival outcomes than White Patients, however there is no significant difference in survival outcomes between Hispanic or American Indian / Alaska Native patients compared to White patient

### Summary Table of Risk Factors by Race:

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/risk_summary_table_by_race.png" alt="Risk Summary Table" width="700"/>

Black patients represent the largest proportion in most of the high risk categories: age over 70, advanced cancer stage, and low median household income. Black patients are also the highest percentage diagnosed with Acral melanoma, which is melanoma on the palms and soles. Acral melanoma is most common in Black, Asian and Hispanic populations, and has a worse prognosis than other subtypes. 

Interestingly, residence in a metopolitan county, a proxy metric for access to healthcare, does not seem to correlate with longer survival times or better outcomes.

## References

Dawes, S. M., Tsai, S., Gittleman, H., Barnholtz-Sloan, J. S., & Bordeaux, J. S. (2016). Racial disparities in melanoma survival. *Journal of the American Academy of Dermatology*, 75(5), 983-991. https://doi.org/10.1016/j.jaad.2016.06.006

National Cancer Institute. (2025). *Cancer Stat Facts: Melanoma of the Skin*. SEER. https://seer.cancer.gov/statfacts/html/melan.html
