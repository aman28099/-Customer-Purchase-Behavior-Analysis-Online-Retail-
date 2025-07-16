# 🛠️ Step 1: Import Required Libraries
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

# 🧹 Step 2: Load & Clean the Data
df = pd.read_csv('online_retail_dataset.csv')  # Replace with your file name
print("Initial shape:", df.shape)
df.dropna(subset=['CustomerID'], inplace=True)
df.drop_duplicates(inplace=True)
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])

# 💡 Step 3: Feature Engineering
df['TotalPrice'] = df['Quantity'] * df['UnitPrice']
df['InvoiceMonth'] = df['InvoiceDate'].dt.to_period('M')

# 📊 Step 4: Key KPIs
total_sales = df['TotalPrice'].sum()
unique_customers = df['CustomerID'].nunique()
top_countries = df['Country'].value_counts().head(5)

print("Total Revenue:", total_sales)
print("Unique Customers:", unique_customers)
print("Top 5 Countries:\n", top_countries)

# 📌 Step 5: Top Products
top_products = df.groupby('Description')['TotalPrice'].sum().sort_values(ascending=False).head(10)
plt.figure(figsize=(10,5))
sns.barplot(x=top_products.values, y=top_products.index)
plt.title('Top 10 Products by Revenue')
plt.xlabel('Revenue')
plt.ylabel('Product')
plt.tight_layout()
plt.show()

# 🌍 Step 6: Country-wise Sales
country_sales = df.groupby('Country')['TotalPrice'].sum().sort_values(ascending=False).head(10)
plt.figure(figsize=(10,5))
sns.barplot(x=country_sales.values, y=country_sales.index, palette="coolwarm")
plt.title('Top 10 Countries by Sales')
plt.xlabel('Sales')
plt.ylabel('Country')
plt.tight_layout()
plt.show()

# 📈 Step 7: Monthly Revenue Trend
monthly_revenue = df.groupby('InvoiceMonth')['TotalPrice'].sum()
monthly_revenue.plot(kind='line', marker='o', figsize=(12,6), title='Monthly Revenue Trend')
plt.ylabel('Revenue')
plt.xlabel('Month')
plt.grid(True)
plt.show()

# 💼 Step 8: Customer Purchase Frequency
customer_freq = df['CustomerID'].value_counts().head(10)
plt.figure(figsize=(10,5))
sns.barplot(x=customer_freq.index.astype(str), y=customer_freq.values)
plt.title('Top 10 Active Customers')
plt.xlabel('Customer ID')
plt.ylabel('Purchase Frequency')
plt.tight_layout()
plt.show()

# 📁 Step 9: Save Cleaned Data
df.to_csv("cleaned_customer_purchase.csv", index=False)

# 📝 Step 10: Export Report (Optional)
summary = {
    "Total Revenue": total_sales,
    "Unique Customers": unique_customers,
    "Top Product": top_products.index[0],
    "Top Country": country_sales.index[0]
}
summary_df = pd.DataFrame(summary.items(), columns=["Metric", "Value"])
summary_df.to_csv("project_summary.csv", index=False)
import seaborn as sns
import matplotlib.pyplot as plt

sns.barplot(x=top_products.values, y=top_products.index)
plt.title('Top 10 Products by Revenue')
plt.show()
