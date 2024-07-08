# Retail and E-Commerce Datasets

This repository offers a comprehensive collection of retail and e-commerce datasets, ideal for use in Large Language Model and Machine Learning projects. These datasets, sourced from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), include detailed customer information, geolocation data, order details, product information, and seller information. To facilitate collaboration across multiple parties, we transformed these datasets, saved them in Parquet files, and compressed them using the ZSTD compression method. For access to the code, please contact cr.ooi@bloomdata.com.my.

## Step 1: ETL Process (Extract, Transform, Load)

> Extracting data from CSV files, handling missing and duplicate values, transforming data, and storing it in a columnar-based database.

### ETL process for Large Language Model projects

<details>
<summary>
Workflow and output of ETL process for product_category_name_translation.csv.
</summary>

- Checking data type consistency, missing values, and duplicate values before processing.

  > The `product_category_name_translation.csv` file has no missing values and duplicate values across all the columns.
  
  > Even though there is no missing value found, there is a need to check further for data integrity.
  
  > Below is the summary of data types, missing, and duplicate values before processing.
  
  ```
  +---+-------------------------------+-----------+----------------+------------------+
  |   | Column                        | Data Type | Missing Values | Duplicate Values |
  +---+-------------------------------+-----------+----------------+------------------+
  | 0 | product_category_name         | object    | 0              | 0                |
  | 1 | product_category_name_english | object    | 0              | 0                |
  +---+-------------------------------+-----------+----------------+------------------+
  ```

- Checking and converting columns to object data type to enable replacement of empty strings, null values, and 'nan' values with 'Unknown'.

  > Column: `product_category_name` already in object data type.
  
  > Column: `product_category_name_english` already in object data type.

- Checking and replacing empty strings, null values, and 'nan' values in each object column with 'Unknown'.
  
  > No empty strings, null values, or 'nan' values found in column: `product_category_name`.
  
  > No empty strings, null values, or 'nan' values found in column: `product_category_name_english`.

- Checking and refining column: `product_category_name` for text with diacritics and special characters.
  
  > No text with diacritics and special characters found in column: `product_category_name`.

- Checking and refining column: `product_category_name_english` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_category_name_english`.

- Since all the columns are unique and they will be merged with `products` table later, no further data transformation is needed.

- Below are the first 3 rows of the `product_category_name_translation` table after processing and storing in the database.

- The complete table is available in `product_category_name_translation.parquet`.

```
+---+------------------------+-------------------------------+
|   | product_category_name  | product_category_name_english |
+---+------------------------+-------------------------------+
| 0 | beleza_saude           | health_beauty                 |
| 1 | informatica_acessorios | computers_accessories         |
| 2 | automotivo             | auto                          |
+---+------------------------+-------------------------------+
```
</details>

<details>
<summary>
Workflow and output of ETL process for products.csv.
</summary>

- Checking data type consistency, missing values, and duplicate values before processing.

  > The `products.csv` file has some missing values and contains duplicate values in most of the columns.
  
  > Below is the summary of data types, missing, and duplicate values before processing.

  ```
  +---+----------------------------+-----------+----------------+------------------+
  |   | Column                     | Data Type | Missing Values | Duplicate Values |
  +---+----------------------------+-----------+----------------+------------------+
  | 0 | product_id                 | object    | 0              | 0                |
  | 1 | product_category_name      | object    | 610            | 32877            |
  | 2 | product_name_lenght        | float64   | 610            | 32884            |
  | 3 | product_description_lenght | float64   | 610            | 29990            |
  | 4 | product_photos_qty         | float64   | 610            | 32931            |
  | 5 | product_weight_g           | float64   | 2              | 30746            |
  | 6 | product_length_cm          | float64   | 2              | 32851            |
  | 7 | product_height_cm          | float64   | 2              | 32848            |
  | 8 | product_width_cm           | float64   | 2              | 32855            |
  +---+----------------------------+-----------+----------------+------------------+
  ```

- Merge `products` table with `product_category_name_translation` table to update product category names from Portugues to English.

- Checking data type consistency, missing values, and duplicate values after joining.

  > The merged DataFrame shows an increase in missing and duplicate values, particularly in the `product_category_name_english` column.
  
  > Below is the summary of data types, missing, and duplicate values after joining.

  ```
  +---+-------------------------------+-----------+----------------+------------------+
  |   | Column                        | Data Type | Missing Values | Duplicate Values |
  +---+-------------------------------+-----------+----------------+------------------+
  | 0 | product_id                    | object    | 0              | 0                |
  | 1 | product_category_name_english | object    | 623            | 32879            |
  | 2 | product_name_lenght           | float64   | 610            | 32884            |
  | 3 | product_description_lenght    | float64   | 610            | 29990            |
  | 4 | product_photos_qty            | float64   | 610            | 32931            |
  | 5 | product_weight_g              | float64   | 2              | 30746            |
  | 6 | product_length_cm             | float64   | 2              | 32851            |
  | 7 | product_height_cm             | float64   | 2              | 32848            |
  | 8 | product_width_cm              | float64   | 2              | 32855            |
  +---+-------------------------------+-----------+----------------+------------------+
  ```

- Checking and converting columns to object data type to enable replacement of empty strings, null values, and 'nan' values with 'Unknown'.
  
  > Column: `product_id` already in object data type.

  > Column: `product_category_name_english` already in object data type.

  > Converted column: `product_name_lenght` from float64 to object data type.

  > Converted column: `product_description_lenght` from float64 to object data type.

  > Converted column: `product_photos_qty` from float64 to object data type.

  > Converted column: `product_weight_g` from float64 to object data type.

  > Converted column: `product_length_cm` from float64 to object data type.

  > Converted column: `product_height_cm` from float64 to object data type.

  > Converted column: `product_width_cm` from float64 to object data type.

- Checking data type consistency, missing values, and duplicate values after converting data types.

  > The DataFrame now shows missing values only in the `product_category_name_english` column. Duplicate values remain the same.
  
  > There is a need to check further for data integrity.
  
  > Below is the summary of data types, missing, and duplicate values after converting data types.

  ```
  +---+-------------------------------+-----------+----------------+------------------+
  |   | Column                        | Data Type | Missing Values | Duplicate Values |
  +---+-------------------------------+-----------+----------------+------------------+
  | 0 | product_id                    | object    | 0              | 0                |
  | 1 | product_category_name_english | object    | 623            | 32879            |
  | 2 | product_name_lenght           | object    | 0              | 32884            |
  | 3 | product_description_lenght    | object    | 0              | 29990            |
  | 4 | product_photos_qty            | object    | 0              | 32931            |
  | 5 | product_weight_g              | object    | 0              | 30746            |
  | 6 | product_length_cm             | object    | 0              | 32851            |
  | 7 | product_height_cm             | object    | 0              | 32848            |
  | 8 | product_width_cm              | object    | 0              | 32855            |
  +---+-------------------------------+-----------+----------------+------------------+
  ```

- Checking and replacing empty strings, null values, and 'nan' values in each object column with 'Unknown'.

  > No empty strings, null values, or 'nan' values found in column: `product_id`.
  
  > Empty strings, null values, or 'nan' values found in column: `product_category_name_english`. Replaced with 'Unknown'.

  > Empty strings, null values, or 'nan' values found in column: `product_name_lenght`. Replaced with 'Unknown'.

  > Empty strings, null values, or 'nan' values found in column: `product_description_lenght`. Replaced with 'Unknown'.

  > Empty strings, null values, or 'nan' values found in column: `product_photos_qty`. Replaced with 'Unknown'.

  > Empty strings, null values, or 'nan' values found in column: `product_weight_g`. Replaced with 'Unknown'.

  > Empty strings, null values, or 'nan' values found in column: `product_length_cm`. Replaced with 'Unknown'.

  > Empty strings, null values, or 'nan' values found in column: `product_height_cm`. Replaced with 'Unknown'.

  > Empty strings, null values, or 'nan' values found in column: `product_width_cm`. Replaced with 'Unknown'.

- Checking and refining column: `product_id` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_id`.

