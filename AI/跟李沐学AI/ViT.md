An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale

[paperswithcode](https://paperswithcode.com/): explore state-of-the-art models in different fields

### Abstract

When pre-trained on large amounts of data and transferred to multiple mid-sized recognition benchmarks, ViT attains excellent results compared with CNN.


### 1 Introduction

* 序列长度限制怎么解决？ 将图片划分成一个个16x16的patch，每个patch通过FC layer形成embedding，再对所有patch的embedding进行transformer。
* 主打可扩展性：模型越大，性能越好，迁移性强。
* 没有像CNN一样引入图像特有的归纳偏置，在模型大小相等的情况下效果不如CNN。但大数据的训练效果出色。像处理NLP一样处理图像。

### 3 Model

![[Pasted image 20230530165228.png]]

* Use 1D array for position embedding (Performance 1D = 2D)
* Use an additional \[class\] token as the model output
* If input size is different, the model will lose the power of position encoding 

### 4 Experiment

![[Pasted image 20230530171537.png|500]]

![[Pasted image 20230530171935.png|400]]

![[Pasted image 20230530173216.png]]

* Best (just a little bit better than state-of-art) performance on all datasets
* Lower training cost
* Larger dataset, larger performance gain
* Can learn 2D position
* Can learn long distance features

### 5 Conclusion

* 如何迁移到分类之外的图像任务，如目标检测？
* 如何做到大规模自监督训练？
* 多模态？