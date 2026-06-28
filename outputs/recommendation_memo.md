# Business Recommendation Memo

**To:** Leadership Team  
**From:** Business Analyst  
**Date:** June 28, 2026  
**Subject:** Onboarding and Activation Campaign Experiment Results & Recommendation  

---

## 1. Executive Summary

This memo evaluates the performance of the new onboarding and activation campaign tested on a randomized cohort of 1,400 unique users split between a Control group (existing onboarding) and a Treatment group (new campaign onboarding). 

Our analysis shows that while the Treatment campaign successfully drove a spike in paid conversion rate (from 3.19% to 7.04%) and early engagement (up 10.37%), it did **not** result in a statistically significant increase in Average Revenue Per User (ARPU). ARPU only rose from $51.97 to $54.25 (a non-significant 4.39% change). Furthermore, the campaign triggered a statistically significant 67.7% surge in customer support tickets (rising from 14.78% to 24.79% of users) and introduced refund risk. 

Based on these findings, we recommend a **targeted roll-out / launch only for selected segments** (Free and Premium cohorts, Mobile/Tablet users, and Referral traffic) while **temporarily holding the launch** for Desktop, Basic plan, and Social media cohorts to resolve onboarding friction and revenue erosion in those areas.

---

## 2. North Star Metric Selection

The selected North Star metric for this experiment is **Average Revenue Per User (ARPU)**. 

### Why this metric?
ARPU directly aligns product engagement with business growth. It is calculated as:
$$\text{ARPU} = \text{Paid Conversion Rate} \times \text{Average Revenue Per Converted User (ARPPU)}$$
By combining the volume of conversion (quantity) and the transaction size (quality), ARPU ensures we do not optimize for conversion volume at the expense of revenue depth. If we had optimized blindly for conversion rate, we would have declared the experiment a massive success, ignoring the 52.74% drop in average spend per paying customer.

---

## 3. KPI Tree Explanation

To understand the drivers of ARPU, we break the North Star metric down into a structured KPI tree:

* **North Star Metric:** Average Revenue Per User (ARPU)
  * **Primary Driver 1: Paid Conversion Rate (Conversion Quantity)**
    * *Sub-driver 1.1:* Landing Page Visit Rate
    * *Sub-driver 1.2:* Trial Start Rate (given Landing Page Visit)
    * *Sub-driver 1.3:* Onboarding Completion Rate (given Trial Start)
  * **Primary Driver 2: Average Revenue Per Converted User (ARPPU - Conversion Quality)**
    * *Sub-driver 2.1:* Plan Selection Mix (ratio of Basic to Premium plans)
    * *Sub-driver 2.2:* Add-on/Upsell Purchases
  * **Primary Driver 3: Average 30-Day Engagement Score**
    * *Sub-driver 3.1:* Active Days in App
    * *Sub-driver 3.2:* Feature Adoption Rate
  * **Guardrail Metrics (Risk Controls):**
    * *Guardrail 1:* Support Ticket Rate (Target: < 15%)
    * *Guardrail 2:* Refund Request Rate (Target: < 0.20%)
    * *Guardrail 3:* Segment-level ARPU stability

---

## 4. Experiment Result Summary

The table below summarizes the key metrics for the 1,400 unique, cleaned users (690 Control, 710 Treatment):

| Metric Name | Control | Treatment | Absolute Diff | Relative Diff | Performance Status |
| :--- | :---: | :---: | :---: | :---: | :--- |
| User Count | 690 | 710 | +20 | +2.90% | Sample Balanced |
| Landing Page Visit Rate | 63.62% | 72.39% | +8.77% | +13.79% | Improved |
| Trial Start Rate | 25.07% | 29.01% | +3.94% | +15.72% | Improved |
| Onboarding Completion Rate | 15.65% | 21.13% | +5.47% | +34.98% | Improved |
| Paid Conversion Rate | 3.19% | 7.04% | +3.85% | +120.87% | Significant Increase |
| Average Revenue Per User (ARPU) | $51.97 | $54.25 | +$2.28 | +4.39% | Flat (Not Significant) |
| Average Revenue Per Converted User (ARPPU) | $1630.10 | $770.41 | -$859.69 | -52.74% | Severe Decline |
| Refund Rate | 0.00% | 0.42% | +0.42% | +inf | Increased Risk |
| Support Ticket Rate | 14.78% | 24.79% | +10.01% | +67.69% | Critical Health Spike |
| Average Engagement Score | 57.02 | 62.94 | +5.91 | +10.37% | Significant Increase |
| Average Days to Convert | 8.86 days | 6.40 days | -2.46 days | -27.79% | Faster conversion |

---

## 5. Hypothesis Test Interpretation

