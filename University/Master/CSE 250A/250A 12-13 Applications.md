### L12-13 Latent Variable Models

* Alternative to bigram model: insert a hidden node $Z \in \{1, 2, \dots, C\}$ between the previous and next words $W, W' \in \{1, 2, \dots, V\}$.
* We have $P(w'|z)$ and $P(z|w)$.

![[Pasted image 20231202175428.png|500]]

![[Pasted image 20231202175600.png|450]]

##### Linear Interpolation of Markov Models

![[Pasted image 20231202204336.png|450]]

* $P_(w_l|w_{l-1}, w_{l-2}) = \lambda_1 P_1(w_l) + \lambda_2 P_2(w_l|w_{l-1})+\lambda_3 P_3(w_l|w_{l-1},w_{l-2})$
* Use corpus A to estimate $P_1, P_2, P_3$
* Use corpus B to estimate $\lambda_1, \lambda_2, \lambda_3$
* Use corpus C to evaluate the mixture model $P_{w''|w, w'}$

##### Noisy-OR Models

![[Pasted image 20231202204632.png|500]]

![[Pasted image 20231202204643.png|500]]

### L14 Hidden Markov Models

![[Unsupervised Learning 445#L25 HMM]]

