# Student Outreach & Retention Analytics

## Project Overview
An end-to-end analytics platform built on a public student performance dataset (1,000 student records) to identify outreach gaps, track performance distribution, and surface equity insights across socioeconomic and demographic segments. Inspired by my experience as a Marketing Outreach & Operations Project Manager at the Rutgers Learning Center, where data-driven decisions directly impacted student engagement and program participation.

## Tools Used
- **SQL** (CSVFiddle / SQLite) — data querying and segmentation
- **Tableau Public** — interactive dashboard creation
- **Excel** — data cleaning and preparation

## Live Dashboard
[View on Tableau Public] (https://public.tableau.com/views/StudentOutreachRetentionAnalytics/Dashboard)

## Key Findings
- 103 out of 1,000 students flagged as At Risk (avg score below 50)
- Free/reduced lunch male students show the highest proportion of At Risk students — identified as the highest priority outreach segment
- Test prep completers score 10+ points higher on average across all genders
- Group E students consistently outperform Group A by 15+ points regardless of parental education level
- Test prep completion rates are consistently low across ALL parental education levels — suggesting an awareness gap rather than a resource gap, recommending broad outreach over targeted intervention

## Dashboards

### Dashboard 1 — Student Outreach & Retention Analytics
- **Retention Risk Map** — 1,000 students segmented into 4 GPA tiers (High, Mid-High, Mid-Low, At Risk)
- **Outreach Funnel** — Average math score by parental education level and lunch type, identifying highest priority outreach groups
- **Test Prep ROI** — Average math score comparison between students who completed test prep vs those who did not, broken down by gender
![Dashboard 1](dashboard1.png)

### Dashboard 2 — Performance Deep Dive
- **Math vs Reading Score Scatter Plot** — 1,000 individual student dots colored by GPA tier, revealing strong positive correlation across all subjects
- **GPA Tier Distribution** — Pie chart showing proportion of students in each performance tier
- **Performance Heatmap** — Average math score by race/ethnicity group and parental education level, surfacing equity gaps
![Dashboard 2](dashboard2.png)

### Dashboard 3 — Demographics & Equity Analysis
- **Race/Ethnicity Radial Chart** — Student distribution across 5 anonymized race/ethnicity groups
- **Subject Score Comparison** — Average math, reading, and writing scores side by side across all parental education levels
- **Test Prep Completion Rates** — Completed vs not completed test prep by parental education level
- **Gender & Socioeconomic Risk** — GPA tier distribution across gender and lunch type combinations, identifying the most vulnerable student segments
![Dashboard 3](dashboard3.png)

## SQL Queries

### Query 1 — GPA Tier Segmentation
```sql
SELECT *,
  ROUND(("math score" + "reading score" + "writing score") / 3.0, 1) AS avg_score,
  CASE
    WHEN ("math score" + "reading score" + "writing score") / 3.0 >= 80 THEN 'Tier 1 - High'
    WHEN ("math score" + "reading score" + "writing score") / 3.0 >= 65 THEN 'Tier 2 - Mid-High'
    WHEN ("math score" + "reading score" + "writing score") / 3.0 >= 50 THEN 'Tier 3 - Mid-Low'
    ELSE 'Tier 4 - At Risk'
  END AS gpa_tier
FROM StudentsPerformance;
```

### Query 2 — Outreach Gap Analysis
```sql
SELECT
  "parental level of education",
  lunch,
  COUNT(*) AS student_count,
  ROUND(AVG("math score"), 1) AS avg_math,
  ROUND(AVG("reading score"), 1) AS avg_reading,
  ROUND(AVG("writing score"), 1) AS avg_writing,
  ROUND(AVG(("math score" + "reading score" + "writing score") / 3.0), 1) AS avg_overall,
  CASE
    WHEN AVG(("math score" + "reading score" + "writing score") / 3.0) < 60 THEN 'High Risk'
    WHEN AVG(("math score" + "reading score" + "writing score") / 3.0) < 70 THEN 'Medium Risk'
    ELSE 'Low Risk'
  END AS outreach_priority
FROM StudentsPerformance
GROUP BY "parental level of education", lunch
ORDER BY avg_overall ASC;
```

### Query 3 — Test Prep Impact Analysis
```sql
SELECT
  "test preparation course",
  gender,
  "race/ethnicity",
  COUNT(*) AS student_count,
  ROUND(AVG("math score"), 1) AS avg_math,
  ROUND(AVG("reading score"), 1) AS avg_reading,
  ROUND(AVG("writing score"), 1) AS avg_writing,
  ROUND(AVG(("math score" + "reading score" + "writing score") / 3.0), 1) AS avg_overall
FROM StudentsPerformance
GROUP BY "test preparation course", gender, "race/ethnicity"
ORDER BY "test preparation course", avg_overall DESC;
```

## Dataset
[Students Performance in Exams — Kaggle](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams)

## Contact
- LinkedIn: [linkedin.com/in/manyapatel](https://www.linkedin.com/in/manyapatel)
- Email: manyapatel284@gmail.com
