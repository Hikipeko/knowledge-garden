### 3 Model Architecture

![[Pasted image 20230519165900.png|400]]

##### 3.1 Encoder and Decoder

The **encoder** is a stack of $N=6$ identical layers. It maps an input sequence of symbol representation $(x_1, \dots, x_n)$ to a sequence of continuous vector representation $z = (z_1, \dots, z_n)$. Given $z$, the **decoder** then generates an output sequence $(y_1, \dots, y_m)$ one element at a time.

![[Pasted image 20230519170740.png|550]]

##### 3.2 Attention

Attention function's input is a query and a set of key-value pairs, and its output is a weighted sum of value depending on the similarity between the query and the keys. This paper uses **Scaled Dot-Product Attention**.
$$
\text{Attention}(Q,K,V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V
$$
**Multi-Head Attention** is similar to the idea of channels in CNN. It allows the model to jointly attend to information from different representation subspaces at different positions. In this paper, a 512-dimension vector is linearly projected to 8 64-dimension vectors, performing the attention function in parallel. The masked version is realized by setting all values to $-\infty$ in the input of the softmax which correspond to illegal connection.

##### 3.3 Point-wise Feed-Forward Networks

$$
\text{FFN}(x) = \max(0, xW_1 + b)W_2 + b_2
$$
$W_1: 512 \times 2048, W_2: 2048 \times 512$.

##### 3.4 Embedding

The embedding is learnable, and is shared between the two embedding layers.

##### 3.5 Position Encoding

In order to represent position and sequence information, a position encoding composed of $sin$ and $cos$ functions is added to the original embedding.

### 4 Why Self-Attention

![[Pasted image 20230519173839.png]]

### 5 Training

We trained on the standard WMT 2014 English-German dataset consisting of about 4.5 million sentence pairs.