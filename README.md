## Bank Marketing Dataset

## The Task
The dataset used in this project is the **Bank Marketing Additional Full Dataset**, available on Kaggle:  
[Bank Marketing Dataset on Kaggle](https://www.kaggle.com/datasets/sahistapatel96/bankadditionalfullcsv)

It contains information about direct marketing campaigns (phone calls) of a Portuguese banking institution.  
The classification goal is to predict whether a client will subscribe to a term deposit (`y`).

---

## Approach

1. **Data Preprocessing**
   - One-hot encoding of categorical variables.
   - Handling missing values.
   - Scaling of numerical features.

2. **Models Tested**
   - Logistic Regression  
   - k-Nearest Neighbors  
   - Decision Tree  
   - Random Forest  
   - LightGBM (baseline, tuned with RandomizedSearch, tuned with Hyperopt)

3. **Evaluation Metrics**
   - ROC AUC  
   - Accuracy  
   - Precision, Recall, F1-score (for both classes)  
   - Confusion Matrix  

4. **Threshold Tuning**
   - Default threshold 0.5 was adjusted (0.55–0.65) to balance FP/FN.  
   - Business rule: if `campaign > 6`, prediction forced to "no" (to reduce unnecessary calls).  

---

##  Results

We list only those results where training accuracy and testing accuracy are nearly equal and high enough.

| Model                     | ROC_AUC | Acc  | P_no | R_no | F1_no | P_yes | R_yes | F1_yes |
|----------------------------|---------|------|------|------|-------|-------|-------|--------|
| Logistic Regression        | 0.7996  | 0.82 | 0.95 | 0.84 | 0.89  | 0.34  | 0.67  | 0.45   |
| kNN                        | 0.7308  | 0.89 | 0.91 | 0.97 | 0.94  | 0.51  | 0.23  | 0.32   |
| Decision Tree              | 0.7965  | 0.84 | 0.95 | 0.86 | 0.90  | 0.37  | 0.63  | 0.46   |
| LightGBM (baseline)        | 0.8126  | 0.85 | 0.95 | 0.88 | 0.91  | 0.40  | 0.66  | 0.50   |
| LightGBM (RandomizedSearch)| 0.8153  | 0.85 | 0.95 | 0.87 | 0.91  | 0.40  | 0.66  | 0.50   |
| LightGBM (Hyperopt)        | 0.8058  | 0.82 | 0.95 | 0.84 | 0.89  | 0.35  | 0.66  | 0.46   |

---

## SHAP Analysis

- **Most important features:**
  - `duration` – longer calls increase probability of "yes".
  - `campaign` – many repeated calls decrease probability of "yes".
  - `poutcome` – success in previous campaigns increases probability.
  - `month` – some months (March, September) are more favorable.

- **Interpretation:**  
  The model learns that **longer conversations and fewer repeated contacts** are strong indicators of success.  
  SHAP confirms that the business rule for `campaign` is consistent with model behavior.

---

## Next Steps
- Test adaptive thresholds for different client segments.  
- Apply probability calibration (Platt/Isotonic).  
- Deploy the model into CRM to prioritize calls.  
- Monitor live performance and adjust thresholds dynamically.  
