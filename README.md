# ner
ner exp logs

>**benchmark (exp_01_20240427.ipynb)**
- Framework: 
    - Embedding
        - Embedding用的'word2vec-google-news-300'，还可以考虑用其他的如bert，glove，fasttext
        - Embedding_dim选的300，trainable=True，实现基于预训练模型的微调
    - BiLSTM
        - 堆叠了三层BiLSTM，三层的units分别设置为50，100，50
    - CRF
    - Model Compile
        - model.complie中选择了AdamW优化器和SigmoidFocalCrossEntropy()损失函数，通过在优化器中添加权重衰减（weight decay=0.001）来进行正则化
    - Validation
        - 可视化的结果用到了tag的矩阵热力图
        - train_test的loss函数迭代图
        - ner的分类矩阵结果（precision，recall，f1-score，accuracy，macro avg，weighted avg）



```
Classification Report:
                precision    recall  f1-score   support

           O       0.99      0.98      0.98     89922
       I-art       0.00      0.00      0.00         0
       B-eve       0.00      0.00      0.00         0
       B-nat       0.00      0.00      0.00         0
         PAD       1.00      1.00      1.00    254555
       B-gpe       0.93      0.94      0.93      1580
       I-per       0.77      0.83      0.80      1670
       B-art       0.00      0.00      0.00         0
       I-tim       0.46      0.87      0.60       337
       I-nat       0.00      0.00      0.00         0
       I-org       0.60      0.52      0.55      1953
       B-geo       0.84      0.82      0.83      3933
       B-tim       0.84      0.88      0.86      1956
       B-per       0.76      0.79      0.77      1651
       I-geo       0.22      0.86      0.35       184
       B-org       0.62      0.64      0.63      1959
       I-eve       0.00      0.00      0.00         0
       I-gpe       0.00      0.00      0.00         0

    accuracy                           0.99    359700
   macro avg       0.45      0.51      0.46    359700
weighted avg       0.99      0.99      0.99    359700
```


>**将embedding部分的trainable=True改为false (exp_01_20240427-(1).ipynb)**

```
Classification Report:
                precision    recall  f1-score   support

         PAD       1.00      1.00      1.00    255750
       I-per       0.76      0.73      0.74      1807
       I-nat       0.00      0.00      0.00         0
       I-org       0.22      0.68      0.34       554
       B-tim       0.71      0.77      0.74      1904
       I-tim       0.04      0.91      0.08        32
       B-per       0.59      0.72      0.65      1341
       I-art       0.00      0.00      0.00         0
       B-geo       0.71      0.71      0.71      3683
       B-eve       0.00      0.00      0.00         0
           O       0.99      0.94      0.97     92803
       B-nat       0.00      0.00      0.00         0
       I-gpe       0.00      0.00      0.00         0
       I-eve       0.00      0.00      0.00         0
       B-gpe       0.76      0.87      0.81      1311
       B-org       0.10      0.77      0.18       268
       B-art       0.00      0.00      0.00         0
       I-geo       0.31      0.87      0.46       247

    accuracy                           0.98    359700
   macro avg       0.34      0.50      0.37    359700
weighted avg       0.99      0.98      0.98    359700
```


>**将三层堆叠的BiLSTM改为一层 (exp_01_20240427-(2).ipynb)**
```
Classification Report:
                precision    recall  f1-score   support

       B-eve       0.00      0.00      0.00         0
       B-nat       0.00      0.00      0.00         0
       I-art       0.00      0.00      0.00         0
       I-tim       0.71      0.73      0.72       642
         PAD       1.00      1.00      1.00    254929
       I-geo       0.63      0.77      0.69       574
       B-gpe       0.93      0.96      0.95      1541
       I-gpe       0.11      1.00      0.20         2
       I-nat       0.00      0.00      0.00         0
       B-org       0.64      0.68      0.66      1943
       B-tim       0.86      0.90      0.88      1909
       B-geo       0.83      0.84      0.83      3669
       I-per       0.79      0.85      0.82      1520
       I-org       0.60      0.63      0.61      1659
           O       0.99      0.98      0.98     89826
       I-eve       0.00      0.00      0.00         0
       B-per       0.77      0.84      0.80      1486
       B-art       0.00      0.00      0.00         0

    accuracy                           0.99    359700
   macro avg       0.49      0.56      0.51    359700
weighted avg       0.99      0.99      0.99    359700
```


>**在一层BiLSTM的基础上，将units从50改为100 (exp_01_20240427-(3).ipynb)**

