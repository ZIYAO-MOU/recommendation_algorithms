## Wide and deep模型

### 点击率预估模型的必要性和可行性

- 必要性

  FM香，为啥还要整这个Wide&Deep呢？

  其缺点在于**：当query-item矩阵是稀疏并且是high-rank的时候（比如user有特殊的爱好，或item比较小众），很难非常效率的学习出低维度的表示**。这种情况下，大部分的query-item都没有什么关系。

  但是dense embedding会导致几乎所有的query-item预测值都是非0的，这就导致了**推荐过度泛化**，会推荐一些不那么相关的物品。相反，简单的linear model却可以通过cross-product transformation来记住这些**exception rules**

- 可行性

  点击率预估问题就是一个**二分类**的问题，在机器学习中可以使用**逻辑回归**作为模型的输出，其输出的就是一个概率值，我们可以将机器学习输出的这个概率值认为是某个用户点击某个广告的概率。

  广告点击率预估是需要得到某个用户对某个广告的点击率，然后结合广告的出价用于排序。

### Wide and deep模型的记忆能力和泛化能力

我们的Wide&Deep模型就能够融合这两种推荐结果做出最终的推荐，得到一个比之前的推荐结果都好的模型。

- 记忆能力

  Memorization趋向于更加保守，推荐用户之前有过行为的items。**Memorization只需要使用一个线性模型即可实现。**

- 泛化能力

  相比之下，generalization更加趋向于提高推荐系统的多样性（diversity），**Generalization需要使用DNN实现。**

- wide

  wide有助于记忆能力的提升。

  wide部分训练时候使用的优化器是带 L1 正则的FTRL算法(Follow-the-regularized-leader)，

  而L1 FTLR是非常注重模型稀疏性质的，也就是说W&D模型采用L1 FTRL是想让Wide部分变得更加的稀疏，即Wide部分的大部分参数都为0，这就大大压缩了模型权重及特征向量的维度。

  关于FTRL算法可以参照

  [FTRL算法详解](https://www.jianshu.com/p/befb9e02d858)

  eg.Google W&D期望wide部分发现这样的规则：**用户安装了应用A，此时曝光应用B，用户安装应用B的概率大。**

- deep

  Deep部分是一个DNN模型，输入的特征主要分为两大类，一类是数值特征(可直接输入DNN)，一类是类别特征(需要经过Embedding之后才能输入到DNN中)，Deep部分的数学形式如下：
  $$
  a^{(l+1)} = f(W^{l}a^{(l)} + b^{l})
  $$
  **我们知道DNN模型随着层数的增加，中间的特征就越抽象，也就提高了模型的泛化能力。** 对于Deep部分的DNN模型作者使用了深度学习常用的优化器AdaGrad，这也是为了使得模型可以得到更精确的解。

### **Wide部分与Deep部分的结合**

W&D模型是将两部分输出的结果结合起来联合训练，将deep和wide部分的输出重新使用一个逻辑回归模型做最终的预测，输出概率值。联合训练的数学形式如下：
$$
P(Y=1|x)=\delta(w_{wide}^T[x,\phi(x)] + w_{deep}^T a^{(lf)} + b)
$$

###  操作流程

- **Retrieval** ：利用机器学习模型和一些人为定义的规则，来返回最匹配当前Query的一个小的items集合，这个集合就是最终的推荐列表的候选集。

- **Ranking**：

  - 收集更细致的用户特征，如：

    - User features（年龄、性别、语言、民族等）
    - Contextual features(上下文特征：设备，时间等)
    - Impression features（展示特征：app age、app的历史统计信息等）

  - 将特征分别传入Wide和Deep

    一起做训练

    。在训练的时候，根据最终的loss计算出gradient，反向传播到Wide和Deep两部分中，分别训练自己的参数（wide组件只需要填补deep组件的不足就行了，所以需要比较少的cross-product feature transformations，而不是full-size wide Model）

    - 训练方法是用mini-batch stochastic optimization。
    - Wide组件是用FTRL（Follow-the-regularized-leader） + L1正则化学习。
    - Deep组件是用AdaGrad来学习。

  - 训练完之后推荐TopN

**所以wide&deep模型尽管在模型结构上非常的简单，但是如果想要很好的使用wide&deep模型的话，还是要深入理解业务，确定wide部分使用哪部分特征，deep部分使用哪些特征，以及wide部分的交叉特征应该如何去选择**

### 代码实战真的来不及了！！！学业繁忙，下次一定

