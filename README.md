# Retail and E-Commerce Datasets

This repository offers a comprehensive collection of retail and e-commerce datasets, ideal for data analysis, machine learning, and large language model projects. These datasets, sourced from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), include detailed customer information, geolocation data, order details, product information, and seller information. To facilitate collaboration across multiple parties, we transformed these datasets, saved them in Parquet files, and compressed them using the ZSTD compression method. For access to the code, please contact cr.ooi@bloomdata.com.my.

### Step 1: Data Preparation
#### Reading CSV Files, Handling Missing and Duplicate Values, Data Cleaning, and Storing in a Columnar-Based Database
We begin by processing the simpler CSV files such as customers.csv, geolocation.csv, products.csv, product_category_name_translation.csv, and sellers.csv. Here are the outputs:

#### customers table:
```
- The 'customers.csv' file has no missing values but contains duplicate values in several columns.
+---+--------------------------+-----------+----------------+------------------+
|   | Column                   | Data Type | Missing Values | Duplicate Values |
+---+--------------------------+-----------+----------------+------------------+
| 0 | customer_id              | object    | 0              | 0                |
| 1 | customer_unique_id       | object    | 0              | 3345             |
| 2 | customer_zip_code_prefix | int64     | 0              | 84447            |
| 3 | customer_city            | object    | 0              | 95322            |
| 4 | customer_state           | object    | 0              | 99414            |
+---+--------------------------+-----------+----------------+------------------+

- No empty strings or null values found in column: 'customer_id'.
- No empty strings or null values found in column: 'customer_unique_id'.
- No empty strings or null values found in column: 'customer_city'.
- No empty strings or null values found in column: 'customer_state'.
- No non-standard English characters found in column: 'customer_id'.
- No non-standard English characters found in column: 'customer_unique_id'.
- No non-standard English characters found in column: 'customer_city'.
- No non-standard English characters found in column: 'customer_state'.
- Since 'customer_id' column is unique, no further data cleaning is needed.

- Below are the first 3 rows of the 'customers' table after processing and storing in the database. The complete table is available in 'customers.parquet'.
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
|   | customer_id                      | customer_unique_id               | customer_zip_code_prefix | customer_city         | customer_state |
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
| 0 | 06b8999e2fba1a1fbc88172c00ba8bc7 | 861eff4711a542e4b93843c6dd7febb0 | 14409                    | franca                | SP             |
| 1 | 18955e83d337fd6b2def6b18a428ac77 | 290c77bc529b7ac935b93aa66c333dc3 | 9790                     | sao bernardo do campo | SP             |
| 2 | 4e7b3e00288586ebd08712fdd0374a03 | 060e732b5b29e8181a18229c7b0b2b5e | 1151                     | sao paulo             | SP             |
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
```

#### geolocation table:
```
- The 'geolocation.csv' file has no missing values but contains duplicate values in all the columns.
+---+-----------------------------+-----------+----------------+------------------+
|   | Column                      | Data Type | Missing Values | Duplicate Values |
+---+-----------------------------+-----------+----------------+------------------+
| 0 | geolocation_zip_code_prefix | int64     | 0              | 981148           |
| 1 | geolocation_lat             | float64   | 0              | 282803           |
| 2 | geolocation_lng             | float64   | 0              | 282550           |
| 3 | geolocation_city            | object    | 0              | 992152           |
| 4 | geolocation_state           | object    | 0              | 1000136          |
+---+-----------------------------+-----------+----------------+------------------+

- No empty strings or null values found in column: 'geolocation_city'.
- No empty strings or null values found in column: 'geolocation_state'.
- Non-standard English characters found in column: 'geolocation_city'.
- Performed normalization on the following column: 'geolocation_city'.
- No non-standard English characters found in column: 'geolocation_state'.
- Since 'geolocation_zip_code_prefix' is considered the key in this table and duplicate values are found across several columns that will be used for joining tables in later stages, data cleaning is needed.
- Performed group by operation on 'geolocation_zip_code_prefix', calculated the mean for 'geolocation_lat' and 'geolocation_lng', and renamed these columns to 'geolocation_lat_mean' and 'geolocation_lng_mean' respectively.

- Below are the first 3 rows of the 'geolocation' table after processing and storing in the database. The complete table is available in 'geolocation.parquet'.
+---+-----------------------------+----------------------+----------------------+-----------------------------+-------------------+
|   | geolocation_zip_code_prefix | geolocation_lat_mean | geolocation_lng_mean | normalized_geolocation_city | geolocation_state |
+---+-----------------------------+----------------------+----------------------+-----------------------------+-------------------+
| 0 | 1001                        | -23.550189776551765  | -46.6340235559042    | sao paulo                   | SP                |
| 1 | 1002                        | -23.54814573176355   | -46.63497921074498   | sao paulo                   | SP                |
| 2 | 1003                        | -23.54899372481316   | -46.635731309975874  | sao paulo                   | SP                |
+---+-----------------------------+----------------------+----------------------+-----------------------------+-------------------+
```

