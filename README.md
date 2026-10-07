# Loan Approval: Financial Risk Analysis

An analysis of 20,000 loan applications to find out which applicant details are linked to a loan being approved, and which ones make little difference.

## Dataset

The data comes from the Financial Risk for Loan Approval dataset. `Loan.csv` has 20,000 applications and 36 columns, with no missing values and no duplicate rows.

I used these columns: `ApplicationDate`, `AnnualIncome`, `CreditScore`, `EmploymentStatus`, `LoanAmount`, `SavingsAccountBalance`, `TotalAssets`, `TotalLiabilities`, `JobTenure`, `HomeOwnershipStatus` and `LoanApproved`.

## Approach

1. Load and preview the data
2. Clean it (convert the date column, check for missing values and duplicates, select columns)
3. Explore each variable with summary statistics and charts
4. Summarize the findings

For every variable I compared the approval rate (approved applicants divided by total applicants) across groups. Raw counts only show where most applicants are, while the approval rate shows who is more likely to be approved. Numeric columns were split into equal-sized quartiles, and credit score was grouped into 50-point bands to fit the range in this data (343 to 712).

## Key Findings

The overall approval rate is 23.9%, so about 76% of applications are rejected.

| Variable | Result |
|---|---|
| Annual income | Strongest factor. Approval rises from about 1% in the lowest income quarter to about 65% in the highest |
| Loan amount | Larger loans are approved less often, from 39.3% for the smallest quarter to 9.1% for the largest |
| Credit score | Moderate effect. About 14% approved below 450, about 45% at 650 and above |
| Assets to liabilities ratio | Moderate effect. 18.3% when liabilities exceed assets, 30.3% when assets are over 5 times liabilities |
| Employment status | Small effect. Unemployed 18.2%, employed 24.0%, self-employed 27.8% |
| Home ownership | Small effect. Mortgage 25.2%, owner 24.9%, renter 22.8% |
| Savings balance | Almost no effect (22.8% to 24.3% across groups) |
| Job tenure | Almost no effect (22.4% to 25.1% across groups) |
| Application date | No trend |

The highest income quarter accounts for 67% of all approved loans (3,223 of 4,780).

## Notes

- The application dates are artificial, with exactly one application per day from 2018 onward. Because of this, time was left out of the later analysis, and the incomplete final year was excluded from the charts.
- Correlation shows association, not cause.
- Savings balance and the assets to liabilities ratio are heavily skewed, so the median describes a typical applicant better than the mean.
- Some groups are small (for example, only 251 applicants have 10+ years of job tenure), so their rates are less reliable.

## Next Steps

- Create a `LoanAmount / AnnualIncome` column, since a loan is easier or harder depending on income.
- Look at `TotalDebtToIncomeRatio` and `InterestRate`, which both show a clear link to approval (correlations of about -0.41 and -0.30).
- Treat `RiskScore` with care. It has the strongest link (about -0.77) but is probably calculated from the same information used to make the decision, so using it to explain approval would be circular.
- Build a prediction model such as logistic regression and test it with a train/test split.

## How to Run

Install the libraries:

```bash
pip install numpy pandas seaborn matplotlib jupyter
```

Place `Loan.csv` in the same folder as the notebook, then run:

```bash
jupyter notebook Loan_Approval_Financial_Risk_Analysis.ipynb
```

Run the cells from top to bottom. The helper functions cell has to be run once before the charts.

## Tools

Python, pandas, NumPy, Seaborn, Matplotlib, Jupyter Notebook
