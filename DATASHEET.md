# Datasheet: Fake News on Social Media (CS 418, Fall 2026)

**Team:** Krish Patel, Mahi Patel, Dravya Dholakiya
**Repo:** https://github.com/kp5036/CS418_Social_Media
**Data accessed:** October 2026

This file documents the data we used, so someone in the future can decide if they can trust it and rebuild it.
When the publisher does not say something, we write **"not documented by the publisher."**

| Part | Who |
|---|---|
| 1. Datasheets for the primary datasets | Krish |
| 2. Scope | Mahi |
| 3. Codebook | Krish |
| 4. Recipe | Krish |
| 5. Access and reuse | Krish |
| 6. People in the data | Dravya |

---

## Summary cards

### Primary 1: Fake News on Twitter During the 2016 Election

| | |
|---|---|
| **Made by** | Grinberg, Joseph, Friedland, Swire-Thompson and Lazer (Lazer Lab, Northeastern University) |
| **Paper** | *Science* 363(6425), 374–378, 2019. https://doi.org/10.1126/science.aau2706 |
| **What it is** | Public summary tables from a study of 16,442 Twitter accounts of U.S. registered voters |
| **Tables we use** | `daily_counts_exposures.csv` (128 rows, one per day) and `total_exposure_counts.csv` (606 rows, one per news website) |
| **Time** | Aug 1 – Dec 6, 2016 |
| **Place** | United States |
| **Labels** | Fake news is labeled by **website**, not by story (black / red / orange site lists) |
| **License** | Zenodo: "Other (Open)". No license file on GitHub. |
| **Biggest limit** | Only daily and per-site totals are public. Data about individual people is restricted. |

### Primary 2: FakeNewsNet (PolitiFact part)

| | |
|---|---|
| **Made by** | Shu, Mahudeswaran, Wang, Lee and Liu (Data Mining and Machine Learning Lab, Arizona State University) |
| **Paper** | *Big Data* 8(3), 2020. arXiv: https://arxiv.org/abs/1809.01286 |
| **What it is** | News articles that PolitiFact fact-checked as fake or real, plus the IDs of tweets that shared them |
| **Files we use** | `politifact_fake.csv` (432 rows) and `politifact_real.csv` (624 rows), one row per article |
| **Time** | Not documented by the publisher (no date column) |
| **Place** | Mostly U.S. political news (PolitiFact is a U.S. fact-checker) |
| **Labels** | Fake / real by **article**, from PolitiFact's fact-checks |
| **License** | No license file. README says "(C) 2019 Arizona Board of Regents on Behalf of ASU". |
| **Biggest limit** | Only tweet IDs are shared, not the tweets. 255 articles have no tweets at all. |

---

## 1. Datasheets

### 1A. Twitter 2016 (Grinberg et al.)

**Motivation**
- *Why was it made?* To learn "how ordinary citizens experience misinformation on social media platforms" during the 2016 election (Grinberg et al. 2019).
- *Who made it?* Nir Grinberg, Kenneth Joseph, Lisa Friedland, Briony Swire-Thompson, and David Lazer.
- *Who paid for it?* The paper says: "D.L. acknowledges support by the ESRC ES N012283/1 and ARO W911NF-12-1-0556." The voter records came from the company TargetSmart.

**Composition**
- *What is one row?* In the daily table, one row is **one day**: how many political news links the panel *could have seen* that day (`N`), and how many of those came from fake news sites. In the website table, one row is **one news website** and its total exposures.
- *How many rows?* 128 days and 606 websites.
- *All of it or a sample?* A sample. The panel is 16,442 Twitter accounts that the authors "linked a sample of U.S. voter registration records to." Exposures come from a random sample of tweets by the accounts each person follows, and only count political tweets with a link.
- *What is missing?* Everything about individual people (who saw or shared what) is in a **restricted release**. The README says these files "contain individual-level data and can only be found in the restricted access release." How the 606 websites in the website table were chosen is not documented by the publisher. The README only says the file "is just used for sizing for Figure 7."

**Collection process**
- *How?* Tweets were collected from Twitter for accounts matched to voter registration records. A website counts as fake if it is on a list built from fact-checkers, journalists, and academics. The paper describes three groups: **black** (171 sites that publish "almost exclusively fabricated stories"), **red** (64 sites flagged by Snopes with a clearly flawed editorial process), and **orange** (65 sites where the annotators were less sure).
- *When?* Aug 1 – Dec 6, 2016.
- *Did people know or consent?* The paper says the study "was approved by Northeastern University's Institutional Review Board." Whether the account owners were told or asked is **not documented by the publisher**. The tweets were public.

