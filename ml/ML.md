# Page 0

Updated it to github with it's pdf version after editing.

Be careful do not over-changing the function.

# **01 Machine Learning Basic**

---

## 01.01 **ML的组件**



- $$
  \begin{aligned}  
  &input: \:x \in X \\
  &Target \:function \:f:\: y \in Y\\
  &Training \: sample: D=\left \{ \left ( x_{1}, y_{1} \right ) ,\left ( x_{2}, y_{2} \right )...\left ( x_{n}, y_{n} \right ) \right \} \\
  &Hypothesis\:function\: g:\:X\longrightarrow Y,g \:is \: as\: close\:to\:f\\
  &g\in H=\left \{ H_{1} , H_{2} ...H_{n} \right \} , H称为假设函数集，包含了好与不好的各种h（假设）
  \end{aligned}
  $$

## 01.02 **ML流程图**

<img src="https://raw.githubusercontent.com/otainginga/Image/main/mlflow.png">



- 机器学习算法一般用A表示
- 假设空间或者叫做假设集合一般用H表示，它是包含各种各样的假设，其中包括好的假设和坏的假设，A从H这个集合中挑选出它认为最好的假设作为g

## 01.03 **ML vs DM & ML vs AI & ML vs Statistic**

- 简单的说就是相辅相成，密不可分，工具与结果的关系
- 别烦了，现在没啥好说的，等深入理解了ml自然就知道了

## 01.04 **输出空间的分类**

- 二元分类
- 多元分类 主要应用模式识别
- 回归分析 输出空间为整个或范围的实数集
- 结构学习

## 01.05 **训练集的标记的分类**

- supervised learning 知道数据输入的同时还知道数据的标记
- unsupervised  learning 训练集没有标记 一般用于聚类
- 半监督学习 通过少量有标记的训练点和大量无标记的训练点达到学习的目的
- 强化学习 （输入 输出 评分）

## 01.06 **获取数据的分类**

- 批量学习
- 在线学习
- 主动学习



# 02 PLA

---



the **perceptron** is an algorithm for supervised learning of binary classifiers.

