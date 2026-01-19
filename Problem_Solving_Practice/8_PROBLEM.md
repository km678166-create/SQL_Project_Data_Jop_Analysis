### 🔄 SQL Data Integration: Quarterly Salary Analysis

# ❓ The Challenge
**Unifying fragmented monthly datasets to perform quarterly high-value job analysis.**

*The raw data was split into separate physical tables for each month (january_jobs, february_jobs, march_jobs). This fragmentation made it impossible to answer a simple business question: "Show me all high-paying jobs (> $70k) posted in Q1 2023."
The challenge was to virtually "stitch" these tables back together into a single dataset on-the-fly without permanently altering the database schema, and then apply a salary filter across the combined result.*

### 🛠️ The Solution
*I used the Set Operator UNION ALL inside a subquery (Derived Table). This allowed me to vertically stack the three monthly tables into a comprehensive "Q1 View," which I then queried as if it were a single table.*

```sql
SELECT
    job_title_short,
    job_location,
    job_via,
    job_posted_date
FROM (
  SELECT
    *
  FROM 
    january_jobs
  UNION ALL
  SELECT
    *
  FROM
    february_jobs
  UNION ALL 
  SELECT
  *
  FROM 
    march_jobs
) AS quarter1_job_postings
WHERE 
salary_year_avg > 70000
ORDER BY 
    salary_year_avg DESC
```

### 💡 Key Insights & Technical Analysis
**1. The Subquery Alias Necessity**

- **Technical Requirement:** When using a subquery in the FROM clause, SQL treats it as a temporary table. This requires an alias (in this case, AS quarter1_job_postings). Without it, the database engine would throw a syntax error because the derived table would be nameless and unreferenceable.

**2. UNION ALL vs. UNION**

- **Performance Optimization:** I chose UNION ALL instead of UNION.

- **UNION** attempts to remove duplicates, which requires an expensive sorting operation.

- **UNION ALL** simply appends the rows. Since we know a job posted in January cannot physically be in the February table, there is no risk of duplicates. This makes UNION ALL significantly faster for large datasets.

**3. Data Type Casting (::DATE)**

- **Formatting:** The query uses ::DATE to strip the timestamp (time of day) from the job_posted_date. This transforms the output from a noisy 2023-01-01 14:30:00 to a clean, report-friendly 2023-01-01.


