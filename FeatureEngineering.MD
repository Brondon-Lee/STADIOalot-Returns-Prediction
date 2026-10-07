# Feature Engineering

## 1. Purpose

The purpose of the feature engineering stage is to prepare the preprocessed Olist e-commerce dataset for machine learning. The dataset contains information about orders, products, payments, customers and sellers.

The feature engineering process prepares the input variables for the two classification models used in this project:

* Logistic Regression
* Random Forest

The target variable is `target`, where:

* `0` = Negative Outcome
* `1` = Positive Outcome

The target was created during preprocessing using the customer review score. Review scores of 1 or 2 were classified as a Negative Outcome, while review scores of 3, 4 or 5 were classified as a Positive Outcome.

---

## 2. Related Notebook

The notebook associated with this stage is:

**`notebooks/FeatureEngineering.ipynb`**

The notebook contains the Python code used to perform the feature preparation, train/test split and machine learning preprocessing.

---

## 3. Input Data

The feature engineering notebook uses the processed dataset created by `Preprocessing.ipynb`.

Input file:

```text id="0yoh2r"
processed_olist.csv
```

The processed dataset contains:

* **99,224 observations**
* **24 columns**

The target variable is separated from the predictor variables before further processing.

---

## 4. Separating Features and Target

The target variable is separated from the input features using:

```python
X = data.drop(columns=["target"])
y = data["target"]
```

This produces:

* `X`: 23 predictor variables
* `y`: 1 target variable

The target distribution is imbalanced:

| **Target** | **Description**  | **Observations** |
| ---------- | ---------------- | ---------------- |
| 0          | Negative Outcome | 14,575           |
| 1          | Positive Outcome | 84,649           |

This class imbalance is considered when interpreting the model results.

---

## 5. Identifying Numerical and Categorical Features

The predictor variables were separated into numerical and categorical features.

### Numerical Features

The numerical variables include:

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
* `delivery_days`
* `delivery_delay_days`
* `delivery_missing`

### Categorical Features

The categorical variables include:

* `order_status`
* `customer_zip_code_prefix`
* `customer_city`
* `customer_state`
* `seller_zip_code`
* `seller_city`
* `seller_state`

The customer and seller postal-code fields were converted from numerical values to strings so that they would be treated as categorical variables rather than continuous numerical measurements.

```python
X["customer_zip_code_prefix"] = (
    X["customer_zip_code_prefix"].astype(str)
)

X["seller_zip_code"] = (
    X["seller_zip_code"].astype(str)
)
```

This prevents the machine learning models from interpreting postal codes as quantities where the numerical difference between two postal codes has meaningful mathematical interpretation.

---

## 6. Train/Test Split

