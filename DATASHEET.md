# Datasheet: Fake News on Social Media

CS 418, Fall 2026. Team: Krish Patel, Mahi Patel, Dravya Dholakiya

This file explains where our data comes from, what is in it, and how to rebuild it. If the publisher doesn't say something, we wrote "not documented by the publisher".

Krish: parts 1, 3, 4, 5. Mahi: part 2. Dravya: part 6.

---

## Quick summary

| | Twitter 2016 (Grinberg et al.) | FakeNewsNet (Shu et al.) |
|---|---|---|
| What it is | fake news exposure on Twitter for 16,442 U.S. voters | news articles PolitiFact labeled fake or real |
| One row | one day (128 rows) / one website (606 rows) | one article (1,056 rows) |
| Time | Aug 1 – Dec 6, 2016 | not documented by the publisher |
| Place | U.S. | U.S. political news |
| License | "Other (Open)" on Zenodo | no license file |
| Main problem | only totals, no individual data | only tweet IDs, no dates |

---

## 1. Datasheets

### 1A. Twitter 2016 (Grinberg et al., *Science* 2019)

**Motivation:** The authors wanted to see "how ordinary citizens experience misinformation on social media platforms" during the 2016 election. It was made by Nir Grinberg, Kenneth Joseph, Lisa Friedland, Briony Swire-Thompson and David Lazer at Northeastern University. The paper says funding came from "ESRC ES N012283/1 and ARO W911NF-12-1-0556".

**Composition:** We use two files. In `daily_counts_exposures.csv`, one row is one day (128 days). It has how many political news links people could have seen (`N`) and how many came from fake news sites. In `total_exposure_counts.csv`, one row is one website (606 sites). This is a sample, not everyone: it's 16,442 Twitter accounts that the authors matched to voter records. Data about individual people is not public. How they picked the 606 websites is not documented by the publisher.

**Collection:** The authors matched voter registration records to Twitter accounts and collected tweets from the accounts those people followed, from Aug 1 to Dec 6, 2016. A website counts as fake if it is on their lists: black (171 sites that make up stories), red (64 sites) and orange (65 sites). The study was approved by Northeastern's IRB. Whether people knew they were in the study is not documented by the publisher.

**Preprocessing:** The publisher removed bots, kept only political tweets with links, and added everything up by day and by website. The raw data is only available by request from the Lazer Lab, and you have to agree not to identify anyone.

**Uses:** It was used for Figures 1 and 7 in the paper. The main finding was that "only 1% of individuals accounted for 80% of fake news source exposures." It should not be used to judge whether one story is true, because labels are per website, not per story. It also shouldn't be used for claims about all Americans or other platforms.

**Distribution:** It's on GitHub (https://github.com/LazerLab/twitter-fake-news-replication) and on Zenodo with a DOI (https://doi.org/10.5281/zenodo.2483311). The license is "Other (Open)".

**Maintenance:** The Lazer Lab maintains it. The last version is v0.6, from April 2019. Plans for updates are not documented by the publisher.

### 1B. FakeNewsNet (Shu et al., 2020)

**Motivation:** The authors say "the lack of comprehensive and community-driven fake news datasets has become one of major roadblocks" for research. It was made by Kai Shu, Deepak Mahudeswaran, Suhang Wang, Dongwon Lee and Huan Liu at Arizona State University. Funding: not documented by the publisher.

**Composition:** One row is one news article that PolitiFact fact-checked. There are 432 fake and 624 real. The columns are `id`, `news_url`, `title` and `tweet_ids`. It's a sample: only articles PolitiFact checked and the team could collect. There is no date, no article text, and no tweet text. The README says this is because of "Twitter privacy policies and news publisher copy rights." 255 articles have no tweets and 61 have no URL.

**Collection:** The labels come from PolitiFact fact-checkers. The tweets were found by searching Twitter for each article's headline. When it was collected is not documented by the publisher. Twitter users were not asked.

