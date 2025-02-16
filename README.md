# Project--Superstore-Business-Analysis

![3827467](https://github.com/user-attachments/assets/927bd9ec-a755-4603-8f12-5eccb32e5fbb)

## Introduction

**Context and Content**

With rising demand for furniture, office equipment, and household items, the retail supermarket has broadened their product offerings to cater to businesses, offices, schools, and households.

Beyond fulfilling shopping needs, The supermarket seek to analyze business data to enhance operations and grow their customer base. Key indicators to monitor include:

Sales and profits over the years to assess growth rates.
Best-selling products for optimizing import and promotion strategies.
Customer growth trends to identify shopping behaviors and expand market reach.
Data analysis will enable the supermarket's manager to make informed decisions, refine marketing strategies, and optimize product portfolios to better meet customer needs.

## Data 

Soure : Kaggle

Link dataset: https://www.kaggle.com/datasets/ishanshrivastava28/superstore-sales

Description: Superstore sales data, including information about orders, customers, products, regions, and profits. 
The data includes 9,994 records with 21 columns, including important information such as:

| Field Name       | Description             | Field Name       | Description             |
|-----------------|------------------------|-----------------|------------------------|
| **Order ID**    | Order information      | **Order Date**  | Date of order placement |
| **Ship Date**   | Shipping date          | **Ship Mode**   | Shipping method         |
| **Customer ID** | Customer identifier    | **Customer Name** | Customer's full name   |
| **Segment**     | Customer type          | **Country**     | Country                |
| **City**        | City                    | **State**       | State/Province         |
| **Region**      | Geographical region    | **Product ID**  | Product identifier     |
| **Product Name** | Product name          | **Category**    | Product category       |
| **Sub-Category** | Product sub-category  | **Sales**       | Revenue (after discount) |
| **Quantity**    | Number of products sold | **Discount**    | Discount applied       |
| **Profit**      | Profit earned           |                 |                        |


**Purpose**

Analyze business data to improve operations to increase revenue and expand market.

Key metrics to track include:

Year-over-year sales and profits to assess growth rates.

Best-selling products to optimize sales and promotional strategies.

Identify:

Customer growth trends to determine purchasing behavior and expand market.
Forecast revenue for the next year

To analyze the business data, I will pose the following five key questions:

1️⃣ **How have revenue and profit grown over time?**  

2️⃣ **Which products are the best-selling and most profitable?**  

3️⃣ **What are the shopping behaviors of customers?**  

4️⃣ **What is the projected revenue for next year based on current data?**  

5️⃣ **Are there opportunities to expand into new markets or product categories?**  

To answer these questions, I have taken the following approach:

**Import all the needed libraries**
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```
**Load the data**

Let's take a look at the loaded data.
Output 5 first rows:
```
df = pd.read_excel('/content/Superstore.xlsx')
df.head()
```
![image](https://github.com/user-attachments/assets/04963d75-7a46-4ecd-ab87-681b5a6f5951)

Let's take a look basic information about the dataset (columns, types, numbers of rows and not null values)

```
df.info()
```
![image](https://github.com/user-attachments/assets/09a7d0d8-69e8-4b37-972f-d7bed91ef897)

Let's take a look descriptive statistics

![image](https://github.com/user-attachments/assets/efaf07cf-c416-4a9f-9379-ba6b637acf07)

**Data Cleaning**

```
Check duplicated
df.duplicated()
```

```
# convert columns to string
columns_string = ['Customer_ID', 'Customer_Name','Postal_Code','Product_ID','Product_Name']
df[columns_string] = df[columns_string].astype('string')
```

```
#check again
df[columns_string ].dtypes
```

```
# check Ship_Mode, Segment,Country,City,State,Region,Category,Sub_Category
df['Ship_Mode'].unique()
df['Segment'].unique()
df['Country'].unique()
df['City'].unique()
df['State'].unique()
df['Region'].unique()
df['Category'].unique()
df['Sub_Category'].unique()
```

```
# convert columns to category
columns_category = ['Ship_Mode', 'Segment','Country','City','State','Region','Category','Sub_Category']
df[columns_category] = df[columns_category].astype('category')
```

```
# check again
df[columns_category].dtypes
```

```
# convert column date to datetime
df['Order_Date'] = pd.to_datetime(df['Order_Date'])
df['Order_Date'].head()
df['Ship_Date'] = pd.to_datetime(df['Ship_Date'])
df['Ship_Date'].head()
```
Check data infomation again

![image](https://github.com/user-attachments/assets/4ca721d6-8e48-469f-b063-d75ff5e11eab)

**Handling missing values**

```
# check null value
df.isnull().sum()
```

```
#Fill null value in column shipmode , country by  mode
df['Ship_Mode'] = df['Ship_Mode'].fillna(df['Ship_Mode'].mode()[0])
df['Country'] = df['Country'].fillna(df['Country'].mode()[0])
```

```
# Recheck data
df.info()
```

