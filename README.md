# Nashville-housing-Data-Cleaning-using-SQL
# Introduction:

Data cleaning is a critical step in the data analysis process, ensuring that the dataset is accurate, consistent, and ready for meaningful analysis. In this project, we will focus on cleaning the Nashville Housing dataset using SQL. This dataset contains information about real estate listings in Nashville, and our goal is to prepare the data for further analysis by addressing issues such as missing values, duplicates, and inconsistencies.

# Step 1: Initial Data Examination and Understanding

Connect to the SQL database and create a table to store the Nashville housing data.
Define the schema based on the columns present in the dataset.
Load the data into the SQL table, assuming a CSV file format.

#SQLQUERY

-- Connect to the database
USE your_database;

-- Create a table for Nashville housing data

CREATE TABLE nashville_housing (
    
    id INT PRIMARY KEY,
    
    address VARCHAR(255),
    
    price INT,
    
    bedrooms INT,
    
    bathrooms INT,
    
    square_feet INT,
    
    year_built INT,
    
    sale_date DATE,
    
    property_type VARCHAR(255)
);

-- Load the data into the database (assuming CSV file)

LOAD DATA INFILE 'path/to/your/nashville_housing_data.csv'

INTO TABLE nashville_housing

FIELDS TERMINATED BY ','

LINES TERMINATED BY '\n'

IGNORE 1 ROWS;


# Step 2: Identify and Handle Missing Values

Identify columns with missing values using SQL queries.
Choose appropriate strategies to handle missing values, such as replacing them with defaults or dropping rows.


#QUERY

-- Identify missing values

SELECT *

FROM nashville_housing

WHERE address IS NULL OR price IS NULL OR bedrooms IS NULL OR bathrooms IS NULL;

-- Handle missing values (replace with defaults or drop rows)

UPDATE nashville_housing

SET bedrooms = 0

WHERE bedrooms IS NULL;

-- Example: 

Drop rows with missing values in the 'price' column

DELETE FROM nashville_housing

WHERE price IS NULL;


# Step 3: Remove Duplicates and Handle Inconsistencies

Use SQL queries to identify and remove duplicate rows based on specific columns.
Correct inconsistencies, such as converting data types to ensure uniformity.

#QUERY

-- Identify and remove duplicates

WITH DuplicateCTE AS (
    
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY address, sale_date ORDER BY id) AS RowNum
    FROM nashville_housing
)

DELETE FROM DuplicateCTE

WHERE RowNum > 1;

-- Correct inconsistent data (e.g., convert 'bedrooms' to integers)

UPDATE nashville_housing

SET bedrooms = CAST(bedrooms AS SIGNED);


# Step 4: Data Verification and Finalization

Perform a final check for outliers or anomalous values that may affect the analysis.
Remove or correct any remaining issues to ensure the dataset's integrity.

#QUERY
-- Check for outliers or anomalous values

SELECT *

FROM nashville_housing

WHERE price < 0 OR bedrooms < 0 OR bathrooms < 0 OR square_feet < 0;

-- Remove outliers or correct anomalous values

DELETE FROM nashville_housing

WHERE price < 0 OR bedrooms < 0 OR bathrooms < 0 OR square_feet < 0;


# Conclusion:

Cleaning the Nashville Housing dataset using SQL involved a systematic approach, including the creation of a database table, identification and handling of missing values, removal of duplicates, and correction of inconsistencies. By executing SQL queries and statements, we ensured that the dataset is well-prepared for subsequent analysis, providing a reliable foundation for extracting meaningful insights into the real estate listings in Nashville.
