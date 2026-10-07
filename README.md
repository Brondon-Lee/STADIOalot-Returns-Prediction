# STADIOalot-Returns-Prediction

A data science project using machine learning to investigate and predict product returns for STADIOalot.

## Project Title

**Predicting and Reducing E-commerce Product Returns at STADIOalot Using Machine Learning**

## Motivation

STADIOalot is a large online retailer that processes around 58 million orders every year. Although the company continues to grow, one of its biggest challenges is turning that growth into profit. The company currently has an operating margin of only 1.9%, which means that unnecessary costs from picking, packing, delivering and returning orders can have a big impact on the business. When millions of orders are being processed, even a small unnecessary cost on each order can add up to a large amount of money.

One area where I believe there is a good opportunity for improvement is product returns. STADIOalot's overall return rate has increased from 11% to 15%, while returns for clothing and shoes have increased from 22% to 29%. This means that more products are being returned and the company has already spent money processing and delivering these orders before having to deal with the return as well. What makes this problem interesting from a data science point of view is that these returns are not always completely random. The information provided by STADIOalot shows that certain products, sellers, sizes, colours and customer buying patterns have higher return rates than others.

At the moment, these patterns are not being used to identify high-risk orders when customers are making their purchases. For this project, I would like to use STADIOalot's historical data to understand what is causing these returns and then build a machine learning model that can predict the likelihood of a product being returned.

STADIOalot already has several years of useful data available, including orders and transactions, product information, delivery information, returns and seller performance. This gives the project a good amount of historical information that can be used to find patterns and train a predictive model.

The purpose of the model would not be to stop customers from placing orders because they are considered likely to return something. Instead, the prediction could help STADIOalot take action before the return happens. For example, customers could be given better product or sizing information, while products or sellers that continuously have high return rates could be identified and investigated.

This would allow the company to use the data it already collects to make better decisions and potentially prevent some avoidable returns. This project also connects directly with STADIOalot's future strategy. One of the company's priorities is to understand what is causing returns and prevent avoidable returns before an order is confirmed.

If the company can identify which orders have a higher chance of being returned and understand why, it could help reduce unnecessary costs while still giving customers a good online shopping experience. This is important for STADIOalot because reducing avoidable costs across millions of orders could contribute towards improving the company's overall profitability.

## Problem Statement

STADIOalot is experiencing an increase in product returns, with the overall return rate increasing from 11% to 15%. The problem is even greater for clothing and shoes, where the return rate has increased from 22% to 29%. With the large number of orders STADIOalot processes every year, these returns create additional costs because the company has already spent money picking, packing and delivering the order before having to process the return.

The information provided by STADIOalot suggests that some returns may be predictable. Certain products, sellers, size and colour combinations and customer buying patterns have higher return rates than others. However, these patterns are currently not being used to identify orders that may have a higher chance of being returned before the purchase is completed.

The problem this project aims to solve is therefore to determine whether STADIOalot's historical data can be used to identify the main factors that contribute to product returns and predict the likelihood of an item being returned.

A machine learning model will be developed using information such as previous orders, product details, seller information, customer purchasing patterns and historical returns. The model would classify orders according to their likelihood of being returned.

The results could then help STADIOalot identify higher-risk orders and understand the factors that are contributing to returns. This information could support actions such as improving product descriptions, providing better sizing information or identifying products and sellers that continuously experience high return rates.

The overall aim of the project is to use STADIOalot's existing data to support earlier and more informed decisions that could reduce avoidable product returns and the costs associated with them.

# Part B – Public Dataset Proof of Concept

## Purpose

The actual STADIOalot data described in the original project proposal contains sensitive customer, transaction and return information. Before applying the proposed approach to sensitive client data, a publicly available dataset was used to demonstrate the feasibility of the data science workflow.

The **Brazilian E-Commerce Public Dataset by Olist** was selected as the public proof-of-concept dataset.

The dataset contains information relating to:

* Orders
* Order items
* Products
* Customers
* Sellers
* Payments
* Customer reviews

The Olist dataset does not contain a direct product-return variable. Therefore, a proxy classification target was created using customer review scores.

