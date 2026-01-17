# 🌍 SQL Date Manipulation: Monthly Job Trends (Timezone Adjusted)

### ❓ Problem Statement
*Analyze the seasonal volume of job postings based on specific time zones.
The goal is to calculate the total number of job postings per month. Crucially, the analysis requires converting the raw UTC timestamps into the 'America/New_York' time zone before extraction to ensuring accurate reporting for US-based business cycles.*

### 🛠️ Solution
*I implemented a Double Time Zone Conversion strategy to accurately shift the timestamp from UTC to New York time. I then extracted the month component and aggregated the total postings.*

```sql
SELECT 
    EXTRACT(MONTH FROM job_posted_date AT TIME ZONE 'UTC' AT TIME ZONE 'America/New_York') AS month,
    COUNT(job_id) AS postings_count
FROM 
    job_postings_fact
GROUP BY
    month
ORDER BY 
    month
```

### 💡 Key Insights & Analysis
1. **Technique: Time Zone Handling (AT TIME ZONE)**

- **The Logic:** job_posted_date is stored in UTC (Coordinated Universal Time).

- **AT TIME ZONE 'UTC':** Explicitly tells SQL to interpret the column as a UTC timestamp.

- **AT TIME ZONE 'America/New_York':** Converts that UTC time into the local New York time.

- **Why It Matters:** A job posted at 1:00 AM UTC on February 1st is actually 8:00 PM EST on January 31st. Without this conversion, that job would be counted in the wrong month (February instead of January). This accuracy is vital for precise monthly reporting.

2. **Date Extraction**

- **Granularity:** By using EXTRACT(MONTH FROM ...) after the conversion, we ensure we are grouping by the "local" month. This reveals true seasonality (e.g., "Is hiring slower in December?").

3. **Business Value**

**Operational Alignment:** Aligning data to the headquarters' time zone (e.g., New York) ensures that reports match the company's fiscal calendar and business hours.