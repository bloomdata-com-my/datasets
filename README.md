# Retail and E-Commerce Datasets

This repository offers a comprehensive collection of retail and e-commerce datasets, sourced primarily from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). These datasets include detailed customer profiles, geolocation data, order specifics, product details, and seller information, making them ideal for showcasing contextual analysis where Large Language Models excel over conventional Machine Learning approaches. To facilitate collaborative efforts across multiple stakeholders in showcasing these capabilities, we have transformed and stored these datasets in Parquet format, utilizing the efficient ZSTD compression method. For access to the code, please reach out to cr.ooi@bloomdata.com.my.

## Step 1: ETL Process (Extract, Transform, Load)

> Processing data from CSV files by extracting, cleaning, transforming, and storing it in a columnar-based database.

### For Contextual Analysis with Large Language Model

<details>
<summary>
Workflow and output of ETL process for product_category_name_translation.csv.
</summary>

- Checking data type consistency, missing values, and duplicate values before processing.
  > - The `product_category_name_translation.csv` file has no missing values and duplicate values across all columns.
  > - Even though no missing values were found, there is a need to check further for data integrity.
  > - Below is the summary of data types, missing, and duplicate values before processing.
    
  ```sql
  +---+-------------------------------+-----------+----------------+------------------+
  |   | Column                        | Data Type | Missing Values | Duplicate Values |
  +---+-------------------------------+-----------+----------------+------------------+
  | 0 | product_category_name         | object    | 0              | 0                |
  | 1 | product_category_name_english | object    | 0              | 0                |
  +---+-------------------------------+-----------+----------------+------------------+
  ```

- Checking and converting columns to object data type to enable replacement of empty strings, null values, and 'nan' values with 'Unknown'.
  > - Column: `product_category_name` already in object data type.
  > - Column: `product_category_name_english` already in object data type.

- Checking and replacing empty strings, null values, and 'nan' values in each object column with 'Unknown'.
  > - No empty strings, null values, or 'nan' values found in column: `product_category_name`.
  > - No empty strings, null values, or 'nan' values found in column: `product_category_name_english`.

- Checking and refining column: `product_category_name` for text with diacritics and special characters.
  >  - No text with diacritics and special characters found in column: `product_category_name`.

- Checking and refining column: `product_category_name_english` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_category_name_english`.

- Since all the columns are unique and they will be merged with `products` DataFrame later, no further data transformation is needed.

- Below are the first 3 rows of the `llm_product_category_name_translation` table after processing and storing in the database.
  > - The complete table is available in `llm_product_category_name_translation.parquet`.

  ```sql
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
  > - The `products.csv` file has some missing values and contains duplicate values in most of the columns.
  > - Below is the summary of data types, missing, and duplicate values before processing.

  ```sql
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

- Merge `products` DataFrame with `llm_product_category_name_translation` table to update product category names from Portugues to English.

- Checking data type consistency, missing values, and duplicate values after joining.
  > - The merged DataFrame shows an increase in missing and duplicate values, particularly in the `product_category_name_english` column.
  > - Below is the summary of data types, missing, and duplicate values after joining.

  ```sql
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
  > - Column: `product_id` already in object data type.
  > - Column: `product_category_name_english` already in object data type.
  > - Converted column: `product_name_lenght` from float64 to object data type.
  > - Converted column: `product_description_lenght` from float64 to object data type.
  > - Converted column: `product_photos_qty` from float64 to object data type.
  > - Converted column: `product_weight_g` from float64 to object data type.
  > - Converted column: `product_length_cm` from float64 to object data type.
  > - Converted column: `product_height_cm` from float64 to object data type.
  > - Converted column: `product_width_cm` from float64 to object data type.

- Checking data type consistency, missing values, and duplicate values after converting data types.
  > - The DataFrame now shows missing values only in the `product_category_name_english` column. Duplicate values remain the same.
  > - There is a need to check further for data integrity.
  > - Below is the summary of data types, missing, and duplicate values after converting data types.

  ```sql
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
  > - No empty strings, null values, or 'nan' values found in column: `product_id`.
  > - Empty strings, null values, or 'nan' values found in column: `product_category_name_english`. Replaced with 'Unknown'.
  > - Empty strings, null values, or 'nan' values found in column: `product_name_lenght`. Replaced with 'Unknown'.
  > - Empty strings, null values, or 'nan' values found in column: `product_description_lenght`. Replaced with 'Unknown'.
  > - Empty strings, null values, or 'nan' values found in column: `product_photos_qty`. Replaced with 'Unknown'.
  > - Empty strings, null values, or 'nan' values found in column: `product_weight_g`. Replaced with 'Unknown'.
  > - Empty strings, null values, or 'nan' values found in column: `product_length_cm`. Replaced with 'Unknown'.
  > - Empty strings, null values, or 'nan' values found in column: `product_height_cm`. Replaced with 'Unknown'.
  > - Empty strings, null values, or 'nan' values found in column: `product_width_cm`. Replaced with 'Unknown'.

- Checking and refining column: `product_id` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_id`.

