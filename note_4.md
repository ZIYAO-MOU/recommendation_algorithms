# 推荐系统之FM

19：24 10/25

### FM公式的理解

模型前半部分就是普通的LR线性组合，后半部分的交叉项：特征组合。

![image-20201025193114336](C:\Users\15905\AppData\Roaming\Typora\typora-user-images\image-20201025193114336.png)

需要估计的参数有 ω0∈R ， ωi∈R ， V∈R ， <⋅,⋅> 是长度为 k 的两个向量的点乘，公式如下：

![image-20201025193343636](C:\Users\15905\AppData\Roaming\Typora\typora-user-images\image-20201025193343636.png)

- ω0 为全局偏置；
- ωi 是模型第 i 个变量的权重;
- ωij=<vi,vj> **特征 i 和 j 的交叉权重**;
- vi 是第 i 维特征的隐向量;
- <⋅,⋅> 代表向量点积;
- k(k<<n) 为隐向量的长度，包含 k 个描述特征的因子。

![image-20201025195208974](C:\Users\15905\AppData\Roaming\Typora\typora-user-images\image-20201025195208974.png)

**解释**：

- vi,f 是一个具体的值；
- 第1个等号：对称矩阵 W 对角线上半部分；
- 第2个等号：把向量内积 vi , vj 展开成累加和的形式；
- 第3个等号：提出公共部分；
- 第4个等号： i 和 j 相当于是一样的，表示成平方过程。

### 应用

工业应用的具体操作步骤：

- 离线训练好FM模型（学习目标可以是CTR）
- 将训练好的FM模型Embedding取出
- 将每个uid对应的Embedding做avg pooling（平均）形成该用户最终的Embedding，item也做同样的操作
- 将所有的Embedding向量放入Faiss等
- 线上uid发出请求，取出对应的user embedding，进行检索召回