- Checking and refining column: `product_category_name_english` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_category_name_english`.

- Checking and refining column: `product_name_lenght` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_name_lenght`.

- Checking and refining column: `product_description_lenght` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_description_lenght`.

- Checking and refining column: `product_photos_qty` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_photos_qty`.

- Checking and refining column: `product_weight_g` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_weight_g`.

- Checking and refining column: `product_length_cm` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_length_cm`.

- Checking and refining column: `product_height_cm` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_height_cm`.

- Checking and refining column: `product_width_cm` for text with diacritics and special characters.

  > No text with diacritics and special characters found in column: `product_width_cm`.

- Checking the validity of duplicate values

  > First 3 duplicate entries in column: `product_category_name_english`: ['baby', 'musical_instruments', 'furniture_decor'].

  > First 3 duplicate entries in column: `product_name_lenght`: ['56.0', '59.0', '56.0'].

  > First 3 duplicate entries in column: `product_description_lenght`: ['206.0', '509.0', '402.0'].

  > First 3 duplicate entries in column: `product_photos_qty`: ['1.0', '1.0', '1.0'].

  > First 3 duplicate entries in column: `product_weight_g`: ['600.0', '200.0', '400.0'].

  > First 3 duplicate entries in column: `product_length_cm`: ['16.0', '17.0', '17.0'].

  > First 3 duplicate entries in column: `product_height_cm`: ['10.0', '10.0', '7.0'].

  > First 3 duplicate entries in column: `product_width_cm`: ['17.0', '13.0', '17.0'].

- Checking data type consistency, missing values, and duplicate values after data cleaning.

  > The DataFrame now shows no missing values. Duplicate values remain the same.
  
  > The duplicate values are acceptable since they are valid in this case.
  
  > Below is the summary of data types, missing, and duplicate values after data cleaning.

  ```
  +---+-------------------------------+-----------+----------------+------------------+
  |   | Column                        | Data Type | Missing Values | Duplicate Values |
  +---+-------------------------------+-----------+----------------+------------------+
  | 0 | product_id                    | object    | 0              | 0                |
  | 1 | product_category_name_english | object    | 0              | 32879            |
  | 2 | product_name_lenght           | object    | 0              | 32884            |
  | 3 | product_description_lenght    | object    | 0              | 29990            |
  | 4 | product_photos_qty            | object    | 0              | 32931            |
  | 5 | product_weight_g              | object    | 0              | 30746            |
  | 6 | product_length_cm             | object    | 0              | 32851            |
  | 7 | product_height_cm             | object    | 0              | 32848            |
  | 8 | product_width_cm              | object    | 0              | 32855            |
  +---+-------------------------------+-----------+----------------+------------------+
  ```  
</details>
