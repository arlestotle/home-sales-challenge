# Challenge 20 - home-sales-challenge

## In this challenge, you'll use your knowledge of SparkSQL to determine key metrics about home sales data. Then you'll use Spark to create temporary views, partition the data, cache and uncache a temporary table, and verify that the table has been uncached.

### Overview 
####
1. Rename the Home_Sales_starter_code.ipynb file as Home_Sales.ipynb.
2. Import the necessary PySpark SQL functions for this assignment.
3. Read the home_sales_revised.csv data in the starter code into a Spark DataFrame.
4. Create a temporary table called home_sales.
5. Answer the following questions using SparkSQL:

- What is the average price for a four-bedroom house sold for each year? Round off your answer to two decimal places.
+----+-------------+
|year|average_price|
+----+-------------+
|2022|    296363.88|
|2021|    301819.44|
|2020|    298353.78|
|2019|     300263.7|
+----+-------------+

- What is the average price of a home for each year it was built that has three bedrooms and three bathrooms? Round off your answer to two decimal places.
+----------+-------------+
|year_built|average_price|
+----------+-------------+
|      2017|    292676.79|
|      2016|    290555.07|
|      2015|     288770.3|
|      2014|    290852.27|
|      2013|    295962.27|
|      2012|    293683.19|
|      2011|    291117.47|
|      2010|    292859.62|
+----------+-------------+

- What is the average price of a home for each year that has three bedrooms, three bathrooms, two floors, and is greater than or equal to 2,000 square feet? Round off your answer to two decimal places.
+----------+-------------+
|year_built|average_price|
+----------+-------------+
|      2017|    280317.58|
|      2016|     293965.1|
|      2015|    297609.97|
|      2014|    298264.72|
|      2013|    303676.79|
|      2012|    307539.97|
|      2011|    276553.81|
|      2010|    285010.22|
+----------+-------------+

- What is the "view" rating for homes costing more than or equal to $350,000? Determine the run time for this query, and round off your answer to two decimal places.
+----+-------------+
|view|average_price|
+----+-------------+
|  99|   1061201.42|
|  98|   1053739.33|
|  97|   1129040.15|
|  96|   1017815.92|
|  95|    1054325.6|
|  94|    1033536.2|
|  93|   1026006.06|
|  92|    970402.55|
|  91|   1137372.73|
|  90|   1062654.16|
|  89|   1107839.15|
|  88|   1031719.35|
|  87|    1072285.2|
|  86|   1070444.25|
|  85|   1056336.74|
|  84|   1117233.13|
|  83|   1033965.93|
|  82|    1063498.0|
|  81|   1053472.79|
|  80|    991767.38|
+----+-------------+
only showing top 20 rows

6. Cache your temporary table home_sales.
7. Check if your temporary table is cached.
8. Using the cached data, run the query that filters out the view ratings with an average price of greater than or equal to $350,000. 
- Determine the runtime and compare it to uncached runtime.
    Using the cached data the runtime was 0.586 seconds whereas the uncached data runtime was 1.591. 
    Cached data is used to quickly load the application or website's information every time the user subsequently opens or visits it.

9. Partition by the "date_built" field on the formatted parquet home sales data.
10. Create a temporary table for the parquet data.
11. Run the query that filters out the view ratings with an average price of greater than or equal to $350,000. 
- Determine the runtime and compare it to uncached runtime.
     Using the parquet data the runtime was 0.562 seconds. 
     Using the cached data the runtime was 0.586 seconds. 
     The parquet files load slightly faster than the cached data because the parquet data is organized into row groups, which are further divided into pages. 
     This allows for fast data access, as only the necessary row groups and pages need to be loaded into memory.

12. Uncache the home_sales temporary table.
13. Verify that the home_sales temporary table is uncached using PySpark.
14. Download your Home_Sales.ipynb file and upload it into your "Home_Sales" GitHub repository.

### Outside Help 
#### For this assignment, I used outside sources such as Stack Overflow, ChatGPT, and Colab AI.
- In the first chunck of code it is important to change ''spark-3.4.0' to 'spark-3.4.2' to match the current version. 
- The HAVING clause in SQL speecifes that an SQL SELECT statement must only return rows were aggregate values meet the specified conditions
    'HAVING AVG(price) >= 350000' 
- Before finding the HAVING clause, I attempted to use 'WHERE price >= 350000' however this line of code returned a table where the view numberic values were sorted by the first digit of the complete number. 
            avg_price_view_350000 = """
            SELECT
              view,
              ROUND(AVG(price), 2) AS average_price
            FROM (
              SELECT *
              FROM home_sales
              WHERE price >= 350000
            ) AS filtered_sales
            GROUP BY view
            ORDER BY view DESC
            """
            spark.sql(avg_price_view_350000).show() 
    - This returns: the view is sorted by the first digit of the complete numberic value. 
        +----+-------------+
        |view|average_price|
        +----+-------------+
        |  99|   1061201.42|
        |  98|   1053739.33|
        |  97|   1129040.15|
        |  96|   1017815.92|
        |  95|    1054325.6|
        |  94|    1033536.2|
        |  93|   1026006.06|
        |  92|    970402.55|
        |  91|   1137372.73|
        |  90|   1062654.16|
        |   9|    401393.34|
        |  89|   1107839.15|
        |  88|   1031719.35|
        |  87|    1072285.2|
        |  86|   1070444.25|
        |  85|   1056336.74|
        |  84|   1117233.13|
        |  83|   1033965.93|
        |  82|    1063498.0|
        |  81|   1053472.79|
        +----+-------------+
- Cashe tables to store data that you access frequently but that does not change often. A cache table can improve  performance by storing the data locally instead of accessing the data directly from the data source. 


