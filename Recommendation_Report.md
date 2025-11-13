# Movie Recommendation System Report

### 1. Introduction

Movie Recommendation systems are a critical component in modern streaming platforms like Amazon Prime, Netflix and YouTube. They help users discover relevant content by analyzing past viewing habits and predicting what the user might also enjoy next. These systems improve engagement and retention by personalizing the experience rather than presenting a static catalog.

This project covers three approaches:
- **User-based Collaborative Filtering**: Identifies users with similar preferences and recommends movies those similar users rated highly.
- **Item-based Collaborative Filtering**: Finds movies similar to those a user has already liked by comparing rating patterns.
- **Pixie Inspired Random Walk**: Models user–movie relationships as a bipartite graph and uses random walks to discover movies connected through the graph structure.

---

### 2. Dataset Description:

This project uses the MovieLens 100K dataset, a standard benchmark for recommendation systems.

| Property | Description |
|--------|--------------|
| Users |943 |
| Movies | 1682 |
| Ratings: 100,000 (scale of 1-5)|

**Preprocessing Steps**:
- Read raw files (DATA/u.data, DATA/u.item, DATA/u.user), assign column names, verify dtypes.
- Convert timestamp to datetime for inspection/optional manipulation later on
- Replaced missing ratings with `0`
- Normalized ratings per user for the random-walk graph
- Built a `user_movie_matrix` with users as rows and movies as columns.

---

### 3. Methodology 

This project implements three distinct recommendation techniques

**3.1 User-Based Collaborative Filtering**
The user-based approach focuses on identifying similar users based on their historical movie ratings. A user–movie matrix is constructed where each row represents a user and each column represents a movie. Using cosine similarity, the system measures how closely two users’ rating patterns align, users with similar ratings for a movie are considered "neighbors". Once user similarity scores are computed, the algorithm aggregates the ratings from the most similar users to estimate how much the target user might like unrated movies. Movies that receive the highest average ratings from these similar users are recommended. this method is effective in recommending movies to users with comparable tastes.

**3.2 Item-Based Collaborative Filtering (IBCF)**
In the item-based approach, the focus shifts from comparing users to comparing movies themselves. The same user–movie matrix is transposed so that each row now represents a movie and each column represents a user. Cosine similarity is then applied to calculate how closely movies are related based on the ratings they received from common users. For a given movie, the system identifies the top most similar movies (those with highly correlated rating patterns) and recommends them to users who liked or rated the original movie. Item-based collaborative filtering tends to perform better in large, sparse datasets.

**3.3 Pixie-Inspired Random Walk (Graph-Based)**
The Pixie-inspired algorithm models the dataset as a bipartite graph consisting of user nodes and movie nodes connected by edges representing ratings. Instead of directly comparing similarities, this method simulates random walks across the graph to discover relevant movies. A random walk starts from a target user node and repeatedly transitions between connected users and movies. The number of times a movie node is visited reflects its relevance to the starting user (the more often it’s encountered, the stronger the connection). This approach captures indirect relationships between users and items. Graph-based techniques like Pixie are highly scalable and have been successfully applied by large-scale recommendation platforms such as Pinterest where real-time personalization and exploration of graph connections yield highly diverse and contextually relevant recommendations.

---

### 4. Implementation Details

The implementation was carried out in a Jupyter Notebook using Python, with libraries such as `pandas`, `numpy`, and `scikit-learn` (used for computing cosine similarity). The environment was containerized using Docker to ensure consistent dependencies and reproducibility across systems. The project included a `Dockerfile`, `docker-compose.yml`, and a `sample.env` configuration file to manage environment variables, dataset paths, and notebook execution. Each recommendation approach was implemented as a self-contained function, following structured steps given.

**4.1 User-Based Collaborative Filtering:**
- The process began by constructing a user–movie rating matrix from the dataset.
- Missing values were replaced with zeros for similarity computation.
- Using cosine_similarity from `scikit-learn`, a user–user similarity matrix was generated.
- For any given user, the algorithm retrieved the similarity scores, sorted them in descending order, and computed an average rating for unrated movies based on ratings from the most similar users.
- The top N movies with the highest average predicted ratings were recommended.

