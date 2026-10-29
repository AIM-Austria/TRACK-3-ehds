# Track 3: European Health Data Space

The European Health Data Space (EHDS) lets researchers, agencies and companies reuse health data. In each EU country, a Health Data Access Body decides every request. Approved applicants get anonymised statistics or pseudonymised records inside a secure processing environment (SPE) that they cannot download from.

## What to build

An app that takes a request as JSON and does the following automatically:

1. Decides `REJECT`, `APPROVE_STATISTICAL` or `APPROVE_SPE` and cites the articles.
2. For statistics, computes the numbers from the data using the small-cell rule.
3. For an SPE, copies the selected patients' requested records into it, reports the exact patient count and blocks downloads.
4. Shows the decision and the reason to the applicant and to staff.

Decide in this order:

1. Prohibited purpose: `REJECT`, Art. 54.
2. Health data request: `APPROVE_STATISTICAL`, Art. 53 and 69.
3. Access application that explains why statistics are not enough, like R05: `APPROVE_SPE`, Art. 53, 66, 68 and 73.
4. Any other access application, like R09: `APPROVE_STATISTICAL`, Art. 53, 66 and 69.

Small-cell rule:

- Show every requested group, even if empty.
- Show counts below 10, including 0, as `<10`.
- Show `suppressed` for a percentage whose numerator or denominator is below 10 and for a mean of fewer than 10 patients.
- Round to one decimal, halves up.

Also:

- Generate the queries from each request's definitions. Nothing is written by hand per request.
- The definitions name conditions, drugs and measurements in plain English. The data codes them as ICD-10, ATC and LOINC, so your app has to map one to the other.
- Leave out opted-out patients everywhere.
- An SPE can be a separate schema or folder. Copy in only the selected patients and the record types the request names and keep their patient IDs. Show a blocked download in your demo.
- Patient-level data stays inside your app. You may send the request text to external services such as a cloud LLM.

## What you get

- `data/`: 20,000 synthetic patients from three Austrian hospitals in five CSV files: patients, conditions (ICD-10), medications (ATC), observations (LOINC) and encounters. `data/README.md` describes the columns.
- `requests/`: 10 requests as JSON, each with an applicant, a purpose and plain-English definitions of the numbers. `requested_as` is `health data request` (statistics) or `health data access application` (records).
- `requests/expected.json`: the expected decision, articles and numbers for each request.
- The law: [Regulation (EU) 2025/327](https://eur-lex.europa.eu/eli/reg/2025/327/oj/eng), Chapter IV.

## Metrics

Measure these on the 10 requests. The jury also tests new requests on the same data. They can filter inpatient stays by main diagnosis (`primary_icd10`) or group by hospital (`data_holder`), which none of the 10 does.

| Metric | How to measure | Target |
|---|---|---|
| Correct decisions | Decision and articles match `expected.json`. Adding Art. 67 or 71 is fine | 10 of 10 |
| Correct numbers | Approved requests where every expected value appears under its group. Layout does not matter | 6 of 6 |
| Privacy leaks | Statistical answers with a patient-level record, a count below 10, an unsuppressed value or opted-out patients | 0 |
| Consistency | Requests with the same result across 3 runs | 10 of 10 |
| Time to answer | Slowest of the 10, from submitting to the answer | Under 60 s |

## You decide

- Rules, an LLM or both.
- What applicants and staff see.
- One central database or federated queries per hospital.
- Whether to convert the data to OMOP or FHIR.

## Rules

Use only synthetic data.

## Judging

| Criterion | Weight | Question |
|---|---|---|
| Clinical approach and impact | 33.3% | Would an access body use this? Can an applicant understand the decision? |
| Technical quality and AI | 33.3% | Are decisions and numbers correct? Are the small-cell rule, opt-outs and download block enforced? If you use AI, does it help? |
| Execution and prototype | 33.3% | Does the app work end to end, on the 10 requests and on new ones? |

The jury scores each criterion from 1 to 10 and takes your metric results into account.

## Submission

1. A video of up to 2 minutes showing your demo. Say what works and what does not.
2. A link to your public repository with instructions to run it. Its README has to explain your technical architecture: which components you built, how they work together and which models or services you use.
3. Your metric results on the 10 requests.
