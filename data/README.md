# Data — not included in this repository

The six source CSV files are provided as part of a data analytics internship project and are **not redistributed here**. This folder documents what they contain so the notebooks can be read and the analysis understood without them.

| File | Rows | Grain (1 row =) | Key columns |
|---|---|---|---|
| `subscribers.csv` | 15,000 | one subscriber | `subscriber_id`, `signup_date`, `plan_type`, `monthly_price_usd`, `country`, `region`, `age`, `tenure_months`, `is_active`, `churn_date` |
| `titles.csv` | 9,000 | one movie or series | `title_id`, `type`, `primary_genre`, `language`, `is_original`, `license_cost_usd`, `license_expiry`, `total_watch_hours`, `total_plays` |
| `watch_history.csv` | 650,000 | one viewing session | `watch_id`, `subscriber_id`, `title_id`, `watch_date`, `device`, `content_duration_min`, `watch_duration_min`, `completion_pct`, `completed` |
| `ratings.csv` | 130,000 | one star rating | `rating_id`, `subscriber_id`, `title_id`, `rating` (1–5), `rating_date` |
| `reviews.csv` | 110,000 | one written review | `review_id`, `subscriber_id`, `title_id`, `review_text`, `sentiment`, `helpful_votes`, `review_date` |
| `watchlist.csv` | 65,000 | one saved title | `watchlist_id`, `subscriber_id`, `title_id`, `added_date`, `watched` |

**Total:** 979,000 records. Star schema with `watch_history` as the fact table and `subscribers` / `titles` as dimensions.

**Coverage:** viewing from March 2017 to May 2026; signups from January 2016 to May 2026; 20 countries, 16 genres, 14 languages, 6 device types, 3 plans.

## To run the notebooks

Place the six CSV files in the repository root (same level as `notebooks/`), then run the notebooks in order. Each notebook loads the raw files and derives what it needs, so any one of them can be run on its own.