The dataset was divided into training and testing datasets using an 80/20 split.

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.20,
    random_state=42,
    stratify=y
)
```

The following settings were used:

* Training data: 80%
* Testing data: 20%
* `random_state = 42`
* `stratify = y`

Stratification was used to maintain a similar proportion of Positive and Negative Outcomes in both the training and testing datasets.

The resulting datasets were:

| **Dataset** | **Observations** | **Features** |
| ----------- | ---------------- | ------------ |
| Training    | 79,379           | 23           |
| Testing     | 19,845           | 23           |

The target distributions were:

### Training

* Positive Outcome: 67,719
* Negative Outcome: 11,660

### Testing

* Positive Outcome: 16,930
* Negative Outcome: 2,915

---

## 7. Numerical Feature Preprocessing

Numerical variables were processed using a Scikit-learn pipeline.

Missing numerical values were handled using median imputation:

```python
SimpleImputer(strategy="median")
```

The numerical variables were then standardised using:

```python
StandardScaler()
```

The numerical preprocessing pipeline was:

```python
numeric_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler())
])
```

Median imputation was selected because it provides a simple method of handling missing numerical values and is less affected by extreme values than mean imputation.

Standardisation was applied because Logistic Regression can benefit from numerical variables being placed on comparable scales.

---

## 8. Categorical Feature Preprocessing

Categorical variables were processed using a separate Scikit-learn pipeline.

Missing categorical values were handled using the most frequent category:

```python
SimpleImputer(strategy="most_frequent")
```

Categorical variables were then converted into numerical features using one-hot encoding:

```python
OneHotEncoder(handle_unknown="ignore")
```

The complete categorical pipeline was:

```python
categorical_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("encoder", OneHotEncoder(handle_unknown="ignore"))
])
```

The `handle_unknown="ignore"` setting allows the preprocessing pipeline to handle categories that may occur in the testing data but were not present in the training data.

---

## 9. Combined Preprocessing Pipeline

The numerical and categorical preprocessing pipelines were combined using `ColumnTransformer`.

```python
preprocessor = ColumnTransformer([
    ("numeric", numeric_pipeline, numeric_features),
    ("categorical", categorical_pipeline, categorical_features)
])
```

The preprocessing was fitted only on the training data:

```python
X_train_processed = preprocessor.fit_transform(X_train)
```

The same fitted transformations were then applied to the testing data:

```python
X_test_processed = preprocessor.transform(X_test)
```

This prevents information from the testing dataset being used when learning the preprocessing transformations.

The same preprocessing approach is therefore applied consistently to both machine learning models.

---

## 10. Resulting Feature Dimensions

After imputation, standardisation and one-hot encoding, the number of features increased because the categorical variables were converted into multiple binary variables.

The final dimensions were:

```text id="9gj8xm"
Training data: (79379, 20955)
Testing data:  (19845, 20955)
```

Therefore, the original 23 predictor variables were transformed into **20,955 machine learning features**.

The increase was mainly caused by high-cardinality categorical variables such as:

* Customer city
* Seller city
* Customer postal code
* Seller postal code

The transformed datasets were stored as sparse matrices to reduce memory usage.

---

## 11. Missing Value Validation

After the preprocessing pipeline was applied, the transformed datasets were checked for missing values.

The result was:

```text id="1nljjr"
Missing values in processed training data: 0
Missing values in processed testing data: 0
```

This confirms that the preprocessing pipeline successfully handled the missing values before model training.

---

## 12. Output Files

The feature-engineered datasets were saved in a `feature_engineered` output folder.

The following files were created:

```text id="a7m9jd"
X_train.npz
X_test.npz
y_train.csv
y_test.csv
```

The `.npz` files contain the sparse feature matrices, while the CSV files contain the corresponding target values.

These files are then used by:

* `notebooks/Model1.ipynb`
* `notebooks/Model2.ipynb`

This ensures that both models use the same feature-engineered training and testing data.

---

## 13. How to Use the Notebook

To reproduce the feature engineering process:

1. Ensure that `processed_olist.csv` has been created by running `notebooks/Preprocessing.ipynb`.
2. Open `notebooks/FeatureEngineering.ipynb`.
3. Update the input file path if the dataset is stored in a different location.
4. Run the notebook cells from beginning to end.
5. The notebook will separate the target from the predictors.
6. The numerical and categorical variables will be identified.
7. The dataset will be divided into training and testing sets.
8. The preprocessing pipeline will perform imputation, one-hot encoding and numerical standardisation.
9. The processed datasets will be saved as sparse matrices.
10. The target datasets will be saved as CSV files.
11. The resulting files can then be used by `notebooks/Model1.ipynb` and `notebooks/Model2.ipynb`.

The notebook should be run from the first cell to the final cell to reproduce the feature engineering process.

---

## 14. Technologies Used

The feature engineering process uses:

* Python 3.13.9
* Pandas 2.3.3
* NumPy 2.3.5
* Scikit-learn 1.7.2
* SciPy
* Jupyter Notebook

The main Scikit-learn techniques used are:

* `train_test_split`
* `ColumnTransformer`
* `Pipeline`
* `SimpleImputer`
* `OneHotEncoder`
* `StandardScaler`

---

## 15. Summary

The feature engineering stage converts the preprocessed Olist dataset into a format suitable for machine learning.

The main techniques include:

* Separating predictor variables and the target
* Identifying numerical and categorical variables
* Converting postal codes to categorical variables
* Stratified 80/20 train/test splitting
* Median imputation for numerical variables
* Most-frequent imputation for categorical variables
* One-hot encoding of categorical variables
* Standardisation of numerical variables
* Preventing test-data leakage by fitting transformations only on training data
* Storing the transformed data as sparse matrices

The final feature-engineered data contains **79,379 training observations and 19,845 testing observations**, with **20,955 transformed features**.

The resulting training and testing datasets are used as the input for the Logistic Regression and Random Forest models.

**Related notebook:** `notebooks/FeatureEngineering.ipynb`
