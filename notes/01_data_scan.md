# Telco Customer Churn — Initial Data Scan (Day 1)

Dataset source: Kaggle — blastchar / Telco Customer Churn  
Rows: **7043**
Columns: **21**

---

## 1. Structure Overview

| Type        | Count |
|-------------|-------|
| int64       | 2     |
| float64     | 1     |
| object      | 18    |

Most categorical columns are stored as `object`, which will be converted to `category` during cleaning.

---

## 2. Key Observations

### • Incorrect Data Type in `TotalCharges`
`TotalCharges` is stored as `object`.  
This typically happens because some entries contain `" "` (empty strings).  
These values will convert to `NaN` when casting to float.

### • No Missing Values (But This is Misleading)
The dataset reports **0 missing values**, but actual missing values are hidden inside `TotalCharges` as blank strings.  
Needs manual conversion.

### • No Duplicate Rows
`df.duplicated()` returned **0** duplicates.

### • Categorical Distribution
- Most categories have **2–3 unique values**
- PaymentMethod has **4 values**
- MonthlyCharges and TotalCharges have **high cardinality** as expected for continuous features.

### • Tenure Distribution Is Bimodal
Strong peaks at:
- **0–5 months** (new customers who churn quickly)
- **65–72 months** (long-term loyal customers)

This supports U-shaped tenure behavior.

### • MonthlyCharges Distribution
Slight negative skew.  
Continuous and wide-spread (1585 unique values).  
Will require binning for feature engineering.

---

## 3. Initial Data Quality Summary

| Column         | Issue                             | Action Needed                  |
|----------------|-----------------------------------|--------------------------------|
| TotalCharges   | Wrong dtype (object), hidden NA   | Convert to numeric, clean      |
| Categorical    | Stored as object                  | Convert to category            |
| tenure         | Bimodal distribution              | Consider bucketing later       |
| MonthlyCharges | High cardinality                  | Bin during feature engineering |

---

## 4. Next Steps (Day 2)

1. Fix `TotalCharges` dtype and missing values  
2. Convert all categorical columns into `category`  
3. Normalize inconsistent category labels  
4. Re-check numerical distributions  
5. Prepare cleaned dataset v1

---

**End of Day 1**
