**I have identified the following risk areas to highlight some of them below:**

1. Duplicates inside the analytics table. Had to work on cleaning the table thoroughly
2. Ensured the key columns to be used such as unit_sold from analytics, visitid and fullvisitorid from analytics and all_session tables are not having null values
3. Identification of proper key columns to join analytics and all_session table was a huge challenge
4. the Sales_sku table looked like a subset of sales_report and felt it as redundant
5. Not all the transaction ids and transaction revenues were available, which made the process of writting query for the given questions tedious
6. Absence of proper primary key values in the tables such as all_Session and analytics makes it difficult to arrive at unique values for ERD generation


**QA Process:**

**1. Ensured the key columns did not have any null values in it**

select * from public.analytics where units_sold is not null

**2. Made the revenue columns consistent across the all_sessions table - created three new columns to handle the casting of data type as below:**

	update all_sessions a
	set total_transaction_revenue =TOT_TRANS_REV
	FROM (
	    SELECT totaltransactionrevenue, (cast(totaltransactionrevenue as double precision) / 1000000) TOT_TRANS_REV
		FROM all_sessions
		WHERE totaltransactionrevenue is not null
	 ) b
	WHERE a.totaltransactionrevenue =b.totaltransactionrevenue


	update all_sessions a
	set product_revenue =PROD_REV
	FROM (
	    SELECT productrevenue, (cast(productrevenue as double precision) / 1000000) PROD_REV
		FROM all_sessions
		WHERE productrevenue is not null
	 ) b
	WHERE a.productrevenue =b.productrevenue

	update all_sessions a
	set transaction_revenue =TRANS_REV
	FROM (
	    SELECT transactionrevenue, (cast(transactionrevenue as double precision) / 1000000) TRANS_REV
		FROM all_sessions
		WHERE transactionrevenue is not null
	 ) b
	WHERE a.transactionrevenue =b.transactionrevenue
	
**3. Worked on creating a consistent dataset across the tables, such as maintaining the datatypes for the revenue and units_sold in the analytics table**

	ALTER TABLE public.analytics
	ALTER COLUMN units_sold type int
	USING units_sold::Integer

	ALTER TABLE public.analytics
	ALTER COLUMN revenue type int
	USING revenue::Integer

**4. Identifying outliers and investigating data points that has deviations from expected value range**

	select min(total_ordered) from public.sales_report

	select max(total_ordered) from public.sales_report
