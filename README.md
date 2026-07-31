# 🎬 Microsoft Film Studio: Movie Industry Market Analysis

[![Python](https://img.shields.io/badge/Python-3.7+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data_Wrangling-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c?style=for-the-badge)](https://matplotlib.org/)
[![TMDB API](https://img.shields.io/badge/TMDB_API-Data_Enrichment-01b4e4?style=for-the-badge&logo=themoviedb&logoColor=white)](https://www.themoviedb.org/)
[![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup-Web_Scraping-336699?style=for-the-badge)](https://www.crummy.com/software/BeautifulSoup/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Analytics_Notebooks-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org/)

An executive data science consultation and strategic market intelligence study evaluating box office revenues, net return on investment (ROI), runtime sweet-spots, genre combinations, and seasonal release windows to guide Microsoft's capital allocation into original film production.

---

## 📌 Executive Summary & Business Context

Microsoft sees tech industry peers aggressively expanding into original film and video content. To launch its own content creation studio successfully without prior film production experience, Microsoft's leadership required empirical, data-driven market intelligence on box office dynamics.

This project answers 4 primary strategic questions posed by executive decision-makers:
1. **Genre Capital Allocation**: Which film genres yield the highest gross earnings, customer ratings, and net profit margins?
2. **Runtime Sweet-Spots**: What are the optimal film durations per genre, and how does runtime impact box office revenue?
3. **Seasonal Release Windows**: What is the most lucrative month and season to release new titles?
4. **Genre Synergy ("Binary Genres")**: Which multi-genre pairings maximize return on investment (ROI)?

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      DATA ENGINE & CONSULTING PIPELINE                  │
└─────────────────────────────────────────────────────────────────────────┘
   Multi-Source Data Ingestion   ──►   Web Scraping & API Enrichment
  (IMDb, Box Office Mojo, RT)        (IMDbPro DOM Scraper + TMDB API)
                │                                    │
                ▼                                    ▼
   Schema Unification & Cleansing ──►   Binary Genre Feature Engineering
                │                                    │
                ▼                                    ▼
   Exploratory Market Intelligence ──► Executive Strategy Presentation
```

---

## 🛠️ Data Engineering & Multi-Source ETL Pipeline

Baseline datasets provided for box office analysis contained severe missing data gaps and sparse coverage. To overcome these limitations, an end-to-end multi-source data ingestion and enrichment pipeline was engineered:

### Data Sources Ingested & Merged
| Source | Format | Records Ingested | Primary Features Used |
| :--- | :--- | :--- | :--- |
| **IMDb Data Dumps** | CSV / TSV | 5+ Tables | Title basics, ratings, crew, runtime, principals |
| **TheNumbers (`tn.movie_budgets.csv`)** | CSV | 5,782 | Production budget, domestic gross, worldwide gross |
| **Box Office Mojo (`bom.movie_gross.csv`)** | CSV | 3,387 | Annual box office gross, studio |
| **Rotten Tomatoes (`rt.movie_info.tsv`)** | TSV | 1,560 | Audience/critic ratings, fresh status |
| **TMDB API Export (`tmdb.movies.csv`)** | CSV / REST API | 26,519 | Popularity indices, vote counts, release dates, TMDB genre codes |
| **Custom IMDbPro Scraping** | Web / HTML | 14,428 | Consolidated budget, gross, region codes, ratings |

### Key Engineering Innovations
- **IMDbPro DOM Scraping Engine**: Designed a custom JavaScript console auto-scroller combined with BeautifulSoup HTML parsing to bypass login lazy-loading limits, expanding viable datapoints from a few hundred to over 14,000 complete records.
- **TMDB REST API Integration**: Integrated `tmdbsimple` framework and TMDB REST API endpoints to capture worldwide popularity scores and vote counts across 26,500+ titles.
- **"Binary Genre" Taxonomy**: Solved multi-label genre noise by engineering a composite *Binary Genre* feature (mapping the primary two core genres of a film), enabling granular statistical comparison across genre intersections.

---

## 📊 Strategic Market Insights & Findings

### 1. High-ROI Genre Selection: Revenue vs. Net Profit
While high-budget Action/Adventure titles generate massive worldwide gross revenue, **Animation, Adventure, Sci-Fi, and Comedy** consistently demonstrate the highest net profit margins and capital efficiency.

| Genre Combination | Average Net Profit | Box Office Category |
| :--- | :--- | :--- |
| **Animation + Adventure** | **$310M+** | High-Yield Blockbuster |
| **Action + Adventure** | **$260M+** | High-Gross / High-Capital |
| **Drama + Romance** | **$65M+** | Mid-Budget Steady ROI |
| **Comedy + Romance** | **$55M+** | Low-Risk / High ROI |

<div align="center">
  <img src="images/profit_genres2.jpeg" alt="Net Profit by Genre" width="85%" />
  <p><em>Figure 1: Net Profit comparison across top Binary Genres.</em></p>
</div>

<div align="center">
  <img src="images/profit_genres2_pie.jpeg" alt="Profit Share Pie Chart" width="70%" />
  <p><em>Figure 2: Distribution of total industry net profit across major genre categories.</em></p>
</div>

---

### 2. Customer Ratings & Quality Distribution
IMDb ratings across the industry follow a normal distribution centered around **6.4 / 10**. High ratings (> 7.5) correlate positively with production budget, but top-rated genres (e.g., Biography, Documentary, Drama) often require lower production budgets than heavy Sci-Fi/VFX blockbusters.

<div align="center">
  <img src="images/rating_whisker.jpeg" alt="IMDb Rating Whisker Plot" width="80%" />
  <p><em>Figure 3: IMDb Rating box-whisker distribution.</em></p>
</div>

---

### 3. Release Window Optimization
Box office earnings exhibit severe seasonal spikes:
- **Summer Window (May – July)**: Peak gross earnings for Action, Adventure, and Animation blockbusters.
- **Winter / Holiday Window (November – December)**: Highest average per-screen gross for Family and Holiday Animation releases.
- **Spring / Autumn Windows**: Ideal for low-budget Horror, Thriller, and Mid-Budget Drama releases.

---

## 📁 Repository Structure & Notebook Workflow

```
Movie-Industry-Market-Analysis/
├── Data/                             # Raw and processed datasets (IMDb, TMDB, Mojo, TheNumbers)
│   ├── tmdb.movies.csv               # TMDB API enriched dataset (26,519 rows)
│   ├── tn.movie_budgets.csv          # Financial budgets & worldwide gross
│   ├── movie_main.csv                # Master clean analytical dataset
│   └── ...                           # IMDb and Rotten Tomatoes tables
├── images/                           # Analytical chart outputs & figures
│   ├── profit_genres2.jpeg
│   ├── profit_genres2_pie.jpeg
│   ├── rating_whisker.jpeg
│   └── ...
├── sites/                            # External API frameworks & site documentation
│   └── movies_project_site_info.ipynb
├── data gathering.ipynb              # ETL Notebook: Scraping, API calls & data ingestion
├── Data_Cleaning_Exploration.ipynb   # Data Wrangling Notebook: Cleaning & feature engineering
├── Visualization.ipynb               # EDA Notebook: Statistical analysis & visualizations
├── presentation.pdf                  # 10-slide Executive Slide Deck for Leadership
├── README.md                         # Project documentation
└── .gitignore                        # Git configuration
```

### Analytical Execution Sequence
1. [`data gathering.ipynb`](file:///e:/My_GitHub__projects/Movie_Industry_Analysis_with_Python_MOD1/data%20gathering.ipynb): Runs web scraping routines (IMDbPro) and TMDB API data pulls.
2. [`Data_Cleaning_Exploration.ipynb`](file:///e:/My_GitHub__projects/Movie_Industry_Analysis_with_Python_MOD1/Data_Cleaning_Exploration.ipynb): Cleans raw missing values, formats currency strings, calculates net profits, and builds binary genres.
3. [`Visualization.ipynb`](file:///e:/My_GitHub__projects/Movie_Industry_Analysis_with_Python_MOD1/Visualization.ipynb): Generates distribution plots, scatter matrices, box plots, and seasonal trend lines.
4. [`presentation.pdf`](file:///e:/My_GitHub__projects/Movie_Industry_Analysis_with_Python_MOD1/presentation.pdf): Non-technical presentation summarizing strategic recommendations for Microsoft executives.

---

## 🚀 Advanced Analytics & Predictive Modeling Roadmap

To extend this market analysis beyond exploratory data analysis into production machine learning:

1. **Predictive Box Office Revenue Model**:
   - Train **XGBoost / LightGBM Regression** models on budget, runtime, release month, director track record, and binary genres to forecast opening weekend & total gross returns prior to greenlighting scripts.
2. **NLP Critic Sentiment Mining**:
   - Extract text reviews from Rotten Tomatoes and IMDb user reviews using `nltk` / `transformers` to perform Aspect-Based Sentiment Analysis (ABSA) on plot pacing, character development, and visual effects.

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
   pip install pandas numpy matplotlib seaborn beautifulsoup4 requests tmdbsimple
   ```

3. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```

4. Open and run the notebooks in sequence:
   - `data gathering.ipynb`
   - `Data_Cleaning_Exploration.ipynb`
   - `Visualization.ipynb`

---

## 📄 License & Attribution
This project is open-source under the [MIT License](LICENSE.md).  
Designed & Developed by **Chamila** as part of the Healthcare Data Scientist Portfolio suite ([`ccdportfolio`](https://github.com/GitHub-ccd/ccdportfolio)).
