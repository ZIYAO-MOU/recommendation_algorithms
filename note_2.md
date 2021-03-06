10/20 20:57

## 协同过滤

根据用户之前的喜好以及其他兴趣相近的用户的选择来给用户推荐物品

一般是仅仅基于用户的行为数据，而不依赖项的任何附加信息

**基于用户的协同过滤算法userCF**

**基于物品的协同过滤算法itemCF**

### 相似性度量方法

- 杰卡德相似系数

  Jaccard系数值越大，样本相似度越高

  J(A,B)=|A∩B|/|A∪B|

  ```
  def jaccard_sim(a, b):
      unions = len(set(a).union(set(b)))
      intersections = len(set(a).intersection(set(b)))
      return intersections / unions
   
  a = ['x', 'y']
  b = ['x', 'z', 'v']
  print(jaccard_sim(a, b))
  ```

  > skrlearn中也有相关实现，但是要求数据进行过encode处理，而且两个数组的长度也必须一样。

  [1]: https://www.biaodianfu.com/jaccard-tanimoto.html

- 余弦相似度

  ![image-20201020185708236](C:\Users\15905\AppData\Roaming\Typora\typora-user-images\image-20201020185708236.png)

- 皮尔逊相关系数

  余弦相似度的修正，去除均值

  **中心化再余弦计得到的相关系数叫作皮尔逊相关系数.**

### 基于用户的协同过滤

eg. Alice对某物品的评分的预测

1.计算Alice与其他用户的相关系数

2.找到与Alice最相近的两个用户，并计算他们的平均分

3.预测Alice对该物品的评分

![image-20201020195217357](C:\Users\15905\AppData\Roaming\Typora\typora-user-images\image-20201020195217357.png)

### 基于物品的协同过滤

基于物品的协同过滤(ItemCF)的基本思想是预先根据所有用户的历史偏好数据计算物品之间的相似性，然后把与用户喜欢的物品相类似的物品推荐给用户。比如物品a和c非常相似，因为喜欢a的用户同时也喜欢c，而用户A喜欢a，所以把c推荐给用户A。**ItemCF算法并不利用物品的内容属性计算物品之间的相似度， 主要通过分析用户的行为记录计算物品之间的相似度， 该算法认为， 物品a和物品c具有很大的相似度是因为喜欢物品a的用户大都喜欢物品c**。

![图片](http://ryluo.oss-cn-chengdu.aliyuncs.com/JavagdvaYX0HSW4PdssV.png!thumbnail)

**基于物品的协同过滤算法主要分为两步：**

- 计算物品之间的相似度
- 根据物品的相似度和用户的历史行为给用户生成推荐列表（购买了该商品的用户也经常购买的其他商品）

基于物品的协同过滤算法和基于用户的协同过滤算法很像， 所以我们这里直接还是拿上面Alice的那个例子来看。

![图片](http://ryluo.oss-cn-chengdu.aliyuncs.com/JavaE306yXB4mGmjIxbn.png!thumbnail)

如果想知道Alice对物品5打多少分， 基于物品的协同过滤算法会这么做：

1. 首先计算一下物品5和物品1， 2， 3， 4之间的相似性(它们也是向量的形式， 每一列的值就是它们的向量表示， 因为ItemCF认为物品a和物品c具有很大的相似度是因为喜欢物品a的用户大都喜欢物品c， 所以就可以基于每个用户对该物品的打分或者说喜欢程度来向量化物品)
2. 找出与物品5最相近的n个物品
3. 根据Alice对最相近的n个物品的打分去计算对物品5的打分情况



1. **召回率**

    对用户u推荐N个物品记为 R(u) , 令用户u在测试集上喜欢的物品集合为 T(u) ， 那么召回率定义为：

​		Recall=∑u|R(u)∩T(u)|∑u|T(u)|

​		这个意思就是说， 在用户真实购买或者看过的影片里面， 我模型真正预测出了多少， 这个考察的是模型推荐		的一个全面性。

​	2.**准确率**
​		准确率定义为：

​		Precision=∑u∣R(u)∩T(u)|∑u|R(u)|

​		这个意思再说， 在我推荐的所有物品中， 用户真正看的有多少， 这个考察的是我模型推荐的一个准确性。
​		为了提高准确率， 模型需要把非常有把握的才对用户进行推荐， 所以这时候就减少了推荐的数量， 而这往往		就损失了全面性， 真正预测出来的会非常少，所以实际应用中应该综合考虑两者的平衡。

​	3.**覆盖率**
​		覆盖率反映了推荐算法发掘长尾的能力， 覆盖率越高， 说明推荐算法越能将长尾中的物品推荐给用户。

​		 Coverage =∣∣⋃u∈UR(u)∣∣|I|

​		该覆盖率表示最终的推荐列表中包含多大比例的物品。如果所有物品都被给推荐给至少一个用户， 那么覆盖		率是100%。

​	4.**新颖度**
​	   用推荐列表中物品的平均流行度度量推荐结果的新颖度。 

​       如果推荐出的物品都很热门， 说明推荐的新颖度较低。

​      由于物品的流行度分布呈长尾分布， 所以为了流行度的平均值更加稳定， 在计算平均流行度时对每个物品的	流行度取对数。

### 协同过滤算法的权重改进

![b9d03763946834028ca474d2f9ef94205c289691](C:\Users\15905\Downloads\b9d03763946834028ca474d2f9ef94205c289691.png)

图二：

如果 item-j 是很热门的商品，导致很多喜欢 item-i 的用户都喜欢 item-j，这时 wij 就会非常大。同样，**几乎所有的物品都和 item-j 的相关度非常高，这显然是不合理的。**

所以图2中分母通过引入 N(j) 来对 item-j 的热度进行惩罚。

图三：

如果需要进一步对热门物品惩罚，可以继续修改公式为如图3所示，**通过调节参数 α ， α 越大，惩罚力度越大，热门物品的相似度越低，整体结果的平均热门程度越低。**

图四：

同样的，Item-based CF 也需要考虑**活跃用户**（即一个活跃用户（专门做刷单）可能买了非常多的物品）的影响，**活跃用户对物品相似度的贡献应该小于不活跃用户**。

**协同过滤的问题：**

协同过滤算法存在的问题之一就是**泛化能力弱**， 即协同过滤无法将两个物品相似的信息推广到其他物品的相似性上。 导致的问题是**热门物品具有很强的头部效应， 容易跟大量物品产生相似， 而尾部物品由于特征向量稀疏， 导致很少被推荐**。

**矩阵分解技术(Matrix Factorization,MF**)被提出， 该方法在协同过滤共现矩阵的基础上， 使用更稠密的隐向量表示用户和物品， 挖掘用户和物品的隐含兴趣和隐含特征， 在一定程度上弥补协同过滤模型处理稀疏矩阵能力不足的问题。 具体细节等后面整理， 这里先铺垫一下
