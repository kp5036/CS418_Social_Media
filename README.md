# CS 418 Group Project: Fake News on Social Media

**Team:** Krish Patel, Mahi Patel, Dravya Dholakiya

## Research Questions

1. Does fake news get shared more than real news on social media (Twitter and Facebook)?
2. During the 2016 election, how much of the news people saw on Twitter came from fake news websites, and did that change closer to election day?
3. Do low-quality news websites get more attention on social media than high-quality ones?

## Primary Datasets

**1. Fake News on Twitter During the 2016 Election**
- Source: Grinberg et al., *Science* 2019 (also in the class datasets folder)
- Link: https://github.com/LazerLab/twitter-fake-news-replication
- Two files: daily counts of fake news on Twitter, and total views per news website

**2. FakeNewsNet (PolitiFact)**
- Source: Shu et al., 2020
- Link: https://github.com/KaiDMML/FakeNewsNet
- News articles that PolitiFact labeled fake or real, with the tweets that shared each one

## Secondary Datasets

**1. BuzzFeed Facebook Fact-Check (2016)**
- Link: https://github.com/BuzzFeedNews/2016-10-facebook-fact-check
- Facebook posts from political pages, rated true or false, with share counts. We will **compare** it with the Twitter data to see if fake news gets more shares on Facebook too.

**2. News Website Quality Ratings**
- Source: Lin et al., *PNAS Nexus* 2023
- Link: https://github.com/hauselin/domain-quality-ratings
- A quality score (0 to 1) for each news website. We will **join** it with the Twitter website data on the website name (like `cnn.com`).

## Getting the Data

All four datasets load from their URLs in `data_acquisition.ipynb` using `pd.read_csv`. The data files are not saved in this repo.

## What the Data Looks Like

| Dataset | Rows | Columns | One row is |
|---|---|---|---|
| Twitter daily counts | 128 | 10 | one day |
| Twitter website totals | 606 | 2 | one news website |
| FakeNewsNet (PolitiFact) | 1,056 | 5 | one news article |
| Facebook fact-check | 2,282 | 12 | one Facebook post |
| Website quality ratings | 11,520 | 2 | one news website |

**Twitter daily counts**
- Time: Aug 1 to Dec 6, 2016. Place: United States.
- Important columns: `date` (text), `N` (int, total news views that day), `pct_fake` (float, fraction of views from fake news sites)

**Twitter website totals**
- Columns: `website` (text), `N` (int, total views)

**FakeNewsNet**
- 432 fake and 624 real articles
- Important columns: `title` (text), `news_url` (text), `tweet_ids` (text, list of tweet IDs), `label` (fake/real, added by us)
- There is no date column.

**Facebook fact-check**
- Time: Sept 19 to 27, 2016. 9 Facebook pages.
- Important columns: `Rating` (text: mostly true / mostly false / mixture / no factual content), `share_count` (float), `Category` (mainstream, left, right)

**Website quality ratings**
- Columns: `domain` (text), `pc1` (float, quality score)
- 534 of the 606 Twitter websites have a rating, so the join should work.

## Notes / Problems

- The Twitter data only has daily totals, not individual tweets. The individual-level data is private.
- Some FakeNewsNet articles have no tweets and some have no URL.
- The Facebook data only covers one week.