```
Classification Report:
                precision    recall  f1-score   support

       B-nat       0.10      0.67      0.17         3
       I-geo       0.68      0.80      0.73       608
       I-per       0.77      0.85      0.81      1502
       B-art       0.00      0.00      0.00         0
       B-geo       0.87      0.83      0.85      4017
       B-tim       0.82      0.92      0.86      1868
         PAD       1.00      1.00      1.00    254893
       I-eve       0.00      0.00      0.00         1
       I-nat       0.00      0.00      0.00         0
       I-art       0.00      0.00      0.00         0
       I-gpe       0.00      0.00      0.00         0
       I-tim       0.65      0.75      0.70       592
       B-eve       0.00      0.00      0.00         0
           O       0.99      0.97      0.98     90452
       B-gpe       0.93      0.95      0.94      1551
       B-per       0.77      0.83      0.80      1555
       I-org       0.50      0.70      0.59      1191
       B-org       0.55      0.76      0.64      1467

    accuracy                           0.99    359700
   macro avg       0.48      0.56      0.50    359700
weighted avg       0.99      0.99      0.99    359700
```


>**将optimizer=AdamW(weight_decay=0.001)中的正则化设定取消，改为optimizer=AdamW(weight_decay=0) (exp_01_20240427-(4).ipynb)**
```
Classification Report:
                precision    recall  f1-score   support

       B-geo       0.84      0.85      0.84      3756
       B-gpe       0.94      0.94      0.94      1607
       I-per       0.82      0.86      0.84      1665
       B-org       0.64      0.68      0.66      1821
       I-org       0.57      0.73      0.64      1261
       B-nat       0.44      0.35      0.39        23
           O       0.99      0.98      0.99     90235
       I-eve       0.16      0.33      0.22        12
       I-geo       0.72      0.77      0.75       684
       B-art       0.08      0.44      0.13         9
       I-gpe       0.43      0.87      0.58        15
       B-tim       0.85      0.89      0.87      1978
       I-tim       0.66      0.77      0.71       539
       I-art       0.00      0.00      0.00         3
       B-per       0.79      0.83      0.81      1584
       B-eve       0.29      0.42      0.34        19
       I-nat       0.50      0.50      0.50         2
         PAD       1.00      1.00      1.00    254487

    accuracy                           0.99    359700
   macro avg       0.60      0.68      0.62    359700
weighted avg       0.99      0.99      0.99    359700
```

>**将optimizer=AdamW(weight_decay=0.001)中的正则化设定增强，改为optimizer=AdamW(weight_decay=0.1) (exp_01_20240427-(5).ipynb)**
```
Classification Report:
                precision    recall  f1-score   support

       I-per       0.00      0.00      0.00         0
       B-tim       0.00      0.00      0.00         0
         PAD       0.00      0.00      0.00         0
       B-per       0.00      0.00      0.00         0
       I-geo       0.00      0.00      0.00         0
       B-nat       0.00      0.00      0.00         0
       I-nat       0.00      0.00      0.00         0
       I-eve       0.00      0.00      0.00         0
           O       0.09      0.86      0.17      9591
       I-tim       0.00      0.00      0.00         0
       B-org       0.00      0.00      0.00         0
       I-art       0.00      0.00      0.00         0
       B-art       0.00      0.00      0.00         0
       B-eve       0.00      0.00      0.00         0
       B-gpe       0.00      0.00      0.00         0
       B-geo       0.91      0.01      0.02    350109
       I-gpe       0.00      0.00      0.00         0
       I-org       0.00      0.00      0.00         0

    accuracy                           0.03    359700
   macro avg       0.06      0.05      0.01    359700
weighted avg       0.89      0.03      0.02    359700
```

>**将optimizer=AdamW(weight_decay=0.001)中的正则化设定增强，改为optimizer=AdamW(weight_decay=0.01) (exp_01_20240427-(6).ipynb)**
```
Classification Report:
                precision    recall  f1-score   support

       B-tim       0.00      0.00      0.00         0
       B-geo       0.85      0.27      0.42     11408
       B-org       0.00      0.00      0.00         0
       I-tim       0.00      0.00      0.00         0
       I-art       0.00      0.00      0.00         0
       I-org       0.00      0.00      0.00         0
       I-geo       0.00      0.00      0.00         0
       I-eve       0.00      0.00      0.00         0
           O       1.00      0.95      0.97     93207
       B-nat       0.00      0.00      0.00         0
       B-eve       0.00      0.00      0.00         0
       B-art       0.00      0.00      0.00         0
       I-nat       0.00      0.00      0.00         0
         PAD       1.00      1.00      1.00    255084
       B-gpe       0.00      0.00      0.00         0
       I-per       0.00      0.00      0.00         0
       B-per       0.00      1.00      0.00         1
       I-gpe       0.00      0.00      0.00         0

    accuracy                           0.96    359700
   macro avg       0.16      0.18      0.13    359700
weighted avg       0.99      0.96      0.97    359700
```


>**将embedding模型由Word2Vec改为FastText - cc.en.300.bin (exp_01_20240427-(7).ipynb)**



-----