# Logistic Regression-Based Credit Scoring for Loan Default Reduction: A Quantitative Framework

**Author:** Macksunday Ayoga  
**Affiliation:** Department of Financial Mathematics, University of Nairobi  
**Contact:** macksundayonyonka@gmail.com  
**Date:** May 2026  

---

## Abstract

Credit risk management remains one of the most critical challenges facing financial institutions globally. This paper presents a logistic regression-based credit scoring framework developed and validated on a portfolio of over 15,000 loan accounts. The model integrates borrower behavioral data, income metrics, and macroeconomic signals — including interest rate sensitivity analysis across a 13%–14.5% range — to produce a robust probability-of-default (PD) score. Implemented within a real lending environment, the framework achieved a **12% reduction in loan defaults** and enabled data-driven segmentation of non-performing loan (NPL) cohorts exceeding a 7% threshold. Results demonstrate that structured quantitative scoring significantly outperforms judgment-based underwriting and provides a replicable foundation for credit risk automation in emerging market financial systems.

**Keywords:** credit scoring, logistic regression, probability of default, NPL, credit risk, emerging markets, financial modeling

---

## 1. Introduction

The extension of credit is a fundamental driver of economic growth. However, the misallocation of credit — particularly in emerging markets — leads to systemic financial instability, elevated non-performing loan (NPL) ratios, and reduced access to capital for creditworthy borrowers. Effective credit risk modeling is therefore not merely a technical exercise; it is a societal and economic imperative.

Traditional credit assessment in many markets still relies heavily on subjective judgment, limited financial history, and informal income verification. This approach is prone to inconsistency, bias, and error. The application of statistical learning methods — specifically logistic regression — offers a transparent, auditable, and scalable alternative.

This paper presents a complete logistic regression credit scoring framework developed for a portfolio of over 15,000 active loan accounts. The study addresses three core objectives:

1. To construct a probability-of-default (PD) model using borrower-level financial and behavioral features.
2. To quantify the sensitivity of loan uptake and default risk to interest rate movements.
3. To segment borrowers by NPL risk and provide actionable credit limit recommendations.

The remainder of this paper is structured as follows: Section 2 reviews the relevant literature; Section 3 describes the data and methodology; Section 4 presents model results; Section 5 discusses policy implications; and Section 6 concludes.

---

## 2. Literature Review

### 2.1 Credit Scoring Models

Credit scoring has evolved significantly since the pioneering work of **Altman (1968)**, who developed the Z-score model for bankruptcy prediction using linear discriminant analysis. The subsequent decades saw a proliferation of statistical and machine learning approaches, including logistic regression (Wiginton, 1980), neural networks (Jensen, 1992), support vector machines (Vapnik, 1995), and gradient boosting methods (Chen & Guestrin, 2016).

Among these, **logistic regression** remains the gold standard in regulated financial environments due to its interpretability, regulatory acceptance (Basel II/III frameworks), and robust performance on structured tabular data (Siddiqi, 2006). The model produces well-calibrated probability estimates that map directly to regulatory capital calculations.

### 2.2 NPL Risk in Emerging Markets

The International Monetary Fund (IMF, 2023) reports that NPL ratios in Sub-Saharan Africa average 10.2%, nearly double the global average. Key drivers include income volatility, informal employment, macroeconomic shocks, and limited credit bureau infrastructure. Studies by **Kipyego & Wandera (2013)** and **Were & Wambua (2014)** highlight the particular challenge of credit risk assessment in Kenya, where mobile lending platforms have rapidly expanded credit access without commensurate risk controls.

### 2.3 Interest Rate Sensitivity

The relationship between interest rates and credit demand is well-documented in monetary economics. Stiglitz & Weiss (1981) demonstrated through their credit rationing model that raising interest rates does not unambiguously reduce credit risk — it may in fact worsen the borrower pool through adverse selection. This has direct implications for portfolio management during periods of central bank rate adjustment.

---

## 3. Data and Methodology

### 3.1 Data Description

The analysis draws on a loan portfolio of **15,247 accounts** observed over a 24-month period. Each account record includes:

| Feature Category | Variables |
|-----------------|-----------|
| Borrower Demographics | Age, employment type, region |
| Financial Profile | Monthly income, existing debt obligations, savings balance |
| Loan Characteristics | Loan amount, tenure, interest rate, collateral type |
| Behavioral Signals | Historical repayment pattern, missed payment count, days-past-due |
| Macroeconomic Context | Central Bank of Kenya (CBK) benchmark rate, inflation index |

The binary outcome variable **Y** is defined as:

$$Y = \begin{cases} 1 & \text{if the account defaults (NPL > 90 days past due)} \\ 0 & \text{otherwise} \end{cases}$$

The overall default rate in the sample was **9.4%**, consistent with regional benchmarks.

### 3.2 Logistic Regression Model

The probability of default for account *i* is modeled as:

