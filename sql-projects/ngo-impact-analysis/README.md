## Analysis 1: Top Assignments by Donation Value

### Problem
Identify the top 5 assignments based on total value of donations, categorized by donor type.

The output includes:
- assignment_name
- region
- donor_type
- total donation amount (rounded to 2 decimal places)

### SQL Query

```sql
SELECT 
    a.assignment_name,
    a.region,
    d.donor_type,
    ROUND(SUM(dn.amount), 2) AS rounded_total_donation_amount
FROM donations AS dn
LEFT JOIN assignments AS a 
    ON a.assignment_id = dn.assignment_id
LEFT JOIN donors AS d 
    ON dn.donor_id = d.donor_id
GROUP BY 
    a.assignment_name, 
    a.region, 
    d.donor_type
ORDER BY rounded_total_donation_amount DESC
LIMIT 5;
```

### Explanation
- The query joins donations, assignments, and donors tables.
- It groups data by assignment, region, and donor type.
- It calculates total donation amount using `SUM()` and rounds it using `ROUND()`.
- Results are sorted in descending order to identify the top 5 assignments.

### Key Insights
- Certain assignments receive significantly higher funding than others.
- Donation amounts vary depending on donor type (individual vs organization).
- Some regions attract higher-value donations, indicating geographic differences in funding.
- High donation value does not always mean highest impact, which requires further analysis.