* Notes Including [Norm](https://blog.csdn.net/liyuanbhu/article/details/51622695)

* Notes including [Gram](https://zhuanlan.zhihu.com/p/75156837)



## 02.01 Limitation

1. Data can only be divided into 2 categories
2. Data must be linearly separable







## 02.02 Case



### step 1:Algebraic to Geometric 



<img src="https://raw.githubusercontent.com/otainginga/Image/main/PLAcase02.png" alt="PLA case step 1" style="zoom:33%;" />



由于目标函数 
$$
\begin{aligned}  
&f\left ( X \right ) =Y,Y\in \left \{ +1,-1 \right \}\\
&then\:we \: have\:hypothesis function\:h\left ( x \right )  =sign\left ( w^{t}x  \right )\\
&注：符号函数f\left ( x \right )= sign\left ( x  \right )图像为：\\

&在二维 R^{2} 空间上，h可以表示为 h\left ( x \right )= sign\left ( w_{0}\cdot1 + w_{1} x_{1} + w_{2} x_{2} \right )


\end{aligned}
$$
![img](https://pic3.zhimg.com/80/v2-002e70fcba0d83ca114ac90d17e75e16_720w.jpg)



based on the feature for the result of the hypthesis fuction(only 1&0), we **can convert algebraic problems into geometric problems** , which is , asume that there is a 2-dimension space, and our task is to find the best line to class them.

### step 2: modify the line(Iteration)

![PLA](https://raw.githubusercontent.com/otainginga/Image/main/PLA_01.jpg)



用 ![[公式]](https://www.zhihu.com/equation?tex=%5Cvec+%7Bw_0%7D) 作为初始直线，不断迭代使得直线一次比一次更好( ![[公式]](https://www.zhihu.com/equation?tex=%5Cvec+%7Bw_0%7D) 是向量不是上面向量的分量！此处的0是迭代次数t=0，不是分量下标0)

![[公式]](https://www.zhihu.com/equation?tex=%5Cvec+w_t) = ![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Bpmatrix%7D+w_0+%5C%5C+w_1+%5C%5C+w_2+%5Cend%7Bpmatrix%7D+%5C) ![[公式]](https://www.zhihu.com/equation?tex=%5Cvec+x) = ![[公式]](https://www.zhihu.com/equation?tex=%5Cbegin%7Bpmatrix%7D+1+%5C%5C+x_1+%5C%5C+x_2+%5Cend%7Bpmatrix%7D)

迭代次数 t = 0,1,2...

- 假设 ![[公式]](https://www.zhihu.com/equation?tex=%5Cvec+w_0) =(0,0,0)，代入训练集，找到一个 ![[公式]](https://www.zhihu.com/equation?tex=%28x_%7Bn%28t%29%7D%2Cy_%7Bn%28t%29%7D%29) 使得 ![[公式]](https://www.zhihu.com/equation?tex=sign%28%5Cvec+w_t%5ET%5Cvec+x_%7Bn%28t%29%7D%29+%5Cne+y_%7Bn%28t%29%7D) ，然后开始修正 ![[公式]](https://www.zhihu.com/equation?tex=%5Cvec+w_0) ，（注：此处用到向量数量积的几何意义，当w与x的夹角小于90度，则数量积为正，则sign值>0；当w与x的夹角大于90度，则数量积为负，则sign值<0）

- 即：角度>90 则修正为<90；反之亦然。 ![[公式]](https://www.zhihu.com/equation?tex=%5Cvec+w_%7Bt%3D1%7D+%5Cleftarrow+%5Cvec+w_t+%2By_%7Bn%28t%29%7D%5Cvec+x_%7Bn%28t%29%7D)
- 直到没有错误为止

### **step 3：**证明修正次数会有上界

### **step 4：**对于线性不可分的训练集的处理

找一条犯错误最小的划分线，一直跑算法，每次保存最少错误的直线，直到指定的次数为止



# 03 ML feasibility demonstration 

---

（feasibility, which is Generalization, extending findings on a single example to a population）

## 03.01 Hoeffding's inequality



### step 1 **证明适用于训练集的g同样适用于整个输入空间**

首先：学习可能是做不到的，在训练集中可以求得一个最佳假设g，但是在训练集外的样本中，g可能和目标函数f相差甚远。

**先从统计学出发，以从罐子中拿球为例：**

![img](https://pic4.zhimg.com/v2-64b38f7a47b5b9fe5d75cc19757a298b_b.jpg)

罐子中橙球比例u，绿球比例1-u，抽样样本N中橙球比例v，绿球比例1-v；可以想象到，v在很大几率上接近u，但是到底有多大的可能性两者接近呢？在此引入**霍夫丁不等式**：

---

*![[公式]](https://www.zhihu.com/equation?tex=P%5B%7C%5Cnu-%5Cmu%7C%3E%5Cvarepsilon%5D+%5Cleq+2exp%28-2%5Cvarepsilon%5E2N%29)*  

---

- ![[公式]](https://www.zhihu.com/equation?tex=%7C%5Cnu-%5Cmu%7C) 表示v与u的接近程度
- 左边表示v与u相差大于 ![[公式]](https://www.zhihu.com/equation?tex=%5Cvarepsilon) 的概率
- 随着N增大，v与u相差较大的概率不断变小
- 总结：v与u相等的结论“可能近似正确（probably approximately correct，PAC）”

Extending to ：

- 右边与u无关，即无需知道真实概率u
- 容忍度 ![[公式]](https://www.zhihu.com/equation?tex=%5Cvarepsilon) 不变，样本N越大概率越小
- 样本N不变，容忍度越大概率越小

### step 2 Generalization

**从罐子取球转换到机器学习（先确定一个假设h）**[证明训练样本的误差不会导致选择到的g对整个输入空间有误差]



| Indiviual                                                    | Crows                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Get one ball from a bin (asume the prob. of getting a red ball won't change) | Get the result from tons of bins. A result from each bin.    |
| 取了一把小球 = 训练样本 <br />橙色球 = h(x) !=f(x) <br />绿色球 = h(x) ==f(x) | Get one result from each bin<br />error rate : the hypothesis function does not perform well in one bin. |
| 橙色球比例u =  整个输入空间中，向量x满足h(x) != f(x)的比例 <br />橙色球比例v = 训练样本中，向量x满足h(x) != f(x)的比例 | 用 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28h%29+%E3%80%81E_%7Bout%7D%28h%29) 表示一个确定的假设函数h在训练样本的错误率( in one bin )和整个输入空间(bin space)的错误率，则有： ![[公式]](https://www.zhihu.com/equation?tex=P%5B%7CE_%7Bin%7D%28h%29-E_%7Bout%7D%28h%29%7C%3E%5Cvarepsilon%5D+%5Cleq+2exp%28-2%5Cvarepsilon%5E2N%29) |
| 因为u与v相等符合pca，所以f(x)与h(x)也符合pca                 | Picking Criteria:  如果求得一个 ![[公式]](https://www.zhihu.com/equation?tex=h) ，使得 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28h%29) 很小，则可以推出 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bout%7D%28h%29) 很小，即 ![[公式]](https://www.zhihu.com/equation?tex=h%5Capprox+f) ，则机器学习算法选一个最小的 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28h%29) ，将 ![[公式]](https://www.zhihu.com/equation?tex=g%5Cleftarrow+h) |

<span style="color:Navy;font-weight:bold;background-color: Yellow">**以上证明了机器学习在训练样本中学习得到的g在整个输入空间中也可行。**</span>



error rate 训练样本分为好的和不好的，如果存在h使得Ein和Eout相差很远，那就是不好的训练样本。

![img](https://pic4.zhimg.com/v2-4d9f3d54a18a3edefecff9f3f47d3437_b.jpg)![img](https://pic4.zhimg.com/80/v2-4d9f3d54a18a3edefecff9f3f47d3437_720w.jpg)

上图包含了M个假设h，而不好的D不是由单一假设就能确定的，而是只要有一个假设在此抽样D上表现不好则该抽样被标记为不好的。现在求训练样本不好的几率是多少：

![img](https://pic4.zhimg.com/v2-574d528dd3031280c049da6a3000b0df_b.jpg)![img](https://pic4.zhimg.com/80/v2-574d528dd3031280c049da6a3000b0df_720w.jpg)

当训练样本足够多，h个数有限大，则可以说每一个假设h都是安全的，即Ein ~ Eout

所以如果通过机器学习找出了g满足 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28g%29+%5Capprox+0) ，则PAC规则可以保证 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bout%7D%28g%29+%5Capprox+0) 

**一切看上去都是那么美好，但是忽略了一个问题，那就是如果h的个数无限多呢？请见下一篇讲解分析。**



# 04 Linear Regression 

---

## 04.01 Case study:



题目：银行贷款问题，训练数据是客户的n维资料和客户评分，求g



- 输出空间：正实数集
- 假设函数： ![[公式]](https://www.zhihu.com/equation?tex=h%28x%29+%3D+%5Cvec+w%5ET%5Cvec+x) （偏置b并入w0=b，x0=1)[*w* means weight]

![img](https://pic2.zhimg.com/v2-f4e47958da411699b6fc118a3476c195_b.jpg)<br>

Left: Linear regression in **1-D** space  Right:Linear regression in **2-D** space 



回归使用的错误衡量是平方误差 ![[公式]](https://www.zhihu.com/equation?tex=%28h%28x_n%29-y_n%29%5E2) ，即损失函数有：

![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28h%29+%3D+%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5EN%28h%28x_n%29-y_n%29%5E2+%5C%5C+%5CRightarrow+E_%7Bin%7D%28%5Cvec+w%29+%3D+%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5EN%28%5Cvec+w_n%5ET%5Cvec+x_n-y_n%29%5E2%5C%5C%5CRightarrow+min+%28E_%7Bin%7D%28%5Cvec+w%29%29)

所以我们的目的就是求解损失函数的最小值，这里我们分别用两种方法求解：随机梯度下降和矩阵求法。

### **Gradient descent**

数学原理及推导见：[机器学习数学：梯度下降法](https://zhuanlan.zhihu.com/p/31074506)

### <span style="font-weight:bold;background-color: Tomato">**Matrix**</span>

求解技巧，将损失函数转换成矩阵运算形式：

![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28%5Cvec+w%29+%3D+%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5EN%28%5Cvec+w_n%5ET%5Cvec+x_n-y_n%29%5E2%5C%5C+%3D+%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5EN%28%5Cvec+x_n%5ET%5Cvec+w_n-y_n%29%5E2%5C%5C+%EF%BC%88%E5%90%91%E9%87%8F%E5%86%85%E7%A7%AF%E7%AC%A6%E5%90%88%E4%BA%A4%E6%8D%A2%E5%BE%8B%EF%BC%89)

---



![[公式]](https://www.zhihu.com/equation?tex=%3D%5Cfrac%7B1%7D%7BN%7D%5Cbegin%7BVmatrix%7D+%5Cvec+x_1%5ET%5Cvec+w+-y_1+%5C%5C+%5Cvec+x_2%5ET%5Cvec+w+-y_2+%5C%5C+%5Cvec+x_n%5ET%5Cvec+w+-y_n+%5C%5C+%5Cend%7BVmatrix%7D%5E2%5C%5C+%EF%BC%88%E5%B9%B3%E6%96%B9%E8%BF%9E%E5%8A%A0%E5%BD%93%E6%88%90%E5%90%91%E9%87%8F%E6%B1%82%E6%A8%A1%E5%86%8D%E5%B9%B3%E6%96%B9%EF%BC%89)

---



![[公式]](https://www.zhihu.com/equation?tex=%3D%5Cdfrac+%7B1%7D%7BN%7D%5Cleft%5C%7C+%5Cbegin%7Bbmatrix%7D+x_%7B1%7D%5ET+%5C%5Cx_%7B2%7D%5ET+%5C%5C%5Cvdots+%5C%5Cx_%7Bn%7D%5ET%5Cend%7Bbmatrix%7D%5Coverrightarrow+%7Bw%7D-%5Cbegin%7Bbmatrix%7D+y_%7B1%7D+%5C%5C+y_%7B2%7D+%5C%5C+%5Cvdots+%5C%5C+y_%7Bn%7D+%5Cend%7Bbmatrix%7D%5Cright%5C%7C+%5E%7B2%7D%5Cquad+%28%E4%BB%A4X+%3D+%5Cbegin%7Bbmatrix%7D+x_%7B1%7D%5ET+%5C%5Cx_%7B2%7D%5ET+%5C%5C%5Cvdots+%5C%5Cx_%7Bn%7D%5ET%5Cend%7Bbmatrix%7D%5Cquad+%5Cvec+y%3D%5Cbegin%7Bbmatrix%7D+y_%7B1%7D+%5C%5Cy_%7B2%7D+%5C%5C%5Cvdots+%5C%5Cy_%7Bn%7D%5Cend%7Bbmatrix%7D%29%5C%5C)

![[公式]](https://www.zhihu.com/equation?tex=%3D%5Cdfrac+%7B1%7D%7BN%7D%5Cleft%5C%7C+X%5Coverrightarrow+%7Bw%7D-%5Coverrightarrow+%7By%7D%5Cright%5C%7C+%5E2+%5Cquad+%28X%3AN%2A%28d%2B1%29+%5Cquad+w%3A%28d%2B1%29%2AN%5Cquad+y%3AN%2A1%29%5C%5C)

则转化为求如下解：

![[公式]](https://www.zhihu.com/equation?tex=%5Cmin_%7Bw%7DE_%7Bin%7D%5Cleft%28+w%5Cright%29+%3D%5Cmin_%7Bw%7D%5Cdfrac+%7B1%7D%7BN%7D%5Cleft%5C%7C+Xw-y%5Cright%5C%7C+%5E%7B2%7D%5C%5C)

当x的维度为一维时，方程的图像如下：

![img](https://pic4.zhimg.com/80/v2-afc88f22463f5f7614662f34044ea78b_720w.jpg)

<span style="color:Navy;font-weight:bold;background-color: Yellow">连续、可微的凹函数</span>

---

Task: find *W* lin such that ▽E in (*W* lin) =0

梯度为0的点就是我们要找到的最低点：

![img](https://pic1.zhimg.com/v2-f5b7a1365b3c550ce3ee8361af4f9398_b.jpg)![img](https://pic1.zhimg.com/80/v2-f5b7a1365b3c550ce3ee8361af4f9398_720w.jpg)

开始之前，复习一下向量运算：<span style="color:Navy;font-weight:bold;background-color: Yellow">向量的平方 = 向量模的平方</span>，即：

- (a+b)^2 = |a+b|^2;
- (a-b)^2=|a-b|^2
- (a+b)^2 = a^2+2ab+b^2=a•a+2a•b+b•b
- (a-b)^2 = a^2-2ab+b^2=a•a-2a•b+b•b
- 等号两边均为标量

![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%5Cleft%28+w%5Cright%29+%3D%5Cdfrac+%7B1%7D%7BN%7D%5Cleft%7C+X%5Coverrightarrow+%7Bw%7D-%5Coverrightarrow+%7By%7D%5Cright%7C+%5E%7B2%7D%5C%5C+%3D%5Cdfrac+%7B1%7D%7BN%7D%5Cleft%28+%5Cleft%28+Xw%5Cright%29+%5Cleft%28+Xw%5Cright%29+-2%5Cleft%28+Xw%5Cright%29+y%2Byy%5Cright%29+%5C%5C+%3D%5Cdfrac+%7B1%7D%7BN%7D%5Cleft%28+%5Cleft%28+Xw%5Cright%29+%5E%7BT%7D%5Cleft%28+Xw%5Cright%29+-2%5Cleft%28+Xw%5Cright%29+%5E%7BT%7Dy%2By%5E%7BT%7Dy%5Cright%29+%5C%5C+%3D%5Cdfrac+%7B1%7D%7BN%7D%5Cleft%28+w%5E%7BT%7DX%5E%7BT%7DXw-2w%5E%7BT%7DX%5E%7BT%7Dy%2By%5E%7BT%7Dy%5Cright%29+%5C%5C+%3D%5Cdfrac+%7B1%7D%7BN%7D%5Cleft%28+w%5E%7BT%7DAw-2w%5E%7BT%7Db%2Bc%5Cright%29+)

---


$$
\begin{align*}
X^{T}X \: as\: A, X^{T}y\: as\: b, y^{T}y \: as\:  c,
Next \:Get \: derivative \:of \:E_{in}

\end{align*}
$$
对向量求导数比较难理解，下图以标量和向量为比较，求梯度

![img](https://pic2.zhimg.com/v2-7b8d8baa9f326a60634e75b274c24375_b.jpg)

![img](https://pic4.zhimg.com/v2-6d24dcf9e2b82d7bde8b65a40222f51f_b.jpg)

<img src="https://www.zhihu.com/equation?tex=%E6%B1%82%E8%A7%A3%EF%BC%9A%5Cnabla+E_%7Bin%7D%5Cleft%28+w%5Cright%29+%3D%5Cdfrac+%7B2%7D%7BN%7D%5Cleft%28+x%5E%7BT%7Dxw-x%5E%7BT%7Dy%5Cright%29+%3D0%5C%5C%5CRightarrow+%5Cdfrac+%7B2%7D%7BN%7D%5Cleft%28+x%5E%7BT%7Dxw-x%5E%7BT%7Dy%5Cright%29+%3D0.%5C%5C+%5CRightarrow+x%5E%7BT%7Dxw%3Dx%5E%7BT%7Dy%5C%5C+%5CRightarrow%5Cleft%28+x%5E%7BT%7Dx%5Cright%29+%5E%7B-1%7D%5Cleft%28+x%5E%7BT%7Dx%5Cright%29+%5Comega%3D%5Cleft%28+x%5E%7BT%7Dx%5Cright%29+%5E%7B-1%7Dx%5E%7BT%7Dy%5C%5C+%5CRightarrow+w+%3D%5Cleft%28+x%5E%7BT%7Dx%5Cright%29%5E%7B-1%7D+x%5E%7BT%7Dy" alt="[公式]" style="zoom:150%;" />

<u>**其中， ![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+x%5E%7BT%7Dx%5Cright%29%5E%7B-1%7D+x%5E%7BT%7D) 叫做矩阵X的伪逆（pseudo-inverse）** ![[公式]](https://www.zhihu.com/equation?tex=X%5E%E2%80%A0)</u> ，注意此处输入矩阵X在很少的情况下才是方阵（N=d+1时）。

而这种伪逆矩阵的形式和方阵中的逆矩阵具有很多相似的性质，因此才有此名称。 ![[公式]](https://www.zhihu.com/equation?tex=x%5ETx) 在大部分的情况下是可逆的，原因是在进行机器学习时，通常满足 ![[公式]](https://www.zhihu.com/equation?tex=N%5Cgg+d%2B1)，即样本数量N远远大于样本的维度d加1。

## <span style="font-weight:bold;background-color: Tomato">**04.02 总结：求解线性回归的方法**</span>



- 构建输入矩阵X与输出向量y

![img](https://pic3.zhimg.com/v2-42d99c54055063a3b16e06928db2a966_b.jpg)

- 计算伪逆矩阵 ![[公式]](https://www.zhihu.com/equation?tex=X%5E%E2%80%A0) = ![[公式]](https://www.zhihu.com/equation?tex=%5Cleft%28+x%5E%7BT%7Dx%5Cright%29%5E%7B-1%7D+x%5E%7BT%7D)

![img](https://pic4.zhimg.com/v2-d7eae3784dd7611dfe4e9cab5f6541bf_b.jpg)

- 求解w

![img](https://pic3.zhimg.com/v2-dd05fcaccd900c630338debec384a1fa_b.jpg)



# 05 Logistic Regression

---

## 05.01Likelihood function

我们常常用**概率(Probability)** 来描述一个事件发生的可能性。

<span style="font-weight:bold;background-color: Tomato">而**似然性(Likelihood)** 正好反过来，意思是一个事件实际已经发生了，反推在什么参数条件下，这个事件发生的概率最大。</span>

用数学公式来表达上述意思，就是: 

- 已知参数 β 前提下，预测某事件 x 发生的条件概率为 ![[公式]](https://www.zhihu.com/equation?tex=P%28x+%7C+%5Cbeta%29) ; 
- 已知某个已发生的事件 x，未知参数 β 的似然函数为 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D%28%5Cbeta%7C+x%29) ； 
- 上面两个值相等，即: ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D%28%5Cbeta%7C+x%29%3DP%28x+%7C+%5Cbeta%29) 。

一个参数 β 对应一个似然函数的值，当 β 发生变化， ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D%28%5Cbeta%7C+x%29) 也会随之变化。当我们在取得某个参数的时候，似然函数的值到达了最大值，说明在这个参数下最有可能发生x事件，即这个参数最合理。

**因此，最优β，就是使当前观察到的数据出现的可能性最大的β。**

---



## 05.02 Maxinum Likelihood function

在二分类问题中，y只取0或1，可以组合起来表示y的概率: ![[公式]](https://www.zhihu.com/equation?tex=P%28y%29%3DP%28y%3D1%29%5E%7By%7DP%28y%3D0%29%5E%7B1-y%7D) 

我们可以把y=1代入上式验证下, 左边是P(y=1), 右边是 ![[公式]](https://www.zhihu.com/equation?tex=P%28y%3D1%29%5E1P%28y%3D0%29%5E0) ，也为P(y=1)。

上面的式子，更严谨的写法需要加上特征x和参数β： 

<img src="https://www.zhihu.com/equation?tex=P%28y%7Cx%2C%5Cbeta%29%3DP%28y%3D1%7Cx%2C%5Cbeta%29%5E%7By%7D%5B1-P%28y%3D1%7Cx%2C%5Cbeta%29%5D%5E%7B1-y%7D" alt="[公式]" style="zoom:150%;" /> 



前面说了， <img src="https://www.zhihu.com/equation?tex=%5Cfrac%7B1%7D%7B1%2Be%5E%7B-x%7B%5Cbeta%7D%7D%7D" alt="[公式]"  /> 表示的就是P(y=1)，代入上式：<img src="https://www.zhihu.com/equation?tex=P%28y%7Cx%2C%5Cbeta%29%3D%5Cleft%28%5Cfrac%7B1%7D%7B1%2Be%5E%7B-+x%7B%5Cbeta%7D+%7D%7D%5Cright%29%5E%7By%7D%5Cleft%281-%5Cfrac%7B1%7D%7B1%2Be%5E%7B-x%7B%5Cbeta%7D%7D%7D%5Cright%29%5E%7B1-y%7D" alt="[公式]" style="zoom:150%;" /> 

根据上一小节说的最优β的定义，也就是最大化我们见到的样本数据的概率，即求下式的最大值。 

<img src="https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D%28%5Cbeta%29%3D%5Cprod_%7Bi%3D1%7D%5E%7Bn%7DP%28y_%7Bi%7D%7Cx_%7Bi%7D%2C%5Cbeta%29%3D%5Cprod_%7Bi%3D1%7D%5E%7Bn%7D%5Cleft%28%5Cfrac%7B1%7D%7B1%2Be%5E%7B+-%7Bx%7D%7Bi%7D%7B%5Cbeta%7D+%7D%7D%5Cright%29%5E%7By%7Bi%7D%7D%5Cleft%281-%5Cfrac%7B1%7D%7B1%2Be%5E%7B-%7Bx%7D_%7Bi%7D%7B%5Cbeta%7D%7D%7D%5Cright%29%5E%7B1-y_i%7D" alt="[公式]" style="zoom:150%;" /> 

这个式子怎么来的呢？其实很简单。

<span style="font-weight:bold;background-color: Tomato">前面我们说了， ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D%28%5Cbeta%7C+x%29%3DP%28x+%7C+%5Cbeta%29) ，对于某个观测值yi，似然函数的值 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BL%7D%28%5Cbeta%7C+y_%7Bi%7D%29) ，就等于条件概率的值 ![[公式]](https://www.zhihu.com/equation?tex=P%28y_%7Bi%7D+%7C+%5Cbeta%29) 。</span>

<span style="font-weight:bold;background-color: Tomato">另外我们知道，如果事件A与事件B相互独立，那么两者同时发生的概率为P(A)*P(B)。那么我们观测到的y1,y2……yn，他们同时发生的概率就是 <img src="https://www.zhihu.com/equation?tex=%5Cprod_%7Bi%3D1%7D%5E%7Bn%7DP%28y_%7Bi%7D%7C%5Cbeta%29" alt="[公式]" style="zoom:150%;" /> 。</span>

因为xi和yi都是我们实际观测到的数据，未知的只有β。

<span style="font-weight:bold;background-color: Tomato">现在问题就变成了**求β在取什么值的时候，L(β)能达到最大值。**</span>  L(β)是所有观测到的y发生概率的乘积，这种情况求最大值比较麻烦，一般会先取对数，将乘积转化成加法。取对数后，转化成下式：

<img src="https://www.zhihu.com/equation?tex=log%5Cmathcal%7BL%7D%28%5Cbeta%29%3D+%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%5Cleft%28%5By_%7Bi%7D%5Ccdot+log%28%5Cfrac%7B1%7D%7B1%2Be%5E%7B+-%7Bx%7D_%7Bi%7D%7B%5Cbeta%7D%7D%7D%29%5D%2B%5B%281-y_%7Bi%7D%29+%5Ccdot+log%281-%5Cfrac%7B1%7D%7B1%2Be%5E%7B-%7Bx%7D_%7Bi%7D%7B%5Cbeta%7D%7D%7D%29%5D%5Cright%29" alt="[公式]" style="zoom:150%;" /> 

接下来想办法求上式的最大值就可以了，求解前，我们要提一下逻辑回归的损失函数。



## 05.03 loss function 

在机器学习领域，总是避免不了谈论损失函数这一概念。**损失函数是用于衡量预测值与实际值的偏离程度，即模型预测的错误程度**。也就是说，这个值越小，认为模型效果越好，举个极端例子，如果预测完全精确，则损失函数值为0。

在线性回归中用到的损失函数是残差平方和SSE： ![[公式]](https://www.zhihu.com/equation?tex=Q%3D%5Csum_%7B1%7D%5E%7Bn%7D%5Cleft%28y_%7Bi%7D-%5Chat%7By%7D_%7Bi%7D%5Cright%29%5E%7B2%7D%3D%5Csum_%7B1%7D%5E%7Bn%7D%5Cleft%28y_%7Bi%7D-%7Bx%7D_%7Bi%7D%7B%5Cbeta%7D%5Cright%29%5E%7B2%7D) , 这是个凸函数，有全局最优解。

如果逻辑回归也用平方损失，那么就是： ![[公式]](https://www.zhihu.com/equation?tex=Q%3D%5Csum_%7B1%7D%5E%7Bn%7D%5Cleft%28y_%7Bi%7D-%5Cfrac%7B1%7D%7B1%2Be%5E%7B+-%7Bx%7D_%7Bi%7D%7B%5Cbeta%7D%7D%7D%5Cright%29%5E%7B2%7D) <span style="font-weight:bold;background-color: Tomato">很遗憾，这个不是凸函数，不易优化，容易陷入局部最小值，</span>所以逻辑函数用的是别的形式的函数作为损失函数，叫**对数损失函数**（log loss function)。 <span style="font-weight:bold;background-color: Tomato">这个对数损失，就是上一小节的**似然函数取对数后，再取相反数**</span>：

<img src="https://www.zhihu.com/equation?tex=J%28%5Cbeta%29+%3D-log%5Cmathcal%7BL%7D%28%5Cbeta%29%3D-+%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%5Cleft%5By_ilogP%28y_i%29%2B%281-y_i%29log%281-P%28y_i%29%29%5Cright%5D" alt="[公式]" style="zoom:150%;" /> 

这个对数损失函数好理解吗？我还是举个具体例子吧。

用文章开头那个例子，假设我们有一组样本，建立了一个逻辑回归模型P(y=1)=f(x)，其中一个样本A是这样的：

公司花了x=1000元做广告定向投放，某个用户看到广告后购买了，此时实际的y=1，f(x=1000)算出来是0.6，这里有-0.4的偏差，是吗？在逻辑回归中不是用差值计算偏差哦，用的是对数损失，所以它的偏差定义为log0.6（其实也很好理解为什么取对数，因为我们算的是P(y=1)，如果算出来的预测值正好等于1，那么log1=0，偏差为0）。

样本B：x=500，y=0，f(x=500)=0.3，偏差为log(1-0.3)=log0.7。

根据log函数的特性，自变量取值在[0,1]间，log出来是负值，而损失一般用正值表示，所以要取个相反数。因此A和B的总损失就是：**(-log0.6-log0.7)**。

之前我们在[用人话讲明白梯度下降](https://zhuanlan.zhihu.com/p/137713040)中解释过梯度下降算法，下面我们就用梯度下降法求损失函数的最小值（也可以用梯度上升算法求似然函数的最大值，这两是等价的）。



![img](https://d2nchlq0f2u6vy.cloudfront.net/13/09/01/ddbb51af3a8414363d9ef19ebe11e294/4cfe2c68fa08c4524545bed9f8cf7ae1/inversegraph.gif)





## 05.04 梯度下降法求解

首先看，对于sigmoid函数 ![[公式]](https://www.zhihu.com/equation?tex=f%28x%29%3D%5Cfrac%7B1%7D%7B1%2Be%5E%7B-x%7D%7D) ，f'(x)等于多少？

如果你还记得导数表中这2个公式，那就好办了（不记得也没关系，这就给你列出来）： 

![[公式]](https://www.zhihu.com/equation?tex=%28%5Cfrac%7B1%7D%7Bx%7D%29%5E%7B%5Cprime%7D%3D-%5Cfrac%7B1%7D%7Bx%5E%7B2%7D%7D)                 ||                   ![[公式]](https://www.zhihu.com/equation?tex=%28e%5E%7Bx%7D%29%5E%7B%5Cprime%7D%3De%5E%7Bx%7D) 

---

根据上两个公式，推导： ![[公式]](https://www.zhihu.com/equation?tex=f%27%28x%29%3D%28%5Cfrac%7B1%7D%7B1%2Be%5E%7B-x%7D%7D%29%27%3D-%5Cfrac%7B%28e%5E%7B-x%7D%29%27%7D%7B%281%2Be%5E%7B-x%7D%29%5E%7B2%7D%7D%3D%5Cfrac%7Be%5E%7B-x%7D%7D%7B%281%2Be%5E%7B-x%7D%29%5E%7B2%7D%7D) .  



到这还不算完哦，我们发现 ![[公式]](https://www.zhihu.com/equation?tex=1-f%28x%29%3D%5Cfrac%7Be%5E%7B-x%7D%7D%7B1%2Be%5E%7B-x%7D%7D) ，而f'(x)正好可以拆分为 ![[公式]](https://www.zhihu.com/equation?tex=%5Cfrac%7Be%5E%7B-x%7D%7D%7B1%2Be%5E%7B-x%7D%7D%5Ccdot%5Cfrac%7B1%7D%7B1%2Be%5E%7B-x%7D%7D) ，也就是说： 

![[公式]](https://www.zhihu.com/equation?tex=f%27%28x%29%3Df%28x%29%5Ccdot%281-f%28x%29%29) .



当然，现在我们的x是已知的，未知的是β，所以后面是对β求导，记： ![[公式]](https://www.zhihu.com/equation?tex=%5Cfrac%7B1%7D%7B1%2Be%5E%7B-x_i%5Cbeta%7D%7D%3Df%28x_i%5Cbeta%29+) 



把它代入前面我们得到逻辑回归的损失函数： 

<img src="https://www.zhihu.com/equation?tex=J%28%5Cbeta%29+%3D-+%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%5Cleft%5By_ilog%28f%28x_i%5Cbeta%29%29%2B%281-y_i%29log%281-f%28x_i%5Cbeta%29%29%5Cright%5D%3D-+%5Csum_%7Bi%3D1%7D%5E%7Bn%7Dg%28%5Cbeta_i%2Cx_i%2Cy_i%29" alt="[公式]" style="zoom:150%;" /> 

简便起见，先撇开求和号看g(β,x,y)。不过这个g(β,x,y)里面也挺复杂的，我们再把里面的 ![[公式]](https://www.zhihu.com/equation?tex=f%28x_i%5Cbeta%29) 挑出来，单独先看它对β向量中的某个βj求偏导是什么样。

根据上面的求导公式，有： <img src="https://www.zhihu.com/equation?tex=%5Cfrac%7B%5Cpartial+f%28x_i%5Cbeta%29+%7D%7B%5Cpartial+%5Cbeta_%7Bj%7D%7D+%3D+f%28x_i%5Cbeta%29%5Ccdot%281-f%28x_i%5Cbeta%29%29%5Ccdot+x_%7Bij%7D" alt="[公式]" style="zoom:150%;" /> 

注意咯，这个xi实际上指的是第i个样本的特征向量，即 ![[公式]](https://www.zhihu.com/equation?tex=%281%2C+x_%7Bi1%7D%2C+...%2Cx_%7Bip%7D%29) ，其中只有xij会和βj相乘，因此求导后整个xi只剩xij了。

理解了前面说的，下面的化简就轻而易举（知乎不能正常显示这个公式，只好写完转成图片粘过来）： 

![img](https://pic2.zhimg.com/v2-ce67a6f59a69426719d2b23a007252b5_b.jpg)

加上求和号： 

<img src="https://www.zhihu.com/equation?tex=%5Cfrac%7B%5Cpartial+J%28%5Cbeta%29%7D%7B%5Cpartial+%5Cbeta_%7Bj%7D%7D+%3D+-+%5Csum_%7Bi%3D1%7D%5E%7Bn%7D+%5Cleft%28y_i-f%28x_i%5Cbeta%29%5Cright%29+%5Ccdot+x_%7Bij%7D+%3D++%5Csum_%7Bi%3D1%7D%5E%7Bn%7D+%5Cleft%28%5Cfrac%7B1%7D%7B1%2Be%5E%7B-x_i%5Cbeta%7D%7D-y_i%5Cright%29+%5Ccdot+x_%7Bij%7D" alt="[公式]" style="zoom:150%;" /> 

有了偏导，也就有了梯度G，即偏导函数组成的向量。

 <span style="font-weight:bold;background-color: Tomato">**梯度下降算法过程：** </span>

1. 初始化β向量的值，即 ![[公式]](https://www.zhihu.com/equation?tex=%5CTheta_%7B0%7D) ，将其代入G得到当前位置的梯度； 
2. 用步长α乘以当前梯度，得到从当前位置下降的距离； 
3.  更新 ![[公式]](https://www.zhihu.com/equation?tex=%5CTheta_1) ，其更新表达式为 ![[公式]](https://www.zhihu.com/equation?tex=%5CTheta_1%3D%5CTheta_0-%5Calpha+G) ； 
4.  重复以上步骤，直到更新到某个 ![[公式]](https://www.zhihu.com/equation?tex=%5CTheta_k) ，达到停止条件，这个 ![[公式]](https://www.zhihu.com/equation?tex=%5CTheta_k) 就是我们求解的参数向量。

## 05.05 Flow of Logistic R

**Classifier：输入训练集是病人的信息，标记是得病与否，要求目标函数判断得病的概率**

首先根据要求设计假设函数如下：

- Target Function： ![[公式]](https://www.zhihu.com/equation?tex=f%28x%29%3DP%28%2B1%7Cx%29%5Cin%5Cleft%5B+0%2C1+%5Cright%5D) / ![[公式]](https://www.zhihu.com/equation?tex=P%28Y%3D1%29%3D%5Cfrac%7B1%7D%7B1%2Be%5E%7B-X%5Cbeta%7D%7D)
- 求各属性的加权总分 ![[公式]](https://www.zhihu.com/equation?tex=w%5ETx) 
- 将分数转化为0-1的值，利用Sigmond 函数 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta%28s%29%3D%5Cfrac%7Be%5Es%7D%7B1%2Be%5Es%7D%3D%5Cfrac%7B1%7D%7B1%2Be%5E%7B-s%7D%7D) （是一个平滑/处处可微分,单调递增的S形（sigmoid)函数，即sigmoid function)

![img](https://pic3.zhimg.com/v2-0254dfceb6c4a2e94a86704a8b7a8a2a_b.jpg)![img](https://pic3.zhimg.com/80/v2-0254dfceb6c4a2e94a86704a8b7a8a2a_720w.jpg)

- <img src="https://www.zhihu.com/equation?tex=h%3D%5Ctheta%28w%5Etx%29%3D%5Cfrac%7B1%7D%7B1%2Be%5E%7B-w%5Etx%7D%7D" alt="[公式]" style="zoom:150%;" /> 
- 求最小误差函数（损失函数）

<img src="https://pic1.zhimg.com/v2-28d7aaf5a40f73cf69b2032932ebfd20_b.jpg" alt="img" style="zoom:150%;" /> 

pla 线性分类 Logistic Regression

转换一下目标函数的表现形式：

<img src="https://pic2.zhimg.com/v2-4ce556db503a7b8d9cb05b3740b09ed9_b.jpg" alt="img" style="zoom:150%;" />

假设有训练集 ![[公式]](https://www.zhihu.com/equation?tex=D%3D%5Cleft%5C%7B+%28x_1%2C%5Ccirc%29%2C%28x_2%2C%5Ctimes%29+...%28x_n%2C%5Ctimes%29%5Cright%5C%7D) ，则通过目标函数产生此样本的概率是(g=likelihood(h)~f ：g取h的最大似然 => g= argmax likelihood(h))： 

<img src="https://www.zhihu.com/equation?tex=P%28D%29%3DP%28x_1%29P%28%5Ccirc%7Cx_1%29%2AP%28x_2%29P%28%5Ctimes%7Cx_2%29...%2AP%28x_n%29P%28%5Ctimes%7Cx_n%29%5C%5C+%3DP%28x_1%29f%28x_1%29%2AP%28x_2%29%281-f%28x_2%29%29...%2AP%28x_n%29%281-f%28x_n%29%29%5C%5C+%5Capprox+P%28x_1%29h%28x_1%29%2AP%28x_2%29%281-h%28x_2%29%29...%2AP%28x_n%29%281-h%28x_n%29%29%5C%5C+%3D+P%28x_1%29h%28x_1%29%2AP%28x_2%29h%28-x_2%29...%2AP%28x_n%29h%28-x_n%29%5Cquad+%28%E5%AF%B9%E7%A7%B0%E6%80%A7%EF%BC%9A1-h%28x%29%3Dh%28-x%29%29%5C%5C+" alt="[公式]" style="zoom:150%;" /> 

注意：其中上式中P（xn）对h的选择没有影响，所有的h都是一样的

则有： ![[公式]](https://www.zhihu.com/equation?tex=likelihood%28logistic%5Cquad+h%29%5Cpropto%5Cprod_%7Bn%3D1%7D%5E%7BN%7Dh%28y_nx_n%29) =>找到一个h，让这个可能性最大（最大似然）

因为我们已经有 ![[公式]](https://www.zhihu.com/equation?tex=h%3D%5Ctheta%28w%5Etx%29%3D%5Cfrac%7B1%7D%7B1%2Be%5E%7B-w%5Etx%7D%7D) ，所以我们要求最大值时的w，如下：

![[公式]](https://www.zhihu.com/equation?tex=max%28w%29%5Cquad+likelihood%28logistic%5Cquad+w%29%5Cpropto%5Cprod_%7Bn%3D1%7D%5E%7BN%7D%5Ctheta%28y_nw%5ETx%29) 

![[公式]](https://www.zhihu.com/equation?tex=max+%5Cprod_%7Bn%3D1%7D%5E%7BN%7D%5Ctheta%28y_nw%5ETx_n%29%5C%5C+%5CRightarrow+max%5Cquad+ln%28%5Cprod_%7Bn%3D1%7D%5E%7BN%7D%5Ctheta%28y_nw%5ETx_n%29%29%28%E8%BF%9E%E5%8A%A0%E8%BD%AC%E8%BF%9E%E4%B9%98%29%5C%5C+%5CRightarrow+max%5Cquad+%5Csum_%7Bn%3D1%7D%5E%7BN%7D%7Bln%5Ctheta%28y_nw%5ETx_n%29%7D%5C%5C+%5CRightarrow+min%5Cquad+%5Csum_%7Bn%3D1%7D%5E%7BN%7D%7B-ln%5Ctheta%28y_nw%5ETx_n%29%7D%28%E6%9C%80%E5%A4%A7%E8%BD%AC%E6%9C%80%E5%B0%8F%29%5C%5C+%5CRightarrow+min%5Cquad+%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5E%7BN%7D%7B-ln%5Ctheta%28y_nw%5ETx_n%29%7D%28%E6%96%B9%E4%BE%BF%E4%B8%8B%E9%9D%A2%E6%93%8D%E4%BD%9C%29%5C%5C%5CRightarrow+min%5Cquad+%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5E%7BN%7D%7B-ln%28%5Cfrac%7B1%7D%7B1%2Bexp%28-y_nw%5ETx_n%29%7D%29%7D%28%E4%BB%A3%E5%85%A5%5Ctheta+%E8%A1%A8%E8%BE%BE%E5%BC%8F%29%5C%5C+%5CRightarrow+min%5Cquad+%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5E%7BN%7D%7Bln%28%7B1%2Bexp%28-y_nw%5ETx_n%29%7D%29%7D%5C%5C+%5CRightarrow+E_%7Bin%7D%28w%29%3D%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bn%3D1%7D%5E%7BN%7Derr%28w%2Cx_n%2Cy_n%29%5C%5C+%5CRightarrow+%E6%89%BE%E5%88%B0%E4%B8%80%E4%B8%AAw%E8%AE%A9E_%7Bin%7D%E6%9C%80%E5%B0%8F) 

接下来，求解方程

![img](https://pic3.zhimg.com/v2-6577666b19c9015aa05018322a9478ca_b.jpg)![img](https://pic3.zhimg.com/80/v2-6577666b19c9015aa05018322a9478ca_720w.jpg)

该函数为连续、可微、凹函数，因此其最小值在梯度为零时取得。

![img](https://pic1.zhimg.com/v2-b82c403d5c07d6eea60b1e223e98a0ac_b.jpg)

对权值向量w的各个分量求解偏微分：

![img](https://pic3.zhimg.com/v2-6d5778355fdb2b87fe4b8013e2551bd6_b.jpg)

Wi分量的梯度（偏微分）为：

![img](https://pic2.zhimg.com/v2-91b273e9ade237ebd55c5a67cd472605_b.jpg)![img](https://pic2.zhimg.com/80/v2-91b273e9ade237ebd55c5a67cd472605_720w.jpg)

Ein整个梯度统一表示为：

![img](https://pic4.zhimg.com/v2-17a1aaf7969cbe5334a735c878eb5cbb_b.jpg)

再观察此函数，发现该函数是以 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta) 函数为权值，对 ![[公式]](https://www.zhihu.com/equation?tex=-y_nx_n) 加权求和函数

联想到PLA算法，并将PLA的求解步骤改写成如： ![[公式]](https://www.zhihu.com/equation?tex=w_%7Bi%2B1%7D%3Dw_i%2B%5Cleft%5B+sign%28w_t%5ETx_n%29%5Cne+y_n+%5Cright%5Dy_nx_n+%5CRightarrow+%5C%5C+w_%7Bi%2B1%7D%3Dw_i%2B%5Ceta%5Cvec+v%5Cquad+%28%5Ceta%3A+%E6%AD%A5%E9%95%BF%EF%BC%8C%5Cvec+v%3A%E6%9B%B4%E6%96%B0%E6%96%B9%E5%90%91%29%EF%BC%88%E8%BF%AD%E4%BB%A3%E4%BC%98%E5%8C%96%EF%BC%89) 

对于逻辑斯蒂回归来说，v是梯度下降的方向， ![[公式]](https://www.zhihu.com/equation?tex=%5Ceta) 是每次下降的步长

为了简单计算，将步长 ![[公式]](https://www.zhihu.com/equation?tex=%5Ceta) 固定，将v作为单位向量仅代表方向，找出使得Ein(w)最小的w，即有： ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathop%7Bmin%7D%5Climits_%7B%7Cv%7C%3D1%7DE_%7Bin%7D%28w_t%2B%5Ceta+v%29) (非线性带约束)，将曲线看做很小的线段(泰勒展开（只对很小的线段步长适用）)，则有：

![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28w_t%2B%5Ceta+v%29%5Capprox+E_%7Bin%7D%28w_t%29%2B%5Ceta+v%5ET%5Cnabla+E_%7Bin%7D%28w_t%29%28%E6%B1%82v%E7%9A%84%E7%BA%BF%E6%80%A7%E5%BC%8F%29%5C%5C) 

因为 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28w_t%29%E6%98%AF%E5%B7%B2%E7%9F%A5%E5%80%BC%EF%BC%8C%5Ceta+%E4%B8%BA%E7%BB%99%E5%AE%9A%E7%9A%84%E5%A4%A7%E4%BA%8E0%E7%9A%84%E5%80%BC)，所以可以转换为一下求最小问题：

![[公式]](https://www.zhihu.com/equation?tex=%5Cmathop%7Bmin%7D%5Climits_%7B%7Cv%7C%3D1%7D%5Cquad+v%5ET%5Cnabla+E_%7Bin%7D%28w_t%29%5C%5C+) 

因为两个向量最小的情况为其方向相反，即乘积为负值，则v如下:

![[公式]](https://www.zhihu.com/equation?tex=v%3D-%5Cfrac%7B%5Cnabla+E_%7Bin%7D%28w_t%29%7D%7B%7C%7C%5Cnabla+E_%7Bin%7D%28w_t%29%7C%7C%7D+%28v%E6%98%AF%E5%8D%95%E4%BD%8D%E5%90%91%E9%87%8F%29%5C%5C)

将v代入 ![[公式]](https://www.zhihu.com/equation?tex=w_%7Bt%2B1%7D) 公式有：

![[公式]](https://www.zhihu.com/equation?tex=w_%7Bt%2B1%7D%3Dw_t-%5Ceta+%5Cfrac%7B%5Cnabla+E_%7Bin%7D%28w_t%29%7D%7B%7C%7C%5Cnabla+E_%7Bin%7D%28w_t%29%7C%7C%7D%5C%5C+) 

更新公式表示权值向量w每次向着梯度的反方向移动一小步，按照此种方式更新可以尽快速度找到Ein最小的w，此种方式称作**梯度下降**。接下来讨论 步长![[公式]](https://www.zhihu.com/equation?tex=%5Ceta) 对梯度下降的影响：

![img](https://pic2.zhimg.com/v2-a0368ccbd382351f2efe08ccd497ec71_b.jpg)

根据上面的w更新公式， 不同![[公式]](https://www.zhihu.com/equation?tex=%5Ceta)对w更新的结果可以如上图一二所示，较小的步长更新正确但较慢，较大的步长容易出错（不适用于泰勒展开） ，图三表示步长可变，与梯度大小成正比，以此我们有新的公式 ![[公式]](https://www.zhihu.com/equation?tex=%5Ceta_%7Bnew%7D+%3D%5Cfrac%7B%5Ceta_%7Bold%7D%7D%7B%7C%5Cnabla+E_%7Bin%7D%28w_t%29%7C%7D) ，再代入上面公式有：

![[公式]](https://www.zhihu.com/equation?tex=w_%7Bt%2B1%7D%3Dw_t-%5Ceta+%5Cnabla+E_%7Bin%7D%28w_t%29%5C%5C+) 

此时 ![[公式]](https://www.zhihu.com/equation?tex=%5Ceta) 固定，被称为学习速率。

**逻辑斯蒂回归算法总结如下：**

- w初始值w0，迭代次数为t
- 计算梯度

![img](https://pic4.zhimg.com/v2-5ca10ca163c3872960fc22fad5dafe13_b.jpg)





# 06 非线性转换

## **二次的假设函数**

如图，非线性可分的训练集，可称为圆圈可分（circular separable）：

![img](https://pic1.zhimg.com/80/v2-5a04f007075f622b993ac0a4d8b31030_720w.jpg)

假设函数为：![[公式]](https://www.zhihu.com/equation?tex=h_%7Bsep%7D%28x%29%3Dsign%28-x_1%5E2-x_2%5E2%2B0.6%29) ，将此假设函数转换为熟悉的线性模型：

![img](https://pic1.zhimg.com/80/v2-479eff6af8f1040dc7e884ec2f2faf08_720w.jpg)

点以上过程将圆圈可分转换为线性可分，这种输入空间转换的过程称为特征转换，就可以在z的世界里用线性的概念和模型进行操作。z中的每一条线不一定对应到x中的正圆曲线，可能是椭圆、双曲线、斜圆、退化二次曲线等等，如果想要表示X空间中所有的二次曲面，Z空间该佮表示呢？设计一个更大的Z空间，其特征转换如公式:

![[公式]](https://www.zhihu.com/equation?tex=%5Cphi_2%28x%29%3D%281%2Cx_1%2Cx_2%2Cx_1%5E2%2Cx_1x_2%2Cx%5E2%29%5C%5C)

通过以上特征转换，Z空间中的每个超平面就对应X空间中各种不同情况的二次曲线（包括直线和各种类型的二次曲线）和常数（全为正或全为负）。

**非线性转换的代价**

当使用Q次多项式转换时，转换函数需要的项数有：
$$
\Phi \left ( x \right ) =\begin{pmatrix}
  &1  &  &  & \\
  &x_{1}  &x_{2}  &...  &x_{d} \\
  &x_{1}^{2} &x_{1}x_{2}  &  &x_{d}^{2} \\
  & ... &  &  & \\
  &x_{1}^{Q}  &x_{1}^{Q-1}  & ... &x_{d}^{Q} 
\end{pmatrix}
$$
此时权值向量 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctilde%7Bw%7D%3D1%2B%5Ctilde%7Bd%7D) (1为常数项，d为所有组合项数的个数)，变换、维度与次数的关系：

![img](https://pic2.zhimg.com/80/v2-32fbf9dfd936b9aa4bf8ea6a2cde791d_720w.jpg)

仅做结论暂不做证明，Ein、Eout、复杂度的关系一般如下：



由图可知，在做多项式转换时，最好是从低次往高次依次测试，而不是开始选取一个高次的多项式作为转换函数，最好从一次式（也称为线性函数）开始，否则就会造成过拟合（见下一篇）。





























# 98 In-depth understanding







# 99 Supplementary

[岭大王]([岭大王 - 知乎 (zhihu.com)](https://www.zhihu.com/people/jerryd99))

[化简可得]([用人话讲明白逻辑回归Logistic regression - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/139122386))

[马冬什么](https://www.zhihu.com/column/c_1114213082062450688)

[线性回归-CSDN博客](https://blog.csdn.net/Sirow/article/details/109141844)

排版：

[文本居中]([LaTeX（1）设置部分文本居中左对齐、居中右对齐_一个瑞特的博客-CSDN博客_latex 右对齐](https://blog.csdn.net/qq_40675882/article/details/84556527))

[排版大全]([Typora数学公式汇总（Markdown） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/261750408?utm_source=wechat_session))



