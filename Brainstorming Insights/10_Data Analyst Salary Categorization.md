# 🏷️ SQL Data Categorization: Data Analyst Salary Buckets

### ❓ Problem Statement

 Categorize salaries for "Data Analyst" roles into specific tiered groups.

The objective is to segment job postings based on their yearly salary into three distinct categories to analyze salary distributions:*

- **High Salary:** $100,000 and above

- **Standard Salary:** Between $60,000 and $100,000

- **Low Salary:** Below $60,000

### 🛠️ Solution
*I utilized a CASE statement to apply conditional logic directly within the SELECT clause, effectively creating a new categorical variable from continuous data. I also filtered specifically for "Data Analyst" roles and removed records with missing salary data to ensure accuracy.*

```sql
SELECT 
    job_id,
    job_title,
    salary_year_avg,
 CASE 
    WHEN salary_year_avg >= 100000 THEN 'high salary'
    WHEN salary_year_avg >= 60000 AND salary_year_avg < 100000 THEN 'Low salary'
    ELSE 'Low salary'
 END AS salary_category
FROM 
    job_postings_fact
WHERE 
    salary_year_avg IS NOT NULL 
    AND job_title_short = 'Data Analyst'
ORDER BY
    salary_year_avg DESC
```

### 💡 Key Insights & Analysis

1. **Concept:** Custom Binning (CASE Statements)
- **Transformation:** The query transforms raw numerical data (salary_year_avg) into meaningful business categories. This technique, often called Data Binning, is essential for dashboards and high-level reporting where stakeholders prefer categories over raw numbers.

- **Flexibility:** Unlike strict mathematical rounding, CASE statements allow for custom, uneven thresholds defined by business logic.

2. **Data Cleaning in Query**

- **Filtering NULLs:** The clause WHERE salary_year_avg IS NOT NULL is critical. In real-world datasets, salary fields often contain null values (e.g., "negotiable"). Including them would distort the analysis or create "uncategorized" rows.

3. **Targeted Analysis**

- **Scope Definition:** By filtering for job_title_short = 'Data Analyst', the analysis remains relevant to a specific audience rather than mixing different job markets (e.g., Executives vs. Interns), which ensures the "High/Standard/Low" definitions make sense for that specific role.