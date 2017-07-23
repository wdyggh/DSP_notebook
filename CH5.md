
# CH5 - Linear System

连续信号一般用 $x(t) \;and\; y(t)$ 表示  
离散信号一般用 $x[n] \;and\; y[n]$ 表示  

连续系统(continus system) 的输入输出信号都为 连续信号  
离散系统(discrete system) 的输入输出信号都为 离散信号  

有趣的是，大多有用的系统都为线性系统(Linear System)，不是说非线性系统无用。  

### Requirements for linearity

假如一个系统是线性系统，必须满足2个性质：同质性(homogeneity)，可加性(additivity)。  
如果能证明一个系统满足以上2个性质，也就证明了这个系统是线性系统。  
第三个性质是移位不变性(shift invariance)。  

下面来了解一下这三个性质：  

1. 如果$x[n]\to|System| \to y[n]$，那么$k\bullet x[n]\to|System| \to k\bullet y[n]$。  
2. 如果$x_1[n]\to|System| \to y_1[n]$，而且$x_2[n]\to|System| \to y_2[n]$，那么$x_1[n]+x_2[n]\to|System| \to y_1[n]+y_2[n]$。  
3. 如果$x[n]\to|System| \to y[n]$，那么$x[n+s]\to|System| \to y[n+s]$。  

### Static Linearity and Sinusoidal Fidelity  

刚刚的三个性质是提供了对线性系统的数学定义。  
那么如何能够更加直观的了解一个线性系统？  
静态线性度(Static Linearity) 和 正弦保真度(Sinusoidal Fidelity)  

Static linearity defines how a linear system reacts when the signals aren't changing, i.e., when they are DC or static.  
一个线性系统的静态响应(Static response)是非常简单的，输出是输入乘一个倍数。  
2个简单的线性系统：欧姆定律(电压和电流)和 胡克定律(力和伸长长度)。  
他们都是过原点的直线，满足2个基本线性系统的性质。  
2个非线性系统的例子：穿过一个二极管的电压和电流，磁力大小和通量密度。  

正弦保真度(Sinusoidal fidelity)：假如一个线性系统的输入为正弦波，那么该系统的输出也为正弦波，而且输入输出有相同的频率。  

### Example of Linear and Nonlinear System  

Example of Linear Systems:  
* Wave propagation  
* Electrical circuits (resistors, capacitors, inductors)  
* Convolution  

Example of Nonlinear Systems:  
* Systems that do not have static linearity  
* Systems that do not have sinusoidal fidelity  
* Systems with a threshold  
* Multiplication of one signal by another signal  
* Hysteresis phenomena  
* Saturation  

### Special Properties of Linearity  

Linearity is commutative, a property involving the combination of 2 or more systems.  
假如在一个级联系统里，包含2个子系统，并且子系统都为线性，那么整个组合系统也是线性的。  
如果$x[n]\to|System\;A|\to|System\;B| \to y[n]$，那么$x[n]\to|System\;B|\to|System\;A| \to y[n]$。  

线性系统的乘法在使用时经常会出错，根据乘的信号，结果可能是线性，也可能是非线性。  
假设一个系统对输入信号乘一个常数，那么结果是线性的。  
假设一个系统对输入信号乘另一个信号，那么结果是非线性的。  

### Superposition: the Foundation of DSP

3个信号相加得到第四个信号$x[n]$，$x[n]=x_0[n]+x_1[n]+x_2[n]$  
对信号进行组合（缩放scaling，相加addition）的过程称为合成（synthesis）。  
分解（decomposition）是合成的逆过程，一个信号可以拆分成几个信号。  

DSP的核心是叠加（superposition）  
假设一线性系统$x[n]\to|System|\to y[n]$，输入为$x[n]$，输出是$y[n]$。  
可以把$x[n]$拆分成$x_0[n],x_1[n],x_2[n],x_3[n],\;etc$，  
$x_k[n]=[0,0,...,x[k],0,0,...], k\in(0,n)$，k位置上的值为$x[k]$，其他为$0$。  
$x_k[n]\to|System|\to y_k[n],\;k\in(0,n)$，$y[n]$信号的合成$y[n]=y_0[n]+y_1[n]+...+y_k[n],\;k\in(0,n)$  
以上可以说明一个再复杂的信号，也能拆分成简单的信号样本经过相同的系统得到的结果的合成。  
一个简单的例子是，$2041\times 4 \to 8000+160+4$  

### Common Decompositions

分解是把一个复杂的问题分成几个小问题去解决，在这主要有2中方法来分解信号：*impulse decomposition* 和 *Fourier decomposition*  

#### Impulse Decomposition

将一组长度为N的信号分成N个样本，并且长度为N。  
每个样本的位置N上的值为原信号的值，其他位置的值为0。  
这样就有了N组脉冲信号。  
已知一个系统对于一个脉冲信号的响应，那么系统的输出就可以计算，卷积也是用的相同方法。  

#### Step Decomposition

将一组长度为N的信号$x[N]$分成N个样本$x_k[N]\;k\in(0,N)$，并且长度为N。  
$x_k[n]= \begin{cases}0  & \quad k< n \\ x[k]-x[k-1] &\quad k\geq n\end{cases}\\n++;\;n\in(0,N)\\ k++; \; k\in(0,N)$  
换句话说$x_k[n]$的位置为$k$开始往后的值为$x[k]-x[k-1]$，之前的都为$0$ 。  

#### Even/Odd Decomposition

将一组长度为N的信号$x[N]$分成2个样本。  
偶数分解 $x_E[n]=\frac{x[n]+x[N-n]}{2}$，关于y轴对称  
奇数分解 $x_O[n]=\frac{x[n]-x[N-n]}{2}$，关于原点对称  

#### Fourier Decomposition

傅里叶分解：一组长度为N的信号，可以分成长度为N+2的信号，其中一半是正弦波，一半是余弦波。  
余弦，低频部分$x_{c0}[n]$（make 0 complete cycles over the N samples）是DC部分,  
其他部分$x_{c1}[n], x_{c2}[n],...$ （make 1,2,3 complete cycles over the N samples）  
正弦信号也是如此。  
一组信号的频率已经确定的话那么剩下的就是振幅（amplitude）。  

傅里叶分解的意义在于
1. 一组复杂的信号能有正弦曲线叠加而来。比如说声音信号。  
2. 线性系统对于正弦曲线的响应总是正弦曲线。  
3. 傅里叶分解是傅里叶分析的基础，同时也包括拉普拉斯，Z变换。  
