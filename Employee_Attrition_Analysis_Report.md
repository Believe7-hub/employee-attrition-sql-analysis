# Employee Attrition & Performance Analysis
### A SQL & BigQuery Data Analytics Project

**Author:** Gabriel Maclean Believe  
**Tools Used:** SQL · Google BigQuery  
**Dataset:** IBM HR Analytics Employee Attrition Dataset  
**Portfolio:** [believeanalyst.wordpress.com](https://believeanalyst.wordpress.com)  
**LinkedIn:** [linkedin.com/in/gabrielbelieve](https://linkedin.com/in/gabrielbelieve)

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Reason for the Project](#2-reason-for-the-project)
3. [Business Questions Answered](#3-business-questions-answered)
4. [Source of Data](#4-source-of-data)
5. [Methodology](#5-methodology)
6. [SQL Queries & Analysis](#6-sql-queries--analysis)
   - 6.1 [Data Preview](#61-data-preview)
   - 6.2 [Missing Value Check](#62-missing-value-check)
   - 6.3 [Key Attrition Metrics](#63-key-attrition-metrics)
   - 6.4 [Attrition by Department](#64-attrition-by-department)
   - 6.5 [Attrition by Job Satisfaction](#65-attrition-by-job-satisfaction)
   - 6.6 [Attrition by Age Group](#66-attrition-by-age-group)
   - 6.7 [Performance vs. Monthly Income](#67-performance-vs-monthly-income)
   - 6.8 [High Attrition Departments](#68-high-attrition-departments)
7. [Summary of Findings](#7-summary-of-findings)
8. [Recommendations](#8-recommendations)
9. [Limitations](#9-limitations)
10. [Conclusion](#10-conclusion)

---

## 1. Project Overview

This project presents an end-to-end **Employee Attrition and Performance Analysis** conducted using **SQL on Google BigQuery**. The analysis examines a structured HR dataset to uncover patterns and drivers behind employee attrition — the rate at which employees leave an organization — and to explore how performance ratings relate to monthly income.

The project demonstrates practical skills in:
- Data validation and quality assurance
- SQL-based exploratory data analysis (EDA)
- Segmentation and grouping logic
- Business metric calculation (attrition rate, active employees, job satisfaction scores)
- Deriving actionable HR insights from raw data

---

## 2. Reason for the Project

Employee attrition is one of the most costly and disruptive challenges facing organizations today. High turnover rates lead to increased recruitment costs, loss of institutional knowledge, reduced team morale, and productivity gaps.

This project was undertaken to:

- **Quantify** the scale of attrition within the organization
- **Identify** which departments, age groups, and job satisfaction levels are most at risk
- **Understand** whether performance ratings correlate with income — a common driver of dissatisfaction and exit
- **Provide** data-backed recommendations that HR and business leadership can act on to reduce avoidable turnover
- **Demonstrate** real-world application of SQL and BigQuery for HR analytics use cases

---

## 3. Business Questions Answered

| # | Business Question |
|---|---|
| 1 | How many employees are in the organization? |
| 2 | How many employees have left (attrition count)? |
| 3 | What is the overall attrition rate (%)? |
| 4 | How many employees are currently active? |
| 5 | What is the average job satisfaction score across the workforce? |
| 6 | Which departments have the highest attrition rates? |
| 7 | Which departments exceed the company-wide average attrition rate? |
| 8 | How does job satisfaction level influence attrition? |
| 9 | Which age groups experience the highest attrition? |
| 10 | Does performance rating influence monthly income? |

---

## 4. Source of Data

| Attribute | Details |
|---|---|
| **Dataset Name** | IBM HR Analytics Employee Attrition & Performance Dataset |
| **Project ID** | `soy-pillar-463915-b8` |
| **Dataset** | `employee` |
| **Table** | `employe_attrition` |
| **Platform** | Google BigQuery |
| **Total Records** | 1,470 employees |
| **Data Type** | Structured HR data |
| **Origin** | Publicly available IBM HR dataset (commonly used for people analytics projects) |

The dataset contains **35 columns** covering employee demographics, job details, satisfaction scores, performance ratings, income, and attrition status.

---

## 5. Methodology

The analysis followed a structured, step-by-step SQL workflow:

### Step 1 — Data Preview
A `SELECT * LIMIT 1000` query was used to inspect the dataset structure, column names, and data types before analysis began.

### Step 2 — Data Quality Check
A comprehensive `COUNTIF(column IS NULL)` query was run across all 35 columns to identify missing values and confirm data completeness before drawing conclusions.

### Step 3 — Metric Calculation
Core HR KPIs were calculated individually:
- Total employees (`COUNT(*)`)
- Attrition count (`COUNT WHERE Attrition = TRUE`)
- Attrition rate (`SUM(attrition) / COUNT(*) * 100`)
- Active employees (`COUNT WHERE Attrition = FALSE`)
- Average job satisfaction (`AVG(JobSatisfaction)`)

### Step 4 — Segmentation Analysis
Data was segmented across three dimensions:
- **Department** — attrition count and rate per department
- **Job Satisfaction** — attrition count and percentage per satisfaction tier
- **Age Group** — employees grouped into brackets (`<25`, `25–35`, `36–45`, `46–55`, `55+`) with attrition rates calculated per group

### Step 5 — Performance vs. Income Analysis
Average monthly income was calculated per `PerformanceRating` level to explore whether high performers are compensated proportionally.

### Step 6 — Advanced Filtering (HAVING Clause)
A subquery using `HAVING` was applied to isolate departments whose attrition rate **exceeds** the company-wide average — identifying the highest-risk units.

---

## 6. SQL Queries & Analysis

### 6.1 Data Preview

```sql
SELECT *
FROM `soy-pillar-463915-b8.employee.employe_attrition`
LIMIT 1000;
```

**Purpose:** Preview the first 1,000 rows to inspect column structure, data types, and sample values before beginning analysis.

---

### 6.2 Missing Value Check

```sql
SELECT
  COUNTIF(Age IS NULL)                    AS missing_age,
  COUNTIF(Attrition IS NULL)              AS missing_attrition,
  COUNTIF(BusinessTravel IS NULL)         AS missing_business_travel,
  COUNTIF(DailyRate IS NULL)              AS missing_daily_rate,
  COUNTIF(Department IS NULL)             AS missing_department,
  COUNTIF(DistanceFromHome IS NULL)       AS missing_distance_from_home,
  COUNTIF(Education IS NULL)              AS missing_education,
  COUNTIF(EducationField IS NULL)         AS missing_education_field,
  COUNTIF(EmployeeCount IS NULL)          AS missing_employee_count,
  COUNTIF(EmployeeNumber IS NULL)         AS missing_employee_number,
  COUNTIF(EnvironmentSatisfaction IS NULL) AS missing_environment_satisfaction,
  COUNTIF(Gender IS NULL)                 AS missing_gender,
  COUNTIF(HourlyRate IS NULL)             AS missing_hourly_rate,
  COUNTIF(JobInvolvement IS NULL)         AS missing_job_involvement,
  COUNTIF(JobLevel IS NULL)               AS missing_job_level,
  COUNTIF(JobRole IS NULL)                AS missing_job_role,
  COUNTIF(JobSatisfaction IS NULL)        AS missing_job_satisfaction,
  COUNTIF(MaritalStatus IS NULL)          AS missing_marital_status,
  COUNTIF(MonthlyIncome IS NULL)          AS missing_monthly_income,
  COUNTIF(MonthlyRate IS NULL)            AS missing_monthly_rate,
  COUNTIF(NumCompaniesWorked IS NULL)     AS missing_num_companies_worked,
  COUNTIF(Over18 IS NULL)                 AS missing_over18,
  COUNTIF(OverTime IS NULL)               AS missing_over_time,
  COUNTIF(PercentSalaryHike IS NULL)      AS missing_percent_salary_hike,
  COUNTIF(PerformanceRating IS NULL)      AS missing_performance_rating,
  COUNTIF(RelationshipSatisfaction IS NULL) AS missing_relationship_satisfaction,
  COUNTIF(StandardHours IS NULL)          AS missing_standard_hours,
  COUNTIF(StockOptionLevel IS NULL)       AS missing_stock_option_level,
  COUNTIF(TotalWorkingYears IS NULL)      AS missing_total_working_years,
  COUNTIF(TrainingTimesLastYear IS NULL)  AS missing_training_times_last_year,
  COUNTIF(WorkLifeBalance IS NULL)        AS missing_work_life_balance,
  COUNTIF(YearsAtCompany IS NULL)         AS missing_years_at_company,
  COUNTIF(YearsInCurrentRole IS NULL)     AS missing_years_in_current_role,
  COUNTIF(YearsSinceLastPromotion IS NULL) AS missing_years_since_last_promotion,
  COUNTIF(YearsWithCurrManager IS NULL)   AS missing_years_with_curr_manager
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition`;
```

**Result:** All columns returned **0 missing values** — confirming the dataset is clean and complete for analysis.

---

### 6.3 Key Attrition Metrics

```sql
-- Total Employees
SELECT COUNT(*) AS total_employees
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition`;
-- Result: 1,470

-- Attrition Count
SELECT COUNT(t2.EmployeeNumber) AS attrition_count
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition` AS t2
WHERE t2.Attrition = TRUE;
-- Result: 237

-- Attrition Rate (%)
SELECT ROUND(SUM(CASE WHEN Attrition = TRUE THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS attrition_rate
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition`;
-- Result: 16.12%

-- Active Employees
SELECT COUNT(*) AS active_employees
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition`
WHERE Attrition = FALSE;
-- Result: 1,233

-- Average Job Satisfaction
SELECT ROUND(AVG(t0.JobSatisfaction), 2) AS avg_job_satisfaction
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition` AS t0;
-- Result: 2.73 (out of 4)
```

| Metric | Value |
|---|---|
| Total Employees | 1,470 |
| Employees Who Left | 237 |
| **Overall Attrition Rate** | **16.12%** |
| Active Employees | 1,233 |
| Avg. Job Satisfaction | 2.73 / 4.00 |

---

### 6.4 Attrition by Department

```sql
SELECT
  Department,
  COUNT(*) AS total_employees,
  SUM(CASE WHEN Attrition = TRUE THEN 1 ELSE 0 END) AS attrition_count,
  ROUND(SUM(CASE WHEN Attrition = TRUE THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS attrition_rate
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition`
GROUP BY Department
ORDER BY attrition_rate DESC;
```

| Department | Total Employees | Attrition Count | Attrition Rate |
|---|---|---|---|
| Sales | — | — | **20.63%** |
| Human Resources | — | — | **19.05%** |
| Research & Development | — | — | ~13–14% |

---

### 6.5 Attrition by Job Satisfaction

```sql
SELECT
  t0.JobSatisfaction,
  t0.Attrition,
  COUNT(DISTINCT t0.EmployeeNumber) AS count_of_employees
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition` AS t0
GROUP BY t0.JobSatisfaction, t0.Attrition
ORDER BY t0.JobSatisfaction, t0.Attrition;
```

| Job Satisfaction (1=Low, 4=High) | Attrition Status | Employee Count |
|---|---|---|
| 1 | False (Stayed) | 223 |
| 1 | True (Left) | 66 |
| 2 | False | 234 |
| 2 | True | 46 |
| 3 | False | 369 |
| 3 | True | — |

> **Key Insight:** Employees with the **lowest job satisfaction (Level 1)** had the highest attrition count of **66** — significantly more than other satisfaction tiers.

---

### 6.6 Attrition by Age Group

```sql
SELECT
  CASE
    WHEN t.Age < 25 THEN '<25'
    WHEN t.Age BETWEEN 25 AND 35 THEN '25-35'
    WHEN t.Age BETWEEN 36 AND 45 THEN '36-45'
    WHEN t.Age BETWEEN 46 AND 55 THEN '46-55'
    ELSE '55+'
  END AS age_group,
  COUNT(*) AS total,
  SUM(CASE WHEN t.Attrition IS TRUE THEN 1 ELSE 0 END) AS left_company,
  ROUND(SUM(CASE WHEN t.Attrition IS TRUE THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS attrition_rate
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition` AS t
GROUP BY age_group
ORDER BY attrition_rate DESC;
```

| Age Group | Total | Left | Attrition Rate |
|---|---|---|---|
| **< 25** | 97 | 38 | **39.18%** |
| 25 – 35 | 632 | 122 | 19.3% |
| 55+ | 47 | 8 | 17.02% |
| 46 – 55 | 226 | 26 | 11.5% |
| 36 – 45 | 468 | 43 | **9.19%** |

> **Key Insight:** Employees **under 25** have a staggeringly high attrition rate of **39.18%** — more than double the company average. The 36–45 group is the most stable.

---

### 6.7 Performance vs. Monthly Income

```sql
SELECT
  PerformanceRating,
  ROUND(AVG(MonthlyIncome), 2) AS avg_income,
  COUNT(*) AS employee_count
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition`
GROUP BY PerformanceRating
ORDER BY PerformanceRating;
```

| Performance Rating | Avg. Monthly Income | Employee Count |
|---|---|---|
| 3 (Meets Expectations) | $6,537.27 | 1,244 |
| 4 (Exceeds Expectations) | $6,313.89 | 226 |

> **Key Insight:** Employees rated **4 (Exceeds Expectations) earn slightly less** on average than those rated 3 — suggesting top performers may feel **undercompensated**, which could be a hidden attrition driver.

---

### 6.8 High Attrition Departments

```sql
SELECT
  t0.Department,
  ROUND(SUM(CASE WHEN t0.Attrition = TRUE THEN 1 ELSE 0 END) * 100.0 / COUNT(t0.EmployeeNumber), 2) AS attrition_rate
FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition` AS t0
GROUP BY t0.Department
HAVING
  ROUND(SUM(CASE WHEN t0.Attrition = TRUE THEN 1 ELSE 0 END) * 100.0 / COUNT(t0.EmployeeNumber), 2) >
  (SELECT ROUND(SUM(CASE WHEN t1.Attrition = TRUE THEN 1 ELSE 0 END) * 100.0 / COUNT(t1.EmployeeNumber), 2)
   FROM `soy-pillar-463915-b8`.`employee`.`employe_attrition` AS t1)
ORDER BY attrition_rate DESC;
```

| Department | Attrition Rate |
|---|---|
| **Sales** | **20.63%** |
| **Human Resources** | **19.05%** |

> Both Sales and Human Resources exceed the company-wide average of **16.12%** and are flagged as **high-risk departments**.

---

## 7. Summary of Findings

| # | Finding |
|---|---|
| 1 | The organization has a **16.12% overall attrition rate** — 237 out of 1,470 employees have left. |
| 2 | **Sales (20.63%)** and **Human Resources (19.05%)** are the highest-attrition departments, both exceeding the company average. |
| 3 | Employees aged **under 25** face the highest attrition at **39.18%** — nearly 4 in 10 young employees leave. |
| 4 | Employees with the **lowest job satisfaction (Level 1)** have the highest attrition count (66 employees), confirming a strong link between dissatisfaction and exit. |
| 5 | **Top performers (Rating 4) earn less on average ($6,313)** than average performers (Rating 3 at $6,537) — a potential compensation inequity that may drive high-performer exits. |
| 6 | The **36–45 age group** is the most stable workforce segment with only a **9.19% attrition rate**. |
| 7 | The dataset is **100% complete** with no missing values across all 35 columns. |

---

## 8. Recommendations

### 🔴 1. Urgent Focus on Young Employee Retention (Under 25)
With a **39.18% attrition rate**, young employees represent a critical retention risk. The organization should:
- Introduce structured **graduate onboarding and mentorship programmes**
- Provide **clear career progression pathways** with defined milestones
- Conduct **stay interviews** within the first 6 months of employment

### 🔴 2. Address Compensation Inequity for High Performers
Top-rated employees (Performance Rating 4) earn **less on average** than those rated 3. The organization should:
- Conduct an immediate **compensation benchmarking review**
- Ensure **performance-linked pay increases** are consistently applied
- Introduce **non-monetary recognition** (promotions, titles, development opportunities) for high performers

### 🟡 3. Targeted Intervention in Sales and HR Departments
Both departments exceed the company-wide attrition rate. Recommended actions:
- Commission **exit interview analysis** to identify specific pain points in each department
- Review **workload, targets, and manager effectiveness** within Sales and HR
- Introduce **department-specific retention incentives** (bonuses, role rotations, L&D access)

### 🟡 4. Improve Job Satisfaction Across the Workforce
With an average satisfaction score of only **2.73 out of 4**, and Level 1 satisfaction linked to the highest attrition, the organization should:
- Deploy **quarterly pulse surveys** to monitor satisfaction in real time
- Empower line managers to act on team-level satisfaction signals
- Review **work-life balance, environment, and relationship quality** — all measurable dimensions in this dataset

### 🟢 5. Leverage the Stable 36–45 Workforce as Internal Champions
This group's **9.19% attrition rate** suggests strong engagement. The organization should:
- Use this cohort for **mentorship, knowledge transfer, and culture-building initiatives**
- Study what drives their retention and replicate those conditions in higher-risk segments

---

## 9. Limitations

- The dataset is **static and cross-sectional** — it does not capture attrition trends over time, limiting the ability to identify seasonal or cyclical patterns.
- The **IBM HR dataset is synthetic** (created for educational purposes), meaning findings cannot be directly generalized to a real organization without validation against live HR data.
- **Causation cannot be inferred** from this analysis alone — SQL aggregation reveals correlation and patterns, but not root causes. Qualitative research (e.g. exit interviews) would be needed to confirm drivers.
- Some potentially relevant columns (e.g. `OverTime`, `DistanceFromHome`, `StockOptionLevel`) were not included in this phase of analysis — further investigation may yield additional insights.

---

## 10. Conclusion

This project successfully applied **SQL and Google BigQuery** to extract meaningful HR insights from a 1,470-employee dataset. Through structured queries covering data validation, metric calculation, segmentation, and advanced filtering, the analysis identified clear attrition risk profiles across departments, age groups, and satisfaction levels.

The most pressing findings — high young-employee attrition, compensation inequity for top performers, and elevated risk in Sales and HR — provide a concrete, data-backed foundation for HR strategy. Organizations that act on these insights can expect measurable improvements in retention, engagement, and cost efficiency.

---

## Project Files

```
📁 employee-attrition-analysis/
│
├── README.md                    ← This report
├── sql/
│   ├── 01_data_preview.sql
│   ├── 02_missing_value_check.sql
│   ├── 03_key_metrics.sql
│   ├── 04_attrition_by_department.sql
│   ├── 05_attrition_by_job_satisfaction.sql
│   ├── 06_attrition_by_age_group.sql
│   ├── 07_performance_vs_income.sql
│   └── 08_high_attrition_departments.sql
└── assets/
    └── [screenshots of BigQuery results]
```

---

## Connect With Me

- 🌐 Portfolio: [believeanalyst.wordpress.com](https://believeanalyst.wordpress.com)
- 💼 LinkedIn: [linkedin.com/in/gabrielbelieve](https://linkedin.com/in/gabrielbelieve)
- 📧 Email: gabrielbelieve7@gmail.com

---

*© 2025 Gabriel Maclean Believe. This project is for portfolio and educational purposes.*
