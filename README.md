 # Overview

 The purpose of this project to help navigate and understand the job market effectively. Insights are primarily focussed on US job market, as there wasn't enough South African data to work with. It delves into the highest earning and in-demand skills to assist in finding optimal job opportunities for data analysis.

 Data is sourced from [Luke Barousse's Python Course]('https://www.lukebarousse.com/courses') which served as a foundation for my analysis, containing detailed information on job titles, salaries, locations, and skills. With use of various Python scripts, I explore key questions such as most demanded skills, salary trends, and the intersection of demand and salary in data.

 # Questions Explored

 1. In-demand skills for 3 most popular data roles
 2. In-demand skills trending for data analysts
 3. How well jobs and skills pay for data analysts
 4. Optimal skills for data analysts to acquire (high demand, high pay)

 # Tools Used

 * **Python:** used to analyse data and unearth critical insights
    * **Pandas Library:** used to analyse the data
    * **Matplotlib Library:** used for visualisations
    * **Seaborn Library:** used to further customise visuals
* **Jupyter Notebooks:** used to run Python scripts
* **Visual Studio Code:** for executing Python scripts
* **Git & GitHub:** essential for version control and sharing Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section highlights all steps taken for data preparation, ensuring accuracy and usability.

## Import & Clean Data

Crucial libraries were imported, and dataset was loaded, followed by initial data cleaning tasks to ensure data quality.

```Python
# import libraries
import ast
import pandas as pd
from datasets import load_dataset
import matplotlib.pyplot as plt
import seaborn as sns

# load data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas() 
    
# data cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter US Jobs

Code used to extract job postings exclusively in the United States.

``` Python
df_US = df[df['job_country'] == 'United States']
```

## The Analysis

Every Jupyter notebook pertaining to this project is aimed at investigating specific aspects of data science job market. This is how I approached each question:

### 1. In-demand skills for 3 most popular data roles

To find most demanded roles for the top 3 most popular jobs, first the required positions were filtered out based on which were most popular, then extracted the top 5 skills for said positions. Below query highlights most popular titles and their top skills, showing which skills should be paid attention, depending of role you are pursuing.

Detailed steps are here: [2_Skills_Count](3_Project/2_Skills_Count.ipynb)

### Visualise Data

``` Python
fig, ax = plt.subplots(len(job_titles), 1)

for i, jt in enumerate(job_titles):
    df_plot = df_merged[df_merged['job_title_short'] == jt].head(5)[::-1]
    sns.barplot(data=df_plot, x='percentage', y='job_skills', ax = ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results
![alt text](/3_Project/images/q1_results.png)

**Insights:**

* SQL is most requested skill for data analysts and engineers, with over half of postings for both positions. For data scientist, Python is most sought-after, appearing in 72% of postings.

* Data engineers require more specialised skills (AWS, Spark, Azure) compared to analysts and scientists, who are expected to be proficient in general data management and analysis tools (SQL, Tableau).

* Python is versatile, as it is in demand across all roles, with more prominence for scientists (72%) and engineers (65%).

### 2. In-demand skills trending for data analysts

To find how skills trended in 2023 for data analysts, analyst positions were filtered out, and grouped skills by month of job posting. With this, the top 5 skills of data analysts by month, highlighting popular skills throughout that year.

Detailed steps are here: [3_Skills_Trend]('/3_Project/3_Skills_Trend.ipynb')

### Visualise Data

``` Python
from matplotlib.ticker import PercentFormatter

sns.lineplot(df_plot, dashes = False, markers = True, palette = 'tab10')

ax = plt.gca()

plt.show()
```

### Results
![alt text](/3_Project/images/q2_results.png)

**Insights:**

* **SQL remains the most in-demand skill** throughout the year, but shows a gradual decline, suggesting it’s becoming a baseline requirement rather than a differentiator.

* **Python demand is steady and slightly increasing**, indicating growing importance for automation and advanced analytics in data analyst roles.

* **Excel demand is resilient but seasonal**, dipping mid-year and rebounding strongly toward year-end, likely tied to business and reporting cycles.

* **Tableau and Power BI show stable, lower demand**, reflecting mature or niche usage rather than rapid growth.


### 3. How well jobs and skills pay for data analysts

### Salary Analysis for Data Professionals

Detailed steps are here: [4_Salary_Analysis]('/3_Project/4_Salary_Analysis.ipynb')

### Visualise Data

```Python
job_list = [df_US_top6[df_US_top6['job_title_short'] == job_title]['salary_year_avg'] for job_title in job_titles]

sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order = job_order)

ax = plt.gca()
plt.show()
```

### Results
![alt text](/3_Project/images/q3_results.png)

**Insights:**

* **Senior roles earn substantially more than non-senior roles**, with Senior Data Scientists and Senior Data Engineers showing the highest median salaries.
* **Data Engineers and Data Scientists generally out-earn Data Analysts**, indicating higher pay for roles focused on engineering and advanced modeling.
* **Wide salary spreads and many high-end outliers** (especially for data science and engineering roles) suggest strong upside potential based on company, location, or specialization.
* **Data Analyst roles have the lowest median salaries but more compact ranges**, indicating more standardized pay compared to other data roles.

### Salary Analysis per Skill

Detailed steps are here: [4_Salary_Analysis]('/3_Project/4_Salary_Analysis.ipynb')

### Visualise Data

```Python
fig, ax = plt.subplots(2, 1)

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

### Results
![alt text](/3_Project/images/q3.1_results.png)

**Insights:**

* **Foundational skills drive employability**: Python, SQL, Excel, Tableau, and Power BI emerge as the most in-demand skills, highlighting their importance as core requirements for data analyst roles across industries.

* **Niche technical skills offer higher earning potential**: Specialized tools such as dplyr, GitLab, Bitbucket, and Solidity are associated with higher median salaries, indicating that advanced or less common skills can significantly increase compensation.

* **Demand and salary are not always aligned**: Several high-paying skills do not rank among the most in-demand, suggesting that while these skills are lucrative, they are required in more specialized roles.

* **Balanced skill development is strategic**: Developing strong foundational analytics skills alongside selective specialization can help analysts remain competitive while positioning themselves for higher-paying opportunities.

### 4. Optimal skills for data analysts to acquire

Detailed steps are here: [5_Optimal_Skills]('/3_Project/5_Optimal_Skills.ipynb')

### Visualise Data

``` Python
df_DA_skills_hd.plot(kind = 'scatter', x = 'skill_percentage', y = 'median_per_skill')

# plots labels for dots on scatter plot
texts = []
for i, txt in enumerate(df_DA_skills_hd.index):
    texts.append(plt.text(df_DA_skills_hd['skill_percentage'].iloc[i], df_DA_skills_hd['median_per_skill'].iloc[i], txt))
adjust_text(texts, arrowprops = dict(arrowstyle = "->", color = 'r', lw = 1))

ax = plt.gca()

plt.show()
```

### Results
![alt text](/3_Project/images/q4_results.png)

**Insights:**

* **Python stands out as the most optimal skill**, combining a high median salary with strong demand, making it one of the most valuable skills for data analysts to master.

* **SQL shows the highest demand**, appearing in the majority of job postings, while still offering a competitive median salary—reinforcing it as a core requirement for data analyst roles.

* **BI and visualization tools offer balanced value**: Skills like *Tableau and Power BI* sit in the mid-to-high range for both salary and demand, highlighting their importance for analytics and reporting roles.

* **Office and presentation tools are lower-impact for salary growth**: While *Excel, PowerPoint, and Word* appear frequently, they are associated with comparatively lower median salaries, suggesting they are foundational rather than differentiating skills.

# What I Learned

My understanding of the data analyst job market was deepened, and my technical skills in Python were developed, especially in relation to data manipulation and visualisation.

* **Advanced Python Use:** libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualisation, and other libraries have assisted in performing complex analysis.

* **Data Cleaning Importance:** learnt that thorough cleaning and preparation are crucial before analysis can be conducted, to ensure accuracy of insight.

* **Strategic Skill Analysis:** emphasised importance of aligning own skills with market demand. to understand correlation between skill demand, salary, and employment availability allows for strategic career planning.
