# 梯度下降
通过迭代计算局部最小值点的方法。  
机器学习中常见模型，已知损失函数f(w)，其中w是权重矩阵，目标是求w，使得f(w)最小。  
迭代规则 $w_{n} = w_{n-1} - \eta f'(w)$  
迭代过程演示:  
以 $f(x)=x^2+2x+1$ 为例， $f'(x)=2x+2$ ，目标求 $x$ 使得 $f(x)$ 达到最小值，设定开始叠代初始值0.  
$x_0=0, \eta=0.05, $  
$f'(0)=2, x_1=0-0.05 \times 2=-0.1 $  
$f'(-0.1)=1.8, x_2=-0.1-1.8 \times 0.05=-0.19 $  
$x_3=-0.271, x_4=-0.3439, ..., x_{100}=-0.9999734386011123 $  
可梯度求解的条件 L-Lipschitz continuous  
存在常数 $L$，对于作用域 $D$ 任意 $x,y$ ， 总是满足 $||\nabla x - \nabla y|| \le L||x-y||$  
多维举例:  
$f(x_0,x_1)=(x_0-4)^2+(x_1-2)^2 $  
转换成矩阵的写法:  
```math
W=\begin{bmatrix}
x_0\\
x_1
\end{bmatrix},
A=\begin{bmatrix}
4 \\
2
\end{bmatrix},
f(W)=(W-A)^T(W-A),
f'(W)=2(W-A),
W_n=W_{n-1}-0.1(W_{n-1}-A),
W_0=\begin{bmatrix}
0\\
0
\end{bmatrix}
```
迭代过程:
```math
W_1=\begin{bmatrix}
0.4 \\
0.2
\end{bmatrix},
W_2=\begin{bmatrix}
0.76 \\
0.38
\end{bmatrix},
W_3=\begin{bmatrix}
1.084 \\
0.542
\end{bmatrix},
W_4=\begin{bmatrix}
1.3756 \\
0.6878
\end{bmatrix},
W_5=\begin{bmatrix}
1.63804 \\
0.81902
\end{bmatrix},...
W_{58}=\begin{bmatrix}
3.99112588 \\
1.99556294
\end{bmatrix}
```

# Softmax
softmax用于多分类过程中，它将多个神经元的输出，映射到（0,1）区间内，可以看成概率来理解，从而来进行多分类  
```math
V=\begin{bmatrix}
v_0, v_1, v_2, ... v_n
\end{bmatrix},
S_i = \frac{e^{v_i}}{\sum\limits_{j=0}^ne^{v_j}}
```
# Cross Entropy Loss Function(交叉熵损失函数)
交叉熵损失函数经常用于分类问题中，特别是在神经网络做分类问题时，也经常使用交叉熵作为损失函数，此外，由于交叉熵涉及到计算每个类别的概率，所以交叉熵几乎每次都和sigmoid(或softmax)函数一起出现  
 $L=-\frac{1}{N}\sum\limits_{i}\sum\limits_{c=1}^{M}y_{ic}lnp_{ic}$  
为了进一步区分相同正确率情况下不同模型的好坏。
比如对于三分类问题:  
模型A [0.1, 0.1, 0.8], [0.2, 0.7, 0.1], [0.4, 0.6, 0]   
模型B [0.1, 0.2, 0.7], [0.2, 0.6, 0.2], [0.1, 0.1, 0.8]  
实际结果为 [0, 0, 1], [0, 1, 0], [1, 0, 0]  
模型A，B前两个都会判对，但是明显模型A比模型B更优，因为它更接近真实答案。即使是错误的第三个结果。  
另一个优点: 求导简单，最优化损失函数。

# Word2Vec
one-shot  
King  [1, 0, 0, 0]  
Man   [0, 1, 0, 0]  
Queen [0, 0, 1, 0]  
Woman [0, 0, 0, 1]  
word vec:  
![w2v](https://ask.hellobi.com/uploads/article/20200509/hnnydodboo.webp)  
通过计算能体现词之间的关系  
![w2v_target](https://ask.hellobi.com/uploads/article/20200509/pwdnuspbqf.webp)  
CBOW
