!pip install pandas matplotlib sqlalchemy
import pandas as pd
import numpy as np

# Simulate IT audit log data
data = {
    'System': ['System_A', 'System_B', 'System_C', 'System_D', 'System_E'],
    'Control_Implemented': [True, False, True, False, True],
    'Last_Audit_Date': ['2023-12-01', '2023-11-15', '2023-12-05', '2023-10-20', '2023-11-10'],
    'Control_Gap': [0, 1, 0, 1, 0],
    'Risk_Score': [8, 3, 6, 4, 7]  # Lower score indicates higher risk
}

df = pd.DataFrame(data)

# Convert date columns to datetime type
df['Last_Audit_Date'] = pd.to_datetime(df['Last_Audit_Date'])

df.head()
# Function to assess risk based on control gaps and risk score
def assess_risk(row):
    if row['Control_Gap'] == 1:
        return 'High Risk'
    elif row['Risk_Score'] >= 7:
        return 'Moderate Risk'
    else:
        return 'Low Risk'

# Apply risk assessment
df['Risk_Level'] = df.apply(assess_risk, axis=1)

df[['System', 'Control_Implemented', 'Risk_Level', 'Last_Audit_Date']]
import matplotlib.pyplot as plt

# Plotting the Risk Levels
plt.figure(figsize=(8,6))
risk_count = df['Risk_Level'].value_counts()
plt.bar(risk_count.index, risk_count.values, color=['red', 'orange', 'green'])
plt.xlabel('Risk Level')
plt.ylabel('Number of Systems')
plt.title('Risk Distribution of IT Systems')
plt.show()


# Filter high-risk systems
high_risk_systems = df[df['Risk_Level'] == 'High Risk']

# Report generation
report = f"High-Risk Systems Report\n{'-'*30}\n"
for index, row in high_risk_systems.iterrows():
    report += f"System: {row['System']}\n"
    report += f"Last Audit: {row['Last_Audit_Date'].strftime('%Y-%m-%d')}\n"
    report += f"Recommendation: Improve controls immediately\n\n"

# Display report
print(report)
df.to_csv('audit_risk_report.csv', index=False)