**4.2 Item-Based Collaborative Filtering:**
- The same rating matrix was transposed to make movies the rows and users the columns.
- After filling missing values with zeros, pairwise cosine similarity between movies was computed.
- Given a target movie, its similarity scores to all other movies were sorted in descending order.
- The top N most similar movies were then returned as recommendations.

**4.3.1 Graph Construction for Pixie Algorithm:**
- A bipartite adjacency list was created from the merged dataset of users, movies, and ratings.
- Each user node was connected to the movies they had rated, and each movie node linked back to its raters.
- The resulting graph structure allowed for bidirectional traversal between users and movies.

**4.3.2 Random Walk Simulation:**
- Starting from a specific user node, a random walk was simulated for a defined number of steps (walk_length).
- At each step, a neighboring node was randomly chosen, alternating between users and movies.
- If a movie node was visited, it was recorded in a visit count dictionary.
- After completing the walk, movies were ranked by visit frequency, and the top N were returned as the user’s recommendations.
- The function outputs a clean Pandas DataFrame with Ranking and Movie Name columns for easy interpretation.

---

### 5. Results and Evaluation

**User-Based Collaborative Filtering**

| Ranking | Movie Name |
|----------|-------------|
| 1 | Little City (1998) |
| 2 | Santa with Muscles (1996) |
| 3 | Prefontaine (1997) |
| 4 | They Made Me a Criminal (1939) |
| 5 | Marlene Dietrich: Shadow and Light (1996) |


**Item-Based Collaborative Filtering**

| Ranking | Movie Name |
|----------|-------------|
| 1 | Top Gun (1986) |
| 2 | Speed (1994) |
| 3 | Raiders of the Lost Ark (1981) |
| 4 | Empire Strikes Back, The (1980) |
| 5 | Indiana Jones and the Last Crusade (1989) |


**Pixie Random Walk-Based Recommendation**

| Ranking | Movie Name |
|----------|-------------|
| 1 | Devil in a Blue Dress (1995) |
| 2 | Rebel Without a Cause (1955) |
| 3 | Lost Highway (1997) |
| 4 | For Whom the Bell Tolls (1943) |
| 5 | Crash (1996) |

**Evaluation**

- The User-Based Collaborative Filtering approach effectively identified similar users based on their rating patterns. It successfully produced personalized recommendations by leveraging shared viewing behavior among users. However, its performance heavily depends on having a sufficient number of overlapping ratings
- The Item-Based Collaborative Filtering model generated strong, thematically consistent recommendations. It identified movies with similar audience reception and rating trends, resulting in stable, genre-consistent suggestions. This approach also proved more scalable, as item relationships are less volatile than individual user preferences.
- The Pixie Random Walk Algorithm produced more diverse and exploratory recommendations by traversing indirect connections within the user–movie graph. Instead of relying on direct similarity, the method leveraged the graph structure to uncover hidden relationships.

**Limitations**

- Sparse rating data can reduce the reliability of similarity-based methods.
- The Pixie algorithm’s stochastic nature can lead to varying results between runs unless averaged over multiple walks.


### 6. Conclusion
As previously mentioned this project implemented three fundamental approaches to movie recommendation (User-Based Collaborative Filtering, Item-Based Collaborative Filtering, and the Pixie-Inspired Random Walk Algorithm) using the MovieLens 100K dataset. Each method demonstrated unique advantages and limitations in balancing personalization, scalability, and recommendation diversity.

User-Based Filtering provided personalized recommendations but relied heavily on shared rating overlap. Item-Based Filtering delivered consistent, thematically aligned suggestions by focusing on relationships between movies. The Pixie Algorithm offered a graph-based approach, uncovering indirect user–movie connections and generating more diverse results.

Future improvements include adding weighted edges, exploring hybrid methods, and applying evaluation metrics to measure performance. Overall, graph-based algorithms like Pixie highlight the growing importance of scalable, real-time personalization in modern recommendation systems such as Pinterest, Netflix, and YouTube.