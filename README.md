# Introduction

This repository is designed to facilitate collaboration with multiple parties by providing extracted and transformed datasets.

The original datasets can be found on [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

### Steps for Preparing Outputs

The following steps were taken to prepare the outputs. To optimize performance with our columnar-based database, we flattened the tables instead of using nested JSON structures. Access to the codes can be requested by contacting cr.ooi@bloomdata.com.my. We also saved all the outputs in Parquet files, which are compressed using the ZSTD compression method.

### 1. Reading All CSV Files, Filling Missing Values, and Storing in Database

Below are examples showing the first 3 rows of each table in the database and the missing value count in each column:

#### customers table
```
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
|   | customer_id                      | customer_unique_id               | customer_zip_code_prefix | customer_city         | customer_state |
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
| 0 | 06b8999e2fba1a1fbc88172c00ba8bc7 | 861eff4711a542e4b93843c6dd7febb0 | 14409                    | franca                | SP             |
| 1 | 18955e83d337fd6b2def6b18a428ac77 | 290c77bc529b7ac935b93aa66c333dc3 | 9790                     | sao bernardo do campo | SP             |
| 2 | 4e7b3e00288586ebd08712fdd0374a03 | 060e732b5b29e8181a18229c7b0b2b5e | 1151                     | sao paulo             | SP             |
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
+---+--------------------------+----------------+
|   | Column                   | Missing Values |
+---+--------------------------+----------------+
| 0 | customer_id              |       0        |
| 1 | customer_unique_id       |       0        |
| 2 | customer_zip_code_prefix |       0        |
| 3 | customer_city            |       0        |
| 4 | customer_state           |       0        |
+---+--------------------------+----------------+
```
`customers table` has zero missing value found.

#### geolocation table
```
+---+-----------------------------+--------------------+--------------------+------------------+-------------------+
|   | geolocation_zip_code_prefix | geolocation_lat    | geolocation_lng    | geolocation_city | geolocation_state |
+---+-----------------------------+--------------------+--------------------+------------------+-------------------+
| 0 | 1037                        | -23.54562128115268 | -46.63929204800168 | sao paulo        | SP                |
| 1 | 1046                        | -23.54608112703553 | -46.64482029837157 | sao paulo        | SP                |
| 2 | 1046                        | -23.54612896641469 | -46.64295148361138 | sao paulo        | SP                |
+---+-----------------------------+--------------------+--------------------+------------------+-------------------+
+---+-----------------------------+----------------+
|   | Column                      | Missing Values |
+---+-----------------------------+----------------+
| 0 | geolocation_zip_code_prefix |       0        |
| 1 | geolocation_lat             |       0        |
| 2 | geolocation_lng             |       0        |
| 3 | geolocation_city            |       0        |
| 4 | geolocation_state           |       0        |
+---+-----------------------------+----------------+
```
`geolocation table` has zero missing value found.

#### order_items table
```
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
|   | order_id                         | order_item_id | product_id                       | seller_id                        | shipping_limit_date | price | freight_value |
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
| 0 | 00010242fe8c5a6d1ba2dd792cb16214 | 1             | 4244733e06e7ecb4970a6e2683c13e61 | 48436dade18ac8b2bce089ec2a041202 | 2017-09-19 09:45:35 | 58.9  | 13.29         |
| 1 | 00018f77f2f0320c557190d7a144bdd3 | 1             | e5f2d52b802189ee658865ca93d83a8f | dd7ddc04e1b6c2c614352b383efe2d36 | 2017-05-03 11:05:13 | 239.9 | 19.93         |
| 2 | 000229ec398224ef6ca0657da4fc703e | 1             | c777355d18b72b67abbeef9df44fd0fd | 5b51032eddd242adc84c38acab88f23d | 2018-01-18 14:48:30 | 199.0 | 17.87         |
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
+---+---------------------+----------------+
|   | Column              | Missing Values |
+---+---------------------+----------------+
| 0 | order_id            |       0        |
| 1 | order_item_id       |       0        |
| 2 | product_id          |       0        |
| 3 | seller_id           |       0        |
| 4 | shipping_limit_date |       0        |
| 5 | price               |       0        |
| 6 | freight_value       |       0        |
+---+---------------------+----------------+
```
`order_items table` has zero missing value found.

#### order_payments table
```
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
|   | order_id                         | payment_sequential | payment_type | payment_installments | payment_value |
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
| 0 | b81ef226f3fe1789b1e8b2acac839d17 | 1                  | credit_card  | 8                    | 99.33         |
| 1 | a9810da82917af2d9aefd1278f1dcfa0 | 1                  | credit_card  | 1                    | 24.39         |
| 2 | 25e8ea4e93396b6fa0d3dd708e76c1bd | 1                  | credit_card  | 1                    | 65.71         |
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
+---+----------------------+----------------+
|   | Column               | Missing Values |
+---+----------------------+----------------+
| 0 | order_id             |       0        |
| 1 | payment_sequential   |       0        |
| 2 | payment_type         |       0        |
| 3 | payment_installments |       0        |
| 4 | payment_value        |       0        |
+---+----------------------+----------------+
```
`order_payments table` has zero missing value found.

#### order_reviews.parquet
```
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
|   | review_id                        | order_id                         | review_score | review_comment_title | review_comment_message | review_creation_date | review_answer_timestamp |
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
| 0 | 7bc2406110b926393aa56f80a40eba40 | 73fc7af87114b39712e6da79b0a377eb | 4            | NaN                  | NaN                    | 2018-01-18 00:00:00  | 2018-01-18 21:46:59     |
| 1 | 80e641a11e56f04c1ad469d5645fdfde | a548910a1c6147796b98fdf73dbeba33 | 5            | NaN                  | NaN                    | 2018-03-10 00:00:00  | 2018-03-11 03:05:13     |
| 2 | 228ce5500dc1d8e020d8d1322874b6f0 | f9e4b658b201a9f2ecdecbb34bed034b | 5            | NaN                  | NaN                    | 2018-02-17 00:00:00  | 2018-02-18 14:36:24     |
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
+---+-------------------------+----------------+
|   | Column                  | Missing Values |
+---+-------------------------+----------------+
| 0 | review_id               |       0        |
| 1 | order_id                |       0        |
| 2 | review_score            |       0        |
| 3 | review_comment_title    |       0        |
| 4 | review_comment_message  |       0        |
| 5 | review_creation_date    |       0        |
| 6 | review_answer_timestamp |       0        |
+---+-------------------------+----------------+
```
`order_reviews table` has zero missing value found.

#### orders table
```
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
|   | order_id                         | customer_id                      | order_status | order_purchase_timestamp | order_approved_at   | order_delivered_carrier_date | order_delivered_customer_date | order_estimated_delivery_date |
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
| 0 | e481f51cbdc54678b7cc49136f2d6af7 | 9ef432eb6251297304e76186b10a928d | delivered    | 2017-10-02 10:56:33      | 2017-10-02 11:07:15 | 2017-10-04 19:55:00          | 2017-10-10 21:25:13           | 2017-10-18 00:00:00           |
| 1 | 53cdb2fc8bc7dce0b6741e2150273451 | b0830fb4747a6c6d20dea0b8c802d7ef | delivered    | 2018-07-24 20:41:37      | 2018-07-26 03:24:27 | 2018-07-26 14:31:00          | 2018-08-07 15:27:45           | 2018-08-13 00:00:00           |
| 2 | 47770eb9100c2d0c44946d9cf07ec65d | 41ce2a54c0b03bf3443c3d931a367089 | delivered    | 2018-08-08 08:38:49      | 2018-08-08 08:55:23 | 2018-08-08 13:50:00          | 2018-08-17 18:06:29           | 2018-09-04 00:00:00           |
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
+---+-------------------------------+----------------+
|   | Column                        | Missing Values |
+---+-------------------------------+----------------+
| 0 | order_id                      |       0        |
| 1 | customer_id                   |       0        |
| 2 | order_status                  |       0        |
| 3 | order_purchase_timestamp      |       0        |
| 4 | order_approved_at             |       0        |
| 5 | order_delivered_carrier_date  |       0        |
| 6 | order_delivered_customer_date |       0        |
| 7 | order_estimated_delivery_date |       0        |
+---+-------------------------------+----------------+
```
`orders table` has zero missing value found.

#### products table
```
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
|   | product_id                       | product_category_name | product_name_lenght | product_description_lenght | product_photos_qty | product_weight_g | product_length_cm | product_height_cm | product_width_cm |
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
| 0 | 1e9e8ef04dbcff4541ed26657ea517e5 | perfumaria            | 40.0                | 287.0                      | 1.0                | 225.0            | 16.0              | 10.0              | 14.0             |
| 1 | 3aa071139cb16b67ca9e5dea641aaa2f | artes                 | 44.0                | 276.0                      | 1.0                | 1000.0           | 30.0              | 18.0              | 20.0             |
| 2 | 96bd76ec8810374ed1b65e291975717f | esporte_lazer         | 46.0                | 250.0                      | 1.0                | 154.0            | 18.0              | 9.0               | 15.0             |
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
+---+----------------------------+----------------+
|   | Column                     | Missing Values |
+---+----------------------------+----------------+
| 0 | product_id                 |       0        |
| 1 | product_category_name      |       0        |
| 2 | product_name_lenght        |      610       |
| 3 | product_description_lenght |      610       |
| 4 | product_photos_qty         |      610       |
| 5 | product_weight_g           |       2        |
| 6 | product_length_cm          |       2        |
| 7 | product_height_cm          |       2        |
| 8 | product_width_cm           |       2        |
+---+----------------------------+----------------+
```
`products table` has some missing values found but these could be due to not all products having complete product details in the table.

#### product_category_name_translation table
```
+---+------------------------+-------------------------------+
|   | product_category_name  | product_category_name_english |
+---+------------------------+-------------------------------+
| 0 | beleza_saude           | health_beauty                 |
| 1 | informatica_acessorios | computers_accessories         |
| 2 | automotivo             | auto                          |
+---+------------------------+-------------------------------+
+---+-------------------------------+----------------+
|   | Column                        | Missing Values |
+---+-------------------------------+----------------+
| 0 | product_category_name         |       0        |
| 1 | product_category_name_english |       0        |
+---+-------------------------------+----------------+
```
`product_category_name_translation table` has zero missing value found.

#### sellers table
```
+---+----------------------------------+------------------------+----------------+--------------+
|   | seller_id                        | seller_zip_code_prefix | seller_city    | seller_state |
+---+----------------------------------+------------------------+----------------+--------------+
| 0 | 3442f8959a84dea7ee197c632cb2df15 | 13023                  | campinas       | SP           |
| 1 | d1b65fc7debc3361ea86b5f14c68d2e2 | 13844                  | mogi guacu     | SP           |
| 2 | ce3ad9de960102d0677a81f5d0bb7b2d | 20031                  | rio de janeiro | RJ           |
+---+----------------------------------+------------------------+----------------+--------------+
+---+------------------------+----------------+
|   | Column                 | Missing Values |
+---+------------------------+----------------+
| 0 | seller_id              |       0        |
| 1 | seller_zip_code_prefix |       0        |
| 2 | seller_city            |       0        |
| 3 | seller_state           |       0        |
+---+------------------------+----------------+
```
`sellers table` has zero missing value found.

### 2. Adding Translated English Names along with the Original Product Category Names Using the Products and Product_Category_Name_Translation Tables

Below is an example showing the first 3 rows of the transformed_products table in the database and the missing value count in each column:

#### transformed_products table
```
+---+----------------------------------+-----------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
|   | product_id                       | product_category_name | translated_product_category_name | product_name_lenght | product_description_lenght | product_photos_qty | product_weight_g | product_length_cm | product_height_cm | product_width_cm |
+---+----------------------------------+-----------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
| 0 | 1e9e8ef04dbcff4541ed26657ea517e5 | perfumaria            | perfumery                        | 40.0                | 287.0                      | 1.0                | 225.0            | 16.0              | 10.0              | 14.0             |
| 1 | 3aa071139cb16b67ca9e5dea641aaa2f | artes                 | art                              | 44.0                | 276.0                      | 1.0                | 1000.0           | 30.0              | 18.0              | 20.0             |
| 2 | 96bd76ec8810374ed1b65e291975717f | esporte_lazer         | sports_leisure                   | 46.0                | 250.0                      | 1.0                | 154.0            | 18.0              | 9.0               | 15.0             |
+---+----------------------------------+-----------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
+---+----------------------------------+----------------+
|   | Column                           | Missing Values |
+---+----------------------------------+----------------+
| 0 | product_id                       |       0        |
| 1 | product_category_name            |       0        |
| 2 | translated_product_category_name |       0        |
| 3 | product_name_lenght              |      610       |
| 4 | product_description_lenght       |      610       |
| 5 | product_photos_qty               |      610       |
| 6 | product_weight_g                 |       2        |
| 7 | product_length_cm                |       2        |
| 8 | product_height_cm                |       2        |
| 9 | product_width_cm                 |       2        |
+---+----------------------------------+----------------+
```
`transformed_products table` has the same missing values as the `products table`.

### 3. Adding Aggregated Latitude and Longitude Columns (Mean) to the Customers and Sellers Tables Using the Geolocation Table
Below are the examples showing the first 3 rows of the transformed_customers and transformed_sellers tables in the database and the missing value count in each column:

#### transformed_customers table
```
+---+----------------------------------+----------------------------------+--------------------------+---------------------+---------------------+-----------------------+----------------+
|   | customer_id                      | customer_unique_id               | customer_zip_code_prefix | customer_lat        | customer_lng        | customer_city         | customer_state |
+---+----------------------------------+----------------------------------+--------------------------+---------------------+---------------------+-----------------------+----------------+
| 0 | 06b8999e2fba1a1fbc88172c00ba8bc7 | 861eff4711a542e4b93843c6dd7febb0 | 14409                    | -20.498488755380297 | -47.396929485900976 | franca                | SP             |
| 1 | 18955e83d337fd6b2def6b18a428ac77 | 290c77bc529b7ac935b93aa66c333dc3 | 9790                     | -23.72799221530055  | -46.54284778999086  | sao bernardo do campo | SP             |
| 2 | 4e7b3e00288586ebd08712fdd0374a03 | 060e732b5b29e8181a18229c7b0b2b5e | 1151                     | -23.531641584683715 | -46.656288753249086 | sao paulo             | SP             |
+---+----------------------------------+----------------------------------+--------------------------+---------------------+---------------------+-----------------------+----------------+
+---+--------------------------+----------------+
|   | Column                   | Missing Values |
+---+--------------------------+----------------+
| 0 | customer_id              |       0        |
| 1 | customer_unique_id       |       0        |
| 2 | customer_zip_code_prefix |       0        |
| 3 | customer_lat             |      278       |
| 4 | customer_lng             |      278       |
| 5 | customer_city            |       0        |
| 6 | customer_state           |       0        |
+---+--------------------------+----------------+
```
`transformed_customers table` has some missing values. These missing values occur because certain customer_zip_code_prefix entries in the `customers table` were not found in the `geolocation table`.

#### transformed_sellers table
```
+---+----------------------------------+------------------------+---------------------+---------------------+----------------+--------------+
|   | seller_id                        | seller_zip_code_prefix | seller_lat          | seller_lng          | seller_city    | seller_state |
+---+----------------------------------+------------------------+---------------------+---------------------+----------------+--------------+
| 0 | 3442f8959a84dea7ee197c632cb2df15 | 13023                  | -22.89384803253408  | -47.06133702244195  | campinas       | SP           |
| 1 | d1b65fc7debc3361ea86b5f14c68d2e2 | 13844                  | -22.38343651404282  | -46.947926542619655 | mogi guacu     | SP           |
| 2 | ce3ad9de960102d0677a81f5d0bb7b2d | 20031                  | -22.909572437655488 | -43.177703112986904 | rio de janeiro | RJ           |
+---+----------------------------------+------------------------+---------------------+---------------------+----------------+--------------+
+---+------------------------+----------------+
|   | Column                 | Missing Values |
+---+------------------------+----------------+
| 0 | seller_id              |       0        |
| 1 | seller_zip_code_prefix |       0        |
| 2 | seller_lat             |       7        |
| 3 | seller_lng             |       7        |
| 4 | seller_city            |       0        |
| 5 | seller_state           |       0        |
+---+------------------------+----------------+
```
`transformed_sellers table` has some missing values. These missing values occur because certain seller_zip_code_prefix entries in the `sellers table` were not found in the `geolocation table`.

### 4. Grouping by Order ID Column in the Order Items Table, Adding Additional Features, and Merging with Transformed Products and Transformed Sellers Tables
Below is an the example showing the first 3 rows of the transformed_order_items tables in the database:

`transformed_order_items table` or `transformed_order_items.parquet`
```
+---+----------------------------------+------------------+----------------------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+----------------------------------+------------------------+---------------------+---------------------+---------------+--------------+---------------------+-------+-------------+---------------+---------------------+
|   | order_id                         | order_item_count | product_id                       | translated_product_category_name | product_name_lenght | product_description_lenght | product_photos_qty | product_weight_g | product_length_cm | product_height_cm | product_width_cm | seller_id                        | seller_zip_code_prefix | seller_lat          | seller_lng          | seller_city   | seller_state | shipping_limit_date | price | total_price | freight_value | total_freight_value |
+---+----------------------------------+------------------+----------------------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+----------------------------------+------------------------+---------------------+---------------------+---------------+--------------+---------------------+-------+-------------+---------------+---------------------+
| 0 | 00010242fe8c5a6d1ba2dd792cb16214 | 1                | 4244733e06e7ecb4970a6e2683c13e61 | cool_stuff                       | 58.0                | 598.0                      | 4.0                | 650.0            | 28.0              | 9.0               | 14.0             | 48436dade18ac8b2bce089ec2a041202 | 27277                  | -22.496952789883665 | -44.127492009916175 | volta redonda | SP           | 2017-09-19 09:45:35 | 58.9  | 58.9        | 13.29         | 13.29               |
| 1 | 00018f77f2f0320c557190d7a144bdd3 | 1                | e5f2d52b802189ee658865ca93d83a8f | pet_shop                         | 56.0                | 239.0                      | 2.0                | 30000.0          | 50.0              | 30.0              | 40.0             | dd7ddc04e1b6c2c614352b383efe2d36 | 3471                   | -23.565095619050492 | -46.51856509433426  | sao paulo     | SP           | 2017-05-03 11:05:13 | 239.9 | 239.9       | 19.93         | 19.93               |
| 2 | 000229ec398224ef6ca0657da4fc703e | 1                | c777355d18b72b67abbeef9df44fd0fd | furniture_decor                  | 59.0                | 695.0                      | 2.0                | 3050.0           | 33.0              | 13.0              | 33.0             | 5b51032eddd242adc84c38acab88f23d | 37564                  | -22.26258413023529  | -46.1711239405763   | borda da mata | MG           | 2018-01-18 14:48:30 | 199.0 | 199.0       | 17.87         | 17.87               |
+---+----------------------------------+------------------+----------------------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+----------------------------------+------------------------+---------------------+---------------------+---------------+--------------+---------------------+-------+-------------+---------------+---------------------+
```
