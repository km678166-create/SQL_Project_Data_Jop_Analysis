# 🗂️ SQL Data Transformation: Experience Level & Remote Analysis

### ❓ Problem Statement
*Classify job postings by experience level and remote flexibility.
The objective is to structure raw job titles into standard seniority categories ('Junior', 'Senior', 'Manager') using keyword pattern matching, and to convert technical boolean flags into user-friendly "Yes/No" labels for remote work availability.*

### 🛠️ Solution
*I utilized CASE statements with Pattern Matching (ILIKE) to scan job titles for specific keywords. This allows for dynamic categorization of unstructured text data. Additionally, I standardized the remote work flag for better readability.*

```sql
SELECT 
    job_id,
    salary_year_avg,

  CASE 
    WHEN job_title ILIKE '%Senior%' THEN 'Senior' 
    WHEN job_title ILIKE '%Manager%' OR job_title ILIKE '%Lead%' THEN 'Lead/Manager'
    WHEN job_title ILIKE '%Junior%' OR job_title ILIKE '%Entry%' THEN 'Junior/Entry'
    ELSE 'Not Specified'
  END AS experience_level,

  CASE 
    WHEN job_work_from_home = TRUE THEN 'Yes'
    ELSE 'No'
  END AS remote_option
FROM 
    job_postings_fact
WHERE
    salary_year_avg IS NOT NULL 
ORDER BY 
    job_id
```

### 💡 Key Insights & Analysis
1. **Technique: Pattern Matching (ILIKE)**

- **Text Classification:** Real-world text data is often messy. By using ILIKE (Case-Insensitive search), the query captures variations like "senior", "Senior", or "SENIOR" without needing complex regular expressions.

- **Hierarchy Logic:** The order of the WHEN clauses matters. For example, if a title is "Senior Manager", this query prioritizes the 'Senior' label (since it appears first). Note: In a production environment, we might adjust this priority depending on business rules.

2. **Data Standardization**

- **Boolean to String Conversion:** Database columns often store data efficiently as TRUE/FALSE (0/1). However, for reporting (Excel/Tableau/PowerBI), stakeholders prefer "Yes/No". This query handles that transformation at the source.

- **Handling Unmatched Data:** The ELSE 'Not Specified' clause is crucial for data integrity. It ensures that rows that don't fit our keywords aren't dropped but are instead flagged for further review.

3. **Business Value**

- **Salary Benchmarking:** By creating these new buckets (experience_level), we can later calculate the average salary per level (e.g., "How much more does a Manager earn compared to a Junior?").

- **Workforce Planning:** Helps identify if the market is currently seeking more entry-level talent or experienced leaders.
