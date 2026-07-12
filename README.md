# Analyzing Olist Marketing Channels and Revenue by Category, State, and Month
Marketing teams want to understand which channels convert leads best, and create the most value to effectively spend their budget, and they want to understand how revenue breaks down by month, product category, and location to help direct their budget. Through a market channel analysis and revenue analysis, respectively, I try to understand the state of a Brazilian e-commerce company to help aid the marketing department in picking the right places to direct their budget.

## Key Findings
* Home and Living goods (e.g. home furniture and appliances), Leisure & Hobbies (e.g. books and toys), and Electronics drive around 55% of revenue, and 57% of orders. While Leisures and Hobbies has a below mean average order value (AOV), the other categories have AOV near the mean.
* Industrial & Construction, Watches and Auto Parts are low volume but high AOV segments of the market
* Fashion & Accessories have the weakest AOV, and a low volume which positions it as a clear underperformer, which could make it prime for more attention.
* Several markets, including Bahia and Pernambuco, have below average order counts and revenue which present a growth opportunity for the company.
* March through August is a mild peak for the company, and November may be a genuine overall peak for the company but the data is limited (September through December have a single data point) so this should be taken with some skepticism.
* Paid search performs better in conversion rate, days to close a deal, and revenue driven than social media, but the difference is not large. Note that the revenue analysis is directional, and doesn't represent the actual difference in revenue driven because of how flawed the method to get the data was.

## Methodology
I downloaded all of the csv files found in the marketing funnel and ecommerce datasets, and then use sqlite3, a Python module, to create a SQLite database to hold the files and let me query them. I also added the optional translations file, and created a column based off of it to help analyze data, since it's easier for me to work with English categories rather thna ones in Portuguese. Additionally, I made a Brazilian state population table based off of 2018 IBGE data to be used in the By State analysis.

I started by analyzing revenue, order count, and AOV by category, but because there were 71 categories, I decided to bin categories into 11 more general categories. I grouped item categories like "bed_bath_table" and "home_furniture" into a more general "Home & Living" category, which was beneficial and made actually figuring out some general trends possible, but I still have two generic categories: "Other" and "Uncategorized." This is definitely a location for improvement.

Next I started on analyzing the same values by state, which also required some binning since it would be difficult to compare revenue, order count, AOV, and revenue/order count per million across all of the states. Using the population table I imported, I was able to find the per million (I used a per million number so the numbers were large enough to not be rounded down to 0) for orders and revenue and I used those to categorize each state into one of four categories: core markets (above average per million order count and revenue values), growth opportunities (below average in both), and two categories that correspond to one metric being below average and the other metric being above average. 

Finally, I analyzed the data by time both by month and by year. The issue with the data is that 2016 has effectively no data in it, and 2018 is almost empty in September and October. 2016 had few orders placed in it, aside from October 2016 which had ~300 orders, but this is not enough to justify it as part of the analysis. 2018 has no data for November and December, and had fewer than 20 orders each for September and October. The combination of these two facts makes certain trends difficult to ascertain. Four months (September to December) have only one data point so seasonality is difficult to determine, and it is difficult to be certain about any trends found.

The final aspect of the analysis is the A/B testing of the two marketing channels: paid search and social. Interestingly, it is not clear what social is, but this is likely social media. I used statistical tests to determine the statistical significance and effect size of what I found: 
* For conversion rates, I used a two-proportion z-test to determine the significance and Cohen's h the effect size. Cohen's h was 0.24, which is a bit larger than the small difference threshold of 0.20.
* For days to close, I used a Mann-Whitney U score (because days to close is skewed positively) to determine statistical significance and a rank biserial score to determine effect size.

I also did a revenue analysis based on an indirect join using the seller_id, which is flawed because a seller may have participated in several marketing campaigns and have several orders which are unrelated to those marketing campaigns. This means the result should be taken with a grain of salt and looked at more directional rather than seeing a specific difference in performance. In this case, paid search did better than social but the actual difference isn't clear from this analysis.

Overall, the difference is small but statistically signficant between the two marketing channels.

## Findings
Home & Living (e.g. home furniture, appliances and housewares), Leisure & Hobbies (e.g. books and toys), and Electronics drive 55% of revenue and 57% of total orders. While Leisure & Hobbies has a below mean average order value (AOV), the other two have near mean AOVs. Importantly, Industrial & Construction, Watches and Auto are three high AOV, low volume segements that could benefit from targeted marketing methods which paid search or social media could provide, though the former is likely more applicable unless social media marketing includes targeting advertising.

There are 18 Brazilian states which have below average revenue and order count per million inhabitants (identified as "Growth Opportunities" in analysis) which means they could benefit from marketing campaigns.

From the given data, seasonality is difficult to confirm because four months (September through December) only have one data point (the other existing months have too few orders). There seems to be a mild peak from March to August, but it's also true that the revenue and order counts are stable throughout the year. However, 2018, despite missing two months and having two low-order months, is on pace to do better than 2017. 

For marketing channels, paid search does better than social media but not by a large margin so both can be used. Perhaps social media campaigns can be more targeted to try to boost their effectiveness.

### Recommendations
* Paid Search is better than social media, but the small difference suggests that social media campaigns need to be tweaked rather than abandoned.
* Niche but profitable segments (Industrial & Construction, Watches and Auto) can benefit from targeted paid search and social media marketing campaigns.
* The 18 Brazilian states marked as "Growth Opportunities" could benefit from marketing campaigns so they can reach or exceed the average order count and revenue per million inhabitants.

## Addendum

### Dataset
* [Olist Brazilian E-Commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
* [Olist Marketing Funnel](https://www.kaggle.com/datasets/olistbr/marketing-funnel-olist)

### Tools Used
* SQLite (via sqlite3 for Python)
* Python (pandas, scipy, statsmodel, matplotlib)
* Tableau [Dashboard Link](https://public.tableau.com/app/profile/kacper.sienkiewicz/viz/OlistMarketingChannelComparison/Overview)
