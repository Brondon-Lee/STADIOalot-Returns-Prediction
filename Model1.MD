# Model 1 – Logistic Regression

## 1. Purpose

The purpose of this model is to predict the customer outcome using the processed Olist e-commerce dataset.

The target variable was created from the customer review score:

* **0 = Negative Outcome** — review score of 1 or 2
* **1 = Positive Outcome** — review score of 3, 4 or 5

The Logistic Regression model was selected as the first classification model because it provides a relatively simple and interpretable baseline for binary classification.

## 2. Related Notebook

The implementation of this model is contained in:

`notebooks/Model1.ipynb`

The trained model is saved in:

`models/logistic_regression_model.pkl`

## 3. Input Data

The model uses the feature-engineered training and testing datasets generated during the feature engineering stage.

The following files are used:

* `X_train.npz`
* `X_test.npz`
* `y_train.csv`
* `y_test.csv`

The feature engineering process produced **20,955 features** after numerical preprocessing and one-hot encoding of categorical variables.

## 4. Model Configuration

The Logistic Regression model was implemented using Scikit-learn.

The following hyperparameters were used:

| Hyperparameter | Value |
| -------------- | ----: |
| `max_iter`     |  1000 |
| `random_state` |    42 |

The model was trained using the training dataset and then used to generate predictions for the unseen test dataset.

## 5. Training and Testing

The dataset was divided using an **80/20 stratified train-test split**.

* Training observations: **79,379**
* Testing observations: **19,845**
* Random state: **42**
* Stratification: **Yes**

Stratification was used to maintain a similar proportion of positive and negative outcomes in both the training and testing datasets.

## 6. Model Evaluation

The Logistic Regression model produced the following results on the test dataset:

| Metric    | Result |
| --------- | -----: |
| Accuracy  | 88.05% |
| Precision | 89.19% |
| Recall    | 97.84% |
| F1 Score  | 93.32% |
| ROC-AUC   | 78.25% |

### Confusion Matrix

```text
[[  908  2007]
 [  365 16565]]
```

The rows represent the actual classes and the columns represent the predicted classes.

The model correctly identified **908 negative outcomes** and **16,565 positive outcomes**. However, it incorrectly classified **2,007 negative outcomes as positive**, showing that the model has difficulty identifying the minority negative class.

## 7. Classification Report

```text
                    precision    recall  f1-score   support

Negative Outcome       0.71      0.31      0.43      2915
Positive Outcome       0.89      0.98      0.93     16930

accuracy                           0.88     19845
macro avg              0.80      0.64      0.68     19845
weighted avg           0.87      0.88      0.86     19845
```

## 8. Interpretation

The model achieved an overall accuracy of approximately **88%** and a high recall of **97.84%**. It was particularly effective at identifying positive outcomes.

However, the dataset is imbalanced, with approximately **85.3% positive outcomes** and **14.7% negative outcomes**. This imbalance affects the model's ability to identify negative outcomes.

The recall for the negative class was only **31%**, meaning that a large proportion of negative outcomes were incorrectly classified as positive.

The ROC-AUC score of **78.25%** indicates that the model has useful discriminatory ability, but there is still room for improvement.

## 9. Output

The model evaluation results were saved as:

`model1_results.csv`

The trained Logistic Regression model was saved as:

`models/logistic_regression_model.pkl`

## 10. How to Run

1. Install the required Python packages listed in `Requirements.txt`.
2. Open `notebooks/Model1.ipynb`.
3. Ensure the feature-engineered files are available.
4. Run the notebook cells in order.
5. The Logistic Regression model will be trained and evaluated.
6. The trained model can be saved to the `models/` directory.

## 11. Technologies

* Python 3.13.9
* Pandas
* NumPy
* Scikit-learn
* SciPy
* Joblib
* Jupyter Notebook

## 12. Summary

Logistic Regression provides a strong baseline for the classification problem. The model achieved **88.05% accuracy** and **78.25% ROC-AUC**. However, the low recall for the negative outcome class demonstrates the effect of class imbalance and indicates that additional modelling techniques or class-balancing methods could be investigated in future work.

