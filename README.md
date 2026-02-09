# exploratory-analysis-of-rain-fall-data-in-india-for-agriculture

""" Exploratory Data Analysis (EDA) of Rainfall Data in India for Agriculture

Author: Nagaraju Vancheti Description: This script performs an exploratory data analysis on Indian rainfall data with an agricultural perspective. It covers data loading, cleaning, descriptive statistics, seasonal analysis, state-wise trends, and visualizations suitable for a GitHub project.

Dataset Assumption:

CSV file containing rainfall data

Typical columns may include: ['STATE', 'YEAR', 'JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC', 'ANNUAL']


You can adapt column names based on your dataset. """

=========================

1. Import Libraries

=========================

import pandas as pd import numpy as np import matplotlib.pyplot as plt import seaborn as sns

Set plot style

sns.set(style="whitegrid")

=========================

2. Load Dataset

=========================

Update the file path as needed

file_path = "rainfall_india.csv"

df = pd.read_csv(file_path)

print("Dataset loaded successfully!\n") print(df.head())

=========================

3. Basic Data Understanding

=========================n

print("\nDataset Info:\n") print(df.info())

print("\nStatistical Summary:\n") print(df.describe())

=========================

4. Data Cleaning

=========================

Check missing values

print("\nMissing Values:\n") print(df.isnull().sum())

Fill missing rainfall values with column mean (if any)

rainfall_columns = df.select_dtypes(include=[np.number]).columns

df[rainfall_columns] = df[rainfall_columns].fillna(df[rainfall_columns].mean())

=========================

5. Annual Rainfall Analysis

=========================

plt.figure(figsize=(10, 5)) plt.plot(df.groupby('YEAR')['ANNUAL'].mean()) plt.title("Average Annual Rainfall in India Over the Years") plt.xlabel("Year") plt.ylabel("Rainfall (mm)") plt.show()

=========================

6. Seasonal Rainfall Analysis

=========================

Define seasons

seasons = { 'Winter': ['JAN', 'FEB'], 'Pre-Monsoon': ['MAR', 'APR', 'MAY'], 'Monsoon': ['JUN', 'JUL', 'AUG', 'SEP'], 'Post-Monsoon': ['OCT', 'NOV', 'DEC'] }

seasonal_rainfall = {}

for season, months in seasons.items(): seasonal_rainfall[season] = df[months].sum(axis=1).mean()

plt.figure(figsize=(8, 5)) plt.bar(seasonal_rainfall.keys(), seasonal_rainfall.values()) plt.title("Average Seasonal Rainfall in India") plt.xlabel("Season") plt.ylabel("Rainfall (mm)") plt.show()

=========================

7. State-wise Rainfall Analysis

=========================

state_rainfall = df.groupby('STATE')['ANNUAL'].mean().sort_values(ascending=False)

plt.figure(figsize=(12, 6)) state_rainfall.head(10).plot(kind='bar') plt.title("Top 10 States by Average Annual Rainfall") plt.xlabel("State") plt.ylabel("Rainfall (mm)") plt.show()

=========================

8. Monsoon Rainfall Trend (Agricultural Importance)

=========================

monsoon_months = ['JUN', 'JUL', 'AUG', 'SEP'] df['MONSOON_RAINFALL'] = df[monsoon_months].sum(axis=1)

plt.figure(figsize=(10, 5)) plt.plot(df.groupby('YEAR')['MONSOON_RAINFALL'].mean()) plt.title("Average Monsoon Rainfall Trend in India") plt.xlabel("Year") plt.ylabel("Rainfall (mm)") plt.show()

=========================

9. Correlation Analysis

=========================

plt.figure(figsize=(10, 6)) sns.heatmap(df[rainfall_columns].corr(), cmap='coolwarm', annot=False) plt.title("Correlation Heatmap of Rainfall Variables") plt.show()

=========================

10. Key Insights

=========================

print("\nKey Insights:") print("- Monsoon months contribute the majority of annual rainfall.") print("- Significant variation exists across states, affecting crop planning.") print("- Long-term trends help identify drought-prone or flood-prone periods.")

print("\nEDA completed successfully.")
