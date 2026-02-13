# Customer Churn Prediction
## Problem Statement

In the highly competitive telecom industry, customers can easily switch between service providers, making retention a critical business challenge. Customer expectations are high, often shaped by individual service experiences—and even brief service disruptions can significantly impact satisfaction. Given the high cost of customer acquisition, retaining existing customers is far more cost-effective than acquiring new ones.

Customer churn refers to subscribers who cancel or fail to renew their services. A high churn rate directly impacts revenue and long-term growth. By analyzing historical customer data and identifying churn drivers, telecom companies can proactively design targeted retention strategies, improve service quality, and enhance overall customer experience.

## Objective

The goal of this project is to predict customer churn by leveraging both numerical and categorical customer attributes. This is formulated as a binary classification problem on an imbalanced dataset, where the objective is to accurately identify customers who are at high risk of churning, enabling timely and data driven intervention strategies.

## Data Understanding & Preprocessing
### Dataset Overview

customerID : Customer ID

gender : Whether the customer is a male or a female

SeniorCitizen : Whether the customer is a senior citizen or not (1, 0)

Partner : Whether the customer has a partner or not (Yes, No)

Dependents : Whether the customer has dependents or not (Yes, No)

tenure : Number of months the customer has stayed with the company

PhoneService : Whether the customer has a phone service or not (Yes, No)

MultipleLines : Whether the customer has multiple lines or not (Yes, No, No phone service)

InternetService : Customer’s internet service provider (DSL, Fiber optic, No)

OnlineSecurity : Whether the customer has online security or not (Yes, No, No internet service)

OnlineBackup : Whether the customer has online backup or not (Yes, No, No internet service)

DeviceProtection : Whether the customer has device protection or not (Yes, No, No internet service)

TechSupport : Whether the customer has tech support or not (Yes, No, No internet service)

StreamingTV : Whether the customer has streaming TV or not (Yes, No, No internet service)

StreamingMovies : Whether the customer has streaming movies or not (Yes, No, No internet service)

Contract : The contract term of the customer (Month-to-month, One year, Two year)

PaperlessBilling : Whether the customer has paperless billing or not (Yes, No)

PaymentMethod : The customer’s payment method (Electronic check, Mailed check, Bank transfer (automatic), Credit card (automatic))

MonthlyCharges : The amount charged to the customer monthly

TotalCharges : The total amount charged to the customer

Churn : Whether the customer churned or not (Yes or No)

### Data Preprocessing

- The dataset doesn't have any missing values
- tenure, MonthlyCharges and TotalCharges features are numerical, all other features are categorical.
- Categorical variables must be transformed into numerical features using appropriate encoding techniques prior to modeling.
- TotalCharges represents numeric values, but it is stored as a string and must be cast to float before modeling.
- TotalCharges = MonthlyCharges × tenure
- Filled missing TotalCharges values only where they were null by recomputing them as MonthlyCharges × tenure, which preserves billing logic instead of using arbitrary imputation.

### Categorical Feature Encoding

Label encoding of categorical variables to prepare the dataset for modeling.
- gender : [0 1] = ['Female' 'Male']
- Partner : [1 0] = ['Yes' 'No']
- Dependents : [0 1] = ['No' 'Yes']
- PhoneService : [0 1] = ['No' 'Yes']
- MultipleLines : [1 0 2] = ['No phone service' 'No' 'Yes']
- InternetService : [0 1 2] = ['DSL' 'Fiber optic' 'No']
- OnlineSecurity : [0 2 1] = ['No' 'Yes' 'No internet service']
- OnlineBackup : [2 0 1] = ['Yes' 'No' 'No internet service']
- DeviceProtection : [0 2 1] = ['No' 'Yes' 'No internet service']
- TechSupport : [0 2 1] = ['No' 'Yes' 'No internet service']
- StreamingTV : [0 2 1] = ['No' 'Yes' 'No internet service']
- StreamingMovies : [0 2 1] = ['No' 'Yes' 'No internet service']
- Contract : [0 1 2] = ['Month-to-month' 'One year' 'Two year']
- PaperlessBilling : [1 0] = ['Yes' 'No']
- PaymentMethod : [2 3 0 1] = ['Electronic check' 'Mailed check' 'Bank transfer (automatic)' 'Credit card (automatic)']
- Churn : [0 1] = ['No' 'Yes']

## Modeling
### Logistic Regression

Logistic regression is a supervised learning method used mainly for classification (often binary).

It models the probability of a class using the sigmoid (logistic) function, producing outputs between 0 and 1.

The model learns weights by maximizing likelihood (or minimizing log loss) and can be regularized (L1/L2) to reduce overfitting.

It’s fast, interpretable (feature weights show direction/strength), and works best when the classes are roughly linearly separable in feature space.

### Random Forest Classifier

A Random Forest Classifier is an ensemble model that builds many decision trees and combines their votes to predict the final class.
Each tree is trained on a bootstrap sample of the data, and at each split it considers a random subset of features, which reduces correlation between trees.
This usually improves accuracy and robustness while lowering overfitting compared to a single decision tree.
Key knobs are n_estimators, max_depth, max_features, and min_samples_leaf; you can also inspect feature importance for interpretability.

### XGBoost Classifier

An XGBoost Classifier is a gradient boosting model that builds trees sequentially, where each new tree corrects the errors of the previous ones.
It minimizes a loss function using gradients, with strong regularization (L1/L2), shrinkage (learning_rate), and row/column subsampling to reduce overfitting.
It often achieves state-of-the-art performance on tabular data, handling nonlinearity and feature interactions very well.
Key parameters include n_estimators, max_depth, learning_rate, subsample, colsample_bytree, and reg_lambda, and it supports early stopping with a validation set.

## Model Evaluation & Comparison
### Evaluation Metrics

In churn prediction, the goal is to identify customers at risk of leaving, not simply to maximize overall accuracy. Due to class imbalance and business impact, the following metrics are more informative.

Recall (Churn = 1) measures the proportion of actual churned customers correctly identified. High recall is critical because missing a churner results in a lost customer with no opportunity for intervention. In practice, it is often preferable to accept some false positives rather than miss high-risk customers.

ROC–AUC evaluates the model’s ability to distinguish between churned and retained customers across all classification thresholds. A higher ROC–AUC indicates stronger ranking capability and is useful for comparing models independent of a fixed decision threshold.

Precision–Recall Tradeoff reflects the balance between identifying true churners and limiting false positives. The optimal tradeoff depends on business costs, such as the expense of retention actions versus the cost of customer loss.

Accuracy alone is insufficient in churn problems because class imbalance can produce deceptively high accuracy while failing to detect churners. As a result, accuracy does not reliably reflect churn prediction performance.