#### products table:
```
- The 'products.csv' file has some missing values and contains duplicate values in several columns. These missing values have been filled with 'nan' before storing into the database.
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

- No empty strings or null values found in column: 'product_id'.
- Empty strings or null values found in column: 'product_category_name'.
- Performed NaN replacement on the following column: 'product_category_name'.
- No non-standard English characters found in column: 'product_id'.
- No non-standard English characters found in column: 'product_category_name'.
- Since 'product_id' column is unique, no further data cleaning is needed.

- Below are the first 3 rows of the 'products' table after processing and storing in the database. The complete table is available in 'products.parquet'.
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
|   | product_id                       | product_category_name | product_name_lenght | product_description_lenght | product_photos_qty | product_weight_g | product_length_cm | product_height_cm | product_width_cm |
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
| 0 | 1e9e8ef04dbcff4541ed26657ea517e5 | perfumaria            | 40.0                | 287.0                      | 1.0                | 225.0            | 16.0              | 10.0              | 14.0             |
| 1 | 3aa071139cb16b67ca9e5dea641aaa2f | artes                 | 44.0                | 276.0                      | 1.0                | 1000.0           | 30.0              | 18.0              | 20.0             |
| 2 | 96bd76ec8810374ed1b65e291975717f | esporte_lazer         | 46.0                | 250.0                      | 1.0                | 154.0            | 18.0              | 9.0               | 15.0             |
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
```

#### product_category_name_translation table:
```
- The 'product_category_name_translation.csv' file has no missing values and duplicate values across all the columns.
+---+-------------------------------+-----------+----------------+------------------+
|   | Column                        | Data Type | Missing Values | Duplicate Values |
+---+-------------------------------+-----------+----------------+------------------+
| 0 | product_category_name         | object    | 0              | 0                |
| 1 | product_category_name_english | object    | 0              | 0                |
+---+-------------------------------+-----------+----------------+------------------+

- No empty strings or null values found in column: 'product_category_name'.
- No empty strings or null values found in column: 'product_category_name_english'.
- No non-standard English characters found in column: 'product_category_name'.
- No non-standard English characters found in column: 'product_category_name_english'.
- Since 'product_category_name' and 'product_category_name_english' columns are unique, no further data cleaning is needed.

- Below are the first 3 rows of the 'product_category_name_translation' table after processing and storing in the database. The complete table is available in 'product_category_name_translation.parquet'.
+---+------------------------+-------------------------------+
|   | product_category_name  | product_category_name_english |
+---+------------------------+-------------------------------+
| 0 | beleza_saude           | health_beauty                 |
| 1 | informatica_acessorios | computers_accessories         |
| 2 | automotivo             | auto                          |
+---+------------------------+-------------------------------+
```

