What other models did you consider for comparison?
What were your unsuccessful attempts along the way?
What are the strengths and weaknesses of the different models being compared?

### 3 Rating Prediction with BERT

In this section, we present two models using BERT for rating prediction, using text review as feature. We try this model because BERT is a powerful language model. It can be fine-tuned with just one additional output layer to create excellent models for a wide range of tasks, such as question answering and classification, without substantial task specific architecture modifications. We try two approaches of using BERT. Our first model is to directly use BERT embedding for linear regression without any fine-tuning. The second model is stack a linear regression layer on top of the BERT layer, and fine-tuning the whole model.

#### 3.1 BERT without Fine-tuning

In this model, we directly run the bert-base-uncased model on each training datum, transforming each input text into 768 length vector. Then, a Ridge liner regression is performed, and mean-squared loss is used as loss function. Regularization factors are chosen from (10, 1, 0.1, 0.01). However, different regularization factors hardly infects the validation error. 

We choose regularization factors to be 0.1, which results in the best validation loss. The test loss is 0.477, which is close to the baseline model performance. This model is unsuccessful. A potential reason is that without fine-tuning, most of the features captured by the bert embedding is not useful for the prediction task.

#### 3.2 BERT with Fine-tuning

![[Pasted image 20231128140934.png|400]]

In this model, we add a linear regression layer on top of the BERT model. It takes the output of “\[CLS\]" token as the input for the linear regression. We use mean-squared loss is used as loss function. The regularization factor is set to be 0.01.

We set epoch number to be 5. The model runs for 42 minutes on a single RTX 4090 GPU. Epoch 3-5 has small training loss while increasing validation loss, showing that the model is overfitting. We choose epoch 2 as our final model as it has lowest validation loss. The test loss is 0.385, which is 21% improvement compared with the baseline model.


