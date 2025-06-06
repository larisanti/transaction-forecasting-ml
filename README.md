# BigQuery ML Transaction Forecasting Lab

This project is a lab exercise completed during the Machine Learning Engineer Learning Path course. It demonstrates a sequence of BigQuery ML queries to build and evaluate a logistic regression model that predicts customer transactions based on website traffic data from Google Analytics.

The project utilizes the `google_analytics_sample` dataset to train and evaluate the model. The model uses features such as operating system, mobile device usage, country, and pageviews to predict whether a visitor will make a transaction.

## Workflow

First, a BigQuery dataset is created, then:

## 1.  **Create a BigQuery ML model:**
   
```sql
CREATE OR REPLACE MODEL `bqml_lab.sample_model`
OPTIONS(model_type='logistic_reg') AS
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*` -- dataset: Google Analytics sample data
WHERE
  _TABLE_SUFFIX BETWEEN '20160801' AND '20170631'
LIMIT 100000; -- limit to 100,000 rows to speed up training
```

![Creating a BigQuery ML model](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/1.1.png)
![Creating a BigQuery ML model - Evaluation](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/1.2.png)

          
---
## 2.  **Evaluate the model:**

```sql
SELECT
  *
FROM
  ml.EVALUATE(MODEL `bqml_lab.sample_model`, (
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label, -- features used for prediction
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'));
```

![Evaluating the model](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/2.png)


---
## 3.  **Predict purchases per country:**

```sql
SELECT
  country,
  SUM(predicted_label) as total_predicted_purchases -- total predicted purchases for the country
FROM
  ml.PREDICT(MODEL `bqml_lab.sample_model`, (
SELECT
  IFNULL(device.operatingSystem, "") AS os, -- features for prediction
  device.isMobile AS is_mobile,
  IFNULL(totals.pageviews, 0) AS pageviews,
  IFNULL(geoNetwork.country, "") AS country
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'))
GROUP BY country
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```

![Predicting transactions by country](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/3.png)

---
## 4.  **Predict purchases per user:**

```sql
SELECT
  fullVisitorId,
  SUM(predicted_label) as total_predicted_purchases -- total predicted purchases for each user
FROM
  ml.PREDICT(MODEL `bqml_lab.sample_model`, ( -- apply the trained model for prediction
SELECT
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(totals.pageviews, 0) AS pageviews,
  IFNULL(geoNetwork.country, "") AS country,
  fullVisitorId
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'))
GROUP BY fullVisitorId
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```

![Predicting transactions per user](https://github.com/larisanti/transaction-forecasting-ml/blob/main/Screenshots/4.png)


## Prerequisites

* A Google Cloud Project
* Access to BigQuery

## Dataset

This project utilizes the following public dataset:

* `bigquery-public-data.google_analytics_sample`

