# Melanoma Disparities Project
A personal project examining racial disparities in melanoma survival outcomes using SEER cancer registry data.

### Research Question
Are racial disparities in melanoma survival explained by a later stage at diagnosis and socioeconomic factors, or do disparities persist even after controlling for these factors?

### Significance
Melanoma, a malignancy of melanocyte cells, is the 5th most common cancer in the United States and the most aggressive of the three major skin cancers (melanoma, basal cell, squamous cell). According to the [National Cancer Institute](https://seer.cancer.gov/statfacts/html/melan.html), approximately 2.2% of men and women will be diagnosed with melanoma of the skin at some point during their lifetime. In 2025, there were 104,960 new cases diagnosed and 8,430 deaths from melanoma in the United States, and the rate of new cases has increased over time since the NCI began tracking cases in 1992.

While melanoma has excellent prognosis when detected at an early stage (over 99% 5-year survival), survival drops to 35% for distant-stage melanoma. Early detection is therefore critical for survival.

Despite a higher incidence of cutaneous melanoma in White patients, Black patients experience significantly worse survival outcomes and increased mortality risk compared to White patients ([Dawes et al., 2016]( https://doi.org/10.1016/j.jaad.2016.06.006)). The factors driving these racial disparities are not fully understood. This analysis examines whether disparities can be explained by stage at diagnosis, melanoma subtype, or rural vs urban location, or if significant differences persist after accounting for these factors.


## Dataset

*Source:* SEER Research Data, 17 Registries, Nov 2024 Submission (2000-2022)  
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
<br>Instructions for requesting access and recreating this dataset can be found in the [data README](data/README.md).

### Final Cohort Summary
<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/cohort_summary_table.png" alt="Cohort Summary Table" width="375"/>

## Analysis Overview

This analysis examines racial disparities in melanoma survival through three stages:

### 1. Data Cleaning  ([01_data_cleaning.ipynb](notebooks/01_data_cleaning.ipynb))
- Standardize variable names
- Remove cases with unknown values for key variables (race, survival time, income, rural-urban status)
- **Final dataset:** 226,587 cases

### 2. Exploratory Analysis  ([02_exploratory_analysis.ipynb](notebooks/02_exploratory_analysis.ipynb))
- Examine distributions of patient demographics, survival outcomes, and stage at diagnosis by race
- Create derived variables for survival analysis:

  - Stage categories *(Early vs Advanced)*
  - Age groups *(<50, 50-69, 70+)*
  - Income tiers *(Low, Medium, High)*
  - Metropolitan status *(Metro vs Non-Metro)*
  - Acral melanoma indicator

### 3. Survival Analysis  ([03_survival_analysis.ipynb](notebooks/03_survival_analysis.ipynb))
- **Kaplan-Meier curves:** Compare overall survival by race, then stratify by cancer stage
- **Log-rank tests:** Assess statistical significance of survival differences
- **Cox proportional hazards regression:** Quantify the association between risk factors and melanoma-specific mortality while controlling for multiple variables. Three progressive models test whether racial disparities persist after accounting for clinical and socioeconomic factors:

  - **Model 1:**  Demographics (age, sex, year of diagnosis)
  - **Model 2:** + Clinical factors (cancer stage, acral melanoma)
  - **Model 3:** + Socioeconomic access (metropolitan residence)
 
# Summary of Results

### Key Findings

***Kaplan-Meier Analysis:***
- Significant racial disparities exist in melanoma survival. Black patients experience the worst outcomes, with median survival time 39 months less than White patients and melanoma-specific mortality rates nearly 3× higher.
- Disparities persist even when stratified by cancer stage. At advanced stage, Black and Asian/Pacific Islander patients demonstrate significantly worse survival than White patients.

***Cox Regression Analysis:***
- Controlling for clinical factors (cancer stage at diagnosis and acral melanoma) reduced racial disparities substantially, indicating these factors explain much of the observed differences in survival outcomes.
- Metropolitan residence showed overall protective effects for all groups but did not further explain the disparities, suggesting geographic access to healthcare does not contribute significantly to racial disparities.

## Overall Disparities in Survival Outcomes
Black patients have a median survival time that is **39 months less** than White patients (76 months compared to 115 months). Asian / Pacific Islander and Hispanic patients also show significant worse survival times compared to White Patients. Notably, American Indian / Alaska Native patients have a median survival time similar to White patients (111 months).
<br><br>

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/boxplot_survival_time_by_race.png" alt="Survival Time By Race" width="800"/>

Looking at melanoma-specific cause of death, Black patients died of melanoma at nearly **3×** the rate of White patients (32.5% vs 11.7%).
<br>

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/barplot_melanoma_deaths_by_race.png" alt="Melanoma Deaths By Race" width="400"/>

*API = Asian or Pacific Islander; AI/AN = American Indian/Alaska Native*

### Survival Curves by Race with Log-Rank Statistical Tests
To visualize the racial disparities, Kaplan-Meier survival curves were plotted by race.

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/km_curves_by_race.png" alt="K-M Curves By Race" width="600"/>

Log-Rank statistical tests confirm that melanoma survival differs significantly across racial groups (overall p < 0.0001). All minority groups have significantly worse survival probabilty than White patients (p < 0.03 for all comparisons), with Black patients showing the largest disparity (test statistic = 532.82, p < 0.0001).

**View the full statistical test results:**
<br>[logrank_overall_summary.csv](data/logrank_overall_summary.csv)
<br>[logrank_pairwise_summary.csv](data/logrank_pairwise_summary.csv)

*These findings confirm that there is a significant racial disparity in survival for melanoma patients.*

## Disparities in Clinical and Socioeconomic Risk Factors

### Cancer Stage at Diagnosis by Race

Black patients are **3.7×** more likely to be diagnosed with distant melanoma (14.3% vs 3.9%), which has a worse prognosis, while White patients are predominantly diagnosed at the earlier localized stage (87.2%).

<br><img src="https://github.com/kpannoni/melanoma-project/blob/main/images/barplot_stage_by_race.png" alt="Cancer Stage By Race" width="450"/>

*API = Asian or Pacific Islander; AI/AN = American Indian/Alaska Native*

Due to sparse data at the distant stage for some minority groups, regional and distant stages were combined into a single "Advanced" category. Black patients are diagnosed with advanced stage melanoma at **3.1×** the rate of White patients (see summary table below).

Overall, minority groups are disproportionately diagnosed at advanced stages of melanoma, which likely accounts for much of the observed racial disparities in survival time. This will need to be tested further with a COX regression analysis.

### Survival Curves by Race Stratified by Cancer Stage

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/km_curves_strat_by_stage_category.png" alt="K-M Curves Stratified by Cancer Stage" width="800"/>

The stratified K-M survival curves show a similar pattern as we saw before, with Black patients having the worst survival probabilities at early and advanced stages, followed by Asian / Pacific Islander patients. Multivariate log-rank tests show a significant difference in survival time across race at both stages. However, racial disparities are overall more pronounced at the advanced stage.

**View the full statistical test results:**
<br>[logrank_by_stage_summary.csv](data/logrank_by_stage_summary.csv)
<br>[logrank_Early_stage_pairwise_summary.csv](data/logrank_Early_stage_pairwise_summary.csv)
<br>[logrank_Advanced_stage_pairwise_summary.csv](data/logrank_Advanced_stage_pairwise_summary.csv)

### Summary Table of Risk Factors by Race:

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/risk_summary_table_by_race.png" alt="Risk Summary Table" width="700"/>

Black patients show the highest proportions across multiple risk categories: age 70+ (31%), advanced stage melanoma (40%), and low-income counties (22%). 

Black patients also have the highest rate of acral melanoma (17%), which occurs on the palms and soles. Acral melanoma is most common in Black, Asian, and Hispanic populations and carries a worse prognosis than other melanoma subtypes.

Notably, Black and White patients reside in non-metropolitan counties at similar rates (12%), indicating similar rural vs urban distributions.

## COX Proportional Hazards Regression

A series of Cox regression models examine whether racial disparities persist after controlling for demographics, clinical factors, and socioeconomic access. The relative risk of melanoma death between groups is quantified with a Hazard Ratio (HR).

**Model 1 (Demographics):** Controls for Age, sex, year of diagnosis  
**Model 2 (+ Clinical Factors):** Adds stage at diagnosis, acral melanoma  
**Model 3 (+ Socioeconomic Access):** Adds metropolitan residence

In the plot below, hazard ratios for each minority group are shown relative to White patients (HR = 1.0).

<img src="https://github.com/kpannoni/melanoma-project/blob/main/images/forest_plot_cox_models.png" alt="Forest Plot Combined Models" width="700"/>

**All minority groups show increased risk of melanoma death compared to White patients (HR > 1).** Black patients have **3.5× the hazard** compared to White patients at baseline (**Model 1**). After controlling for clinical factors (**Model 2**), Black patients' hazard decreased from 3.5× to 1.65× compared to White patients, indicating that advanced stage at diagnosis and higher rates of acral melanoma explain much of, but not all, the observed racial disparities.

Controlling for metropolitan residence (**Model 3**) resulted in minimal change in racial hazard ratios, suggesting that rural vs urban location does not explain the remaining disparities. 

***Note:*** Median household income could not be included due to insufficient sample sizes in the low-income tier for some minority groups.

---

**Additional findings:**
- Advanced stage melanoma is the largest risk factor (HR = 11.65 vs early stage)
- Acral melanoma diagnosis increased risk (HR = 1.18)
- Metropolitan residence was protective overall (HR = 0.87) but did not explain racial disparities

Summary table comparing results of the three models: [combined_model_table.png](images/combined_model_table.png)

## Conclusion

This analysis revealed significant racial disparities in melanoma survival, with Black patients experiencing the worst outcomes and being disproportionately represented in multiple risk categories: advanced stage at diagnosis, age 70+, low-income counties, and acral melanoma subtype. These disparities persist even when stratified by cancer stage, indicating that minorities have worse outcomes regardless of disease severity at diagnosis.

The Cox regression analysis showed that clinical factors (stage at diagnosis and acral melanoma) account for a significant portion of the observed racial disparities, reducing Black patients' hazard from 3.5× to 1.65× compared to White patients. However, racial disparities remain even after controlling for demographics, clinical factors, and metropolitan residence as a proxy for geographic access to healthcare. This suggests that there are additional unmeasured factors contributing to survival disparities, such as treatment differences, screening patterns, implicit bias in care delivery, or biological differences. 

## References

Dawes, S. M., Tsai, S., Gittleman, H., Barnholtz-Sloan, J. S., & Bordeaux, J. S. (2016). Racial disparities in melanoma survival. *Journal of the American Academy of Dermatology*, 75(5), 983-991. https://doi.org/10.1016/j.jaad.2016.06.006

National Cancer Institute. (2025). *Cancer Stat Facts: Melanoma of the Skin*. SEER. https://seer.cancer.gov/statfacts/html/melan.html
