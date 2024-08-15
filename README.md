# SQL commands used for Data Cleaning

This project consists of various SQL commands that can be used in data cleaning. We often come across large data sets that require data cleaning. The question arises what are the few initial steps that we need to consider. The dataset that I have used in this project is from an automobile industry and it is named as ***automobile_data***

### Uploading the Dataset

![image](https://github.com/user-attachments/assets/208aa3db-240e-4198-95ec-0d5ef07f2e04)

### Running the Query

To find out if only two unique values are present for the fuel type and the data is clean.

```
SELECT
  DISTINCT fuel_type
FROM `absolute-accord-429300-g1.cars.car_info`
  LIMIT 1000
```

  ![image](https://github.com/user-attachments/assets/64d43235-6ed5-4a6b-8f20-692df117eaae)

 ### To inspect a column with numerical data
  
```
SELECT
  MIN(length) AS min_length,
  MAX(length) AS max_length
FROM `absolute-accord-429300-g1.cars.car_info`
```
  ![image](https://github.com/user-attachments/assets/7e93c6ca-08ac-4224-980a-5be06a95deb5)

### Fill in the Missing Data

```
SELECT
  *
FROM `absolute-accord-429300-g1.cars.car_info`
  Where num_of_doors IS null
```
  ![image](https://github.com/user-attachments/assets/c307b9d4-9b52-4d06-92fd-278bd1930bcd)

To fill in the missing values, update the table using the following query.

  ```
UPDATE
  `absolute-accord-429300-g1.cars.car_info`
SET
  num_of_doors = "four"
WHERE
  make = "dodge"
  AND fuel_type = "gas"
  AND body_style = "sedan";
```
![image](https://github.com/user-attachments/assets/b4aa1c18-a5e9-4bb0-9066-ef9a73ca6c4e)

We get a message saying the table was updated. To check if it was implemented run the previous query again.

```
  SELECT
  *
  FROM `absolute-accord-429300-g1.cars.car_info`
  Where num_of_doors IS null
```
  ![image](https://github.com/user-attachments/assets/0f6f05f8-6490-4d59-9510-28b5a8ed457c)

  As you can see we only get Mazda as the result and Dodge from the result has disappeared.

### Identify Potential Errors

```
  SELECT
   DISTINCT num_of_cylinders
  FROM
   your project name.cars.car_info;
```

  ![image](https://github.com/user-attachments/assets/69729fc2-a1a0-42d8-991a-24b8a4455786)

  Correct the spelling error in all rows:

```
UPDATE
  your project name.cars.car_info
SET
  num_of_cylinders = "two"
WHERE
  num_of_cylinders = "tow";
```

  ![image](https://github.com/user-attachments/assets/008a568a-ee62-404e-81eb-4b7c10450a95)

 Let us run the query again and see if the above query was reflected.

 ![image](https://github.com/user-attachments/assets/d99c5e4a-1c3f-4df5-8cd4-e30f94e43cb9)

Let us go ahead and see if the compression ratio values should range between 7 and 23 

![image](https://github.com/user-attachments/assets/e543bd76-b9d8-4956-8414-28c858a6180f)

Running the following query to see whether it is true and we don't have any outliers.

```
 SELECT
  MIN(compression_ratio) AS min_compression_ratio,
  MAX(compression_ratio) AS max_compression_ratio
FROM
 `absolute-accord-429300-g1.cars.car_info`
```

 ![image](https://github.com/user-attachments/assets/a98bfd35-39b8-462c-949d-bf14e87dbe89)

 This returns a maximum value of 70 but as seen earlier the max value should be 23. So the 70 is most likely 7.0. 
 Run the above query again without the row with 70 to make sure that the rest of the values fall within the expected 
 range of 7 to 23.

 ```
 SELECT
  MIN(compression_ratio) AS min_compression_ratio,
  MAX(compression_ratio) AS max_compression_ratio
FROM
  `absolute-accord-429300-g1.cars.car_info`
WHERE
  compression_ratio <> 70;
  ```
 
  ![image](https://github.com/user-attachments/assets/ae610fe5-7960-41da-9de4-b67d08df855d)

  Now the highest value is 23, which aligns with the data description. Before we correct the erroneous value of 70 we need to check with the manager and also see whether this error is only in one row or multiple rows before we delete/rectify it. Using the query below to count how many rows I would be deleting:

```
SELECT
   COUNT(*) AS num_of_rows_to_delete
FROM
   `absolute-accord-429300-g1.cars.car_info`
WHERE
   compression_ratio = 70;
```

   ![image](https://github.com/user-attachments/assets/e28a3a96-be48-4761-bfcf-2e09f9ab9623)

   We get the result as 1 and hence we can go and delete it. To delete that row we need to run the following query.

   ```
   DELETE `absolute-accord-429300-g1.cars.car_info`
WHERE compression_ratio = 70;
```

   ![image](https://github.com/user-attachments/assets/ece71e84-2506-48ba-a002-7a69f133d353)

### Ensuring Consistency.

```
SELECT
  DISTINCT drive_wheels
FROM
 `absolute-accord-429300-g1.cars.car_info`;
```

 ![image](https://github.com/user-attachments/assets/cf24a48a-1504-4390-b16d-969f8d61e04b)

As you can see, 4WD appears twice in the results. However, because you used a SELECT DISTINCT statement to return unique values, this probably means there’s an extra space in one of the 4WD entries that makes it different from the other 4WD. Let us run the following code to see which 4WD has this extra space.

```
SELECT
  DISTINCT drive_wheels,
  LENGTH(drive_wheels) AS string_length
FROM
  `absolute-accord-429300-g1.cars.car_info` 
```

![image](https://github.com/user-attachments/assets/8f1323dc-e573-40fd-ab33-cb1ee7e669d1)

The LENGTH statement helps us  determine the length of each of these string variables as show above. As you can see, some instances of the 4wd string have four characters instead of the expected three (4wd has 3 characters). In that case, you can use the TRIM function to remove all extra spaces in the drive_wheels column

```
UPDATE
  `absolute-accord-429300-g1.cars.car_info`
SET
  drive_wheels = TRIM(drive_wheels)
WHERE TRUE;
```

```
SELECT
  DISTINCT drive_wheels
FROM
 `absolute-accord-429300-g1.cars.car_info`
```

![image](https://github.com/user-attachments/assets/b29f41f9-f2cd-478b-8729-aad399550320)

As you can see the extra space from 4WD has been removed using the TRIM function and hence when we use the DISTINCT function this time it only gives us 3 values in return.

## Advanced data-cleaning functions.

CAST():
When you import data that doesn't already exist in your SQL tables, the data types from the new dataset might not have been imported correctly.
Basically, CAST can be used to convert anything from one data type to another.

In order to perform this function I have uploaded another data set names laurens_customer_data and created a table with the name laurens_customer_purchase.

![image](https://github.com/user-attachments/assets/ba83bfc4-aa2c-4ff2-9297-9f175a66caa9)

Lets say we want to sort all the purchases based on purchase price in descending order which means we want most ex[pensive purchase to show on top of the list in our results. 

```
SELECT 
 purchase_price
FROM 
 `absolute-accord-429300-g1.laurens_customers_data.laurens_customer_purchase` 
Order BY
purchase_price DESC
```

![image](https://github.com/user-attachments/assets/668cb1dd-7033-4685-bf57-cdc627aa9613)

As you can see we have 89.85 value listed on the top instead of 799.99 which is not true to our query as we have written the query such that we get values in descending order. This is because when we look into schema of the table we see purchase_price is under _String_ data type. Let us run a SQL query using Cast to fix this.

```
SELECT 
 CAST (purchase_price AS Float64)
FROM 
 `absolute-accord-429300-g1.laurens_customers_data.laurens_customer_purchase` 
Order BY
CAST(purchase_price AS Float64) DESC
```

![image](https://github.com/user-attachments/assets/0ccf9249-f6e2-455a-9209-3f9eca66e91d)


Let's consider a different scenario and write a SQL query that will pull date and purchase_price for all purchases that occurred between December 1st, 2020 and December 31st, 2020

```
SELECT 
 date,
 purchase_price
FROM 
 `absolute-accord-429300-g1.laurens_customers_data.laurens_customer_purchase` 
WHERE
 date between '2020-12-01' AND '2020-12-31'
```

![image](https://github.com/user-attachments/assets/a006a82b-584f-45d2-a6a4-f53e04349ce7)

As you can see, the date columns look odd, consisting of the date and  time. In order to get just date and not time in return we will run the following query using the Cast function again

```
SELECT 
 CAST (date AS date) AS date_only,
 purchase_price
FROM 
 `absolute-accord-429300-g1.laurens_customers_data.laurens_customer_purchase` 
WHERE
 date between '2020-12-01' AND '2020-12-31'
```

CONCAT()

This function is used to add strings together to create new text strings that can be used as unique keys.

```
SELECT 
  CONCAT (product_code,product_color) AS new_product_code
FROM 
 `absolute-accord-429300-g1.laurens_customers_data.laurens_customer_purchase` 
WHERE
 product='couch'
```

![image](https://github.com/user-attachments/assets/f6e80502-0c93-4c37-be0d-fc2a10d97f96)

This helps us find out which colour couches are more in demand and whether customers prefer one colour over the other.

COALESCE():

This function is used to return non-null values in a list.

![image](https://github.com/user-attachments/assets/b47399ab-00ac-4631-b5ac-79439018cf79)

As we can see there are null values in our table. Lets run a query using COALESCE() to solve this issue.

![image](https://github.com/user-attachments/assets/fad678fd-9170-432a-a919-2a64135944c5)

This is how the query retrieves the product code for null values. Coalesce can also be used in calculations.