Statistical testing (at $\alpha = 0.05$) revealed the following conclusions:

1. **Paid Conversion Rate:** The Z-test yielded a Z-score of 3.2640 and a p-value of 0.0011. Since $p < 0.05$, we reject the null hypothesis. The increase in conversion rate is statistically significant and not due to chance.
2. **ARPU:** The t-test yielded a t-statistic of 0.1122 and a p-value of 0.9107. Since $p > 0.05$, we fail to reject the null hypothesis. The $2.28 ARPU increase is statistically insignificant. 
3. **Engagement Score:** The t-test yielded a t-statistic of 8.0103 and a p-value of < 0.0001. We reject the null hypothesis; the campaign drove a real, statistically significant lift in early engagement.
4. **Support Ticket Rate:** The Z-test yielded a Z-score of 4.6921 and a p-value of < 0.0001. We reject the null hypothesis. The spike in help-desk tickets is highly significant.

---

## 6. Guardrail Analysis

* **Support Ticket Rate (FAIL):** The Treatment group support rate jumped to 24.79% (almost 1 in 4 users) compared to 14.78% in Control. This represents a heavy strain on customer support teams and indicates that the new onboarding workflow creates substantial confusion or technical glitches.
* **Refund Rate (WARNING):** Refunds rose from 0% in Control to 0.42% (3 users) in Treatment. While the absolute number is small, it suggests a post-purchase mismatch in expectations.
* **Revenue Quality (FAIL):** The average transaction value (ARPPU) dropped by 52.74%. The campaign induced higher volume but significantly lower spend per converting customer.

---

## 7. Segment-level Insights

A deep-dive analysis by segment reveals that the campaign affects user cohorts differently:

* **Plan Type:** 
  * *Free & Premium:* Highly successful. Free users saw conversion rates grow from 3.06% to 9.29% (+204%) and ARPU double from $22.73 to $51.18 (+125%). Premium users saw conversion rise from 2.75% to 6.25% (+127%) and ARPU rise from $53.86 to $100.56 (+86.7%).
  * *Basic:* Unsuccessful. Basic plan conversion was flat (3.62% Control vs 3.88% Treatment), but ARPU collapsed by 62.7% (from $98.69 down to $36.76).
* **Traffic Source:**
  * *Referral:* Highly successful. Conversion rate jumped from 2.47% to 10.99% and ARPU tripled ($33.31 to $103.00).
  * *Social:* Unsuccessful. Social media traffic conversion fell from 7.75% in Control to 6.06% in Treatment, and ARPU fell 62.1% (from $137.14 down to $51.94).
* **Device Type:**
  * *Mobile & Tablet:* Highly successful. Mobile/Tablet ARPU surged.
  * *Desktop:* Unsuccessful. Desktop conversion improved slightly, but ARPU was cut in half, dropping from $84.94 to $41.15.

---

## 8. Final Recommendation: Launch Only for Selected Segment

Based on the data, we recommend against a general, full-scale launch. Instead, we recommend to **Launch Only for Selected Segment**. 

### Rationale:
A general launch would burden our support teams with a 67.7% increase in support tickets without delivering any significant revenue gains (ARPU remained flat). However, launching the campaign selectively allows us to capture massive revenue lifts while avoiding segment-specific declines:

1. **Approved Segments:** Roll out the campaign onboarding to users signing up on **Mobile and Tablet devices**, users on **Free and Premium plans**, and users coming from **Referral, Organic, and Paid Search** traffic.
2. **Held Segments:** Retain the existing (Control) onboarding experience for users on the **Basic plan**, users signing up via **Desktop**, and traffic arriving from **Social media**.

---

## 9. Risks and Limitations

* **Support Team Overload:** If we launch to approved segments, the support ticket rate will still rise. We must scale support capacity or simplify the new onboarding setup.
* **Basic Plan Revenue Loss:** The treatment campaign somehow prevents Basic plan users from selecting high-value tiers or add-ons (reducing ARPPU).
* **Sample Size for Rare Events:** The refund rate calculations are based on very few events (3 refunds in Treatment, 0 in Control). While statistically indicative of risk, the exact size of this risk requires a larger sample size.

---

## 10. Next Steps

1. **Segment-Targeted Roll-out:** Configure the onboarding flow routing in the product to deliver the Treatment experience only to the approved segments.
2. **Product Redesign for Held Segments:** Run a qualitative usability review of the Desktop and Social media signup experiences to understand why they performed poorly, and redesign the step-by-step onboarding flow to reduce confusion.
3. **Friction Analysis:** Review support ticket logs from the Treatment group to identify the precise steps or features causing the support ticket spike.
4. **Re-Test:** Run a follow-up experiment in 3 months for the held segments once design tweaks are in place.
