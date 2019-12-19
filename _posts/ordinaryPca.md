# ordinary pca
## 问题描述
众所周知，`PCA`是数据降维的方法，数据集有$n$个$d$维的数据点$x_1,x_2, \dots,x_n$，其中$x_i=\left[\begin{matrix} x_{i1} \\ x_{i2} \\ \vdots \\ x_{id}\end{matrix}\right]$ ，降维不是简单的从这$d$个维度去掉几个维度。如下图所示，不是从$x$轴或$y$轴去掉一个维度，而是从新选择一组坐标轴，从新的坐标系中去掉几个维度，最主要的坐标轴叫第一主成分，其次叫第二主成分。。。

`新的坐标轴选择的标准`是数据集在这个坐标轴上的投影尽量分散，也就是方差最大。

**用数学表示：**

给定数据集$X_{d*n}=\left[\begin{matrix}x_1 & x_2 &\dots &x_n\end{matrix}\right]_{d*n}$.

均值$\bar{x}=\frac{1}{n}\sum_{i=1}^nx_i$,

把数据集移动到原点，$\tilde{X}=\left[\begin{matrix}x_1-\bar{x} & x_2-\bar{x} &\dots &x_n-\bar{x}\end{matrix}\right]_{d*n}$，方便表示，假设$X$均值在原点。

