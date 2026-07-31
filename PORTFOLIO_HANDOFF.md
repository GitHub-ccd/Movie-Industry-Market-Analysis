# Portfolio Handoff Payload: Microsoft Film Studio Market Intelligence

This document contains the standardized project payload for integrating the **Microsoft Film Studio: Box Office Market Intelligence** project into Chamila's Healthcare Data Scientist Portfolio website (`ccdportfolio`).

---

## 📦 JSON Payload for `ccdportfolio/src/data/projects.json`

Copy and paste the JSON object below into `ccdportfolio/src/data/projects.json`:

```json
{
  "id": "movie-industry-market-analysis",
  "title": "Microsoft Film Studio: Box Office Market Intelligence",
  "category": "Full-Stack & Personal Apps",
  "image": "/img/projects/movie-industry-market-analysis.png",
  "summary": "Data-driven executive consulting study evaluating box office revenues, net ROI, movie runtimes, ratings, and seasonal release windows to guide Microsoft's entry into original film production.",
  "description": "When Microsoft leadership evaluated entering the original video content market alongside industry peers, they required empirical market intelligence to navigate an unfamiliar industry. This project delivers an executive-level consultation analyzing box office financial dynamics across thousands of historical film titles.\n\nTo overcome severe missing-data limitations in baseline public datasets, an advanced multi-source ETL pipeline was built. This included a custom JavaScript DOM auto-scroller and BeautifulSoup HTML parsing engine to scrape over 14,000 IMDbPro records, paired with REST API integration targeting TMDB to ingest popularity indices and vote distributions across 26,500+ movies. Data was unified into a custom 'Binary Genre' taxonomy to reduce multi-label genre noise.\n\nKey strategic takeaways demonstrated that high-grossing action blockbusters carry high capital risk, while Animation/Adventure and Comedy/Romance deliver superior net profit margins and capital efficiency. Release window analysis highlighted critical revenue surges during Q2 Summer and Q4 Holiday release slots.",
  "tech": [
    "Python",
    "Pandas",
    "Matplotlib",
    "Seaborn",
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

`Python` • `Pandas` • `Matplotlib` • `Seaborn` • `BeautifulSoup (Web Scraping)` • `TMDB REST API` • `Jupyter Notebooks`

---

## 🎨 Suggested Prompt for 2026 AI Banner Image Generation

Use the prompt below with `generate_image` or Google Imagen 3 in the `ccdportfolio` session to generate `/img/projects/movie-industry-market-analysis.png`:

> **Prompt**:  
> *"A sleek, modern 3D data visualization banner for a movie industry analytics platform. Features floating futuristic holographic charts showing box office revenue growth, film genre pie charts, film reels, cinematic lighting in deep indigo and neon blue tones, and subtle Microsoft-inspired tech aesthetics. Professional 8k resolution, modern UI background, clean glassmorphism style, vibrant data graphs, vector style."*

---

## 🔗 Repository Reference
- **GitHub Repository**: [https://github.com/GitHub-ccd/Movie-Industry-Market-Analysis](https://github.com/GitHub-ccd/Movie-Industry-Market-Analysis)
- **Local Location**: `e:\My_GitHub__projects\Movie_Industry_Analysis_with_Python_MOD1`
