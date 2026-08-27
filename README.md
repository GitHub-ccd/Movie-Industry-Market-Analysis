# 🎬 Movie Industry Market Analysis

[![Python](https://img.shields.io/badge/Python-3.7+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data_Wrangling-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine_Learning-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c?style=for-the-badge)](https://matplotlib.org/)
[![TMDB API](https://img.shields.io/badge/TMDB_API-Data_Enrichment-01b4e4?style=for-the-badge&logo=themoviedb&logoColor=white)](https://www.themoviedb.org/)
[![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup-Web_Scraping-336699?style=for-the-badge)](https://www.crummy.com/software/BeautifulSoup/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Analytics_Notebooks-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org/)

A Flatiron Data Science bootcamp project from 2020 — box office EDA and a genre/runtime/seasonality strategy write-up on IMDb, Box Office Mojo, Rotten Tomatoes, and TMDB data — rebuilt in 2026 with two things that didn't exist before: a Random Forest revenue regressor and a TF-IDF critic-sentiment classifier.

---

## What this was

The assignment (Flatiron Mod 1, 2020) used a fictional business case: pretend a company — the brief used Microsoft — wants to get into original film production and needs help figuring out what kind of films to make. Four questions came with it: which genres do best at the box office, what runtime works per genre, when's the best time of year to release, and which genre *pairs* do best. That's a real assignment premise, not something I'm claiming happened — I want to be clear about that distinction because the 2026 rebuild's first pass at this README blurred it into something that read like an actual consulting engagement.

The real work was EDA: pull IMDb's title/ratings/crew tables, Box Office Mojo's gross figures, Rotten Tomatoes reviews, and TMDB's metadata, then merge, clean, and chart. The provided datasets had big missing-data gaps, and a classmate — Jesse Numan — had an idea for closing them: scrape IMDbPro directly. I adapted his scraper (a JavaScript console auto-scroller plus BeautifulSoup HTML parsing to get past the login lazy-loading) and used it to pull about 14,000 additional records. That collaboration is the reason the dataset was usable at all, and it got dropped from the README somewhere in the 2026 rebuild — restoring it here.

Multi-label genre data was a mess — a film could carry three or four genre tags, and a lot of the standard analysis falls apart once you're not looking at one clean category. My fix was to keep only the first two genre tags per film and call the result a "binary genre." That's my own term, coined for this project, and the label choice — and the one-hot genre features the 2026 models still use — traces straight back to it.

Data cleaning had real friction I didn't smooth over in the commit history: `rt.movie_info.tsv` failed to load, then failed to clean, then finally worked, across three separate commits in March 2020. That's not a complaint about the tooling — it's what actually happened, and I'd rather the log show it than pretend the pipeline came together cleanly on the first pass.

## What changed in the 2026 rebuild

The 2020 project stopped at EDA — charts, a genre-profit breakdown, and a written recommendation. There was no predictive model and no NLP. The rebuild added both, and neither is a refactor of something that already existed; they're new work layered on top of the original analysis.

**Revenue regressor** (`Predictive_ROI_Modeling.ipynb`). A Random Forest trained on production budget, release month, TMDB popularity/vote metrics, and the binary-genre one-hot flags, across the 1,976 titles that survive the merge and cleaning. I picked a tree ensemble on purpose, not by default: budget doesn't map to revenue linearly — a $200M film doesn't earn ten times what a $20M film earns, there's saturation at the top and a floor at the bottom — and budget interacts with genre, since $50M buys a lot in horror and almost nothing in a VFX action film. A tree ensemble picks up both without me specifying either, and it doesn't assume anything about the shape of the residuals, which matters because revenue is heavily right-skewed. Linear regression would've needed me to guess the transformations up front.

Trained with TMDB's `popularity`, `vote_average`, and `vote_count` included, the model reports $R^2 = 0.7385$ — but those three features accumulate *after* a film releases and are partly driven by how well it actually did, which inflates the score without providing real pre-release signal. The notebook drops them and fits on pre-release features only: $R^2 = 0.5287$ (MAE $72.89M, RMSE $138.75M). **0.5287 is the number I'm reporting** — it's what the model can actually see before a film comes out. 0.7385 is stated here for context on why the ablation happened; the notebook fits the ablated feature set only, it doesn't compute both side by side anymore.

**Sentiment classifier** (`NLP_Review_Sentiment_Analysis.ipynb`). TF-IDF (2,500 n-gram features, unigrams and bigrams) into a Logistic Regression classifier on Rotten Tomatoes critic reviews, predicting Fresh vs. Rotten. The source file has 54,432 reviews; 48,869 have both review text and a label, and the model trains on all of them — accuracy 75.3%, ROC-AUC 0.8233.

**Two claims the rebuild made that weren't true, caught during the retrofit pass and corrected:** the README claimed a Gradient Boosting Regressor was compared against Random Forest — it was imported and never fit. It also claimed Naive Bayes was compared against Logistic Regression for sentiment — never trained. Both are gone now. The rebuild also stated "2,380+" enriched titles; that number was the pre-deduplication merge count, not the actual training set (1,976, after removing duplicate movie/year matches). Fixed.

Baseline: [`baseline-pre-rebuild`](../../tree/baseline-pre-rebuild-branch) — the repo exactly as it stood before any of this, for anyone who wants to read the diff themselves.

## What I think the original got wrong

The assignment asked for "actionable insights" to guide a capital-allocation decision, and 2020-me delivered bar charts, pie charts, and a narrative conclusion — not anything that quantifies a prediction or a confidence level. For a business case that's explicitly about where to put money, "Family-SciFi genre does well at box office" is an observation, not a decision-ready answer. I don't think that gap was obvious to me at the time; EDA felt like the deliverable because EDA was what Mod 1 taught.

I also think the original framing tried to have it both ways — a joke title ("What are you thinking Bill? Can I help!!!") sitting on top of business language about capital allocation and executive decision-makers. Picking one register would have made the project easier to trust either as a fun bootcamp exercise or as a serious analysis; mixing them undercut both.

## What the original got right

The core questions — genre, runtime, seasonality, genre pairing — are still exactly what the 2026 models answer, just with a regressor and a classifier instead of a bar chart. Nobody had to go back and re-scope the project six years later.

The binary-genre taxonomy survived unchanged. It's a crude simplification — collapsing a film to its first two listed genres throws out real information — but it's the same simplification the 2026 feature engineering uses, because it still works for this purpose and I haven't found a reason to replace it.

The four-source data pipeline (IMDb, Box Office Mojo, Rotten Tomatoes, TMDB) plus the IMDbPro scrape is still the foundation everything else sits on. That part didn't need modernizing.

## Roads not taken

| Approach | Why it was dropped |
|---|---|
| Reporting $R^2 = 0.7385$ (with TMDB engagement features) as the headline number | Re-ran without `popularity`, `vote_average`, `vote_count` and $R^2$ dropped to 0.5287. Those features are downstream of the box-office outcome, so they inflate the score without providing real pre-release signal — dropped in favor of reporting the ablated number as the one that means something. |
| Computing both the leaky and ablated $R^2$ side by side in-notebook | Original approach, briefly implemented as a separate leakage-check cell. Superseded once the feature-set edit above became the only fitting cell — the notebook now fits the ablated set exclusively, so 0.7385 is stated in the README for context but isn't a number the notebook computes anymore. |
| A genre-profit table in the rebuild's original README (Animation+Adventure at $310M+, and similar figures for three other genre pairs) | Doesn't trace to `Visualization.ipynb` — that notebook is unchanged since 2020, and its own written conclusion says "Family-SciFi genre does well at box office and have high profits compared to other genres," a different genre than the table claimed. Removed rather than rebuilt. |

This rebuild otherwise went in close to a straight line — I picked Random Forest and TF-IDF up front and didn't benchmark either against XGBoost, LightGBM, or a transformer embedding. The comparison work I'd want here isn't done.

---

## 🛠️ Data Engineering & Multi-Source ETL Pipeline

### Data Sources Ingested & Merged
| Source | Format | Records | Primary Features Used |
| :--- | :--- | :--- | :--- |
| **IMDb Data Dumps** | CSV / TSV | 5+ Tables | Title basics, ratings, crew, runtime, principals |
| **TheNumbers (`tn.movie_budgets.csv`)** | CSV | 5,782 | Production budget, domestic gross, worldwide gross |
| **Box Office Mojo (`bom.movie_gross.csv`)** | CSV | 3,387 | Annual box office gross, studio |
| **Rotten Tomatoes (`rt.reviews.tsv`)** | TSV | 54,432 | Critic review text, rating, fresh/rotten status |
| **TMDB API Export (`tmdb.movies.csv`)** | CSV / REST API | 26,517 | Popularity indices, vote counts, release dates, TMDB genre codes |
| **Custom IMDbPro Scraping** | Web / HTML | ~14,428 | Consolidated budget, gross, region codes, ratings |

### Current pipeline (2020 EDA + 2026 ML additions)
```
   Multi-Source Ingestion (IMDb, Mojo, RT) ──►  Web Scraping (IMDbPro) + TMDB API
                     │                                         │
                     ▼                                         ▼
   Wrangling & Binary Genre Engineering   ──►  Exploratory Visual Analytics
                     │                                         │
                     ▼                                         ▼
   Random Forest Revenue Regressor        ──►  TF-IDF + Logistic Regression
   (2026)                                       Sentiment Classifier (2026)
```

<div align="center">
  <img src="images/predictive_roi_features.png" alt="Feature Importances" width="85%" />
  <p><em>Top 10 feature importances, revenue regressor.</em></p>
</div>

<div align="center">
  <img src="images/predictive_actual_vs_pred.png" alt="Actual vs Predicted Revenue" width="65%" />
  <p><em>Actual vs. predicted worldwide gross revenue ($ millions).</em></p>
</div>

<div align="center">
  <img src="images/nlp_sentiment_confusion_matrix.png" alt="NLP Sentiment Confusion Matrix" width="55%" />
  <p><em>Rotten Tomatoes critic review sentiment confusion matrix.</em></p>
</div>

---

## 📁 Repository Structure & Notebook Workflow

```
Movie-Industry-Market-Analysis/
├── Data/                             # Raw and processed datasets (IMDb, TMDB, Mojo, TheNumbers)
│   ├── tmdb.movies.csv               # TMDB API enriched dataset (26,517 rows)
│   ├── tn.movie_budgets.csv          # Financial budgets & worldwide gross
│   ├── rt.reviews.tsv                # Rotten Tomatoes critic reviews (54,432 rows)
│   └── movie_main.csv                # Master clean analytical dataset
├── images/                           # Analytical & ML chart outputs
│   ├── predictive_roi_features.png   # ML Feature Importances
│   ├── predictive_actual_vs_pred.png # Actual vs Predicted Revenue ($M)
│   ├── nlp_sentiment_confusion_matrix.png # NLP Sentiment Confusion Matrix
│   ├── profit_genres2.jpeg           # Original 2020 genre-profit EDA figure
│   └── ...
├── Predictive_ROI_Modeling.ipynb     # 2026: Revenue regressor (R^2 = 0.53, ablated feature set)
├── NLP_Review_Sentiment_Analysis.ipynb # 2026: Critic sentiment classifier (AUC = 0.82)
├── data gathering.ipynb              # 2020: Scraping, API calls & data ingestion
├── Data_Cleaning_Exploration.ipynb   # 2020: Data wrangling & feature engineering
├── Visualization.ipynb               # 2020: EDA & the original genre/profit analysis
├── presentation.pdf                  # 2020: original slide deck
├── README.md                         # Project documentation
└── .gitignore                        # Git configuration
```

---

## 💻 How to Run & Reproduce

### Prerequisites
- Python 3.7+
- Jupyter Notebook or JupyterLab

### Installation
1. Clone this repository:
   ```bash
   git clone https://github.com/GitHub-ccd/Movie-Industry-Market-Analysis.git
   cd Movie-Industry-Market-Analysis
   ```

2. Install required Python packages:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn beautifulsoup4 requests tmdbsimple
   ```

3. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```

4. Open and run notebooks:
   - `Predictive_ROI_Modeling.ipynb` *(2026 — revenue regressor, fits the ablated/leakage-safe feature set)*
   - `NLP_Review_Sentiment_Analysis.ipynb` *(2026 — sentiment classifier)*
   - `Visualization.ipynb` *(2020 — original EDA)*

---

## 📄 License & Attribution
This project is open-source under the [MIT License](LICENSE.md).
Original 2020 analysis by **Chamila**, Flatiron Data Science bootcamp — IMDbPro scraping approach adapted from a classmate, **Jesse Numan**. 2026 rebuild as part of the Healthcare Data Scientist Portfolio suite ([`ccdportfolio`](https://github.com/GitHub-ccd/ccdportfolio)).

---

*The 2026 rebuild was agent-assisted: I set the direction and the methodology, an AI agent did much of the implementation and drafting, and I've kept the pre-rebuild baseline so the change is readable.*
