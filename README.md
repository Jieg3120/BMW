# BMW
BMW’s global sales from 2010–2024
import pandas as pd
import plotly.express as px
import matplotlib.pyplot as plt

# === 1. Load the dataset ===
df= pd.read_csv('BMW sales data (2010-2024) (1).csv')

# Ensure Year is numeric
df['Year'] = df['Year'].astype(int)

# Group by year and model to get total sales
year_model_sales = df.groupby(['Year', 'Model'])['Sales_Volume'].sum().reset_index()

# Create animated bar chart with text labels
fig = px.bar(
    year_model_sales,
    x='Model',
    y='Sales_Volume',
    color='Model',
    animation_frame='Year',
    text='Sales_Volume',  # <-- display numbers
    title="BMW Model Sales Over Time (2010–2024)",
    labels={'Sales_Volume': 'Sales Volume', 'Model': 'BMW Model'},
    range_y=[0, year_model_sales['Sales_Volume'].max() * 1.2]  # extra space for labels
)

# Adjust text position and style
fig.update_traces(
    texttemplate='%{text:.0f}',  # show as integer
    textposition='outside'       # place numbers above bars
)

# Layout optimization
fig.update_layout(
    xaxis={'categoryorder': 'total descending'},
    showlegend=False,
    template='plotly_white',
    margin=dict(t=80, b=60)
)

# Show chart
fig.show()

# === 2. Annual sales trend ===
yearly_sales = df.groupby('Year')['Sales_Volume'].sum()
plt.figure(figsize=(8, 5))
plt.plot(yearly_sales.index, yearly_sales.values, marker='o', color='royalblue')
plt.title("BMW Total Sales Trend (2010–2024)")
plt.xlabel("Year")
plt.ylabel("Sales Volume")
plt.grid(True)
plt.tight_layout()
plt.show()

# === 3. Top 10 models by sales ===
model_sales = df.groupby('Model')['Sales_Volume'].sum().sort_values(ascending=False).head(10)
plt.figure(figsize=(8, 5))
model_sales.plot(kind='bar', color='teal')
plt.title("Top 10 Best-Selling BMW Models")
plt.xlabel("Model")
plt.ylabel("Total Sales Volume")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# === 4. Sales distribution by region ===
region_sales = df.groupby('Region')['Sales_Volume'].sum().sort_values(ascending=False)
plt.figure(figsize=(7, 7))
region_sales.plot(kind='pie', autopct='%1.1f%%', startangle=120, colors=plt.cm.Paired.colors)
plt.title("Sales Distribution by Region")
plt.ylabel("")
plt.tight_layout()
plt.show()

# === 5. Price vs. sales volume ===
plt.figure(figsize=(7, 5))
plt.scatter(df['Price_USD'], df['Sales_Volume'], alpha=0.6, color='darkorange')
plt.title("Price vs. Sales Volume")
plt.xlabel("Price (USD)")
plt.ylabel("Sales Volume")
plt.grid(True)
plt.tight_layout()
plt.show()

# === 6. Sales by fuel type ===
fuel_sales = df.groupby('Fuel_Type')['Sales_Volume'].sum()
plt.figure(figsize=(6, 4))
fuel_sales.plot(kind='bar', color='lightcoral')
plt.title("Sales by Fuel Type")
plt.xlabel("Fuel Type")
plt.ylabel("Sales Volume")
plt.tight_layout()
plt.show()

# === 7. Sales by transmission type ===
trans_sales = df.groupby('Transmission')['Sales_Volume'].sum()
plt.figure(figsize=(6, 4))
trans_sales.plot(kind='bar', color='seagreen')
plt.title("Sales by Transmission Type")
plt.xlabel("Transmission Type")
plt.ylabel("Sales Volume")
plt.tight_layout()
plt.show()

