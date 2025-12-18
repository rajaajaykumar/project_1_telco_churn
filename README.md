# Telco Customer Churn Analysis (Pandas Project)

This project analyzes customer churn behavior for a telecom company using **Python + Pandas**, focusing on customer segmentation, churn drivers, and business insights.

**Goal:**  
Identify why customers churn and which customer segments are at the highest risk, using pure Pandas (no ML), and produce a clear narrative with actionable insights.

---

# Project Highlights

- Complete cleaning of the raw Telco churn dataset  
- Exploratory Data Analysis (EDA)  
- Segmentation using:
  - Tenure cohorts  
  - ARPU tiers  
  - Contract length  
  - Payment method risk  
- Feature engineering to produce ML-ready dataset  
- 12+ high-impact business insights  
- Professional documentation & reproducible analysis  

---

# Key Business Insights

### **1. Contract Type Is the Biggest Churn Driver**
Month-to-month customers churn at **42.7%**, compared to **2.8%** for two-year contracts.

### **2. Electronic Check Users Are Extremely High-Risk**
Churn rate: **45.29%** — nearly triple that of autopay customers.

### **3. Fiber Optic Customers Are Dissatisfied**
Churn rate: **41.89%**, double DSL.

### **4. Lack of Support Services Predicts Churn**
Customers without TechSupport or OnlineSecurity churn at ~31%.

### **5. New Customers (0–12 months) Have 47.68% Churn**
Early churn dominates; retention must focus here.

### **6. High-ARPU Customers Churn More**
Contrary to expectation — high-paying segments churn at **34.07%**.

*(Full list of insights in `report.md`)*

---

# Feature Engineering Included

| Feature | Description |
|--------|-------------|
| `tenure_group` | Cohort buckets (0–12, 13–24, …) |
| `ARPU_tier` | Low/Medium/High revenue segmentation |
| Risk Flags | is_fiber, is_monthly_contract, is_electronic_check |
| Protection Flags | no_tech_support, no_online_security |
| `avg_cost_per_month` | TotalCharges / tenure |
| `contract_value_proxy` | MonthlyCharges × contract_length |

These improve downstream modeling and interpretability.

---

# Project Structure

```text
├── data/
│ ├── raw/
│ ├── processed/
├── notebooks/
├── reports/
│ └── report.md
└── README.md
```

---

# How to Run

```bash
pip install -r requirements.txt
jupyter notebook
```

Open the notebooks in order:

1. 01_data_load_and_scan.ipynb
2. 02_cleaning.ipynb
3. 03_analysis_groupby.ipynb
4. 04_feature_engineering.ipynb
5. 05_full_eda.ipynb

---

# Deliverables
- cleaned_dataset_v1.csv
- cleaned_featured_dataset_v2.csv
- All notebooks (analysis + feature engineering)
- report.md (business narrative)
- README.md
- Skills demonstrated: Pandas, EDA, feature engineering, business analytics, data storytelling.
