# Job Salary Data Categorization Analysis
- **The goal** is to categorize job postings into two distinct groups based on the availability of salary information.
- **Objective:** Create a unified dataset labeling jobs as either 'With Salary Info' or 'Without Salary Info'.
- **Key Deliverables:** A list containing job_id, job_title, and the custom classification column salary_info.

### SQL Solution
Using UNION ALL to combine two disjoint sets of data (jobs with salary vs. jobs without).

```sql
(Select
    job_id ,
    job_title ,
    'With Salary Info' AS salary_info
FROM 
    job_postings_fact 
WHERE 
    salary_year_avg IS NOT NULL 
    OR salary_hour_avg IS NOT NULL
)
UNION ALL 
(SELECT
    job_id ,
    job_title ,
   'Without Salary Info' AS salary_info 
FROM 
    job_postings_fact 
WHERE 
    salary_year_avg IS NULL 
    AND salary_hour_avg IS NULL
)
ORDER BY 
    salary_info DESC , 
    job_id
```

### 💡 Key Insights & Technical Analysis
- Data Quality Audit (Data Completeness):

*This query serves as a quick Data Quality Check. By categorizing the data, we can easily visualize or count the ratio of postings missing critical salary data versus those that are complete. This is crucial for analysts deciding whether to impute missing values or drop them.*

**Performance Optimization (UNION ALL vs. UNION):**

- I used UNION ALL instead of UNION. Since the two conditions (Has Salary vs. No Salary) are mutually exclusive, there is no possibility of duplicate rows between the two sets.

- UNION ALL is faster because it skips the "duplicate check" step that the standard UNION operator performs.

**Logical Segmentation:**

- While a CASE WHEN statement could achieve a similar result in a single pass, using UNION physically separates the logic. This is particularly useful if the data for "With Salary" and "Without Salary" came from different tables or required drastically different transformations before combining.

**Custom Labeling:**

- Creating a static column ('With Salary Info' AS salary_info) effectively "tags" the rows, transforming raw null/non-null values into business-friendly categories ready for reporting tools (like PowerBI or Tableau)