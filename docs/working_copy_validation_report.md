# Working-copy validation report

Validation was rerun from the restructured project using the final v3 notebook.

| Check | Result |
|---|---|
| Notebook code cells executed | 35 of 35; no execution errors |
| Raw source dimensions | 1,018 rows × 23 columns |
| Raw CSV SHA-256 | `2fe2621d35a42223e704b60581496e8a586a00913b16bc29393b46d4394f1c31` |
| Raw integrity during notebook run | Pass; hash unchanged |
| Working copy | 1,018 rows; 24 columns including `district_original` |
| District standardization | 300 records affected; 28 raw labels to 18 standardized labels |
| Missing temperature values | 12 retained |
| Non-numeric temperature value | `Munshiganj` retained |
| Suspicious temperature value | `8` retained |
| High temperature values | Both `107` values retained |
| Temperature imputation | None |
| Zero-fever records | Both retained |
| Notebook validation assertions | All 12 passed |
| Notebook version label | v3 |

The analytical temperature subset uses 99–106°F and has 1,002 records. That filter affects only the temperature comparison; it does not remove records from the raw or working dataset.

The source creator also cautions that age/gender/district/area combinations may be distinctive. The raw and processed CSVs are included in the repository at the user’s direction, with source attribution. The raw file hash above confirms it remains unchanged.

