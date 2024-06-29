Obs: This is a reproduction of the exercise proposed by the course Python Data Analytics - Full Course for Beginners by Luke Barousse (https://www.youtube.com/watch?v=wUSDVGivd-8)

# The Analysis

## 1. What are the most demand skills for the top 3 most popular demand roles?
To find the most demanded skills for the most popular demand roles, I filtered out those positions by the top 5 skills for the top 3 job titles. 

The notebook with my python code is called [2_Skills_Count.ipynb](3_Project\2_Skills_Count.ipynb)

### Visualize Data
For the chart, I use the following code:

```python
fig, ax = plt.subplots(len(job_titles),1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x = 'skill_percent', y = 'job_skills', ax = ax[i], hue = 'skill_count', palette = 'dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0,80)
    
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v+1, n, f'{v:.0f}%', va='center') 

    if i != len(job_titles) - 1:
        ax[i].set_xticks([]) 

fig.suptitle('Likelihood of Top Skills in Job Postings', fontsize = 15)
fig.tight_layout(h_pad = 0.5)  # fix the overlap
plt.show()

```

### Results
![Top_Skills](3_Project\images\Top_Skills_in_Job_Postings.png)
*Figure 1. Likelihood of Top Skills in Job Postings in the US*

### Luke's Insights
 - Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Enegineers (65%).
 - SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half of the job postings for both roles. For Data Engineer, Python is the most sought-after skill, appearing in 68% of job postings.
 - Data Engineers require more specialized technical skills (AWS< Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau)

 ## 2. How are in-demand Skills trending for Data Analysts?
 ### Visualize Data
 ```python
#Plot top 5 skills

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data = df_plot, dashes=False, palette = 'tab10')

sns.set_theme(style='ticks')
sns.despine()   # remove the outer board
plt.title('Trending Top Skills For Data Analysts in the US')
plt.xlabel('2023')
plt.ylabel('Likelihood in Job Postings')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in [0,1,4]:
    plt.text(11.2, df_plot.iloc[-1,i], df_plot.columns[i])

plt.text(11.2, 27.5, df_plot.columns[2])
plt.text(11.2, 25.5, df_plot.columns[3])

plt.show()
 ```
 
 ![Trending Top Skills for Data Analysts in the US](3_Project\images\Top_Trending_Skills_US.png)
*Figure 2. Trending Top Skills for Data Analysts in the US in 2023*

### Insights:
- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
- Excel is the 2nd most demanded skill, with 40% of job postings requiring it.
- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. SAS, while less demanded compared to the others, shows also a relatively stable demand.

## 3. How well jobs and skills pay for Data Analysts?

### Salary Distribution for Data Jobs
 #### Visualize Data
 ```python
 sns.boxplot(data = df_US_top6, x = 'salary_year_avg', y = 'job_title_short', order = job_order)

plt.title('Salary Distribution in the US')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0,600000)
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))   # FuncFormatter
plt.show()
 ```
 
 ![Salary Distribution of Data Jobs in the US](3_Project\images\Salary_Distribution.png)
*Figure 3. Salary Distribution for Jobs in the US*

#### Insights:
- There is a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.
- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills in circumstances can lead to high pay in these roles. 
- The median salaries increase with the seniority and specialization roles. Senior roles not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

### Highest Paid & Most Demanded Skills for Data Jobs in the US
#### Visualize Data
 ```python
 fig, ax = plt.subplots(2,1)

#Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data = df_DA_top_pay, x = 'median', y = df_DA_top_pay.index, ax = ax[0], hue = 'median', palette='dark:b_r')

#Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data = df_DA_skills, x = 'median', y = df_DA_skills.index, ax = ax[1], hue = 'median', palette = 'light:b')

plt.show()

 ```
 
 ![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](3_Project\images\Highest_Paid_and_Most_Demanded_Skills_US.png)
*Figure 4. The Highest Paid & the Most Demanded Skills for Data Analysts in the US*

#### Insights:
- The top graph shows specialized technical skills (such as `dplyr`, `Bitbucket`,and `Gitlab`) are associated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.
- The bottom graph highlights that foundational skills like `Excel`, `Powerpoint`, and `SQL` are the most in-demand, even though they may not offer the highest salaries. It demonstrates the importance of these core skills for employability in data analysis roles. 
- There is a clear distinction between skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

## 4. What is the most optimal skill to learn for Data Analysts?
### Methodology
1. Group skills to determine median salary and likelihood of being in posting
2. Visualize median salary vs. percent skill demand
3. (Optional) Determine if certain technologies are more prevalent

#### Visualize Data
 ```python
 from adjustText import adjust_text

sns.scatterplot(
    data = df_plot,
    x = 'skill_percent', 
    y = 'median_salary',
    hue = 'technology'
)

sns.despine()
sns.set_theme(style='ticks')

texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i],df_DA_skills_high_demand['median_salary'].iloc[i],txt))

adjust_text(texts, arrowprops = dict(arrowstyle = "->",color = 'gray', lw=1))

ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))   # FuncFormatter

# Formatting x-axis with commas
ax.xaxis.set_major_formatter(mticker.FuncFormatter(lambda x, pos: f'{int(x):,}%'))   # FuncFormatter

plt.show()
 ```
 
 ![Most Optimal Skills for Data Analysts in the US](3_Project\images\Most_Optimal_Skills_Data_Analysts_US.png)
*Figure 5. The Most Optimal Skills by Type for Data Analysts in the US*

#### Insights:
- The graph shows that most of the `programming` skills (in blue) tend to clust at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.
- Analyst tools (in green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.
- The database skills (in orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.