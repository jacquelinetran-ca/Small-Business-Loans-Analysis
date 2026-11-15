# Small Business Loan Default Prediction

A logistic regression model that predicts whether a small business borrower will default, and a threshold choice that turns those predictions into approve or reject decisions. This is an example of a project completed in my Introduction to Data Science course.

## Tools
Python (pandas, statsmodels, scikit-learn) in Google Colab

## Data
`small_business_loans.csv`: 3,000 past loans.

| Column | Meaning |
|---|---|
| `late_payments` | Number of past late payments |
| `years_in_business` | How long the business has operated |
| `default` | 1 = defaulted, 0 = repaid |

722 of the 3,000 loans (24%) defaulted.

## What the Notebook Does
1. **Fits a logistic regression** predicting default from late payments and years in business.
2. **Scores applicants:** the model gives each applicant a probability of default.
3. **Shows why a 0.5 cutoff fails:** rejecting only applicants with a probability above 50% catches just 74 of 722 defaults.
4. **Builds the model properly:** splits the data 75% train / 25% test, fits on the training set only, and evaluates on unseen test loans.
5. **Applies a low cutoff (0.09)** chosen from a cost analysis, because approving a loan that defaults costs far more than turning away a good borrower.

## Key Findings
- **Both factors matter** (p < 0.001):
  - Each additional late payment raises the odds of default by about **69%**.
  - Each additional year in business lowers the odds of default by about **10%**.
- **Example applicants:** 2 late payments and 5 years in business gives a **39%** chance of default. 7 late payments and 10 years gives **84%**.
- **Results at the 0.09 cutoff (test set, 750 loans):**

| | Predicted repay | Predicted default |
|---|---|---|
| **Actually repaid** | 55 | 515 |
| **Actually defaulted** | 2 | 178 |

  - **Recall: 98.9%.** The model catches 178 of 180 defaults.
  - **Precision: 25.7%.** Only about 1 in 4 rejected applicants would actually have defaulted.
  - **Accuracy: 31.1%.** It's low on purpose, because the cutoff is set to avoid missed defaults rather than to be right most often.

## Trade-off
The 0.09 cutoff almost eliminates bad loans, but it rejects about 90% of borrowers who would have repaid. That's the right call only if a default costs roughly ten times more than the profit lost on a turned-away good loan. A lender wanting more loan volume would raise the cutoff and accept more risk.
