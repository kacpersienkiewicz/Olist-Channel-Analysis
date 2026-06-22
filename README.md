# Marketing Channel Performance Analysis: Paid Search vs. Social Media Using the Olist Brazilian E-Commerce Dataset
Marketing teams want to understand which channels convert leads best, and create the most value to effectively spend their budget, which can be solved by analyzing the conversion rate of these various channels and checking for effects on revenue. This project applies A/B testing to compare two acquisition channels, paid search and social, using a two-proportion z-test for statistical significance and Cohen's h for practical effect size.

The Olist Brazilian E-Commerce Dataset offers an opportunity to do that statistical analysis as it contains a large amount of order records as well as a marketing funnel which contains marketing qualified leads (MQL), as well as when those leads were closed. The plan is to compare two channels: paid search and social (it is not clear from documentation if this means organic social media posts, social media ads, paid promotion by influencers etc.), and see how they compare.

## Executive Summary
* Paid search performs better than social media, but the difference is small.
* It take around 4 fewer days to close a paid search deal than social media.
* Paid search drives more revenue than social media, but a more focused analysis is required to know exactly how much more revenue it drives.

## Dataset
* [Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
* [Olist Marketing Funnel](https://www.kaggle.com/datasets/olistbr/marketing-funnel-olist)

I downloaded all CSV files from both links but only used the two marketing funnel files (olist_marketing_qualified_leads_dataset.csv and olist_closed_deals_dataset.csv), olist_order_items_dataset.csv, and olist_order_payments_dataset.csv. This was because the two order files were used to do a rudimentary revenue analysis, and the order_items file allowed for an easy join from the marketing funnel files to the order payments file.

The marketing funnel data tracks leads from initial contact (Marketing Qualified Leads) through to closed deals which allows conversion rates to be calculated at the lead level.

There are limits to this dataset, as the connection between the marketing funnel files and the order records is weak, and leads to the revenue analysis not being as impactful as it could be. It still provides an interesting data point, but this could definitely be improved so this type of analysis could be better leveraged to judge different marketing channels.

## Key Questions
1. Do paid search and social differ significantly in lead conversion rate?
2. Do paid search and social leads differ in time to close?
3. Is there a difference in revenue for both channels?

## Methodology
After creating the SQLite database, the first thing I did was try to figure out which two channels to compare and ended on social and paid search because they were similarly sized as around 1500 total leads. These two became the A/B test, and I decided to run a two-proportions z-test to figure out how significant this is. With a p value much smaller than 0.05 (~3.119E-11), and a z value of 6, this means that the difference is significant but it's not clear how large the difference is. 

Cohen's h value was used to determine the effect size which resulted in a value a bit more than the standard threshold for a small difference (0.24 versus the 0.2 threshold). Coupled with a p value under the standard threshold (0.05), this means that the difference between the two channels is significant but the difference isn't massive. This was corroborated by the confidence intervals which had no overlaps with each others.

The next aspect was a time-series analysis where I looked at the monthly and cumulative conversion rate, comparing the two. The dataset is a bit limited here because seasonality generally requires two years of data, and there are some strange trends in the year of data. The first three months include a month where neither origin converts (makes sense for the first month), and a second month where social had no conversions. 2018 also marked the start of a massive ramp up for the company which makes it difficult to derive anything for sure.

A 4-day difference in median days to close between channels appeared modest, so a Mann-Whitney U test was used to determine whether this difference was statistically meaningful because the data had a heavy positive skew. Since the Mann-Whitney U score was high (6000) and the rank-biserial score was low (0.1718), the difference is significant but small.

Due to the indirect joins through seller_id, the revenue analysis is directional and not definitive. Sellers may have participated in multiple marketing channels and or have multiple orders which distorts the revenue results. Regardless of this, finding the average revenue by seller gives the following result: ~21k revenue per seller for paid search, and ~11.5k revenue per seller for social which is a large difference.

## Findings
Overall, paid search outperformed social in terms of conversion rate, time to close leads, and revenue metrics, but the effect sizes were typically small, suggesting that social channels are definitely performing worse but not massively worse. Revenue per seller showed a larger gap than the days to close or conversion rate analysis but given its limitations it should be treated less as a conclusive result, and more directional.

This means that optimizing the social funnel is a bigger priority than getting rid of it or dramatically changing its budget. It's important to note that the time-series analysis was limited by having less than a year of data and a marked increase in activity during the second half of the dataset which makes it hard to say anything definitive about seasonal trends.

### Limitations
* The revenue join via seller_id makes some assumptions about the data that are likely false. Each seller may have been part of multiple promotions or marketing channels, or they've made multiple orders which could skew the data.
* The sub-year time period makes time series analysis difficult especially any seasonality analysis.
* 2018 ramping up activity also makes it difficult to figure out if any of the trends are tied to growth at Olist versus the effectiveness of the marketing channels.
* Leads self-select for a marketing channel which may skew channel performance irrespective of actual channel quality.
* There is also ambiguity surrounding what "social" channels mean in the dataset. It could mean influencer marketing, social media ads, organic social media posts or something else.

## Tools and Technologies
* SQLite (created via sqlite3 for Python)
* Python (pandas, scipy, statsmodel, matplotlib)
* Tableau [Dashboard Link](https://public.tableau.com/app/profile/kacper.sienkiewicz/viz/OlistMarketingChannelComparison/Overview)