$$P(Y_i = 1 \mid \mathbf{x}_i) = \frac{1}{1 + e^{-(\beta_0 + \boldsymbol{\beta}^T \mathbf{x}_i)}}$$

where $\mathbf{x}_i$ is the feature vector for borrower *i* and $\boldsymbol{\beta}$ are the model coefficients estimated by maximum likelihood:

$$\hat{\boldsymbol{\beta}} = \arg\max_{\boldsymbol{\beta}} \sum_{i=1}^{n} \left[ y_i \log P_i + (1 - y_i) \log(1 - P_i) \right]$$

### 3.3 Feature Selection and Engineering

Feature selection was performed using a combination of:

- **Weight of Evidence (WoE)** transformation to handle categorical variables and improve linearity.
- **Information Value (IV)** scoring, retaining features with IV > 0.10 (moderate predictive power).
- **Variance Inflation Factor (VIF)** analysis to detect and remove multicollinear predictors (threshold: VIF < 5).

Key engineered features included:

- **Debt-to-Income Ratio (DTI):** $DTI_i = \frac{\text{Total Monthly Obligations}_i}{\text{Monthly Income}_i}$
- **Repayment Consistency Index (RCI):** proportion of on-time payments in the prior 12 months
- **Income-Loan Ratio (ILR):** $ILR_i = \frac{\text{Loan Amount}_i}{\text{Annual Income}_i}$

### 3.4 Credit Scorecard Construction

Model coefficients were transformed into a point-based scorecard using the standard odds-to-score mapping:

$$\text{Score} = A - B \cdot \log(\text{Odds})$$

where:
- $\text{Odds} = \frac{P(\text{default})}{1 - P(\text{default})}$
- Constants $A$ and $B$ are calibrated so that a score of **600** corresponds to odds of 50:1 (good:bad) and scores double at **20 points per double of odds**.

Borrowers are classified into five risk bands:

| Score Range | Risk Band | Recommended Action |
|-------------|-----------|-------------------|
| 720–850 | Very Low | Approve; maximum credit limit |
| 640–719 | Low | Approve; standard limit |
| 580–639 | Medium | Approve with conditions |
| 520–579 | High | Reduce limit; enhanced monitoring |
| < 520 | Very High | Decline or require collateral |

### 3.5 Interest Rate Sensitivity Analysis

To model the impact of CBK rate changes on loan uptake and default probability, a sensitivity analysis was conducted across the observed rate corridor of **13% to 14.5%** (25 basis point increments). For each rate level *r*, the expected default probability was re-estimated as:

$$\hat{P}(\text{default} \mid r) = \frac{1}{1 + e^{-(\hat{\beta}_0 + \hat{\beta}_r \cdot r + \hat{\boldsymbol{\beta}}_{-r}^T \mathbf{x}_{-r})}}$$

Loan uptake elasticity was computed as:

$$\epsilon_r = \frac{\Delta Q / Q}{\Delta r / r}$$

where $Q$ is the number of new loan applications per period.

---

## 4. Results

### 4.1 Model Performance

The final logistic regression model was trained on 70% of the data (10,673 accounts) and validated on the remaining 30% (4,574 accounts). Performance metrics are reported below:

| Metric | Training Set | Validation Set |
|--------|-------------|----------------|
| AUC-ROC | 0.821 | 0.808 |
| Gini Coefficient | 0.642 | 0.616 |
| KS Statistic | 0.412 | 0.389 |
| Accuracy (threshold = 0.5) | 88.3% | 87.1% |
| Precision | 0.74 | 0.71 |
| Recall | 0.69 | 0.67 |

An AUC of **0.808** on the holdout set indicates strong discriminatory power, well above the regulatory minimum of 0.70 recommended under Basel internal ratings-based (IRB) approaches.

### 4.2 Top Predictive Features

By Information Value, the five most predictive variables were:

| Rank | Feature | Information Value | Direction |
|------|---------|-------------------|-----------|
| 1 | Repayment Consistency Index | 0.48 | Negative (higher RCI → lower PD) |
| 2 | Debt-to-Income Ratio | 0.39 | Positive |
| 3 | Days Past Due (prior 6 months) | 0.35 | Positive |
| 4 | Income-Loan Ratio | 0.29 | Positive |
| 5 | Employment Type (formal vs informal) | 0.21 | Positive (informal → higher PD) |

### 4.3 Default Reduction Outcomes

Deployment of the scorecard as the primary underwriting decision tool resulted in:

- **12% reduction** in the 12-month default rate (from 9.4% to 8.3%) relative to the prior judgment-based system.
- Improved **risk-adjusted return on the portfolio** by eliminating high-risk originations in the Very High band.
- Identification of **1,847 accounts** in the NPL > 7% cohort for targeted early intervention (restructuring, enhanced collection outreach).

### 4.4 Interest Rate Sensitivity Findings

