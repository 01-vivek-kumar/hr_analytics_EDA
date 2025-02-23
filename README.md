# HR Employee Engagement Analysis

## 🚀 Introduction
In this project, I explored an HR dataset to analyze employee engagement. The goal was to identify factors influencing engagement and provide actionable insights for HR teams.

## 📚 Step 1: Importing Libraries
To start, I imported essential Python libraries for data manipulation, visualization, and analysis:

```python
import pandas as pd 
import numpy as np 
import matplotlib.pyplot as plt
import seaborn as sns

import warnings
warnings.filterwarnings('ignore')
```

📂 Step 2: Uploading the Dataset
Since I worked in Google Colab, I uploaded the dataset directly from my computer:

```
from google.colab import files

# Upload file from your computer
uploaded = files.upload()
```

## 📥 Step 3: Loading the Dataset
After uploading the dataset, I loaded it into a pandas DataFrame for exploration:

```python
df = pd.read_csv('HRDataset_v14.csv')
```

## 🔍 Step 4: Exploring the Dataset
To understand the dataset structure, I performed an initial exploration using pandas functions.

### 🔸 First Few Rows
To get a quick look at the dataset:

```python
df.head()

```

🔸 Column Names
To check all available features:

df.columns
df.info()
df.describe()
df.shape
df.dtypes
df.index


## 🧹 Step 5: Data Cleaning for EDA

Before diving into the analysis, I cleaned the dataset to ensure accurate insights.

### 🔸 Handling Null Values
To identify missing values in the dataset:

```python
df.isnull().sum()  # Date of Termination has null values

```

🔸 How I Handled It
I considered how to address the missing values and followed this approach:

Team Discussion: Ideally, consult the team to decide whether the missing values are relevant.
Common Approaches:
For numerical columns:
median if there are outliers,
mean if there are no outliers.
For categorical columns: Use the mode.
My Solution: In this case, I filled the DateofTermination null values with "0" to indicate no termination:

```
df.DateofTermination.fillna('0', inplace=True)
```

### 🔸 Rechecking Null Values
After filling the missing values, I rechecked the dataset to ensure no null values remained:

```python
df.isnull().sum()  # Date of Termination is now handled
```

## 🔄 Step 6: Checking for Duplicate Values

To ensure data integrity, I checked for duplicate rows in the dataset:

```python
df.duplicated().sum()  # No duplicate values found
```

Although there were no duplicates, it's always good practice to drop any if found:

## 📊 Step 7: Analyzing Data Quality

To further assess data quality, I checked the unique values, data types, and null counts for each column:

```python
for col in df.columns:
    print(df[col].value_counts(), '\n', 'Count of na:', df[col].isna().sum(), '\n')
```

🔸 Key Findings
Data Types: All columns had appropriate data types.
Unique Values: No redundant names or inconsistent values were found.
Null Values: Most columns were clean, except for ManagerID, which had 8 missing values. I planned to investigate this further before deciding how to handle it.

## 📈 Step 8: Exploratory Data Analysis (EDA)

With a clean dataset, I began the exploratory data analysis to uncover patterns, trends, and insights about employee engagement.

The EDA process involved:
1. **Univariate Analysis:** Analyzing individual features.  
2. **Bivariate Analysis:** Exploring relationships between features.  
3. **Multivariate Analysis:** Understanding how multiple variables interact.  

Let's dive into the analysis!

### 💰 Step 8.1: Employees with the Highest Salary

To identify the top 10 highest-paid employees, I sorted the dataset by the `Salary` column in descending order:

```python
# Top 10 highest employee salaries
df.Salary.sort_values(ascending=False, axis=0).head(10)
```

### 🌟 Step 8.2: Performance Score Analysis

Understanding employee performance is crucial for HR to identify who excels and who might need support. I analyzed the distribution of performance scores:

```python
df.PerformanceScore.value_counts()
```

### 🔍 Step 8.3: Identifying Employees on PIP

To further investigate, I identified the employees with a **Performance Improvement Plan (PIP)** status:

```python
# Unique performance scores
df.PerformanceScore.unique()

# Names of employees on PIP
df['Employee_Name'][df.PerformanceScore == 'PIP']

# Full details of employees on PIP
df[df.PerformanceScore == 'PIP']
```

This step helped pinpoint employees who might need additional training, mentorship, or support to improve their performance.
HR can use this information for targeted engagement strategies.