**Preprocessing**
- *What did the publisher do?* They removed bot accounts ("stringent measures to exclude bot accounts"), kept only political tweets with links, labeled websites as fake or not, and added up the counts by day and by site. The website network was k-anonymized (README).
- *Is the raw data available?* Only by request. The README says to "contact d.lazer@northeastern.edu", and people must sign an agreement that they "shall not attempt to identify, reidentify, or otherwise deanonymize the dataset."

**Uses**
- *Used for:* Figure 1 (daily fake news share) and Figure 7 (website network) in the *Science* paper. The paper found that "only 1% of individuals accounted for 80% of fake news source exposures."
- *Should NOT be used for:* judging whether a single *story* is true (labels are per website), identifying people, or making claims about all Americans or other platforms. The authors say the results "might not generalize to other platforms."

**Distribution**
- GitHub repo: https://github.com/LazerLab/twitter-fake-news-replication
- Zenodo DOI: https://doi.org/10.5281/zenodo.2483311 (version v0.6 is https://doi.org/10.5281/zenodo.2651401, published April 25, 2019)
- License: Zenodo lists it as "Other (Open)". There is no LICENSE file in the GitHub repo.

**Maintenance**
- Maintained by the Lazer Lab. The last release is v0.6 (April 2019). Plans for updates are not documented by the publisher. You would know about changes from new Zenodo versions or GitHub commits. We used commit `ff58308`.

### 1B. FakeNewsNet (Shu et al.)

**Motivation**
- *Why was it made?* The paper says "the lack of comprehensive and community-driven fake news datasets has become one of major roadblocks" for fake news research.
- *Who made it?* Kai Shu, Deepak Mahudeswaran, Suhang Wang, Dongwon Lee, and Huan Liu.
- *Who paid for it?* We could not find a funding statement in the arXiv paper or the GitHub README. Not documented by the publisher in the sources we read.

**Composition**
- *What is one row?* One news article that PolitiFact fact-checked, with `id`, `news_url`, `title`, and `tweet_ids` (README: "Tweet ids of tweets sharing the news. This field is list of tweet ids separated by tab.")
- *How many rows?* 432 fake and 624 real, which matches Table 2 of the paper.
- *All of it or a sample?* A sample. It only has articles PolitiFact checked *and* the team could collect. It is not all political news.
- *What is missing?* There is no date column. The article text and the tweets themselves are not included. The README says "Complete dataset cannot be distributed because of Twitter privacy policies and news publisher copy rights." 255 articles have no tweet IDs and 61 have no URL.

**Collection process**
- *How?* Labels come from PolitiFact, where "journalists and domain experts review the political news." Article pages were downloaded from their URLs. If a page was gone, the team used the Wayback Machine. Tweets were found with Twitter's Advanced Search API using "search queries … formed from the headlines of news articles."
- *When?* Not documented by the publisher. The IDs run from `politifact7` to `politifact15655`.
- *Did people know or consent?* Twitter users were not asked. There is no privacy or consent statement in the paper.

**Preprocessing**
- *What did the publisher do?* They removed special characters from the headline searches and only share IDs, not tweet content.
- *Is the raw data available?* Partly. The repo has scripts to re-download the tweets with Twitter API keys, but the Twitter (now X) API is no longer free, so this may not work anymore.

**Uses**
- *Used for:* fake news detection research. The paper lists "fake news detection, evolution, mitigation, and malicious account detection."
- *Should NOT be used for:* labeling individual Twitter users as fake news spreaders, or treating tweet counts as the number of people who saw an article. Tweets were found by searching for the headline, so some may be missed.

**Distribution**
- GitHub: https://github.com/KaiDMML/FakeNewsNet (CSV files in the `dataset` folder)
- License: there is no LICENSE file. The README says "(C) 2019 Arizona Board of Regents on Behalf of ASU" and asks users to cite the papers.

**Maintenance**
- Maintained by the ASU DMML lab. The README says "Please use the current up-to-date version of dataset", and the old version is in the branch `old-version`. You would know about updates from GitHub commits. We used commit `654361e`.

---

## 2. Scope (Mahi)

> TODO Mahi: for each research question, fill in the target population, access frame, sample, what is missing, and what that limits.

