# Flight Delay Prediction & Operational Optimization

## Overview

This project predicts whether a U.S. domestic flight is likely to be delayed by 15 minutes or more before departure. The goal is to help airline operations teams move from reactive delay management to proactive decision-making by identifying high-risk flights early enough to support staffing, gate planning, passenger communication, and recovery actions.

The project uses 2024 U.S. Bureau of Transportation Statistics (BTS) On-Time Performance data and applies machine learning models to classify flight delay risk using only pre-departure information.

## Business Problem

Flight delays create downstream operational issues across aircraft scheduling, crew planning, gate availability, passenger rebooking, and customer experience. A single delayed flight can propagate through the network when the same aircraft, crew, or airport capacity is reused later in the day.

The key question addressed in this project is:

> Can we predict departure delay risk before takeoff using schedule, carrier, airport, route, and historical performance signals?

The model is not intended to catch every delayed flight. Instead, it identifies a high-confidence subset of flights where proactive intervention may be most useful.

## Dataset

**Source:** U.S. Bureau of Transportation Statistics (BTS) On-Time Performance Data
**Period:** January 2024 to December 2024
**Raw data size:** Approximately 6.7 million U.S. domestic flight records
**Sample used:** 15% chunk-based random sample
**Cleaned modeling dataset:** 1,044,833 active departures
**Target variable:** Departure delay of 15 minutes or more

Cancelled and diverted flights were removed because they do not represent standard departure delay behavior. Records with missing departure delay values were also excluded.

## Tools & Technologies

* Python
* pandas
* NumPy
* scikit-learn
* XGBoost
* matplotlib
* seaborn
* Jupyter Notebook

## Feature Engineering

The model uses only information available before departure to avoid data leakage.

Key features include:

* Month
* Day of month
* Day of week
* Scheduled departure hour
* Weekend indicator
* Peak-hour indicator
* Holiday indicator
* Distance
* Distance group
* Airline carrier
* Origin airport
* Destination airport
* Carrier historical delay rate
* Origin airport historical delay rate

Historical delay-rate features were calculated using the training data only and then mapped to the test set to prevent future information from leaking into the model.

## Modeling Approach

Three classification models were trained and compared:

1. Logistic Regression
2. Random Forest
3. XGBoost

The workflow included:

* Data cleaning and preprocessing
* Leakage-safe feature engineering
* Train/test split with stratification
* Model training and comparison
* Feature importance analysis
* Permutation importance analysis
* Partial dependence interpretation
* Business impact estimation

## Model Performance

XGBoost performed best across the main classification metrics.

| Model               | Accuracy | Precision | Recall |     F1 | ROC AUC |
| ------------------- | -------: | --------: | -----: | -----: | ------: |
| Logistic Regression |   0.7932 |    0.3912 | 0.0039 | 0.0078 |  0.6578 |
| Random Forest       |   0.7980 |    0.6313 | 0.0505 | 0.0934 |  0.7155 |
| XGBoost             |   0.8030 |    0.6248 | 0.1131 | 0.1915 |  0.7295 |

The XGBoost model achieved:

* **ROC AUC:** 0.7295
* **Accuracy:** 80.3%
* **Precision:** 62.5%
* **Recall:** 11.3%

The relatively high precision suggests the model is useful for selective alerting, where airlines act on a smaller set of high-risk flights instead of flagging the entire network.

## Key Insights

The analysis showed that flight delay risk is not randomly distributed. Delay patterns varied strongly by:

* Time of day
* Month and season
* Airline carrier
* Origin airport
* Historical delay behavior

The strongest predictors included scheduled departure hour, month, carrier historical delay rate, and origin airport historical delay rate.

Operationally, this means airlines can use pre-departure risk scoring to prioritize recovery resources where they are most likely to create value.

## Business Impact

The model was evaluated as a selective alerting tool. Under conservative assumptions, proactive risk scoring could help airlines identify a subset of delay-prone flights early and support targeted interventions.

Estimated annual impact:

* Overall delay rate in sample: 20.6%
* Estimated annual delayed flights: 1,382,210
* Delays caught proactively: 156,304
* Assumed avoidable cost per delayed flight: $85
* Estimated annual savings: approximately **$3.99M**

## Recommendations

Based on the model results, the project recommends:

1. Use pre-departure delay risk scoring for operational triage.
2. Add schedule buffers for high-risk evening flights.
3. Prioritize carrier-specific and airport-specific interventions.
4. Use chain-breaking actions for late-aircraft and carrier-related delay patterns.
5. Expand future models with weather forecasts, route congestion, and aircraft rotation data.

## My Contributions

This was completed as an academic team project. My major contributions included:

* Cleaning and preparing BTS flight data for modeling
* Engineering leakage-safe historical delay features
* Building and comparing classification models
* Evaluating model performance using ROC AUC, precision, recall, and F1-score
* Interpreting model outputs using feature importance and permutation importance
* Translating model results into operational recommendations and business impact estimates
* Preparing analysis outputs for final reporting and presentation

## Repository Structure

```text
Flight-Delay-Prediction-ML/
│
├── Flight_Delay_Analysis.ipynb      # Main analysis and modeling notebook
├── Data_Dictionary.docx             # Variable definitions and feature documentation
├── README.md                        # Project overview and documentation
└── .gitignore                       # Files excluded from version control
```

## Dataset Note

The full BTS dataset is not included in this repository because of file size. The data can be downloaded from the U.S. Bureau of Transportation Statistics TranStats portal.

Dataset source:
https://www.transtats.bts.gov/

## Future Improvements

Future work can improve the model by adding:

* Weather forecast data
* Aircraft tail-number rotation history
* Airport congestion indicators
* Route-level delay propagation features
* Delay duration prediction instead of binary classification
* Real-time operational scoring pipeline

## Project Status

Completed as part of a graduate-level Programming for Data Science project.
