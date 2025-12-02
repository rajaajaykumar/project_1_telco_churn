# Customer Churn Analysis — Exploratory Data Analysis (EDA) Report

- Dataset: Telco Customer Churn (Kaggle)
- Rows analyzed: **7032**
- Columns (after feature engineering): **35** / 36 (including the `contract_value_bucket`)


# 1. Introduction

This analysis investigates the factors driving customer churn in a telecom subscription business.
Using Pandas-based analysis and engineered features, the goal is to uncover:

- Who is churning?
- Why are they churning?
- Which behavioral or financial segments pose the highest risk?
- What business actions can reduce churn?

The dataset includes demographics, service usage, contract terms, payment methods, and monthly charges. Feature engineering created additional segments such as ARPU tiers, tenure cohorts, risk flags, and value-perception metrics.

Overall churn rate: **26.58%**


# 2. Key Drivers of Churn (High-Impact Insights)

## 1. Contract Type is the Single Strongest Churn Driver
| Contract Type    | Churn Rate |
|||
| Month-to-month   | **42.7%** |
| One-year         | 11.3%     |
| Two-year         | 2.8%      |

**Customers on month-to-month contracts churn 15× more than two-year customers.**
Low commitment → high switching.


## 2. Payment Method Strongly Influences Churn
| Payment Method           | Churn Rate |
|--||
| Electronic check         | **45.29%** |
| Credit card (auto)       | 15.25%     |
| Bank transfer (auto)     | 16.73%     |
| Mailed check             | 19.20%     |

**Electronic check users churn 3× more** than customers on autopay methods.
This segment is high-risk and price-sensitive.


## 3. Fiber-Optic Customers Are Highly Dissatisfied
| Internet Service | Churn Rate |
|||
| Fiber optic      | **41.89%** |
| DSL              | 18.99%     |
| No internet      | 7.43%      |

Fiber customers pay more and churn more — indicating service or expectation gaps.


## 4. Missing Support Services = ~31% Churn
| Service Missing        | Churn Rate |
|||
| No Online Security     | **31.37%** |
| No Tech Support        | **31.23%** |

Customers without support services churn **twice as much** as customers who have them.

Support availability directly improves retention.


## 5. New Customers Are Extremely High-Risk
| Tenure Group | Churn Rate |
|--||
| 0–12 months  | **47.68%** |
| 13–24        | 28.71%     |
| 25–36        | 21.63%     |
| 61–72        | 6.60%      |

**Nearly half of new customers churn in the first year.**
A strong early-life retention program is needed.


# 3. Revenue & Value-Based Insights

## 6. High-ARPU Customers Churn the Most
| ARPU Tier | Churn Rate |
|--||
| Low       | 15.91%     |
| Medium    | 29.77%     |
| High      | **34.07%** |

**Higher-paying customers are more likely to churn.**
This connects directly to fiber-optic plans and monthly contracts.


## 7. Loyal Customers Generate Higher Lifetime Value
| Segment | LTV (approx.) |
||-|
| Non-churners | **₹2555** |
| Churners     | ₹1531     |

Keeping customers longer dramatically increases lifetime value.
Retention → revenue.


## 8. Contract Value Buckets Show Extreme Behavior
| Contract Value Bucket | Churn Rate |
|||
| Q1 (Low Value)         | 30.63%     |
| Q2                     | **53.04%** |
| Q3                     | 13.21%     |
| Q4 (High Value)        | 9.39%      |

The mid-low contract value segment (Q2) is the **most unstable**, while high-contract-value users show extreme loyalty.


# 4. Demographic Insights (Lower Impact)

## 9. Senior Citizens Churn Significantly More
- Senior citizen churn rate: **41.68%**
- Non-seniors: 23.65%

Seniors may need targeted support or simplified plans.

## 10. Customers with Dependents Are More Loyal
- With dependents: **15.53% churn**
- Without dependents: 31.28%

Families stay longer due to bundled usage and higher switching friction.

## 11. Gender Has No Impact on Churn
- Male: 26.20%
- Female: 26.96%


# 5. Summary of Root Causes

The churn problem is most concentrated among customers who:

- use **electronic check payment**,
- are on **month-to-month** contracts,
- subscribe to **fiber optic** internet,
- lack **support/security services**,
- have **high monthly charges**,
- and are within **their first 12 months** of tenure.

These customers combine to form the **highest-risk churn cluster**.


# 6. Recommendations (Actionable)

1. **Convert month-to-month users to annual contracts** (discounts, incentives).
2. **Target electronic-check customers** with autopay migration offers.
3. **Improve fiber-optic service reliability** and set expectations clearly.
4. Offer **security and tech support bundles** as retention tools.
5. Build **an onboarding program** for customers in their first 0–12 months.
6. Run **targeted outreach** for high-ARPU customers at risk.
7. Tailor offers for **senior citizens** (simplified plans, priority support).


# 7. Files & Outputs

- `cleaned_dataset_v1.csv` — basic cleaned dataset
- `cleaned_featured_dataset_v2.csv` — feature engineered dataset
- `04_data_analysis.ipynb` — groupby and pivot analysis
- `05_full_eda.ipynb` — visualization + EDA
- `report.md` (this file)
- `README.md` (to be assembled on Day 6)


# End of Report

---

# Notes

### Why Recalculate LTV (feature creation)
- The key reason for calculating this new LTV is to create a feature that captures the **monetary expectation** or the **current customer valuation**, rather than relying on the `TotalCharges` which is slightly less than LTV as the customers often receive introductory discounts or special promotions.

| Feature | Calculation | Interpretation | Predictive Value |
| --- | --- | --- |--- |
| `TotalCharges` | Actual historic revenue | What the customer has paid (Backward-looking). | Measures historical worth. |
| `LTV` (New) | `MonthlyCharges` × `tenure` | What the customer is valued at based on their current rate (Forward-looking assumption). | Measures the monetary gap between the current rate and the historical average. |

- By comparing these two values (or by simply using the calculated LTV), the ML model can learn:
    - High Churn Risk: Customers whose TotalCharges is much lower than this calculated LTV are likely high-risk. They have been paying less historically but are now paying a higher MonthlyCharges. This disparity creates a **negative value perception** and is a strong predictor of churn.