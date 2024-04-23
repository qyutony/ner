# ner
ner exp log

| item | framework | exp log |
| :---: | :---: | :---: |
| exp_01 | word2vec, bilstm, crf | 1）embedding用的'word2vec-google-news-300'，还可以考虑用其他的如bert，glove，fasttext ；2）embedding_dim选的300，trainable=True；3） 堆叠了三层bilstm，三层的units分别设置为50，100，50；4）model.complie中选择了AdamW优化器和SigmoidFocalCrossEntropy()损失函数，通过在优化器中添加权重衰减（weight decay）来进行正则化；5）可视化的结果用到了tag的矩阵热力图，train_test的loss函数迭代图，ner的分类矩阵结果（precision，recall，f1-score，accuracy，macro avg，weighted avg） |

