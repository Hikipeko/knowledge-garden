### Abstract

This project explores and compares different coordinate descent methodologies against the conventional gradient descent approach, focusing on their efficiency in terms of convergence speed and final loss values. The experiment is conducted on a logistic regression task, utilizing a dataset for wine classification, and negative log-likelihood is used as loss function.

Two distinct strategies for selecting coordinates are implemented and compared. The first strategy Random Coordinate Descent selects a coordinate at random as a baseline approach. The second strategy Max Coordinate Descent selects the coordinate having largest gradient. This is under the hypothesis that prioritizing coordinates with larger gradients will lead to faster convergence.

The gradient update follows the traditional gradient descent methodology: for a selected coordinate, the gradient is calculated. Next, its weight is subtracted by the product of a fixed learning rate and the calculated gradient. This process requires the differentiability of the loss function. Nonetheless, it could be generalized to non-differentiable loss functions if sub-gradient can be calculated.

This is under the hypothesis that prioritizing coordinates with larger gradients will lead to faster convergence.

### Convergence:

Since Loss function is negative of log likelihood, the convergency of the log likelihood is good indication for stop. For gradient descent, convergence is presumed when five consecutive updates all decrease the loss by less than $\epsilon = 1e-4$. This criterion, however, does not directly translate to coordinate descent due to its singular coordinate updates per iteration. Intuitively, since 14 is the feature dimension, 14 iterations of coordinate descent has comparable change on gradient compared with gradient descent. Thus, I adjust the convergence check for coordinate descent to every 14 iterations, aligning the evaluation scale with gradient descent for fairness in comparison.

### Experimental Results

\\subsection{Methodology}

Since using scklearn packages have too much implementation details. The tricks involved can hurt comparison. Thus I implement all logistic regression myself. Avoid difference in implementation. Consistent for comparison. More rigorous.

I split data into 80% training and 20% testing. The data is then normalized to have mean of 0 and std of 1. In my implementation of logistic regression, the loss function is

$$\hat{y}_i = \frac{1}{1 + e^{-(w \cdot x_i + b)}}$$

$$L(w) = -\frac{1}{N} \sum_{i=1}^{N} [y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i)]$$

In each iteration of coordinate descent, $w_i$ is chosen as the coordinate to update. after the gradient is calculated, the new weight is updated as 

$$
w_i \gets w_i - \alpha \times \frac{\partial L(w)}{\partial w_i}
$$
.

Gradient descent, Random Coordinate Descent, Max Coordinate Descent are implemented.  Random Coordinate selects a coordinate at random. In comparison, Max Coordinate Descent selects the coordinate having largest gradient in terms of L2-norm. This is under the hypothesis that prioritizing coordinates with larger gradients will lead to faster convergence. When deriving $L^*$, to ensure optimality, a low tolerance $\epsilon = 1e-5$ is set. During the subsequent experiments, the same convergence criteria  $\epsilon = 1e-4$ is set for all of three methods

\subsection{Result Analysis}

After the same convergence criteria is set, each method is run, and how their value of loss function changes with iteration is plotted in figure 1 for comparison. The iteration number and final value of loss function is displayed in table 1. Note that the iteration of standard gradient descent is scaled to be able to compare to coordinate descent. As discussed above, 14 iterations of coordinate descent has comparable change on gradient compared with gradient descent. Each iteration of gradient descent is scaled as 14 iteration for coordinate descent.

From figure 1, the loss curve of gradient descent and Random Coordinate Descent is overlapping, while Random Coordinate Descent stops earlier. This indicates that Random Coordinate Descent have similar behaviour as gradient descent. This might because in both methods, the expectation of one coordinate being descent is the same. When number of iteration is large, their behavior more and more similar to each other. The Max Coordinate Descent decent faster to $L^*$ than the previous two methods. This might be a good approach to do coordinate descent.

| Method                    | Iterations to Converge | Final Loss Value |
| ------------------------- | ---------------------- | ---------------- |
| Gradient Descent Optimal  | -                      | 0.115            |
| Gradient Descent          | 165830 (11845)         | 0.348            |
| Random Coordinate Descent | 33121                  | 1.293            |
| Max Coordinate Descent    | 115907                 | 0.235            |

From table 2, we find although Random Coordinate Descent is fastest to converge, it has worst final loss value,10 times  more than $L^*$, and 3 times more than the other two methods. The Max Coordinate Descent is better than gradient descent method as it requires 30.1% less iterations to converge, while has significant 32.5% less final loss value. In conclusion, Max Coordinate Descent is the best approach in terms of iterations to converge and final loss value.

Besides comparing the final loss value after convergence, It's worthy to compare loss after same number of iteration. Setting the iteration to be 100,000 Result is in table 2. We see that after same number of iteration, Random Coordinate Descent has almost same loss as gradient descent. While Max Coordinate Descent is 49.7% better than the previous methods. This again indicates that Max coordinate descent is the best method among three.

| Method                    | Final Loss Value |
| ------------------------- | ---------------- |
| Gradient Descent          | 0.531            |
| Random Coordinate Descent | 0.532            |
| Max Coordinate Descent    | 0.267            |

Critical evaluation

I think a first possible improvement is to speed up Max Coordinate Descent algorithm. It calculate all the gradient in each iteration, make it slow. Some possibilities are only evaluating part of coordinate, and choose the best one to update. Another possibility is to use reinforcement learning. For example, bandit can be used to learn a good strategy finding good coordinates. Promising, however, need to justify the additional computational resourced is worthy of the cost.
Another improvement is to try different optimization methods besides gradient descent. E.g. Newton's method, cross-entropy, conjugate gradient can be tested.
