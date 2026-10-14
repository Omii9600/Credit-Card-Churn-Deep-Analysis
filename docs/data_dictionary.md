# Data dictionary

Source file: `BankChurners.csv` (10,127 rows x 23 columns). Descriptions are as interpreted in this project; the original dataset page on Kaggle is the authority on definitions.

## Original columns used (21)

| Column | Type | Meaning | Notes |
|---|---|---|---|
| `CLIENTNUM` | id | Unique customer identifier | 10,127 unique values |
| `Attrition_Flag` | category | `Existing Customer` or `Attrited Customer` | Target: 1,627 attrited (16.07%) |
| `Customer_Age` | int | Age in years | 26-73 |
| `Gender` | category | `F` / `M` | No women in income bands above \$60K |
| `Dependent_count` | int | Number of dependents | 0-5 |
| `Education_Level` | category | Highest education | 15.0% `Unknown` |
| `Marital_Status` | category | Marital status | 7.4% `Unknown` |
| `Income_Category` | category | Annual income band | 11.0% `Unknown` |
| `Card_Category` | category | Blue / Silver / Gold / Platinum | 93.2% Blue |
| `Months_on_book` | int | Tenure with the bank, in months | 13-56; no customers in their first year |
| `Total_Relationship_Count` | int | Number of bank products held | 1-6 |
| `Months_Inactive_12_mon` | int | Months with no card activity in the last 12 | 0-6 |
| `Contacts_Count_12_mon` | int | Contacts between bank and customer in the last 12 months | 0-6; direction of contact not recorded |
| `Credit_Limit` | float | Card credit limit (\$) | Capped at 34,516 |
| `Total_Revolving_Bal` | int | Balance carried month to month (\$) | 0-2,517; 5% of customers at the cap |
| `Avg_Open_To_Buy` | float | Unused credit line (\$) | Exactly `Credit_Limit - Total_Revolving_Bal`; redundant |
| `Total_Amt_Chng_Q4_Q1` | float | Q4 transaction amount as a multiple of Q1 | 1.0 = unchanged |
| `Total_Trans_Amt` | int | Total transaction amount, last 12 months (\$) | 510-18,484 |
| `Total_Trans_Ct` | int | Total transaction count, last 12 months | 10-139 |
| `Total_Ct_Chng_Q4_Q1` | float | Q4 transaction count as a multiple of Q1 | 1.0 = unchanged; core early-warning signal |
| `Avg_Utilization_Ratio` | float | Revolving balance / credit limit | 0-0.999; equals `Total_Revolving_Bal / Credit_Limit` to rounding |

## Original columns dropped (2)

| Column | Why |
|---|---|
| `Naive_Bayes_Classifier_..._1` | Output of a classifier that had already predicted attrition. AUC against the real outcome = 1.000. Leakage. |
| `Naive_Bayes_Classifier_..._2` | Complement of the above (the two sum to 1 on every row). AUC = 0.000. Leakage. |

## Engineered columns

| Column | Definition | Used in |
|---|---|---|
| `churn` | 1 if `Attrition_Flag == "Attrited Customer"` else 0 | everywhere |
| `avg_ticket` | `Total_Trans_Amt / Total_Trans_Ct` | Step 3, Q2 |
| `age_band` | 26-35, 36-45, 46-55, 56-65, 66-73 | Fig. 4 |
| `tenure_band` | 13-24, 25-36, 37-48, 49-56 months | Fig. 4 |
| `act_tier` | `<=45`, `46-65`, `>65` transactions | Q5, Q6, Q7 |
| `ct_bin`, `amt_bin`, `ctchg_bin`, `rb_bin` | Bins of transactions, spend, Q4/Q1 count ratio, revolving balance | Q2, Q3, Q7 |
| `ct_band`, `chg_band` | Coarser bins used for the activity x trend heat map | Q3 |
| `contacts_grp`, `inactive_grp` | Grouped contacts (0-1, 2-3, 4-6) and inactivity (0-1, 2, 3, 4-6) | Q4 |
| `util_flag` | `Zero utilisation` if `Avg_Utilization_Ratio == 0`, else `Revolving (>0)` | Q7 |
| `rev_est` | **Illustrative** annual revenue: `1.5% x Total_Trans_Amt + 15% x Total_Revolving_Bal` | Q8, Q11 |
| `spend_decile` | Decile of `Total_Trans_Amt` (D1 lowest, D10 highest) | Q8, Q9 |
| `persona` | High-activity core / Steady light users / Cooling regulars / Single-product drifters / Exit-bound (rules in Q9) | Q9 |
| `is_train` | Stratified 70/30 split, seed 42 | Q9, Q10, Q11 |
| `score`, `decile` | Scorecard risk score and risk decile on the held-out customers | Q10, Q11 |

## Assumptions (set once at the top of the notebook; illustrative only)

| Name | Default | Meaning |
|---|---|---|
| `INTERCHANGE_RATE` | 0.015 | Share of card spend earned as interchange |
| `REVOLVE_YIELD` | 0.15 | Annual yield on revolving balances, net of write-offs |
| `COST_PER_CONTACT` | 10.0 | Cost of one retention outreach including expected offer cost |
