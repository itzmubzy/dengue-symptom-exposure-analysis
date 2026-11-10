# Early Detection of Dengue Infection Using Machine Learning: A Survey Dataset from Bangladesh

Contributors: Md Niazul Islam Naim and Abir Hasan, Department of Computer Science and Engineering, Daffodil International University, Bangladesh.

## Dataset and files

The dataset contains 1,018 questionnaire responses recorded through a bilingual English–Bengali Google Forms questionnaire for dengue research in Bangladesh. The researcher describes hospital-based collection at Dhaka Medical College Hospital and has confirmed the collection as final/frozen. Recorded outcomes comprise 630 dengue-positive and 388 dengue-negative responses.

Both public CSVs contain **1,018 rows and all 23 research columns**. These cover age, gender, district and residence type; dengue history and exposure; fever duration and maximum temperature; headache, retro-orbital, muscle and joint pain on 0–10 scales; vomiting frequency; eight symptoms referring to illness days 1–3; and the recorded final dengue test result. All symptoms and pain ratings are retained. The source export has 26 columns. Submission timestamp, participation-consent response and data-entry role are excluded from both public CSVs; the source export remains unchanged in private study records.

| File | Contents |
|---|---|
| Dengue_Raw_Research_Data_v1.csv | Public raw research extract, 1,018 × 23. Research values, original spelling, missing entries, anomalies, outcome labels and row order are preserved. Research headings are renamed using the dictionary. |
| Dengue_Cleaned_Research_Data_v1.csv | Cleaned and median-imputed research data, 1,018 × 23, with **zero missing cells**. Includes every research field. Of 1,018 temperature values, 1,004 are retained recorded values and 14 are explicitly documented estimates. |
| Data_Dictionary.csv | All 23 original bilingual questions, definitions, units, value labels, validation ranges, raw/cleaned missing counts, imputation counts and interpretation notes. |
| Cleaning_Log.csv | Complete before/after mappings, affected response positions, administrative exclusions, median-imputation provenance and validation/review findings. |
| README.md | Provenance, file descriptions, cleaning and imputation rules, exact reproduction example, quality findings, reuse guidance, permissions and checksums. |

CSV files use UTF-8 with BOM, comma delimiters and standard quoting. Headers are unique. Row order is unchanged. Response positions in the log start at 1 for the first data record; they are not patient identifiers. Quoted dictionary questions can span physical text lines. Only these five files are included in the archive.

## Cleaning and imputation

1. Preserve all responses and research columns. Map original headings to the concise names in Data_Dictionary.csv. Retain dengue_result unchanged, including the bilingual strings.
2. Apply Unicode NFKC normalization and trim surrounding whitespace in working predictors. Standardize gender as male/female, binary responses as yes/no, uncertain responses as unknown, and residence type as urban/semi_urban/rural. Unknown is the recorded Not sure category, not a missing value or no.
3. Standardize district case and whitespace. Apply explicit spelling maps Munsiganj to Munshiganj, Chadpur to Chandpur and Kishorganj to Kishoreganj. These infer intended spellings; they do not verify residence. District standardization changes 300 cells and gives 18 categories.
4. Parse numeric values with broad inclusive screening bounds: age 0–120, fever duration 0–60 days, maximum temperature 90–110°F, pain ratings 0–10 and vomiting 0–100/day. These are screening rules, not clinical reference intervals. Retain valid numeric values and zeros without rounding or clipping.
5. Identify 12 source temperature blanks. Treat temperature entries 8 (response 265) and Munshiganj (response 468) as invalid and temporarily missing. These entries remain unchanged in public raw. No other research field has missing observations.
6. Calculate the median from the remaining **1,004 valid recorded temperatures: 103°F**. Replace all 14 missing/invalid working temperatures with 103°F. This deterministic full-dataset imputation uses no outcome labels. The inserted values are estimates, not measurements recovered from clinical records.
7. Keep the two recorded 107°F temperatures at responses 3 and 626, with review flags. Preserve rare observations and all four pain scales; IQR screening does not trigger automatic deletion. No row deletion, deduplication, scaling, class balancing or model-specific feature selection is performed.

The 14 imputed temperature positions are:

`14 | 166 | 227 | 265 | 266 | 307 | 312 | 402 | 468 | 506 | 540 | 554 | 889 | 942`

These same positions are available in value_change rows whose operation contains median_imputation in Cleaning_Log.csv. Twelve had empty raw values; two had invalid raw entries. Imputation flags are provided through this log without adding another research column. Restoring these 14 positions to missing recovers the cleaned data before imputation exactly.

