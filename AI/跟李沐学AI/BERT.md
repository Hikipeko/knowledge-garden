![[Pasted image 20230530160239.png]]

* Bidirectional Encoder Representations from Transformer.
* Pretrain deep bidirectional representations from unlabeled text by jointly conditioning on both left and right context in all layers. (Encoder of the Transformer)
* Trained by adding \[MASK\] to the original text.
* Can be fine-tuned with just one additional output layer to create models for a wide range of tasks, such as QA and language inference.