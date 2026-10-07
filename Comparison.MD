# Model Comparison

## Purpose

This document compares the performance of Model 1, Logistic Regression, and Model 2, Random Forest.

Both models were evaluated using the same test dataset and the same performance metrics to provide a fair comparison.

The public Olist dataset does not contain a direct product-return variable. Therefore, the project uses customer review outcomes as a proxy target, where review scores of 1–2 are classified as a Negative Outcome and scores of 3–5 are classified as a Positive Outcome.

## Related Notebook

The comparison analysis is available in:

`notebooks/Comparison.ipynb`

The notebook loads both trained models and the same test dataset, generates predictions and compares their performance.

## Dataset Used

Both models were evaluated using the same test dataset containing:

* 19,845 observations
* 20,955 transformed features
* 2,915 Negative Outcomes (14.69%)
* 16,930 Positive Outcomes (85.31%)

The target variable is imbalanced, with Positive Outcomes making up 85.31% of the test dataset.

## Performance Comparison

| Metric    | Model 1 - Logistic Regression | Model 2 - Random Forest |
| --------- | ----------------------------: | ----------------------: |
| Accuracy  |                        0.8805 |                  0.8531 |
| Precision |                        0.8919 |                  0.8531 |
| Recall    |                        0.9784 |                  1.0000 |
| F1 Score  |                        0.9332 |                  0.9207 |
| ROC-AUC   |                        0.7825 |                  0.7760 |

## Accuracy

Model 1 achieved an accuracy of 88.05%, compared with 85.31% for Model 2.

Model 1 therefore achieved the higher overall accuracy.

However, accuracy should not be considered on its own because the dataset is imbalanced. A model that mainly predicts the majority Positive Outcome class can still achieve a relatively high accuracy.

## Precision

Model 1 achieved a precision of 89.19%, compared with 85.31% for Model 2.

Model 1 therefore performed better in terms of precision.

## Recall

Model 2 achieved a recall of 100%, compared with 97.84% for Model 1.

However, this result needs to be interpreted carefully. Model 2 predicted every test observation as Positive Outcome. This resulted in 100% recall for the Positive Outcome class but 0% recall for the Negative Outcome class.

Model 1 was able to identify some Negative Outcomes, making it more useful when considering both classes.

## F1 Score

Model 1 achieved an F1 score of 93.32%, compared with 92.07% for Model 2.

The higher F1 score indicates that Model 1 provides a better balance between precision and recall.

## ROC-AUC

Model 1 achieved a ROC-AUC score of 78.25%, compared with 77.60% for Model 2.

Model 1 therefore showed slightly better ability to distinguish between the two outcome classes.

## Confusion Matrix Comparison

### Model 1 - Logistic Regression

```text
[[  908  2007]
 [  365 16565]]
```

Model 1 correctly identified 908 Negative Outcomes and 16,565 Positive Outcomes.

However, it incorrectly classified 2,007 Negative Outcomes as Positive Outcomes and 365 Positive Outcomes as Negative Outcomes.

### Model 2 - Random Forest

```text
[[    0  2915]
 [    0 16930]]
```

Model 2 correctly identified all 16,930 Positive Outcomes but failed to identify any of the 2,915 Negative Outcomes.

The confusion matrix therefore shows that Model 2 is strongly biased towards the majority class.

## Overall Comparison

Based on the results, **Model 1, Logistic Regression, performed better overall**.

Model 1 achieved higher:

* Accuracy
* Precision
* F1 Score
* ROC-AUC

Model 2 only achieved a higher overall recall score. However, this was caused by the model predicting every observation as Positive Outcome.

The F1 score and confusion matrix provide a better indication of the difference between the models because they show how the models perform beyond the majority class.

## Conclusion

Logistic Regression is the better-performing model for this public proof-of-concept dataset.

It achieved an accuracy of 88.05%, an F1 score of 93.32% and a ROC-AUC of 78.25%. More importantly, it was able to identify some Negative Outcomes, while the Random Forest model identified none.

The results also highlight the effect of class imbalance on model performance. Further model development could focus on improving the identification of Negative Outcomes through techniques such as class balancing, threshold adjustment and further feature engineering.

For the purpose of this project, Model 1 provides the stronger baseline model to take forward.

## How to Run

1. Open `notebooks/Comparison.ipynb`.
2. Make sure the test dataset and both trained models are available.
3. Run the notebook cells from top to bottom.
4. The notebook generates predictions for both models, calculates the comparison metrics, displays the confusion matrices and produces the final comparison summary.

The comparison results are also saved as:

`model_comparison_results.csv`
