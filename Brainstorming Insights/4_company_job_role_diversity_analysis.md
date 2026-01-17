# 🏢 Corporate Role Diversity Analysis

**📝 Project Description**
- This analysis explores the organizational structure of various companies to identify those with the most diverse professional environments. By quantifying the range of unique job titles, we distinguish between companies that hire in bulk for specific roles versus those expanding across a wide spectrum of functional areas.

**🎯 Problem Statement**
- The objective is to identify the top 10 organizations with the highest number of unique job titles. The challenge lies in filtering out duplicate job postings for the same role to measure the true "breadth" of opportunities within each company.


# 🛠️ The SQL Solution

```sql
WITH title_diversity AS (
SELECT 
    company_id,
    COUNT(DISTINCT job_title) AS unique_titles
FROM 
    job_postings_fact
GROUP BY 
    company_id 
)

SELECT 
    company_dim.name,
    title_diversity.unique_titles
FROM
    title_diversity
INNER JOIN
    company_dim ON title_diversity.company_id = company_dim.company_id
ORDER BY
    unique_titles DESC
LIMIT 10
```


# 💡 Key Insights & Methodology
**1. Defining "Diversity" (Breadth vs. Volume)**
**Analytical Logic:** I utilized COUNT(DISTINCT job_title) rather than a simple count.

**Why it matters:** A company hiring 1,000 employees for a single role (e.g., "Customer Support") has high volume but low diversity. In contrast, a company hiring 100 employees across 20 different specialized roles shows high functional diversity. This metric reveals the complexity of the company's operations.

**2. Modular Query Design (CTE)**
Technical Approach: A Common Table Expression (title_diversity) was used to isolate the aggregation logic (counting titles per company) from the final presentation logic (joining with company names).

Benefit: This separation of concerns makes the code cleaner, more readable, and easier to debug or expand in the future.

**3. Data Context**
The results are limited to the top 10 companies, focusing the analysis on the major players in the market who are likely operating across multiple verticals (Engineering, Sales, Marketing, etc.).