The target was defined as:

* **0 – Negative Outcome:** review score of 1 or 2
* **1 – Positive Outcome:** review score of 3, 4 or 5

This allows the proposed machine learning workflow to be tested on a real-world e-commerce dataset while clearly recognising that the target used in Part B is a proxy and is not the same as a confirmed product-return outcome.

## Dataset Summary

After preprocessing and merging the relevant Olist datasets, the final modelling dataset contained:

**99,224 observations and 24 columns.**

The target distribution was:

| Outcome          | Observations | Proportion |
| ---------------- | -----------: | ---------: |
| Negative Outcome |       14,575 |     14.69% |
| Positive Outcome |       84,649 |     85.31% |
| **Total**        |   **99,224** |   **100%** |

The imbalance between the two classes was considered during model evaluation because accuracy alone may give a misleading indication of performance.

# Part B Documentation

The following documents describe the technical implementation required for Part B.

### Preprocessing

[Preprocessing.MD](Preprocessing.MD)

Describes the data loading, quality checks, date conversion, dataset merging, target creation, missing-value handling, feature creation and removal of inappropriate modelling columns.

### Feature Engineering

[FeatureEngineering.MD](FeatureEngineering.MD)

Describes the preparation of the modelling features, target separation, categorical and numerical feature handling, train-test split, encoding, scaling and transformation.

### Model 1 – Logistic Regression

[Model1.MD](Model1.MD)

Describes the Logistic Regression model, its hyperparameters, training process, evaluation metrics and results.

### Model 2 – Random Forest

[Model2.MD](Model2.MD)

Describes the Random Forest model, its hyperparameters, training process, evaluation metrics and results.

# Jupyter Notebooks

The executable Jupyter notebooks are stored in the `notebooks/` folder.

* [Preprocessing.ipynb](notebooks/Preprocessing.ipynb)
* [FeatureEngineering.ipynb](notebooks/FeatureEngineering.ipynb)
* [Model1.ipynb](notebooks/Model1.ipynb)
* [Model2.ipynb](notebooks/Model2.ipynb)

The notebooks provide the implementation of the preprocessing, feature engineering and two machine learning models described in the Part B documentation.

# Trained Models

The trained models are stored in the `models/` folder.

* `models/logistic_regression_model.pkl`
* `models/random_forest_model.pkl`

The Logistic Regression and Random Forest models were trained using the feature-engineered training data and evaluated using the unseen test dataset.

# Model Results

Two classification models were developed and evaluated.

| Model               | Accuracy | Precision |  Recall | F1 Score | ROC-AUC |
| ------------------- | -------: | --------: | ------: | -------: | ------: |
| Logistic Regression |   88.05% |    89.19% |  97.84% |   93.32% |  78.25% |
| Random Forest       |   85.31% |    85.31% | 100.00% |   92.07% |  77.60% |

## Logistic Regression

The Logistic Regression model achieved an accuracy of **88.05%** and a ROC-AUC of **78.25%**.

The model performed well at identifying positive outcomes, with a recall of **97.84%** for the positive class. However, its recall for the negative class was only approximately **31%**, showing that the class imbalance made it difficult to identify negative outcomes.

The confusion matrix was:

```text
[[  908  2007]
 [  365 16565]]
```

## Random Forest

The Random Forest model achieved an accuracy of **85.31%** and a ROC-AUC of **77.60%**.

However, further investigation showed that the model predicted every test observation as the majority positive class.

The confusion matrix was:

```text
[[    0  2915]
 [    0 16930]]
```

This means that the Random Forest model correctly identified all positive outcomes but failed to identify any negative outcomes.

Therefore, the 85.31% accuracy should not be interpreted as evidence that the model performs well. It is approximately equal to the proportion of the majority class in the dataset.

Based on the current results, Logistic Regression performed better than the Random Forest model and was more useful for identifying the minority negative class.

# Part C – Model Performance and Comparison

## Purpose

Part C evaluates the performance of the two machine learning models developed in Part B using the unseen test dataset.

The two models were evaluated using the same test data and multiple classification metrics to provide a fair comparison.

The metrics used were:

