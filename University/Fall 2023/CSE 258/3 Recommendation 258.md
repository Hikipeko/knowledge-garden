### 1 Similarity-Base Recommender Systems

#### Collaborative Filtering

See [[Unsupervised Learning 445#L20 Collaborative Filtering]]

#### Measuring Similarity

1. Jaccard similarity $|A \cap B| / |A \cup B|$
2. Cosine, works for arbitrary vectors
3. Pearson correlation

#### Similarity-Based Rating Prediction

$$
r(u,i) = \frac{\sum_{j \in I_u \backslash\{i\}} r_{u,j} \cdot\text{sim}(i,j)}{\sum_{j \in I_u \backslash\{i\}} \text{sim}(i,j)}
$$

### Latent-Factor Models

* The Netflix Prize
* The previous approaches are unsupervised, how can we do supervised?
* Want f(user features, movie features) to star rating
* Should we use features or not?
	* Against: if the feature were useful, the model would "discover" a latent dimension corresponding to e.g. `isActionMovie`.
	* For: **cold start** latent-factor models are useless if the user or the item was never observed before

### Dimensionality Reduction for Binary Prediction

* The observations are binary
* Problem of regression: the model was penalized for not predicting zero
* **One-class recommendation**

### Recommender Systems Evaluation