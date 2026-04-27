# Computer Science Journal Finder

**Data Mining Final Project — Akdeniz University**

An automated journal recommendation system that, given the abstract of an unpublished manuscript, identifies and ranks the **top-5 most relevant Computer Science journals** from a corpus of 455 Web of Science journals and 23,061 articles.

---

## Overview

| | |
|---|---|
| **Dataset** | 23,061 CS articles from 455 Web of Science journals (2000–2018) |
| **Retrieval** | TF-IDF + Cosine Similarity / BM25 |
| **Clustering** | K-Means (K=8), Hierarchical (Ward), DBSCAN |
| **Topic Modelling** | LDA (8 topics) |
| **Evaluation** | Precision@5, Recall@5, NDCG@5, MRR — 8 ground-truth queries |

---

## Project Structure

```
journal_finder/
├── journal_finder.ipynb        # Main notebook (54 cells, fully executed)
├── data/
│   └── CompSciencePub.sqlite   # SQLite database (Web of Science)
├── report/
│   ├── Journal_finder.pdf      # IEEE Conference format report
│   └── Journal_finder.tex      # LaTeX source
├── requirements.txt
└── README.md
```

---

## Notebook Contents

| Section | Description |
|---|---|
| **1** | Library imports |
| **2** | Database connection & data loading |
| **3** | Text preprocessing (HTML cleaning, stopword removal, stemming) |
| **4** | TF-IDF vectorization (10,000 features, unigram+bigram) |
| **5** | Journal Finder — cosine similarity, 3 test abstracts |
| **6** | K-Means topic clustering (Elbow method, K=8, 2D visualization) |
| **7** | LDA topic modelling (8 topics, word-topic heatmap) |
| **8** | Journal–topic relationship analysis |
| **9** | Summary & interactive demo |
| **10** | Algorithm comparison: TF-IDF vs BM25 / K-Means vs Hierarchical vs DBSCAN |

---

## Key Results

### Journal Recommendation (TF-IDF)

| Test | Domain | Top Journal | Score |
|---|---|---|---|
| 1 | Deep Learning | Neurocomputing | 0.2418 |
| 2 | Network Security | Automated Software Engineering | 0.2249 |
| 3 | Distributed Databases | VLDB Journal | 0.2155 |
| 4 | Big Data *(ground truth)* | IEEE Comp. Intelligence Mag. | **Rank 3** ✓ |

### TF-IDF vs BM25 (8 ground-truth queries)

| Metric | TF-IDF | BM25 |
|---|---|---|
| Precision@5 | **0.650** | 0.475 |
| NDCG@5 | **0.646** | 0.537 |
| MRR | 0.781 | **0.833** |

### Clustering Comparison (K=8)

| Algorithm | Silhouette ↑ | Davies-Bouldin ↓ | Calinski-Harabasz ↑ |
|---|---|---|---|
| **K-Means** | **0.069** | 3.070 | **719.7** |
| Hierarchical (Ward) | 0.040 | 3.627 | 535.7 |
| DBSCAN | −0.230 | 1.652 | 5.5 |

---

## Setup & Usage

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

> The notebook uses its own virtual environment (`.venv`). To use it:
> ```bash
> python -m venv .venv
> source .venv/bin/activate   # macOS/Linux
> pip install -r requirements.txt
> ```

### 2. Run the notebook

Open `journal_finder.ipynb` in Jupyter and run all cells:

```bash
jupyter notebook journal_finder.ipynb
```

Or execute headlessly:

```bash
jupyter nbconvert --to notebook --execute --inplace journal_finder.ipynb
```

### 3. Try your own abstract

In **Section 9** (interactive demo cell), replace the `MY_ABSTRACT` variable:

```python
MY_ABSTRACT = """
Your abstract text here...
"""
```

---

## Methodology

### TF-IDF + Cosine Similarity

Each journal is represented as the mean TF-IDF vector of its articles (centroid). A query abstract is vectorized and ranked by cosine similarity against all 455 journal centroids.

### BM25

Journals are treated as concatenated pseudo-documents. BM25 scoring uses k₁=1.5 and b=0.75 (standard IR defaults).

### K-Means Clustering

Raw TF-IDF matrix (23,061 × 10,000) is reduced to 50 dimensions via Truncated SVD (LSA), L2-normalized, then clustered with K-Means. Optimal K=8 selected via the Elbow Method.

### LDA

Raw term-frequency vectors (5,000 features) are used as input to Latent Dirichlet Allocation with 8 topics, matching the K-Means configuration for comparability.

---

## Report

The full IEEE Conference format report is available at `report/Journal_finder.pdf`.

---

## Dependencies

| Package | Purpose |
|---|---|
| `pandas`, `numpy` | Data manipulation |
| `scikit-learn` | TF-IDF, K-Means, SVD, cosine similarity |
| `nltk` | Stopword removal, stemming |
| `gensim` | LDA topic modelling |
| `rank-bm25` | BM25 retrieval |
| `matplotlib`, `seaborn`, `wordcloud` | Visualization |
