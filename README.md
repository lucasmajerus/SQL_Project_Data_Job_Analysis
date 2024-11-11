# Introduction 
📊 Welcome to my SQL portfolio project, where I'm looking at the data job market, with a particular focus on data analyst positions. This project is a personal exploration aimed at training me in data analysis. 
The project itself consisted in identifying the highest-paying jobs, the most in-demand skills and the intersection between high demand and high pay in the field of data analysis.

Discover my SQL queries here : [project_sql folder](/project_sql)

# Background
This project was motivated by my desire to better understand the job market for data analysts. I wanted to find out which skills are the best paid and most in demand, in order to make my job search more targeted and effective. 

The data used for this analysis comes from Luke Barousse's [SQL COURSE](https://lukebarousse.com/sql). The data includes details of job titles, salaries, locations and skills required. 

### The questions I wanted to answer through my SQL queries were.

1. What are the highest-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to acquire for a data analyst wishing to maximize his or her value on the job market?

# Tools I Used
In this project, I utilized a variety of tools to conduct my analysis:

- **SQL**: Enabled me to interact with the database, extract insights, and answer my key questions through queries.
- **PostgreSQL**: As the database management system, PostgreSQL allowed me to store, query, and manipulate the job posting data.
- **Visual Studio Code:** This open-source administration and development platform helped me manage the database and execute SQL queries.

# Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here’s how I approached each question:

### 1. Top Paying Data Analyst Jobs
To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.
```sql
SELECT
	job_id,
	job_title,
	job_location,
	job_schedule_type,
	salary_year_avg,
	job_posted_date
FROM
	job_postings_fact
WHERE
	job_title = 'Data Analyst'
	AND salary_year_avg IS NOT NULL
	AND job_location ILIKE 'Anywhere'
ORDER BY
	salary_year_avg DESC 
LIMIT 10
```
Here's the breakdown of the top data analyst jobs in 2023:
- **Wide Salary Range:** The top 10 paying data analyst roles span from $184,000 to $650,000, indicating significant salary potential in the field.
- **Diverse Employers:** Companies like Meta or AT&T are among those offering high salaries, showing a broad interest across different industries.
- **Job Title Variety:** There's a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within data analytics.