**Preprocessing:** They only share tweet IDs. The repo has a script to download the full tweets, but it needs Twitter (now X) API keys, which are no longer free.

**Uses:** It was made for fake news detection research. It should not be used to label people as fake news spreaders, or to count how many people saw an article.

**Distribution:** It's on GitHub as CSV files (https://github.com/KaiDMML/FakeNewsNet). There is no license file; the README says "(C) 2019 Arizona Board of Regents on Behalf of ASU".

**Maintenance:** The ASU lab maintains it. The README says to "use the current up-to-date version", and the old version is in a separate branch.

---

## 2. Scope (Mahi)

> TODO Mahi

| Research question | Target population | Access frame | Sample | Who/what is missing | What we can't claim |
|---|---|---|---|---|---|
| 1. Does the typical fake article get shared more than the typical real one? | TODO | TODO | TODO | TODO | TODO |
| 2. How did the fake news share on Twitter change over time? | TODO | TODO | TODO | TODO | TODO |
| 3. Is low-quality news concentrated in a few websites, and do they get more views? | TODO | TODO | TODO | TODO | TODO |

---

## 3. Codebook

| Column | Table | Type | Units | Meaning | Range / values | Missing | Where it comes from |
|---|---|---|---|---|---|---|---|
| `date` | twitter_daily | date | day | the day | 2016-08-01 to 2016-12-06 | none | raw, changed to date in eda.ipynb Section 2 |
| `ts_rel_days` | twitter_daily | int | days | days from election day (negative = before) | -99 to 28 | none | raw |
| `N` | twitter_daily | int | exposures | all political news links people could have seen that day | 238,332 to 1,514,602 | none | raw |
| `black`, `red`, `orange` | twitter_daily | int | exposures | exposures to each type of fake news site | 1,315 to 20,738 | none | raw |
| `pct_fake` | twitter_daily | float | fraction | (black + red + orange) / N | 0.019 to 0.083 | none | raw |
| `website` | twitter_sites | text | – | website name, like cnn.com | 606 sites | none | raw |
| `N` | twitter_sites | int | exposures | total exposures to that website | 47 to 3,404,953 | none | raw |
| `id` | news | text | – | PolitiFact article ID | politifact7 to politifact15655 | none | raw |
| `news_url` | news | text | – | link to the article | URLs | NaN (61 rows) | raw |
| `title` | news | text | – | headline | text | none | raw |
| `tweet_ids` | news | text | – | tweet IDs separated by tabs | list of numbers | NaN (255 rows) | raw |
| `label` | news | text | – | fake or real | fake, real | none | **we made it**: from the file name, eda.ipynb Section 0 |
| `num_tweets` | news | number | tweets | how many tweets shared the article | 0 to 29,060 | set to 0 | **we made it**: counted from `tweet_ids`, Sections 1 and 2 |
| `domain` | ratings | text | – | website name | 11,520 sites | none | raw (Lin et al. 2023) |
| `pc1` | ratings | float | score | website quality score, 1 = best | 0 to 1 | none | raw |
| `quality` | joined | category | – | quality group | low (< 0.4), medium (0.4–0.7), high (> 0.7) | none | **we made it**: from `pc1`, Section 4 Plot 3 |
| `Rating` | facebook | text | – | BuzzFeed's truth rating | mostly true, mostly false, mixture of true and false, no factual content | none | raw (BuzzFeed 2016) |
| `share_count` | facebook | float | shares | times the post was shared | 1 to 1,088,995 | NaN (70 rows) | raw |

---

## 4. Recipe

All steps are in `eda.ipynb`. If you run it from top to bottom, you get the same tables. We don't store any data files in the repo.