* Accuracy
* Precision
* Recall
* F1 Score
* ROC-AUC
* Confusion Matrix

Because the target variable is imbalanced, with 85.31% of observations classified as Positive Outcome, accuracy was not considered on its own.

## Model 1 Performance

The Logistic Regression model achieved the following results:

| Metric    |  Score |
| --------- | -----: |
| Accuracy  | 88.05% |
| Precision | 89.19% |
| Recall    | 97.84% |
| F1 Score  | 93.32% |
| ROC-AUC   | 78.25% |

The detailed Model 1 performance analysis is available in:

[Model1Performance.MD](Model1Performance.MD)

The related executable notebook is:

[Model1Performance.ipynb](notebooks/Model1Performance.ipynb)

The Logistic Regression model achieved the highest overall performance across most of the evaluation metrics. However, the model still struggled to identify the minority Negative Outcome class, with a recall of approximately 31% for that class.

## Model 2 Performance

The Random Forest model achieved the following results:

| Metric    |   Score |
| --------- | ------: |
| Accuracy  |  85.31% |
| Precision |  85.31% |
| Recall    | 100.00% |
| F1 Score  |  92.07% |
| ROC-AUC   |  77.60% |

The detailed Model 2 performance analysis is available in:

[Model2Performance.MD](Model2Performance.MD)

The related executable notebook is:

[Model2Performance.ipynb](notebooks/Model2Performance.ipynb)

Although the Random Forest achieved 100% recall for the positive class, further analysis showed that it predicted every observation as Positive Outcome. It therefore failed to identify any of the Negative Outcome observations.

This shows why accuracy and recall should be interpreted together with the confusion matrix and other performance metrics.

## Model Comparison

The two models were evaluated using the same test dataset.

| Metric    | Logistic Regression | Random Forest |
| --------- | ------------------: | ------------: |
| Accuracy  |          **88.05%** |        85.31% |
| Precision |          **89.19%** |        85.31% |
| Recall    |              97.84% |   **100.00%** |
| F1 Score  |          **93.32%** |        92.07% |
| ROC-AUC   |          **78.25%** |        77.60% |

The detailed comparison is available in:

[Comparison.MD](Comparison.MD)

The related executable notebook is:

[Comparison.ipynb](notebooks/Comparison.ipynb)

### Comparison Conclusion

Based on the results, **Logistic Regression performed better overall**.

It achieved higher accuracy, precision, F1 Score and ROC-AUC than the Random Forest model. Although Random Forest achieved a higher recall score, this was because it classified every test observation as Positive Outcome.

Logistic Regression was able to correctly identify some Negative Outcomes, while Random Forest identified none.

The F1 Score also provides a useful comparison because it considers both precision and recall. Logistic Regression achieved an F1 Score of **93.32%**, compared with **92.07%** for Random Forest.

Therefore, Logistic Regression is the preferred model from the two models tested in this proof-of-concept.

However, the results also show that further work is required to improve the identification of Negative Outcomes. Future modelling could investigate class balancing, threshold adjustment and additional feature engineering.

## Part C Notebooks

The executable notebooks used for the Part C analysis are stored in the `notebooks/` folder:

* [Model1Performance.ipynb](notebooks/Model1Performance.ipynb)
* [Model2Performance.ipynb](notebooks/Model2Performance.ipynb)
* [Comparison.ipynb](notebooks/Comparison.ipynb)

The Part C notebooks load the trained models and the unseen test dataset, generate predictions and calculate the required performance metrics.

## Part C Documentation

The supporting Part C documentation is stored in the repository root:

* [Model1Performance.MD](Model1Performance.MD)
* [Model2Performance.MD](Model2Performance.MD)
* [Comparison.MD](Comparison.MD)

# Part D – Recommendations Report

## Purpose

Part D provides recommendations based on the model performance and comparison results from Part C.

The report answers the following questions:

1. Which model is best between Model 1 and Model 2?
2. How can the selected model be improved?
3. How will the selected model need to be adapted to work with the STADIOalot data requested in SS1?
4. Do the results align with the related literature from Part A?

