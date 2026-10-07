# Model 2 Performance

## Purpose

This document presents the performance results of Model 2, which is the Random Forest model developed for the STADIOalot Returns Prediction project.

The model was evaluated using the same test dataset used for Model 1 so that the two models can be compared fairly.

The public Olist dataset does not contain a direct product-return variable. Therefore, the project uses customer review outcomes as a proxy target, where review scores of 1–2 are classified as a Negative Outcome and scores of 3–5 are classified as a Positive Outcome.

## Related Notebook

The performance analysis is available in:

`notebooks/Model2Performance.ipynb`

The notebook loads the saved test dataset and trained Random Forest model, generates predictions and calculates the required performance metrics.

## Dataset Used

The test dataset contains:

* 19,845 observations
* 20,955 transformed features
* 2,915 Negative Outcomes (14.69%)
* 16,930 Positive Outcomes (85.31%)

The target variable is imbalanced, with Positive Outcomes making up the majority of the test dataset.

## Performance Metrics

The following metrics were used to evaluate Model 2:

| Metric    |  Score |
| --------- | -----: |
| Accuracy  | 0.8531 |
| Precision | 0.8531 |
| Recall    | 1.0000 |
| F1 Score  | 0.9207 |
| ROC-AUC   | 0.7760 |

### Accuracy

The model achieved an accuracy of 85.31%.

However, this result needs to be interpreted together with the class distribution because Positive Outcomes account for 85.31% of the test dataset.

### Precision

The precision score was 85.31%. This is the same as the proportion of Positive Outcomes in the test dataset because the model predicted every observation as Positive Outcome.

### Recall

The recall score was 100%. This means that the model correctly identified all actual Positive Outcomes.

However, this does not mean that the model successfully classified both classes. The model failed to identify any Negative Outcomes.

### F1 Score

The F1 score was 92.07%. Although this is relatively high, it is mainly driven by the model's performance on the majority Positive Outcome class.

### ROC-AUC

The ROC-AUC score was 77.60%, showing that the model has some ability to distinguish between the two outcomes based on its predicted probabilities.

## Confusion Matrix

The confusion matrix was:

```text
[[    0  2915]
 [    0 16930]]
```

This shows that:

* 0 Negative Outcomes were correctly identified.
* 2,915 Negative Outcomes were incorrectly classified as Positive Outcomes.
* 0 Positive Outcomes were incorrectly classified as Negative Outcomes.
* 16,930 Positive Outcomes were correctly identified.

The model therefore predicted **all 19,845 test observations as Positive Outcome**.

## Classification Report

| Class            | Precision | Recall | F1 Score | Support |
| ---------------- | --------: | -----: | -------: | ------: |
| Negative Outcome |      0.00 |   0.00 |     0.00 |   2,915 |
| Positive Outcome |      0.85 |   1.00 |     0.92 |  16,930 |

The classification report clearly shows the problem with the model. The Random Forest did not identify any of the Negative Outcome observations.

## Class Distribution

The test set contains:

| Outcome          |  Count | Percentage |
| ---------------- | -----: | ---------: |
| Negative Outcome |  2,915 |     14.69% |
| Positive Outcome | 16,930 |     85.31% |

This imbalance is important when interpreting the model's accuracy.

## Interpretation

The Random Forest achieved an accuracy of 85.31%, but this performance is largely explained by the class imbalance.

The model predicted every test observation as Positive Outcome. As a result, it achieved 100% recall for the Positive Outcome class but 0% recall for the Negative Outcome class.

This means that the model is not useful for identifying Negative Outcomes in its current form. The results demonstrate why accuracy alone should not be used when evaluating a classification model with an imbalanced target.

The Random Forest therefore provides a useful comparison against Model 1, but Model 1 provides a better overall result for this dataset because it identifies at least some of the Negative Outcome observations.

## How to Run

1. Open `notebooks/Model2Performance.ipynb`.
2. Make sure the test dataset and trained Random Forest model are available.
3. Run the notebook cells from top to bottom.
4. The notebook generates the predictions, performance metrics, confusion matrix, classification report and results table.

The performance results are also saved as:

`model2_performance_results.csv`
