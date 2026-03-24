# Student Mental Health Analysis

## Project Overview
This project explores student mental health data using PostgreSQL to identify patterns and trends related to student wellbeing when they go to a university that is in a different country.

## Objectives
- Analyze a study that concludes that international students have a higher risk of mental health than the general population 
- To find out if social connectedness (belonging to a social group) and acculturative stress (stress associated with joining a new culture) are predictive of depression.
- Explore the data to find out if the length of stay is a contributing factor.

## Dataset
The dataset contains student data including types of students (international or domestic), age, length of stay in years, total score of depression (PHQ-9 test), total score of connectedness (SCS test), total score of acculturative stress (ASISS test), and academic level.

## SQL Analysis

Example queries used:

- Average of the total score of depression, social connectedness and acculturative stress which are rounded to two decimal places.
- Counting of the number of international students for each length of stay.
- Filtering the types of students to only show the results for international students.
- Grouping and sorting data by length of stay.

## Example Query

SELECT stay, COUNT (*) AS count_int, ROUND(AVG(todep), 2) AS average_phq, ROUND(AVG(tosc), 2) AS average_scs, ROUND(AVG(toas), 2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;

## Key Insights

- Most students are in early stay durations (1–3), indicating a skew toward newer students.
- Depression (PHQ) scores show a slight increase with longer stay durations.
- Social connectedness (SCS) levels remain relatively stable across all stay groups.
- Acculturative stress (AS) appears higher among students with moderate stay durations.

## Tools Used
- SQL (PostgreSQL)
- Data Analysis
