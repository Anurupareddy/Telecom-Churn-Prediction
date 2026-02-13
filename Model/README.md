# Key Evaluation Metrics for Churn Prediction

In churn prediction, the goal is to identify customers at risk of leaving, not simply to maximize overall accuracy. Due to class imbalance and business impact, the following metrics are more informative.

Recall (Churn = 1) measures the proportion of actual churned customers correctly identified. High recall is critical because missing a churner results in a lost customer with no opportunity for intervention. In practice, it is often preferable to accept some false positives rather than miss high-risk customers.

ROC–AUC evaluates the model’s ability to distinguish between churned and retained customers across all classification thresholds. A higher ROC–AUC indicates stronger ranking capability and is useful for comparing models independent of a fixed decision threshold.

Precision–Recall Tradeoff reflects the balance between identifying true churners and limiting false positives. The optimal tradeoff depends on business costs, such as the expense of retention actions versus the cost of customer loss.

Accuracy alone is insufficient in churn problems because class imbalance can produce deceptively high accuracy while failing to detect churners. As a result, accuracy does not reliably reflect churn prediction performance.

# Model Comparison and Business Recommendation

All three models demonstrate strong ability to identify churners, with recall for churned customers (Class 1) close to or above 0.78, which is critical for retention-focused use cases.

Logistic Regression provides a solid baseline with a churn recall of 0.79 and ROC–AUC of 0.80. However, its lower precision for churners results in more false positives, increasing unnecessary retention costs.

Random Forest improves overall performance, achieving a higher ROC–AUC of 0.83 and better balance between precision (0.50) and recall (0.78) for churners. This leads to more efficient targeting of retention efforts with fewer wasted interventions.

XGBoost delivers the highest ROC–AUC (0.84) and the highest churn recall (0.82), indicating the strongest ability to rank high-risk customers correctly. While precision remains comparable to other models, the improved recall ensures fewer churners are missed.

XGBoost is the preferred model for business deployment, as it maximizes churner detection while providing the best overall ranking performance. This enables earlier and more accurate identification of high-risk customers, supporting targeted retention strategies and improved revenue protection.