Compared with the preceding 23-column cleaned file, this update changes exactly 14 temperature cells and no other data cell. Relative to the public raw, the final cleaned file contains 13,548 changed cells, chiefly standardized category labels. Each is represented once in the value-change log. Raw data retain the 12 original blanks and both original invalid values; raw data are not imputed.

## Verified checks

| Check | Result |
|---|---|
| Raw / cleaned dimensions | 1,018 × 23 / 1,018 × 23 |
| Cleaned missing cells | 0 |
| Temperature provenance | 1,004 recorded values + 14 median estimates |
| Recorded outcomes | 630 positive / 388 negative, unchanged |
| Exact duplicate rows | 0 in the original 26-column export, public raw and cleaned; 0 identical full 22-variable predictor profiles |
| Observed numeric values | Every valid original measurement and pain score retained, including zeros |
| Formats and labels | Consistent headings, row widths, Unicode, whitespace and category mappings; no hidden missing tokens or formula prefixes |
| Reconstruction | Public raw plus Cleaning_Log.csv reproduce every cleaned cell and the exact CSV bytes |
| Integrity | ZIP CRC, exact five-file membership and all four CSV hashes verified |

Duplicate checks do not independently establish unique patients. Recorded uncertain responses remain unknown for household/surroundings exposure (7) and stagnant water (98). All 1,018 records are complete in the final cleaned file, but complete fields do not make imputed values observed measurements.

Age units are not specified in the original question; years are assumed for interpretation. Four responses report vomiting without early nausea; different symptoms and question time windows do not establish a contradiction. A record with zero fever days and maximum temperature 99°F is retained. The ever-diagnosed question does not date a diagnosis relative to the current episode, so it is not relabeled using the outcome. These reviews and the retained numerical extremes are documented in the log.

## Reproduce the released cleaned file

Copy the public raw and apply only log entries with entry_type=value_change, checking raw_value before assigning cleaned_value at the listed response positions. The log includes both category cleaning and all 14 imputations. Informational entries do not change cells.

The following Python 3 standard-library code was executed with Python 3.12.14. It reconstructs in memory and compares file bytes without overwriting either CSV. Run from the folder containing the five extracted files:

```python
from pathlib import Path
import csv, io, hashlib

root = Path('.')  # Run in the folder containing the five extracted files.
def read(name):
    with (root / name).open(encoding='utf-8-sig', newline='') as f:
        return list(csv.reader(f, strict=True))
rows = read('Dengue_Raw_Research_Data_v1.csv')
headers = rows[0]
assert len(rows) == 1019 and len(headers) == 23
with (root / 'Cleaning_Log.csv').open(encoding='utf-8-sig', newline='') as f:
    entries = list(csv.DictReader(f))
seen = set()
for entry in entries:
    if entry['entry_type'] != 'value_change':
        continue
    col = headers.index(entry['variable'])
    positions = [int(p) for p in entry['response_positions'].split('|')]
    assert len(positions) == int(entry['affected_rows']) == len(set(positions))
    for pos in positions:
        assert 1 <= pos < len(rows) and (pos, col) not in seen
        assert rows[pos][col] == entry['raw_value']
        rows[pos][col] = entry['cleaned_value']
        seen.add((pos, col))
assert len(seen) == 13548
assert rows == read('Dengue_Cleaned_Research_Data_v1.csv')
buffer = io.StringIO(newline='')
csv.writer(buffer, lineterminator='\n').writerows(rows)
rebuilt = buffer.getvalue().encode('utf-8-sig')
assert rebuilt == (root / 'Dengue_Cleaned_Research_Data_v1.csv').read_bytes()
print('PASS: all cleaned cells and file bytes reproduced.')
print('SHA-256:', hashlib.sha256(rebuilt).hexdigest())
```

## Use in machine-learning evaluation

This released cleaned CSV uses a median calculated from the full dataset. For train/test or cross-validation performance estimation, **restore the 14 logged temperature positions to missing before splitting**, then fit the imputer and encoders only within each training fold. The recorded values in public raw and the imputation positions in the log make this possible. Do not carry a full-dataset imputation statistic into held-out evaluation. A missingness indicator may be included in the fitted preprocessing pipeline; its suitability depends on the intended use.

dengue_result is the outcome and must not be used as a predictor. Full-illness duration, maxima and averages may not be available at an early prediction time. Keep all research fields for reuse, but assess predictor timing separately for each analysis. Retrospective early-symptom responses do not establish prospective collection.

