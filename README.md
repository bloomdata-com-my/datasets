# Retail and E-Commerce Datasets

This repository offers a comprehensive collection of retail and e-commerce datasets, ideal for use in large language model projects. These datasets, sourced from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), include detailed customer information, geolocation data, order details, product information, and seller information. To facilitate collaboration across multiple parties, we transformed these datasets, saved them in Parquet files, and compressed them using the ZSTD compression method. For access to the code, please contact cr.ooi@bloomdata.com.my.

## Step 1: ETL Process (Extract, Transform, Load)

> Extracting data from CSV files, handling missing and duplicate values, transforming data, and storing it in a columnar-based database.

Output of `python3 product_category_name_translation.py`:

- The `product_category_name_translation.csv` file has no missing values and duplicate values across all the columns.
- No empty strings or null values found in column: `product_category_name`.
- No empty strings or null values found in column: `product_category_name_english`.
- No non-standard English characters found in column: `product_category_name`.
- No non-standard English characters found in column: `product_category_name_english`.
- Since `product_category_name` and `product_category_name_english` columns are unique and they will be joined with `products` table later, no further data transformation is needed.

```
Summary of Data Types, Missing, and Duplicate Values:
+---+-------------------------------+-----------+----------------+------------------+
|   | Column                        | Data Type | Missing Values | Duplicate Values |
+---+-------------------------------+-----------+----------------+------------------+
| 0 | product_category_name         | object    | 0              | 0                |
| 1 | product_category_name_english | object    | 0              | 0                |
+---+-------------------------------+-----------+----------------+------------------+
```

- Below are the first 3 rows of the `product_category_name_translation` table after processing and storing in the database.
- The complete table is available in `product_category_name_translation.parquet`.

```
First 3 rows of product_category_name_translation table:
+---+------------------------+-------------------------------+
|   | product_category_name  | product_category_name_english |
+---+------------------------+-------------------------------+
| 0 | beleza_saude           | health_beauty                 |
| 1 | informatica_acessorios | computers_accessories         |
| 2 | automotivo             | auto                          |
+---+------------------------+-------------------------------+
```

Output of `python3 products.py`:

- The `products.csv` file has some missing values and contains duplicate values in several columns. 
- No empty strings or null values found in column: `product_id`.
- Empty strings or null values found in column: `product_category_name`.
- Performed empty strings and null values replacement with `unknown` on the following column: `product_category_name`.
- No non-standard English characters found in column: `product_id`.
- No non-standard English characters found in column: `product_category_name`.
- Since `product_id` and `product_category_name_english` columns are unique and they will be joined with `products` table later, no further data transformation is needed.
- Since 'product_id' column is unique, no further data cleaning is needed.

```
These missing values have been filled with 'nan' before storing into the database.
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
```
- Below are the first 3 rows of the 'products' table after processing and storing in the database. The complete table is available in 'products.parquet'.
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
|   | product_id                       | product_category_name | product_name_lenght | product_description_lenght | product_photos_qty | product_weight_g | product_length_cm | product_height_cm | product_width_cm |
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
| 0 | 1e9e8ef04dbcff4541ed26657ea517e5 | perfumaria            | 40.0                | 287.0                      | 1.0                | 225.0            | 16.0              | 10.0              | 14.0             |
| 1 | 3aa071139cb16b67ca9e5dea641aaa2f | artes                 | 44.0                | 276.0                      | 1.0                | 1000.0           | 30.0              | 18.0              | 20.0             |
| 2 | 96bd76ec8810374ed1b65e291975717f | esporte_lazer         | 46.0                | 250.0                      | 1.0                | 154.0            | 18.0              | 9.0               | 15.0             |
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
```