### 📊 Step 8.4: Count of Employees Needing PIP

To determine how many employees are currently on a **Performance Improvement Plan (PIP):**

```python
# Count of employees on PIP
len(df[df.PerformanceScore == 'PIP'])
```

### 🚩 Step 8.5: Identifying Potential Attrition Risks

As an HR professional, it's important to identify employees who might be at risk of leaving the company. Key indicators include **frequent absences** or **poor performance**.

To start, I analyzed the distribution of employee absences:

```python
# Distribution of absences (percentage)
df.Absences.value_counts(normalize=True) * 100
```

### 🔍 Step 8.6: Insights from Absence Analysis

The analysis revealed a key insight:

- **Highest Number of Leaves:** 20 days, taken by **4.5%** of employees.

### 🔸 Insight:
- This group of employees may require further attention to understand if the absences are due to personal issues, workplace dissatisfaction, or health concerns.
- Cross-referencing this with performance scores can help HR take a balanced approach—whether it's providing support, initiating conversations, or planning interventions.


### 💍 Step 8.7: Marital Status of Employees

Understanding employees' marital status can help HR plan inclusive and thoughtful engagement activities, such as celebrating **Children's Day**, **Parents' Day**, or **Work Anniversaries**.

To analyze this, I checked the distribution of marital status:

```python
# Count of married vs. unmarried employees
df['MarriedID'].value_counts()
```

🔸 Insight:
187 employees are married.
This insight can help HR personalize employee engagement initiatives, creating a more connected and supportive workplace culture.

### 🌟 Step 8.8: Employees Contributing to Special Projects

Employees involved in **Special Projects** often go beyond their regular responsibilities, indicating higher engagement and contribution.

To identify such employees:

```python
# Employees with special project contributions
df[df['SpecialProjectsCount'] != 0]
```

70 employees are actively contributing to special projects.
These employees can be recognized for their efforts and considered for leadership roles or rewards.


 ## 📊 Step 9: Visualization Analysis - Univariate

To better understand the distribution of individual features, I conducted **univariate analysis** using visualizations. This helps identify patterns, outliers, and the overall spread of data.

### 📊 Step 9.1: Stacked Bar Chart - Top 10 vs. Lowest 10 Salaries

To visually compare the **Top 10** and **Lowest 10** salaries, I used a **stacked bar chart**.