| Research question | Target population | Access frame | Sample | Who/what is missing | What we can't claim |
|---|---|---|---|---|---|
| 1. Does the typical fake article get shared more than the typical real one? | TODO | TODO | TODO | TODO | TODO |
| 2. How did the fake news share on Twitter change over time? | TODO | TODO | TODO | TODO | TODO |
| 3. Is low-quality news concentrated in a few websites, and do they get more views? | TODO | TODO | TODO | TODO | TODO |

---

## 3. Codebook

"Origin" says where the column comes from: the raw file, or the notebook cell that makes it.

### Twitter daily counts (`twitter_daily`)

| Column | Type | Units | Meaning | Range / allowed values | Missing | Origin |
|---|---|---|---|---|---|---|
| `date` | datetime | day | the day | 2016-08-01 to 2016-12-06 | none | raw, changed from text to date in `eda.ipynb` Section 2 |
| `ts_rel_days` | int | days | days from election day (negative = before) | -99 to 28 | none | raw |
| `N` | int | exposures | all political news links the panel could have seen that day | 238,332 to 1,514,602 | none | raw |
| `black` | int | exposures | exposures to "black" (fabricated) fake sites | 1,315 to 6,110 | none | raw |
| `red` | int | exposures | exposures to "red" fake sites | 6,390 to 20,738 | none | raw |
| `orange` | int | exposures | exposures to "orange" fake sites | 4,250 to 17,813 | none | raw |
| `pct_fake` | float | fraction (0–1) | (black + red + orange) / N | 0.019 to 0.083 | none | raw (we checked it matches the counts) |

### Twitter website totals (`twitter_sites`)

| Column | Type | Units | Meaning | Range / allowed values | Missing | Origin |
|---|---|---|---|---|---|---|
| `website` | text | – | website domain, like `cnn.com` | 606 unique values | none | raw |
| `N` | int | exposures | total exposures to that site, Aug–Dec 2016 | 47 to 3,404,953 | none | raw |

### FakeNewsNet (`news`, then `news_clean`)

| Column | Type | Units | Meaning | Range / allowed values | Missing | Origin |
|---|---|---|---|---|---|---|
| `id` | text | – | PolitiFact article ID | `politifact7` to `politifact15655` | none | raw |
| `news_url` | text | – | link to the article | any URL (some lack `http`, 196 are web.archive.org links) | `NaN` (61 rows) | raw |
| `title` | text | – | article headline | any text | none | raw |
| `tweet_ids` | text | – | tweet IDs separated by tabs | list of numbers | `NaN` (255 rows) | raw |
| `label` | text | – | PolitiFact's verdict | `fake`, `real` | none | **made by us**: added from the file name in `eda.ipynb` Section 0 |
| `num_tweets` | float | tweets | number of tweet IDs for the article | 0 to 29,060 | `NaN` in `news`, set to 0 in `news_clean` | **made by us**: `tweet_ids.str.split().str.len()` in Section 1, missing filled with 0 in Section 2 |

### Website quality ratings (`ratings`, secondary)

| Column | Type | Units | Meaning | Range / allowed values | Missing | Origin |
|---|---|---|---|---|---|---|
| `domain` | text | – | website domain | 11,520 unique values | none | raw (Lin et al. 2023) |
| `pc1` | float | score (0–1) | combined quality score from expert ratings, 1 = best | 0.0 to 1.0 | none | raw |

### Joined table (`joined` = `twitter_sites` + `ratings`)

| Column | Type | Units | Meaning | Range / allowed values | Missing | Origin |
|---|---|---|---|---|---|---|
| `quality` | category | – | quality group from `pc1` | `low` (0–0.4), `medium` (0.4–0.7), `high` (0.7–1) | none | **made by us**: `pd.cut` in Section 4, Plot 3 |

### Facebook fact-check (`facebook`, secondary)

| Column | Type | Units | Meaning | Range / allowed values | Missing | Origin |
|---|---|---|---|---|---|---|
| `Rating` | text | – | BuzzFeed's truth rating of the post | `mostly true`, `mostly false`, `mixture of true and false`, `no factual content` | none | raw (BuzzFeed News 2016) |
| `share_count` | float | shares | times the post was shared on Facebook | 1 to 1,088,995 | `NaN` (70 rows) | raw |
| `Category` | text | – | type of page | `mainstream`, `left`, `right` | none | raw |

---

## 4. Recipe (raw files → analysis tables)

