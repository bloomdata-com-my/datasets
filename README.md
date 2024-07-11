# Retail and E-Commerce Datasets

[Work in progress]

This repository provides a comprehensive collection of retail and e-commerce datasets, mainly sourced from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). These datasets include detailed customer profiles, geolocation data, order specifics, product details, and seller information, making them ideal for showcasing contextual analysis where Large Language Models excel over conventional Machine Learning approaches. To support collaboration, we have transformed and stored these datasets in Parquet format using the efficient ZSTD compression method. For access to the code, please reach out to cr.ooi@bloomdata.com.my.

## Step 1: ETL Process (Extract, Transform, Load)

> Processing data from CSV files by extracting, cleaning, transforming, and storing it in a columnar-based database.

### For Contextual Analysis with Large Language Model

<details>
<summary>
Workflow and output of ETL process for product_category_name_translation.csv.
</summary>

- Checking data type consistency, missing values, and duplicate values before processing.
  > - The `product_category_name_translation.csv` file has no missing values and duplicate values across all columns.
  > - No missing values were found, but further checks for data integrity are necessary.
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

- Checking and normalizing text with diacritics and special characters in each object column.
  > - No text with diacritics and special characters found in column: `product_category_name`.
  > - No text with diacritics and special characters found in column: `product_category_name_english`.

- All columns are unique and clean. As they will be merged with the `products` DataFrame, no additional data transformation is needed.

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
<br>
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

- Checking and normalizing text with diacritics and special characters in each object column.
  > - No text with diacritics and special characters found in column: `product_id`.
  > - No text with diacritics and special characters found in column: `product_category_name_english`.
  > - No text with diacritics and special characters found in column: `product_name_lenght`.
  > - No text with diacritics and special characters found in column: `product_description_lenght`.
  > - No text with diacritics and special characters found in column: `product_photos_qty`.
  > - No text with diacritics and special characters found in column: `product_weight_g`.
  > - No text with diacritics and special characters found in column: `product_length_cm`.
  > - No text with diacritics and special characters found in column: `product_height_cm`.
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

- Checking data type consistency, missing values, and duplicate values after data cleaning.
  > - The DataFrame now shows no missing values. Duplicate values remain the same.
  > - The duplicate values are acceptable since `product_id` is unique in this case.
  > - Below is the summary of data types, missing, and duplicate values after data cleaning.

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
<br>
</details>

<details>
<summary>
Workflow and output of ETL process for geolocation.csv.
</summary>

- Checking data type consistency, missing values, and duplicate values before processing.
  > - The `geolocation.csv` file has no missing values but contains duplicate values in all columns.
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

- Performed group by operation on `geolocation_zip_code_prefix`, `geolocation_city`, and `geolocation_state`, calculated the mean for `geolocation_lat` and `geolocation_lng`, and renamed these columns to `geolocation_lat_mean` and `geolocation_lng_mean` respectively.
  
- Checking data type consistency, missing values, and duplicate values after grouping by operation and mean calculation.
  > - The grouped by DataFrame with mean calculation shows reduced duplicate values.
  > - Below is the summary of data types, missing, and duplicate values after grouping by operation and mean calculation.
  
  ```sql
  +---+-----------------------------+-----------+----------------+------------------+
  |   | Column                      | Data Type | Missing Values | Duplicate Values |
  +---+-----------------------------+-----------+----------------+------------------+
  | 0 | geolocation_zip_code_prefix | int64     | 0              | 8897             |
  | 1 | geolocation_lat_mean        | float64   | 0              | 61               |
  | 2 | geolocation_lng_mean        | float64   | 0              | 61               |
  | 3 | geolocation_city            | object    | 0              | 19901            |
  | 4 | geolocation_state           | object    | 0              | 27885            |
  +---+-----------------------------+-----------+----------------+------------------+
  ```

- Inspecting the grouped by DataFrame with mean calculation
  > - The grouped by DataFrame with mean calculation highlights inconsistencies in the `geolocation_city` column due to uncleaned data. Variations like `sao paulo` and `são paulo` indicate the reason why duplicate values exist even we have grouped by the DataFrame with mean calculation.
  > - Below is the first 3 rows of the grouped by DataFrame with mean calculation.
  
  ```sql
  +---+-----------------------------+----------------------+----------------------+------------------+-------------------+
  |   | geolocation_zip_code_prefix | geolocation_lat_mean | geolocation_lng_mean | geolocation_city | geolocation_state |
  +---+-----------------------------+----------------------+----------------------+------------------+-------------------+
  | 0 | 1001                        | -23.550214793274414  | -46.634018776332134  | sao paulo        | SP                |
  | 1 | 1001                        | -23.549997981678136  | -46.63406019929013   | são paulo        | SP                |
  | 2 | 1002                        | -23.548437764688757  | -46.63512916227373   | sao paulo        | SP                |
  +---+-----------------------------+----------------------+----------------------+------------------+-------------------+
  ```

  > - Additionally, there are 8011 unique values in `geolocation_city` column, including the variances.
  > - Below shows the first 20 unique values from the `geolocation_city` column of the grouped by DataFrame with mean calculation.
  ```sql
  Unique values in 'geolocation_city' column: 8011
  +----+------------------------+
  |    | geolocation_city       |
  +----+------------------------+
  | 0  | sao paulo              |
  | 1  | são paulo              |
  | 2  | sao bernardo do campo  |
  | 3  | jundiaí                |
  | 4  | taboão da serra        |
  | 5  | sãopaulo               |
  | 6  | sp                     |
  | 7  | sa£o paulo             |
  | 8  | sao jose dos campos    |
  | 9  | osasco                 |
  | 10 | carapicuiba            |
  | 11 | carapicuíba            |
  | 12 | barueri                |
  | 13 | santana de parnaiba    |
  | 14 | santana de parnaíba    |
  | 15 | pirapora do bom jesus  |
  | 16 | jandira                |
  | 17 | itapevi                |
  | 18 | cotia                  |
  | 19 | vargem grande paulista |
  +----+------------------------+
  ```

- Since the original Kaggle repository lacks a complete list of official Brazilian city names, we will utilize OpenAI to refine the `geolocation_city` to `refined_geolocation_city`.
  > - Processing the `list_braziliancities.csv` with embeddings and storing in the database.
  > - Below are the first 3 rows of the `llm_brazil_city_names` table after processing and storing in the database.
  > - The complete table is available in `llm_brazil_city_names.parquet`.

```sql
```

<br>
</details>
