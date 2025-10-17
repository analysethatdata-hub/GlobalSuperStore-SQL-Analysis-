# GlobalSuperStore-SQL-Analysis
This is an analysis of retail data
# GlobalSuperStore Analysis SQL Project

## Project Overview

**Project Title**: GlobalSuperStore Analysis  
**Level**: Beginner  


This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore and analyze retail sales data. The project involves creating table in sql using sqlalchemy,data cleaning and performing exploratory data analysis (EDA) through SQL queries. 

## Objectives

1. **Create a table GlobalSuperStore**: Create and populate a table with the provided data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.


## Project Structure

### 1. Table Creation


- **Table Creation**: The table is imported from a csv file into sql server.


### 2. Data Preprocessing & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Ship Mode Count**: Find out how many unique shipping modes are in the dataset.
- **Customerid Count**: Identify all unique customerid's in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.
- **Duplicate Value Check**: Check for any duplicate values in the dataset and delete records with duplicate data.
- **Segment Count**:Find out how many unique segments are in the dataset.
- **City Count**:Find out how many unique cities are in the dataset.
- **State Count**:Find out how many unique states are in the dataset.
- **Countries Count**:Find out how many unique countries are in the dataset.
- **Regions Count**:Find out how many unique regions are in the dataset.
- **Categories Count**:Find out how many unique Categories are in the dataset.
- **SubCategories Count**:Find out how many unique SubCategories are in the dataset.
- **Productname Count**:Find out how many unique products are in the dataset.
- **Order priority Count**:Find out how many unique order priority are in the dataset.


```sql


select count(*) as no_of_transactions from Global_Superstore


select * 
from Global_Superstore 
where row_id is null
or order_id is null,
or order_date is null,
or ship_date is null, 
or ship_mode is null,
or customer_id is null,
or customer_name is null,
or segment is null,
or city is null,
or states is null,
or country is null,
or markets is null,
or regions is null,
or product_id is null,
or categories is null,
or sub_categories is null,
or product_name is null,
or quantities is null,
or freightcost is null, 
or order_priority is null,
or years is null,
or months is null,
or sales is null


select row_id,
count(*) as no_of_transactions 
from Global_Superstore
group by row_id
having count(*) >1

select ship_mode 
from Global_Superstore
group by ship_mode

select top 5 customer_id 
from Global_Superstore
group by customer_id 

select top 5 customer_name
from Global_Superstore
group by customer_name

select segment
from Global_Superstore
group by segment


select top 5 city
from Global_Superstore
group by city

select top 5 states
from Global_Superstore
group by states

select top 5 countries
from Global_Superstore
group by countries

select markets
from Global_Superstore
group by markets

select regions
from Global_Superstore
group by regions

select category
from Global_Superstore
group by category

select sub_category
from Global_Superstore
group by sub_category

select top 5 product_name
from Global_Superstore
group by product_name

select order_priority
from Global_Superstore
group by order_priority

select top 5 countries,count(distinct states) as states,count(distinct city) as cities
from Global_Superstore 
group by countries
order by count(distinct states) desc,count(distinct city) desc

select category,count(distinct sub_category) as subcategories
from Global_Superstore 
group by category
order by count(distinct sub_category)


```

### 3. Data Exploration & Findings

The following SQL queries were developed to explore the data:

1. **SQL query to returns Total Sales**:

```sql
select round(sum(sale),2) as Total_Sales from Global_Superstore
```

2. **SQL Query to find Revenues by Year**:
```sql
	select year(order_date)as years,
        round(sum(sale),2) as Total_Sales 
        from Global_Superstore 
        group by year(order_date)
        order by year(order_date)
```

3. **SQL Query to find the busiest period**:
```sql
	select year(order_date)as years,month(order_date) as months,
        round(sum(sale),2) as Total_Sales 
        from Global_Superstore 
        group by year(order_date),month(order_date)
        order by Total_Sales desc

```

4. **SQL Query to find the busiest period**:
```sql
	select year(order_date)as years,month(order_date) as months,
        round(sum(sale),2) as Total_Sales 
        from Global_Superstore 
        group by year(order_date),month(order_date)
        order by Total_Sales desc
```