The report also discusses the class imbalance found in the dataset and the limitation of using the Olist review score as a proxy outcome instead of an actual product-return outcome.

## Recommended Model

Logistic Regression was selected as the preferred model based on the overall results.

It achieved higher accuracy, precision, F1 Score and ROC-AUC than Random Forest. Although Random Forest achieved 100% recall for the positive class, the confusion matrix showed that it predicted every observation as Positive Outcome and failed to identify any Negative Outcomes.

## Model Improvements

The selected Logistic Regression model could be improved by addressing the class imbalance and improving the available features.

Possible improvements include:

* Using class weighting or resampling techniques.
* Adjusting the classification threshold.
* Adding more relevant product, customer and transaction features.
* Performing additional hyperparameter tuning.
* Using cross-validation to provide more reliable performance estimates.
* Investigating additional models such as Gradient Boosting.

## Adaptation to STADIOalot Data

The current proof of concept uses the Olist review score as a proxy target because the public dataset does not contain actual product-return information.

When the model is applied to STADIOalot, the target variable should be replaced with the actual return outcome from the data requested in SS1.

The model can then use appropriate information about:

* Customers
* Products
* Orders
* Sellers
* Transactions
* Delivery
* Previous returns

Care would also need to be taken to prevent data leakage. Only information that would have been available before the return occurs should be used when making the prediction.

## Literature Alignment

The results are generally consistent with the literature reviewed in Part A.

Heilig et al. (2016) showed that machine learning and ensemble approaches can be used for product return prediction. Mishra and Dutta (2024) compared different machine learning models and highlighted the importance of product, order and transaction variables when predicting returns. Duong et al. (2025) also showed that product characteristics can be useful for understanding return behaviour and that interpretable machine learning can provide additional insight.

The results from this project support the general idea that machine learning can be used to identify patterns in e-commerce customer behaviour.

However, the results cannot be directly compared with studies that use actual product returns because the Olist proof-of-concept uses a review-score proxy. The final STADIOalot model should therefore be retrained and evaluated using actual return outcomes.

## Part D Report

The complete Part D Recommendations Report is available in the `reports/` folder:

[PartD_Recommendations_Report.pdf](reports/PartD_Recommendations_Report.pdf)

# Repository Structure

The repository has been structured to keep the different parts of the project organised and make it easier to manage the data, code, models, experiments and reports.

```text
STADIOalot-Returns-Prediction/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── experiments/
│   ├── setup/
│   └── results/
│
├── models/
│   ├── logistic_regression_model.pkl
│   └── random_forest_model.pkl
│
├── notebooks/
│   ├── Preprocessing.ipynb
│   ├── FeatureEngineering.ipynb
│   ├── Model1.ipynb
│   ├── Model2.ipynb
│   ├── Model1Performance.ipynb
│   ├── Model2Performance.ipynb
│   └── Comparison.ipynb
│
├── reports/
│   └── PartD_Recommendations_Report.pdf
│
├── scripts/
│
├── Preprocessing.MD
├── FeatureEngineering.MD
├── Model1.MD
├── Model2.MD
├── Model1Performance.MD
├── Model2Performance.MD
├── Comparison.MD
├── README.md
├── Requirements.txt
└── .gitignore
```

## Folder Descriptions

