# 🎯 SQL Complex Filtering: Targeted Analyst Search

### ❓ The Challenge
*Identify high-value job opportunities with varying criteria for different roles.
The request was not straightforward: we needed to extract job listings for two specific roles ("Data Analyst" and "Business Analyst"), but each role had a different salary threshold to be considered "high-paying." Additionally, the search had to be restricted to either a specific physical location or remote work.*

- **Data Analyst:** Must pay > $100,000

- **Business Analyst:** Must pay > $70,000

- **Location:** Only 'Boston, MA' or 'Anywhere' (Remote)

### 🧠 The Solution
*To solve this, I used Compound Conditional Logic. I combined a standard IN operator for the location filter with a nested AND/OR structure to handle the specific salary requirements for each job title individually.*

```SQL
SELECT
    job_title_short,
    job_location,
    job_via,
    salary_year_avg
FROM
    job_postings_fact
WHERE
    job_location IN ('Boston, MA', 'Anywhere') AND
    (
        (job_title_short = 'Data Analyst' AND salary_year_avg > 100000) OR
        (job_title_short = 'Business Analyst' AND salary_year_avg > 70000)
    );
```

### 💡 Key Insights & Technical Analysis
1. **Operator Precedence (The Parentheses Matter)**

- **The Problem:** Without the parentheses wrapping the job title/salary logic, SQL might have misinterpreted the OR condition, potentially returning any Business Analyst job regardless of location.

- **The Fix:** By grouping the logic as (Condition A) OR (Condition B), I ensured that the AND job_location... filter applied to both groups strictly.

2. **Strategic Filtering**

- **Custom Benchmarks:** This query demonstrates the ability to apply dynamic thresholds. In real-world analysis, "High Salary" is subjective to the role. A senior Data Scientist's floor is different from a Junior Analyst's. This query respects those nuances in a single pass.

3. **Query Efficiency**

- **Single Pass:** Instead of running two separate queries (one for Data Analysts in Boston, one for Business Analysts in Boston) and merging them, we extract the exact dataset needed in one efficient execution.