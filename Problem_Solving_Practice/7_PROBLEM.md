# 🌐 SQL Data Analysis: Top Remote Skills in Demand

### ❓ The Challenge

**Identify the most critical technical skills required for the remote job market.**

*With the rise of remote work, the demand for specific tools changes. The goal was to isolate job postings specifically marked as "Work From Home" and quantify which 5 skills appeared most frequently in those descriptions.
The technical challenge involved filtering, aggregating, and joining data across three different tables without creating a "spaghetti code" query that is hard to read or debug.*

# 🛠️ The Solution
**I adopted a Modular CTE (Common Table Expression) approach.**

- **CTE (remote_job_skills):** This focused solely on the "Calculation Engine"—filtering for remote jobs (job_work_from_home = True) and counting skill occurrences by ID.

- **Main Query:** This handled the "Presentation Layer"—joining the calculated IDs back to the skills_dim table to retrieve readable names and ranking the top 5.

```sql
WITH remote_job_skills AS (
 SELECT
    skill_id,
    COUNT(*) AS skill_count
 FROM 
    skills_job_dim 
 INNER JOIN 
    job_postings_fact ON skills_job_dim.job_id = job_postings_fact.job_id
 WHERE 
    job_work_from_home = True
 GROUP BY 
    skill_id
)

SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    skill_count
FROM 
    skills_dim
INNER JOIN
    remote_job_skills ON skills_dim.skill_id = remote_job_skills.skill_id
ORDER BY 
    skill_count DESC
LIMIT 5
```

### 💡 Key Insights & Technical Analysis
**1. Code Modularity (The "Divide and Conquer" Strategy)**

- **Why CTEs?** By isolating the "counting logic" inside the WITH block, the main query becomes incredibly clean. It reads like plain English: "Take the remote skills count, add names, sort them, and show me the top 5." This makes the code reusable and easy for other analysts to audit.

**2. Inner Join vs. Left Join**

- **Strategic Choice:** I used INNER JOIN in both stages.

- **First,** because we only care about jobs that have associated skills.

- **Second,** because we only care about skills that actually exist in our dimension table. This strict joining ensures zero "ghost data" (nulls) in the final report.

**3. Business Value**

- **Career Strategy:** This query answers the question: "If I want to work from home, what should I learn?"

- **Market Insight:** It reveals if remote work favors specific technologies (e.g., Cloud tools, Python) over others (e.g., On-premise hardware skills).