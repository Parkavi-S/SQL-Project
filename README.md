# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
This project aims at the implementation of the SQL concepts for the eCommerce database for the following such as:
  1. Extracting and Loading the raw data from .csv files
  2. Cleaning the data
  3. Analyzing Data
  4. Transforming the data according to our needs to write queries
  5. QA the data to exclude obvious errors, nulls and outliers     

## Process:

### Extracting and Loading the Raw Data from .csv files into database

* Opened the .CSV file and checked the data types of the underlying data
* Had trouble in loading the data using PGAdmin console UI, hence created the table manually and loaded data using the copy command
* Initially had data types that were not compatible with the data, hence changed the data types wherever required in the table creation script and loaded the data

  CREATE TABLE tmp_all_sessions (
  fullVisitorId numeric(20,0),
  channelGrouping VARCHAR(50),
  time1 int,
  country VARCHAR(20),
  city VARCHAR(50),
  totalTransactionRevenue VARCHAR(50),	
  transactions VARCHAR(50),
	timeOnSite INTEGER,
	pageviews INTEGER,
	sessionQualityDim INTEGER,
	date DATE,
	visitID INTEGER,
	type VARCHAR(50),
	productRefundAmount VARCHAR(50),
	productQuantity VARCHAR(50),
	productPrice INTEGER,
	productRevenue VARCHAR(50),
	productSKU VARCHAR(50),
	v2ProductName VARCHAR(100),
	v2ProductCategory VARCHAR(100),
	productVariant VARCHAR(100),
	currencyCode VARCHAR(10),
	itemQuantity VARCHAR(50),
	itemRevenue VARCHAR(50),
	transactionRevenue VARCHAR(50),
	transactionId VARCHAR(50),
	pageTitle VARCHAR(3000),
	searchKeyword VARCHAR(100),
	pagePathLevel1 VARCHAR(100),
	eCommerceAction_type INTEGER,
	eCommerceAction_step INTEGER,
	eCommerceAction_option VARCHAR(100)
)

copy public.all_sessions (fullvisitorid, channelgrouping, time1, country, city, totaltransactionrevenue, transactions, timeonsite, pageviews, sessionqualitydim, date, visitid, type, productrefundamount, productquantity, productprice, productrevenue, productsku, v2productname, v2productcategory, productvariant, currencycode, itemquantity, itemrevenue, transactionrevenue, transactionid, pagetitle, searchkeyword, pagepathlevel1, ecommerceaction_type, ecommerceaction_step, ecommerceaction_option)
FROM 'C:\Program Files (x86)\PostgreSQL\15\bin\Database\all_sessions.CSV'
DELIMITER ','
CSV HEADER ;

### Cleaning the data

* Removed duplicate values from the tables
* Transformed data types in the tables wherever necessary
* Dropped the unwanted null value columns while moving data from the temp table to the main table
* Added composite primary keys for the all_sessions and analytics tables and created primary keys in other tables
* Ensured there are no garbage values in the City column in the all_sessions table by replacing them with NULL
* Product prices and revenues were divided by 100000 to make it as actual values

  
### Analyzing the data

* Tried to explore and understand the source tables and their attributes
* explored the possibilities of joins between analytics and all_sessions table
* explored the data for different product categories to identify the number of products in each of the product categories
* Identified the pattern how the product price varies with the Channelgrouping
* Analyzed the sales_report table for the total orders and their corresponding sentiment scores and magnitude to understand the given data
  
### Transformation of the data to ensure it is useful for writing queries out of the source tables

* Made the revenue columns consistent across the all_sessions table - created three new columns to handle the casting of data type
* Consistent dataset across tables and maintaining the data types
* Created the Total_Transaction_Revenue column for summarizing the impact of revenue generated
* Worked on building the query and answering the starting with questions section

### ERD Generated for the eCommerce database

![image](https://github.com/Parkavi-S/SQL-Project/assets/67069604/ce852e08-218e-40aa-8aa1-c2f1fe46f54b)


### QA the data to exclude obvious errors, nulls and outliers

 * Ensured there are no duplicates in the unique value columns
 * Ensured there are no nulls in the primary key columns
 * Validated the dataset consistency across the tables, for data type constraints
 * Identifying outliers and investigating data points that has deviations from expected value range

## Results

Question 1: What is the highest of product viewed in United States and what is the sales for the same from the sales report vs ordered quantity

Answer: The above query shows that the product that has highest views and orders is " Men's 100% Cotton Short Sleeve Hero Tee White" which belongs to the "Men's T-Shirts" product Category, which resonates the results of starting with questions

Question 2: What are the different split for the above product by channel grouping and other observations related to the product

Answer: On research product sku - GGOEGAAX0104 - which is - Men's 100% Cotton Short Sleeve Hero Tee White that is being most searched on the web page from United states. Out of these searches- most of the page hits are related organic search channel grouping. Also the product price does vary within the same product, based on the type how the product is searched. If the product is searched through organic search, the price is 16.99 USD, but if it is searched via referral it is 13.59 USD

Question 3: Which is the product that has the most total sales across the world

Answer: The above query shows that the product "Ballpoint LED Light Pen" is the most sold product across the world.

## Challenges 

 * Analytics table had lot of duplicate entries and required a clean up which brought the data from 4 million to 1.5 million rows
 * There is no proper primary keys to be created in the all_sessions and analytics table
 * The fullvisitorid from all_sessions did not have match with the fullvisitorid in the analytics table, this behaviour was also noticed with the visitid column
 * There are lot of null values in the transaction id and transaction revenue columns making it difficult to calculate revenue based upon each transaction
 * The same holds good for the totaltransactionsrevenue column. only 81 out or 15k columns have the totaltransactionrevenue data populated making it difficult to calculate aggregate values for all the transaction
 * Itemrevnue and Itemquantity columns were null due to which itemized revenue generated could not be calculated
 * Also identified while generating ERD that we cannot have a foreign key relationship between products and sales_by_sku table as there are certain productsku which are present in sales_by_sku but not in the products table

## Future Goals
* Ensure that we could create customized sequence columns to introduce primary keys for proper data integrity and relationships
* Understand the business terms for each data and the proper calculations for the aggregate values based on key indicators
* Introduce stored procedural concepts to handle data loads wherever possible
* Had opportunity to use window functions as part of this project, but in the future would work on converting them to user defined functions and re use them wherever appropriate
