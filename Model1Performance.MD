# Model 1 Performance

## Purpose

This document presents the performance results of Model 1, which is the Logistic Regression model developed for the STADIOalot Returns Prediction project.

The model was evaluated using the test dataset created during the preprocessing and feature engineering stages.

The public Olist dataset does not contain a direct product-return variable. Therefore, the project uses customer review outcomes as a proxy target, where review scores of 1–2 are classified as a Negative Outcome and scores of 3–5 are classified as a Positive Outcome.

## Related Notebook

The performance analysis is available in:

`notebooks/Model1Performance.ipynb`

The notebook loads the saved test dataset and trained Logistic Regression model, generates predictions and calculates the required performance metrics.

## Dataset Used

The test dataset contains:

* 19,845 observations
* 20,955 transformed features
* 2,915 Negative Outcomes (14.69%)
* 16,930 Positive Outcomes (85.31%)

The target variable is therefore imbalanced, with Positive Outcomes making up the majority of the test dataset.

## Performance Metrics

The following metrics were used to evaluate Model 1:

| Metric    |  Score |
| --------- | -----: |
| Accuracy  | 0.8805 |
| Precision | 0.8919 |
| Recall    | 0.9784 |
| F1 Score  | 0.9332 |
| ROC-AUC   | 0.7825 |

### Accuracy

The model achieved an accuracy of 88.05%, meaning that approximately 88% of the test observations were classified correctly.

### Precision

The precision score was 89.19%. This indicates that when the model predicted a Positive Outcome, the prediction was correct most of the time.

### Recall

The recall score was 97.84%. This shows that the model identified most of the actual Positive Outcomes in the test dataset.

### F1 Score

The F1 score was 93.32%. This provides a balance between precision and recall and indicates strong overall classification performance for the majority class.

### ROC-AUC

The ROC-AUC score was 78.25%. This indicates that the model has a reasonable ability to distinguish between the two outcome classes.

## Confusion Matrix

The confusion matrix was:

```text
[[  908  2007]
 [  365 16565]]
```

This shows that:

* 908 Negative Outcomes were correctly identified.
* 2,007 Negative Outcomes were incorrectly classified as Positive Outcomes.
* 365 Positive Outcomes were incorrectly classified as Negative Outcomes.
* 16,565 Positive Outcomes were correctly identified.

## Classification Report

| Class            | Precision | Recall | F1 Score | Support |
| ---------------- | --------: | -----: | -------: | ------: |
| Negative Outcome |      0.71 |   0.31 |     0.43 |   2,915 |
| Positive Outcome |      0.89 |   0.98 |     0.93 |  16,930 |

The results show that the model performs much better on the Positive Outcome class than the Negative Outcome class.

The Negative Outcome recall is only 31%, meaning that a large number of negative outcomes are not identified by the model. This is likely influenced by the imbalance in the target variable, where only 14.69% of the test observations are Negative Outcomes.

## Interpretation

Overall, Model 1 achieved good performance based on accuracy, precision, recall and F1 score. However, the results also show an important limitation.

Although the model has an overall accuracy of 88.05% and an F1 score of 93.32%, it does not identify the minority Negative Outcome class particularly well. The recall for Negative Outcomes is only 31%.

Therefore, accuracy on its own would give an overly positive view of the model's performance. The confusion matrix, class-specific metrics and class distribution provide additional information about how the model performs across both classes.

For this project, the Logistic Regression model provides a useful baseline and performs better than simply predicting the majority class, but further improvements would be needed if identifying Negative Outcomes is the main business objective.

## How to Run

1. Open `notebooks/Model1Performance.ipynb`.
2. Make sure the test dataset and trained Logistic Regression model are available.
3. Run the notebook cells from top to bottom.
4. The notebook generates the predictions, performance metrics, confusion matrix, classification report and results table.

The performance results are also saved as:

`model1_performance_results.csv`
