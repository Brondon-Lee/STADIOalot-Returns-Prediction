# Preprocessing

## Overview

The Olist Brazilian E-Commerce Dataset contains several related datasets with information about orders, customers, products, sellers, payments and customer reviews. Before the machine learning models can be trained, the data needs to be cleaned and prepared so that it can be used for analysis and modelling.

## Datasets Used

The following datasets are used during the preprocessing stage:

* `olist_orders_dataset.csv`
* `olist_order_reviews_dataset.csv`
* `olist_order_items_dataset.csv`
* `olist_products_dataset.csv`
* `olist_customers_dataset.csv`
* `olist_sellers_dataset.csv`
* `olist_order_payments_dataset.csv`

## Preprocessing Techniques

The following preprocessing steps will be performed:

1. Load the required datasets into pandas DataFrames.
2. Merge the datasets using common identifiers such as:

   * `order_id`
   * `customer_id`
   * `product_id`
   * `seller_id`
3. Check for duplicate records and remove duplicates where necessary.
4. Check for missing values and handle them using suitable methods.
5. Convert date columns into the correct datetime format.
6. Create delivery-related variables using the available order and delivery dates.
7. Create a binary target variable from the customer review score:

   * **Negative Outcome** — Review Score 1–2
   * **Positive Outcome** — Review Score 3–5
8. Remove columns that are not required for modelling or could cause data leakage.
9. Convert categorical variables into a format that can be used by the machine learning models.
10. Prepare the cleaned data for the feature engineering stage.
11. Save the processed dataset for use in the next stage.

## Output

The processed dataset will be saved in:

`data/processed/`

## Related Script

The preprocessing code is contained in:

`scripts/preprocessing/preprocessing.py`

## How to Run

The preprocessing script can be run using:

`python scripts/preprocessing/preprocessing.py`

The script will load the raw datasets, perform the required preprocessing steps and save the processed dataset for use during feature engineering and modelling.
