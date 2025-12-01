# Feature Engineering + Outlier Detection (Day 4)

## Steps

- Step 1 — Outlier Detection (MonthlyCharges, TotalCharges, LTV)

- Step 2 — Tenure Buckets (already created, refine as a categorical feature)

- Step 3 — ARPU Tiers (Low / Medium / High revenue)
    - create revenue tiers using quantiles
    - 3-level segmentation, useful for EDA & ML

- Step 4 — Risk Flags (high risk payment method, fiber, no security, etc.)
    - convert the strongest churn predictors into direct risk flags.
    - High-risk payment (electronic check users)
    - High-risk contract type (month-to-month)
    - High-risk internet service (Fiber optic)
    - Missing protection services (TechSupport, OnlineSecurity, DeviceProtection)
    - Senior citizen risk flag (strong demographic predictor)

- Step 5 — Ratio Features (Value-Perception Metrics)
    - These features help measure whether customers perceive good value
    - This helps identify:
        - high-value short-term customers
        - long-term low-value customers
        - mispriced segments
    - Cost per month of tenure (stability indicator) — historical average
    - Protection-to-cost ratio (expected value indicator)
    - Contract value proxy (monthly charges × contract length assumption)

- Step 6 — Export cleaned_featured_dataset_v2.csv
    - Final Validation & Featured dataset export

## Summary

Rows: **7032**
Columns: **35**

Engineered features:
- tenure_group
- ARPU_tier
- Flags: is_electronic_check, is_monthly_contract, is_fiber, no_*, is_senior
- Ratios: avg_cost_per_month, security_to_cost_ratio
- Economics: contract_length, contract_value_proxy

## Notes

### Why `reorder_categories` Matters for Ordinal Tenure Groups — Step 2 (Tenure Buckets)

`reorder_categories` is Necessary to establish an **Ordinal Relationship** among the tenure groups.

| Key | Explanation |
| --- | --- |
| Ordinal Data: | Tenure groups (0-12, 13-24, etc.) have an inherent rank or order. 13-24 is unambiguously greater than 0-12 |
| Pandas Default: | When pd.cut creates the categories, it sets the `ordered=True` flag for the categories **by default**. However, when you then run `.astype('category')`, the resulting categories might sometimes lose this ordering or adopt an alphabetical/lexicographical order if not explicitly set |
| Explicit Ordering: | Running reorder_categories **explicitly confirms and enforces** the correct sequential order you define, ensuring Pandas and subsequent machine learning libraries (especially those that process ordered data, like decision trees or certain forms of encoding) correctly interpret that the category 61-72 is "higher" than 0-12 |
| Model Stability: | It makes the feature stable and clean for modeling. Although many models (like One-Hot Encoding) ignore order, explicitly defining the ordinal nature protects against inconsistent behavior and is essential if you use Ordinal Encoding |

### 2. Interpreting the Security-to-Cost Risk Ratio — Step 5 (Ratio Features)

Scenario (for `security_to_cost_ratio`):
- A. High Cost, High Risk: customer pays high MonthlyCharges, but lacks security (numerator=1, denominator/cost= -> risk=high)
- B. Low Cost, High Risk: customer pays low MonthlyCharges, but lacks security (numerator=1, denominator/cost=low -> risk=higher)
- C. Low Risk (Has Security): customer has security (numerator=0 -> risk=0)

The ML model will learn that a higher ratio (i.e., a smaller denominator, meaning low Monthly Charges for a high-risk (no security) customer) is associated with a higher probability of churn. It helps the model find vulnerable customers who are likely to perceive their cheap, unprotected service as poor value.

Insights:
- A's Risk (0.01): The risk comes from paying a premium for a service that is missing a basic feature (Online Security).
- B's Risk (0.05): The risk comes from being a cheap, unprotected customer who is easily swayed by any new, moderately-priced competitor that offers better value/security.
- The ML model learns: "Customers who are cheap and unprotected are the most likely to leave."