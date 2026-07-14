# ==========================================
# Data Cleaning & Reporting Automation
# ==========================================

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# ------------------------------------------
# Step 1: Create Sample Dataset
# ------------------------------------------

data = {
    "Employee_ID": [101, 102, 103, 104, 105, 105],
    "Name": ["John", "Alice", "Bob", np.nan, "David", "David"],
    "Department": ["HR", "IT", "Finance", "IT", "HR", "HR"],
    "Salary": [50000, 60000, np.nan, 55000, 65000, 65000],
    "City": ["Chennai", "Bangalore", "Mumbai", "Chennai", None, None]
}

df = pd.DataFrame(data)

print("\nOriginal Dataset:")
print(df)

# ------------------------------------------
# Step 2: Data Cleaning
# ------------------------------------------

# Remove duplicate rows
df = df.drop_duplicates()

# Fill missing names
df["Name"] = df["Name"].fillna("Unknown")

# Fill missing salary with average salary
df["Salary"] = df["Salary"].fillna(df["Salary"].mean())

# Fill missing city with 'Unknown'
df["City"] = df["City"].fillna("Unknown")

# Remove extra spaces and convert text to title case
text_columns = ["Name", "Department", "City"]

for col in text_columns:
    df[col] = df[col].astype(str).str.strip().str.title()

print("\nCleaned Dataset:")
print(df)

# ------------------------------------------
# Step 3: Save Cleaned Data
# ------------------------------------------

df.to_csv("Cleaned_Data.csv", index=False)

# ------------------------------------------
# Step 4: Generate Summary Report
# ------------------------------------------

report = {
    "Total Rows": len(df),
    "Total Columns": len(df.columns),
    "Missing Values": df.isnull().sum().sum(),
    "Duplicate Rows": df.duplicated().sum(),
    "Average Salary": df["Salary"].mean(),
    "Maximum Salary": df["Salary"].max(),
    "Minimum Salary": df["Salary"].min()
}

report_df = pd.DataFrame(report.items(), columns=["Metric", "Value"])

print("\nSummary Report:")
print(report_df)

report_df.to_csv("Summary_Report.csv", index=False)

# ------------------------------------------
# Step 5: Department-wise Employee Count
# ------------------------------------------

dept_count = df["Department"].value_counts()

plt.figure(figsize=(8,5))
dept_count.plot(kind="bar")
plt.title("Department-wise Employee Count")
plt.xlabel("Department")
plt.ylabel("Number of Employees")
plt.xticks(rotation=0)
plt.tight_layout()
plt.savefig("Department_Report.png")
plt.show()

# ------------------------------------------
# Step 6: Salary Distribution
# ------------------------------------------

plt.figure(figsize=(8,5))
plt.hist(df["Salary"], bins=5)
plt.title("Salary Distribution")
plt.xlabel("Salary")
plt.ylabel("Frequency")
plt.tight_layout()
plt.savefig("Salary_Distribution.png")
plt.show()

print("\nAutomation Completed Successfully!")
print("Files Generated:")
print("1. Cleaned_Data.csv")
print("2. Summary_Report.csv")
print("3. Department_Report.png")
print("4. Salary_Distribution.png")
