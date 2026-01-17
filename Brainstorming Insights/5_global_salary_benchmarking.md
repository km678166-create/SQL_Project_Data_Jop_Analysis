# 📊 Project: Comparative Salary Analysis by Country

**❓ Problem Statement**
- How can we classify job salaries as above or below average based on the overall salary benchmark of the country where each job is located?

*The goal is to generate a list of job postings that includes:
Company name
Job salary
Salary classification relative to the country’s average salary
Results ordered by the posting month*


### 💻 SQL Query
```sql
WITH avg_salaries AS (
SELECT
    job_country,
    AVG(salary_year_avg) AS avg_salary
FROM 
    job_postings_fact
GROUP BY 
    job_country
)

SELECT 
    --Gets basic job info
    job_postings_fact.job_id,
    job_postings_fact.job_title,
    company_dim.name AS company_name,
    job_postings_fact.salary_year_avg AS salary_rate,
    CASE
    WHEN job_postings_fact.salary_year_avg > avg_salaries.avg_salary 
    THEN 'Above Average'
    ELSE 'Below Average'
    END AS salary_category,
    EXTRACT (MONTH FROM job_posted_date) AS posting_month
FROM 
    job_postings_fact
INNER JOIN 
    company_dim ON job_postings_fact.company_id = company_dim.company_id
INNER JOIN 
    avg_salaries ON job_postings_fact.job_country = avg_salaries.job_country
ORDER BY
    -- Sorts it by the most recent job postings
    posting_month DESC
```
### 💡 Key Insights & Solution Approach

**This solution demonstrates advanced data analysis and SQL engineering skills through a clean, business-oriented approach:**

**🔹 Modular Logic with CTEs**
- A Common Table Expression (avg_salaries) is used to calculate the average salary per country in advance.
This improves readability, reusability, and performance compared to embedding complex subqueries directly in the SELECT statement.

**🔹 Conditional Categorization**
-A CASE WHEN statement dynamically classifies each job’s salary as:
Above Average
Below Average
This transforms raw numerical data into clear, decision-ready insights for stakeholders.

**🔹 Data Enrichment**
The fact table (job_postings_fact) is joined with the dimension table (company_dim) using an INNER JOIN to retrieve company names, producing a complete and presentation-ready dataset.

**🔹 Temporal Analysis**
The EXTRACT function is used to isolate the month of job posting, enabling future seasonal or trend-based analysis.