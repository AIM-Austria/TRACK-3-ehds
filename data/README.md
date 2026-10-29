# Data

20,000 synthetic patients from three Austrian hospitals. The data runs until 31 December 2025. Use that date as the reference date for ages and for deciding which medications are active. A patient's age on that date is 2025 minus their birth year.

## patients.csv

| Column | Meaning |
|---|---|
| `patient_id` | Pseudonym, used in all files |
| `sex` | `female` or `male` |
| `birth_date` | YYYY-MM-DD |
| `region` | Federal state where the patient lives |
| `data_holder` | Hospital that holds the record: Klinikum Nord, Klinikum Süd or Klinikum West |
| `opted_out` | `true` if the patient opted out of having their data reused (Art. 71). This applies to 839 patients |

## conditions.csv

One row per diagnosis: `patient_id`, `icd10`, `description`, `onset_date`. In requests, a diagnosis means a row in this file. `primary_icd10` in `encounters.csv` is only used when a request names it.

## medications.csv

One row per prescription: `patient_id`, `atc`, `name`, `start_date`, `end_date`. An empty `end_date` means the patient was still taking the drug on 31 December 2025.

## observations.csv

One row per measurement: `patient_id`, `loinc`, `name`, `value`, `unit`, `date`. There is at most one value per patient, test and day. Match on `loinc`. The `name` column is a readable label.

| LOINC | Measurement | Unit in the file |
|---|---|---|
| `4548-4` | HbA1c | `%` |
| `39156-5` | BMI | `kg/m2` |
| `8480-6` | Systolic blood pressure | `mm[Hg]` |
| `62238-1` | eGFR (kidney function) | `mL/min/{1.73_m2}` |

## encounters.csv

One row per visit, from 2023 to 2025: `encounter_id`, `patient_id`, `data_holder`, `type` (`inpatient`, `outpatient` or `emergency`), `start_date`, `end_date`, `primary_icd10`.

`primary_icd10` is the main diagnosis of an inpatient stay. It is empty for other visits. It can be a code that is not in `conditions.csv`, such as a fracture or pneumonia.
