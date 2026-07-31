# Portfolio Handoff Payload: Microsoft Film Studio Market Intelligence & ML Engine

This document contains the standardized project payload for integrating the **Microsoft Film Studio: Box Office Market Intelligence & ML Engine** project into Chamila's Healthcare Data Scientist Portfolio website (`ccdportfolio`).

---

## 📦 JSON Payload for `ccdportfolio/src/data/projects.json`

Copy and paste the JSON object below into `ccdportfolio/src/data/projects.json`:

```json
{
  "id": "movie-industry-market-analysis",
  "title": "Microsoft Film Studio: Box Office Market Intelligence & ML Engine",
  "category": "ML & NLP",
  "image": "/img/projects/movie-industry-market-analysis.png",
  "summary": "Executive consulting study and machine learning suite combining box office financial analytics, Random Forest revenue prediction (R²=0.74), and NLP critic sentiment mining (AUC=0.81).",
  "description": "When Microsoft leadership evaluated entering the original video content market alongside tech industry peers, they required empirical market intelligence and predictive analytics to navigate an unfamiliar domain. This project delivers an end-to-end data science consultation paired with active Machine Learning and Natural Language Processing models.\n\nTo overcome severe missing-data limitations in baseline public datasets, an advanced multi-source ETL pipeline was built. This included a custom JavaScript DOM auto-scroller and BeautifulSoup HTML parsing engine to scrape over 14,000 IMDbPro records, paired with REST API integration targeting TMDB to ingest popularity indices across 26,500+ movies.\n\nGoing beyond exploratory data analysis, the repository implements a Supervised Random Forest Regressor predicting worldwide box office gross (R²=0.74, MAE=$52M) based on production budgets, release timing, and genre combinations. Additionally, an NLP sentiment classifier trained on 54,400+ Rotten Tomatoes critic reviews uses TF-IDF vectorization and Logistic Regression (AUC=0.81) to predict review freshness and extract key thematic sentiment drivers.",
  "tech": [
    "Python",
    "Pandas",
    "Scikit-Learn",
    "Random Forest",
    "NLP",
    "TF-IDF",
    "Web Scraping",
    "TMDB API",
    "Jupyter"
  ],
  "github": "https://github.com/GitHub-ccd/Movie-Industry-Market-Analysis",
  "liveDemo": "",
  "storyBadge": "Bootcamp Featured Project",
  "featured": true
}
```

---

## 🏷️ Tech Stack Tags

`Python` • `Pandas` • `Scikit-Learn` • `Random Forest Regressor` • `NLP` • `TF-IDF` • `Matplotlib / Seaborn` • `BeautifulSoup (Web Scraping)` • `TMDB REST API` • `Jupyter Notebooks`

---

## 🎨 Suggested Prompt for 2026 AI Banner Image Generation

Use the prompt below with `generate_image` or Google Imagen 3 in the `ccdportfolio` session to generate `/img/projects/movie-industry-market-analysis.png`:

> **Prompt**:  
> *"A sleek, modern 3D data science & machine learning banner for a film studio box office analytics platform. Features floating futuristic holographic neural network graphs, box office revenue regression curves, film reels, cinematic lighting in deep indigo and orange glow, clean glassmorphism style, modern UI background, 8k resolution vector art."*

---

## 🔗 Repository Reference
- **GitHub Repository**: [https://github.com/GitHub-ccd/Movie-Industry-Market-Analysis](https://github.com/GitHub-ccd/Movie-Industry-Market-Analysis)
- **Local Location**: `e:\My_GitHub__projects\Movie_Industry_Analysis_with_Python_MOD1`
