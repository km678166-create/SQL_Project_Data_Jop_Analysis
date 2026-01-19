# 🔗 SQL Relational Query: Skills Compensation Analysis

### ❓ The Challenge
*Quantify the financial value of technical skills by bridging disparate datasets.
The business question was simple: "Which skills command the highest salaries?"
However, the engineering challenge was significant due to the Normalized Database Schema. The necessary data was siloed across three separate tables:*

**1- skills_dim:** Contains the static skill names (e.g., "Python").

**2- job_postings_fact:** Contains the financial data (Salary).

**3- skills_job_dim:** The "Junction Table" that links the two.
The problem was to construct a query that could traverse these relationships to associate a specific dollar amount with an abstract skill name.

### 🛠️ The Solution

*I implemented a Multi-Table Join strategy. I anchored the query on the skills_dim table and used sequential LEFT JOINs to pull in job counts and salary data, aggregating them to calculate the average market value per skill.*

```sql
SELECT 
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS number_of_job_postings,
    AVG(job_postings_fact.salary_year_avg) AS average_salary_for_skill
FROM 
    skills_dim
LEFT JOIN
    skills_job_dim ON skills_dim.skill_id = skills_job_dim.skill_id
LEFT JOIN
    job_postings_fact ON skills_job_dim.job_id = job_postings_fact.job_id
GROUP BY
    skills_dim.skills
ORDER BY
    average_salary_for_skill DESC
```

### 💡 Key Insights & Technical Analysis
**1. Navigating Many-to-Many Relationships**

- **The Junction Table:** In a normalized database, jobs and skills have a "Many-to-Many" relationship (one job has many skills; one skill is in many jobs). This query demonstrates the ability to handle this complexity by correctly joining through the intermediate bridge table (skills_job_dim).

**2. Analytical Pitfall: The "Small Sample Size" Trap**

- **Critical Observation:** Looking at the results, the top-paying skill is "debian" ($196,500). However, the number_of_job_postings is only 1.

- **The Problem:** Sorting purely by "Average Salary" can be misleading because it floats outliers (rare skills with one lucky high-paying job) to the top.

- **The Takeaway:** A data analyst must be careful. While "debian" technically has the highest average, a skill like "mongo" (Rank #4, $169k avg) is likely a more reliable target because it has 382 postings, proving sustained high demand.

**3. Data Completeness (LEFT JOIN)**

- Why LEFT JOIN? Using LEFT JOIN ensures we retain all skills from our dimension table, even if they currently have no job postings attached. This allows us to see gaps in the market (skills with 0 demand) rather than just disappearing them from the report.*