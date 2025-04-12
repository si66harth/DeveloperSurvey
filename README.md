# DeveloperSurvey
The software development industry continues to evolve rapidly, shaping economies and innovation worldwide. Understanding the composition and preferences of developers is critical for organizations aiming to attract talent and align with market demands
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
df = pd.read_csv('C:/Users/SIDDHARTH/Downloads/survey_results_public.csv')

# === Data Cleaning ===

# Strip column names
df.columns = df.columns.str.strip()

# Replace blank strings with NaN
df.replace(r'^\s*$', np.nan, regex=True, inplace=True)

# Drop columns where all values are NaN
df.dropna(axis=1, how='all', inplace=True)

# Drop rows with more than 70% missing values
threshold = len(df.columns) * 0.7
df.dropna(thresh=threshold, inplace=True)

# Reset index
df.reset_index(drop=True, inplace=True)

# Convert salary to numeric for plotting
df['ConvertedCompYearly'] = pd.to_numeric(df['ConvertedCompYearly'], errors='coerce')

# Convert Age to numeric (fixing the previous error)
df['Age'] = pd.to_numeric(df['Age'], errors='coerce')

# === Visualizations ===

# 1. Bar Chart – Top 10 Countries with Most Responses
plt.figure(figsize=(10, 6))
df['Country'].value_counts().head(10).plot(kind='bar', color='skyblue')
plt.title('Top 10 Respondent Countries')
plt.xlabel('Country')
plt.ylabel('Number of Respondents')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# 2. Pie Chart – Distribution of Top 5 Developer Types
dev_types = df['DevType'].dropna().str.split(';')
all_dev_types = pd.Series([item.strip() for sublist in dev_types for item in sublist])
top_dev_types = all_dev_types.value_counts().head(5)

plt.figure(figsize=(7, 7))
top_dev_types.plot.pie(autopct='%1.1f%%', startangle=140, colors=sns.color_palette("Set2"))
plt.title('Top 5 Developer Types')
plt.ylabel('')
plt.tight_layout()
plt.show()

# 3. Histogram – Age Distribution
plt.figure(figsize=(8, 5))
df['Age'].dropna().plot.hist(bins=30, color='orchid', edgecolor='black')
plt.title('Age Distribution of Respondents')
plt.xlabel('Age')
plt.ylabel('Frequency')
plt.grid(True)
plt.tight_layout()
plt.show()

# 4. Box Plot – Salary Distribution by Education Level
plt.figure(figsize=(12, 6))
sns.boxplot(data=df, x='EdLevel', y='ConvertedCompYearly')
plt.xticks(rotation=90)
plt.title('Yearly Compensation by Education Level')
plt.xlabel('Education Level')
plt.ylabel('Compensation (USD)')
plt.tight_layout()
plt.show()

# 5. Count Plot – Top 10 Programming Languages
lang_series = df['LanguageHaveWorkedWith'].dropna().str.split(';')
all_langs = pd.Series([lang.strip() for sublist in lang_series for lang in sublist])
top_langs = all_langs.value_counts().head(10)

plt.figure(figsize=(10, 6))
sns.barplot(x=top_langs.values, y=top_langs.index, palette='viridis')
plt.title('Top 10 Programming Languages Used')
plt.xlabel('Number of Mentions')
plt.ylabel('Programming Language')
plt.tight_layout()
plt.show()



# Ensure Age is numeric and drop NaN
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Ensure numeric salary
df['ConvertedCompYearly'] = pd.to_numeric(df['ConvertedCompYearly'], errors='coerce')

# Drop rows with missing values in relevant columns
remote_df = df[['RemoteWork', 'ConvertedCompYearly']].dropna()

# Optional: Filter out extreme salaries
remote_df = remote_df[remote_df['ConvertedCompYearly'] < 300000]  # Filter for better visualization

# Create the box plot
plt.figure(figsize=(10, 6))
sns.boxplot(data=remote_df, x='RemoteWork', y='ConvertedCompYearly', palette='coolwarm')
plt.title('Yearly Compensation by Remote Work Status')
plt.xlabel('Remote Work')
plt.ylabel('Compensation (USD)')
plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()
