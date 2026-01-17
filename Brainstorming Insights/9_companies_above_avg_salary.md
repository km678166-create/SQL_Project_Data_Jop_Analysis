# 💰 SQL Data Analysis: Identifying High-Paying Companies

### ❓ Problem Statement

- Identify companies that offer an average salary exceeding the overall market average.

- The goal is to filter the dataset to find "premium" employers. This involves calculating the average salary for each individual company and comparing it against the global average salary across all job postings.

### 🛠️ Solution
- I employed a Nested Subquery approach.

**Subquery 1 (FROM Clause):** Calculates the average salary per specific company.

**Subquery 2 (WHERE Clause):** Calculates the single global average salary to serve as the benchmark threshold.

```sql
SELECT 
    company_salaries.name
FROM (
SELECT 
    company_dim.company_id,
    company_dim.name,
    AVG(salary_year_avg) AS avg_salary
FROM 
    company_dim
INNER JOIN 
    job_postings_fact ON company_dim.company_id = job_postings_fact.company_id
GROUP BY 
    company_dim.company_id,
    company_dim.name   
) AS company_salaries
WHERE 
    company_salaries.avg_salary > (SELECT AVG(salary_year_avg)
    FROM 
     job_postings_fact)
```

### 💡 Key Insights & Analysis
**1. Technique:** Comparative Filtering
- This query demonstrates a powerful pattern: Dynamic Benchmarking.

- Instead of hard-coding a salary threshold (e.g., WHERE avg_salary > 100000), the query dynamically calculates the market average.

- This ensures the analysis remains accurate even as the underlying data changes or inflation adjusts salary bands.

**2. Structural Complexity (Dual Subqueries)**
- This solution proves mastery of complex SQL structures by combining two types of subqueries:

- **Derived Table (Table Subquery):** The company_salaries block transforms raw job rows into a clean list of company-level statistics.

- **Scalar Subquery:** The (SELECT AVG(...) FROM ...) block returns a single value to be used strictly for comparison in the WHERE clause.

**3. Business Value**
- **Talent Attraction:** Helps job seekers identify top-tier payers in the market.

- **Competitive Analysis:** Allows companies to benchmark their compensation packages against the "Market Average" to determine if they are leading or lagging.