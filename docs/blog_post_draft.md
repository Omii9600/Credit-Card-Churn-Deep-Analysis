# I analysed 10,127 credit card customers. Demographics told me nothing - behaviour told me everything.

*Draft for Medium (optional Step 7). Upload the figures from `images/` and embed code cells from the notebook.*

---

A bank loses **16 out of every 100** credit card customers. The obvious next step is to ask *who* they are: older or younger, richer or poorer, married or single?

I tested exactly that on a Kaggle dataset of 10,127 customers. The answer surprised me.

## Demographics are noise

I scored every field with Information Value, a technique credit-risk teams use to rank predictors, and added a column of pure random numbers as a benchmark. Age, gender, income, education, marital status, dependents, tenure and card tier all scored between 0.002 and 0.014. The random column scored 0.007. A scorecard built only on demographics has an AUC of 0.53 - a coin flip.

Behaviour is another story: transactions (1.91), spend (1.89), revolving balance (1.07) and the change in activity between Q1 and Q4 (0.96) were in a different league.

## Churn is a hump, not a slope

I expected "less activity means more churn". It is not that simple. Customers making 35-50 transactions a year churn at **45.6%** and make up **52% of all churners**; below that, churn dips, and above 95 transactions *nobody* left. Spending $2-3K a year means 48% churn; spending $3.5-4.5K means 9 churners out of 2,449 customers.

And churners do not spend less per purchase - the median ticket is $56 vs $55. They just reach for the card less often.

## The warning light: a falling trend

Customers whose Q4 transaction count fell below half of Q1 churn at **50.9%** versus 10.6% for everyone else. Combine that with a thin relationship (one or two products) and light usage, and the "Exit-bound" group churns at **93%**.

## Three traps I nearly fell into

1. **Leakage.** The file ships with two extra columns: the output of a Naive Bayes model that had already seen the answer. Their AUC against the real outcome was 1.000. Any model using them would look perfect and be useless.
2. **Post-decision symptoms.** All 73 customers with a revolving balance of $1-$400 churned, and so did all 54 with six bank contacts. They look like the best predictors in the data, but they are what an account looks like *after* a customer has decided to leave. A scorecard that includes them looks better (AUC 0.944 vs 0.915) and helps less.
3. **Averages that lie.** Churned customers hold 11% of spend but 16% of headcount - so churn costs *less* than it seems. Yet the top-20% spenders churn at 13% and hold 41.5% of the churned spend, and activity signals cannot see them.

## From insight to action

I built an explainable Weight-of-Evidence scorecard (NumPy and Pandas only) and tested it on 3,039 customers it had never seen. The riskiest 10% were 78% churners - 4.8 times the base rate. Under illustrative economics ($10 per outreach, one year of revenue), a campaign on that list breaks even if it saves about 9% of the customers it reaches.

## What I would do next

- Add timestamps to confirm that falling activity *precedes* churn rather than accompanying it.
- Run a randomised pilot with a hold-out group to measure the real save rate.
- Give high-value leavers their own watch-list.

**Full notebook and code:** `<GitHub link>` - **Kaggle:** `<Kaggle link>`

*Tools: Python, Pandas, NumPy, Matplotlib, Seaborn.*
