# Dataset notes and analysis decisions

## Source and attribution

The raw research CSV is the public Version 1 Mendeley file for *Early Detection of Dengue Infection Using Machine Learning: A Survey Dataset from Bangladesh*, by Md Niazul Islam Naim and Abir Hasan (DOI 10.17632/t2h5528tc4.1; CC BY 4.0). The source page lists Daffodil International University and describes 1,018 bilingual English–Bengali Google Forms questionnaire responses collected in Bangladesh.

The source authors report that dengue test outcomes were not independently verified against laboratory or clinical records. Test modality and illness day at testing are not specified. Sampling representativeness is not established in the public description. See `Dataset_Source_README.md`, `Dataset_Data_Dictionary.csv`, and `Dataset_Cleaning_Log.csv` for source-provided documentation.

## Raw-data integrity

- File: `data/raw/Dengue_Raw_Research_Data_v1.csv`
- Shape: 1,018 rows × 23 columns
- SHA-256: `2fe2621d35a42223e704b60581496e8a586a00913b16bc29393b46d4394f1c31`
- The raw file is retained byte-for-byte; district standardization is only applied in the working copy.

## Notebook decisions

- Final analytical notebook is v3. Its research questions, preprocessing, calculations, findings, and charts are preserved; only repository-relative input/output paths changed.
- `df_working` is a separate copy and adds `district_original` before trimming district whitespace and applying four explicit spelling/case mappings. The notebook reports 300 affected rows: 296 whitespace changes plus four label mappings; distinct labels go from 28 to 18.
- Two records have `fever_days = 0`; both are preserved. The whole dataset is described as “study participants.”
- Temperature entries are not imputed or changed. The data retain 12 blanks, one nonnumeric `Munshiganj`, one `8`, and two values of `107`. The v3 comparison is restricted to 99–106°F (1,002 records), excluding 16 values for that comparison only.
- There are 630 positive and 388 negative recorded outcome labels (61.89% and 38.11% within this dataset). This is not Bangladesh dengue prevalence.
- The EDA is descriptive. It does not diagnose, claim causality, train a model, or use inferential significance tests.

## Sharing and privacy note

The creator-provided source README reports ethics approval, consent, and permission for public data sharing; approval documents are not in the public package. It also notes that combinations of age, gender, district, and area can be distinctive (198 singleton combinations among 327 in the cleaned data), a possible linkage concern rather than proof of identification. Accordingly, raw and processed row-level CSVs remain local and are excluded from Git; obtain the raw CSV from the cited Mendeley source to reproduce the notebook. The packaged creator README contains a draft-era statement that the DOI is reserved, while the current Mendeley record lists Version 1 as published. Cite the current repository record.


Mendeley lists Version 1 as published on 12 October 2026. The accompanying source README includes a draft-era reserved-DOI note; cite the current published repository record.
