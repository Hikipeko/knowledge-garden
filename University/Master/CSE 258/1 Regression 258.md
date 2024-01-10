See [[Supervised Learning 445]].

### Feature Transforms

* How do preferences toward certain beers vary with age?
* ABV?
* Gender (discrete)?
* Month?

### Categorical Features

* One hot encoding
* Temporal features e.g. month

### Regression Diagnostics

* Mean-square error (MSE) $\frac1N \lVert y - X\theta \rVert_2^2$
* $FVU(f) = MSE(f) / Var(y)$, 1 is trivial and 0 is perfect.
* $R^2 = 1-FVU(f)$

### Overfitting

* See [[Traditional Methods 498#Overfitting]].
* See [[Supervised Learning 445#L4 Gradient Descent]]

### Model Selection and Summary

* Validation set is used to tune the model.