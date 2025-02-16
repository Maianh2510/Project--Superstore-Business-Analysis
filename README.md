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
![image](https://github.com/user-attachments/assets/e229e297-c2be-42b2-85ac-b55be36c989e)

**Handling outliers**

Check Outliers

```
plt.figure(figsize=(8,5))
sns.boxplot(data=df[['Sales']])
plt.title("Boxplot for Sales ")
plt.show()
```

```
plt.figure(figsize=(8,5))
sns.boxplot(data=df[['Profit']])
plt.title("Boxplot for Profit ")
plt.show()
````

![image](https://github.com/user-attachments/assets/33989ecf-dd71-44c2-ad70-3f3ed74aa0c4)

**Comment**
Data Partitioning:
With picture show scattered round points above and below the box. It mean both Sales and Profit variables have many outliers.

Remove outliers

```
#Remove Outliers
def rm_outliner(col_name):
    Q1 = df[col_name].quantile(0.25)
    Q3 = df[col_name].quantile(0.75)
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    df_result = df[(df[col_name] >= lower_bound) & (df[col_name] <= upper_bound)]
    return df_result
```

```
df = rm_outliner(col_name = 'Sales')
df = rm_outliner(col_name = 'Profit')
```

```
#Recheck
df.describe()
```

**Correclations**

```
numberical_columns =  ['Sales','Quantity','Discount','Profit']
numberical_columns
```

```
coreclation_matrix = df[numberical_columns].corr()
coreclation_matrix
```

```
sns.heatmap(coreclation_matrix,annot=True, cmap='coolwarm')
plt.title('sales correlation matrix')
plt.show()
```

![image](https://github.com/user-attachments/assets/9c21f95d-7f22-42cd-a612-cfec705374cd)

## Comments

**Sales & Profit (0.4)**: Sales and Profit are positively correlated (0.4), meaning that when sales increase, profits also tend to increase, but the correlation is not very strong.

**Sales & Quantity (0.11**): Sales and sales quantity are very weakly correlated (0.11), indicating that selling more products does not necessarily increase sales significantly.

**Sales & Discount (-0.089)**: There is almost no significant correlation between sales and discounts, indicating that discounts do not have much impact on sales.

**Quantity & Profit (0.18)**: Sales quantity and profits are weakly positively correlated, meaning that selling more products can increase profits, but not significantly.

**Discount & Profit (-0.48)**: There is a clear negative correlation (-0.48), meaning that when prices are reduced, profits tend to decrease, which makes sense because discounts directly affect profit margins.

**Discount & Quantity (0.011)**: There is almost no correlation between discounts and sales volume, which may be because customers do not respond strongly to discount programs.

## Conclusion:
*  Discounts do not have a positive impact on revenue but instead reduce profits.

*   Increasing sales volume does not mean a strong increase in revenue.
*   Discount policies need to be considered more carefully to avoid negative impacts on profits.

```
#Export the Csv file for visualization in Power BI
df.to_csv("sales.csv", index=False)
```

Link colab here : https://colab.research.google.com/drive/1ueZE6A30ANo2Q2l9_uoThWB3-3CiZS05?usp=sharing

You can see all code in there . And I have attached the code guide in this post.

Then I used Power Bi to calculate the numbers using DAX and draw a visual chart. 
Here is the DAX code snippet I used :

```
Đơn hàng trung bình
Average_Oder_Value = DIVIDE([Total_Sales], [Total_Oder], 0)

Biên lợi nhuận trung bình
Average_Profit_Margin = 
VAR TotalProfit = SUM(sales[Profit])
VAR TotalSales = SUM(sales[Sales])
RETURN DIVIDE(TotalProfit, TotalSales, 0)

Doanh thu trung bình theo ngày
Avg_Daily_Sales = 
VAR TotalSales = SUM('sales'[Sales])
VAR TotalDays = DISTINCTCOUNT('sales'[Order_Date])
RETURN TotalSales / TotalDays

Tốc độ tăng trưởng bình quân
CAGR_Sales = 
VAR StartYear = MIN('Order_Dates'[Year]) 
VAR EndYear = MAX('Order_Dates'[Year]) 
VAR StartSales = CALCULATE(SUM('sales'[Sales]), 'Order_Dates'[Year] = StartYear)
VAR EndSales = CALCULATE(SUM('sales'[Sales]), 'Order_Dates'[Year] = EndYear)
VAR NumYears = EndYear - StartYear
RETURN 
IF(NumYears > 0, 
    (EndSales / StartSales) ^ (1 / NumYears) - 1, 
    BLANK()
)

Số khách hàng mua lại
Customers_MoreThanOnce = 
VAR RepeatCustomers =
    FILTER(
        VALUES(sales[Customer_ID]),
        CALCULATE(COUNT(sales[Order_ID])) > 1
    )
RETURN COUNTROWS(RepeatCustomers)


Tỷ lệ biên lợi nhuận
Profit_Margin = DIVIDE(SUM(sales[Profit]),SUM(sales[Net Sales]),0)*100

Tỷ lệ khách hàng quay lại
Repeat_customers = [Customers_MoreThanOnce]/[Total_customer]

Tổng chi phí giá vốn
Total_COGS = SUMX(
    sales, 
    (sales[Net Sales] - sales[Profit]
))

Tổng số khách hàng
Total_customer = DISTINCTCOUNT(sales[Customer_ID])

Tổng số đơn hàng
Total_Oder = DISTINCTCOUNT(sales[Order_ID])

Tổng số lợi nhuận
Total_Profit = SUM(sales[Profit])

Tổng doanh thu
Total_Sales = SUM(sales[Sales])

Tạo các bảng :

Calendar = CALENDARAUTO()

Category_SubCategory = SUMMARIZE(
    sales,
    sales[Category],
    sales[Sub_Category]
)

City_Table = SUMMARIZE(
    sales,
    sales[City],
    sales[State],
    sales[Region]
)

Customer_Table = SUMMARIZE(
    sales,
    sales[Customer_ID],
    sales[Customer_Name],
    sales[Segment],
    sales[Region]
)

Order_Dates = 
ADDCOLUMNS(
    DISTINCT(SELECTCOLUMNS('Sales', "Order Date", 'sales'[Order_Date])),
    "Year", YEAR([Order Date]),
    "Month", MONTH([Order Date]),
    "Month Name", FORMAT([Order Date], "MMMM"),
    "Day", DAY([Order Date]),
     "Weekday", FORMAT([Order Date], "ddd")
)
Orders_Summary = SUMMARIZE(
    sales,
    sales[Order_ID],
    sales[Order_Date],
    sales[Customer_ID],
    sales[Customer_Name],
    sales[Product_ID],
    sales[Product_Name],
    sales[Category],
    sales[Sub_Category],
    sales[City],
    sales[State],
    sales[Region],
    sales[Ship_Mode]
)

Product_Table = SUMMARIZE(
    sales,
    sales[Product_ID],
    sales[Product_Name],
    sales[Category],
    sales[Sub_Category]
)

Region_Table = DISTINCT(sales[Region])

Segment_Table = DISTINCT(sales[Segment])

ShipMode_Table = DISTINCT(sales[Ship_Mode])
```

Data Modeling

![image](https://github.com/user-attachments/assets/b9180741-f40b-43d7-a10e-547ae4f108f9)









