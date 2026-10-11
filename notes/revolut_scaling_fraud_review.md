# Scaling fraud review at Revolut: why a ranked queue beats extra headcount

Mehak Gupta | October 2026 | Draft

## 1. The pressure

Revolut closed 2025 with 68.3m retail customers, up 30%, and revenue of $6.0bn, up 46% (Revolut, 2026). It also reports that the number of potential fraud cases reviewed each day rose tenfold, without stating the baseline or the absolute volume (Revolut, 2026). If that rise is year on year, review workload per customer grew about 7.7 times (10 divided by 1.3). That is my calculation, not a reported figure.

Fraud review is also a reputational issue. In the first eight months of 2025, Revolut had more complaints about authorised push payment fraud referred to the Financial Ombudsman than the other firms in Which?'s comparison: 1,875, against 1,161 for Barclays and 1,063 for Monzo. These are raw counts, not adjusted for customer numbers (Cavaglieri, 2025).

## 2. Why more reviewers is not enough

Revolut had already doubled its financial crime headcount between 2021 and 2023 (Methri, 2023), and told Which? that the team makes up more than a third of its global workforce (Cavaglieri, 2025). Matching a tenfold workload with headcount is hard to sustain. The real question is how reviewer time is spent.

My Project 1 analysis tested this on simulated PaySim transactions (Lopez-Rojas, Elmir and Axelsson, 2016). A single rule that caught 97.6% of fraud raised about 31,900 alerts a day. At an assumed 36 reviews per analyst per day, that needs about 886 analysts. A queue ranked by model score, limited to 360 alerts a day (10 analysts), caught 98.5% of fraud cases and 99.1% of fraud value. Working the rule's queue in random order at the same capacity caught 1.1%. The data is synthetic and the review time is my assumption, so the results show the method, not Revolut's reality. A sensitivity analysis in my Revolut Scaling Model notebook shows the saving does not depend on the simulated result: at a queue precision of 10%, far below the simulated 74%, a ranked queue still needs about 92% fewer reviewers than the fixed rule at 0.83%.

## 3. Proposal

Run a four-week pilot in one market and one alert type, for example outbound transfers flagged by an existing rule:

1. Rank alerts by predicted risk instead of arrival order.
2. Set the daily review cap from reviewer capacity, and send lower-ranked alerts to low-friction automated checks, such as an in-app confirmation prompt.
3. Compare against a control group that keeps the current order.

Measures of success:

- Fraud value caught per reviewer hour
- Precision at the daily cap
- Time to decision
- Share of genuine payments delayed
- Fraud complaints per million payments

## 4. Risks

- Ranking errors can push real fraud below the cap, so the pilot needs a sampled audit of unreviewed alerts.
- Automated friction can frustrate genuine customers, so delay rates must be tracked from day one.
- Tactics shift quickly. In 2023 Revolut said scams starting on four social platforms had doubled in a year and made up two thirds of attempted fraud (Methri, 2023), so models need regular retraining on recent confirmed cases.

## References

Cavaglieri, C. (2025) 'Revolut still the worst UK firm for fraud complaints, warns Which?', *Which?*, 1 October. Available at: https://www.which.co.uk/news/article/revolut-still-the-worst-uk-firm-for-fraud-complaints-warns-which-axqr43W94wLn (Accessed: 11 October 2026).

Lopez-Rojas, E., Elmir, A. and Axelsson, S. (2016) 'PaySim: a financial mobile money simulator for fraud detection', *28th European Modeling and Simulation Symposium*, Larnaca, Cyprus.

Methri, G. (2023) 'Revolut doubles financial crime headcount to combat frauds', *IBS Intelligence*, 16 October. Available at: https://ibsintelligence.com/ibsi-news/revolut-doubles-financial-crime-headcount-to-combat-frauds/ (Accessed: 11 October 2026).

Revolut (2026) *Revolut reports record profit of $2.3bn for 2025 as revenue surges to $6bn*. Available at: https://www.revolut.com/en-AT/news/revolut_reports_record_profit_of_2_3bn_for_2025_as_revenue_surges_to_6bn/ (Accessed: 11 October 2026).
