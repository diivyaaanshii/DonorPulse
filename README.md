# DonorPulse — Donor Retention & Intelligence Dashboard

## Overview

DonorPulse is a large-scale donor retention and lifecycle analysis project built using the official DonorsChoose Open Data.

The project analyzes more than **11.3 million donation records** to understand donor behavior, identify high-value and at-risk donor segments, measure retention across donor cohorts, and translate the findings into actionable donor-retention strategies.

The analysis combines **PostgreSQL, SQL, Python, RFM segmentation, cohort analysis, statistical testing, and data visualization** to demonstrate an end-to-end data analytics workflow.

---

## Business Objective

The primary objective is to understand:

- How many donors contribute only once versus repeatedly
- How donation frequency relates to donor value
- Which donor segments are valuable, promising, or at risk
- How donor retention changes across acquisition cohorts
- Whether one-time and repeat donors have significantly different donation-value distributions
- Which donor segments should be prioritized for retention and re-engagement

The goal is to transform transactional donation data into **business-focused retention insights and recommendations**.

---

## Key Findings

### 1. Donor Retention

- **3,466,570** unique donors
- **11,377,479** donation records
- Approximately **$890.2M** in total donation value
- **71.08%** of donors are one-time donors
- **28.92%** are repeat donors

Despite representing only 28.92% of the donor base, repeat donors generate approximately **86.8% of total donation value**.

### 2. Donation Frequency & Donor Value

Average donor value increases substantially as donation frequency increases.

| Donations per Donor | Donors | Average Donated |
|---:|---:|---:|
| 1 | 2,463,949 | $47.70 |
| 2 | 489,979 | $108.79 |
| 3 | 178,477 | $175.70 |
| 4 | 90,600 | $241.70 |
| 5 | 55,093 | $306.30 |
| 6 | 36,071 | $383.22 |

This demonstrates that encouraging donors to make additional contributions can have a significant impact on long-term donor value.

### 3. RFM Segmentation

RFM analysis identified six major donor lifecycle segments:

| Segment | Donors | Share |
|---|---:|---:|
| Potential Loyalists | 851,028 | 24.55% |
| At Risk | 708,333 | 20.43% |
| One-Time / Low Frequency | 540,021 | 15.58% |
| Champions | 481,486 | 13.89% |
| New / Promising | 449,218 | 12.96% |
| Loyal Donors | 436,484 | 12.59% |

The large **Potential Loyalist** and **At Risk** populations indicate significant opportunities for both conversion and re-engagement.

### 4. Statistical Testing

A **Mann–Whitney U test** was used to compare donor-level total donation values between one-time and repeat donors.

The result was statistically significant:

**p < 0.001**

This provides strong evidence that the donation-value distributions of one-time and repeat donors differ.

Statistical significance does not establish causation; the result is interpreted alongside the observed differences in donor frequency and monetary value.

---

## Technology Stack

### Programming & Analysis

- **Python**
- **Pandas**
- **NumPy**
- **SciPy**

### Database & Analytics

- **PostgreSQL**
- **SQL**
- RFM Analysis
- Cohort Analysis
- Statistical Testing

### Visualization

- **Matplotlib**
- **Seaborn**
- **Jupyter Notebook**

### Development & Version Control

- **Git**
- **GitHub**

---

## Dataset Details

The project uses the official **DonorsChoose Open Data** available through the ICPSR data repository.

### Dataset Scale

| Dataset | Records |
|---|---:|
| Donations | 11,377,479 |
| Projects | 2,149,817 |
| Unique Donors | 3,466,570 |
| Total Donation Value | ~$890.2M |

### Dataset Coverage

The dataset contains DonorsChoose project and donation activity covering projects posted from **September 2002 through June 2019**, with activity tracked through **December 2019**.

### Donation Data

Important donation attributes include:

- Donation ID
- Donor ID
- Project ID
- Donation Amount
- Donation Month
- Donor Type
- Payment attributes
- Matching information
- Teacher referral information

### Project Data

Important project attributes include:

- Project ID
- Grade Level
- Subject Category
- Subject Subcategory
- Students Reached
- School State
- Project Posting Month
- Project Status
- Teacher ID

### Data Validation

All **11,377,479 donation records** successfully matched to project records through `project_id`, resulting in a **100% donation-to-project match rate**.

### Data Handling

The original record-level datasets are **not included in this GitHub repository**.

Raw and processed record-level data are retained locally and can be obtained directly from the official DonorsChoose Open Data source.

---

## Data & Analysis Model

The project follows a relational analytical workflow from raw donation and project records to donor segmentation and cohort retention analysis.

### Logical Data Model

```text
                 ┌──────────────────────┐
                 │    donations_raw      │
                 │──────────────────────│
                 │ donation_id          │
                 │ donor_id             │
                 │ project_id           │
                 │ amount               │
                 │ created_month        │
                 │ donor_type           │
                 └──────────┬───────────┘
                            │
                            │ project_id
                            ▼
                 ┌──────────────────────┐
                 │     projects_raw      │
                 │──────────────────────│
                 │ project_id           │
                 │ grade_level          │
                 │ subject_category     │
                 │ subject_subcategory  │
                 │ school_state         │
                 │ students_reached     │
                 │ posted_month         │
                 │ project_status       │
                 └──────────────────────┘


                 donations_raw
                       │
                       ▼
                 ┌──────────────────────┐
                 │      donor_rfm       │
                 │──────────────────────│
                 │ donor_id             │
                 │ recency_months       │
                 │ frequency            │
                 │ monetary             │
                 │ rfm_score            │
                 │ donor_segment        │
                 └──────────────────────┘


                 donations_raw
                       │
                       ▼
              ┌──────────────────────────┐
              │ donor_cohort_retention   │
              │──────────────────────────│
              │ cohort_month             │
              │ months_since_cohort     │
              │ donor_count              │
              │ retention_rate           │
              └──────────────────────────┘