# 📊 SQL Statistical Aggregation: Salary Range Analysis

### ❓ The Challenge

*Determine accurate salary benchmarks for various data roles while eliminating statistical noise.
The goal was to analyze the salary spectrum (Minimum, Maximum, and Average) for different job titles.
However, a major data quality issue existed: some job titles had very few postings (e.g., only 1 or 2). Including these "rare" roles would skew the analysis, as a single outlier salary could misrepresent the entire category. The challenge was to filter out these low-volume categories after aggregation to ensure statistical significance.*

### 🛠️ The Solution
*I utilized aggregate functions (AVG, MIN, MAX) for the calculations and implemented the HAVING clause to filter groups based on their size.*

```sql
SELECT 
    job_title_short,
    AVG(salary_year_avg) AS avg_salary,
    MIN(salary_year_avg) AS lowest_avg_salary_offered,
    MAX(salary_year_avg) AS highest_avg_salary_offered
FROM 
    job_postings_fact
GROUP BY
    job_title_short
HAVING
    COUNT(job_id) > 5
```

### 💡 Key Insights & Technical Analysis
1. **Filtering Aggregates (HAVING vs. WHERE)**

- **The Problem:** You cannot use WHERE count(job_id) > 5 because WHERE filters rows before grouping. We needed to filter the groups themselves.

- **The Fix:** Using HAVING allows us to apply conditions to the aggregated results. This ensures that we only analyze job titles that have a "sample size" of more than 5 postings, making the resulting salary averages more reliable.

2. **Range Analysis (Min/Max Spread)**

- **Market Insight:** By calculating both MIN and MAX alongside the AVG, we reveal the salary spread.

- **Example:** If a "Data Analyst" has a narrow spread (Low: $80k, High: $90k), it's a stable, standardized role.

- If it has a wide spread (Low: $40k, High: $200k), it suggests high variance in seniority or industry demand.

3. **Business Value**

- **Compensation Benchmarking:** This query provides the foundational data for HR departments to set competitive salary bands. It answers: "What is the absolute floor and ceiling for this role in the current market?"