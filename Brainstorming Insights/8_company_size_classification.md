# 🏢 SQL Data Segmentation: Categorizing Companies by Size

### ❓ Problem Statement
- **Classify companies into size categories ('Small', 'Medium', 'Large') based on their job posting volume.**
*The objective is to segment companies by aggregating their total number of job postings and applying conditional logic to assign a size label:*

- **Small:** Less than 10 postings

- **Medium:** 10 to 50 postings

- **Large:** More than 50 postings

### 🛠️ Solution
*I implemented a subquery to first calculate the total job counts per company. This pre-aggregated data is then passed to the outer query, where a CASE statement classifies each company.*

```sql
SELECT 
    company_id,
    name,
 CASE
    WHEN job_count < 10 THEN 'Small' 
    WHEN job_count BETWEEN 10 AND 50 THEN 'Medium'
    ELSE 'Large'
    END AS company_size
FROM (
SELECT 
    company_dim.company_id,
    company_dim.name,
    COUNT(job_postings_fact.job_id) AS job_count
FROM 
    company_dim
INNER JOIN 
    job_postings_fact ON company_dim.company_id = job_postings_fact.company_id
GROUP BY
    company_dim.company_id,
    company_dim.name
) AS company_job_count;
```

### 💡 Key Insights & Analysis
**1. Technique:** Subquery in the FROM Clause
Unlike the previous problem where I used a CTE, here I utilized a Derived Table (Subquery in FROM).

**Aggregation First:** We cannot use the COUNT() aggregate directly inside a CASE statement efficiently without grouping first. The subquery handles the heavy lifting of aggregation, providing a clean "table" (company_job_count) for the main query to read.

**2. Data Transformation: Binning**
- Continuous to Categorical: The query demonstrates a classic data science technique called Binning. We take a continuous variable (job_count) and transform it into categorical groups (Small, Medium, Large).

- Logic: The CASE statement is the SQL equivalent of IF-ELSE logic, allowing for dynamic labeling based on numerical thresholds.

**3. Business Context**
*Classifying companies by "hiring volume" rather than just "revenue" or "employee count" provides unique insights:*

**Hiring Trends:** A "Large" hiring volume indicates a company is in a growth phase or has high turnover.

**Targeting:** Job seekers or B2B services can use this to target specific segments (e.g., "I want to work for a boutique startup" vs. "I want a corporate giant").