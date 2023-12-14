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


https://community.teamviewer.com/Chinese/kb/articles/5265-%E7%96%91%E4%BC%BC%E7%94%A8%E4%BA%8E%E5%95%86%E4%B8%9A%E7%94%A8%E9%80%94-%E6%A3%80%E6%B5%8B%E5%88%B0%E5%95%86%E4%B8%9A%E7%94%A8%E9%80%94-%E7%9A%84%E4%BF%A1%E6%81%AF