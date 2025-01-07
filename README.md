****Customer Segmentation and Insights – RFM & KMeans Clustering Project ****

This project implements an ETL (Extract, Transform, Load) pipeline to process Kickstarter campaign data. The pipeline transforms raw input data into structured dimensional and fact tables, enabling efficient analytics for decision-making and reporting.

**Use Cases**
Analyze Campaign Success Rates: Track success/failure rates across countries and categories.
Study Trends: Monitor monthly and yearly trends in campaign launches and outcomes.
Evaluate Funding Patterns: Understand backer behavior, funding amounts, and campaign success.

**Features**
Automated Dimensional Table Creation:
Scripts to dynamically populate date, country, and subcategory dimensions.
Detailed Fact Tables:
Fact tables capture campaign outcomes, timelines, and funding details.
Easy-to-Use SQL Scripts:
Modular and reusable SQL scripts for the ETL processes, which can be run independently.

**Tables Description**
dim_date: Stores date-related information (e.g., campaign launch and deadline dates).
dim_country: Stores country details where the campaigns were launched.
dim_subcategory: Stores details about subcategories for the campaigns.
fact_campaign_outcome: Contains campaign success/failure status, funding goal, backer details, etc.
fact_campaign_timeline: Tracks campaign duration (launch to deadline).
fact_campaign_funding: Stores pledged amounts, backer information, and financial data for each campaign.

**Prerequisites**
Database Platform: SQL-compatible database (e.g., MySQL, PostgreSQL).
Source Data: Ensure the silver_kickstarter_part2 table exists with the following columns:
ID, Name, Category, Subcategory, Country, Launched, Deadline, Goal, Pledged, Backers, State.
Database Permissions: Ensure you have write permissions to create and populate tables.
Setup and Usage
Step 1: Clone the Repository
git clone https://github.com/yourusername/kickstarter-etl-pipeline.git
cd kickstarter-etl-pipeline

Step 2: Execute SQL Scripts
Run the scripts in the following order to create and populate the tables:
populate_dim_date.sql - Populates the date dimension.
populate_dim_country.sql - Populates the country dimension.
populate_dim_subcategory.sql - Populates the subcategory dimension.
populate_fact_campaign_outcome.sql - Populates the campaign outcome fact table.
populate_fact_campaign_timeline.sql - Populates the campaign timeline fact table.
populate_fact_campaign_funding.sql - Populates the campaign funding fact table.

Step 3: Verify Table Creation
After running the scripts, ensure the following tables are created and populated:
dim_date
dim_country
dim_subcategory
fact_campaign_outcome
fact_campaign_timeline
fact_campaign_funding

Step 4: Perform Analytics
Leverage the processed data to perform custom queries and analysis. Below are a few examples:

Example Queries
Campaign Success Rate by Country and Category:

%sql
SELECT country, category, COUNT(*) AS total_campaigns, 
       SUM(CASE WHEN state = 'successful' THEN 1 ELSE 0 END) AS successful_campaigns
FROM fact_campaign_outcome
JOIN dim_country ON fact_campaign_outcome.country_id = dim_country.country_id
JOIN dim_category ON fact_campaign_outcome.category_id = dim_category.category_id
GROUP BY country, category;
Monthly Campaign Launches:

%sql
SELECT EXTRACT(MONTH FROM launched) AS month, COUNT(*) AS total_launches
FROM fact_campaign_timeline
GROUP BY month;
Funding Trends Over Time:

%sql
SELECT EXTRACT(YEAR FROM launched) AS year, 
       SUM(pledged) AS total_pledged, 
       COUNT(*) AS total_campaigns
FROM fact_campaign_funding
GROUP BY year
ORDER BY year;

**Error Handling and Logging**
The ETL scripts should handle common data issues, such as missing values or data type mismatches.
Logs are created during script execution, allowing users to track any errors or anomalies during the ETL process.

**Testing**
After executing the ETL scripts, you can run the example queries mentioned above to verify that the tables are populated correctly and contain the expected data.

**Data Quality Checks**
Ensure that all pledged amounts are numeric and backer data is consistent.
Check for any missing or invalid data in the input tables before running the ETL process.
Dependencies
Ensure you have the following:

SQL-compatible database platform (Databricks,AWS).
Access to the source table silver_kickstarter_part2.


