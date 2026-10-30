SS3 Part B – Application of Final Method to Client Data
Project Overview
This project applies the final machine learning method developed for the STADIOalot Returns Prediction project to newly supplied client data.

The aim is to predict whether an individual order item is likely to be returned using historical customer, seller and product information available at the time an order is placed.

The prediction is performed at the Order_Item_ID level, with each record representing a single purchased item.

Business Problem
Product returns create significant operational costs in e-commerce environments. These costs include reverse logistics, inspection, repackaging, refunds and inventory management.

The objective of this project is to identify factors associated with product returns and develop a predictive model capable of identifying higher-risk order items before a return occurs.

This information can assist STADIOalot in reducing avoidable returns and improving profitability.

Datasets Used
The following datasets were supplied as part of the client data extract:

table2_orders_and_transactions.csv
table3_product_catalogue.csv
table4_returns.csv
table5_customer_purchase_and_return_history.csv
table6_marketplace_seller_data.csv
The delivery dataset was not used for model training because several delivery variables only become available after the order has been placed and would introduce data leakage.

Target Variable
A new target variable called Returned was created.

Definition:

Returned = 1 if the Order_Item_ID appears in the returns dataset.
Returned = 0 if the Order_Item_ID does not appear in the returns dataset.
This converted the business problem into a binary classification problem.

Data Preparation
The following preprocessing activities were performed:

Loaded and inspected all client datasets.
Identified duplicate records.
Merged order, product, customer and seller information.
Created the Returned target variable.
Handled missing values.
Encoded categorical variables.
Split the dataset into training, validation and testing subsets.
To prevent data leakage, the dataset was split using Customer_ID rather than individual order items.

Machine Learning Model
A Random Forest Classifier was selected for this project.

Model parameters:

n_estimators = 200
max_depth = 15
class_weight = balanced
random_state = 42
The model was trained using the training dataset and evaluated using unseen test data.

Model Evaluation
The following evaluation metrics were calculated:

Accuracy
Precision
Recall
F1 Score
These metrics were used to assess the effectiveness of the return prediction model.

Results Visualisation
The following visualisations were produced:

Return Distribution
Confusion Matrix
Feature Importance Chart
These visualisations help explain the performance of the model and identify the variables most strongly associated with product returns.

Repository Structure
data/raw/                 Client datasets

notebooks/SS3_PartB.ipynb Main implementation notebook

README.md                 Main project documentation

README_SS3_PartB.md       SS3 Part B documentation
