# 📊 SQL Project: Company Skills & Salary Analysis
### 📝 Problem Statement
The goal of this query is to analyze the recruitment landscape for various companies by focusing on two key metrics:

**Skill Demand:** Calculate the count of unique skills required across all job postings for each company.

**Compensation:** Identify the highest average annual salary offered, specifically for roles that require technical skills (excluding general roles with no listed skills).

### 💡 Solution Approach
To ensure the solution is modular and efficient, the query uses Common Table Expressions (CTEs):

**First CTE (required_skills):**

- Calculates the count of distinct skills (COUNT DISTINCT) per company.

- Utilizes a LEFT JOIN to ensure every company from the database is included in the final result, even if they currently have no job postings (returning a count of 0 instead of dropping the record).

**Second CTE (max_salary):**

- Derives the maximum average yearly salary for each company.

- Includes a crucial filter (WHERE job_id IN ...) to ensure salary logic is applied only to jobs that have at least one associated skill, providing a more accurate benchmark for skilled labor.

**Main Query:**

- Joins the primary company_dim table with the pre-calculated metrics from the CTEs to output the final report, ordered alphabetically by company name.

### 💻 SQL Query

```sql
WITH required_skills AS (
SELECT 
    company_dim.company_id,
    COUNT(DISTINCT skills_job_dim.skill_id) AS 
    unique_skills_required
FROM 
    company_dim
LEFT JOIN 
    job_postings_fact ON job_postings_fact.company_id = company_dim.company_id
LEFT JOIN 
    skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
GROUP BY 
    company_dim.company_id
) ,

max_salary AS (
SELECT 
    job_postings_fact.company_id,
    MAX(job_postings_fact.salary_year_avg) AS highest_average_salary
FROM 
    job_postings_fact
WHERE 
    job_postings_fact.job_id IN (SELECT job_id FROM skills_job_dim)
GROUP BY
    job_postings_fact.company_id
)

SELECT
    company_dim.name,
    required_skills.unique_skills_required ,
    max_salary.highest_average_salary
FROM 
    company_dim
LEFT JOIN 
    required_skills ON company_dim.company_id = required_skills.company_id
LEFT JOIN 
    max_salary ON company_dim.company_id = max_salary.company_id
ORDER BY 
    company_dim.name
```
### 🚀 Key Insights
**Data Integrity:** Using LEFT JOINS is the optimal choice to preserve the master list of companies. It handles missing data gracefully by returning NULL or 0 rather than excluding smaller or inactive companies.

**Modular Code:** Breaking down complex aggregations into separate CTEs significantly improves code readability and maintainability compared to using complex nested subqueries.

**Conditional Precision:** The solution demonstrates how to apply specific constraints to one metric (calculating salary only for skilled jobs) while keeping another metric broad (counting skills across the board), ensuring data accuracy.