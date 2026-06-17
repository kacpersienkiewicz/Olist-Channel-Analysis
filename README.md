# Comparing Paid Search and Social Channel Performance Using Marketing Funnel Data from the Olist Brazilian E-Commerce Dataset
Marketing teams want to understand which channels convert leads best, and create the most value to effectively spent their budget, which can be solved by analyzing the conversion rate of these various channels and checking for effects on revenue. A/B testing channels and then analyzing them using tests like two-proportions z-testing can see if the difference is significant and other tests, like Cohen's h, can be used to figure out how large the difference is. 

The Olist Brazilian E-Commerce Dataset offers an opportunity to do that statistical analysis as it contains a large amount of order records as well as a marketing funnel which contains marketing qualified leads (MQL), as well as when those leads were closed. The plan is to compare two channels: paid search and social (it is not clear from documentation if this means organic social media posts, social media ads, paid promotion by influencers etc.), and see how they compare.

## Dataset
* (Olist Brazilian E-Commerce Dataset)[https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce]
* (Olist Marketing Funnel)[https://www.kaggle.com/datasets/olistbr/marketing-funnel-olist]

I ended up downloaded all of the csv files from both links, however I only used  both of the marketing funnel files (olist_marketing_qualified_leads_dataset.csv and olist_closed_deals_dataset.csv), olist_order_items_dataset.csv, and olist_order_payments_dataset.csv. This was because the two order files were used to do a rudimentary revenue analysis, and the order_items file allowed for an easy join from the marketing funnel files to the order payments file.

There are limits to this dataset, as the connection between the marketing funnel files and the order records is weak, and leads to the revenue analysis not being as impactful as it could be. It still provides an interesting data point, but this could definitely be improved so this type of analysis could be better leveraged to judge different marketing chanels.

## Key Questions
1. Do paid search and social differ signficantly in lead conversion rate?
2. How does days to close depend on the marketing channel?
3. Is there a difference in revenue for both channels?

## Methodology
After creating the SQLite database, the first thing I did was try to figure out which two channels to compare and ended on social and paid search because they were similarly sized as around 1500 total leads. These two became the A/B test, and I decided to run a two-proportions z-test to figure out how signficant this is. With a p value much smaller than 0.05 (~3.119E-11), and a z value of 6, this means that the difference is significant but it's not clear how large the difference is. 

Cohen's h value was used to determine the effect size which resulted in a value a bit more than the standard threshold for a small difference (0.24 versus the 0.2 threshold). Coupled with a p value under the standard threshold (0.05), this means that the difference between the two channels is significant but the difference isn't massive. This was corroborated by the confidence intervals which had no overlaps with each others.

The next aspect was a time-series analysis where I looked at the monthly and cumulative conversion rate, comparing the two. The dataset is a bit limited here because seasonality generally requires two years of data, and there are some strange trends in the year of data. The first three months include a month where neither origin converts (makes sense for the first month), and a second month where social had no conversions. 2018 also marked the start of a massive ramp up for the company which makes it difficult to derive anything for sure.

Next I analyze the time it took for leads to close which led to a small difference of 4 days between social and paid search, which seemed statistically insignificant. Since the data was heavily positively skewed, I used a Mann-Whitney U score to confirm that the difference is small and significant; because of the high score (6000), and a significant p value it was confirmed to be small and insignificant.

The final step was to do a simple revenue analysis which relied on a join between the marketing funnel on the seller ID found in the order items table, which could be used to join the order payments table and get the revenue. This is imperfect and introduces some problems into the analysis: sellers could be subject to several promotions and marketing channels, and could have several orders, especially since they could be businesses rather than individuals. Regardless of this, finding the average revenue by seller gives a fairly close number, which provides more evidence that there is definitely a difference between these two channels, but it's small.

## Findings
Overall, paid search outperformed social in terms of conversion rate, time to close leads, and revenue metrics, but the effect sizes were typically small, suggesting that social channels are definitely performing worse but not massively worse. This means that optimizing the social funnel is a bigger priority than getting rid of it or dramtically changing its budget. It's important to note that the time-series analysis was limited by having less than a year of data and a marked increase in activity during the second half of the dataset which makes it hard to say anything definitive about seasonal trends.


### Limitations
* The revenue join via seller_id makes some assumptions about the data that are likely false. Each seller may have been part of multiple promotions or marketing channels, or they've made multiple orders which could skew the data.
* The sub-year time period makes time series analysis difficult especially any seasonality analysis.
* 2018 ramping up activity also makes it difficult to figure out if any of the trends are tied to growth at Olist versus the effectiveness of the marketing channels.
* There is also ambiguity surrounding what "social" channels mean in the dataset. It could mean influencer marketing, social media ads, organic social media posts or something else.
