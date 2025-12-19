# Column Audit – Credit Risk Customer Segmentation

## Dataset Overview
- Each row in the dataset represents a single loan issued to a borrower.
- The data includes borrower profile information, loan characteristics, credit history, and post-loan repayment outcomes.
- The objective of this project is to assess credit risk using only information that would have been available at the time of loan approval.

---

## Initial Data Inspection
Based on inspection of the first few rows of the dataset:
- Borrower income and employment information is partially self-reported and may contain missing or noisy values.
- Credit history variables such as delinquencies, debt-to-income ratio, and credit utilization appear to be strong indicators of repayment behavior.
- The dataset includes both individual and joint loan applications, leading to structured missing values in joint income and joint debt-to-income fields.
- Several columns capture repayment activity after loan issuance and must be excluded from model inputs to avoid data leakage.

---

## Column Classification Strategy
Columns are grouped into the following categories:
- **A – Borrower Profile:** Personal and employment-related attributes.
- **B – Loan Characteristics:** Details of the loan issued by the lender.
- **C – Credit History & Behavior:** Historical indicators of borrower repayment behavior.
- **D – Post-Loan / Outcome Variables:** Information recorded after loan issuance (leakage risk).
- **E – Time Variables:** Dates and time-based indicators.

---

## Initial Column Audit (High-Impact Fields)

| Column Name | Category | Use in Modeling | Rationale |
|------------|----------|----------------|-----------|
| annual_income | A | Yes | Proxy for borrower repayment capacity |
| verified_income | A | Yes | Indicates reliability of reported income |
| debt_to_income | C | Yes | Measures borrower’s existing debt burden |
| delinq_2y | C | Yes | Captures recent delinquency behavior |
| loan_amount | B | Yes | Represents lender’s exposure |
| term | B | Yes | Longer loan terms increase default risk |
| interest_rate | B | Yes | Risk-based pricing assigned at approval time |
| grade | B | Yes | Internal risk grading by the lender |
| loan_status | D | Target | Final loan outcome used as risk label |

---

## Data Leakage Considerations
The following columns capture information that becomes available only after loan issuance and must not be used as predictive features:

| Column Name | Reason for Exclusion |
|------------|----------------------|
| balance | Reflects remaining loan balance post-issuance |
| paid_total | Encodes repayment behavior |
| paid_principal | Directly reflects principal repayment |
| paid_interest | Directly reflects interest repayment |
| paid_late_fees | Indicates post-loan delinquency |

Including these variables would leak future information into the model and artificially inflate performance.

---

## Target Variable Definition
The target variable for this project is derived from the `loan_status` column:
- **Good loans (0):** Fully Paid
- **Bad loans (1):** Charged Off / Default
- Loans that are still current or in late/grace periods will be excluded from modeling to reduce label noise.

This definition aligns with common banking practices for credit risk modeling.

---