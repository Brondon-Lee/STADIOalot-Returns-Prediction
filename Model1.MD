# Model 1 – Logistic Regression

## 1. Purpose

The purpose of this model is to predict the customer outcome using the processed Olist e-commerce dataset.

The target variable was created from the customer review score:

* **0 = Negative Outcome** — review score of 1 or 2
* **1 = Positive Outcome** — review score of 3, 4 or 5

The Olist dataset does not contain a direct product-return variable. Therefore, the review-score classification is used as a proxy target for the Part B proof-of-concept.

Logistic Regression was selected as the first classification model because it provides a relatively simple and interpretable baseline for binary classification.

---

## 2. Related Notebook

The implementation of this model is contained in:

`notebooks/Model1.ipynb`

The trained model is saved in:

`models/logistic_regression_model.pkl`

---

## 3. Input Data

The model uses the feature-engineered training and testing datasets generated during the feature engineering stage.

The following files are used:

* `X_train.npz`
* `X_test.npz`
* `y_train.csv`
* `y_test.csv`

The feature engineering process produced **20,955 transformed features** after numerical preprocessing and one-hot encoding of categorical variables.

The model therefore receives the same feature-engineered representation that was created for the modelling stage.

---

## 4. Model Configuration

The Logistic Regression model was implemented using Scikit-learn.

The following hyperparameters were used:

| **Hyperparameter** | **Value** |
| ------------------ | --------- |
| `max_iter`         | 1000      |
| `random_state`     | 42        |

The model was trained using the training dataset and then used to generate predictions for the unseen test dataset.

`max_iter=1000` was used to provide sufficient iterations for the optimisation process to converge when working with the high-dimensional feature set.

---

## 5. Training and Testing

The dataset was divided using an **80/20 stratified train-test split** during the feature engineering stage.

* Training observations: **79,379**
* Testing observations: **19,845**
* Random state: **42**
* Stratification: **Yes**

Stratification was used to maintain a similar proportion of positive and negative outcomes in both the training and testing datasets.

The Logistic Regression model was trained only on the training data. The test data was kept separate and used to evaluate the model's performance on unseen observations.

---

## 6. Model Evaluation

The Logistic Regression model produced the following results on the test dataset:

| **Metric** | **Result** |
| ---------- | ---------- |
| Accuracy   | 88.05%     |
| Precision  | 89.19%     |
| Recall     | 97.84%     |
| F1 Score   | 93.32%     |
| ROC-AUC    | 78.25%     |

### Confusion Matrix

```text id="h94lhw"
[[  908  2007]
 [  365 16565]]
```

The rows represent the actual classes and the columns represent the predicted classes.

The model correctly identified:

* **908 negative outcomes**
* **16,565 positive outcomes**

The model incorrectly classified:

* **2,007 negative outcomes as positive**
* **365 positive outcomes as negative**

The results show that the model performs substantially better on the majority Positive Outcome class than on the minority Negative Outcome class.

---

## 7. Classification Report

```text id="azf4he"
                    precision    recall  f1-score   support

Negative Outcome       0.71      0.31      0.43      2915
Positive Outcome       0.89      0.98      0.93     16930

accuracy                           0.88     19845
macro avg              0.80      0.64      0.68     19845
weighted avg           0.87      0.88      0.86     19845
```

---

## 8. Interpretation

The Logistic Regression model achieved an overall accuracy of approximately **88.05%** and a ROC-AUC of **78.25%**.

The model performed particularly well at identifying Positive Outcomes, achieving a recall of **97.84%** for that class.

However, the dataset is imbalanced, with approximately:

* **85.3% Positive Outcomes**
* **14.7% Negative Outcomes**

This imbalance affects the model's ability to identify the minority Negative Outcome class.

The recall for the Negative Outcome class was only **31%**. This means that the model identified fewer than one-third of the actual negative outcomes, while many negative outcomes were classified as positive.

The Negative Outcome F1 score was **0.43**, compared with **0.93** for the Positive Outcome class. This further demonstrates that the model is much stronger at predicting the majority class.

The ROC-AUC score of **78.25%** indicates that the model has useful discriminatory ability between the two classes, although the model could still be improved, particularly in identifying Negative Outcomes.

For the current proof-of-concept, Logistic Regression provides a useful baseline and performs better than the Random Forest model evaluated in this project.

---

## 9. Output

The trained Logistic Regression model was saved as:

`models/logistic_regression_model.pkl`

The model evaluation results were also generated during development as:

`model1_results.csv`

The trained model can be used later with data that has undergone the same preprocessing and feature engineering steps.

---

## 10. How to Run

1. Install the required Python packages listed in `Requirements.txt`.
2. Ensure that the feature-engineered files have been created using `notebooks/FeatureEngineering.ipynb`.
3. Open `notebooks/Model1.ipynb`.
4. Ensure the following files are available:

   * `X_train.npz`
   * `X_test.npz`
   * `y_train.csv`
   * `y_test.csv`
5. Run the notebook cells in order.
6. The Logistic Regression model will be trained using the training dataset.
7. Predictions will be generated for the test dataset.
8. The model will be evaluated using accuracy, precision, recall, F1 score and ROC-AUC.
9. The trained model can be saved to the `models/` directory.

The same feature engineering process must be applied to any new dataset before it can be passed to the trained model.

---

## 11. Technologies

* Python 3.13.9
* Pandas 2.3.3
* NumPy 2.3.5
* Scikit-learn 1.7.2
* SciPy
* Joblib
* Jupyter Notebook

---

## 12. Summary

Logistic Regression provides a strong baseline for the binary classification problem.

The model achieved:

* **88.05% accuracy**
* **89.19% precision**
* **97.84% recall**
* **93.32% F1 score**
* **78.25% ROC-AUC**

However, the low **31% recall for the Negative Outcome class** demonstrates the effect of the imbalanced target distribution.

The model is effective at identifying the majority Positive Outcome class but has difficulty identifying Negative Outcomes. Future work could investigate techniques such as class weighting, resampling or alternative algorithms to improve minority-class performance.

**Related notebook:** `notebooks/Model1.ipynb`

**Trained model:** `models/logistic_regression_model.pkl`
