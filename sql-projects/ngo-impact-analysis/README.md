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
- Some regions attract higher-value donations, indicating geographic differences in funding.
- High donation value does not always mean highest impact, which requires further analysis.

## Analysis 2: Top Regional Impact Assignments

### Problem
Identify the most impactful assignment in each region based on impact score, while also considering the number of donations received.

### SQL Query

```sql
WITH assignment_stats AS (
    SELECT 
        a.assignment_name,
        a.region,
        a.impact_score,
        COUNT(dn.donation_id) AS num_total_donations
    FROM assignments AS a
    JOIN donations dn
        ON dn.assignment_id = a.assignment_id
    GROUP BY 
        a.assignment_name,
        a.region,
        a.impact_score
),
ranked_assignments AS (
    SELECT *,
        ROW_NUMBER() OVER (
            PARTITION BY region
            ORDER BY impact_score DESC, num_total_donations DESC
        ) AS rank
    FROM assignment_stats
)
SELECT 
    assignment_name, 
    region, 
    impact_score, 
    num_total_donations
FROM ranked_assignments
WHERE rank = 1
ORDER BY region;
```

### Explanation
- A Common Table Expression (CTE) is used to first calculate total donations per assignment.
- The `ROW_NUMBER()` window function ranks assignments within each region.
- Data is partitioned by region and sorted by impact score and number of donations.
- The top-ranked assignment (rank = 1) is selected for each region.

### Key Insights
- Other assignments receive many donations but may not have the highest impact.
- Regional differences show variation in both funding and project effectiveness.
- Combining impact score with donation count provides a more balanced evaluation.

