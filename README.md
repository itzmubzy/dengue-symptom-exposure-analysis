# Exploring Dengue-Related Symptoms, Fever Characteristics, and Exposure Factors Among Study Participants in Bangladesh

**Course:** BDS3106 — Programming for Data Science  
**Project stage:** Mid-term exploratory data analysis  
**Final notebook:** [`notebooks/Dengue_Symptom_and_Exposure_Analysis_v3.ipynb`](notebooks/Dengue_Symptom_and_Exposure_Analysis_v3.ipynb)

## Overview

This project explores how reported symptoms, fever characteristics, and exposure factors vary between study participants with recorded dengue-positive and dengue-negative test results. It describes patterns in this dataset; it does not diagnose dengue, establish causation, or estimate dengue prevalence in Bangladesh. Two records have `fever_days = 0`, so the notebook uses “study participants” rather than describing every record as a febrile patient.

The work is intended to help the general public in Bangladesh understand patterns in the available survey records. It does not offer medical recommendations.

## Problem statement

Symptoms, fever characteristics, and exposure reports may differ between participants with positive and negative recorded dengue results. This project explores those differences using descriptive analysis and clear visualizations while accounting for data-quality issues and avoiding diagnostic, causal, or population-prevalence claims.

## Research questions

1. How do fever duration and maximum temperature differ by recorded dengue result?
2. Which early symptoms (nausea, fatigue, appetite loss, abdominal pain, diarrhea, bleeding, and rash) show the largest differences between groups?
3. How do headache, retro-orbital, muscle, and joint pain scores, and vomiting frequency, differ between groups?
4. How are household dengue exposure, stagnant water, area type, and previous dengue infection associated with recorded test result?

## Dataset and attribution

The project uses the original 1,018-row, 23-variable raw CSV, unchanged. The Mendeley record describes 1,018 questionnaire responses collected in Bangladesh through a bilingual English–Bengali Google Forms questionnaire. The accompanying creator README further reports hospital-based collection at Dhaka Medical College Hospital; that detail is creator-reported and cannot be independently verified from the CSV. The file contains 630 recorded positive and 388 recorded negative outcomes. Those counts describe this dataset only.

- **Title:** *Early Detection of Dengue Infection Using Machine Learning: A Survey Dataset from Bangladesh*
- **Authors:** Md Niazul Islam Naim; Abir Hasan
- **Published:** 12 October 2026 (Version 1)
- **Institution listed:** Daffodil International University
- **Source:** [Mendeley Data, Version 1](https://data.mendeley.com/datasets/t2h5528tc4/1)
- **DOI:** [10.17632/t2h5528tc4.1](https://doi.org/10.17632/t2h5528tc4.1)
- **Licence:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

The source documentation says recorded outcomes were not independently verified against laboratory or clinical records, and does not specify test modality or illness day at testing. Dataset source notes and dictionary are in [`docs/`](docs/). The creator reports ethics approval, consent, and permission for public data sharing; signed documents are not in the public package. The creator also notes that combinations of age, gender, district, and area can be distinctive, including 198 singleton combinations among 327 in the cleaned data. This is a possible linkage concern, not proof of identification.

The raw CSV and project working copy are included in this repository under CC BY 4.0 with source attribution. The original raw file is unchanged. The dataset creator reports permission for public data sharing and also notes possible linkage from distinctive age, gender, district, and area combinations; this is a privacy consideration even though the source dataset is publicly available.

## Method and data handling

The notebook audits the raw data, checks missingness and exact duplicates, reviews district labels and temperature anomalies, and verifies the raw file with SHA-256. It creates a separate working copy, standardizes district whitespace/spelling while preserving `district_original`, and validates that the rows and temperature values are retained. No rows are deleted and no temperature is imputed. The 12 blank, one text (`Munshiganj`), one `8°F`, and two `107°F` entries remain in the working copy. For the temperature comparison only, the notebook analyzes values from 99°F to 106°F (1,002 records) and reports that filter explicitly.

The EDA compares group summaries, symptom percentages, pain scores, vomiting frequency, and exposure-category positive proportions with tables and accessible charts. No predictive model or inferential significance tests are used.

## Preliminary findings

Within the specified 99–106°F comparison subset, median maximum recorded temperature was 104°F among positive-labeled records and 102°F among negative-labeled records (N=1,002). Median recorded fever duration was 7 versus 6 days, respectively. Appetite loss and fatigue had the largest positive percentage-point differences in reported early symptoms (69.31 and 58.70 points); abdominal pain and diarrhea were more frequent in the negative-labeled group in this dataset. These are descriptive group differences, not diagnostic evidence or causal effects.

## Limitations

The public source does not provide independent laboratory/clinical verification of outcomes, test type, or test timing. The creator README reports hospital-based collection at Dhaka Medical College Hospital, but the detailed recruitment frame and population representativeness are not established; findings should not be generalized to Bangladesh as a whole. Temperature anomalies affect the temperature comparison, and survey responses can reflect reporting and measurement variation. The source creator identifies distinctive demographic/location combinations as a possible linkage concern. Results are exploratory associations in the available records.

## Run the notebook

The raw CSV is included in `data/raw/`. From the repository root, install the listed dependencies and start Jupyter:

```bash
python -m pip install -r requirements.txt
python -m jupyter notebook notebooks/Dengue_Symptom_and_Exposure_Analysis_v3.ipynb
```

Run cells from top to bottom. The notebook uses repository-relative paths, reads `data/raw/`, and writes the reproducible working copy to `data/processed/`.

## Team contributions

| Member | Suggested focus |
|---|---|
| 24-58809-2 | Coordination, project overview, and presentation integration |
| 24-58816-2 | Dataset provenance and source attribution |
| 24-58216-2 | Data dictionary, audit, and validation documentation |
| 24-57300-2 | Research-question mapping and EDA communication |

These are coordination areas; all four members should understand the complete notebook and findings for the individual viva.




