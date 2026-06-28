# Capstone Project: Onboarding and Activation Campaign Experiment Analysis

**Author:** Sangita Chatterjee

This project evaluates a new onboarding and activation campaign for a subscription-based digital product company. Through A/B testing and statistical analysis, we assess whether to roll out the new onboarding experience to all users, reject it, or apply a selective segment-level launch.

---

## 1. Business Context

The company launched a new onboarding and activation campaign aiming to improve user conversion and early engagement. Users were randomized into two groups:
* **Control Group:** Experienced the existing onboarding workflow.
* **Treatment Group:** Experienced the new onboarding campaign.

Leadership needs to make a data-backed recommendation on whether to launch the treatment group experience to all users, continue testing, or deploy it selectively.

---

## 2. Dataset Description

The analysis is based on `data/campaign_experiment_data.xlsx`. The dataset contains 1,408 user records with the following columns:
* `user_id`: Unique identifier for each user.
* `signup_date`: Date of account registration.
* `experiment_group`: "Control" or "Treatment".
* `region`: Geographical region (East, North, South, West).
* `device_type`: Device used (Desktop, Mobile, Tablet).
* `traffic_source`: Referral channel (Email, Organic, Paid Search, Referral, Social).
* `plan_type`: Subscription tier selected at signup (Basic, Free, Premium).
* `visited_landing_page`: Binary indicator (1 if visited, 0 otherwise).
* `started_trial`: Binary indicator (1 if started trial, 0 otherwise).
* `completed_onboarding`: Binary indicator (1 if completed onboarding, 0 otherwise).
* `converted_to_paid`: Binary indicator (1 if converted, 0 otherwise).
* `revenue_30d`: Product revenue generated within 30 days of signup.
* `support_tickets_30d`: Count of support tickets raised in the first 30 days.
* `refund_requested`: Binary indicator (1 if refund requested, 0 otherwise).
* `days_to_convert`: Calendar days to conversion (only for converted users).
* `engagement_score`: Engagement score ranging from 0 to 100.

---

## 3. Data Cleaning and Preparation

Data cleaning and quality checks were performed inside `analysis/data_cleaning.py`. The original dataset had several anomalies that were handled as follows:

1. **Duplicate User IDs:** Identified 8 duplicate user records. All 8 rows were exact duplicates, so they were dropped, reducing the sample size from 1,408 to 1,400 unique users (690 Control, 710 Treatment).
2. **Missing Volumetric Values:** 
   * `device_type` (18 missing values) and `traffic_source` (24 missing values): Categorized as "Unknown" to preserve all user records and prevent sample selection bias.
   * `engagement_score` (14 missing values): Imputed using the median score of each user's respective experiment group to keep group variance stable (Control group median: 56.4, Treatment group median: 62.6).
   * `days_to_convert` (1,336 missing values): Maintained as null (NaN) since these correspond strictly to users who did not buy. This is structurally valid and expected.
3. **Invalid Binaries:** All binary columns were audited and verified to contain only 0 and 1.
4. **Outliers:** Checked the `revenue_30d` column and found a skewed distribution including high-value users in Control (e.g. $8,610.72 and $6,788.95) and in Treatment (e.g. $2,660.21). These represent high-spend transactions. Since they reflect true purchase events, they were kept in the analysis but highlighted for their impact on means.

---

## 4. North Star Metric Selection

The **Average Revenue Per User (ARPU)** was selected as the North Star Metric. 

ARPU is calculated as:
$$\text{ARPU} = \frac{\text{Total Revenue}}{\text{Total Users}}$$
While conversion rate is a direct measure of campaign performance, optimizing for conversion rate alone can lead to bad business outcomes if we convert many low-tier users who generate negligible revenue while increasing support costs. ARPU balances volume (conversion rate) and value (average spend per customer), tying the product campaign directly to the business bottom line.

---

## 5. KPI Tree Summary

Our KPI tree shows how the North Star metric breaks down into actionable drivers and risk indicators:

* **North Star Metric:** Average Revenue Per User (ARPU)
  * **Primary Driver 1: Paid Conversion Rate**
    * *Sub-driver 1.1:* Landing Page Visit Rate
    * *Sub-driver 1.2:* Trial Start Rate (given Landing Page Visit)
    * *Sub-driver 1.3:* Onboarding Completion Rate (given Trial Start)
  * **Primary Driver 2: Average Revenue Per Converted User (ARPPU)**
    * *Sub-driver 2.1:* Plan Selection Mix (ratio of Basic to Premium plans)
    * *Sub-driver 2.2:* Add-on/Upsell Purchases
  * **Primary Driver 3: Average 30-Day Engagement Score**
    * *Sub-driver 3.1:* Active Days in App
    * *Sub-driver 3.2:* Feature Adoption Rate
  * **Guardrail Metrics (Risk Controls):**
    * *Guardrail 1:* Support Ticket Rate (Indicator of friction/confusion; Target: < 15%)
    * *Guardrail 2:* Cancel/Refund Request Rate (Indicator of buyer regret/misleading onboarding; Target: < 0.20%)
    * *Guardrail 3:* Segment-level ARPU stability