5. **SQL Query to measure YOY growth**:
```sql
with curr as(
        select year(order_date) as years,round(SUM(sale),2) as Total_Sales
        from Global_Superstore
        group by year(order_date)
        --order by year(order_date)
        ), prev_yr as(
        select *,
        LAG(Total_Sales,1,Total_Sales) over(order by years) as prev_sales
        from curr
        )
        select *,
        round((Total_Sales-prev_sales)/prev_sales,2)*100 as YoY
        from prev_yr
```
6. **SQL Query to measure YOY growth for quantities**:
```sql
	with curr_qt as(
        select year(order_date) as years,round(SUM(quantities),2) as Total_Quantities
        from Global_Superstore
        group by year(order_date)
        --order by year(order_date)
        ), prev_yr_qt as(
        select *,
        LAG(Total_Quantities,1,Total_Quantities) over(order by years) as prev_quantities
        from curr_qt
        )
        select *,
        round((Total_Quantities-prev_quantities)/prev_quantities,2)*100 as YoY_qt
        from prev_yr_qt

```
7. **SQL Query to measure MoM growth**:
```sql
	with mon as(
        select year(order_date)as years,month(order_date) as months,
        round(sum(sale),2) as Total_Sales 
        from Global_Superstore 
        group by year(order_date),month(order_date)
        --order by year(order_date) desc,month(order_date) desc
        ),prev_mon as(
        select *,
        LAG(Total_Sales,1,Total_Sales) over(order by years,months) as prev_months
        from mon
        )
        select *,
        round((Total_Sales-prev_months)/prev_months,2)*100 as MoM
        from prev_mon
```
8. **SQL Query to measure MoM growth of quantities**:
```sql
	with mon_qt as(
        select year(order_date)as years,month(order_date) as months,
        round(sum(quantities),2) as Total_Quantities 
        from Global_Superstore 
        group by year(order_date),month(order_date)
        --order by year(order_date) desc,month(order_date) desc
        ),prev_mon_qt as(
        select *,
        LAG(Total_Quantities,1,Total_Quantities) over(order by years,months) as prev_months_qt
        from mon_qt
        )
        select *,
        round((Total_Quantities-prev_months_qt)/prev_months_qt,2)*100 as MoM_qt
        from prev_mon_qt
```
9. **SQL Query to measure category sales**:
```sql
	select category,sub_category,round(sum(sale),2) as Total_Sales
        from Global_Superstore 
        group by category,sub_category
        order by Total_Sales desc
```

10. **SQL Query to measure category by quantities sold**
```sql
	select category,sub_category,round(sum(quantities),2) as Total_Quantities
        from Global_Superstore 
        group by category,sub_category
        order by Total_Quantities desc
```
11. **SQL Query to measure categories by freightcost**
```sql
	select category,sub_category,round(sum(freightcost),2) as Total_Cost
        from Global_Superstore 
        group by category,sub_category
        order by Total_Cost desc
```
## Findings

- **Analysis**:Sales have increased in each of the operating years.The last four Months of 4 months of the year are typically the busiest period. The exception is June where Sales spike,but then fall the following month.The sales have increased by 16,29 and 25% since inception. There was a slight decrease from 2013 to 2014 of 4%. But overall it has been generally significant growth. The month to month changes are very volatile,if we observe June 2013, we can notice the previous month had growth of 60% and June also had a 60% growth only for July sales to decrease by 46%,this is an alarming trend. We also observe that the first 2 months are usually the worst when it comes to sales. Sales Quantities have increased in each of the operating years. Sales Quantities vary substantially according to absolute values from month to month. Sales Quantities have increased in each of the operating years. The 2014 26% increment is the same as the previous period which is still encouraging but alarming, because it has not exceeded the previous year,which might require further investigation. Sales Quantities are very strong in November we can observe that the last 3 years we have enjoyed 40% increments,with only 2014 where there was a decrease to 31% which is also alarming. We also observe that August is a very outstanding Month for quantities sold as these range between 50 and 70%. In actual fact August seems to be our busiest period in terms of Sales Quantities. Most sales are shipped via standard class. Sales quantities also reflect the same as above. Standard class bears the most cost. Leading country by sales value is the USA, New York state and New York city being the leader by sales value. In Australia, the state of New South Wales is the leading state and city being Sydney. Leading country by sales volume is the USA, New York state and New York city being the leader. In second is the Philippines, the state of National Capital is the leading state and city being Manila. Asia Pacific is the leading market in terms of sales value. Asia Pacific also leads by sales volume. Leading region by far is the Central region by sales value. The Central region also leads by sales volume. The central region also leads by freight cost. When we include countries we observe that the Eastern region of the USA,which is New York is the leading region. Technology is the leading category by sales value. Office supplies are leading by sales volume. Technology is the leader by freight cost. In furniture sales chairs are the leading subcategory,In office supplies the top seller is storage and finally in Technology the Phones are the leading sub category by sales value. Overall the phones are leading by sales value. In furniture sales of chairs are the leading subcategory,In office supplies the top seller are Binders and finally in Technology the Phones are the leading sub category by sales volume. Overall the Binders are leading by sales volume.


## Conclusion

This project serves as an introduction to SQL for data analysts, covering table creation, data cleaning and exploratory data analysis. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.


## Author - analysethatdata

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles.