#### sellers table:
```
- The 'sellers.csv' file has no missing values but contains duplicate values in several columns.
+---+------------------------+-----------+----------------+------------------+
|   | Column                 | Data Type | Missing Values | Duplicate Values |
+---+------------------------+-----------+----------------+------------------+
| 0 | seller_id              | object    | 0              | 0                |
| 1 | seller_zip_code_prefix | int64     | 0              | 849              |
| 2 | seller_city            | object    | 0              | 2484             |
| 3 | seller_state           | object    | 0              | 3072             |
+---+------------------------+-----------+----------------+------------------+

- No empty strings or null values found in column: 'seller_id'.
- No empty strings or null values found in column: 'seller_city'.
- No empty strings or null values found in column: 'seller_state'.
- No non-standard English characters found in column: 'seller_id'.
- Non-standard English characters found in column: 'seller_city'.
- Performed normalization on the following column: 'seller_city'.
- No non-standard English characters found in column: 'seller_state'.
- Since 'seller_id' column is unique, no further data cleaning is needed.

- Below are the first 3 rows of the 'sellers' table after processing and storing in the database. The complete table is available in 'sellers.parquet'.
+---+----------------------------------+------------------------+------------------------+--------------+
|   | seller_id                        | seller_zip_code_prefix | normalized_seller_city | seller_state |
+---+----------------------------------+------------------------+------------------------+--------------+
| 0 | 3442f8959a84dea7ee197c632cb2df15 | 13023                  | campinas               | SP           |
| 1 | d1b65fc7debc3361ea86b5f14c68d2e2 | 13844                  | mogi guacu             | SP           |
| 2 | ce3ad9de960102d0677a81f5d0bb7b2d | 20031                  | rio de janeiro         | RJ           |
+---+----------------------------------+------------------------+------------------------+--------------+
```

#### order_items table:
```
- The 'order_items.csv' file has no missing values but contains duplicate values across all the columns.
+---+---------------------+-----------+----------------+------------------+
|   | Column              | Data Type | Missing Values | Duplicate Values |
+---+---------------------+-----------+----------------+------------------+
| 0 | order_id            | object    | 0              | 13984            |
| 1 | order_item_id       | int64     | 0              | 112629           |
| 2 | product_id          | object    | 0              | 79699            |
| 3 | seller_id           | object    | 0              | 109555           |
| 4 | shipping_limit_date | object    | 0              | 19332            |
| 5 | price               | float64   | 0              | 106682           |
| 6 | freight_value       | float64   | 0              | 105651           |
+---+---------------------+-----------+----------------+------------------+

No empty strings or null values found in column: 'order_id'.
No empty strings or null values found in column: 'product_id'.
No empty strings or null values found in column: 'seller_id'.
No empty strings or null values found in column: 'shipping_limit_date'.
No non-standard English characters found in column: 'order_id'.
No non-standard English characters found in column: 'product_id'.
No non-standard English characters found in column: 'seller_id'.
No non-standard English characters found in column: 'shipping_limit_date'.

showing the first 3 rows of order_items table, complete table can be obtained at order_items.parquet.
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
|   | order_id                         | order_item_id | product_id                       | seller_id                        | shipping_limit_date | price | freight_value |
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
| 0 | 00010242fe8c5a6d1ba2dd792cb16214 | 1             | 4244733e06e7ecb4970a6e2683c13e61 | 48436dade18ac8b2bce089ec2a041202 | 2017-09-19 09:45:35 | 58.9  | 13.29         |
| 1 | 00018f77f2f0320c557190d7a144bdd3 | 1             | e5f2d52b802189ee658865ca93d83a8f | dd7ddc04e1b6c2c614352b383efe2d36 | 2017-05-03 11:05:13 | 239.9 | 19.93         |
| 2 | 000229ec398224ef6ca0657da4fc703e | 1             | c777355d18b72b67abbeef9df44fd0fd | 5b51032eddd242adc84c38acab88f23d | 2018-01-18 14:48:30 | 199.0 | 17.87         |
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
```

