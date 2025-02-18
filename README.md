# Data Science Job Market Insights

## Overview
This project explores the data science job market, focusing on understanding high-demand roles, essential skills, and salary insights for data professionals. With an emphasis on data analysts, data scientists, and software engineers, this analysis aims to guide job seekers in identifying lucrative opportunities within the data science field.

## Questions Answered
1. **What are the most common job titles in data science?**
2. **What are the most demanded skills for the top 3 most popular data roles?**
3. **What is the most optimal skill to learn for Data Analysts?**

## Methodology

### Data Cleaning and Preparation:
- Loaded and cleaned a dataset of job postings.
- Converted date strings to datetime objects for time-based analysis.
- Processed skill lists from JSON format to Python lists for easier manipulation.

```python
import ast
import pandas as pd
import seaborn as sns

import matplotlib.pyplot as plt

# Loading Data
df= pd.read_csv("/data_jobs.csv")
     

#Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
     
df_kenya= df[df["job_country"]=="Kenya"]
df_skills = df_kenya.explode('job_skills')
df_skills[['job_title', 'job_skills']]
```

## Tools Used
- **Python:** For all data manipulation and analysis.
- **Libraries:**
  - **Pandas:** Data manipulation and analysis.
  - **Seaborn, Matplotlib:** Data visualization.
### Analysis:
## Common Job Titles in Data Science
**Explanation:** 
This analysis identifies which data science roles are most frequently listed in job postings, giving an overview of market demand.

**Notebook:** [EDA.ipynb](_EDA.ipynb)

**Visualization:**
```python
df_plot = df['job_title_short'].value_counts().reset_index()
df_plot.columns = ['job_title_short', 'count']
sns.set_theme(style='ticks')
sns.barplot(data=df_plot, x='count', y='job_title_short', palette='dark:b_r')
sns.despine()
plt.title('Number of Jobs per Job Title')
plt.xlabel('Number of Jobs')
plt.ylabel('')
plt.show()
```
**Results:**

![Top Data Roles](Images/Top%20Data%20roles.png)

**Insights**

Data Analyst, Data Engineer, and Data Scientist roles are leading the job market, indicating a strong need for skills in data analysis, infrastructure, and advanced analytics.
Career Path: These roles offer clear career progression paths within the data field.
Skill Overlap: There's a significant overlap in required skills, suggesting versatility is key

## Most demanded skills for the top 3  data roles in Kenya

**Explanation:**

This part of the analysis identifies the skills most in demand for the three most popular data roles, helping to pinpoint what skills to focus on for each role.

Notebook: [Skill_Demand](_Skill_Demand.ipynb)

**Visualization:**

```python
fig, ax = plt.subplots(len(job_titles), 1, figsize=(8, 3* len(job_titles)))

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_count[df_skills_count['job_title_short'] == job_title].head(5)[::-1]


    # Plot data
    sns.barplot(data=df_plot,  x='skill_count', y='job_skills', ax=ax[i], palette='YlOrBr' )

    # Formatting
    ax[i].set_title(job_title, fontsize=12)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].set_xlabel('Skill Count')
    ax[i].set_xlim(0, df_skills_count['skill_count'].max() + 150)  # Dynamic scale


# Set overall figure title
fig.suptitle('Counts of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5)  # Adjust spacing

# Show plot
plt.show()
```
**Results:**

![Skill demand for top 3 data roles in kenya](/Images/Skill%20Demand%20for%20Top%203%20Roles%20In%20Kenya.png)

**Insights:**
Data Analyst: Excel is the most demanded skill, followed closely by SQL, Python, R, and Tableau. This indicates a strong need for traditional data handling tools alongside modern programming languages for analysis.

Data Scientist: Python leads in demand, with R and SQL also being highly sought after. Tableau and SAS are less common, suggesting a focus on programming and statistical skills over visualization tools in this role.

Software Engineer: Python is again the top skill, highlighting its versatility. SQL, Java, AWS, and Kubernetes follow, showing a blend of database management, programming, and cloud technology skills are crucial for software engineers in data-related roles.

## Optimal Skills for Data Analysts

**Explanation:**
By examining the skill sets associated with Data Analyst positions in the US, we determine which skills are most prevalent and correlate them with median salaries to find the 'most optimal' skill.

Notebook: [Optimal_Skill](_Optimal_Skill.ipynb)

**Visualization:**
```python
skill_limit = 5

df_DA_skills_high_demand = df_DA_skills[df_DA_skills['skill_percent'] > skill_limit]
     


filtered_df = df_DA_skills_high_demand[df_DA_skills_high_demand['median_salary'] <= 108000]

# Create figure
plt.figure(figsize=(8, 6))

# Scatter plot
plt.scatter(filtered_df['skill_percent'], filtered_df['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')
plt.title('Most Optimal Skills for Data Analysts in the US')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Formatting salary in K

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(filtered_df.index):
    texts.append(plt.text(filtered_df['skill_percent'].iloc[i],
                          filtered_df['median_salary'].iloc[i],
                          " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

# Show the plot
plt.show()
```

**Results:**

![Optimal Skill For Data Analyst](/Images/Optimal%20skill.png)

**Insights:**
Python and SQL are the most valuable skills, offering high demand and salaries over $100K.
AWS and Azure are lucrative but less common.
Tableau and Power BI are important for visualization, with salaries around $90K.
Excel is widely demanded but correlates with lower salaries around $85K.


## What I Learned

Through this analysis of job postings, I gained insights into the critical skills required for various data roles. I learned that proficiency in Python is universally valuable across Data Analysts, Data Scientists, and Software Engineers, emphasizing its importance in the data ecosystem. SQL's widespread necessity confirmed its foundational role in data management. For Data Analysts, traditional tools like Excel remain highly relevant, whereas Data Scientists lean more towards statistical programming like R

## Challenges I Faced

Limited Data for Kenya: Sourcing enough job posting data specifically for Kenya was challenging, which might have affected the accuracy of our analysis.
Complex Data Analysis: Handling the complexity of data analysis, especially with multiple skill sets across different roles, required significant effort.
Data Issues: Dealing with inconsistent or missing data in the dataset was a hurdle, impacting the reliability of our findings.

## Conclusion
This project provided valuable insights into the data science job market in Kenya, highlighting the demand for skills like Python, SQL, and Excel across different roles. Despite challenges such as limited data availability, complex analysis, and data inconsistencies, the analysis confirmed the critical importance of these skills for Data Analysts, Data Scientists, and Software Engineers. Job seekers can use these findings to tailor their skill development, focusing on the most lucrative and in-demand competencies in the Kenyan job market.
