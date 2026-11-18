# StreamFlix — Content Analytics

End-to-end analysis of a subscription video-on-demand platform: **979,000 records across six tables**, 2017 to May 2026, taken from raw CSV files to a cleaned dataset, ten business KPIs, a five-page dashboard and a management report for the CEO.

**Analyst:** Meghana J · [LinkedIn](https://linkedin.com/in/meghana-ji-16904a21b) · meghnaj6702@gmail.com

---

## The short version

| | |
|---|---|
| Watch hours analysed | 3,333,468 across 650,000 viewing sessions |
| Active rate / churn | 74.66% active · 25.34% churn |
| Average completion | 65.31% |
| Monthly recurring revenue | $175,869 (ARR $2.11m) · ARPU $15.70 |
| Watchlist conversion | 46.23% |
| Hit concentration | top 10% of titles = 31.0% of all plays |

Three findings drove the recommendations:

1. **The ad-supported plan is the retention problem.** Basic with Ads churns at 34.3% against 24.1% on Standard and 18.1% on Premium — a 16-point spread, roughly three times the spread across age bands and regions.
2. **Premium is a third of subscribers and half of revenue.** 32.5% of active accounts, 47.6% of MRR. Losing one Premium subscriber costs as much as losing three on the ad tier.
3. **Series are ten times more efficient than films.** TV shows return 342.7 watch hours per $1 million of licence cost; films return 35.2. The gap between the best and worst *genre* is only 1.6×.

Two numbers the analysis deliberately refuses to report:

- **The growth trend.** Monthly viewing rises every month for nine years and May 2026 alone holds 34.6% of all sessions. That is an artefact of how the dataset was generated, not a business trend, so no growth rate is claimed anywhere in this project.
- **The level of retention.** 20.2% of viewing is dated *after* the subscriber churned, so the churn flag cannot be trusted as a timeline. Retention is reported as a trend (newer cohorts retain worse: 97.2% at 12 months for the 2017 cohort, 80.4% for 2025) but never as a level.

---

## Repository structure

```
├── notebooks/
│   ├── Phase1_DataCleaning_MeghanaJ.ipynb    # profiling, quality checks, QC report
│   ├── Phase2_EDA_MeghanaJ.ipynb             # 10 charts, each with observations
│   └── Phase3_KPIs_MeghanaJ.ipynb            # 10 KPIs + cohort retention bonus
├── charts/                                   # all 13 charts as PNG (150 dpi)
├── dashboard/
│   ├── Phase4_Dashboard_MeghanaJ.xlsx        # 5 pages, working dropdown filters
│   └── Phase4_Dashboard_Preview.pdf          # all five pages as a PDF
├── report/
│   └── Phase4_Report_MeghanaJ.pdf            # 3-page management summary for the CEO
├── powerbi/
│   └── StreamFlix_Measures.dax               # every DAX measure, ready to paste
├── requirements.txt
└── data/README.md                            # what the data contains (files not included)
```

---

## Phase 1 — Data loading, cleaning and quality

Loaded all six tables, profiled every column, and ran the full checklist: nulls, duplicates, data types, outliers, referential integrity, churn logic, completion consistency and categorical domains.

**The data is structurally excellent.** Zero duplicate rows, zero duplicate primary keys, zero orphan foreign keys across 955,000 event rows, every date and numeric column parsing cleanly, no session longer than its content, and `completion_pct` agreeing with the duration ratio to 0.03 points.

Only two columns carry nulls, and both are meaningful: `churn_date` (74.66%, i.e. still active) and `license_expiry` (21.84%, i.e. Originals with no third-party licence). Neither is imputed.

**The real defect is temporal, not structural:** 131,199 sessions (20.2%) are dated after the subscriber's churn date, about 20% of ratings and reviews predate signup, and 26,223 sessions postdate the title's licence expiry. Flagged rather than deleted, so later phases can include or exclude them deliberately.

Also documented: the `completed` flag uses a strict `> 90` threshold, so 535 sessions at exactly 90.0% are flagged `False`.

## Phase 2 — Exploratory analysis

Ten charts with written observations, in `notebooks/Phase2_EDA_MeghanaJ.ipynb` and as PNGs in `charts/`.

- Viewing volume by month and by year, with the artefact called out
- Genre share of watch hours — Drama 15.1%, Comedy 12.7%, Action 11.8%; top three = 39.6%
- Movies vs series — series take 86.5% of hours from 41.2% of sessions (10.8 hours per session against 1.2)
- Top 10 countries — US 27.3%, India 11.4%, UK 8.6%; top ten = 83.4%
- Plan mix, device mix (Smart TV 34.1%), age distribution (median 35; 25–44 = 55.9% of hours)
- Completion by genre — flat at 64.7%–66.8% around a 65.3% average, so genre does not drive finishing
- Review sentiment — 68.2% positive, 11.1% negative, a 6.1:1 ratio

## Phase 3 — Business KPIs

All ten KPIs from the brief, each calculated in its own labelled block, plus segmentation and the optional cohort-retention analysis.

| KPI | Value | Target | |
|---|---|---|---|
| Total watch hours | 3,333,468 | — | |
| Active rate | 74.66% | > 70% | met |
| Churn rate | 25.34% | < 30% | met |
| Avg completion | 65.31% | > 60% | met |
| MRR | $175,869 | — | ARR $2.11m |
| ARPU | $15.70 | — | below Premium price |
| Watch hours / active sub | 297.7 | — | |
| Watchlist conversion | 46.23% | > 40% | met |
| Hit concentration (top 10%) | 31.0% | — | low, spread catalogue |
| Originals share of hours | 27.17% | — | |

**Cohort retention (bonus).** Retention at 3/6/12 months is 97.7% / 95.3% / 92.2% on a churn-based definition, with subscribers too young to observe excluded from each column. The activity-based definition gives 16.4% / 19.8% / 22.0%. Both are reported and the disagreement is explained, because choosing the flattering one would be dishonest.

## Phase 4 — Dashboard and management report

`dashboard/Phase4_Dashboard_MeghanaJ.xlsx` — five pages, each with a working dropdown filter (year, format, plan, device, content type), built entirely from live formulas so every number traces back to the Data sheet: engagement overview, content performance, subscriber insights, experience, and catalogue investment.

`report/Phase4_Report_MeghanaJ.pdf` — a three-page summary for the CEO in plain language, with an executive summary, three findings, four risks, four opportunities, four specific recommendations and a KPI scorecard. Recommendations are quantified: bringing ad-tier churn down to the Standard rate retains about 455 subscribers a year, worth roughly $38,000 in subscription revenue.

`powerbi/StreamFlix_Measures.dax` — every measure written in DAX for a Power BI version of the same dashboard, including the retention measures and the watch-hours-per-$1M efficiency ratio.

---

## Reproducing this

```bash
pip install -r requirements.txt
```

Place the six source CSV files in the repository root (`subscribers.csv`, `titles.csv`, `watch_history.csv`, `ratings.csv`, `reviews.csv`, `watchlist.csv`), then run the notebooks in order. Phase 1 rebuilds its own frames; Phase 2 and 3 each load the raw CSVs and re-derive what they need, so any notebook can be run on its own.

Charts are written to `charts/` at 150 dpi.

---

## What I would do differently with more time

- **Model the retention event properly.** The dataset records churn as a flag with a date, and the date does not constrain activity. With cancellation events and session-level dates, cohort curves could be measured rather than inferred.
- **Add advertising revenue.** The ad tier's economics cannot be judged without revenue per user, which is not in the data. Today the tier looks expensive per hour of streaming delivered and we cannot say whether it pays for itself.
- **Test the series-over-films finding against a proper cost model.** Licence cost is a single number per title; it does not split acquisition from production or account for amortisation across the licence term.

---

*Dataset provided as part of a data analytics internship project. The underlying CSV files are not redistributed in this repository.*
