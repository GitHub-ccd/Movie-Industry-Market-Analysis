# 📦 Best Practices for Managing Large Data Files in GitHub Repositories

When building Data Science and Machine Learning projects, storing large datasets (CSVs, TSVs, Parquet, Model Weights) directly inside a standard Git repository is considered bad practice. 

Git is designed for versioning plain text source code. Tracking large binary or CSV files bloats the `.git` packfile history, makes `git clone` painfully slow, and can trigger GitHub's **100 MB strict single-file limit** or **2 GB total repository size warning**.

Here are the **4 industry-standard architectures** used by data scientists to handle large data files efficiently.

---

## 🏆 Summary of Architectures

| Strategy | Best Used For | Repo Clone Size | Effort | Cost |
| :--- | :--- | :--- | :--- | :--- |
| **1. `.gitignore` + Automated Downloader** | Portfolios & Open-Source Repos | Extremely Small (< 5 MB) | Low | Free |
| **2. GitHub Release Assets** | Zipped Datasets & Model Checkpoints | Extremely Small (< 5 MB) | Low | Free |
| **3. Git LFS (Large File Storage)** | Teams needing Git-versioned binary data | Pointer-only until pull | Medium | Free tier (1GB) |
| **4. External Data Hubs (HF/Kaggle/S3)** | Public Benchmark Datasets & Big Data | Extremely Small (< 5 MB) | Medium | Free / Usage |

---

## 1. `.gitignore` + Sample Data Strategy *(Fastest & Simplest)*

Keep lightweight processed datasets (e.g., `movie_main.csv` < 2 MB) tracked in Git so the project runs out-of-the-box, while ignoring heavy raw raw datasets (e.g. 50 MB+ IMDb raw dumps).

### Implementation Steps
1. Add heavy raw patterns to `.gitignore`:
   ```gitignore
   # Ignore heavy raw dataset dumps
   Data/raw/
   Data/imdb.title.principals.csv
   Data/imdb.name.basics.csv
   Data/genres.csv
   *.zip
   *.tar.gz
   ```
2. Untrack previously committed large files from Git cache without deleting local files:
   ```bash
   git rm --cached Data/heavy_file.csv
   git commit -m "chore: untrack large raw data files from git history"
   ```

---

## 2. GitHub Release Assets *(Best for Reproducible Portfolios)*

Upload zipped raw data archives to a GitHub Release (e.g. `v1.0.0-data`), and provide a lightweight 10-line Python script (`scripts/download_data.py`) to fetch and extract the data automatically.

### Python Downloader Example (`scripts/download_data.py`)
```python
import os
import urllib.request
import zipfile

DATA_URL = "https://github.com/GitHub-ccd/Movie-Industry-Market-Analysis/releases/download/v1.0.0/data_raw.zip"
DATA_DIR = "Data"
ZIP_PATH = os.path.join(DATA_DIR, "data_raw.zip")

def fetch_data():
    if not os.path.exists(DATA_DIR):
        os.makedirs(DATA_DIR)
    
    if not os.path.exists(os.path.join(DATA_DIR, "imdb.title.principals.csv")):
        print("Downloading raw dataset from GitHub Releases...")
        urllib.request.urlretrieve(DATA_URL, ZIP_PATH)
        print("Extracting dataset...")
        with zipfile.ZipFile(ZIP_PATH, 'r') as zip_ref:
            zip_ref.extractall(DATA_DIR)
        os.remove(ZIP_PATH)
        print("Dataset ready!")
    else:
        print("Raw dataset already present locally.")

if __name__ == "__main__":
    fetch_data()
```

---

## 3. Git LFS (Large File Storage) *(Official GitHub Tool)*

GitHub's native extension for tracking large files. Replaces heavy files in the git tree with tiny text pointer files.

### Implementation Steps
1. Install Git LFS:
   ```bash
   git lfs install
   ```
2. Track specific large file patterns:
   ```bash
   git lfs track "Data/*.csv"
   git lfs track "*.parquet"
   ```
3. Commit `.gitattributes`:
   ```bash
   git add .gitattributes
   git commit -m "chore: configure Git LFS tracking for CSV files"
   ```

> ⚠️ **Note**: GitHub provides 1 GB of free Git LFS storage and monthly bandwidth per account.

---

## 4. External Data Hubs (Hugging Face / Kaggle / AWS S3 / Zenodo)

For datasets exceeding several gigabytes, host the data on an external free data hub and load it dynamically inside your Jupyter notebooks:

- **Hugging Face Datasets**: `from datasets import load_dataset`
- **Kaggle Datasets**: `import kagglehub; path = kagglehub.dataset_download(...)`
- **Zenodo / OSF**: Provides a permanent DOI for scientific publications.

---

## 🔒 Security Best Practice: Safeguarding API Credentials

Never commit API keys (TMDB, OpenAI, AWS keys) or credentials to GitHub.

### Recommended Pattern (`.env` + `os.environ`)
1. Create a `.env` file (and add `.env` to `.gitignore`):
   ```env
   TMDB_API_KEY=your_secret_api_key_here
   ```
2. Load keys safely in Python notebooks:
   ```python
   import os
   from dotenv import load_dotenv

   load_dotenv()
   api_key = os.environ.get("TMDB_API_KEY", "YOUR_TMDB_API_KEY_HERE")
   ```
