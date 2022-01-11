# 分析数学

泛函：$f \to R$

准确的来说，对于集合$M(X;Y)$为集合$X$到集合$Y$的映射的集合，$x_0$是$X$的一个固定元素。定义函数$F:M(X;Y) \to Y$，让每个函数$f \in M $对应$f(x_0) \in Y$。

算子：$f \to f$

系统的构形：这个系统的质点间的相互位置决定的性质

对于$n$个质点，在三维空间下的构形为$R^{3n}$

系统的相空间：

与构形$q$对于的速度$v$一起组成序偶$(q,v)$组成的直积$R^{3n} \times R^{3n}$的一个子集$\Phi$称为相空间

函数的定义：

1. 首先，我们定义关系这一概念

2. 其次，满足：

    ​	$(xRy_1)\wedge(xRy_2)\Rightarrow(y_1=y_2)$

    的关系$R$称为函数关系，简称函数

映射分类：

+ 满射：$f(X) = Y$
+ 单射：$(f(x_1) = f(x_2))\Rightarrow(x_1 = x_2)$
+ 双射：既是满射又是单射

以下四组条件定义一个集合为实数集$R$，其元素称为实数

1. 加法公理，定义了一个映射（加法）：

​$$+:R \times R \rightarrow R$$

​且以下条件成立：

+ $\exists 0 \in R, \forall x \in R, x + 0 = 0 + x = x$				

+ $\forall x \in R, \exists -x \in R, x + (-x) = (-x) + x = 0$

+ $\forall x, y, z \in R, x + (y + z) = (x + y) + z$

+ $\forall x, y \in R, x + y = y + x$

如果某集合$G$上定义了满足公理1，2，3的运算，我们说在$G$上给定了群结构，即$G$是群。如果这个运算是加法，则称为加法群。

如果一个群还满足公理四，则称这个群为交换群，或阿贝尔群。

实数集是加法阿贝尔群。

2. 乘法公理，定义一个映射（乘法）：

    ​	$\cdot : R \times R \rightarrow R$

​		且以下条件成立：

+ $\exists 1 \in R\backslash0, \forall x \in R, x \cdot 1 = 1 \cdot x = x$ 

+ $\forall x \in R, \exists x^{-1} \in R, x \cdot x^{-1} = x^{-1} \cdot x = 1$

+ $\forall x, y, z \in R, x \cdot (y \cdot z) = (x \cdot y) \cdot z$
+ $\forall x, y \in R, x \cdot y = y \cdot x$

可证，$R \backslash 0$是乘法群。

加法与乘法的联系。乘法相对于加法满足分配律，即

$\forall x, y, z \in R, (x + y) \cdot z = x \cdot z + y \cdot z$

如果某集合$G$满足了上述的加法公理和乘法公理，则称$G$为代数域，简称域。

3. 序公理。

$R$的元素存在关系$\leq$，满足以下条件：

+ $\forall x \in R, x \leq x$
+ $(x \leq y) \wedge (y \leq x) \Rightarrow (x = y)$ 
+ $(x \leq y) \wedge (y \leq z) \Rightarrow (x \leq z)$
+ $\forall x, y \in R, (x \leq y) \vee (y \leq x)$

$\leq$称为不等关系，如果一个集合满足前三个条件，则该集合称为偏序集。如果一个偏序集还满足第四个条件，该集合称为线性序集。

实数集为线性序集。

5. 完备性公理（连续性公理），如果$X$与$Y$是$R$的非空子集，且$\forall x \in X, y \in Y, x \le y \Rightarrow (\exists c \in R, x \le c \le y)$

