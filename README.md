# Movie Recommendation System

A comprehensive Jupyter Notebook implementing:
1. User-Based Collaborative Filtering
2. Item-Based Collaborative Filtering
3. Random Walk (Pixie-Inspired Graph Algorithm)

All wrapped in a reproducible environment using **Docker** and **docker-compose**.

---

## Overview

This project builds a complete movie recommendation system using the **MovieLens 100K dataset**.  
It demonstrates multiple ways to compute user and movie similarities and explores a graph-based random walk for dynamic recommendations.

---

## Features

- [x] User-based collaborative filtering using cosine similarity
- [x] Item-based collaborative filtering between movies
- [x] Weighted Pixie random walk using bipartite graphs
- [x] Report on the Recommendations
- [x] Explanation of Pixie Algorithm
- [x] Fully documented notebook

---

## Dataset

The **MovieLens 100K** dataset containing:
- Users: 943
- Movies: 1682
- Ratings: 100,000 (scale of 1-5)

### Files Used
users.csv
movie.csv
ratings.csv

---

## Data Preprocessing
- Missing ratings handled with either 0-fill method.  
- Merged datasets to include movie titles.  
- Constructed `user_movie_matrix` using `pivot()` in Pandas.

---

## Methods Implemented

| Method | Description |
|--------|--------------|
| `recommend_movies_for_user(user_id, num=5)` | Recommends movies based on similar users |
| `recommend_movies(movie_name, num=5)` | Suggests movies similar to a given movie |
| `weighted_pixie_recommend(user_id, walk_length=15, num=5)` | Performs a random walk on the user–movie graph to recommend items |

---

## Algorithm Summaries
1. User-Based Collaborative Filtering
- Builds cosine similarity between users.
- Predicts unseen movie ratings using neighbors’ weighted averages.
- Excludes titles already rated by the target user.

2. Item-Based Collaborative Filtering
- Transposes the user–movie matrix to compare movies directly.
- Computes pairwise cosine similarity.
- Returns movies most similar to the given title.

3. Pixie-Inspired Random Walk
- Constructs a bipartite graph (users ↔ movies).
- Starts from a selected user node and walks randomly across connected nodes.
- Tracks visit frequency; highly visited movies are recommended.
- Inspired by Pinterest’s Pixie graph-based recommender architecture.

---

## Running the Notebook

You can run this project either using Docker (recommended) for an isolated, reproducible setup, or locally in your own Python environment.

### Option 1: Run with Docker and docker-compose (Recommended)
The repository includes both a Dockerfile and docker-compose.yml, so you can start a fully configured Jupyter Notebook container instantly.
1. Clone the repository:
```bash
git clone https://github.com/Sm3ggl3s/Movie-Recommendation.git
cd Movie-Recommendation
```
2. Configure your environment variables

A .env file is required for Docker Compose.
A template has already been provided as sample.env.

Create a copy of it:
```bash
cp sample.env .env
```
Then open .env in any editor and adjust the paths if needed.

3. Build and start the container:
```bash
docker compose up --build
```
4. Once the container starts, you’ll see a URL like:
```bash
http://127.0.0.1:8888/?token=...
```
Copy this URL into your browser to open Jupyter Notebook.
5. Inside Jupyter, open the notebook:
```bash
Programming_Assignment-1.ipynb
```

### Option 2: Run Locally (Manual Setup)
If you prefer running directly on your machine without Docker:
1. Create a Python virtual environment:
```bash
python -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate
```

2. Install required dependencies:
```bash
pip install -r requirements.txt
```

3. Start Jupyter Notebook:
```bash
jupyter notebook
```

---

## Reports & Documentation
Included per submission requirements:
- `Programming_Assignment-1.ipynb` – Full code + markdown explanations
- `Pixie_Algorithm_Explanation.pdf` – 3–5 paragraph write-up of Pixie method
- `Recommendation_Report.pdf` – Project report (Introduction, Dataset, Methodology, Results)
- CSV exports (`users.csv`, `movies.csv`, `ratings.csv`)

---

## Submission Checklist

- [x] All three recommendation algorithms implemented
- [x] Code explained with markdown cells
- [x] Report and Pixie explanation included
- [x] CSV exports saved
- [x] Repository public and linked

---

<p align="center">
  <strong>Author:</strong> James Anderson<br>
  <em>University of North Carolina at Charlotte</em><br>
  <em>Master of Science in Computer Science (Fall 2025)</em>
</p>

---