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

### QA the data to exclude obvious errors, nulls and outliers

 * Ensured there are no duplicates in the unique value columns
 * Ensured there are no nulls in the primary key columns

## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
