# Netflix Content Strategy & Audience Classification

**Applied Predictive Analytics: Course End Project**

Exploratory analysis and machine learning on the public Netflix catalog. The project visualizes how the catalog is built and has grown, then trains models to predict which audience a title is meant for: **Kids/Family**, **Teens** or **Adults**.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/<your-username>/<your-repo>/blob/main/Netflix_Content_Strategy.ipynb)

---

## Table of Contents

- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Methodology](#methodology)
- [Visualizations](#visualizations)
- [Predictive Models](#predictive-models)
- [How to Run](#how-to-run)
- [Tech Stack](#tech-stack)
- [Key Insights](#key-insights)
- [Limitations and Future Work](#limitations-and-future-work)
- [Author](#author)
- [Acknowledgements](#acknowledgements)

---

## Problem Statement

Netflix adds thousands of titles across many countries and languages. Knowing a title's audience fit drives parental controls, recommendation ranking and regional catalog decisions, yet reviewing metadata title by title does not scale.

**Core question:** *Can we predict a title's audience fit directly from its metadata?*

## Dataset

| Attribute | Detail |
|---|---|
| Name | Netflix Movies and TV Shows |
| Source | [Kaggle: shivamb/netflix-shows](https://www.kaggle.com/datasets/shivamb/netflix-shows) |
| File | `netflix_titles.csv` |
| Size | About 8,800 titles, 12 columns |
| Target | `audience_group` (3 classes), derived from the `rating` column |

**Columns:** `show_id`, `type`, `title`, `director`, `cast`, `country`, `date_added`, `release_year`, `rating`, `duration`, `listed_in`, `description`.

The notebook downloads the dataset automatically with `kagglehub`, so no manual download is needed.

## Project Structure

```
.
├── Netflix_Content_Strategy.ipynb          # Complete Colab notebook (EDA + models)
├── Netflix_Content_Strategy_Report.docx    # Full written project report
├── Netflix_Content_Strategy_Part1_v2.pptx  # Part 1 presentation
└── README.md
```

## Methodology

1. **Load** the dataset from Kaggle with `kagglehub`.
2. **Parse and clean**
   - Convert `date_added` to a date and extract `year_added`.
   - Split the mixed-unit `duration` column (minutes for movies, seasons for TV shows) into `duration_value`.
   - Take the first listed country as `primary_country`.
   - Count genre tags in `listed_in` as `num_genres`.
3. **Engineer the target label** by mapping `rating` to three audience groups:

   | Audience group | Ratings |
   |---|---|
   | Kids/Family | G, PG, TV-Y, TV-Y7, TV-Y7-FV, TV-G |
   | Teens | PG-13, TV-14, TV-PG |
   | Adults | R, TV-MA, NC-17 |

   Titles rated NR, UR or with malformed values are excluded.
4. **Visualize** the catalog (Part 1).
5. **Model** the audience group (Part 2).

## Visualizations

| # | Visualization | Question answered |
|---|---|---|
| 1 | Content type split (count and pie) | What share of the catalog is movies vs. TV shows? |
| 2 | Titles added per year (stacked area) | How has the catalog grown over time? |
| 3 | Top 10 genres (bar) | Which genres dominate? |
| 4 | Audience group distribution (bar) | How is content split across Kids, Teens and Adults? |
| 5 | Top 10 producing countries (bar) | Where does the content come from? |

## Predictive Models

**Task:** multi-class classification with three classes.

**Features:** `type`, `release_year`, `duration_value`, `num_genres`, and `primary_country` (top 15 countries one-hot encoded, the rest grouped as *Other*).

**Split:** 80% train and 20% test, stratified by class, `random_state=42`.

| Model | Configuration |
|---|---|
| Logistic Regression | `max_iter=2000` (baseline) |
| Random Forest Classifier | `n_estimators=300`, `max_depth=12` |

**Evaluation:** accuracy, macro precision, macro recall and macro F1 (macro averaging keeps the smaller Kids/Family class from being hidden), confusion matrices, and Random Forest feature importance.

Exact metric values are printed when you run the notebook.

## How to Run

### Google Colab (recommended)

1. Open [Google Colab](https://colab.research.google.com) and choose **File → Upload notebook**.
2. Upload `Netflix_Content_Strategy.ipynb`.
3. Choose **Runtime → Run all**. Sign in to Kaggle if prompted.
4. If the download fails, get `netflix_titles.csv` from the Kaggle page, upload it to Colab, and set `csv_path` to its filename.

### Locally

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

pip install pandas numpy matplotlib seaborn scikit-learn kagglehub jupyter
jupyter notebook Netflix_Content_Strategy.ipynb
```

## Tech Stack

- **Language:** Python 3
- **Environment:** Google Colab
- **Data:** pandas, NumPy, kagglehub
- **Visualization:** matplotlib, seaborn
- **Machine learning:** scikit-learn

## Key Insights

- The catalog is movie-heavy: roughly 70% movies and 30% TV shows.
- Additions grew rapidly from about 2015 to 2019, then slowed.
- International Movies, Dramas and Comedies are the leading genres.
- Adult-rated content is the largest audience group; Kids/Family is the smallest.
- The United States is the largest producer, followed by India and the United Kingdom.

## Limitations and Future Work

- The data has no viewership or revenue figures, so it describes the catalog rather than title success.
- The target comes from the official rating, so the model learns to mimic existing rating decisions.
- The dataset is a snapshot from around 2021.
- Genre is used only as a count; one-hot genre features and text features from `description` (such as TF-IDF) could improve predictions.
- Future work: feature scaling for Logistic Regression, a majority-class baseline, class weighting, cross-validation, hyperparameter tuning and gradient boosting models.

## Author

**Your Name**
Roll No. / Class: *your details*
[GitHub](https://github.com/<your-username>)

## Acknowledgements

- Dataset by [Shivam Bansal](https://www.kaggle.com/shivamb) on Kaggle.
- Built with pandas, seaborn, matplotlib and scikit-learn.