#### order_payments table:
```
order_payments csv has zero missing value across all the columns.
+---+----------------------+----------------+
|   | Column               | Missing Values |
+---+----------------------+----------------+
| 0 | order_id             | 0              |
| 1 | payment_sequential   | 0              |
| 2 | payment_type         | 0              |
| 3 | payment_installments | 0              |
| 4 | payment_value        | 0              |
+---+----------------------+----------------+

showing the first 3 rows of order_payments table, complete table can be obtained at order_payments.parquet.
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
|   | order_id                         | payment_sequential | payment_type | payment_installments | payment_value |
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
| 0 | b81ef226f3fe1789b1e8b2acac839d17 | 1                  | credit_card  | 8                    | 99.33         |
| 1 | a9810da82917af2d9aefd1278f1dcfa0 | 1                  | credit_card  | 1                    | 24.39         |
| 2 | 25e8ea4e93396b6fa0d3dd708e76c1bd | 1                  | credit_card  | 1                    | 65.71         |
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
```

#### order_reviews table:
```
order_reviews csv has some missing values. This may be due to not all reviews were filled in the application. These missing values have been filled with 'nan' before storing into the database.
+---+-------------------------+----------------+
|   | Column                  | Missing Values |
+---+-------------------------+----------------+
| 0 | review_id               | 0              |
| 1 | order_id                | 0              |
| 2 | review_score            | 0              |
| 3 | review_comment_title    | 87656          |
| 4 | review_comment_message  | 58247          |
| 5 | review_creation_date    | 0              |
| 6 | review_answer_timestamp | 0              |
+---+-------------------------+----------------+

showing the first 3 rows of order_reviews table, complete table can be obtained at order_reviews.parquet.
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
|   | review_id                        | order_id                         | review_score | review_comment_title | review_comment_message | review_creation_date | review_answer_timestamp |
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
| 0 | 7bc2406110b926393aa56f80a40eba40 | 73fc7af87114b39712e6da79b0a377eb | 4            | nan                  | nan                    | 2018-01-18 00:00:00  | 2018-01-18 21:46:59     |
| 1 | 80e641a11e56f04c1ad469d5645fdfde | a548910a1c6147796b98fdf73dbeba33 | 5            | nan                  | nan                    | 2018-03-10 00:00:00  | 2018-03-11 03:05:13     |
| 2 | 228ce5500dc1d8e020d8d1322874b6f0 | f9e4b658b201a9f2ecdecbb34bed034b | 5            | nan                  | nan                    | 2018-02-17 00:00:00  | 2018-02-18 14:36:24     |
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
```

#### orders table:
```
orders csv has some missing values. This may be due to certain orders being in different stages of the order status or not being updated. These missing values have been filled with 'nan' before storing into the database.
+---+-------------------------------+----------------+
|   | Column                        | Missing Values |
+---+-------------------------------+----------------+
| 0 | order_id                      | 0              |
| 1 | customer_id                   | 0              |
| 2 | order_status                  | 0              |
| 3 | order_purchase_timestamp      | 0              |
| 4 | order_approved_at             | 160            |
| 5 | order_delivered_carrier_date  | 1783           |
| 6 | order_delivered_customer_date | 2965           |
| 7 | order_estimated_delivery_date | 0              |
+---+-------------------------------+----------------+

showing the first 3 rows of orders table, complete table can be obtained at orders.parquet.
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
|   | order_id                         | customer_id                      | order_status | order_purchase_timestamp | order_approved_at   | order_delivered_carrier_date | order_delivered_customer_date | order_estimated_delivery_date |
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
| 0 | e481f51cbdc54678b7cc49136f2d6af7 | 9ef432eb6251297304e76186b10a928d | delivered    | 2017-10-02 10:56:33      | 2017-10-02 11:07:15 | 2017-10-04 19:55:00          | 2017-10-10 21:25:13           | 2017-10-18 00:00:00           |
| 1 | 53cdb2fc8bc7dce0b6741e2150273451 | b0830fb4747a6c6d20dea0b8c802d7ef | delivered    | 2018-07-24 20:41:37      | 2018-07-26 03:24:27 | 2018-07-26 14:31:00          | 2018-08-07 15:27:45           | 2018-08-13 00:00:00           |
| 2 | 47770eb9100c2d0c44946d9cf07ec65d | 41ce2a54c0b03bf3443c3d931a367089 | delivered    | 2018-08-08 08:38:49      | 2018-08-08 08:55:23 | 2018-08-08 13:50:00          | 2018-08-17 18:06:29           | 2018-09-04 00:00:00           |
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
```
