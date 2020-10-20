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

