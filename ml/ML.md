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



## 05.02 Maximum Likelihood function

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





# 07 Overfitting

---

## 07.01 overfitting basic

![img](https://www.researchgate.net/profile/Lean-Yu/publication/3297508/figure/fig4/AS:340309096583175@1458147419813/Overfitting-underfitting-and-model-complexity.png)

.

![errorrate.jpg](https://github.com/otainginga/Image/blob/main/errorrate.jpg?raw=true)





## 07.02 以开车为比喻：

- 机器学习 => 开车
- 过拟合 => 出车祸
- 使用过度的VC维 => 开得太快
- 噪音 => 颠簸的路面
- 数据量的大小 => 对路面状况的观察

**噪音与数据量对过拟合的作用**

首先，我们有两份数据，这两份数据由**10次多项式和50次多项式**的目标函数产生，前者有噪音，后者没有噪音，数据点如图所示：

![img](https://pic4.zhimg.com/80/v2-e44906e8d9175a0fc52b548b5ff7384b_720w.jpg)

**train set A、train set B**

1、使用两种学习模型（二次式假设空间 ![[公式]](https://www.zhihu.com/equation?tex=H_2) 和十次多项式假设空间 ![[公式]](https://www.zhihu.com/equation?tex=H_%7B10%7D) ）对训练集A\B进行学习得到的假设函数g和错误率如下：

![img](https://pic4.zhimg.com/80/v2-d0a6d599f008b576ec05490199d2f36f_720w.jpg)

为什么二次式模型与目标函数的次数有很大差距反而比10次多项式模型学习能力还好？这与训练样本量有关，如下图：

![img](https://pic4.zhimg.com/80/v2-76afb43894798f6b34059ba8d768f3f3_720w.jpg)

从图中可以看出，数据量少时，尽管2次假设的Ein比10次函数的Ein大很多，但是2次假设中Ein和Eout的差距比10次假设小的多，因此在样本点不多时，低次假设的学习泛化能力更强，即在灰色区域（样本不多的情况下）中，高次假设函数发生了过拟合。

上述阐述了在含有噪音的情况下，低次多项式假设比和目标函数同次的多项式假设表现更好，那如何解释在50次多项式函数中也是二次式表现好的现象呢？**因为50次目标函数对于不论是2次假设还是10次假设都相当于一种含有噪音的情况（两者都无法做到50次的目标函数，因此相当于含有噪音）。**

注：噪音的进一步理论阐述暂不描述。（噪音也是造成过拟合的重要原因）

## 07.03 **如何处理过拟合**

再以开车为例：

- 从简单的模型出发  => 开慢点
- 数据清理/裁剪（data cleaning/pruning） => 更准确的路况信息
- 数据提示（data hinting） => 获取更多的路况信息
- 正则化（regularization） => 踩刹车
- 验证（validation） => 安装仪表盘

# <*>**Regularization**(正则化)

---



防止过拟合的一种有效措施即是正则化假设。

**正则化假设**

将假设函从高次多项式的数降至低次，如同开车时的踩刹车，将速度降低，如下图所示，右边表示高次多项式函数，明显产生了过拟合现象，而左边的表示使用正则化后的低次函数。

![img](https://pic4.zhimg.com/80/v2-3755ce0a187a1f58815e118760d0aeff_1440w.jpg)

如何降次呢？把降次的问题转换成带**有限制（constraint）**条件的问题。下面以10次多项式和二次式为例
$$
\begin{aligned}  
&H_{10}: w_{0}+ w_{1}x+ w_{2}x^{2}...+ w_{10}x^{10}\\
&H_{2}: w_{0}+ w_{1}x+ w_{2}x^{2}
\end{aligned}
$$
其中二次式可以转化为：
$$
\begin{aligned} 
&H_{2}\equiv  \left \{ 
\mathbb{R}^{10+1} \\ 

while \: w_{3}=...=w_{10}=0 \right \} \\


&extending \:  to \\


&H'_{2}\equiv \left \{  \mathbb{R}^{10+1} \\ 

\:while \ge 8 \: of \: W_{q}=0, \\
number\: of\: w\: being \:0 \:\ge \:8
 \right \}
 \end{aligned}
$$


假设空间之间的关系为： ![[公式]](https://www.zhihu.com/equation?tex=H_2%5Csubset+H_2%5E%7B%27%7D%5Csubset+H_%7B10%7D)

由于 ![[公式]](https://www.zhihu.com/equation?tex=H_2%5E%7B%27%7D) 的minEin

![img](https://pic2.zhimg.com/80/v2-a6c30d4b20c98e4aeadb68dc03c26a11_1440w.jpg)

是一个NP-hard问题，所以将假设空间再次改写为（权值向量w的模的平方小于C）：

![img](https://pic3.zhimg.com/80/v2-a8e893e804ec70b77ecb97ac36bce576_1440w.jpg)

![img](https://pic3.zhimg.com/80/v2-8490d503cb3c5cb87d12641bdb20bada_1440w.jpg)

（此时可能w的分向量个数大于3，暂时不要问为啥，我也不知道为啥）H(c)为正则化假设空间，即假设限制条件的假设空间。正则化假设空间中最好的假设用符号 ![[公式]](https://www.zhihu.com/equation?tex=w_%7BREG%7D) 表示。

**权值衰减正则化**

上节中得到的minEin可以表达为：

![img](https://pic2.zhimg.com/80/v2-06ab8a19082b4bbfde287b695e630bd1_1440w.jpg)

![img](https://pic2.zhimg.com/80/v2-d5c140c05099ece4b233f76124df7edd_1440w.jpg)

式子中![[公式]](https://www.zhihu.com/equation?tex=%28wz-y%29%5E2) 和 ![[公式]](https://www.zhihu.com/equation?tex=w%5E2) 在 ![[公式]](https://www.zhihu.com/equation?tex=R%5E%7Bq%2B1%7D) 空间中是如下图所示的两个超球体（椭圆球体和正圆球体）

![img](https://pic4.zhimg.com/80/v2-ac9d087861031965d8d47407e696332b_1440w.jpg)

爆炸性的东西来了！w的点要在球面上移动（不能超出球面），又同时要接近无限制条件下的最小点，那么，w的移动方向必须满足两个条件（红色向量即w方向是球面法向量）：1、移动方向是与球面法向量垂直，2、移动方向要是梯度反方向的一个分量向量；那何时降到最小？即梯度反方向（蓝色）不存在与球切线方向（绿色）相同的分量，只有当梯度反方向与法向量平行时，才无法得到下一个方向，即达到了最优点，两向量平行可以得出公式（也即拉格朗日函数的几何意义）：

![[公式]](https://www.zhihu.com/equation?tex=a%3D%5Clambda+b%5C%5C+%5CRightarrow+-%5Cnabla+E_%7Bin%7D%28w_%7BREG%7D%29%3D%5Clambda+w_%7BREG%7D%5C%5C+%5CRightarrow+%5Cnabla+E_%7Bin%7D%28w_%7BREG%7D%29%2B%5Clambda+w_%7BREG%7D%3D0%5C%5C+%5CRightarrow+%5Cnabla+E_%7Bin%7D%28w_%7BREG%7D%29%2B%5Cfrac%7B2%5Clambda%7D%7BN%7Dw_%7BREG%7D%3D0%5C%5C)

将线性回归中求的 ![[公式]](https://www.zhihu.com/equation?tex=%5Cnabla+E_%7Bin%7D%28w_%7BREG%7D%29) 代入，则有：

![img](https://pic4.zhimg.com/80/v2-f174ec7e6a1ab1824a48b80d89fc37f3_1440w.jpg)

![img](https://pic3.zhimg.com/80/v2-eb65e7550bf33cf32ede12bdba6ff50e_1440w.jpg)又称为：岭回归 啊哈~岭大王

考虑一下式子 ![[公式]](https://www.zhihu.com/equation?tex=%5Cnabla+E_%7Bin%7D%28w_%7BREG%7D%29%2B%5Cfrac%7B2%5Clambda%7D%7BN%7Dw_%7BREG%7D) ，做积分得到 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Bin%7D%28w%29%2B%5Cfrac%7B%5Clambda%7D%7BN%7Dw%5ETw) ，我们定义 ![[公式]](https://www.zhihu.com/equation?tex=E_%7Baug%7D%28w%29%3DE_%7Bin%7D%28w%29%2B%5Cfrac%7B%5Clambda%7D%7BN%7Dw%5ETw) 为增广错误（augmented error）， ![[公式]](https://www.zhihu.com/equation?tex=w%5ETw) 为正则化项（regularizer），所以我们神奇的用无约束条件的求解Ein代替了有约束条件的求解Ein！

我们的最终求解公式可以表示为：

![img](https://pic1.zhimg.com/80/v2-88ca3b084eec6de6339fbcef0c79a63c_1440w.jpg)

下图为不同 ![[公式]](https://www.zhihu.com/equation?tex=%5Clambda) 对拟合的影响：

![img](https://pic2.zhimg.com/80/v2-c6e524925dc2cde571cdaefd37427755_1440w.jpg)

越大的 对应着越短的权值向量w，同时也对应着越小的约束半径C

**总结**

上面讨论称为L2正则化：

![img](https://pic3.zhimg.com/80/v2-05fe9afe6e1dab7ae480f23e9c9977f2_1440w.jpg)

还有L1正则化：

![img](https://pic3.zhimg.com/80/v2-6def1309e04d62686992ec670a05e8aa_1440w.jpg)

![img](https://pic2.zhimg.com/80/v2-5b6daaa4ad30f4de998aea3ef2553fad_1440w.jpg)







# 15 Decision Tree

---

　　决策树算法在机器学习中算是很经典的一个算法系列了。它既可以作为分类算法，也可以作为回归算法，同时也特别适合集成学习比如随机森林。本文就对决策树算法原理做一个总结，上篇对ID3， C4.5的算法思想做了总结，下篇重点对CART算法做一个详细的介绍。选择CART做重点介绍的原因是scikit-learn使用了优化版的CART算法作为其决策树算法的实现。

## 15.01 ID3

### 15.01.01 决策树ID3算法的信息论基础

　　　　机器学习算法其实很古老，作为一个码农经常会不停的敲if, else if, else,其实就已经在用到决策树的思想了。只是你有没有想过，有这么多条件，用哪个条件特征先做if，哪个条件特征后做if比较优呢？怎么准确的定量选择这个标准就是决策树机器学习算法的关键了。1970年代，一个叫昆兰的大牛找到了用信息论中的熵来度量决策树的决策选择过程，方法一出，它的简洁和高效就引起了轰动，昆兰把这个算法叫做ID3。下面我们就看看ID3算法是怎么选择特征的。

　　　　首先，我们需要熟悉信息论中熵的概念。熵度量了事物的不确定性，越不确定的事物，它的熵就越大。具体的，随机变量X的熵的表达式如下：
$$
H(X)=\sum_{n}^{i=1} p_{i}log p_{i}
$$
　　　　其中n代表X的n种不同的离散取值。而pi代表了X取值为i的概率，log为以2或者e为底的对数。举个例子，比如X有2个可能的取值，而这两个取值各为1/2时X的熵最大，此时X具有最大的不确定性。值为
$$
H(X)=-\left (\frac{1}{2}log\frac{1}{2}+  \frac{1}{2}log\frac{1}{2} \right)=log2
$$
如果一个值概率大于1/2，另一个值概率小于1/2，则不确定性减少，对应的熵也会减少。比如一个概率1/3，一个概率2/3，则对应熵为
$$
H(X)=-\left (\frac{1}{3}log\frac{1}{3}+  \frac{2}{3}log\frac{2}{3} \right)=log3-\frac{2}{3}log2<log2
$$
　　　　熟悉了一个变量X的熵，很容易推广到多个个变量的联合熵，这里给出两个变量X和Y的联合熵表达式：
$$
H(X,Y)=-\sum_{x_{i}\in X}^{} \sum_{y_{i}\in Y}^{} p(x_{i}y_{i})logp(x_{i}y_{i})
$$
　　　　有了联合熵，又可以得到条件熵的表达式H(X|Y)，条件熵类似于条件概率,它度量了我们的X在知道Y以后剩下的不确定性。表达式如下：
$$
H(X|Y)=-\sum_{x_{i}\in X}^{} \sum_{y_{i}\in Y}^{} p(x_{i}|y_{i})logp(X|y_{i})=\sum_{j=1}^{n}p(y_{j})H(X|y_{j})
$$


　　　　好吧，绕了一大圈，终于可以重新回到ID3算法了。我们刚才提到<span style="font-weight:bold;background-color: Tomato">H(X)度量了X的不确定性，**条件熵H(X|Y)度量了我们在知道Y以后X剩下的不确定性**，那么H(X)-H(X|Y)呢？从上面的描述大家可以看出，它度量了X在知道Y以后不确定性减少程度，这个度量我们在信息论中称为互信息，，记为I(X,Y)</span>。在决策树ID3算法中叫做信息增益。ID3算法就是用信息增益来判断当前节点应该用什么特征来构建决策树。信息增益大，则越适合用来分类。

　　　　上面一堆概念，大家估计比较晕，用下面这个图很容易明白他们的关系。左边的椭圆代表H(X),右边的椭圆代表H(Y),中间重合的部分就是我们的互信息或者信息增益I(X,Y), 左边的椭圆去掉重合部分就是H(X|Y),右边的椭圆去掉重合部分就是H(Y|X)。两个椭圆的并就是H(X,Y)。

<img src="https://github.com/otainginga/Image/blob/main/hxy.png?raw=true" alt="hxy.png" style="zoom: 50%;" />

### 15.02 决策树ID3算法的思路

　　　　上面提到ID3算法就是用信息增益大小来判断当前节点应该用什么特征来构建决策树，用计算出的信息增益最大的特征来建立决策树的当前节点。这里我们举一个信息增益计算的具体的例子。比如我们有15个样本D，输出为0或者1。其中有9个输出为1， 6个输出为0。 样本中有个特征A，取值为A1，A2和A3。在取值为A1的样本的输出中，有3个输出为1， 2个输出为0，取值为A2的样本输出中,2个输出为1,3个输出为0， 在取值为A3的样本中，4个输出为1，1个输出为0.
$$
\begin{aligned} 
　&样本D的熵为：\\
　&H(D)\\
　&样本D在特征下的条件熵为：\\ 
　&H(D|A)\\
　&对应的信息增益为: I(D,A)=H(D)−H(D|A)=0.083　
　
\end{aligned}
$$


　　　　　　　　　　　　　

　　　　下面我们看看具体算法过程大概是怎么样的。
$$
输入的是m个样本，样本输出集合为D，每个样本有n个离散特征，特征集合即为A，输出为决策树T。\\
\begin{aligned} 
　　　　&算法的过程为：\\
　　　　&1)初始化信息增益的阈值\epsilon \\
　　　　&2）判断样本是否为同一类输出D_{i}，如果是则返回单节点树T。标记类别为D_{i}\\
　　　　&3) 判断特征是否为空，如果是则返回单节点树T，标记类别为样本中输出类别D实例数最多的类别。\\
　　　　&4）计算A中的各个特征（一共n个）对输出D的信息增益，选择信息增益最大的特征A_{g}\\
　　　　&5) 如果A_{g}的信息增益小于阈值\epsilon，则返回单节点树T，标记类别为样本中输出类别D实例数最多的类别。\\
　　　　&6）否则，按特征A_{g}的不同取值A_{gi}将对应的样本输出D分成不同的类别D_{i}。\\
　　　　&每个类别产生一个子节点。对应特征值为A_{gi}。返回增加了节点的数T。\\
　　　　&\\
　　　　&7）对于所有的子节点，令D=D_{i},A=A−{A_{g}}D=Di,A=A−{Ag}递归调用2-6步，\\
　　　　&得到子树T_{i}并返回。
　　　　\end{aligned}
$$




### 15.03 决策树ID3算法的不足

　　　　ID3算法虽然提出了新思路，但是还是有很多值得改进的地方。　　

1. 　　　　ID3没有考虑连续特征，比如**长度，密度都是连续值**，无法在ID3运用。这大大限制了ID3的用途。
2. 　　　　ID3采用**信息增益大的特征优先建立决策树**的节点。很快就被人发现，在相同条件下，取值比较多的特征比取值少的特征信息增益大。比如一个变量有2个值，各为1/2，另一个变量为3个值，各为1/3，其实他们都是完全不确定的变量，但是取3个值的比取2个值的信息增益大。如果校正这个问题呢？
3. 　　　　ID3算法对于缺失值的情况没有做考虑
4. 　　　　没有考虑过拟合的问题
5. 　　　　ID3 算法的作者昆兰基于上述不足，对ID3算法做了改进，这就是C4.5算法，也许你会问，为什么不叫ID4，ID5之类的名字呢?那是因为决策树太火爆，他的ID3一出来，别人二次创新，很快 就占了ID4， ID5，所以他另辟蹊径，取名C4.0算法，后来的进化版为C4.5算法。下面我们就来聊下C4.5算法

### 15.04 决策树C4.5算法的改进

　　　　上一节我们讲到ID3算法有四个主要的不足，一是不能处理连续特征，第二个就是用信息增益作为标准容易偏向于取值较多的特征，最后两个是缺失值处理的问和过拟合问题。昆兰在C4.5算法中改进了上述4个问题。

　　　　<span style="font-weight:bold;background-color: Tomato">对于不能处理连续特征， C4.5的思路是将连续的特征离散化。</span>比如m个样本的连续特征A有m个，从小到大排列为a1,a2,...,ama1,a2,...,am,则C4.5取相邻两样本值的平均数，一共取得m-1个划分点，其中第i个划分点Ti表示Ti表示为：Ti=ai+ai+12Ti=ai+ai+12。对于这m-1个点，分别计算以该点作为二元分类点时的信息增益。选择信息增益最大的点作为该连续特征的二元离散分类点。比如取到的增益最大的点为atat,则小于atat的值为类别1，大于atat的值为类别2，这样我们就做到了连续特征的离散化。要注意的是，与离散属性不同的是，如果当前节点为连续属性，则该属性后面还可以参与子节点的产生选择过程。

　　　　<span style="font-weight:bold;background-color: Tomato">对于第二个问题，信息增益作为标准容易偏向于取值较多的特征的问题。我们引入一个信息增益比的变量IR(X,Y)，它是信息增益和特征熵的比值。</span>span>表达式如下：
$$
I_{R}(D,A)=\frac{I(A,D)}{H_{A}(D)}
$$


　　　　其中D为样本特征输出的集合，A为样本特征，对于特征熵HA(D) 表达式如下：
$$
H_{A}(D) = -\sum_{i=1}^{n} \frac{|D_{i}|}{|D|}log2\frac{|D_{i}|}{|D|}\\
其中n为特征A的类别数， D_{i}为特征A的第i个取值对应的样本个数。|D|为样本个数。
$$


　　　　特征数越多的特征对应的特征熵越大，它作为分母，可以校正信息增益容易偏向于取值较多的特征的问题。

　　　　对于第三个缺失值处理的问题，主要需要解决的是两个问题，一是在样本某些特征缺失的情况下选择划分的属性，二是选定了划分属性，对于在该属性上缺失特征的样本的处理。

　　　　对于第一个子问题，对于某一个有缺失特征值的特征A。C4.5的思路是将数据分成两部分，对每个样本设置一个权重（初始可以都为1），然后划分数据，一部分是有特征值A的数据D1，另一部分是没有特征A的数据D2. 然后对于没有缺失特征A的数据集D1来和对应的A特征的各个特征值一起计算加权重后的信息增益比，最后乘上一个系数，这个系数是无特征A缺失的样本加权后所占加权总样本的比例。

　　　　对于第二个子问题，可以将缺失特征的样本同时划分入所有的子节点，不过将该样本的权重按各个子节点样本的数量比例来分配。比如缺失特征A的样本a之前权重为1，特征A有3个特征值A1,A2,A3。 3个特征值对应的无缺失A特征的样本个数为2,3,4.则a同时划分入A1，A2，A3。对应权重调节为2/9,3/9, 4/9。

 

　　　　对于第4个问题，C4.5引入了正则化系数进行初步的剪枝。具体方法这里不讨论。下篇讲CART的时候会详细讨论剪枝的思路。

　　　　除了上面的4点，C4.5和ID的思路区别不大。

　　　　

### 15.05 决策树C4.5算法的不足与思考

　　　　C4.5虽然改进或者改善了ID3算法的几个主要的问题，仍然有优化的空间。

1. 由于决策树算法非常容易过拟合，因此对于生成的决策树必须要进行剪枝。剪枝的算法有非常多，C4.5的剪枝方法有优化的空间。思路主要是两种，一种是预剪枝，即在生成决策树的时候就决定是否剪枝。另一个是后剪枝，即先生成决策树，再通过交叉验证来剪枝。后面在下篇讲CART树的时候我们会专门讲决策树的减枝思路，主要采用的是后剪枝加上交叉验证选择最合适的决策树。
2. C4.5生成的是多叉树，即一个父节点可以有多个节点。很多时候，在计算机中二叉树模型会比多叉树运算效率高。如果采用二叉树，可以提高效率。
3. C4.5只能用于分类，如果能将决策树用于回归的话可以扩大它的使用范围。
4. C4.5由于使用了熵模型，里面有大量的耗时的对数运算,如果是连续值还有大量的排序运算。如果能够加以模型简化可以减少运算强度但又不牺牲太多准确性的话，那就更好了。

　　　　这4个问题在CART树里面部分加以了改进。所以目前如果不考虑集成学习话，在普通的决策树算法里，CART算法算是比较优的算法了。<span style="font-weight:bold;background-color: Tomato">scikit-learn的决策树使用的也是CART算法</span>。

在下篇里我们会重点聊下CART算法的主要改进思路，上篇就到这里。下篇请看[决策树算法原理(下)](http://www.cnblogs.com/pinard/p/6053344.html)。

 　　在[决策树算法原理(上)](http://www.cnblogs.com/pinard/p/6050306.html)这篇里，我们讲到了决策树里ID3算法，和ID3算法的改进版C4.5算法。对于C4.5算法，我们也提到了它的不足，比如模型是用较为复杂的熵来度量，使用了相对较为复杂的多叉树，只能处理分类不能处理回归等。对于这些问题， CART算法大部分做了改进。CART算法也就是我们下面的重点了。由于CART算法可以做回归，也可以做分类，我们分别加以介绍，先从CART分类树算法开始，重点比较和C4.5算法的不同点。接着介绍CART回归树算法，重点介绍和CART分类树的不同点。然后我们讨论CART树的建树算法和剪枝算法，最后总结决策树算法的优缺点。

## 15.02 CART

### 15.02.01 CART分类树算法的最优特征选择方法

　　　　我们知道，在ID3算法中我们使用了信息增益来选择特征，信息增益大的优先选择。在C4.5算法中，采用了信息增益比来选择特征，以减少信息增益容易选择特征值多的特征的问题。但是无论是ID3还是C4.5,都是基于信息论的熵模型的，这里面会涉及大量的对数运算。能不能简化模型同时也不至于完全丢失熵模型的优点呢？有！CART分类树算法使用基尼系数来代替信息增益比，基尼系数代表了模型的不纯度，基尼系数越小，则不纯度越低，特征越好。这和信息增益(比)是相反的。

　　　　具体的，在分类问题中，假设有K个类别，第k个类别的概率为pkpk, 则基尼系数的表达式为：



Gini(p)=∑k=1Kpk(1−pk)=1−∑k=1Kp2kGini(p)=∑k=1Kpk(1−pk)=1−∑k=1Kpk2



　　　　如果是二类分类问题，计算就更加简单了，如果属于第一个样本输出的概率是p，则基尼系数的表达式为：



Gini(p)=2p(1−p)Gini(p)=2p(1−p)



　　　　对于个给定的样本D,假设有K个类别, 第k个类别的数量为CkCk,则样本D的基尼系数表达式为：



Gini(D)=1−∑k=1K(|Ck||D|)2Gini(D)=1−∑k=1K(|Ck||D|)2



　　　　特别的，对于样本D,如果根据特征A的某个值a,把D分成D1和D2两部分，则在特征A的条件下，D的基尼系数表达式为：



Gini(D,A)=|D1||D|Gini(D1)+|D2||D|Gini(D2)Gini(D,A)=|D1||D|Gini(D1)+|D2||D|Gini(D2)



　　　　大家可以比较下基尼系数表达式和熵模型的表达式，二次运算是不是比对数简单很多？尤其是二类分类的计算，更加简单。但是简单归简单，和熵模型的度量方式比，基尼系数对应的误差有多大呢？对于二类分类，基尼系数和熵之半的曲线如下：

![img](https://images2015.cnblogs.com/blog/1042406/201611/1042406-20161111105202170-1563882835.jpg)

　　　　从上图可以看出，基尼系数和熵之半的曲线非常接近，仅仅在45度角附近误差稍大。因此，基尼系数可以做为熵模型的一个近似替代。而CART分类树算法就是使用的基尼系数来选择决策树的特征。同时，为了进一步简化，CART分类树算法每次仅仅对某个特征的值进行二分，而不是多分，这样CART分类树算法建立起来的是二叉树，而不是多叉树。这样一可以进一步简化基尼系数的计算，二可以建立一个更加优雅的二叉树模型。

### 15.02.02 CART分类树算法对于连续特征和离散特征处理的改进

　　　　对于CART分类树连续值的处理问题，其思想和C4.5是相同的，都是将连续的特征离散化。唯一的区别在于在选择划分点时的度量方式不同，C4.5使用的是信息增益比，则CART分类树使用的是基尼系数。

　　　　具体的思路如下，比如m个样本的连续特征A有m个，从小到大排列为a1,a2,...,ama1,a2,...,am,则CART算法取相邻两样本值的平均数，一共取得m-1个划分点，其中第i个划分点Ti表示Ti表示为：Ti=ai+ai+12Ti=ai+ai+12。对于这m-1个点，分别计算以该点作为二元分类点时的基尼系数。选择基尼系数最小的点作为该连续特征的二元离散分类点。比如取到的基尼系数最小的点为atat,则小于atat的值为类别1，大于atat的值为类别2，这样我们就做到了连续特征的离散化。要注意的是，与ID3或者C4.5处理离散属性不同的是，如果当前节点为连续属性，则该属性后面还可以参与子节点的产生选择过程。

 

　　　　对于CART分类树离散值的处理问题，采用的思路是不停的二分离散特征。

　　　　回忆下ID3或者C4.5，如果某个特征A被选取建立决策树节点，如果它有A1,A2,A3三种类别，我们会在决策树上一下建立一个三叉的节点。这样导致决策树是多叉树。但是CART分类树使用的方法不同，他采用的是不停的二分，还是这个例子，CART分类树会考虑把A分成{A1}和{A2,A3}{A1}和{A2,A3}, {A2}和{A1,A3}{A2}和{A1,A3}, {A3}和{A1,A2}{A3}和{A1,A2}三种情况，找到基尼系数最小的组合，比如{A2}和{A1,A3}{A2}和{A1,A3},然后建立二叉树节点，一个节点是A2对应的样本，另一个节点是{A1,A3}对应的节点。同时，由于这次没有把特征A的取值完全分开，后面我们还有机会在子节点继续选择到特征A来划分A1和A3。这和ID3或者C4.5不同，在ID3或者C4.5的一棵子树中，离散特征只会参与一次节点的建立。

### 15.02.03 CART分类树建立算法的具体流程

　　　　上面介绍了CART算法的一些和C4.5不同之处，下面我们看看CART分类树建立算法的具体流程，之所以加上了建立，是因为CART树算法还有独立的剪枝算法这一块，这块我们在第5节讲。

　　　　算法输入是训练集D，基尼系数的阈值，样本个数阈值。

　　　　输出是决策树T。

　　　　我们的算法从根节点开始，用训练集递归的建立CART树。

　　　　1) 对于当前节点的数据集为D，如果样本个数小于阈值或者没有特征，则返回决策子树，当前节点停止递归。

　　　　2) 计算样本集D的基尼系数，如果基尼系数小于阈值，则返回决策树子树，当前节点停止递归。

　　　　3) 计算当前节点现有的各个特征的各个特征值对数据集D的基尼系数，对于离散值和连续值的处理方法和基尼系数的计算见第二节。缺失值的处理方法和上篇的C4.5算法里描述的相同。

　　　　4) 在计算出来的各个特征的各个特征值对数据集D的基尼系数中，选择基尼系数最小的特征A和对应的特征值a。根据这个最优特征和最优特征值，把数据集划分成两部分D1和D2，同时建立当前节点的左右节点，做节点的数据集D为D1，右节点的数据集D为D2.

　　　　5) 对左右的子节点递归的调用1-4步，生成决策树。

　　　

　　　　对于生成的决策树做预测的时候，假如测试集里的样本A落到了某个叶子节点，而节点里有多个训练样本。则对于A的类别预测采用的是这个叶子节点里概率最大的类别。

### 15.02.04 CART回归树建立算法

　　　　CART回归树和CART分类树的建立算法大部分是类似的，所以这里我们只讨论CART回归树和CART分类树的建立算法不同的地方。

　　　　首先，我们要明白，什么是回归树，什么是分类树。两者的区别在于样本输出，如果样本输出是离散值，那么这是一颗分类树。如果果样本输出是连续值，那么那么这是一颗回归树。

　　　　除了概念的不同，CART回归树和CART分类树的建立和预测的区别主要有下面两点：

1. 　　　　 **连续值的处理方法不同**
2. 　　　　 **决策树建立后做预测的方式不同。**

 

　　　　对于连续值的处理，我们知道CART分类树采用的是用基尼系数的大小来度量特征的各个划分点的优劣情况。这比较适合分类模型，但是对于回归模型，我们使用了常见的和方差的度量方式，CART回归树的度量目标是，对于任意划分特征A，对应的任意划分点s两边划分成的数据集D1和D2，求出使D1和D2各自集合的均方差最小，同时D1和D2的均方差之和最小所对应的特征和特征值划分点。表达式为：



minA,s[minc1∑xi∈D1(A,s)(yi−c1)2+minc2∑xi∈D2(A,s)(yi−c2)2]min⏟A,s[min⏟c1∑xi∈D1(A,s)(yi−c1)2+min⏟c2∑xi∈D2(A,s)(yi−c2)2]



　　　　其中，c1c1为D1数据集的样本输出均值，c2c2为D2数据集的样本输出均值。

 

　　　　对于决策树建立后做预测的方式，上面讲到了CART分类树采用叶子节点里概率最大的类别作为当前节点的预测类别。而回归树输出不是类别，它采用的是用最终叶子的均值或者中位数来预测输出结果。

　　　　除了上面提到了以外，CART回归树和CART分类树的建立算法和预测没有什么区别。

### 15.02.05 CART树算法的剪枝

　　　　CART回归树和CART分类树的剪枝策略除了在度量损失的时候一个使用均方差，一个使用基尼系数，算法基本完全一样，这里我们一起来讲。

　　　　由于决策时算法很容易对训练集过拟合，而导致泛化能力差，为了解决这个问题，我们需要对CART树进行剪枝，即类似于线性回归的正则化，来增加决策树的泛化能力。但是，有很多的剪枝方法，我们应该这么选择呢？CART采用的办法是后剪枝法，即先生成决策树，然后产生所有可能的剪枝后的CART树，然后使用交叉验证来检验各种剪枝的效果，选择泛化能力最好的剪枝策略。也就是说，<span style="font-weight:bold;background-color: Tomato">CART树的剪枝算法可以概括为两步，第一步是从原始决策树生成各种剪枝效果的决策树，第二部是用交叉验证来检验剪枝后的预测能力，选择泛化预测能力最好的剪枝后的数作为最终的CART树。</span>

 　　　　首先我们看看剪枝的损失函数度量，在剪枝的过程中，对于任意的一刻子树T,其损失函数为：



Cα(Tt)=C(Tt)+α|Tt|Cα(Tt)=C(Tt)+α|Tt|



　　　　其中，αα为正则化参数，这和线性回归的正则化一样。C(Tt)C(Tt)为训练数据的预测误差，分类树是用基尼系数度量，回归树是均方差度量。|Tt||Tt|是子树T的叶子节点的数量。

　　　　当α=0α=0时，即没有正则化，原始的生成的CART树即为最优子树。当α=∞α=∞时，即正则化强度达到最大，此时由原始的生成的CART树的根节点组成的单节点树为最优子树。当然，这是两种极端情况。一般来说，αα越大，则剪枝剪的越厉害，生成的最优子树相比原生决策树就越偏小。对于固定的αα，一定存在使损失函数Cα(T)Cα(T)最小的唯一子树。

 

　　　　看过剪枝的损失函数度量后，我们再来看看剪枝的思路，对于位于节点t的任意一颗子树TtTt，如果没有剪枝，它的损失是



Cα(Tt)=C(Tt)+α|Tt|Cα(Tt)=C(Tt)+α|Tt|



　　　　如果将其剪掉，仅仅保留根节点，则损失是



Cα(T)=C(T)+αCα(T)=C(T)+α



　　　　当α=0α=0或者αα很小时，Cα(Tt)<Cα(T)Cα(Tt)<Cα(T) , 当αα增大到一定的程度时

Cα(Tt)=Cα(T)Cα(Tt)=Cα(T)

。当αα继续增大时不等式反向，也就是说，如果满足下式：





α=C(T)−C(Tt)|Tt|−1α=C(T)−C(Tt)|Tt|−1



　　　　TtTt和TT有相同的损失函数，但是TT节点更少，因此可以对子树TtTt进行剪枝，也就是将它的子节点全部剪掉，变为一个叶子节点TT。

 

　　　　最后我们看看CART树的交叉验证策略。上面我们讲到，可以计算出每个子树是否剪枝的阈值αα，如果我们把所有的节点是否剪枝的值αα都计算出来，然后分别针对不同的αα所对应的剪枝后的最优子树做交叉验证。这样就可以选择一个最好的αα，有了这个αα，我们就可以用对应的最优子树作为最终结果。

 

　　　　好了，有了上面的思路，我们现在来看看CART树的剪枝算法。

　　　　输入是CART树建立算法得到的原始决策树T0T0。

　　　　输出是最优决策子树TαTα。

　　　　算法主要过程如下：

　　　　1）初始化k=0,T=T0k=0,T=T0， 最优子树集合ω={T}ω={T}。

　　　　2）αmin=∞αmin=∞

　　　　3) 从叶子节点开始自下而上计算各内部节点t的训练误差损失函数Cα(Tt)Cα(Tt)（回归树为均方差，分类树为基尼系数）, 叶子节点数|Tt||Tt|，以及正则化阈值α=min{C(T)−C(Tt)|Tt|−1,αmin}α=min{C(T)−C(Tt)|Tt|−1,αmin}, 更新αmin=ααmin=α

　　　　4) αk=αminαk=αmin。

　　　　5）自上而下的访问子树t的内部节点，如果C(T)−C(Tt)|Tt|−1≤αkC(T)−C(Tt)|Tt|−1≤αk时，进行剪枝。并决定叶节点t的值。如果是分类树，则是概率最高的类别，如果是回归树，则是所有样本输出的均值。这样得到αkαk对应的最优子树TkTk

　　　　6）最优子树集合ω=ω∪Tkω=ω∪Tk

　　　　7) k=k+1,T=Tkk=k+1,T=Tk, 如果T不是由根节点单独组成的树，则回到步骤2继续递归执行。否则就已经得到了所有的可选最优子树集合ωω.

　　　　8) 采用交叉验证在ωω选择最优子树TαTα

### 15.02.06 CART算法小结

　　　　上面我们对CART算法做了一个详细的介绍，CART算法相比C4.5算法的分类方法，采用了简化的二叉树模型，同时特征选择采用了近似的基尼系数来简化计算。当然CART树最大的好处是还可以做回归模型，这个C4.5没有。下表给出了ID3，C4.5和CART的一个比较总结。希望可以帮助大家理解。

| 算法 | 支持模型   | 树结构 | 特征选择         | 连续值处理 | 缺失值处理 | 剪枝   |
| ---- | ---------- | ------ | ---------------- | ---------- | ---------- | ------ |
| ID3  | 分类       | 多叉树 | 信息增益         | 不支持     | 不支持     | 不支持 |
| C4.5 | 分类       | 多叉树 | 信息增益比       | 支持       | 支持       | 支持   |
| CART | 分类，回归 | 二叉树 | 基尼系数，均方差 | 支持       | 支持       | 支持   |

　　　　看起来CART算法高大上，那么CART算法还有没有什么缺点呢？有！主要的缺点我认为如下：

　　　　1）应该大家有注意到，无论是ID3, C4.5还是CART,在做特征选择的时候都是选择最优的一个特征来做分类决策，但是大多数，分类决策不应该是由某一个特征决定的，而是应该由一组特征决定的。这样决策得到的决策树更加准确。这个决策树叫做多变量决策树(multi-variate decision tree)。在选择最优特征的时候，多变量决策树不是选择某一个最优特征，而是选择最优的一个特征线性组合来做决策。**这个算法的代表是OC1，**这里不多介绍。

　　　　2）如果样本发生一点点的改动，就会导致树结构的剧烈改变。这个可以通过集成学习里面的随机森林之类的方法解决。　　　

## 15.03 决策树算法小结

　　　　终于到了最后的总结阶段了，这里我们不再纠结于ID3, C4.5和 CART，我们来看看决策树算法作为一个大类别的分类回归算法的优缺点。这部分总结于scikit-learn的英文文档。

### 15.03.01 Benefits

　　　　首先我们看看决策树算法的优点：

　　　　1）简单直观，生成的决策树很直观。

　　　　2）基本不需要预处理，不需要提前归一化，处理缺失值。

　　　　3）使用决策树预测的代价是O(log2m)O(log2m)。 m为样本数。

　　　　4）既可以处理离散值也可以处理连续值。很多算法只是专注于离散值或者连续值。

　　　　5）可以处理多维度输出的分类问题。

　　　　6）相比于神经网络之类的黑盒分类模型，决策树在逻辑上可以得到很好的解释

　　　　7）可以交叉验证的剪枝来选择模型，从而提高泛化能力。

　　　　8） 对于异常点的容错能力好，健壮性高。

### 15.03.02 Back

　　　　1）决策树非常容易过拟合，导致泛化能力不强。<span style="font-weight:bold;background-color: Tomato">可以通过设置节点最少样本数量和限制决策树深度来改进。</span>

　　　　2）决策树会因为样本发生一点点的改动，就会导致树结构的剧烈改变。<span style="font-weight:bold;background-color: Tomato">可以通过集成学习之类的方法解决。</span>

　　　　3）寻找最优的决策树是一个NP难的问题，我们一般是通过启发式方法，容易陷入局部最优。<span style="font-weight:bold;background-color: Tomato">可以通过集成学习之类的方法来改善。</span>

　　　　4）有些比较复杂的关系决策树很难学习，比如异或。这个就没有办法了，一般这种关系可以换神经网络分类方法来解决。

　　　　5）如果某些特征的样本比例过大，生成决策树容易偏向于这些特征。<span style="font-weight:bold;background-color: Tomato">这个可以通过调节样本权重来改善。</span>





## 15.04 Scikit-learn & DT

![img](https://2.bp.blogspot.com/-EvSXDotTOwc/XMfeOGZ-CVI/AAAAAAAAEiE/oePFfvhfOQM11dgRn9FkPxlegCXbgOF4QCLcBGAs/s1600/confusionMatrxiUpdated.jpg)



## 15.05  Decision Tree in R

[R语言与机器学习学习笔记（分类算法）（2）决策树算法_beta-CSDN博客](https://blog.csdn.net/yujunbeta/article/details/14986219/?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.control&spm=1001.2101.3001.4242)



参考：

[决策树scikit-learn重要参数详解_小狐狸-CSDN博客](https://blog.csdn.net/u010591976/article/details/105825363)

[scikit-learn决策树算法类库使用小结 - 刘建平Pinard - 博客园 (cnblogs.com)](https://www.cnblogs.com/pinard/p/6056319.html)

[A Step by Step ID3 Decision Tree Example - Sefik Ilkin Serengil (sefiks.com)](https://sefiks.com/2017/11/20/a-step-by-step-id3-decision-tree-example/)



| 参数                                             | DecisionTreeClassifier                                       | DecisionTreeRegressor                                        |
| ------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 特征选择标准criterion                            | 可以使用"gini"或者"entropy"，前者代表基尼系数，后者代表信息增益。一般说使用默认的基尼系数"gini"就可以了，即CART算法。除非你更喜欢类似ID3, C4.5的最优特征选择方法。 | 可以使用"mse"或者"mae"，前者是均方差，后者是和均值之差的绝对值之和。推荐使用默认的"mse"。一般来说"mse"比"mae"更加精确。除非你想比较二个参数的效果的不同之处。 |
| 特征划分点选择标准splitter                       | 可以使用"best"或者"random"。前者在特征的所有划分点中找出最优的划分点。后者是随机的在部分划分点中找局部最优的划分点。默认的"best"适合样本量不大的时候，而如果样本数据量非常大，此时决策树构建推荐"random" |                                                              |
| 划分时考虑的最大特征数max_features               | 可以使用很多种类型的值，默认是"None",意味着划分时考虑所有的特征数；如果是"log2"意味着划分时最多考虑log2Nlog2N个特征；如果是"sqrt"或者"auto"意味着划分时最多考虑N−−√N个特征。如果是整数，代表考虑的特征绝对数。如果是浮点数，代表考虑特征百分比，即考虑（百分比xN）取整后的特征数。其中N为样本总特征数。一般来说，如果样本特征数不多，比如小于50，我们用默认的"None"就可以了，如果特征数非常多，我们可以灵活使用刚才描述的其他取值来控制划分时考虑的最大特征数，以控制决策树的生成时间。 |                                                              |
| 决策树最大深max_depth                            | 决策树的最大深度，默认可以不输入，如果不输入的话，决策树在建立子树的时候不会限制子树的深度。一般来说，数据少或者特征少的时候可以不管这个值。如果模型样本量多，特征也多的情况下，推荐限制这个最大深度，具体的取值取决于数据的分布。常用的可以取值10-100之间。 |                                                              |
| 内部节点再划分所需最小样本数min_samples_split    | 这个值限制了子树继续划分的条件，如果某节点的样本数少于min_samples_split，则不会继续再尝试选择最优特征来进行划分。 默认是2.如果样本量不大，不需要管这个值。如果样本量数量级非常大，则推荐增大这个值。我之前的一个项目例子，有大概10万样本，建立决策树时，我选择了min_samples_split=10。可以作为参考。 |                                                              |
| 叶子节点最少样本数min_samples_leaf               | 这个值限制了叶子节点最少的样本数，如果某叶子节点数目小于样本数，则会和兄弟节点一起被剪枝。 默认是1,可以输入最少的样本数的整数，或者最少样本数占样本总数的百分比。如果样本量不大，不需要管这个值。如果样本量数量级非常大，则推荐增大这个值。之前的10万样本项目使用min_samples_leaf的值为5，仅供参考。 |                                                              |
| 叶子节点最小的样本权重和min_weight_fraction_leaf | 这个值限制了叶子节点所有样本权重和的最小值，如果小于这个值，则会和兄弟节点一起被剪枝。 默认是0，就是不考虑权重问题。一般来说，如果我们有较多样本有缺失值，或者分类树样本的分布类别偏差很大，就会引入样本权重，这时我们就要注意这个值了。 |                                                              |
| 最大叶子节点数max_leaf_nodes                     | 通过限制最大叶子节点数，可以防止过拟合，默认是"None”，即不限制最大的叶子节点数。如果加了限制，算法会建立在最大叶子节点数内最优的决策树。如果特征不多，可以不考虑这个值，但是如果特征分成多的话，可以加以限制，具体的值可以通过交叉验证得到。 |                                                              |
| 类别权重class_weight                             | 指定样本各类别的的权重，主要是为了防止训练集某些类别的样本过多，导致训练的决策树过于偏向这些类别。这里可以自己指定各个样本的权重，或者用“balanced”，如果使用“balanced”，则算法会自己计算权重，样本量少的类别所对应的样本权重会高。当然，如果你的样本类别分布没有明显的偏倚，则可以不管这个参数，选择默认的"None" | 不适用于回归树                                               |
| 节点划分最小不纯度min_impurity_split             | 这个值限制了决策树的增长，如果某节点的不纯度(基尼系数，信息增益，均方差，绝对差)小于这个阈值，则该节点不再生成子节点。即为叶子节点 。 |                                                              |
| 数据是否预排序presort                            | 这个值是布尔值，默认是False不排序。一般来说，如果样本量少或者限制了一个深度很小的决策树，设置为true可以让划分点选择更加快，决策树建立的更加快。如果样本量太大的话，反而没有什么好处。问题是样本量少的时候，我速度本来就不慢。所以这个值一般懒得理它就可以了。 |                                                              |























































# 98 In-depth understanding







# 99 Supplementary

## 99.01 Issue

[岭大王](https://www.zhihu.com/people/jerryd99)

[化简可得]([用人话讲明白逻辑回归Logistic regression - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/139122386))

[马冬什么](https://www.zhihu.com/column/c_1114213082062450688)

[线性回归-CSDN博客](https://blog.csdn.net/Sirow/article/details/109141844)

[零基础入门深度学习(1) - 感知器 - 作业部落 Cmd Markdown 编辑阅读器 (zybuluo.com)](https://www.zybuluo.com/hanbingtao/note/433855)

[Sirow / Repositories (github.com)](https://github.com/Sirow?tab=repositories)

[刘建平Pinard - 博客园 (cnblogs.com)](https://www.cnblogs.com/pinard/)



[笔试面试-红色石头的个人博客 (redstonewill.com)](https://redstonewill.com/category/written-interview/)

## 99.02 Layout

[文本居中]([LaTeX（1）设置部分文本居中左对齐、居中右对齐_一个瑞特的博客-CSDN博客_latex 右对齐](https://blog.csdn.net/qq_40675882/article/details/84556527))

[排版大全]([Typora数学公式汇总（Markdown） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/261750408?utm_source=wechat_session))

## 99.03 Josh Starmer

### ML<80>

### NN<13>

### Statistics<46>

### Linear regression<10>

### Logistic Regression<7>
