# BigQuery ML Transaction Forecasting Lab

This project is a lab exercise completed during the Machine Learning Engineer Learning Path course. It demonstrates a sequence of BigQuery ML queries to build and evaluate a logistic regression model that predicts customer transactions based on website traffic data from Google Analytics.

The project utilizes the `bigquery-public-data.google_analytics_sample` dataset to train and evaluate the model. The model uses features such as operating system, mobile device usage, country, and pageviews to predict whether a visitor will make a transaction.

## Workflow

This lab involves the following steps:

1. Create a BigQuery dataset
   
2. Create a BigQuery ML model: SQL query in the file `01_create_model.sql`
   ![Creating a BigQuery ML model](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/1.1.png)
   ![Creating a BigQuery ML model - Evaluation](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/1.2.png)
   
3. Evaluate the model: SQL query in the file `02_evaluate_model.sql`
   ![Evaluating the model](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/2.png)

4. Predict purchases per country: SQL query in the file `03_predict_country.sql`
   ![Predicting transactions by country](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/3.png)

5. Predict purchases per user: SQL query in the file `04_predict_user.sql`
   ![Predicting transactions per user](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/4.png)

### Prerequisites

- A Google Cloud Project.
- Access to BigQuery.

### Dataset

- The project uses the public dataset: `bigquery-public-data.google_analytics_sample`.