```python
# Stacked bar chart for salary comparison
c = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
y1 = df['Salary'].sort_values(ascending=False).head(10)
y2 = df['Salary'].sort_values(ascending=False).tail(10)

plt.figure(figsize=(12, 7))
plt.bar(c, y1, color='g', label='Highest Salaries')
plt.bar(c, y2, color='r', label='Lowest Salaries')
plt.title('Top 10 Salary vs Lowest 10 Salary', fontsize=14)
plt.xticks(c)
plt.ylabel('Salaries')
plt.legend()
plt.grid(False)
plt.show()

```
![download](https://github.com/user-attachments/assets/00be1bb0-5f08-4bea-a634-eaf836c8405d)

### 🔍 Insights:
1. **Lowest Salaries:**  
   - The **red bars** represent the lowest 10 salaries.  
   - These salaries are **closely clustered**, indicating **minimal variation** among the lowest-paid employees.

2. **Highest Salaries:**  
   - The **green bars** represent the highest 10 salaries.  
   - There's a **significant variation** among top earners, showing that **higher-level roles** have more **diverse pay structures**.
   

### 🔹 9.2: Recruitment Source Analysis
 

As an HR professional, it's crucial to understand **which recruitment sources** are most effective in hiring candidates.

---

#### 🔢 Unique Sources of Hiring
```python
df.RecruitmentSource.unique()
```

```
l = df.RecruitmentSource.value_counts().sort_values(ascending=False)
plt.barh(l.index, l.values, color='red')
plt.title("Source of Recruitment", fontsize=12)
plt.xlabel("Number of Candidates Hired")
plt.ylabel("Recruitment Source")
plt.show()

```

![download](https://github.com/user-attachments/assets/d49a37b2-27e1-4703-98ac-11f99d432409)



🔍 Insights:
Top Sources:

Indeed is the most common recruitment source, followed by LinkedIn and Google Search.
Low-Performing Sources:

Some sources contributed minimally to hiring, indicating a need for evaluation.

### 📊 9.3: Performance Trend Analysis

Understanding **employee performance categories** helps HR identify strengths and areas for improvement within the workforce.

---

#### 🔢 Performance Score Distribution
```python
df['PerformanceScore'].value_counts()

```
```
z = df['PerformanceScore'].value_counts()

plt.figure(figsize=(10,6))
sns.lineplot(data=z, marker='o', color='purple', linewidth=2)

plt.title("Performance Trend Analysis")
plt.xlabel("Performance Score")
plt.ylabel("Number of Employees")
plt.grid()
plt.show()

```

![download](https://github.com/user-attachments/assets/129bf13d-47e8-4575-8842-74d34387b5b3)


#insights - #insights>>generally trend is increasing if we go from PIP to fully meets this


### 📊 Step 12: Employee Satisfaction Analysis

**Employee satisfaction** is a key metric for HR to understand overall employee morale and engagement.

---

#### 🔢 Satisfaction Rating Distribution
```python
b = df.EmpSatisfaction.value_counts()

plt.stem(b.index, b)
plt.ylabel("No of Employees")
plt.xticks(b.index)
plt.xlabel("Rating Given")
plt.ylabel("Employee Satisfaction")
plt.show()
```

![download](https://github.com/user-attachments/assets/895ccf50-a417-49c9-ab37-3bf127807bc3)


🔍 Insights:
Rating Trend:

The most common rating is 3, indicating a neutral satisfaction level among employees.
Ratings 1 and 5 are less frequent, showing fewer employees are extremely dissatisfied or highly satisfied.
HR Focus:

HR can investigate why many employees are giving a mid-level rating and explore ways to boost satisfaction.


## Multivariate Analysis

**Outliers in Salary by Department:**  
Analyzing salary distribution across departments helps HR identify potential salary discrepancies and outliers.

---

### 📊 Boxplot: Salary Distribution by Department
```python
plt.figure(figsize = (15,8))
sns.boxplot(x = 'Department', y = 'Salary', data = df, palette = 'viridis')

plt.xlabel("Department")
plt.ylabel("Salary")
plt.xticks(rotation = 45)
plt.show()

```

![download](https://github.com/user-attachments/assets/2046eaee-e49c-4f13-ac34-6eb489f053dc)

Executives Paid Highest:

As expected, the Executive department leads in salary, reflecting their senior-level responsibilities.
Least Salary in Production:

The Production department has the lowest salary range, likely due to entry-level roles and standardized pay scales.
Outliers Detected:

Outliers in Engineering, IT, and Executive salaries suggest either high-performing employees, special projects, or discrepancies.



# for different positions plot the engagement survey

```
plt.figure(figsize =(10, 6))
sns.barplot(x = 'Position', y = 'EngagementSurvey', data = df, palette = 'viridis')
plt.xlabel('Position')
plt.ylabel('Engagement Survey')
plt.xticks(rotation = 80)
plt.title('Engagement Survey by Position')
plt.show()
```


![download](https://github.com/user-attachments/assets/7cddcb11-7d2f-4a6b-ad57-37e45a5055cd)


# marital status by gender

```

sns.countplot(x = 'MaritalDesc', hue = 'GenderID', data = df, palette = 'rainbow')
plt.show()

```

![download](https://github.com/user-attachments/assets/b81d8e5d-54c6-45fd-8a5e-6c69026a4f88)


#Insights >> Most of the males are single

# what is the avg enagement score for employees in each department

```
df.groupby('Department')['EngagementSurvey'].mean()
```

#insights >> Executive office has the highest engagement surveys


# How many employees have been terminated for each position

```

df[df['Termd'] == 1].groupby('Position')['Employee_Name'].count()
```



# how many employees have been terminated for each reason
```
df[df['Termd'] == 1].groupby('TermReason')['Employee_Name'].count()
```

#insight - Another Position and Unhappy and seeking more money are the primary reason


# What is the mdeian salary of male and female employees
df.columns
```
df.groupby('Sex').Salary.median()
```

# What is total absences and average engagement survey score for each department
```
df.groupby('Department').agg({'Absences': 'sum', 'EngagementSurvey' : 'mean'})
```

# what is the total number of special projects and average absences for emloyees in each gender category
```
df.groupby('Sex').agg({'Absences': 'mean', 'SpecialProjectsCount': 'sum'})

```