- Checking and refining column: `product_category_name_english` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_category_name_english`.

- Checking and refining column: `product_name_lenght` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_name_lenght`.

- Checking and refining column: `product_description_lenght` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_description_lenght`.

- Checking and refining column: `product_photos_qty` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_photos_qty`.

- Checking and refining column: `product_weight_g` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_weight_g`.

- Checking and refining column: `product_length_cm` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_length_cm`.

- Checking and refining column: `product_height_cm` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_height_cm`.

- Checking and refining column: `product_width_cm` for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `product_width_cm`.

- Checking the validity of duplicate values
  > - First 3 duplicate entries in column: `product_category_name_english`:

  ```sql
  +---+-------------------------------+------------------+
  |   | product_category_name_english | duplicates_count |
  +---+-------------------------------+------------------+
  | 0 | baby                          | 918              |
  | 1 | musical_instruments           | 288              |
  | 2 | furniture_decor               | 2656             |
  +---+-------------------------------+------------------+
  ```

  > - First 3 duplicate entries in column: `product_name_lenght`:

  ```sql
  +---+---------------------+------------------+
  |   | product_name_lenght | duplicates_count |
  +---+---------------------+------------------+
  | 0 | 56.0                | 1674             |
  | 1 | 59.0                | 2024             |
  | 2 | 56.0                | 1674             |
  +---+---------------------+------------------+
  ```

  > - First 3 duplicate entries in column: `product_description_lenght`:

  ```sql
  +---+----------------------------+------------------+
  |   | product_description_lenght | duplicates_count |
  +---+----------------------------+------------------+
  | 0 | 206.0                      | 24               |
  | 1 | 509.0                      | 39               |
  | 2 | 402.0                      | 34               |
  +---+----------------------------+------------------+
  ```

  > - First 3 duplicate entries in column: `product_photos_qty`

  ```sql
  +---+--------------------+------------------+
  |   | product_photos_qty | duplicates_count |
  +---+--------------------+------------------+
  | 0 | 1.0                | 16488            |
  | 1 | 1.0                | 16488            |
  | 2 | 1.0                | 16488            |
  +---+--------------------+------------------+
  ```

  > - First 3 duplicate entries in column: `product_weight_g`

  ```sql
  +---+------------------+------------------+
  |   | product_weight_g | duplicates_count |
  +---+------------------+------------------+
  | 0 | 600.0            | 956              |
  | 1 | 200.0            | 2083             |
  | 2 | 400.0            | 1205             |
  +---+------------------+------------------+
  ```

  > - First 3 duplicate entries in column: `product_length_cm`

  ```sql
  +---+-------------------+------------------+
  |   | product_length_cm | duplicates_count |
  +---+-------------------+------------------+
  | 0 | 16.0              | 5519             |
  | 1 | 17.0              | 1309             |
  | 2 | 17.0              | 1309             |
  +---+-------------------+------------------+
  ```

  > - First 3 duplicate entries in column: `product_height_cm`

  ```sql
  +---+-------------------+------------------+
  |   | product_height_cm | duplicates_count |
  +---+-------------------+------------------+
  | 0 | 10.0              | 2547             |
  | 1 | 10.0              | 2547             |
  | 2 | 7.0               | 1234             |
  +---+-------------------+------------------+
  ```

  > - First 3 duplicate entries in column: `product_width_cm`

  ```sql
  +---+------------------+------------------+
  |   | product_width_cm | duplicates_count |
  +---+------------------+------------------+
  | 0 | 17.0             | 1117             |
  | 1 | 13.0             | 1132             |
  | 2 | 17.0             | 1117             |
  +---+------------------+------------------+
  ```

- Checking data type consistency, missing values, and duplicate values after data cleaning and transformation..
  > - The DataFrame now shows no missing values. Duplicate values remain the same.
  > - The duplicate values are acceptable since they are valid in this case.
  > - Below is the summary of data types, missing, and duplicate values after data cleaning and transformation.

  ```sql
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

