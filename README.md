UIDAI Datathon — Aadhaar Enrolment & Update Analysis
Project Title

Unlocking Societal Trends in Aadhaar Enrolment and Updates

1. Overview

This project analyses Aadhaar enrolment and update datasets released by UIDAI to identify societal patterns, system usage trends, regional anomalies, and predictive indicators that can support informed administrative and operational decision-making.

The analysis focuses on understanding the transition of the Aadhaar system from enrolment-driven expansion to lifecycle-driven maintenance, using time-series trends, geographic patterns, age segmentation, and normalized ratio-based indicators.

2. Datasets Used

The analysis uses the official UIDAI datasets provided for the datathon:

2.1 Aadhaar Enrolment Data

Key columns:

date

state

district

pincode

age_0_5

age_5_17

age_18_greater

total_enrollments

2.2 Biometric Update Data

Key columns:

date

state

district

pincode

bio_age_5_17

bio_age_17_

total_biometric

2.3 Demographic Update Data

Key columns:

date

state

district

pincode

demo_age_5_17

demo_age_17_

total_demography

3. Repository Structure
.
├── Enrollment_data_cleaning.ipynb
├── Biometric_data_cleaning.ipynb
├── Demography_data_cleaning.ipynb
├── DAX_Codes.txt
├── Heat_Map.mp4
└── README.md

4. Methodology
4.1 Data Cleaning & Preprocessing (Python)

Each dataset is cleaned independently using Jupyter notebooks:

Date parsing and normalization

Creation of year_month for temporal aggregation

Validation of age-bucket totals against overall counts

Removal of invalid or inconsistent records

Aggregation at district–month level for analysis

The cleaned datasets are then exported and loaded into Power BI for analytical modeling and visualization.

4.2 Analytical Modeling (Power BI)

A Dimensional Model is used:

Dim_Date table with month-level granularity

Fact tables for Enrolment, Biometric Updates, and Demographic Updates

One-to-many relationships from Dim_Date to fact tables

To avoid distortion from near-zero denominators, district-level ratio analysis excludes districts with fewer than 10,000 total enrolments.

5. Key DAX Measures

The following DAX measures are used for analysis and visualization.

5.1 Enrolment Measures
Total Enrollments =
SUM ( final_aadhar_enrollment_data[total_enrollments] )

Total Age 0-5 Enrollments =
SUM ( final_aadhar_enrollment_data[age_0_5] )

Total Age 5-17 Enrollments =
SUM ( final_aadhar_enrollment_data[age_5_17] )

5.2 Age-Based Enrolment Shares
Child Enrollment Share =
DIVIDE (
    [Total Age 0-5 Enrollments] + [Total Age 5-17 Enrollments],
    [Total Enrollments]
)

Adult Enrollment Share =
DIVIDE (
    SUM ( final_aadhar_enrollment_data[age_18_greater] ),
    [Total Enrollments]
)

5.3 Update Measures
Total Biometric Updates =
SUM ( final_biometric_data[total_biometric] )

Total Demographic Updates =
SUM ( final_demographic_data[total_demography] )

Total Updates =
[Total Biometric Updates] + [Total Demographic Updates]

5.4 Adult Update Components
Demographic Updates 17+ =
SUM ( final_demographic_data[demo_age_17_] )

Biometric Updates 17+ =
SUM ( final_biometric_data[bio_age_17_] )

5.5 Adult Update Share
Adult Update Share =
DIVIDE (
    [Biometric Updates 17+] + [Demographic Updates 17+],
    [Total Updates]
)

5.6 Trend & Ratio Indicators
MoM Enrolment Change =
[Total Enrollments]
-
CALCULATE (
    [Total Enrollments],
    PARALLELPERIOD ( Dim_Date[month_start], -1, MONTH )
)

Update Intensity =
DIVIDE ( [Total Updates], [Total Enrollments] )

Update Load Ratio (Raw) =
DIVIDE ( [Total Updates], [Total Enrollments] )

6. Visual Analysis & Insights

The analysis yields the following core insights:

Aadhaar system activity is dominated by updates rather than new enrolments, indicating a shift from expansion to maintenance.

Enrolment demand is episodic, driven by time-bound campaigns rather than continuous growth.

Districts operate in distinct system modes, ranging from expansion-heavy to maintenance-heavy usage.

Adult users account for the majority of Aadhaar update activity, reflecting lifecycle-driven system demand.

High demographic update load with low enrolment in certain districts suggests population mobility or demographic churn, rather than new population growth.

7. Predictive Indicators (Non-ML)

No machine learning models were applied due to limited temporal depth. Instead, interpretable predictive indicators were extracted:

Enrolment volatility as a leading indicator of campaign-driven demand

Stable update intensity as a predictor of sustained system load

Persistent district-level update pressure as an indicator of future operational stress

Adult update dominance as a signal of long-term maintenance demand

8. Tools & Technologies

Python (Pandas, NumPy) — data cleaning and preprocessing

Power BI — modeling, DAX calculations, and visualization

Jupyter Notebook — reproducible analysis workflow

9. Notes on Interpretation

Update metrics include both biometric and demographic updates, with biometric refresh forming a core component of adult-driven system maintenance.

Demographic update patterns are treated as indicative of mobility or churn, not as direct measures of migration.

10. Conclusion

This analysis provides a data-driven understanding of how Aadhaar usage patterns evolve over time, across regions, and across age groups. The insights are intended to support anticipatory capacity planning, targeted service delivery, and informed policy decisions, aligning with UIDAI’s operational and governance objectives.