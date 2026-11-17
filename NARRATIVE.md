# Analyses that did not make it into the story

These are things we tried during EDA that we left out of our three findings, and why.

- **Scatter plot of website quality vs. Twitter views (log scale).** We plotted each website's quality score against its total Twitter views. We replaced it with a bar chart of share of views by quality group, because a log-scale scatter plot is hard for most people to read. We kept its Spearman correlation (0.17) as a number.

- **Daily news volume vs. fake news share.** On days with more total news, the fake news share was lower (correlation -0.51). We only used this as a check for Finding 2, because big news days and the election happen at the same time, so we can't separate the two effects.

- **Fake news split by site type (black, red, orange).** All three types dropped after the election: black went from 0.77% to 0.47%, red from 2.72% to 1.95%, and orange from 2.09% to 1.48%. We left it out because it tells the same story as Finding 2.

- **Mean tweets instead of median.** Using the mean flips the result: real articles average 643 tweets and fake articles 376, because a few real articles went hugely viral. We mention this in Finding 1, but we use medians because the data is very skewed.

- **Facebook posts rated "mixture of true and false" or "no factual content".** We left these out of the main comparison because they are not clearly true or false.

- **How concentrated views are.** The top 10% of low-quality sites got 66% of low-quality views, but the top 10% of high-quality sites got 78% of high-quality views. We didn't make this its own finding, because most views going to a few sites is true for all news sites, not just fake news.

- **GossipCop part of FakeNewsNet.** This part is about celebrity news. We didn't use it because our questions are about political news.