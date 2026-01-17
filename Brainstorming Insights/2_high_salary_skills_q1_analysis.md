# High-Salary Jobs & Skills Analysis

**📝 Project Description**
- This project analyzes the labor market trends for the first quarter of 2023 (January - March). The goal is to identify the most in-demand skills for high-paying roles. The raw data was fragmented across separate monthly tables, presenting a challenge to aggregate the dataset and correlate it with specific technical skills to derive actionable insights.

**🎯 Problem Statement**
- The objective is to construct a unified dataset for Q1 by combining three distinct monthly tables (January, February, March). The query extracts comprehensive job details (location, title, salary) and their associated skills, specifically filtering for positions offering an average yearly salary exceeding $70,000.

**🛠️ The SQL Solution**
```sql
SELECT
    job_postings_q1.job_id,
    job_postings_q1.job_title_short,
    job_postings_q1.job_location,
    job_postings_q1.job_via,
    job_postings_q1.salary_year_avg,
    skills_dim.skills,
    skills_dim.type 
From (
SELECT * 
FROM 
    january_jobs
UNION ALL
SELECT *
FROM 
    february_jobs
UNION ALL
SELECT *
FROM 
    march_jobs
) AS job_postings_q1

LEFT JOIN 
    skills_job_dim ON job_postings_q1.job_id = skills_job_dim.job_id
LEFT JOIN 
    skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE 
    job_postings_q1.salary_year_avg > 70000
ORDER BY 
    job_postings_q1.job_id
```

### 💡 Key Insights & Methodology
1. Data Aggregation Strategy
**Performance Efficiency:**
- Utilized UNION ALL instead of UNION to merge the monthly tables. This method is computationally more efficient for large datasets.

**Data Integrity:** 
- This approach ensures that no data is arbitrarily removed, preserving potentially meaningful duplicate listings that are relevant for temporal analysis (e.g., a job reposted in consecutive months).

**2. "Skills Economy" Analysis**
- Correlation: By linking job postings to the skills_dim table, the query moves beyond a simple job listing. It creates an analytical view that connects Compensation with Competencies, helping to answer the question: "Which technical skills drive salaries above the $70k threshold?"

**3. Handling Missing Data**
- Inclusivity: A LEFT JOIN was strategically chosen to include all high-paying job postings in the final report, even those without specific skill tags in the database (Null Handling). This ensures a comprehensive view of the high-salary market segment without data loss.