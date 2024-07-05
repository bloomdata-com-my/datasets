# Introduction

This repository is designed to facilitate collaboration with multiple parties by providing extracted and transformed datasets.

We also saved all the outputs in Parquet files, which are compressed using the ZSTD compression method.

The original datasets can be found on [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

### Steps for Preparing Outputs
The following steps were taken to prepare the outputs. To optimize performance with our columnar-based database, we flattened the tables instead of using nested JSON structures. Access to the codes can be requested by contacting cr.ooi@bloomdata.com.my.

### 1. Reading All CSV Files, Filling Missing Values, and Storing in Database

Below are examples showing the first 3 rows of each table in the database:

`customers table` or `customers.parquet`
```
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
|   | customer_id                      | customer_unique_id               | customer_zip_code_prefix | customer_city         | customer_state |
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
| 0 | 06b8999e2fba1a1fbc88172c00ba8bc7 | 861eff4711a542e4b93843c6dd7febb0 | 14409                    | franca                | SP             |
| 1 | 18955e83d337fd6b2def6b18a428ac77 | 290c77bc529b7ac935b93aa66c333dc3 | 9790                     | sao bernardo do campo | SP             |
| 2 | 4e7b3e00288586ebd08712fdd0374a03 | 060e732b5b29e8181a18229c7b0b2b5e | 1151                     | sao paulo             | SP             |
+---+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
```

`geolocation table` or `geolocation.parquet`
```
+---+-----------------------------+--------------------+--------------------+------------------+-------------------+
|   | geolocation_zip_code_prefix | geolocation_lat    | geolocation_lng    | geolocation_city | geolocation_state |
+---+-----------------------------+--------------------+--------------------+------------------+-------------------+
| 0 | 1037                        | -23.54562128115268 | -46.63929204800168 | sao paulo        | SP                |
| 1 | 1046                        | -23.54608112703553 | -46.64482029837157 | sao paulo        | SP                |
| 2 | 1046                        | -23.54612896641469 | -46.64295148361138 | sao paulo        | SP                |
+---+-----------------------------+--------------------+--------------------+------------------+-------------------+
```

`order_items table` or `order_items.parquet`
```
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
|   | order_id                         | order_item_id | product_id                       | seller_id                        | shipping_limit_date | price | freight_value |
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
| 0 | 00010242fe8c5a6d1ba2dd792cb16214 | 1             | 4244733e06e7ecb4970a6e2683c13e61 | 48436dade18ac8b2bce089ec2a041202 | 2017-09-19 09:45:35 | 58.9  | 13.29         |
| 1 | 00018f77f2f0320c557190d7a144bdd3 | 1             | e5f2d52b802189ee658865ca93d83a8f | dd7ddc04e1b6c2c614352b383efe2d36 | 2017-05-03 11:05:13 | 239.9 | 19.93         |
| 2 | 000229ec398224ef6ca0657da4fc703e | 1             | c777355d18b72b67abbeef9df44fd0fd | 5b51032eddd242adc84c38acab88f23d | 2018-01-18 14:48:30 | 199.0 | 17.87         |
+---+----------------------------------+---------------+----------------------------------+----------------------------------+---------------------+-------+---------------+
```

`order_payments table` or `order_payments.parquet`
```
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
|   | order_id                         | payment_sequential | payment_type | payment_installments | payment_value |
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
| 0 | b81ef226f3fe1789b1e8b2acac839d17 | 1                  | credit_card  | 8                    | 99.33         |
| 1 | a9810da82917af2d9aefd1278f1dcfa0 | 1                  | credit_card  | 1                    | 24.39         |
| 2 | 25e8ea4e93396b6fa0d3dd708e76c1bd | 1                  | credit_card  | 1                    | 65.71         |
+---+----------------------------------+--------------------+--------------+----------------------+---------------+
```

`order_reviews.parquet`
```
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
|   | review_id                        | order_id                         | review_score | review_comment_title | review_comment_message | review_creation_date | review_answer_timestamp |
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
| 0 | 7bc2406110b926393aa56f80a40eba40 | 73fc7af87114b39712e6da79b0a377eb | 4            | NaN                  | NaN                    | 2018-01-18 00:00:00  | 2018-01-18 21:46:59     |
| 1 | 80e641a11e56f04c1ad469d5645fdfde | a548910a1c6147796b98fdf73dbeba33 | 5            | NaN                  | NaN                    | 2018-03-10 00:00:00  | 2018-03-11 03:05:13     |
| 2 | 228ce5500dc1d8e020d8d1322874b6f0 | f9e4b658b201a9f2ecdecbb34bed034b | 5            | NaN                  | NaN                    | 2018-02-17 00:00:00  | 2018-02-18 14:36:24     |
+---+----------------------------------+----------------------------------+--------------+----------------------+------------------------+----------------------+-------------------------+
```

`orders table` or `orders.parquet`
```
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
|   | order_id                         | customer_id                      | order_status | order_purchase_timestamp | order_approved_at   | order_delivered_carrier_date | order_delivered_customer_date | order_estimated_delivery_date |
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
| 0 | e481f51cbdc54678b7cc49136f2d6af7 | 9ef432eb6251297304e76186b10a928d | delivered    | 2017-10-02 10:56:33      | 2017-10-02 11:07:15 | 2017-10-04 19:55:00          | 2017-10-10 21:25:13           | 2017-10-18 00:00:00           |
| 1 | 53cdb2fc8bc7dce0b6741e2150273451 | b0830fb4747a6c6d20dea0b8c802d7ef | delivered    | 2018-07-24 20:41:37      | 2018-07-26 03:24:27 | 2018-07-26 14:31:00          | 2018-08-07 15:27:45           | 2018-08-13 00:00:00           |
| 2 | 47770eb9100c2d0c44946d9cf07ec65d | 41ce2a54c0b03bf3443c3d931a367089 | delivered    | 2018-08-08 08:38:49      | 2018-08-08 08:55:23 | 2018-08-08 13:50:00          | 2018-08-17 18:06:29           | 2018-09-04 00:00:00           |
+---+----------------------------------+----------------------------------+--------------+--------------------------+---------------------+------------------------------+-------------------------------+-------------------------------+
```

`products table` or `products.parquet`
```
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
|   | product_id                       | product_category_name | product_name_lenght | product_description_lenght | product_photos_qty | product_weight_g | product_length_cm | product_height_cm | product_width_cm |
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
| 0 | 1e9e8ef04dbcff4541ed26657ea517e5 | perfumaria            | 40.0                | 287.0                      | 1.0                | 225.0            | 16.0              | 10.0              | 14.0             |
| 1 | 3aa071139cb16b67ca9e5dea641aaa2f | artes                 | 44.0                | 276.0                      | 1.0                | 1000.0           | 30.0              | 18.0              | 20.0             |
| 2 | 96bd76ec8810374ed1b65e291975717f | esporte_lazer         | 46.0                | 250.0                      | 1.0                | 154.0            | 18.0              | 9.0               | 15.0             |
+---+----------------------------------+-----------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
```

`product_category_name_translation table` or `product_category_name_translation.parquet`
```
+---+------------------------+-------------------------------+
|   | product_category_name  | product_category_name_english |
+---+------------------------+-------------------------------+
| 0 | beleza_saude           | health_beauty                 |
| 1 | informatica_acessorios | computers_accessories         |
| 2 | automotivo             | auto                          |
+---+------------------------+-------------------------------+
```

`sellers table` or `sellers.parquet`
```
+---+----------------------------------+------------------------+----------------+--------------+
|   | seller_id                        | seller_zip_code_prefix | seller_city    | seller_state |
+---+----------------------------------+------------------------+----------------+--------------+
| 0 | 3442f8959a84dea7ee197c632cb2df15 | 13023                  | campinas       | SP           |
| 1 | d1b65fc7debc3361ea86b5f14c68d2e2 | 13844                  | mogi guacu     | SP           |
| 2 | ce3ad9de960102d0677a81f5d0bb7b2d | 20031                  | rio de janeiro | RJ           |
+---+----------------------------------+------------------------+----------------+--------------+
```

### 2. Replacing the Original Product Category Names in Portuguese with Their Translated English Names Using the Products and Product_Category_Name_Translation Tables
Below is an example showing the first 3 rows of the transformed_products table in the database:

`transformed_products table` or `transformed_products.parquet`
```
+---+----------------------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
|   | product_id                       | translated_product_category_name | product_name_lenght | product_description_lenght | product_photos_qty | product_weight_g | product_length_cm | product_height_cm | product_width_cm |
+---+----------------------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
| 0 | 1e9e8ef04dbcff4541ed26657ea517e5 | perfumery                        | 40.0                | 287.0                      | 1.0                | 225.0            | 16.0              | 10.0              | 14.0             |
| 1 | 3aa071139cb16b67ca9e5dea641aaa2f | art                              | 44.0                | 276.0                      | 1.0                | 1000.0           | 30.0              | 18.0              | 20.0             |
| 2 | 96bd76ec8810374ed1b65e291975717f | sports_leisure                   | 46.0                | 250.0                      | 1.0                | 154.0            | 18.0              | 9.0               | 15.0             |
+---+----------------------------------+----------------------------------+---------------------+----------------------------+--------------------+------------------+-------------------+-------------------+------------------+
```

### 3. Adding Aggregated Latitude and Longitude Columns (Mean) to the Customers and Sellers Tables Using the Geolocation Table
Below are the examples showing the first 3 rows of the transformed_customers and transformed_sellers tables in the database:

`transformed_customers table` or `transformed_customers.parquet`
```
+---+----------------------------------+----------------------------------+--------------------------+---------------------+---------------------+-----------------------+----------------+
|   | customer_id                      | customer_unique_id               | customer_zip_code_prefix | customer_lat        | customer_lng        | customer_city         | customer_state |
+---+----------------------------------+----------------------------------+--------------------------+---------------------+---------------------+-----------------------+----------------+
| 0 | 06b8999e2fba1a1fbc88172c00ba8bc7 | 861eff4711a542e4b93843c6dd7febb0 | 14409                    | -20.498488755380297 | -47.396929485900976 | franca                | SP             |
| 1 | 18955e83d337fd6b2def6b18a428ac77 | 290c77bc529b7ac935b93aa66c333dc3 | 9790                     | -23.72799221530055  | -46.54284778999086  | sao bernardo do campo | SP             |
| 2 | 4e7b3e00288586ebd08712fdd0374a03 | 060e732b5b29e8181a18229c7b0b2b5e | 1151                     | -23.531641584683715 | -46.656288753249086 | sao paulo             | SP             |
+---+----------------------------------+----------------------------------+--------------------------+---------------------+---------------------+-----------------------+----------------+
```

278 missing values were noticed after joining the `customers table` and `geolocation table` since these `customer_zip_code_prefix` were not found in the `geolocation table`.
```
Missing values in each column:
customer_id                   0
customer_unique_id            0
customer_zip_code_prefix      0
customer_lat                278
customer_lng                278
customer_city                 0
customer_state                0
```

`transformed_sellers table` or `transformed_sellers.parquet`
```
+---+----------------------------------+------------------------+---------------------+---------------------+----------------+--------------+
|   | seller_id                        | seller_zip_code_prefix | seller_lat          | seller_lng          | seller_city    | seller_state |
+---+----------------------------------+------------------------+---------------------+---------------------+----------------+--------------+
| 0 | 3442f8959a84dea7ee197c632cb2df15 | 13023                  | -22.89384803253408  | -47.06133702244195  | campinas       | SP           |
| 1 | d1b65fc7debc3361ea86b5f14c68d2e2 | 13844                  | -22.38343651404282  | -46.947926542619655 | mogi guacu     | SP           |
| 2 | ce3ad9de960102d0677a81f5d0bb7b2d | 20031                  | -22.909572437655488 | -43.177703112986904 | rio de janeiro | RJ           |
+---+----------------------------------+------------------------+---------------------+---------------------+----------------+--------------+
```

7 missing values were noticed after joining the `sellers table` and `geolocation table` since these `seller_zip_code_prefix` were not found in the `geolocation table`.
```
Missing values in each column:
seller_id                 0
seller_zip_code_prefix    0
seller_lat                7
seller_lng                7
seller_city               0
seller_state              0
```

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
