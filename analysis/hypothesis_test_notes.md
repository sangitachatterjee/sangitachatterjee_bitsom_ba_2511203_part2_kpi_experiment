# Hypothesis Testing Notes

This document frames and analyzes the hypotheses for the onboarding and activation campaign experiment. 

The primary business goal is to increase user conversion and engagement without hurting the overall unit economics (revenue per user) or degrading the customer experience (indicated by refunds and support ticket rates).

---

## 1. Primary Success Metric: Paid Conversion Rate

The main metric chosen to evaluate the onboarding campaign is the **Paid Conversion Rate** (the proportion of signed-up users who convert to paying customers). 

### Why this metric?
Paid conversion rate measures the bottom-funnel business impact. While landing page visits or trial starts show upstream interest, they do not guarantee revenue. Paid conversion rate directly validates whether the new onboarding experience successfully guides users to value and prompts them to pay.

### Hypothesis Framework
* **Null Hypothesis ($H_0$):** $P_{\text{treatment}} - P_{\text{control}} = 0$ (The new campaign onboarding experience does not affect the paid conversion rate compared to the existing onboarding experience).
* **Alternate Hypothesis ($H_a$):** $P_{\text{treatment}} - P_{\text{control}} \neq 0$ (The new campaign onboarding experience has a different paid conversion rate than the existing onboarding experience).
* **Test Type:** Two-tailed test. A two-tailed test is used to remain conservative, allowing us to detect if the treatment group performs significantly better or significantly worse (e.g. if the new experience adds friction that drives users away).
* **Significance Level ($\alpha$):** 0.05 (Standard 95% confidence level).

### Test Inputs & Calculations
* **Control Group:** $n_c = 690$ users, $x_c = 22$ converted users ($\hat{p}_c = 3.19\%$)
* **Treatment Group:** $n_t = 710$ users, $x_t = 50$ converted users ($\hat{p}_t = 7.04\%$)
* **Pooled Proportion ($p$):** $(22 + 50) / (690 + 710) = 72 / 1400 = 0.0514$ ($5.14\%$)
* **Standard Error (SE):** $\sqrt{p(1 - p)(1/n_c + 1/n_t)} = \sqrt{0.0514 \times 0.9486 \times (1/690 + 1/710)} = 0.0118$

### Test Statistic & P-Value
* **Z-statistic:** $(\hat{p}_t - \hat{p}_c) / \text{SE} = (0.070423 - 0.031884) / 0.011807 = 3.2640$
* **P-value:** 0.001099 (calculated using normal distribution CDF)

### Decision Rule
* If the p-value is less than 0.05, we reject the null hypothesis.

### Interpretation and Business Impact
Because the p-value (0.0011) is much lower than 0.05, we reject the null hypothesis. There is strong statistical evidence that the new campaign onboarding experience significantly improves the paid conversion rate. It more than doubled the conversion proportion from 3.19% to 7.04%. 

---

## 2. Value Metric: Average Revenue Per User (ARPU)

While conversion rate tells us the *quantity* of buyers, ARPU tells us the *financial value* generated across all signups. We need to verify if the conversion increase translates into meaningful revenue growth.

### Hypothesis Framework
* **Null Hypothesis ($H_0$):** $\mu_{\text{treatment}} - \mu_{\text{control}} = 0$ (The average revenue per user remains the same between treatment and control).
* **Alternate Hypothesis ($H_a$):** $\mu_{\text{treatment}} - \mu_{\text{control}} \neq 0$ (The average revenue per user differs between treatment and control).
* **Test Type:** Two-tailed Welch-t test (unequal variances assumed).
* **Significance Level ($\alpha$):** 0.05.

### Test Inputs
* **Control Group:** $n_c = 690$, mean = $51.97, standard deviation = $473.20
* **Treatment Group:** $n_t = 710$, mean = $54.25, standard deviation = $250.65

### Test Statistic & P-Value
* **T-statistic:** 0.1122
* **Degrees of Freedom (Welch approximation):** 1040.8
* **P-value:** 0.9107

### Decision Rule & Interpretation
Since the p-value (0.9107) is extremely high, we fail to reject the null hypothesis. The small increase in average revenue per user ($54.25 in treatment vs $51.97 in control, a difference of only $2.28) is not statistically significant and can easily be attributed to random noise. 

**Critical Business Warning:** Even though conversion rate doubled, ARPU was flat. This occurred because the average spend per converted user (ARPPU) fell sharply from $1630.10 in Control to $770.41 in Treatment. The control group benefited from fewer, but much higher-paying conversions (including outliers of $8610.72 and $6788.95), whereas the treatment group converted more users at a lower price point.

---

## 3. Engagement Metric: Average 30-Day Engagement Score

We must test if the new campaign successfully drives early engagement among all users who sign up.

### Hypothesis Framework
* **Null Hypothesis ($H_0$):** $\mu_{\text{treatment\_engagement}} - \mu_{\text{control\_engagement}} = 0$
* **Alternate Hypothesis ($H_a$):** $\mu_{\text{treatment\_engagement}} - \mu_{\text{control\_engagement}} \neq 0$
* **Test Type:** Two-tailed Welch-t test.
* **Significance Level ($\alpha$):** 0.05.

### Test Inputs
* **Control Group:** $n_c = 690$, mean = 57.02, standard deviation = 13.77
* **Treatment Group:** $n_t = 710$, mean = 62.94, standard deviation = 13.85

### Test Statistic & P-Value
* **T-statistic:** 8.0103
* **Degrees of Freedom:** 1397.3
* **P-value:** < 0.0001 (virtually zero)

### Decision Rule & Interpretation
We reject the null hypothesis. The increase in the average engagement score (from 57.02 to 62.94) is statistically significant. The treatment campaign successfully keeps users active and engaged during their first month.

---

## 4. Guardrail Metric: Support Ticket Rate

We must monitor whether the new onboarding campaign creates post-signup friction, leading to a surge in help-desk inquiries.

### Hypothesis Framework
* **Null Hypothesis ($H_0$):** $P_{\text{treatment\_tickets}} - P_{\text{control\_tickets}} = 0$
* **Alternate Hypothesis ($H_a$):** $P_{\text{treatment\_tickets}} - P_{\text{control\_tickets}} \neq 0$
* **Test Type:** Two-tailed Z-test for two proportions.
* **Significance Level ($\alpha$):** 0.05.

### Test Inputs
* **Control Group:** $n_c = 690$, users with >0 tickets = 102 (14.78%)
* **Treatment Group:** $n_t = 710$, users with >0 tickets = 176 (24.79%)

### Test Statistic & P-Value
* **Z-statistic:** 4.6921
* **P-value:** < 0.0001

### Decision Rule & Interpretation
We reject the null hypothesis. The increase in support ticket rate (from 14.78% in Control to 24.79% in Treatment) is highly significant. This suggests that the treatment campaign, while converting more users, introduced friction, confusion, or issues that forced users to reach out to support. This is a critical risk that must be addressed.
