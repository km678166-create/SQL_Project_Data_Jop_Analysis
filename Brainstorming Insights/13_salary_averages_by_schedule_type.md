# 📅 SQL Date Filtering: Salary Analysis by Schedule Type

### ❓ Problem Statement
*Calculate average salaries for different job schedule types (e.g., Full-time, Part-time) for recent postings.
The goal is to determine the average yearly and hourly compensation for various employment types, considering only job postings created after June 1, 2023, to ensure the analysis reflects current market conditions.*

### 🛠️ Solution
*I applied a WHERE clause to filter records by a specific date range (> '2023-06-01'). Then, I used GROUP BY to aggregate salary data across different work schedules, calculating distinct averages for both yearly and hourly pay rates.*

```sql
SELECT 
    job_schedule_type,
    AVG(salary_year_avg) AS avg_yearly_salary,
    AVG(salary_hour_avg) AS avg_hourly_salary
FROM 
    job_postings_fact
WHERE 
    job_posted_date > '2023-06-1'
GROUP BY 
    job_schedule_type
ORDER BY
    job_schedule_type
```

### 💡 Key Insights & Analysis
1. **Technique: Temporal Filtering**

- **Relevance:** Analyzing "all-time" data can be misleading due to inflation or market shifts. By filtering for job_posted_date > '2023-06-01', the analysis focuses strictly on recent trends, providing more actionable insights for job seekers today.

- **Date Formats:** SQL is flexible with date strings. Using the standard ISO format (YYYY-MM-DD) ensures compatibility and prevents errors.

2. **Comparative Analysis (Yearly vs. Hourly)**

- **Dual Metrics:** Some jobs (like "Contractor" or "Part-time") often pay hourly, while "Full-time" roles pay yearly. By calculating both averages (AVG(salary_year_avg) and AVG(salary_hour_avg)), we avoid missing data for categories that predominantly use one payment model over the other.

3. **Business Value**

- **Strategic Decisions:** A freelancer might look at this data to decide if "Contract" rates are more lucrative than "Part-time" employment.

- **Compensation Strategy:** HR teams can use this to benchmark their offers against the specific schedule type they are hiring for (e.g., "Are we underpaying our interns compared to the market average?").