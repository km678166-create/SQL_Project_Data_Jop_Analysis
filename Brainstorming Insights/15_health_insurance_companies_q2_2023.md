#  Project Title: Health Insurance Job Postings Analysis (Q2 2023)

### ❓1. Problem Statement
**Objective:** Identify and rank companies that posted job openings offering Health Insurance during the Second Quarter of 2023.

**📊 2.Technical Approach**
Data Integration: Performed an INNER JOIN between the company_dim and job_postings_fact tables to bridge job details with company metadata.

**Filtering Logic:**
- Applied a boolean filter job_health_insurance = TRUE to exclude roles without benefits.

- Used EXTRACT(QUARTER FROM ...) to pinpoint the specific timeframe (Q2).

- Aggregation: Grouped data by company name and utilized COUNT() to quantify the opportunities.

- Sorting: Ordered results by volume (DESC) to immediately highlight the top contributors.

### 💻 3. The SQL Solution 

```sql
SELECT
    name AS company_name,
    COUNT(job_id) AS job_postings_count
FROM 
    company_dim
INNER JOIN
    job_postings_fact ON company_dim.company_id = job_postings_fact.company_id
WHERE 
    job_health_insurance = TRUE
    AND EXTRACT(QUARTER FROM job_posted_date) = 2
GROUP BY
    company_name
Having
    COUNT(job_postings_fact.job_id) > 0
ORDER BY
    job_postings_count DESC
```

### 💡4. Business Insights & Analysis
*This query analyzes employer competitiveness regarding employee benefits. By isolating the "Health Insurance" factor, we derive the following insights:*

- **Employer Value Proposition:** Identifies companies that prioritize employee well-being, a key differentiator for attracting top talent.

- **Hiring Volume with Benefits:** Moves beyond raw job counts to focus on "quality" roles (those with benefits), providing a clearer picture of the premium job market in Q2.

- **Seasonal Trends:** Filtering by specific quarters allows stakeholders to compare hiring bursts and benefit offerings across different timeframes.