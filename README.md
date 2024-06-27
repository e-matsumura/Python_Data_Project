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
![Likelihood of Top Skills in Job Postings](3_Project\images\Likelihood of Top Skills in Job Postings.png)