Everything is in `eda.ipynb`. Run it from top to bottom and you get the same tables. No data files are stored in this repo.

| Step | What happens | Code | Rows after |
|---|---|---|---|
| 1 | Download the Twitter daily counts CSV from the LazerLab GitHub | `eda.ipynb` Section 0 | 128 |
| 2 | Download the Twitter website totals CSV | Section 0 | 606 |
| 3 | Download `politifact_fake.csv` and `politifact_real.csv` from FakeNewsNet and add `label` = fake / real | Section 0 | 432 + 624 |
| 4 | Stack fake and real into one table `news` | Section 0 (`pd.concat`) | 1,056 |
| 5 | Download the Facebook fact-check CSV and the website ratings CSV | Section 0 | 2,282 and 11,520 |
| 6 | Count tweets per article: `num_tweets` | Section 1 | 1,056 |
| 7 | Change `date` from text to datetime and sort by date | Section 2, "Anomaly 1" cell | 128 |
| 8 | Check there are no missing days and that `pct_fake` matches the counts (0 problems) | Section 2, "Anomaly 2" cell | 128 |
| 9 | Drop articles whose `id` is labeled both fake and real | Section 2, "Cleaning" cell | 1,056 → 1,052 (4 dropped) |
| 10 | Drop junk pages ("Account Suspended", "for sale", "under construction", MSN homepage) | Section 2, "Cleaning" cell | 1,052 → 1,032 (20 dropped) |
| 11 | Set missing `num_tweets` to 0, giving `news_clean` | Section 2, "Cleaning" cell | 1,032 (423 fake, 609 real; 250 with 0 tweets) |
| 12 | **Join** `twitter_sites` with `ratings`, giving `joined` | Section 4, Plot 3 (`merge`) | 534 |
| 13 | Make `quality` groups from `pc1` | Section 4, Plot 3 | 534 |

**Join details (step 12)**
- Key: `twitter_sites.website` = `ratings.domain` (inner join)
- **Matched:** 534 of 606 Twitter websites
- **Not matched:** 72 Twitter websites (1.6% of all exposures). Most are subdomains like `motto.time.com` that don't match the parent domain in the ratings. 10,986 rated websites are not in the Twitter data, which is expected.

The Facebook data is **compared** with FakeNewsNet (shares by rating vs. tweets by label), not joined, so it has no join key.

---

## 5. Access and reuse (FAIR)

| Dataset | Persistent link | Version / accessed | Format | License / terms | Can others reuse our processed data? |
|---|---|---|---|---|---|
| Twitter 2016 (primary) | https://doi.org/10.5281/zenodo.2483311 | v0.6 (Zenodo), GitHub commit `ff58308`, Oct 2026 | CSV | Zenodo "Other (Open)"; no GitHub license | Yes, our code can be reused. We don't publish the data, but anyone can rebuild it with `eda.ipynb`. Cite Grinberg et al. 2019. |
| FakeNewsNet (primary) | https://github.com/KaiDMML/FakeNewsNet | commit `654361e`, Oct 2026 | CSV (tab-separated tweet IDs inside one column) | No license file; "(C) 2019 Arizona Board of Regents on Behalf of ASU" | Only rebuild it from the source with our code. Since there is no open license, we don't share copies. Cite Shu et al. |
| Facebook fact-check (secondary) | https://github.com/BuzzFeedNews/2016-10-facebook-fact-check | commit `339d3e9`, Oct 2026 | CSV | No license file | Same: rebuild from the source and cite BuzzFeed News. |
| Website quality ratings (secondary) | https://github.com/hauselin/domain-quality-ratings (paper: https://doi.org/10.1093/pnasnexus/pgad286) | commit `5671d57`, Oct 2026 | CSV | No license file; README says "Feel free to reuse the data" | Yes, with citation of Lin et al. 2023. |

**FAIR check**
- **Findable:** The Twitter data has a DOI. The others only have GitHub links, which could disappear, so we recorded the commit we used.
- **Accessible:** All four download with a plain URL and no login. The individual-level Twitter data needs a signed agreement.
- **Interoperable:** All are CSV. FakeNewsNet puts a list of IDs inside one column, which needs extra parsing.
- **Reusable:** Only the Twitter data has a stated license. The others have none, so reuse beyond research with citation is unclear.

---

## 6. People in the data (Dravya)

> TODO Dravya: who is in the data, how they are represented, which groups could be missing or misrepresented, and the CARE principles.
