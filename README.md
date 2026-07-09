# maude-cochlear-implant-analysis
FDA MAUDE adverse event analysis for cochlear implant devices using Python
# Post Market Surveillance Analysis: Cochlear Implant Adverse Events
### FDA MAUDE Database | Python | June 2026

---

## Project Overview

This project analyses real adverse event reports from the FDA MAUDE (Manufacturer and User Facility Device Experience) database for cochlear implant devices. The analysis applies post market surveillance principles and ISO 14971 risk reasoning to identify failure mode patterns, manufacturer reporting distribution, and patient outcome severity.

The dataset covers cochlear implant devices (Product Code PGQ) reported between 2023 and 2026. Python was used for all data loading, cleaning, and visualization using pandas and matplotlib.

---

## Data Source

| Parameter | Detail |
|---|---|
| Source | FDA MAUDE Database (accessdata.fda.gov) |
| Device Category | Cochlear Implants (Product Code PGQ) |
| Date Range | 2023 to 2026 |
| Raw File | 73 rows, 15 columns |
| Cleaned Dataset | 66 rows, 12 columns |
| Tool | Python (pandas, matplotlib) in Google Colab |

---

## Data Quality Issues Found and Fixed

- Rows 66 to 72 contained FDA disclaimer footer text captured as data rows during export. Removed using `iloc[:66]`.
- Two columns were entirely empty and dropped: `Unnamed: 0` and `Exemption Number`.
- Event Date column had 22 unparseable values (30% of records) converted to NaT. These rows are excluded from year based analysis.
- File was exported as binary Excel format despite a `.csv` extension. Loaded using `read_excel()` with `latin-1` encoding.
- Manufacturer column had two name variants for the same company (`COCHLEAR LTD` and `COCHLEAR LIMITED`). Standardised using `.replace()` before analysis.
- Device Problem and Patient Problem columns contained semicolon separated combinations (e.g. `Circuit Failure; Migration`). Split using `.str.split(';')` and `.explode()` to count individual codes independently.
- Patient Problem chart filtered to remove reporting limitation entries to make clinical findings visible.

---

## Key Findings

### Event Type
85% of reports (56 of 66) were classified as injuries, with 15% (10) as malfunctions. However, injury severity is not captured in this dataset. High injury report volume may reflect mandatory reporting compliance rather than device safety concerns. Interpretation requires severity grading and report rate normalization by units sold.

### Manufacturer Distribution
Cochlear Ltd accounts for 60 of 66 records (91%) versus MED-EL at 6 (9%). This difference cannot be interpreted as a safety signal without normalizing by market share and units sold. Differences in reporting culture and vigilance team capacity may also contribute to the observed distribution.

### Device Problem Failure Modes

| Failure Mode | Count | Engineering Note |
|---|---|---|
| Therapeutic or Diagnostic Output Failure | 19 | Most frequent technical failure |
| Appropriate Term/Code Not Available | 16 | Reporting limitation, not a failure mode |
| Output Problem | 10 | Likely overlaps with output failure category |
| Migration | 8 | Electrode movement post implantation, typically surgical or biological origin |
| Insufficient Information | 7 | Reporting limitation |
| Circuit Failure | 6 | Potential for impedance and signal integrity impact |
| Impedance Problem | 5 | Could result from electrode damage or biological response |
| Device Appears to Trigger Rejection | 1 | Low frequency but high severity, potential explantation pathway |

Migration and circuit failure are the most clinically significant failure modes beyond output failures. Migration is generally attributed to surgical technique and biological fixation rather than instrument handling. Impedance and circuit failure could theoretically result from electrode damage during surgical handling, however this risk is considered low when surgical instruments meet Class Ir design requirements under EU MDR.

### Patient Outcomes

| Patient Problem | Count | Clinical Significance |
|---|---|---|
| No Clinical Signs or Conditions | 12 | No observable patient harm at time of report |
| Pain | 6 | Most frequent adverse patient outcome |
| Unspecified Infection | 3 | High severity for permanently implanted device |
| Undesired Nerve Stimulation | 2 | Functional impact on patient experience |
| Failure of Implant | 2 | Potential explantation pathway |
| Tinnitus | 2 | Often pre-existing in cochlear implant recipients |
| Abscess | 1 | High severity, surgical intervention likely required |
| Inflammation | 1 | May indicate early infection or immune response |

Infection related patient problems (unspecified infection, abscess, inflammation) combined with implant failure account for 7 records. For permanently implanted devices, these outcomes carry explantation risk. Severity justifies close monitoring regardless of low absolute report counts, consistent with ISO 14971 risk evaluation principles where severity drives risk classification independent of probability.

---

## Known Limitations

- Year based analysis covers only 44 of 66 records due to unparseable date values in 22 rows.
- MAUDE is a passive surveillance system. Report counts cannot be used to calculate device failure rates or compare safety profiles without normalization by units on market.
- Injury severity is not captured. A count of 56 injury reports does not indicate clinical impact without severity grading.
- Manufacturer report volumes reflect reporting behavior and vigilance team capacity as well as actual device performance.
- Dataset covers a limited time window (2023 to 2026) and a single device category. Findings are not generalizable beyond this scope.

---

## Tools and Libraries

- Python 3
- pandas
- matplotlib
- Google Colab

---

## About This Project

This project was built as a portfolio piece demonstrating the application of Python data analysis skills to a medical device regulatory context. The analytical approach applies ISO 14971 risk reasoning and EU MDR post market surveillance principles throughout, distinguishing reporting limitations from genuine failure signals and applying severity weighted risk assessment to low frequency high consequence outcomes.
