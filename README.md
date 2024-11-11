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

![Top paying roles](/Assets/1_top_paying_roles.png)
*Bar graph visualizing the salary for the top 10 ones for data analysts; generated by a prompt in ChatGpt from my SQL query results*

### 2. Skills for Top Paying Jobs
To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.
```sql
-- Gets the top 10 paying Data Analyst jobs 
WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        salary_year_avg
    FROM
        job_postings_fact
    WHERE
        job_title_short = 'Data Analyst'
				AND salary_year_avg IS NOT NULL
        AND job_location = 'Anywhere'
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)
SELECT
    top_paying_jobs.job_id,
    job_title,
    salary_year_avg,
    skills
FROM
    top_paying_jobs
	INNER JOIN
    skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
	INNER JOIN
    skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC
```
Here's the breakdown of the most demanded skills for the top 10 highest paying data analyst jobs in 2023:

- **SQL** is leading with a bold count of 8.
- **Python** follows closely with a bold count of 7.
- **Tableau** is also highly sought after, with a bold count of 6. Other skills like R, Snowflake, Pandas, and Excel show varying degrees of demand.
AJOUTER PHOTO

### 3. In-Demand Skills for Data Analysts
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.
```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```
Here's the breakdown of the most demanded skills for data analysts in 2023:
- **SQL and Excel** remain fundamental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.
- **Programming and Visualization Tools** like **Python**, **Tableau**, and **Power BI** are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.

| Skills   | Demand Count |
|----------|--------------|
| SQL      | 7291         |
| Excel    | 4611         |
| Python   | 4330         |
| Tableau  | 3745         |
| Power BI | 2609         |

*Table of the demand for the top 5 skills in data analyst job postings*

### 4. Skills Based on Salary
Exploring the average salaries associated with different skills revealed which skills are the highest paying.
```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```
Here is a breakdown of the results for top paying skills for Data Analysts. Could you give me some quick trends and summary from these insights ? 
Answer : 
- **Big Data and Data Engineering Skills Lead in Pay**: Skills like PySpark and Couchbase command top salaries, highlighting a premium on handling large-scale data processing.
- **Version Control and Collaboration Tools:** Knowledge in tools like Bitbucket and GitLab is highly compensated, reflecting demand for collaborative and version-controlled workflows.
- **Cloud & DevOps Integration:** Skills in Kubernetes, Airflow, and GCP are well-paid, indicating the importance of cloud infrastructure and automation in data analyst roles.

| Skills       | Average Salary ($) |
|--------------|---------------------|
| pyspark      | 208,172             |
| bitbucket    | 189,155             |
| couchbase    | 160,515             |
| watson       | 160,515             |
| datarobot    | 155,486             |
| gitlab       | 154,500             |
| swift        | 153,750             |
| jupyter      | 152,777             |
| pandas       | 151,821             |
| elasticsearch| 145,000             |

*Table of the average salary for the top 10 paying skills for data analysts*

### 5. Most Optimal Skills to Learn
Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.
```sql
SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

| Skill ID | Skills     | Demand Count | Average Salary ($) |
|----------|------------|--------------|---------------------|
| 8        | go         | 27           | 115,320            |
| 234      | confluence | 11           | 114,210            |
| 97       | hadoop     | 22           | 113,193            |
| 80       | snowflake  | 37           | 112,948            |
| 74       | azure      | 34           | 111,225            |
| 77       | bigquery   | 13           | 109,654            |
| 76       | aws        | 32           | 108,317            |
| 4        | java       | 17           | 106,906            |
| 194      | ssis       | 12           | 106,683            |
| 233      | jira       | 20           | 104,918            |

*Table of the most optimal skills for data analyst sorted by salary*
Here's a breakdown of the most optimal skills for Data Analysts in 2023:
- **High-Demand Programming Languages:** Python and R stand out for their high demand, with demand counts of 236 and 148 respectively. Despite their high demand, their average salaries are around $101,397 for Python and $100,499 for R, indicating that proficiency in these languages is highly valued but also widely available.
- **Cloud Tools and Technologies:** Skills in specialized technologies such as Snowflake, Azure, AWS, and BigQuery show significant demand with relatively high average salaries, pointing towards the growing importance of cloud platforms and big data technologies in data analysis.
- **Business Intelligence and Visualization Tools:** Tableau and Looker, with demand counts of 230 and 49 respectively, and average salaries around $99,288 and $103,795, highlight the critical role of data visualization and business intelligence in deriving actionable insights from data.
- **Database Technologies:** The demand for skills in traditional and NoSQL databases (Oracle, SQL Server, NoSQL) with average salaries ranging from $97,786 to $104,534, reflects the enduring need for data storage, retrieval, and management expertise.

# What I Learned
Throughout this project, I honed several key SQL techniques and skills:

- 🧩 **Complex Query Construction**: Learning to build advanced SQL queries that combine multiple tables and employ functions like **`WITH`** clauses for temporary tables.
- 📊 **Data Aggregation**: Utilizing **`GROUP BY`** and aggregate functions like **`COUNT()`** and **`AVG()`** to summarize data effectively.
- 💡**Analytical Thinking**: Developing the ability to translate real-world questions into actionable SQL queries that got insightful answers.
