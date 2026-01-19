# 📈 SQL Financial Modeling: Project Earnings Forecasting

### ❓ The Challenge
*Simulating the financial impact of a rate increase on project revenue.
The business question was: "How would our total revenue per project change if we increased our hourly rate by $5?"
The challenge lies in calculating two distinct financial metrics simultaneously from the same raw dataset: the Actual Current Earnings (based on historical data) and the Projected Earnings (based on a hypothetical scenario). This requires applying mathematical transformations inside aggregate functions.*

### 🛠️ The Solution
- I used aggregate functions (SUM) combined with arithmetic operators to create a side-by-side comparison.

- **Current State:** hours_spent * hours_rate

- **Future State:** hours_spent * (hours_rate + 5)

```sql
SELECT 
	project_id,
    SUM(hours_spent * hours_rate) AS current_earnings,
    SUM(hours_spent * (hours_rate + 5)) AS projected_earnings
FROM 
	invoices_fact
GROUP BY 
	project_id
```

### 💡 Key Insights & Technical Analysis
1. **Scenario Analysis in SQL**

- **The Power of "What-If":** Instead of exporting data to Excel to model a price hike, this query performs the simulation directly in the database. This technique is highly scalable—whether you have 10 projects or 10 million rows, the calculation happens instantly.

- **Order of Operations:** The parentheses in (hours_rate + 5) are critical. They ensure the rate is increased before multiplying by hours spent. Without them, SQL would multiply first (PEMDAS), leading to completely wrong financial projections.

2. **Derived Metrics**

- **On-the-Fly Calculation:** We created two new business metrics (current_earnings, projected_earnings) that don't exist in the database table. This demonstrates the ability to transform raw operational data (hours log) into strategic financial data (revenue forecast).

3. **Business Value**

- **Decision Support:** This output allows management to instantly see which projects would yield the highest ROI from a rate increase, helping them decide if a $5 raise is "reasonable" or if it would generate significant extra revenue.