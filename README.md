📊 Data Immersion & Wrangling Project
📌 Project Overview

This project focuses on data cleaning, transformation, and preparation using Python (Pandas).

The objective was to:

Understand the dataset

Identify data quality issues

Clean and transform the data

Generate meaningful insights

Create an analysis-ready dataset

📂 Dataset Information

Dataset Used: People Dataset (CSV format)
Records: 100
Features Include:

User Id

First Name

Last Name

Gender

Email

Phone

Date of Birth

Job Title

The dataset contains mixed data types such as text, numeric, and date fields, making it ideal for practicing data wrangling techniques.

🛠️ Tools & Technologies

Python

Pandas

NumPy

Matplotlib

Jupyter Notebook

🔎 Step 1: Data Understanding

Loaded dataset using Pandas

Explored structure using:

head()

info()

describe()

Created a Data Dictionary documenting each column

⚠️ Step 2: Data Quality Assessment

Identified the following issues:

Missing values

Duplicate records

Inconsistent phone number formats

Date column stored as string

Potential outliers in Age

🧹 Step 3: Data Cleaning & Transformation

Performed:

✔ Removed duplicate rows
✔ Handled missing values using dropna() and fillna()
✔ Standardized phone number format
✔ Converted Date of Birth to datetime
✔ Created new feature: Age
✔ Reordered columns

Example:

df['Date of birth'] = pd.to_datetime(df['Date of birth'])
df['Age'] = (pd.Timestamp.today() - df['Date of birth']).dt.days // 365
📊 Step 4: Exploratory Data Analysis (EDA)

Age distribution analysis

Gender distribution

Top job roles

Basic statistical summary

Visualization example:

df['Sex'].value_counts().plot(kind='bar')
📁 Final Output

Cleaned dataset exported as:

people_cleaned.csv

Data is now:

Structured

Consistent

Analysis-ready

📈 Key Insights

Most common job roles identified

Age distribution trends observed

Gender distribution analyzed

Outliers detected and reviewed

🚀 Project Outcome

This project demonstrates:

Practical data wrangling skills

Data quality assessment

Feature engineering

Exploratory data analysis

Clean coding practices

🎥 LinkedIn Walkthrough

A 3–5 minute video explaining:

Dataset overview

Issues identified

Cleaning process

Insights generated

🏷️ Skills Demonstrated

Data Cleaning

Data Transformation

Feature Engineering

Exploratory Data Analysis

Python Programming

Pandas

📌 How to Run This Project
git clone <your-repository-link>
cd <project-folder>
pip install -r requirements.txt
jupyter notebook
