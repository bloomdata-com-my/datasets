# Introduction

The purpose of this repository is to facilitate collaboration with multiple parties by providing extracted and transformed datasets.

All Parquet files are compressed using the ZSTD compression type.

The original datasets can be found at [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).

Examples of first 3 rows print for:

Output of customers.parquet:
```
+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
| customer_id                      | customer_unique_id               | customer_zip_code_prefix | customer_city         | customer_state |
+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
| 06b8999e2fba1a1fbc88172c00ba8bc7 | 861eff4711a542e4b93843c6dd7febb0 | 14409                    | franca                | SP             |
| 18955e83d337fd6b2def6b18a428ac77 | 290c77bc529b7ac935b93aa66c333dc3 | 9790                     | sao bernardo do campo | SP             |
| 4e7b3e00288586ebd08712fdd0374a03 | 060e732b5b29e8181a18229c7b0b2b5e | 1151                     | sao paulo             | SP             |
+----------------------------------+----------------------------------+--------------------------+-----------------------+----------------+
```
