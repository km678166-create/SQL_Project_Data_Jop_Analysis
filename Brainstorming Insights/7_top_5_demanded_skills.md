# 📊 SQL Data Analysis: Identifying Top 5 In-Demand Skills

### ❓ Problem Statement
- Identify the top 5 skills most frequently mentioned in job postings.
The goal is to determine which technical skills are currently most sought after by employers by calculating the frequency of each skill in job descriptions and retrieving their specific names.

### 🛠️ Solution
- I utilized a Common Table Expression (CTE) to first aggregate and count skill occurrences, ensuring the code remains modular and readable. Then, I performed an INNER JOIN to map these counts to the actual skill names.

```sql
WITH num_skills AS(
SELECT
    skills_job_dim.skill_id,
    COUNT(skills_job_dim.job_id) AS skill_count
FROM 
    skills_job_dim
GROUP BY
    skill_id
)

SELECT
    skills_dim.skills
FROM
    skills_dim
INNER JOIN
    num_skills ON skills_dim.skill_id = num_skills.skill_id
ORDER BY
    skill_count DESC
LIMIT 5
```

# 💡 Key Insights & Analysis
**1. Approach:** CTE vs. Subquery
- While the problem statement suggested a standard subquery, I opted for a CTE (WITH num_skills AS...).

- Why? CTEs create a temporary result set that makes the main query cleaner and easier to read. It separates the "calculation" logic (counting skills) from the "retrieval" logic (getting names).

**2. Data Logic**
- **Aggregation:** The query groups data by skill_id to calculate the volume of demand for every individual skill.

- **Ranking:** By ordering by skill_count DESC, we instantly float the "market dominators" to the top.

- **Optimization:** The LIMIT 5 clause ensures efficient processing by only returning the most critical data points needed for the report.

**3. Business Value**
- This query effectively answers the question: "What should a job seeker learn next?"

- By isolating the top 5 skills, we filter out noise and niche technologies.

- This data drives strategic decisions for career development and curriculum planning (e.g., focusing on Python or SQL if they appear in the top 5).