The recorded test outcomes have not been independently verified against laboratory or clinical records. Test modality and illness day of testing are not specified in the supplied data. This data release makes no model-performance or clinical-validation claim.

## Ethics approval, consent and privacy

The research project, "Early Detection of Dengue Infection Using Machine Learning", received ethics approval from the Research Ethics Committee (REC), Faculty of Science and Information Technology (FSIT), Daffodil International University, Bangladesh.

| Approval detail | Information |
|---|---|
| Approving body | Research Ethics Committee (Institutional Review Board), Daffodil International University |
| Reference number | REC-FSIT-2026/2023 |
| Approval date | 22 July 2026 |
| Study described in the letter | Observational, non-interventional research using questionnaire-based and clinical data |

The approval letter specifies voluntary participation, informed consent, anonymization and use for academic research. Hospital permission, departmental/supervisory permission and doctor questionnaire-review documents have also been provided by the researcher.

The depositing researcher confirms permission for public data sharing and consent covering respondents under 18. These are researcher-confirmed statements; individual consent records are not included in this public package. Signed approval/permission documents remain in the private study records and may be supplied to editors or moderators through an appropriate confidential channel.

Submission timestamps, participation-consent responses and data-entry roles are excluded from both public CSVs. No names, contact details, patient identifiers, dates of birth or exact addresses were found in the research schema or email/mobile pattern scan. Age, gender, district and area can still form distinctive combinations: the cleaned data contain 327 such combinations, of which 198 occur once. This is a possible linkage consideration, not proof of identification or a guarantee of anonymity. Reusers should respect participant privacy.

## License and citation

The depositing researcher selected CC BY 4.0. Reusers should credit the dataset authors, cite the actual repository record and version, link the license and identify modifications. Signed approval documents are not included in the public archive.

The established archive name is Dengue_Survey_Data_Bangladesh_v1.zip. Its v1 suffix is a filename convention; the published repository record determines the version and citation. If a previous version is published, submit changes as a new version of the existing record. The current draft reserves DOI 10.17632/t2h5528tc4.1. The supplied preview labels it reserved and not active; use the final repository citation after publication.

## File integrity

SHA-256 hashes of the four CSV files:

- Cleaning_Log.csv: `f079b706f76679b3e7c3dfe9d312d864dc65e1096262b48e5865e5844982b822`
- Data_Dictionary.csv: `91a10f3a6e210d38e339e504a3e19a9006388bc28b1bbca35d5a1ba9b13f851d`
- Dengue_Cleaned_Research_Data_v1.csv: `f9c180cfb4c268861edede9a974c0266c58b2634909e6742f7aa321a7a862659`
- Dengue_Raw_Research_Data_v1.csv: `2fe2621d35a42223e704b60581496e8a586a00913b16bc29393b46d4394f1c31`

README does not hash itself. Automated checks establish file consistency and traceable preparation; they do not independently confirm clinical measurements or repository acceptance.

## Guidance consulted

- [UK Data Service: prepare data for deposit](https://ukdataservice.ac.uk/help/deposit-data/prepare-your-data-for-deposit/): documented variable definitions, missing-value codes and data preparation.
- [UK Data Service: QAMyData](https://ukdataservice.ac.uk/about/research-and-development/past-projects/qamydata/): examples of quality checks for missingness, duplication, outliers and identifiers. The present checks use purpose-written code; QAMyData itself was not run.
- [scikit-learn: common pitfalls](https://scikit-learn.org/stable/common_pitfalls.html): learn imputation and other fitted preprocessing from training folds to prevent leakage.
- [Mendeley Data: requirements](https://www.elsevier.support/mendeley/answer/what-are-the-requirements-for-datasets-) and [file formats](https://data.mendeley.com/file-formats): CSV and ZIP uploads are supported; archive members need individual descriptions and ZIP previews are not supported. Moderation acceptance is a repository decision. These computational checks do not substitute for the researcher's verification of the collected observations.
- [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/): license terms.
- District naming: [Munshiganj](https://www.munshiganj.gov.bd/en), [Chandpur](https://www.chandpur.gov.bd/en), [Kishoreganj](https://www.kishoreganj.gov.bd/en/site/page/ZObT-%E0%A6%9C%E0%A7%87%E0%A6%B2%E0%A6%BE%E0%A6%B0-%E0%A6%AA%E0%A6%9F%E0%A6%AD%E0%A7%82%E0%A6%AE%E0%A6%BF).
