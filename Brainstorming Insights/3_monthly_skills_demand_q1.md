# 📊 Monthly Skills Demand Analysis (Q1 2023)

**📝 Project Description**
- This project tracks the monthly demand for specific technical skills during the first quarter of 2023. By consolidating fragmented data from separate monthly tables, the analysis highlights trends in skill popularity, allowing for a granular view of the labor market's evolving requirements.

**🎯 Problem Statement**
- The goal is to analyze the frequency of job postings for each skill on a monthly basis (January to March). The raw data is distributed across three separate tables (january_jobs, february_jobs, march_jobs), requiring unification and temporal extraction to calculate the total postings_count per skill per month.

### 🛠️ The SQL Solution
```sql
WITH combined_job_postings AS(
SELECT 
    job_id,
    job_posted_date
FROM 
january_jobs
UNION ALL 
SELECT 
    job_id,
    job_posted_date
FROM 
    february_jobs
UNION ALL
SELECT 
    job_id,
    job_posted_date
FROM 
march_jobs
),

monthly_skill_demand AS(
SELECT 
    skills_dim.skills,
    EXTRACT(YEAR FROM combined_job_postings.job_posted_date) AS year,
    EXTRACT(MONTH FROM combined_job_postings.job_posted_date) AS month,
    COUNT(combined_job_postings.job_id) AS postings_count 
FROM 
    combined_job_postings
INNER JOIN 
    skills_job_dim ON combined_job_postings.job_id = skills_job_dim.job_id
INNER JOIN 
    skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
GROUP BY 
    skills_dim.skills,
    year,
    month
)

SELECT 
    skills,
    year,
    month,
    postings_count
FROM 
    monthly_skill_demand
ORDER BY 
    skills,
    year,
    month
```

#### 💡 Key Insights & Methodology
**1. Modular Logic (CTEs vs. Subqueries)**
**Approach:** Utilized Common Table Expressions (CTEs) to structure the query.

**Insight:** Unlike nested subqueries, CTEs (combined_job_postings, monthly_skill_demand) break the complex logic into readable, logical steps:

**Consolidate:** Merge raw data.

**Enrich:** Join with dimension tables and extract dates.

**Present:** Final selection.

**2. Temporal Granularity**
**Approach:** Applied the EXTRACT(YEAR/MONTH) function on the job_posted_date.

**Insight:** Instead of a generic quarterly total, this enables Time-Series Analysis. We can observe if a skill (e.g., Python) spiked in January but dropped in March, providing deeper market intelligence than a simple aggregate count.

**3. Data Unification Strategy**
**Approach:** Used UNION ALL to merge the three monthly tables.

**Insight:** UNION ALL is chosen over UNION for performance efficiency, as we know the monthly datasets are distinct (no need to check for duplicates across months), ensuring faster processing of large datasets.