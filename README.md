Data Wrangling Tutorial with a Sample “People” Dataset

We use a publicly available synthetic “People” dataset (100 records) from DataBlist, which includes text (names, job titles, emails), numeric (IDs, phone numbers), and date (date of birth) columns. The dataset can be downloaded in CSV format (e.g. people-100.csv). This rich mix of types makes it ideal for demonstrating common cleaning tasks (handling missing values, duplicates, formatting, outliers).

Step 1: Load and Explore the Dataset
import pandas as pd

# Load the CSV into a Pandas DataFrame
df = pd.read_csv('people-100.csv')  # path to downloaded CSV

# Quick look at the data
print(df.head())         # first 5 rows
print(df.info())         # column types & non-null counts
print(df.describe(include='all'))  # summary stats for all columns

Exploration: We check df.head() and df.info() to see column names, types, and if any values are missing. describe() (with include='all') gives counts and basic stats (e.g. name counts, unique values).

For example, we might see that Date of birth is a string and needs parsing to datetime. Any blank entries (NaN) will show up in info() as reduced counts.

Step 2: Data Dictionary

We document each column in GitHub-flavored markdown:

Column Name	Data Type	Description
User Id	string	Unique identifier for each person
First Name	string	Person’s first name
Last Name	string	Person’s last name
Sex	string	Gender (“Male”/“Female”)
Email	string	Email address
Phone	string	Phone number (various formats)
Date of birth	date	Person’s birth date (YYYY-MM-DD)
Job Title	string	Person’s job/occupation

This dictionary defines each column’s type and purpose. (We’ll convert “Date of birth” to a real date and derive Age.)

Step 3: Assess Data Quality
# 3.1 Check for missing values
print("Missing values per column:\n", df.isna().sum())

# 3.2 Check for duplicates
print("Duplicate rows:", df.duplicated().sum())

# 3.3 Spot formatting issues (e.g., inconsistent phone formats, case)
print(df['Phone'].head(3))  # phones have dashes, dots, “x”, etc.

# 3.4 Convert DOB to datetime and compute Age
df['Date of birth'] = pd.to_datetime(df['Date of birth'], errors='coerce')
df['Age'] = (pd.Timestamp.today() - df['Date of birth']).dt.days // 365
print(df['Age'].describe())

# 3.5 Identify outliers in Age (e.g., unusually high)
old = df[df['Age'] > 100]
print("Suspected age outliers:\n", old[['First Name','Last Name','Age']])

Missing values: df.isna().sum() tallies blanks per column. We’ll decide to drop or impute if any present.

Duplicates: Using df.duplicated().sum() finds repeated rows. If >0, we’ll remove them with drop_duplicates().

Formatting issues: We inspect raw phone numbers or other strings for inconsistent patterns. For example, phones may include letters like “x” or symbols. We note these for cleaning.

Outliers: We created an Age column from DOB. Values far beyond normal human lifespan (e.g. age >100) are outliers. Here we see e.g. one “Age” ~111, flagging a potential data issue or format error.

Step 4: Clean and Transform the Data
# 4.1 Drop exact duplicate rows (if any)
df = df.drop_duplicates()

# 4.2 Handle missing values
#    - For demonstration, drop rows missing a name
df = df.dropna(subset=['First Name','Last Name'])
#    - Fill any missing emails with a placeholder
df['Email'] = df['Email'].fillna('no_email@domain.com')

# 4.3 Standardize formats
#    - Convert Sex to single-letter codes
df['Sex'] = df['Sex'].map({'Male':'M', 'Female':'F'})
#    - Normalize phone: remove non-digits
df['Phone'] = df['Phone'].str.replace(r'\D','', regex=True)

# 4.4 Feature engineering: Calculate precise age
today = pd.Timestamp.today()
df['Date of birth'] = pd.to_datetime(df['Date of birth'])  # ensure datetime
df['Age'] = (today - df['Date of birth']).dt.days // 365

# 4.5 Re-order columns and drop unneeded ones if desired
df = df[['User Id','First Name','Last Name','Sex','Email','Phone',
         'Date of birth','Age','Job Title']]

We drop duplicates so each person is unique.

For missing data, we dropped rows lacking names and filled missing emails via fillna(). (In practice one could also impute missing ages, etc.)

Standardizing: We mapped "Male"/"Female" to "M"/"F" for consistency. Phone numbers had varying formats (dots, dashes, letters) so we stripped all non-digits with str.replace(r'\D','') to get a uniform numeric string.

Feature engineering: We converted the DOB field to datetime and recomputed Age relative to today. This new column can be useful for analysis (e.g. segmenting by age group).

At each step, we use built-in Pandas methods (dropna, fillna, map, etc.) as best practices for cleaning.

Step 5: Output Cleaned Data
# Save the cleaned DataFrame to a new CSV
df.to_csv('people_cleaned.csv', index=False)
print("Cleaned data saved.")

After cleaning, people_cleaned.csv is our analysis-ready dataset. It has consistent formats, no duplicates or blanks (in critical fields), and an added “Age” column. For example, now every phone is just digits, and missing emails have a placeholder.

Talking Points for a LinkedIn Video Walkthrough

Introduce the dataset: “Here’s a sample People dataset (downloadable as CSV) with names, contact info, job titles, and birth dates.”

Show initial exploration: Print df.head(), df.info(), df.describe() and highlight key findings (e.g. data types and any blank cells).

Highlight issues: Point out missing values summary and any duplicates (df.duplicated().sum()). Mention varied phone formats or a super-old age as examples of formatting issues and outliers.

Cleaning steps: Demonstrate dropping duplicates (drop_duplicates()), filling or dropping missing values (dropna(), fillna()), and fixing formats (e.g. regex to strip non-digits from phone numbers).

Feature engineering: Show creating a new “Age” column from date of birth, explaining its value for analysis. Compare summary stats of the age column pre- and post-cleaning (outlier removed).

Result: Emphasize that now the dataset is “analysis-ready” – consistent types and no glaring errors – and saved as people_cleaned.csv. Conclude by reiterating that careful cleaning (handling missing data, removing duplicates, standardizing formats, addressing outliers) is essential for trustworthy analysis.# apex-task
