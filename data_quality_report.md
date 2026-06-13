# Data Quality Report

## D2C Customer Churn Capstone — Part 1

**Author:** Dhananjay Singh

**Snapshot Date:** 2025-09-30

**Datasets Audited:** customers, orders, support_tickets, web_events_snapshot, churn_labels, intervention_history


---

## 1. Missing Values

| File          | Column         | Missing Count | Missing % | Observation                                                                                                                                              |
| ------------- | -------------- | ------------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| customers.csv | `loyalty_tier` | ~1,386        | ~57.8%    | Most of the missing values belong to customers who are not enrolled in the loyalty programme. These can be labelled as `"Non-enrolled"` during analysis. |
| customers.csv | `skin_type`    | ~401          | ~16.7%    | This is a self-reported field and some customers chose not to provide it. Missing values can be labelled as `"Not Provided"`.                            |
| orders.csv    | `rating`       | ~80           | ~0.8%     | Product ratings are optional, which explains the small number of missing values. Average ratings can be calculated using available ratings only.         |


**Impact on Analysis:** Most missing values are found in `loyalty_tier`. These values mainly represent customers who never joined the loyalty programme rather than a problem in data collection.

---

## 2. Duplicate-like Records

| File          | Issue                   | Count        | Detail                                                                                           |
| ------------- | ----------------------- | ------------ | ------------------------------------------------------------------------------------------------ |
| orders.csv    | `_DUP` suffix order IDs | 12 | These records were intentionally added and should be removed before any analysis or aggregation. |
| orders.csv    | Fully duplicate rows    | 0            | No completely duplicated rows were found.                                                        |
| customers.csv | Duplicate `customer_id` | 0            | Each customer appears only once.                                                                 |

**Action Taken:** All orders with an `_DUP` suffix were removed before calculating customer-level features.

---

## 3. Post-Snapshot Orders

| File       | Column       | Observation                                                                                                  |
| ---------- | ------------ | ------------------------------------------------------------------------------------------------------------ |
| orders.csv | `order_date` | Some orders occur after 2025-09-30. These orders belong to the future period used to determine churn status. |

**Count:** 1872.

**Action Taken:** Only orders on or before 2025-09-30 were used for feature creation. Orders after the snapshot date were excluded.

---

## 4. Outliers

| File       | Column         | Observation                                                                                                                             |
| ---------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| orders.csv | `gross_amount` | A small number of orders have very high values, reaching up to approximately ₹24,789, while most orders fall within a much lower range. |

**Action Taken:**

* Outliers were retained during analysis because they may represent genuine high-value purchases.
* Spending-related features can be log-transformed during modeling if required.

---

## 5. Date Consistency

| Check                                        | Finding                                  |
| -------------------------------------------- | ---------------------------------------- |
| `signup_date` after snapshot                 | No cases found.                          |
| `ticket_date` after snapshot                 | No cases found.                          |
| First order before signup                    | No cases found.                          |
| `web_events_snapshot` `snapshot_date` column | All rows contain 2025-09-30 as expected. |

---

## 6. Join / Key Issues

| Join                             | Finding                                                                                     |
| -------------------------------- | ------------------------------------------------------------------------------------------- |
| customers ↔ orders               | Some customers have no orders. Missing aggregated values should be handled appropriately.   |
| customers ↔ support_tickets      | Some customers never raised a support ticket. This is expected.                             |
| customers ↔ web_events           | One-to-one match across customers.                                                          |
| customers ↔ churn_labels         | One-to-one match across customers.                                                          |
| customers ↔ intervention_history | One-to-one match across customers.                                                          |


---

## 7. Columns That Require Attention

| Column                   | File                     | Reason                                                                                     |
| ------------------------ | ------------------------ | ------------------------------------------------------------------------------------------ |
| Orders after 2025-09-30  | orders.csv               | These belong to the future churn period and should not be used as features.                |
| `churn_next_60d`         | churn_labels.csv         | This is the target variable and should not be included as an input feature.                |
| `split`                  | churn_labels.csv         | Used only for train, validation, and test assignment.                                      |
| `manual_priority_bucket` | intervention_history.csv | Can be useful for analysis but may already reflect business knowledge about customer risk. |

---

## 8. Summary of Data Preparation Steps

1. Keep only orders on or before 2025-09-30.
2. Remove all orders with `_DUP` in the order ID.
3. Replace missing `loyalty_tier` values with `"Non-enrolled"`.
4. Replace missing `skin_type` values with `"Not Provided"`.
5. Calculate average ratings using available ratings only.
6. Replace missing `ticket_count` values with 0 after aggregation.
7. Apply log transformation to spending-related features if needed during modeling.
8. Exclude `churn_next_60d` from the feature set since it is the target variable.
