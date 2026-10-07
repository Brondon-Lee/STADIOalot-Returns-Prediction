# Preprocessing

## 1. Overview

The Olist Brazilian E-Commerce Dataset contains several related datasets with information about orders, customers, products, sellers, payments and customer reviews.

Before the machine learning models can be trained, the datasets need to be combined, cleaned and prepared for analysis and modelling.

The preprocessing stage prepares the data for the feature engineering stage.

---

## 2. Related Notebook

The notebook associated with this stage is:

**`notebooks/Preprocessing.ipynb`**

The notebook contains the Python code used to load, inspect, merge and clean the Olist datasets and create the target variable.

---

## 3. Datasets Used

The following datasets are used during preprocessing:

* `olist_orders_dataset.csv`
* `olist_order_reviews_dataset.csv`
* `olist_order_items_dataset.csv`
* `olist_products_dataset.csv`
* `olist_customers_dataset.csv`
* `olist_sellers_dataset.csv`
* `olist_order_payments_dataset.csv`

---

## 4. Data Loading

The datasets are loaded into Pandas DataFrames using `pandas.read_csv()`.

The main datasets and their original sizes were:

| **Dataset** | **Observations** | **Columns** |
| ----------- | ---------------- | ----------- |
| Orders      | 99,441           | 8           |
| Reviews     | 99,224           | 7           |
| Order Items | 112,650          | 7           |
| Products    | 32,951           | 9           |
| Customers   | 99,441           | 5           |
| Sellers     | 3,095            | 4           |
| Payments    | 103,886          | 5           |

---

## 5. Data Quality Checks

The datasets were checked for:

* Missing values
* Duplicate rows
* Data types
* Dataset dimensions

The original datasets contained no complete duplicate rows.

Missing values were identified in several columns, particularly delivery dates, review comments and some product attributes.

---

## 6. Date Conversion

The relevant order date fields were converted to the appropriate datetime format using Pandas.

The following columns were converted:

* `order_purchase_timestamp`
* `order_approved_at`
* `order_delivered_carrier_date`
* `order_delivered_customer_date`
* `order_estimated_delivery_date`

Invalid date values were converted to missing values where applicable.

---

## 7. Dataset Merging

The related datasets were combined using their common identifiers.

The main identifiers used during the merging process were:

* `order_id`
* `customer_id`
* `product_id`
* `seller_id`

Orders and reviews were first combined using `order_id`.

Order-item and product information were then combined and summarised at order level before being merged with the main dataset.

Payment information was summarised by order.

Customer information was joined using `customer_id`.

Seller information was combined with order-item information and summarised at order level before being joined to the main dataset.

The resulting dataset was maintained at approximately one row per reviewed order.

---

## 8. Target Variable

A binary target variable was created from the customer review score.

The target definition was:

| **Review Score** | **Target** | **Outcome**      |
| ---------------- | ---------- | ---------------- |
| 1–2              | 0          | Negative Outcome |
| 3–5              | 1          | Positive Outcome |

The target distribution was:

| **Target**           | **Observations** | **Proportion** |
| -------------------- | ---------------- | -------------- |
| 0 – Negative Outcome | 14,575           | 14.69%         |
| 1 – Positive Outcome | 84,649           | 85.31%         |

The target is therefore imbalanced, with Positive Outcome representing the majority class.

It is important to note that the Olist dataset does not contain a direct product-return variable. The review-score classification is therefore used as a **proxy target for the Part B proof of concept**.

---

## 9. Order-Level Features Created

Several order-level variables were created while preparing the data.

These included:

* `number_of_items`
* `total_price`
* `total_freight`
* `number_of_products`
* `average_product_weight`
* `average_product_length`
* `average_product_height`
* `average_product_width`
* `average_product_photos`
* `total_payment_value`
* `payment_installments`
* `number_of_payments`
* `number_of_sellers`

These variables summarise information from the related order-item, product, payment and seller datasets.

---

## 10. Delivery Features

Delivery-related features were created using the order dates.

### Delivery Days

The number of days between order purchase and customer delivery was calculated as:

```text
delivery_days =
order_delivered_customer_date - order_purchase_timestamp
```

