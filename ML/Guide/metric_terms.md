---

title: (draft)评估模型的指标

date: 2019-11-25 14:29:44
tags: [Draft, Guide]
categories: [ML]

---

<!-- more -->

# Example

A school is running a machine learning primary diabetes scan on all of its students.
The output is either diabetic (+ve) or healthy (-ve).

- True positive (TP): Prediction is +ve and X is diabetic, we want that
- True negative (TN): Prediction is -ve and X is healthy, we want that too
- False positive (FP): Prediction is +ve and X is healthy, false alarm, bad
- False negative (FN): Prediction is -ve and X is diabetic, the worst

**If it starts with True then the prediction was correct**

**If it starts with False then the prediction was incorrect**

**Positive or negative indicates the output of our program. While true or false judges this output whether
correct or incorrect.**


```

           --------------------------+--------------------------------+--------
                                     |          Predicted (预测)      |
                                     |--------------------------------|
                                     |               |                |
                                     |    Positive   |    Negative    |
                                     |               |                |
           --------------------------+--------------------------------|
                       |             |               |                |
                       |  Positive   |      TP       |      FN        |
                       |             |               |     去真       |
                       |             |   真正例      |    假负例      |
             Observed  |-------------|---------------+----------------|
                       |             |               |                |
             (实际)    |             |      FP       |      TN        |
                       |  Negative   |     存伪      |                |
                       |             |   假正例      |     真负例     |
           --------------------------+--------------------------------+--------

TP + FP: 程序输出为Positive(+ve, labeled as diabetic)
TP + FN:
```



# Term

- harmonic mean: 调和平均数, 倒数平均数

- Accuracy: 准确度

> It's the ratio of the correctly labeled subjects to the whole pool of subjects.
Accuracy is the most intuitive one.
Accuracy answers the following question: **How many students did we correctly label out of all the students?**
Accuracy = (TP+TN)/(TP+FP+FN+TN)
>
> > 瞧不出病的大夫,是个负责任的好大夫吗?当然不是!这也就是在分类问题上,为什么要引入F1 Score的原因.
> > 患癌症的人毕竟是少数, 一个不会看病的大夫只要把人都判为健康, 准确度也是很高的, 其实他是个骗子

- Precision: 精确度, 查准率 (预测)

> Precision is the ratio of the correctly +ve labeled by our program to all +ve labeled.
Precision answers the following: **How many of those who we labeled as diabetic are actually diabetic?**
Precision = TP/(TP+FP) 预测为正的样本中实际正样本的比例 (在所有被预测为正类的样本中真实结果也为正类的占比)

```
                     ***********    ***********             A: 正确判为癌症(模型诊断筛选出患病的病人)
                 ****           ****           ***          B: 误判为癌症
               **              **   **            **        A + C: 癌症患者(所有真实癌症病人)
              *               *       *             *       A + B: 被判为癌症
             *               *         *             *      Precision = A / (A + B), 表明 判定'患病'的准确度
             *        C      *    A    *    B        *      Recall = A / (A + C)
             *               *         *             *
              *               *       *             *
               **              **   **            **
                 ****           ****           ***
                     ***********    ***********

                 实际为正的样本     预测为正的样本

```

- Sensitivity(aka Recall): 灵敏度, 查全率, 召回率 (真实+预测)

> 医学: 病人中得出阳性检测的样本占病人总数的百分比;不漏诊(漏诊即应该为阳性被诊断为阴性)的概率.

> Recall is the ratio of the correctly +ve labeled by our program to all who are diabetic in reality.
Recall answers the following question: **Of all the people who are diabetic, how many of those we correctly predict?**
Recall = TP/(TP+FN)  实际正样本中预测为正的比例 (在所有真实结果为正类的样本中预测结果也为正类的占比)


> 一个例子:
> > 若希望将好瓜尽可能多地选出来,则可通过增加选瓜的数量来实现,如果将所有西瓜都选上,那么所有的好瓜也必然
> > 都被选上了(查全率高),但这样查准率就会较低;若希望选出的瓜中好瓜比例尽可能高,则可只挑选最有把握的瓜
> > ,但这样就难免会漏掉不少好瓜,使得查全率较低.


- Specificity: 特异性

> 医学: 健康人中得出阴性检测的样本占健康人总数的百分比;不误诊(误诊为应该为阴性但是被诊断为阳性)的概率.

> Specificity is the correctly -ve labeled by the program to all who are healthy in reality.
Specifity answers the following question: **Of all the people who are healthy, how many of those did we correctly predict?**
Specificity = TN/(TN+FP)

- F1-score (aka F-Score / F-Measure)

> F1 Score considers both precision and recall.
It is the harmonic mean(average) of the precision and recall.
F1 Score is best if there is some sort of balance between precision (p) & recall (r) in the system.
F1 Score = 2*(Recall * Precision) / (Recall + Precision) 精确率和召回率调和平均数

- AUC

二分类模型的评价 Area under Curve(曲线下的面积)

> Q1: 为什么不直接通过计算准确率来对模型进行评价呢?

> 机器学习中的很多模型对于分类问题的预测结果大多是概率,即属于某个类别的概率,如果计算准确率的话,就要把概
率转化为类别,这就需要设定一个阈值,概率大于某个阈值的属于一类,概率小于某个阈值的属于另一类,而阈值的设定
直接影响了准确率的计算.


# References

1. [Accuracy, Recall, Precision, F-Score & Specificity, which to optimize on?](https://towardsdatascience.com/accuracy-recall-precision-f-score-specificity-which-to-optimize-on-867d3f11124)

2. [Basic evaluation measures from the confusion matrix](https://classeval.wordpress.com/introduction/basic-evaluation-measures/)

3. [翻墙: mAP (mean Average Precision) for Object Detection](https://medium.com/@jonathan_hui/map-mean-average-precision-for-object-detection-45c121a31173)

4. [为什么需要F1 Score,而不是Accuracy?](https://zhuanlan.zhihu.com/p/49895905)

5. [F1-micro 与 F1-macro区别和计算原理](https://blog.csdn.net/lyb3b3b/article/details/84819931)

6. [模型评估-性能度量(分类问题)](https://blog.csdn.net/weixin_43378396/article/details/90695013)