- Below are the first 3 rows of the `llm_products` table after processing and storing in the database.
  > - The complete table is available in `llm_products.parquet`.

  ```sql
  +---+----------------------------------+-------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
  |   | product_id                       | product_category_name_english | product_name_lenght | product_description_lenght | product_photos_qty | product_weight_g | product_length_cm | product_height_cm | product_width_cm |
  +---+----------------------------------+-------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
  | 0 | 1e9e8ef04dbcff4541ed26657ea517e5 | perfumery                     | 40.0                | 287.0                      | 1.0                | 225.0            | 16.0              | 10.0              | 14.0             |
  | 1 | 3aa071139cb16b67ca9e5dea641aaa2f | art                           | 44.0                | 276.0                      | 1.0                | 1000.0           | 30.0              | 18.0              | 20.0             |
  | 2 | 96bd76ec8810374ed1b65e291975717f | sports_leisure                | 46.0                | 250.0                      | 1.0                | 154.0            | 18.0              | 9.0               | 15.0             |
  +---+----------------------------------+-------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
  ```
  
</details>

<details>
<summary>
Workflow and output of ETL process for products.csv.
</summary>

- Checking data type consistency, missing values, and duplicate values before processing.
  > - The `geolocation.csv` file has no missing values but contains duplicate values across all columns.
  > - Below is the summary of data types, missing, and duplicate values before processing.

  ```sql
  +---+-----------------------------+-----------+----------------+------------------+
  |   | Column                      | Data Type | Missing Values | Duplicate Values |
  +---+-----------------------------+-----------+----------------+------------------+
  | 0 | geolocation_zip_code_prefix | int64     | 0              | 981148           |
  | 1 | geolocation_lat             | float64   | 0              | 282803           |
  | 2 | geolocation_lng             | float64   | 0              | 282550           |
  | 3 | geolocation_city            | object    | 0              | 992152           |
  | 4 | geolocation_state           | object    | 0              | 1000136          |
  +---+-----------------------------+-----------+----------------+------------------+
  ```

- Checking and converting columns to object data type to enable replacement of empty strings, null values, and 'nan' values with 'Unknown'.
  > - Converted column: `geolocation_zip_code_prefix` from int64 to object data type.
  > - Converted column: `geolocation_lat from float64` to object data type.
  > - Converted column: `geolocation_lng from float64` to object data type.
  > - Column: `geolocation_city` already in object data type.
  > - Column: `geolocation_state` already in object data type.

- Checking data type consistency, missing values, and duplicate values after converting data types.
  > - The DataFrame shows that the missing and duplicate values remain unchanged.
  > - There is a need to check further for data integrity.
  > - Below is the summary of data types, missing, and duplicate values after converting data types.

  ```sql
  +---+-----------------------------+-----------+----------------+------------------+
  |   | Column                      | Data Type | Missing Values | Duplicate Values |
  +---+-----------------------------+-----------+----------------+------------------+
  | 0 | geolocation_zip_code_prefix | object    | 0              | 981148           |
  | 1 | geolocation_lat             | object    | 0              | 282803           |
  | 2 | geolocation_lng             | object    | 0              | 282550           |
  | 3 | geolocation_city            | object    | 0              | 992152           |
  | 4 | geolocation_state           | object    | 0              | 1000136          |
  +---+-----------------------------+-----------+----------------+------------------+
  ```

- Checking and replacing empty strings, null values, and 'nan' values in each object column with 'Unknown'.
  > - No empty strings, null values, or 'nan' values found in column: `geolocation_zip_code_prefix`.
  > - No empty strings, null values, or 'nan' values found in column: `geolocation_lat`.
  > - No empty strings, null values, or 'nan' values found in column: `geolocation_lng`.
  > - No empty strings, null values, or 'nan' values found in column: `geolocation_city`.
  > - No empty strings, null values, or 'nan' values found in column: `geolocation_state`.

- Checking and refining column: 'geolocation_zip_code_prefix' for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `geolocation_zip_code_prefix`.

- Checking and refining column: 'geolocation_lat' for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `geolocation_lat`.

- Checking and refining column: 'geolocation_lng' for text with diacritics and special characters.
  > - No text with diacritics and special characters found in column: `geolocation_lng`.

- Checking and refining column: 'geolocation_city' for text with diacritics and special characters.
  > - Below are the first 3 unique texts with diacritics and special characters found in column: `geolocation_city`.
  > - Performing refinement with text-to-text generation model.
  > - The refined column has been renamed to `refined_geolocation_city`.

  ```python
  6 - Original: são paulo, Normalized: sao paulo, Refined: Sao Paulo
  ```

</details>