1. **Download** all the CSVs from their GitHub links (Section 0). Twitter daily has 128 rows, Twitter sites 606, FakeNewsNet 432 fake + 624 real, Facebook 2,282, ratings 11,520.
2. **Add a `label` column** (fake or real) and stack the two FakeNewsNet files together (Section 0), which gives 1,056 rows.
3. **Count the tweets** for each article to make `num_tweets` (Section 1).
4. **Fix the date type**: change `date` from text to a date and sort by it (Section 2, Anomaly 1 cell).
5. **Check the Twitter data**: there are no missing days, and `pct_fake` matches the counts (Section 2, Anomaly 2 cell).
6. **Drop 4 rows** whose ID was labeled both fake and real (Section 2, Cleaning cell), leaving 1,052.
7. **Drop 20 junk rows** like "Account Suspended" and "website is for sale" (Section 2, Cleaning cell), leaving 1,032.
8. **Set missing `num_tweets` to 0** (Section 2, Cleaning cell), which gives `news_clean`: 1,032 rows (423 fake, 609 real).
9. **Join** `twitter_sites` with `ratings` on `website` = `domain` (Section 4, Plot 3).
   - **Matched: 534.** Not matched: 72 Twitter websites, mostly subdomains like motto.time.com (1.6% of exposures).
10. **Make `quality` groups** from `pc1` (Section 4, Plot 3).

The Facebook data is only compared with FakeNewsNet, not joined, so it has no join key.

---

## 5. Access and reuse (FAIR)

| Dataset | Link | Version we used | Format | License | Can others reuse our data? |
|---|---|---|---|---|---|
| Twitter 2016 | https://doi.org/10.5281/zenodo.2483311 | v0.6, accessed Oct 2026 | CSV | "Other (Open)" | Yes, by running our notebook. Cite Grinberg et al. |
| FakeNewsNet | https://github.com/KaiDMML/FakeNewsNet | commit 654361e, Oct 2026 | CSV | no license file | Rebuild it with our notebook. We don't share copies since there's no license. |
| Facebook fact-check | https://github.com/BuzzFeedNews/2016-10-facebook-fact-check | commit 339d3e9, Oct 2026 | CSV | no license file | Same as FakeNewsNet |
| Website ratings | https://github.com/hauselin/domain-quality-ratings | commit 5671d57, Oct 2026 | CSV | no license file, but the README says "Feel free to reuse the data" | Yes, cite Lin et al. |

- **Findable:** Only the Twitter data has a DOI. The others are GitHub links, so we wrote down the commit we used.
- **Accessible:** All of them download for free with no login.
- **Interoperable:** All are CSV files.
- **Reusable:** Only the Twitter data has a license, so reusing the others is less clear.

---

## 6. People in the data (Dravya)

People are represented in our datasets mostly through their activity on social media and through the news websites or pages they interact with. In the Twitter 2016 dataset, the people included were registered U.S. voters who used Twitter and could be matched to voter registration records. This means people who were not registered voters, did not use Twitter, or could not be matched are missing. The study said the group was largely similar to U.S. voters on Twitter in age, gender, race, and political party, but it still does not represent everyone. Bots were removed, although some partly automated accounts may have remained.

There are also possible problems with how groups are represented. Fake news is sometimes labeled at the website level instead of the individual story level, so all writers or owners of a website may be grouped together even if not every article is false. FakeNewsNet only includes articles checked by PolitiFact and tweets found by searching article headlines, so it may miss other political topics, users, or ways that stories were shared. The Facebook dataset is also limited because it only includes posts from nine selected political pages. These choices can affect conclusions about which people or groups are more likely to share or see fake news.

Using the CARE principles also helps us think about the people behind the data. Decisions based on these datasets could affect social media users, journalists, news organizations, political groups, and researchers. Since users may not have directly agreed to be part of these studies, the data should be used carefully and without trying to identify individual people.

CARE principles

Collective benefit: The data can help researchers understand how misinformation spreads and potentially improve public awareness.

Authority to control: Social media users had limited control over how their public activity was included, so restrictions against re-identification are important.

Responsibility: Researchers should explain the limitations of the data and avoid making broad claims about groups that are not fully represented.

Ethics: The data should be used in ways that protect privacy and do not unfairly label or target individual users or communities.