Across the 13%–14.5% rate corridor, the analysis revealed:

- A **loan uptake elasticity of -1.34**, indicating that a 1% increase in interest rates reduces new applications by approximately 1.34% — consistent with elastic demand in this market segment.
- Default probability increased by an average of **0.6 percentage points** per 50 basis point rate increase, driven primarily by rising monthly obligation burdens on variable-rate borrowers.
- Borrowers in the Medium risk band (580–639) showed the highest rate sensitivity, with default probability increasing by **1.2 percentage points** per 50 bps — suggesting this segment requires proactive limit management during tightening cycles.

### 4.5 Credit Limit Recommendations

Credit limits were calibrated using a behavioral scoring overlay on the PD model:

$$\text{Credit Limit}_i = \text{Base Limit} \times f(\text{Score}_i) \times g(\text{Monthly Income}_i)$$

where $f(\cdot)$ is a monotonically increasing score-to-multiplier function and $g(\cdot)$ caps exposure at 3× monthly income for all risk bands.

---

## 5. Discussion and Policy Implications

### 5.1 Scorecard as Risk Infrastructure

The results confirm that a well-calibrated logistic regression scorecard can substantially improve credit portfolio quality in an emerging market context. Crucially, the model achieves this while maintaining interpretability — each score point corresponds to an explicit combination of borrower characteristics, enabling credit officers to explain decisions to applicants and regulators alike.

### 5.2 NPL Segmentation for Early Intervention

The identification of the NPL > 7% cohort (1,847 accounts) demonstrates the value of risk-tiered monitoring. Portfolio-level NPL management shifts from reactive (responding to defaults after the fact) to proactive (restructuring vulnerable loans before default crystallizes). This approach aligns with CBK's Prudential Guidelines on asset classification and provisioning.

### 5.3 Interest Rate Risk and Vulnerable Segments

The finding that Medium-risk borrowers are most sensitive to rate increases has direct implications for credit portfolio stress testing. Institutions should model CBK rate shock scenarios (+50 bps, +100 bps) as standard components of their credit risk appetite framework.

### 5.4 Limitations

This study has several limitations worth acknowledging:

- **Data availability:** Credit bureau coverage in Kenya remains incomplete, limiting the richness of external credit history data.
- **Model drift:** Logistic regression coefficients require periodic recalibration as economic conditions change.
- **Sample bias:** The portfolio reflects the client base of a single institution and may not generalize across lenders or market segments.

---

## 6. Conclusion

This paper demonstrates that logistic regression-based credit scoring, when rigorously implemented with domain-appropriate feature engineering, delivers meaningful and measurable improvements in loan portfolio quality. The 12% default reduction achieved in this study has direct implications for capital efficiency, provisioning costs, and responsible lending practices.

Future research directions include: (1) benchmarking logistic regression against gradient boosting and neural network alternatives on the same portfolio; (2) integrating mobile financial data (M-Pesa transaction history) as additional behavioral signals; and (3) developing a dynamic credit limit adjustment mechanism triggered by real-time behavioral score updates.

The framework presented here is replicable, auditable, and suitable for adaptation across credit markets in Sub-Saharan Africa and beyond.

---

## References

1. Altman, E. I. (1968). Financial ratios, discriminant analysis and the prediction of corporate bankruptcy. *Journal of Finance*, 23(4), 589–609.
2. Basel Committee on Banking Supervision. (2006). *International Convergence of Capital Measurement and Capital Standards* (Basel II). Bank for International Settlements.
3. Central Bank of Kenya. (2024). *Prudential Guidelines for Institutions Licensed Under the Banking Act*. CBK.
4. Chen, T., & Guestrin, C. (2016). XGBoost: A scalable tree boosting system. *Proceedings of KDD 2016*, 785–794.
5. IMF. (2023). *Regional Economic Outlook: Sub-Saharan Africa*. International Monetary Fund.
6. Kipyego, D. K., & Wandera, M. (2013). Effects of credit information sharing on nonperforming loans: The Kenya experience. *European Scientific Journal*, 9(13).
7. Siddiqi, N. (2006). *Credit Risk Scorecards: Developing and Implementing Intelligent Credit Scoring*. Wiley.
8. Stiglitz, J., & Weiss, A. (1981). Credit rationing in markets with imperfect information. *American Economic Review*, 71(3), 393–410.
9. Vapnik, V. (1995). *The Nature of Statistical Learning Theory*. Springer.
10. Were, M., & Wambua, J. (2014). What factors drive interest rate spread in Kenya's banking sector? *Review of Development Finance*, 4(2), 73–82.
11. Wiginton, J. C. (1980). A note on the comparison of logit and discriminant models of consumer credit behavior. *Journal of Financial and Quantitative Analysis*, 15(3), 757–770.

---

*© 2026 Macksunday Ayoga. This paper is released under the MIT License and is available for academic and professional use with attribution.*
