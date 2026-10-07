# Model 2 – Random Forest

## 1. Purpose

The purpose of this model is to predict the customer outcome using the processed Olist e-commerce dataset.

The target variable was created from the customer review score:

* **0 = Negative Outcome** — review score of 1 or 2
* **1 = Positive Outcome** — review score of 3, 4 or 5

The Random Forest model was selected as the second classification model because it can capture non-linear relationships between features and does not assume that the relationship between the input variables and target is linear.

## 2. Related Notebook

The implementation of this model is contained in:

`notebooks/Model2.ipynb`

The trained model is saved in:

`models/random_forest_model.pkl`

## 3. Input Data

The model uses the feature-engineered training and testing datasets generated during the feature engineering stage.

The following files are used:

* `X_train.npz`
* `X_test.npz`
* `y_train.csv`
* `y_test.csv`

The feature engineering process produced **20,955 features** after numerical preprocessing and one-hot encoding of categorical variables.

## 4. Model Configuration

The Random Forest classifier was implemented using Scikit-learn.

The following hyperparameters were used:

| Hyperparameter      | Value |
| ------------------- | ----: |
| `n_estimators`      |   200 |
| `max_depth`         |    15 |
| `min_samples_split` |     5 |
| `random_state`      |    42 |
| `n_jobs`            |    -1 |

The model was trained using the training dataset and then evaluated using the unseen test dataset.

## 5. Training and Testing

The dataset was divided using an **80/20 stratified train-test split** during the feature engineering stage.

* Training observations: **79,379**
* Testing observations: **19,845**
* Random state: **42**
* Stratification: **Yes**

Stratification was used to maintain a similar proportion of positive and negative outcomes in both datasets.

## 6. Model Evaluation

The Random Forest model produced the following results on the test dataset:

| Metric    |  Result |
| --------- | ------: |
| Accuracy  |  85.31% |
| Precision |  85.31% |
| Recall    | 100.00% |
| F1 Score  |  92.07% |
| ROC-AUC   |  77.60% |

### Confusion Matrix

```text
[[    0  2915]
 [    0 16930]]
```

The model classified every test observation as a **Positive Outcome**.

As a result, it correctly classified all 16,930 positive outcomes but failed to identify any of the 2,915 negative outcomes.

## 7. Classification Report

```text
                    precision    recall  f1-score   support

Negative Outcome       0.00      0.00      0.00      2915
Positive Outcome       0.85      1.00      0.92     16930

accuracy                           0.85     19845
macro avg              0.43      0.50      0.46     19845
weighted avg           0.73      0.85      0.79     19845
```

## 8. Interpretation

The Random Forest model achieved an accuracy of **85.31%**. However, the confusion matrix shows that the model predicted every observation as a positive outcome.

This means that the model achieved **100% recall for the positive class**, but **0% recall for the negative class**.

The result is strongly influenced by the class imbalance in the dataset, where approximately **85.3% of observations are positive outcomes** and only **14.7% are negative outcomes**.

Therefore, the accuracy of 85.31% should not be interpreted as evidence that the model performs well. It is approximately equal to the proportion of the majority class and indicates that the model has effectively learned to predict the majority class.

The ROC-AUC score of **77.60%** indicates some discriminatory ability, but the classification results show that the model requires improvement before it could be considered useful for identifying negative outcomes.

## 9. Comparison with Model 1

Compared with Logistic Regression, the Random Forest model achieved:

* Lower accuracy: **85.31% vs 88.05%**
* Lower F1 score: **92.07% vs 93.32%**
* Lower ROC-AUC: **77.60% vs 78.25%**
* No ability to identify the negative class in the current configuration

Based on these results, Logistic Regression performed better on the test dataset.

## 10. Output

The model evaluation results were saved as:

`model2_results.csv`

The trained Random Forest model was saved as:

`models/random_forest_model.pkl`

## 11. How to Run

1. Install the required Python packages listed in `Requirements.txt`.
2. Open `notebooks/Model2.ipynb`.
3. Ensure the feature-engineered files are available.
4. Run the notebook cells in order.
5. The Random Forest model will be trained and evaluated.
6. The trained model can be saved to the `models/` directory.

## 12. Technologies

* Python 3.13.9
* Pandas
* NumPy
* Scikit-learn
* SciPy
* Joblib
* Jupyter Notebook

## 13. Summary

The Random Forest model provided a second classification approach for comparison with Logistic Regression. Although it achieved **85.31% accuracy**, it predicted every test observation as the majority positive class.

The results demonstrate the importance of considering class-specific metrics rather than accuracy alone. In its current configuration, the Random Forest model is not effective at identifying negative outcomes and would require further improvement, such as class balancing or hyperparameter optimisation.
