# 推荐系统学习笔记

10/19 20:57

## 概述

1. ###### what why who

2. ###### 常用测评指标

   - 用户满意度

   - 预测准确度

     1. 评分预测

        RMSE(均方差)

        MAE(绝对平均误差)

     2. TOPN

        Precision/Recall

     3. 覆盖率

        研究物品在推荐列表中出现的次数分布来描述推荐系统挖掘长尾的能力

        长尾（可以让低流行度的商品也得到销售？）

        ![img](https://bkimg.cdn.bcebos.com/pic/d62a6059252dd42a9035a3be003b5bb5c9eab8e3?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U4MA==,g_7,xp_5,yp_5)

        分布平缓的化，覆盖率较高，分布比较陡，说明覆盖率比较低

        **信息熵定义覆盖率**

        ![img](https://img-blog.csdnimg.cn/20201018172839795.png)

        **基尼系数定义覆盖率**

        ![img](https://img-blog.csdnimg.cn/20201018173109977.png)

        G的大小也可以用于判断长尾，G越大越容易出现长尾

     4. 多样性

        ![img](https://img-blog.csdnimg.cn/20201018173318550.png)

     5. 新颖性

     6. AUC曲线

        AUC：ROC曲线下与坐标轴围成的面积

        ROC FP/TP 假的真了/真的真了

        ![img](https://img-blog.csdnimg.cn/20201018175508764.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h6eTQ1OTE3Njg5NQ==,size_16,color_FFFFFF,t_70)

3. 召回

   召回阶段负责将海量的候选集快速缩小几万到几千的规模，排序层则负责对缩小后的候选集进行精确排序。

   召回层：候选集大，特征少，快速召回，也要保证召回率（多路召回策略）

   排序层：使用更复杂的模型

   - 多路召回策略

     策略之间不相关---》并发多线程同时执行

     召回策略与业务强相关，业务一般指真实场景下考虑别的召回规则

     依然存在问题：选择的的策略之间信息割裂。无法考虑综合影响。

   - embedding召回

     将稀疏的向量（eg独热向量）转换成稠密的向量

     embedding对独热向量做了平滑

     主流的embedding技术：Graph text image
