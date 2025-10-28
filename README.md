# D599Task1
This repository contains my D599 Task 1.

# Import required libraries and load the CSV file using pandas
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
df = pd.read_csv('C:/Users/suman/Downloads/Employee Turnover Dataset.csv')

# Display the first few rows of the dataset
print(df.head())
df.shape

# Identify duplicate rows
duplicates = df[df.duplicated()] 
print("Duplicate rows:\n", duplicates)

# Identify missing values
missing_values = df.isnull().sum()
print("Missing values per column:", missing_values)

# Identify inconsistent values
print("PaycheckMethod unique values:", df["PaycheckMethod"].unique())
print("CompensationType unique values:", df["CompensationType"].unique())
print("Gender unique values:", df["Gender"].unique()) 
print("MaritalStatus unique values:", df["MaritalStatus"].unique())

# Identify formatting errors
print(df.dtypes)
print(df.head())

# Identify negative values
print("Negative salaries:\n", df[df["AnnualSalary"] < 0])
print("Negative commute distances:\n", df[df["DrivingCommuterDistance"] < 0]) 

# Identify columns with potential outliers
print(df.min(numeric_only=True))
print(df.max(numeric_only=True))

# Identify outliers
Q1 = df["AnnualSalary"].quantile(0.25)
print("Q1:", Q1)
Q3 = df["AnnualSalary"].quantile(0.75)
print("Q3:", Q3)
iqr = Q3 - Q1
print("IQR:", iqr)
lower_limit = Q1-1.5*iqr
print("lower limit:", lower_limit)
upper_limit = Q3+1.5*iqr
print("upper limit:", upper_limit)
outliers = df[(df["AnnualSalary"] < lower_limit) | (df["AnnualSalary"] > upper_limit)]
print("outliers:", outliers)  

# Create boxplot
sns.boxplot(x="AnnualSalary", data=df)
plt.title("Distribution of Salary")
plt.show()

# Removed duplicate rows
df = df.drop_duplicates()
print("Duplicates after removal:", df.duplicated().sum())

# Handle missing values
df.loc[:, "NumCompaniesPreviouslyWorked"] = df["NumCompaniesPreviouslyWorked"].fillna(df["NumCompaniesPreviouslyWorked"].median())
df.loc[:, "AnnualProfessionalDevHrs"] = df["AnnualProfessionalDevHrs"].fillna(df["AnnualProfessionalDevHrs"].median())
df.loc[:, "TextMessageOptIn"] = df["TextMessageOptIn"].fillna(df["TextMessageOptIn"].mode()[0])

# Correct inconsistent values
df.loc[:,'PaycheckMethod'] = df['PaycheckMethod'].replace({
    'Mail Check': 'Mailed Check',
    'Mail_Check': 'Mailed Check',
    'MailedCheck': 'Mailed Check',
    'Direct_Deposit': 'Direct Deposit',
    'DirectDeposit': 'Direct Deposit'})

# Correct formatting errors and negative values
df.loc[:,'HourlyRate '] = df['HourlyRate '].str.replace('$', '', regex=False).astype(float)
df.loc[:,'AnnualSalary'] = df['AnnualSalary'].abs() 
df.loc[:,'DrivingCommuterDistance'] = df['DrivingCommuterDistance'].abs()

# Confirm that outliers don't need to be removed
print(outliers[['AnnualSalary','Age','Tenure']])

# create another boxplot
print(outliers[['AnnualSalary','Age','Tenure']])
Q1 = df["AnnualSalary"].quantile(0.25)
print("Q1:", Q1)
Q3 = df["AnnualSalary"].quantile(0.75)
print("Q3:", Q3)
iqr = Q3 - Q1
print("IQR:", iqr)
lower_limit = Q1-1.5*iqr
print("lower limit:", lower_limit)
upper_limit = Q3+1.5*iqr
print("upper limit:", upper_limit)
outliers = df[(df["AnnualSalary"] < lower_limit) | (df["AnnualSalary"] > upper_limit)]
print("outliers:", outliers)

# Create boxplot again to make sure negative values were handled
sns.boxplot(x="AnnualSalary", data=df)
plt.title("Distribution of Salary")
plt.show()

# Export the cleaned dataset to a new CSV file
df.to_csv('C:/Users/suman/Downloads/Employee Turnover Cleaned Dataset.csv', index=False)