### Delivery Delay

The difference between actual customer delivery and estimated delivery was calculated as:

```text
delivery_delay_days =
order_delivered_customer_date - order_estimated_delivery_date
```

### Delivery Missing Indicator

A binary variable called `delivery_missing` was created:

* `0` = delivery date available
* `1` = delivery date missing

Missing delivery dates were retained because the absence of a delivery date may contain useful information about an order.

---

## 11. Missing Value Handling

Missing numerical values in the order-level dataset were handled using median values.

The numerical fields included:

* Product measurements
* Number of products
* Freight value
* Total price
* Number of sellers
* Number of items
* Payment value
* Payment information

Missing seller categorical values were replaced with:

```text
Unknown
```

Missing `order_approved_at` values were replaced using the corresponding `order_purchase_timestamp`.

Delivery-related missing values were retained so that the `delivery_missing` feature could capture whether a delivery date was unavailable.

Additional missing-value handling is performed later during feature engineering using the machine learning preprocessing pipeline. The feature engineering pipeline applies median imputation to numerical features and most-frequent imputation to categorical features.

---

## 12. Columns Removed

Several columns were removed before saving the processed dataset.

These included:

* `review_score`
* `order_id`
* `customer_id`
* `customer_unique_id`
* `order_purchase_timestamp`
* `order_approved_at`
* `order_delivered_carrier_date`
* `order_delivered_customer_date`
* `order_estimated_delivery_date`

The `review_score` was removed because it was used to create the target variable.

Identifier columns were removed because they do not provide useful direct predictive information for the modelling task.

Raw date columns were removed after the relevant delivery features had been created.

Removing these columns also prevents information that should not be directly available as model features from being used during modelling.

---

## 13. Final Processed Dataset

After preprocessing, the final dataset contained:

```text
99,224 observations
24 columns
```

The final dataset consisted of:

* 1 binary target variable
* 16 numerical modelling features
* 7 categorical modelling features

The final dataset was saved as:

```text
processed_olist.csv
```

This processed dataset is used as the input for the feature engineering stage.

---

## 14. Validation

The final processed dataset was checked for:

* Dataset shape
* Duplicate rows
* Target distribution
* Data types
* Missing values

The final dataset contained **520 rows that were identical across the remaining modelling columns** after identifiers and raw date fields had been removed.

These were not duplicate rows in the original source datasets. They resulted from different orders becoming identical across the retained modelling features.

The rows were therefore retained rather than arbitrarily removing potentially valid observations.

The final dataset shape was:

```text
(99,224, 24)
```

---

## 15. How to Use the Notebook

To reproduce the preprocessing process:

1. Place all seven raw Olist CSV files in the required input folder.
2. Open `notebooks/Preprocessing.ipynb`.
3. Update the file paths in the notebook if the datasets are stored in a different location.
4. Run the notebook cells from beginning to end.
5. The notebook will load and inspect the datasets.
6. The datasets will be merged using their relevant identifiers.
7. Date fields will be converted to datetime format.
8. Order-level and delivery-related variables will be created.
9. The binary target variable will be created from the review score.
10. Missing values will be handled.
11. Unnecessary identifiers and raw date columns will be removed.
12. The processed dataset will be saved as `processed_olist.csv`.
13. The resulting file can then be used by `notebooks/FeatureEngineering.ipynb`.

The notebook should be run from the first cell to the final cell to reproduce the preprocessing process.

---

## 16. Technologies Used

The preprocessing stage was developed using:

* Python 3.13.9
* Pandas 2.3.3
* Jupyter Notebook

---

## 17. Summary

The preprocessing stage combines the seven Olist datasets into a single order-level modelling dataset.

The main preprocessing techniques include:

* Data loading and inspection
* Missing-value identification and handling
* Duplicate checking
* Date conversion
* Dataset merging
* Order-level aggregation
* Delivery feature creation
* Binary target creation
* Removal of identifiers and raw date variables
* Final dataset validation

The resulting dataset contains **99,224 observations and 24 columns** and is saved as `processed_olist.csv`.

**Related notebook:** `notebooks/Preprocessing.ipynb`