The KPI Tree diagram is saved as `outputs/kpi_tree.png` and screenshotted in `screenshots/kpi_tree_preview.png`.

---

## 6. Experiment Analysis Approach and Hypothesis Summary

Using Excel and automated Python statistical scripts, we performed hypothesis testing at a significance level ($\alpha$) of 0.05. We conducted two-proportion Z-tests for rate metrics, and Welch's t-tests (unequal variances assumed) for mean metrics.

A summary of the results is detailed in the table below:

| Metric Tested | Statistical Test | Control Value | Treatment Value | P-value | Significant? (alpha=0.05) | Business Interpretation |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **Paid Conversion Rate** (Primary Metric) | 2-Prop Z-test | 3.19% | 7.04% | 0.0011 | **Yes (Increase)** | Campaign successfully doubled the purchase conversion rate. |
| **Average Revenue Per User (ARPU)** (North Star) | Welch t-test | $51.97 | $54.25 | 0.9107 | **No Change** | Net revenue gain is flat due to drop in transaction sizes. |
| **Average Engagement Score** (Primary Metric) | Welch t-test | 57.02 | 62.94 | < 0.0001 | **Yes (Increase)** | The onboarding campaign successfully boosted early application use. |
| **Support Ticket Rate** (Guardrail Metric) | 2-Prop Z-test | 14.78% | 24.79% | < 0.0001 | **Yes (Increase)** | Campaign introduced significant user friction and support demand. |

---

## 7. Guardrail Metrics Considered

We evaluated 3 main guardrail metrics to assess the risks of the new campaign:
1. **Support Ticket Rate:** A spike from 14.78% to 24.79% in the Treatment group. This statistical increase indicates that the new onboarding campaign caused user confusion, raising support costs.
2. **Refund Request Rate:** Rose from 0% in Control to 0.42% in Treatment. While small, this indicates potential post-conversion issues.
3. **Average Days to Convert:** Fell from 8.86 days in Control to 6.40 days in Treatment. This shows that the campaign successfully shortened the sales cycle for converted users.

---

## 8. Segment-Level Insights

Looking deeper at the segments (Plan Type, Traffic Source, Device Type, and Region) revealed important details:

* **High-Performing Segments:**
  * **Free & Premium Plans:** Free plan conversion tripled (3.06% to 9.29%) and ARPU doubled ($22.73 to $51.18). Premium plan conversion expanded (2.75% to 6.25%) and ARPU increased by 86.7% ($53.86 to $100.56).
  * **Referral traffic:** Conversion soared from 2.47% to 10.99%, and ARPU tripled ($33.31 to $103.00).
  * **Mobile & Tablet:** Showed strong ARPU increases.
* **Low-Performing Segments:**
  * **Basic Plan:** Conversion was flat (3.62% Control vs 3.88% Treatment), but ARPU fell 62.7% ($98.69 to $36.76).
  * **Social Media Traffic:** Conversion declined (7.75% Control vs 6.06% Treatment), and ARPU dropped 62.1% ($137.14 to $51.94).
  * **Desktop Users:** ARPU was cut in half, dropping from $84.94 to $41.15.

---

## 9. Final Recommendation

Based on these findings, we recommend: **Launch only for selected segments**.

We should route the new activation campaign onboarding exclusively to **Mobile and Tablet users**, users on **Free and Premium cohorts**, and traffic arriving via **Referral, Organic, and Paid Search**. We must keep the existing (Control) onboarding for **Desktop and Basic plan users**, and traffic coming from **Social media**, until we can redesign those paths to fix the dropdown in transaction value.

---

## 10. Assumptions and Limitations

* **Assumption of Homogeneity:** We assume that user behavior over the 30-day testing window represents long-term usage, though early onboarding effects might fade over time.
* **Telemetry and Tracker Bugs:** The increase in support tickets might be partially driven by installation bugs in the new campaign telemetry rather than UX friction alone.
* **Small Sample sizes for Rare Events:** Refund rate calculations are based on only 3 refund cases in the Treatment group. A larger cohort is needed to run significance testing on refunds.

---

## 11. Screenshots Included

The following screenshots are available in the `screenshots/` directory:
1. `screenshots/summary_metrics.png`: A summary table comparing Control vs Treatment across all key success and guardrail metrics, color-coded by performance change.
2. `screenshots/hypothesis_test_output.png`: A table detailing the statistical tests (Z-test and t-test), test statistics, p-values, and statistical significance records.
3. `screenshots/kpi_tree_preview.png`: The visual preview of the KPI Tree showing how the North Star metric splits into drivers and sub-drivers with guardrails at the bottom.
