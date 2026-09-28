# STADIOalot-Returns-Prediction
A data science project using machine learning to predict and reduce product returns for STADIOalot.
## Project Title
**Predicting and Reducing E-commerce Product Returns at STADIOalot Using Machine Learning**

## Motivation
STADIOalot is a large online retailer that processes around 58 million orders every year. Although the company continues to grow, one of its biggest challenges is turning that growth into profit. The company currently has an operating margin of only 1.9%, which means that unnecessary costs from picking, packing, delivering and returning orders can have a big impact on the business. When millions of orders are being processed, even a small unnecessary cost on each order can add up to a large amount of money.

One area where I believe there is a good opportunity for improvement is product returns. STADIOalot's overall return rate has increased from 11% to 15%, while returns for clothing and shoes have increased from 22% to 29%. This means that more products are being returned and the company has already spent money processing and delivering these orders before having to deal with the return as well. 
What makes this problem interesting from a data science point of view is that these returns are not always completely random. The information provided by STADIOalot shows that certain products, sellers, sizes, colours and customer buying patterns have higher return rates than others. At the moment, these patterns are not being used to identify high-risk orders when customers are making their purchases. 
For this project, I would like to use STADIOalot's historical data to understand what is causing these returns and then build a machine learning model that can predict the likelihood of a product being returned. STADIOalot already has several years of useful data available, including orders and transactions, product information, delivery information, returns and seller performance. This gives the project a good amount of historical information that can be used to find patterns and train a predictive model. 
The purpose of the model would not be to stop customers from placing orders because they are considered likely to return something. Instead, the prediction could help STADIOalot take action before the return happens. For example, customers could be given better product or sizing information, while products or sellers that continuously have high return rates could be identified and investigated. This would allow the company to use the data it already collects to make better decisions and potentially prevent some avoidable returns.
This project also connects directly with STADIOalot's future strategy. One of the company's priorities is to understand what is causing returns and prevent avoidable returns before an order is confirmed. If the company can identify which orders have a higher chance of being returned and understand why, it could help reduce unnecessary costs while still giving customers a good online shopping experience. This is important for STADIOalot because reducing avoidable costs across millions of orders could contribute towards improving the company's overall profitability.

## Problem Statement
STADIOalot is experiencing an increase in product returns, with the overall return rate increasing from 11% to 15%. The problem is even greater for clothing and shoes, where the return rate has increased from 22% to 29%. With the large number of orders STADIOalot processes every year, these returns create additional costs because the company has already spent money picking, packing and delivering the order before having to process the return.
The information provided by STADIOalot suggests that some returns may be predictable. Certain products, sellers, size and colour combinations and customer buying patterns have higher return rates than others. However, these patterns are currently not being used to identify orders that may have a higher chance of being returned before the purchase is completed.
The problem this project aims to solve is therefore to determine whether STADIOalot's historical data can be used to identify the main factors that contribute to product returns and predict the likelihood of an item being returned. A machine learning model will be developed using information such as previous orders, product details, seller information, customer purchasing patterns and historical returns.
The model would classify orders according to their likelihood of being returned. The results could then help STADIOalot identify higher-risk orders and understand the factors that are contributing to returns. This information could support actions such as improving product descriptions, providing better sizing information or identifying products and sellers that continuously experience high return rates.
The overall aim of the project is to use STADIOalot's existing data to support earlier and more informed decisions that could reduce avoidable product returns and the costs associated with them.

## Repository Structure

The repository has been structured to keep the different parts of the project organised and make it easier to manage the data, code, models, experiments and reports throughout the project.

- **data/raw/** – Contains the original datasets received for the project. The raw data will be kept unchanged.
- **data/processed/** – Contains datasets that have been cleaned and prepared for analysis and machine learning.
- **models/** – Contains the machine learning models developed and saved during the project.
- **experiments/setup/** – Contains the setup and configuration used for the different experiments.
- **experiments/results/** – Contains the results of experiments and model comparisons.
- **scripts/preprocessing/** – Contains scripts used to clean and prepare the data.
- **scripts/statistical_analysis/** – Contains statistical helper and comparison scripts used to analyse the data and compare results.
- **scripts/modelling/** – Contains scripts used to train, test and evaluate the machine learning models.
- **scripts/visualisation/** – Contains scripts used to create graphs and visualisations from the data and model results.
- **notebooks/** – Contains Jupyter notebooks used for data exploration, analysis and testing ideas.
- **reports/** – Contains project reports and supporting documents, including the Data Request report.
