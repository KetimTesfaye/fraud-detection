End-to-End Fraud Detection Ecosystem

A comprehensive machine learning engine built to identify fraudulent patterns across an E-Commerce Transaction Stream (9.36% native anomaly rate) and a hyper-imbalanced Bank Credit Card Dataset (0.17% fraud rate).

Task 1: Class Imbalance Resolution
Methodology: Implemented SMOTE (Synthetic Minority Over-sampling Technique) strictly within the training folds to balance the banking dataset (50/50 target ratio).
Leakage Prevention: Left the validation and testing datasets completely untouched and raw to ensure accurate production performance tracking.
Alternative Rejected: Random Undersampling was skipped because it would discard over 99% of valid consumer data, stripping out crucial behavioral baselines.

 Task 2: Model Performance & Selection

Models were evaluated side-by-side using F1-Score and **AUC-PR (Area Under Precision-Recall), backed by a Stratified 5-Fold Cross-Validation loop.

E-Commerce Winner: Tuned LightGBM (F1: `0.8023` | AUC-PR: `0.8195` | Variance: `±0.0018`)
Bank Credit Card Winner: Tuned LightGBM + SMOTE (F1: `0.8872` | AUC-PR: `0.8951` | Variance: `±0.0012`)

 Operational Selection Rationale
While Random Forest achieved competitive predictive power, its inference latency reached 115.2ms, failing the real-time Production SLA Threshold of <20ms. LightGBM was selected for production because it delivers elite classification metrics at an ultra-fast inference footprint of 10.5ms.


 Task 3: Model Explainability (SHAP Audit)

Post-hoc interpretability was integrated using game-theoretic **SHAP (Shapley Additive Explanations)** to translate abstract tree leaf splits into auditable, transparent compliance actions.

 Core Fraud Drivers
1. `time_since_signup`: The single strongest global driver. Micro-latencies between registration and checkout trigger extreme fraud risk scores.
2. `device_tx_count`:Identifies velocity spikes associated with automated card-testing bot attacks on a single hardware token.

 Counterintuitive Behavioral Discovery
Low-Value Fraud Camouflage: The model successfully flagged low transaction values as high-risk, unmasking automated dark-web card-testing scripts running micro-purchases (£1.00 - £5.00) designed to evade standard bank alerts.
The Passive Device Trap: Sophisticated fraudsters intentionally throttle and delay transactions across matured profiles to register a clean `device_tx_count`, bypassing basic static perimeter rules.


 How to Run the Pipeline

1. Install dependencies: `pip install pandas numpy scikit-learn lightgbm xgboost imbalanced-learn shap`
2. Run Core Training: Execute `notebooks/modeling.ipynb` to build benchmarks, cross-validate profiles, and serialize weights.
3. Run Audit Engine: Execute `notebooks/shap-explainabllity.ipynb` to inspect global profiles and local true/false incident force plots.