* **data/raw/** – Contains original datasets used for the project where applicable. Raw data should remain unchanged.
* **data/processed/** – Contains datasets that have been cleaned and prepared for analysis and machine learning.
* **models/** – Contains trained machine learning models saved for later use.
* **experiments/setup/** – Contains setup and configuration information used for experiments.
* **experiments/results/** – Contains experimental results and model comparison results.
* **scripts/** – Contains supporting Python scripts used throughout the project.
* **notebooks/** – Contains Jupyter notebooks used for preprocessing, feature engineering and model development.
* **reports/** – Contains project reports and supporting documents, including the Part D Recommendations Report.

# Requirements

The project dependencies are listed in:

`Requirements.txt`

The main technologies used include:

* Python 3.13.9
* Pandas 2.3.3
* NumPy 2.3.5
* Scikit-learn 1.7.2
* Matplotlib 3.10.6
* Seaborn 0.13.2
* SciPy
* Joblib
* Jupyter Notebook

# RAAIDD Log

The RAAIDD log will be used throughout the project to keep track of risks, actions, assumptions, issues, decisions and dependencies that could affect the STADIOalot product returns prediction project.

| RAAIDD           | Description                                                                                                                                                                              |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Risk 1**       | Product categories may be inconsistent across the catalogue, which could make it difficult to accurately compare return patterns between similar products.                               |
| **Risk 2**       | Missing or unreliable seller information could affect the accuracy of the machine learning model, especially if seller performance is an important predictor of returns.                 |
| **Risk 3**       | The number of returned and non-returned items may be unbalanced, which could cause the model to perform well on the majority class while struggling to identify returned items.          |
| **Risk 4**       | Historical return patterns may change over time as customer behaviour, products, sellers and business processes change, which could reduce the performance of the model on newer orders. |
| **Risk 5**       | The public proof-of-concept dataset does not contain a direct product-return variable, requiring a proxy target to be used during Part B.                                                |
| **Action 1**     | Inspect the datasets for missing values, duplicates, incorrect data types and inconsistent values before starting the modelling process.                                                 |
| **Action 2**     | Combine the order, product, review, seller, customer and payment datasets using appropriate identifiers.                                                                                 |
| **Action 3**     | Perform exploratory data analysis and feature engineering to identify patterns associated with customer outcomes.                                                                        |
| **Action 4**     | Train and compare different machine learning classification models and evaluate how well they identify negative outcomes.                                                                |
| **Action 5**     | Evaluate models using multiple metrics, including accuracy, precision, recall, F1 score and ROC-AUC.                                                                                     |
| **Assumption 1** | Order, product, customer, seller and review information in the public dataset can be linked using appropriate identifiers.                                                               |
| **Assumption 2** | The public Olist dataset is sufficiently representative to demonstrate the proposed e-commerce machine learning workflow.                                                                |
| **Assumption 3** | The historical data contains enough observations to train and evaluate classification models.                                                                                            |
| **Assumption 4** | Review scores can be used as a proxy classification target for the proof-of-concept stage, while recognising that they do not directly represent product returns.                        |
| **Issue**        | The public Olist dataset does not contain a direct product-return indicator. A proxy target based on customer review scores was therefore created for Part B.                            |
| **Decision 1**   | The Olist dataset will be used as a public proof-of-concept dataset because the actual STADIOalot data is sensitive and was not available for this stage of the project.                 |
| **Decision 2**   | Review scores of 1–2 are classified as Negative Outcome and review scores of 3–5 as Positive Outcome.                                                                                    |
| **Decision 3**   | Logistic Regression and Random Forest will be used as the two classification models for Part B.                                                                                          |
| **Dependency 1** | Data exploration and modelling depend on the required dataset being available and successfully loaded.                                                                                   |
| **Dependency 2** | Data cleaning and preprocessing must be completed before reliable feature engineering and model training can take place.                                                                 |
| **Dependency 3** | Feature engineering must be completed before the machine learning models can be trained.                                                                                                 |
| **Dependency 4** | The target variable must be created consistently before model training and evaluation.                                                                                                   |
| **Dependency 5** | Access to STADIOalot's actual historical returns data would be required to validate the proof-of-concept approach against the real business problem.                                     |

# Project Progression

The project is being developed in stages.

**SS1** established the business problem, motivation, data requirements, repository structure and RAAIDD log for the proposed STADIOalot product returns prediction project.

**Part B** extended this work by demonstrating the proposed data science approach using a publicly available e-commerce dataset. The public dataset was used as a proof of concept before the approach can be applied to sensitive STADIOalot data.

**Part C** evaluated and compared the Logistic Regression and Random Forest models using multiple performance metrics and identified Logistic Regression as the preferred model.

**Part D** provides recommendations based on the results from Part C, including possible model improvements, adaptation to the requested STADIOalot data and comparison with the literature reviewed in Part A.

The future objective is to apply the validated approach to appropriate STADIOalot data containing actual product-return outcomes, subject to data availability and approval.
