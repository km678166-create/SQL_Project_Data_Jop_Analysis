# 🏠 SQL Data Aggregation: Remote vs. On-Site Companies

### ❓ Problem Statement
*Quantify the adoption of Remote Work across different companies.
The objective is to calculate and compare the total number of unique companies that offer "Work From Home" (WFH) options versus those that strictly require on-site presence.*

### 🛠️ Solution
*I used Conditional Aggregation to calculate two separate metrics within a single SELECT statement. By combining COUNT, DISTINCT, and CASE, I effectively "pivoted" the data to get a side-by-side comparison in one result row.*

```sql
SELECT
    COUNT(DISTINCT CASE WHEN job_work_from_home = TRUE THEN company_id END) AS wfh_companies,
    COUNT(DISTINCT CASE WHEN job_work_from_home = FALSE THEN company_id END) AS non_wfh_companies
FROM 
    job_postings_fact
```

### 💡 Key Insights & Analysis

1. **Technique: Conditional Aggregation (Pivoting)**

- **Efficiency:** Instead of running two separate queries (one for WFH, one for non-WFH) or using a GROUP BY that returns multiple rows, this method returns a single-row summary.

- **Logic:** The CASE statement acts as a filter inside the aggregation. If the condition is met, it returns the company_id; otherwise, it returns NULL (which COUNT ignores).

2. **Data Integrity: DISTINCT**

- **Avoiding Duplicates:** Since one company posts multiple jobs, a simple *COUNT(*) would be misleading (it would count postings, not companies). Using DISTINCT company_id ensures we are counting the unique organizations behind the jobs.*

3. **Business Value**

- **Market Trends:** This metric acts as a proxy for "Modern vs. Traditional" workplace culture.

- **Policy Analysis:** It helps understand the saturation of remote-friendly employers in the market.