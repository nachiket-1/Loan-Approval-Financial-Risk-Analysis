# Loan Approval: Financial Risk Analysis

## About this project

I used this project to explore which applicant details are associated with loan approval and to build a baseline model that predicts the approval decision recorded in the dataset.

The dataset contains 20,000 applications. It is synthetic, so the results are useful for practising data analysis and model evaluation, but they should not be treated as evidence about how a real lender makes decisions.

## Questions I explored

- How does the approval rate vary with annual income, credit score and loan amount?
- Does the size of a loan relative to a person's annual income tell us more than loan amount alone?
- How do debt-to-income ratio and other financial variables relate to approval?
- How well can a simple classification model reproduce the recorded approval decision?
- Do the model's predictions vary across applicant groups?

## Dataset

The repository includes `Loan.csv`. The dataset has 20,000 rows and 36 columns. The notebook checks data types, missing values and duplicate rows before analysis.

The application dates run from January 2018 to October 2072, with one date per day. This pattern is artificial, so I did not use the date as a meaningful time trend.

The target used for classification is `LoanApproved`.

**Important limitation:** this target records whether an application was approved. It does not record whether a borrower later repaid the loan or defaulted. The model therefore predicts historical approval decisions; it is not a validated loan-default model.

## Approach

1. Load the data and check its basic quality.
2. Compare approval rates across groups of applicant and loan characteristics.
3. Create a loan-to-income ratio to put loan size in context.
4. Examine variables such as `RiskScore` and `InterestRate` for possible leakage or circular relationships.
5. Train a logistic regression model using applicant features.
6. Compare its performance with a Random Forest and use five-fold cross-validation.
7. Review precision, recall, F1-score and ROC-AUC alongside accuracy.
8. Explore how prediction outcomes differ across groups.

## Main findings

- Approval is unevenly distributed across the dataset: 4,780 of 20,000 applications were approved, giving an overall approval rate of 23.9%.
- Annual income, loan-to-income ratio and debt-to-income ratio show strong associations with the recorded approval decision.
- In the notebook's grouped analysis, the lowest annual-income quartile has an approval rate of about 1.0%, compared with 64.5% for the highest quartile.
- Savings balance and job tenure show little association with approval in the reported tests.
- `RiskScore` is strongly associated with the target. Because it may be calculated as part of the lender's decision process, including it could make the prediction circular. I excluded it from the primary model and treated the model that includes it as a comparison only.

These are associations in a synthetic dataset. They do not show that changing one feature would cause a particular applicant's approval outcome to change.

## Model results

The notebook reports the following results for logistic regression on a held-out test set:

| Metric | Result |
|---|---:|
| Accuracy | 88.3% |
| Precision | 79.9% |
| Recall | 68.4% |
| F1-score | 73.7% |
| ROC-AUC | 0.941 |

The dataset is imbalanced: only 23.9% of applications are approved. A model that predicts rejection for every application would have 76.1% accuracy, so accuracy alone is not enough to judge performance.

At the default 0.5 threshold, the model has higher precision than at lower thresholds, but it misses more of the applications that were approved in the dataset. The notebook compares thresholds of 0.3, 0.4 and 0.5 to show this trade-off.

Five-fold cross-validation reports mean accuracy of approximately 88.0% for logistic regression and 87.9% for Random Forest. The similar results support keeping logistic regression as a simpler, more interpretable baseline.

The metrics above are the notebook's reported outputs; they have not been independently reproduced in a clean environment as part of this README revision.

## Fairness and limitations

The notebook compares model outcomes across groups, including age, marital status, education, employment and home ownership. Some groups have different approval rates or different proportions of historically approved applications identified by the model.

These differences deserve attention, but they do not by themselves establish discrimination. The data is synthetic, and the model does not predict repayment outcomes. A real lending system would require careful validation, appropriate legal and fairness review, and monitoring before use.

Other limitations:

- The data is synthetic and may not reflect real applicants or real lending policy.
- Historical approval is not the same as creditworthiness or repayment performance.
- A random train/test split does not establish performance on future applications.
- The selected threshold depends on the relative consequences of false approvals and false rejections.
- Group comparisons should be interpreted alongside sample sizes and uncertainty, not only point estimates.

## Files

- `Loan.csv` — dataset used in the analysis
- `Loan_Approval_Financial_Risk_Analysis.ipynb` — analysis and modelling notebook
- `Loan_Approval_Financial_Risk_Analysis.pdf` — PDF version of the project
- `Lender_Brief.pdf` — short, plain-language summary
- `images/` — project figures

## Run the notebook

Use Python 3 with Jupyter Notebook or JupyterLab. From the repository folder, install the main libraries used in the notebook:

```bash
python -m pip install pandas numpy matplotlib seaborn scikit-learn scipy jupyter
```

Then start Jupyter and open `Loan_Approval_Financial_Risk_Analysis.ipynb`.

For reproducible results, restart the notebook kernel and run all cells from top to bottom. The command above installs current compatible releases; if results depend on specific package versions, record the versions from the environment used to verify the notebook.

## Next steps

- Run the notebook from a clean environment and record the package versions.
- Add group-level precision, recall and uncertainty intervals to the fairness analysis.
- Select a decision threshold using a clearly stated validation objective.
- If a suitable real repayment dataset becomes available, evaluate default prediction separately from approval prediction.

---

This project is for learning and analysis. It is not intended to make real lending decisions.
