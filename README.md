# Exploratory Analysis of Super Store Sales Data Using Python


## About the Project
In this project, we will carry out exploratory analysis of sales data.
We are provided with a dataset containing sales details and we are required to carry out the following tasks on the data

1. **Sales trend analysis** in which we will carry out an analysis of sales over different time period based on the date column

2. **Ship mode analysis:** We will compare the different ship-modes in the dataset to determine which of the shipping modes is commonly used by the customers

3. **Geographical Analysis:** Here, we will carry out EDA on sales based on the locations of the customer as provided in the data

4. **Product Analysis:** Here, we will dig into the performance of each product category and sub-categories. We will aim to find the best-performing products and the least-performing ones.

5. **Customer Analysis:** Here we will attempt to segment the customers and analyze their buying behavior to understand which of the segments contributes more to the overall sales performance. We will also carry out an analysis of customer loyalty by examining the purchase behavior of customers based on their frequency of orders.


## Tools
We will be carrying out the analysis using the Jupyter Notebook editor. The libraries used for this project are Pandas, NumPy, and Matplotlib.
- Pandas: For reading in  CSV file. We also used Pandas for data transformation and data manipulation.
- Numpy: Used in some Mathematical operations necessary to extract insight from the data
- Matplotlib: Used for data visualization

## Steps Taken

#### Import necessary libraries
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

#### Import in the data
```
data = pd.read_csv(r"C:\Users\tajud\Desktop\Super store.csv")
```

#### General overview of the data
```
data.info()
```

#### Numerical overview of the data
```
data.describe()
```

#### Data cleaning
- Check for duplicates
```
data[data[["Order ID","Customer ID","Product ID"]].duplicated()]
```
- How many duplicates entries do we have
```
data[data[["Order ID","Customer ID","Product ID"]].duplicated()].shape
```

- Remove duplicates from the data
```
  data = data[~(data[["Order ID","Customer ID","Product ID"]].duplicated())]
```
Now that the data is without duplicates, we can proceed to carry out the required EDA.

### Customer Analysis
##### Customer Segmentation: We check through our data to see the unique set of customers segment we have

  ```
  customer_segments = data["Segment"].unique()
  print(customer_segments)
  ```
- Next, we proceed to get the counts of these segments. That is how many of our customers are in this segments
```
segment_count = data["Segment"].value_counts().reset_index()

segment_count = segment_count.rename(columns = {"index": "Customer segment","Segment":"Total Count"})
print(segment_count)
```

- Next, we proceed to Visualize the above segmentation
  ```
  plt.pie(segment_count["Total Count"], 
  labels = segment_count["Customer segment"],autopct="%1.1f%%")
  plt.title("Customer Segments")
  plt.show()
  ``` 
- Next is to analyze the sales per customer segments
   ```
  sales_per_segments = data.groupby("Segment")["Sales"].sum().reset_index()
  sales_per_segments = sales_per_segments.rename(columns = {"Segments":"Customer Segment","Sales":"Total Sales"})
  
  print(sales_per_segments)
    ```
-  Visualizing the above result
   ```
    plt.pie(sales_per_segments["Total Sales"], labels = sales_per_segments["Segment"],autopct="%1.1f%%")
    plt.title("Sales Per Customer Segment")
    plt.show()
   ```
 ##### Customer Loyalty: Here, we are going to explore customer loyalty based on their repeat purchases. We will group the data according to Customer ID, Customer Name, and Segment, and get the frequency of their orders

```
Order_freq = data.groupby(["Customer ID","Customer Name","Segment"])
["Order ID"].count().reset_index()

Order_freq = Order_freq.rename(columns={"Order ID":"No. of Orders"})

print(Order_freq)
```

### Shipping Analysis
- Here, we will carry out EDA on the shipping modes used by the customers anytime they make orders.

```
shipping_modes = data["Ship Mode"].unique()

print(shipping_modes)
```

- Next, we proceed to see which shipping mode is most often used by customers
```
mode_freq = data["Ship Mode"].value_counts().reset_index()
mode_freq = mode_freq.rename
(columns = {"index":"Shipping Mode", "Ship Mode":"Freq"})

print(mode_freq)
```

- Visualizing the result
 ```
plt.bar(mode_freq["Shipping Mode"],mode_freq["Freq"])
plt.title("Ship Modes Freq")

plt.show()
```

### Geographical Analysis of Customers and Sales
- Next, we proceed to see the number of countries we have in our dataset and the number of customers we have in each of the countries.

 ```
countries = data["Country"].value_counts().reset_index()
countries = countries.rename(columns = {"index":"Country","Country":"No. of Customers"})

countries
```
- Next, we proceed to see the sales the superstore has made from each country so far.
  
 ```
country_sales  = data.groupby("Country")["Sales"].sum().reset_index()
country_sales
```

#### Product Analysis
- First, we look into the product categories as shown below

 ```
product_cat = data["Category"].unique()
print(product_cat)
```

- We also have product subcategories. Let us take a look at that.
 ```
data["Sub-Category"].unique()
```

- Next, we proceed to find out the number of sub-categories we have under each of the categories.

 ```
sub_cat_count = data.groupby("Category")
["Sub-Category"].nunique().reset_index().sort_values(by="Sub-Category")
sub_cat_count
```

##### Sales Per Product Category: Next, we proceed to find the sales made by the superstore per product category

 ```
cat_sales = data.groupby("Category")["Sales"].sum().reset_index()
cat_sales
```

- Visualizing the result
 ```
plt.bar(cat_sales["Category"],cat_sales["Sales"])
plt.title("Sales per Product Category")

plt.show()
```

- Sales Per Product Subcategory
 ```
sub_cat_sales = data.groupby("Sub-Category")["Sales"].sum()
sub_cat_sales.reset_index()
```

- Visualizing the result

 ```
sub_cat_sales = sub_cat_sales.reset_index()

plt.bar(sub_cat_sales["Sub-Category"],sub_cat_sales["Sales"])
plt.title("Sales per subcategory")
plt.xticks(rotation=90)

plt.show()
```

### Sales Trend Analysis

In the next part of the project, we are going to be carrying out an analysis of the trend of sales.
To achieve this, we will first convert the date column of the data into the pandas datetime format.

```
data["Order Date"] = pd.to_datetime(data["Order Date"], dayfirst=True)
```

- Yearly Sales

 ```
Yearly_sales = data.groupby(data["Order Date"].dt.year)["Sales"].sum().reset_index()
Yearly_sales.rename(columns= {"Order Date":"Year"}, inplace=True)
```

- Monthly Sales
  
```
monthly_sales = data.groupby(data["Order Date"].dt.month)["Sales"].sum().reset_index()
monthly_sales.rename(columns= {"Order Date":"Month"}, inplace=True)
```

- Quarterly sales
```
quarterly_sales = data.groupby(data["Order Date"].dt.quarter)["Sales"].sum().reset_index()
quarterly_sales.rename(columns= {"Order Date":"Quarter"}, inplace=True)
```

For more details on the project findings and visualizations, [see here](https://mavenanalytics.io/project/10617)
  
  





  




 
  
