# 🔍 SQL Pattern Matching: Filtering for Entry-Level Analyst Roles

### ❓ The Challenge
*Accurately filtering job titles to exclude Senior-level roles from unstructured text data.
The dataset contains raw job titles (e.g., "Sr. Data Analyst", "Data Analyst II", "Business Analyst Lead") without a dedicated "seniority_column".
The challenge was to write a query that specifically targets "Data Analyst" or "Business Analyst" roles while strictly excluding any position explicitly labeled as "Senior". A simple exact match would fail because job titles vary wildly in formatting.*

### 🛠️ The Solution
- I used Wildcard Pattern Matching (LIKE) combined with Boolean Logic.

- **Inclusion:** I captured variations of titles using LIKE '%Data%' or LIKE '%Business%' combined with LIKE '%Analyst%'.

- **Exclusion:** I applied a negative filter NOT LIKE '%Senior%' to remove high-level roles from the dataset.

```sql
SELECT 
	job_title,
	job_location AS location,
	salary_year_avg AS salary
FROM 
	job_postings_fact
WHERE 
    (job_title LIKE '%Data%' OR job_title LIKE 'Business')
    AND (job_title LIKE '%Analyst%')
    AND (job_title NOT LIKE '%Senior%')
```

### 💡 Key Insights & Technical Analysis
1. **Complex Boolean Logic (Order of Operations)**

- **The Critical Parentheses:** The query uses (A OR B) AND C.

- job_title LIKE '%Data%' OR job_title LIKE '%Business%' is wrapped in parentheses.

- **Why this matters:** Without parentheses, SQL might interpret the AND first, breaking the logic and potentially returning "Business" jobs that aren't "Analysts", or failing to apply the "Senior" exclusion to both groups.

2. **Negative Pattern Matching (NOT LIKE)**

- **Data Cleaning:** This technique is essential when dealing with "dirty" or free-text data. Since we don't have a clean "Experience Level" ID, we have to infer it. Using NOT LIKE acts as a soft filter to remove noise (Senior roles) from the target dataset (Entry/Mid-level roles).

3. **Wildcard Flexibility (%)**

- **Robustness:** Using % (e.g., %Analyst%) allows the query to catch variations like "Junior Data Analyst", "Data Analyst II", or "Business Intelligence Analyst", ensuring we don't miss relevant opportunities just because of extra words in the title.