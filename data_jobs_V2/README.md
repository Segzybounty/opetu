# 📊 Data Jobs Analytics Dashboard

**An end-to-end Power BI Business Intelligence solution analyzing global job market trends across data-focused roles.**
---

## 🖼️ Dashboard Preview

![Data Jobs Dashboard — Overview](/images/project2_image1.png)
![Data Jobs Dashboard — Overview](/images/project2_image2.png)
![Data Jobs Dashboard — Overview](/images/pr

*Executive overview: KPIs, skill popularity, and salary comparison by role.*

---

## 🎯 Overview

Job seekers, recruiters, and workforce analysts all face the same problem: **data on the global job market is scattered, inconsistent, and hard to benchmark against.** This project consolidates over 2 million job postings into a single, interactive Power BI dashboard that answers questions like:

- Which data roles are in highest demand right now?
- How do salaries compare across technical vs. business-facing roles?
- Which skills actually show up most in job postings?
- Where is hiring activity concentrated globally?
- Which platforms drive the most recruitment volume?

The result is a decision-ready tool for **recruiters** benchmarking compensation, **job seekers** evaluating career paths, and **analysts** tracking market trends.

---

## 🧩 Problem Statement

The rapid growth of the data industry has created a fragmented job market landscape:

- Job seekers struggle to gauge realistic salary expectations
- Recruiters lack a fast way to benchmark roles competitively
- Analysts need reliable, centralized trend data — not scattered job boards

This dashboard centralizes role demand, salary variation, global hiring distribution, benefits, platform activity, and year-over-year trends into one interactive tool with drill-through navigation from macro to role-specific views.

---

## ⚙️ Tech Stack

| Layer | Tool |
|---|---|
| Visualization & Dashboarding | Power BI Desktop |
| ETL / Data Cleaning | Power Query |
| Calculations & KPIs | DAX |
| Data Modeling | Star Schema (Fact/Dimension) |
| Source Data | Excel / CSV job postings dataset |

### Data Model

- **Fact table:** `job_postings_fact`
- **Dimension tables:** `company_dim`, `date_dim`, `skills_dim`, `skills_job_dim`, `schedule_type`

A star schema was used deliberately over a flat/wide table to reduce redundancy, improve query performance, and keep the model scalable as new job posting data is added.

---

## 📈 Key Insights

### Market Size & Compensation
- **2 million** total job postings analyzed
- **€117K** median yearly salary | **€48** median hourly rate
- ~**4.4** skills requested per job posting on average

### Role Demand
- **Data Analyst** and **Data Engineer** lead in posting volume
- **Data Scientist** roles remain consistently high-demand
- Senior roles (Senior Data Engineer, Senior Data Scientist) post in lower volume but command the highest salary bands

### Salary Stratification
- **Senior Data Scientist** tops the salary range (~€150K median)
- Engineering-heavy roles (ML Engineer, Senior Data Engineer) dominate the upper band
- Business-facing roles (Business Analyst, Data Analyst) offer lower pay but broader accessibility

### Hiring Trend
- Postings trended **downward through 2024**, from ~53K in January to ~14K in November — signaling market cooling and/or seasonal contraction

### Data Analyst Deep Dive (Drill-Through)
| Metric | Value |
|---|---|
| Median Yearly Salary | €93K |
| Median Hourly Salary | €33 |
| No Degree Required | 59.4% |
| Remote (WFH) | 6.6% |
| Health Insurance Offered | 13.9% |
| Full-Time Postings | ~422K |

### Global Distribution & Platforms
- Strongest hiring activity in **North America, Europe, and Asia**
- **LinkedIn** dominates recruitment volume (104K postings), followed by BeBee (54K), Indeed (34K), Trabajo.org (22K), and Recruit.net (14K)

---

## 🧠 What This Project Demonstrates

- **Data modeling:** designing a normalized star schema for analytical performance
- **ETL:** cleaning and shaping raw job posting data with Power Query
- **DAX:** building KPI measures and analytical logic beyond implicit aggregations
- **Dashboard UX:** macro-to-micro navigation via drill-through, slicers, and cross-filtering
- **Analytical storytelling:** translating raw metrics into insights relevant to distinct audiences (recruiters vs. job seekers vs. analysts)

---

## 🚀 How to Explore

1. Clone/download this repository
2. Open `dashboard/data-jobs-dashboard.pbix` in Power BI Desktop
3. Use the **Select Title** and **Select Country** slicers to filter by role or region
4. Click into any role on the salary chart to trigger the **drill-through** view for role-specific insights

---

## 🔮 Future Enhancements

- Add a time-series forecast for job posting volume
- Expand benefits analysis (equity, PTO, bonus structures) where data permits
- Integrate a live/refreshable data source instead of a static CSV snapshot

---

## ✅ Conclusion

The Data Jobs Analytics Dashboard delivers a comprehensive, interactive view of the global data jobs landscape — highlighting strong demand for Data Analysts, Data Engineers, and Data Scientists; clear salary stratification favoring senior and engineering-heavy roles; concentrated hiring activity in major global tech hubs; the dominance of full-time employment and LinkedIn as a recruitment platform; and limited remote-work availability alongside moderate benefit coverage.

This project is an enhancement of my first version of this dashboard, rebuilt with a more rigorous star schema data model, refined DAX measures, and a cleaner drill-through experience between the macro (executive overview) and role-specific views. It reflects a deliberate iteration on scope, data modeling discipline, and analytical storytelling compared to that earlier build — and demonstrates my continued growth in applying Power BI, DAX, ETL, and dashboard design to real-world workforce analytics problems.

---

👤 Author

Olufade Segun Sunday Data / Business Analyst | Power BI · SQL · Python 📍 Espoo, Finland

Feedback and collaboration ideas are welcome — feel free to open an issue or connect.

## 👤 Author

**Olufade Segun Sunday**
Data / Business Analyst | Power BI · SQL · Python
📍 Espoo, Finland

Feedback and collaboration ideas are welcome — feel free to open an issue or connect.