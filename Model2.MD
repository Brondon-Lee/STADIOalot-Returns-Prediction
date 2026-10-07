# Model 2 – Random Forest

## 1. Purpose

The purpose of this model is to predict the customer outcome using the processed Olist e-commerce dataset.

The target variable was created from the customer review score:

* **0 = Negative Outcome** — review score of 1 or 2
* **1 = Positive Outcome** — review score of 3, 4 or 5

The Olist dataset does not contain a direct product-return variable. Therefore, the review-score classification is used as a proxy target for the Part B proof-of-concept.

The Random Forest model was selected as the second classification model because it can capture non-linear relationships between features and does not require the relationship between the input variables and target to be linear.

---

## 2. Related Notebook

The implementation of this model is contained in:

`notebooks/Model2.ipynb`

The trained model is saved in:

`models/random_forest_model.pkl`

---

## 3. Input Data

The model uses the feature-engineered training and testing datasets generated during the feature engineering stage.

The following files are used:

* `X_train.npz`
* `X_test.npz`
* `y_train.csv`
* `y_test.csv`

The feature engineering process produced **20,955 transformed features** after numerical preprocessing and one-hot encoding of categorical variables.

The Random Forest model therefore receives the same feature-engineered representation used by the Logistic Regression model.

---

## 4. Model Configuration

The Random Forest classifier was implemented using Scikit-learn.

The following hyperparameters were used:

| **Hyperparameter**  | **Value** |
| ------------------- | --------- |
| `n_estimators`      | 200       |
| `max_depth`         | 15        |
| `min_samples_split` | 5         |
| `random_state`      | 42        |
| `n_jobs`            | -1        |

### Hyperparameter Description

* **`n_estimators = 200`** — the model uses 200 decision trees.
* **`max_depth = 15`** — each tree is limited to a maximum depth of 15.
* **`min_samples_split = 5`** — at least five samples are required to split an internal node.
* **`random_state = 42`** — ensures reproducibility.
* **`n_jobs = -1`** — allows the model to use all available CPU cores during training.

The model was trained using the training dataset and evaluated using the unseen test dataset.

---

## 5. Training and Testing

The dataset was divided using an **80/20 stratified train-test split** during the feature engineering stage.

* Training observations: **79,379**
* Testing observations: **19,845**
* Random state: **42**
* Stratification: **Yes**

Stratification was used to maintain a similar proportion of positive and negative outcomes in both datasets.

The Random Forest model was trained only on the training data. The test data was kept separate and used to evaluate performance on unseen observations.

---

## 6. Model Evaluation

The Random Forest model produced the following results on the test dataset:

| **Metric** | **Result** |
| ---------- | ---------- |
| Accuracy   | 85.31%     |
| Precision  | 85.31%     |
| Recall     | 100.00%    |
| F1 Score   | 92.07%     |
| ROC-AUC    | 77.60%     |

### Confusion Matrix

```text id="1okf9p"
[[    0  2915]
 [    0 16930]]
```

The rows represent the actual classes and the columns represent the predicted classes.

The model classified every test observation as a **Positive Outcome**.

As a result:

* All **16,930 positive outcomes** were correctly classified.
* None of the **2,915 negative outcomes** were correctly identified.
* All **2,915 negative outcomes** were classified as positive.

---

## 7. Classification Report

```text id="x1pfv3"
                    precision    recall  f1-score   support

Negative Outcome       0.00      0.00      0.00      2915
Positive Outcome       0.85      1.00      0.92     16930

accuracy                           0.85     19845
macro avg              0.43      0.50      0.46     19845
weighted avg           0.73      0.85      0.79     19845
```

---

## 8. Interpretation

The Random Forest model achieved an accuracy of **85.31%**. However, the confusion matrix shows that the model predicted every observation as a Positive Outcome.

This means that the model achieved:

* **100% recall for the Positive Outcome class**
* **0% recall for the Negative Outcome class**

The result is strongly influenced by the class imbalance in the dataset, where approximately **85.3% of observations are Positive Outcomes** and **14.7% are Negative Outcomes**.

Therefore, the accuracy of 85.31% should not be interpreted as evidence that the model performs well. It is approximately equal to the proportion of the majority class.

The model is effectively behaving like a majority-class classifier for the current test data.

The ROC-AUC score of **77.60%** indicates that the model has some discriminatory ability based on its prediction scores. However, the default classification threshold resulted in no Negative Outcome predictions.

The current configuration would therefore not be suitable for a business application where identifying Negative Outcomes is important.

Further investigation could include:

* Class weighting
* Resampling techniques
* Hyperparameter optimisation
* Threshold adjustment
* Alternative classification algorithms

---

## 9. Comparison with Model 1

Compared with Logistic Regression, the Random Forest model achieved:

| **Metric**              | **Logistic Regression** | **Random Forest** |
| ----------------------- | ----------------------- | ----------------- |
| Accuracy                | 88.05%                  | 85.31%            |
| Precision               | 89.19%                  | 85.31%            |
| Recall                  | 97.84%                  | 100.00%           |
| F1 Score                | 93.32%                  | 92.07%            |
| ROC-AUC                 | 78.25%                  | 77.60%            |
| Negative Outcome Recall | 31%                     | 0%                |

Based on these results, Logistic Regression performed better overall.

Most importantly, Logistic Regression was able to identify some Negative Outcomes, while the Random Forest model identified none.

This makes Logistic Regression the stronger of the two models for the current proof-of-concept.

---

## 10. Output

The model evaluation results were generated during development as:

`model2_results.csv`

The trained Random Forest model was saved as:

`models/random_forest_model.pkl`

The trained model can be used later with data that has undergone the same preprocessing and feature engineering steps.

---

## 11. How to Run

1. Install the required Python packages listed in `Requirements.txt`.
2. Ensure that the feature-engineered files have been created using `notebooks/FeatureEngineering.ipynb`.
3. Open `notebooks/Model2.ipynb`.
4. Ensure the following files are available:

   * `X_train.npz`
   * `X_test.npz`
   * `y_train.csv`
   * `y_test.csv`
5. Run the notebook cells in order.
6. The Random Forest model will be trained using the training dataset.
7. Predictions will be generated for the test dataset.
8. The model will be evaluated using accuracy, precision, recall, F1 score and ROC-AUC.
9. The trained model can be saved to the `models/` directory.

The same feature engineering process must be applied to any new dataset before it can be passed to the trained model.

---

## 12. Technologies

* Python 3.13.9
* Pandas 2.3.3
* NumPy 2.3.5
* Scikit-learn 1.7.2
* SciPy
* Joblib
* Jupyter Notebook

---

## 13. Summary

The Random Forest model provided a second classification approach for comparison with Logistic Regression.

Although it achieved **85.31% accuracy**, the model predicted every test observation as the majority Positive Outcome class.

The model therefore achieved **0% recall for the Negative Outcome class**, demonstrating that accuracy alone is not sufficient for evaluating this imbalanced classification problem.

Compared with Logistic Regression, Random Forest achieved lower accuracy, F1 score and ROC-AUC and failed to identify any Negative Outcomes.

The current Random Forest configuration would therefore require further improvement before being considered suitable for identifying higher-risk outcomes.

Potential future improvements include class balancing, threshold optimisation and hyperparameter tuning.

**Related notebook:** `notebooks/Model2.ipynb`

**Trained model:** `models/random_forest_model.pkl`