假设要求的一个坐标轴为$u$，点$x$在$u$上的投影为$u^Tx$。所有点的投影为$u^TX=\left[\begin{matrix}u^Tx_1 &u^T x_2 &\dots &u^Tx_n\end{matrix}\right]_{1*n}$，这$n$个值的方差为$\frac{1}{n-1}u^TX(u^TX)$，要这个值最大，$u$如果大小没有限制，同一个方向可以无限增大，方差就会无限增大，就没有意义了，我们要找的是一个方向，所以限制$u^Tu=1$.
![Alt](https://img-blog.csdnimg.cn/20191213211457925.png#pic_center =400x400)
## 数学推导
综上所述，计算第一主成分$u$，可以转化为下面的凸优化问题。
$$\max_u \quad u^TXX^Tu \\ s.t. \quad u^Tu=1$$

其中，$XX^T$是协方差矩阵，$S=XX^T_{d*d}=\left[ \begin{matrix} x_1 & x_2 & \dots x_n\end{matrix}\right]\left[ \begin{matrix} x_1^T \\ x_2^T \\ \vdots  \\x_n\end{matrix}\right]$ ,$S_{i,j}$是第$i$维属性和第$j$维属性的协方差​。

可以用Lagrange对偶条件求解：
$$\begin{aligned} 
L(u,\lambda)&=u^TSu-\lambda(u^Tu-1) \\
\frac{\partial{L}}{\partial{u}}&= 2Su-2\lambda u \\ &=0\\
Su&=\lambda u
\end{aligned}$$

$u$是$S$的特征向量，$\lambda$是特征值。
目标函数$u^TSu=u^T\lambda u=\lambda u^Tu=\lambda$，最大化$\lambda$。

第一主成分$u$就是$S=XX^T$的最大特征值对应的特征向量，方差就是最大特征值。
第二主成分是$S=XX^T$的次大特征值对应的特征向量。
假设降到$p$维，就可以选$XX^T$的最大$p$个特征值对应的特征向量$u_1,u_2,\dots,u_p$作为新的坐标轴，剩下的维度丢掉。
对$X$进行奇异值求解，$X=U\Sigma^{\frac{1}{2}} V^T$，则$XX^T=U\Sigma U^T$，所以$S=XX^T$的特征值、特征向量可以通过对$X$奇异值求解得到。

主成分们用矩阵表示：$U_{d*p}=\left[\begin{matrix}u_1 &u_2 & \dots & u_p \end{matrix}\right]$.

$d$维的点$x$，降到$p$维后的数据表示：$y_{p*1}=U^Tx$.

降维后的点再用原来的坐标系表示，也就是恢复为$d$维，$\hat{x}=Uy=UU^Tx$.


**整个PCA的流程：**
- 计算$X$的均值$\bar{x}=\frac{1}{n}\sum_{i=1}^nx_i$
- $\tilde{X}=\left[\begin{matrix}x_1-\bar{x} & x_2-\bar{x} &\dots &x_n-\bar{x}\end{matrix}\right]_{d*n}$
- 对$\tilde{X}$进行奇异值分解，$\tilde{X}=U\Sigma^{1/2}V^T$，取最大的$p$个特征值对应的特征向量$U$
- 降维后的数据集$Y_{p*n}=U^T\tilde{X}$
- 降维后的数据集再映射回原空间$\hat{X}_{d*n}=UY=UU^TX$
- 一个新的点$x$，降维后$y=U^Tx$
- 恢复到$d$维，$\hat{x}=UU^Tx$


## 举个栗子
## 线代知识补充
$n$维空间中的一个向量$x$，我们平常见到的向量都是这样写的，如$x=\left[\begin{matrix} x_1 \\ x_2 \\ \vdots \\ x_n\end{matrix}\right]$。

但这并不是向量的本来面目，向量本质就是空间中的一个点而已。

在$n$维空间中选择线性不相关的$n$个向量$\left[\begin{matrix}i_1, i_2,\dots,i_n\end{matrix}\right]$，空间中的任何向量都可以用这n个向量线性表示，$x=x_1i_1+x_2i_2+\dots+x_ni_n$，另一个向量$y=y_1i_1+y_2i_2+\dots+y_ni_n$，$x$可以表示为$x=\left[\begin{matrix} x_1 \\ x_2 \\ \vdots \\ x_n\end{matrix}\right]$，$y$可以表示为$y=\left[\begin{matrix} y_1 \\ y_2 \\ \vdots \\y_n\end{matrix}\right]$，这样向量就可以用数字表示了。

$\left[\begin{matrix}i_1, i_2,\dots,i_n\end{matrix}\right]$是空间的`一组基`，如果它们都是单位向量($i_i^Ti_i=1$)，并且相互正交($i_i^Ti_j=0,i\ne j$)，这组基就叫标准正交基，可以把它们放到矩阵的列中，$I=\left[\begin{matrix}i_1, i_2,\dots,i_n\end{matrix}\right]$，向量$x$可以表示为$Ix$。


我们可以选择另外一组线性不相关的$n$个向量作为一组基(basis)，$U=\left[\begin{matrix}u_1, u_2,\dots,u_n\end{matrix}\right]$，$x=x_1'u_1+x_2'u_2+\dots+x_n'u_n=Ux'$，$x$可以表示为$x=\left[\begin{matrix} x_1' \\ x_2' \\ \vdots \\ x_n'\end{matrix}\right]$。

一个向量$x$，选择不同的基，有不同的表示形式。它们内在的联系是同一个向量，在不同的基下，有不同的系数，即$Ix=Ux'$。
如果$U,I$都是标准正交基，即$U^TU=I$，那么$x'=U^Tx$。这个就是`基变换`。

标准正交基特性$U^TU=I, UU^T=u_1u_1^T+u_2u_2^T+\dots+u_n^Tu=I$

$x=Ux',x'=U^Tx, x'=UU^Tx$
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191214145505640.png)
## 其他
我看到一张图，很好看，[出处在这里](https://github.com/apachecn/fe4ml-zh/blob/master/docs/6.%E9%99%8D%E7%BB%B4%EF%BC%9A%E7%94%A8_PCA_%E5%8E%8B%E7%BC%A9%E6%95%B0%E6%8D%AE%E9%9B%86.md)
![Alt](https://img-blog.csdnimg.cn/20191214175558705.png#pic_center)
## 参考
https://www.youtube.com/watch?v=jeOEXCFK30M
https://wiki.math.uwaterloo.ca/statwiki/index.php?title=stat841f11
https://www.youtube.com/watch?v=L-pQtGm3VS8
http://www.math.uwaterloo.ca/~aghodsib/courses/f06stat890/notes/lec5.pdf
https://zh.wikipedia.org/wiki/%E4%B8%BB%E6%88%90%E5%88%86%E5%88%86%E6%9E%90