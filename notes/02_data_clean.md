# Data Cleaning (Day 2)

## Summary

- Fixed TotalCharges dtype (object → numeric)

- Removed 11 invalid rows (null)

- Converted all relevant object columns → category

- Standardized categorical values (merged “No phone service”, “No internet service”)

<!-- - Cleaned casing (No, data was already clean in terms of case) -->

- Saved cleaned dataset v1

## Structure of Cleaned Data

Rows: **7032**
Columns: **21**

| Type        | Count |
|-------------|-------|
| int64       | 2     |
| float64     | 2     |
| object      | 1     |
| object      | 16    |

## Note: Handling Categorical Dtypes in Pandas
- `.replace()` on Categorical columns raises a FutureWarning when modifying category labels.
- Recommended method: `.cat.rename_categories()`, but it fails if the new label already exists → `ValueError: Categorical categories must be unique.`
- Therefore, renaming "No internet service" → "No" directly on a Categorical column is not allowed.
- Workaround / Best practice:
    - Apply `.replace()` before converting to category → then use `.astype("category")`.
