# Data Sources

This project uses two publicly available datasets from the Centers for Disease Control and Prevention (CDC). The raw datasets were downloaded from CDC public data portals and cleaned using R.

## 1. Primary Data Source

**Dataset name:** Commercial Medical Insurance (MSCANCC) - Vision and Eye Health Surveillance  
**Source:** CDC Vision and Eye Health Surveillance System (VEHSS)  
**Data link:** https://data.cdc.gov/Vision-Eye-Health/Commercial-Medical-Insurance-MSCANCC-Vision-and-Ey/a35h-9yn4/about_data

### Description

This dataset contains claims-based summary measures of vision and eye health indicators among commercially insured individuals. In this project, the dataset was used to examine diagnosed diabetic eye disease prevalence in a commercially insured population.

### Variables Used

Key variables used from this dataset included:

- `year_start`
- `state_abbr`
- `location_desc`
- `question`
- `response`
- `risk_factor`
- `risk_factor_response`
- `age`
- `sex`
- `race_ethnicity`
- `data_value`
- `low_confidence_limit`
- `high_confidence_limit`
- `sample_size`
- `numerator`

### Analytic Use

The primary analytic subset was filtered to:

- Question: `Annual prevalence of diagnosed diabetic eye diseases`
- Response: `All Diabetic eye diseases`
- Geographic level: `State`
- Age: `All ages`
- Sex: `Both sexes`
- Race/ethnicity: `All races`
- Risk factor: `Diabetes`
- Risk-factor response: `Yes`

The outcome variable `data_value` was renamed as `diab_eye_prev` to represent diagnosed diabetic eye disease prevalence.

---

## 2. Supplementary Data Source

**Dataset name:** Nutrition, Physical Activity, and Obesity - Behavioral Risk Factor Surveillance System  
**Source:** CDC  
**Data link:** https://data.cdc.gov/Nutrition-Physical-Activity-and-Obesity/Nutrition-Physical-Activity-and-Obesity-Behavioral/hn4x-zwk7/about_data

### Description

This dataset contains state-level public health indicators related to nutrition, physical activity, obesity, and weight status. In this project, the dataset was used as a supplementary source to provide state-level behavioral risk context.

### Variables Used

Key variables used from this dataset included:

- `year_start`
- `location_abbr`
- `location_desc`
- `question`
- `data_value`
- `stratification_category1`
- `low_confidence_limit`
- `high_confidence_limit`
- `sample_size`

### Indicators Selected

Two behavioral indicators were selected:

- Percent of adults aged 18 years and older who have obesity
- Percent of adults who engage in no leisure-time physical activity

These indicators were renamed as:

- `obesity_pct`
- `inactive_pct`

The dataset was reshaped from long format to wide format so that each state-year record contained both behavioral variables.

---

## Data Integration

The two datasets were joined at the state-year level.

### Join Keys

| MarketScan / VEHSS Variable | BRFSS Variable |
|---|---|
| `state_abbr` | `location_abbr` |
| `year_start` | `year_start` |

A left join was used to preserve all MarketScan state-year records. Records with missing values for diagnosed diabetic eye disease prevalence, obesity prevalence, or physical inactivity prevalence were removed from the final complete-case analytic dataset.

---

## Notes on Raw Data

Raw data files are not included in this repository if file size limits apply. The original datasets can be accessed through the CDC public data links above.

The R Markdown analysis file documents the full workflow for:

1. Loading the raw data  
2. Cleaning variable names  
3. Filtering the analytic sample  
4. Selecting BRFSS behavioral indicators  
5. Reshaping BRFSS data  
6. Joining the two datasets  
7. Creating figures and correlation summaries  

---

## Reproducibility

The main analysis file is:

```text
Project_dataclean.Rmd
