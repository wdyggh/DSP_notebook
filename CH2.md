

# CH2 - Statistics, Probability and Noise

### Mean and Standard Deviation

Mean 平局值，通常用$u$(小写希腊字母 *mu* )来表示，  
假设一组型号的长度为$N$，  
那么他的Mean可以用以下公式来表示，  
$u=\frac{1}{N}\sum_{i=0}^{N-1}x_i$  
也可以写成  
$u=(x_0+ x_1 + x_2+...+x_N-1)/N$  
在电子学领域，Mean 被成为 DC(direct current) 成分，  
AC(alternating current) 指的是信号在 Mean 值附近的波动。  

Standrad Deviation 标准方差 $\sigma$(小写希腊字母 *sigma*)  
$\sigma^2=\frac{1}{N-1}\sum_{i=0}^{N-1}(x_i-u)^2$  
也可以写成  
$\sigma=\sqrt{(x_0-u)^2+(x_1-u)^2+...+(x_{N-1}-u)^2 \;/(N-1)}$  
标准方差描述了信号偏离了平均值多远，  
方差讲的是偏离(波动)的能量(power)。  
另一个熟悉的名词就是 **rms** (root-mean-square)，  
根据方差定义，标准方差描述的是信号的 AC 部分，  
rms 则同时包括了 AC 和DC 成分。  
如果一个信号没有 DC 成分，那信号的 rms 值就接近于 标准方差。  

以上计算标准方差的公式中，包括了一组完整的信号，  
当有新的信号需要添加时，需要重新计算，  
浪费了计算机的能量。  
接下来要介绍一种新的方法计算标准方差，  
具有高效低噪音的效果。  
$\sigma^2=\frac{1}{N-1}[\sum_{i=0}^{N-1}x_i^2 - \frac{1}{N}(\sum_{i=0}^{N-1}x_i)^2]$  
或者表示为  
$\sigma^2=\frac{1}{N-1}[sum\; of\; squares - \frac{sum^2}{N}]$  
当接收一组信号时，有3个重要的因素：
* 已经处理信号的长度  
* 以上信号的和(sum)  
* 以上信号平方的和(sum of squares)  
知道这3点就能轻松计算出mean和standard deviation。  

### Signal vs. Underlying Process

信号，潜在加工
在实际操作中，长度为N的随机信号的典型错误可以表示为：  
$Typical \; error =\frac{\sigma}{N^{1/2}}$  
如果长度N是一个非常小的值，那么计算得到的mean的error就会很大，  
因为长度为N的信号不能充分代表整个信号，  
所以N要足够大，当N近似$\infty$时，error才近似$0$。  

然而当要计算整个信号的标准方差时，只知道N个信号的mean  
如果使用N个信号的mean计算，就包括了一个统计噪音(statistical noise)  
为了反映这个噪音，在计算标准方差时，用 $N-1$ 代替了$N$。  
如果N足够大时，差距没那么明显，反之，使用$N-1$代替则显得更加精确。  

### The Histogram, Pmf and Pdf

直方图描述了一组信号在所有值所占信号数。  
假设有一组长度为 256000 的 8bit 信号，  
那么他的直方图就描述了信号在0~255内各个值得信号数量。  
如下图：  

![DSP_CH2_histogram.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_histogram.PNG)

在直方图中，所有值得和等于信号的长度  
$N=\sum_{i=0}^{M-1}H_i$  
$N$ 是信号长度，$H_i$是各个值的信号数量，$M$是值的数量。  

从直方图也能方便计算出平均值和标准方差  
$u=\frac{1}{N}\sum_{i=0}^{M-1}i H_i$  

标准方差：  
$\sigma^2 = \frac{1}{N-1} \sum_{i=0}^{M-1}(i-u)^2H_i$  

**probability mass function** (pmf) 概率质量函数  
在计算直方图时，总是使用有限的信号长度，  
但pmf应该需要无限的信号长度。  
实际中，pmf是可以从直方图预测出来。  
换句话说，直方图中的值除以信号的长度，就能得到近似pmf的图。  
pmf的值总是在0~1之间，并且各个值的和为1。  
pmf描述的是下个产生的值为多少的概率。  
由以上直方图得到的pmf如下：  

![DSP_CH2_pmf.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_pmf.PNG)

**直方图和概率质量函数只适用于离散信号，像储存在电脑的数字。**  
想要处理连续信号，比如电压的连续变化。  
就要用到 **probability distribution function** (pdf) 概率密度函数  
pdf是一条连续的曲线，从$-\infty$ 到 $+\infty$，曲线以下的面积和为1。  

![DSP_CH2_pdf.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_pdf.PNG)

如果要计算图中120~121之间的概率，可以近似为 $(120-121)\times0.03=0.03$  

在实际操作中，你会发现直方图的x轴不一定都是整数，  
不是整数的话，一定区间内就会有无限种可能的值，可能对应不同的数。  
当然要细分所有的可能性，也是不现实的。  
所以在考虑小数的情况下可以将x轴分块(bins)。  
原始信号如下：  

![DSP_CH2_histo_sig.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_histo_sig.PNG)

分成601块时：  

![DSP_CH2_histo1.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_histo1.PNG)

分成9块时：  

![DSP_CH2_histo2.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_histo2.PNG)

### The Normal Distribution  

正太分布，又叫高斯分布，是以德国数学家高斯的名字命名。  
原始曲线的基本形状可以表示成：  
$y(x)=e^{-x^2}$  
如下图：  

![DSP_CH2_Gaussian1.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_Gaussian1.PNG)

原始曲线加上mean和标准方差就能构成完整的高斯分布曲线。  
$P(x)=\frac{1}{\sqrt{2\pi}\;\sigma}\,e^{-(x-u)^2/2\sigma^2}$  

mean$=0,\sigma=1$ :  

![DSP_CH2_Gaussian2.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_Gaussian2.PNG)

mean$=20,\sigma=3$ :  
![DSP_CH2_Gaussian3.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_Gaussian3.PNG)

在统计学中，高斯分布是充分统计，因为只要知道$u$,$\sigma$就能确定整个分布。  

**cumulative distribution function**(cdf) 累计分布函数  
cdf是pdf的积分。用$\phi(x)$来表示  
正太分布的cdf如下：  
![DSP_CH2_Gaussian_cdf.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_Gaussian_cdf.PNG)

可以看出最大值为1，说明正太分布曲线下的面积和为1。  
$\phi(-2)=0.0228$指的是$-\infty$ 到$-2$的概率为 $2.28\%$  
$\phi(1)=0.8413$指的是$-\infty$ 到$1$的概率为 $84.13\%$  
如果要计算$-2\to1$之间的概率的话 $\phi(1)-\phi(-2)=0.8185$  

### Digital Noise Generation

在数字信号处理中，有些时候需要产生不同随机噪音，并添加到数据中来验证算法的可行性。  
假如有一组mean$=0.5, \;\sigma =1/\sqrt{12}$随机信号RND，  
![DSP_CH2_Noise1.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_Noise1.PNG)

当$x=RND+RND$时，x的mean是1，variance也是之前的2倍，标准方差则是$\sqrt2$倍。  
![DSP_CH2_Noise2.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_Noise2.PNG)

当$x=RND+RND+... +RND(12\; times)$时，x的mean是$6.0$，variance也是之前的$12$倍，标准方差则是$1$倍。  
![DSP_CH2_Noise3.PNG](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH2_Noise3.PNG)  
