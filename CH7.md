
# CH7 - Properties of Convolution

卷积的性质，首先会介绍常用的几种脉冲响应，然后是关于级联和并联的线性系统，  
再是相关性的介绍，最后是卷积的一个实质性问题。

### Common Impulse Response

#### Delta Function
最简单的脉冲响应就是Delta Function，  
在卷积中，所有信号和Delta Function 卷积输出的还是原来信号。  
用公式表示成 $x[n] \ast \delta[n] = x[n]$。  

对Delta Function稍作改变，就能得到不同的输出。  
$x[n] \ast k\, \delta[n] =k\, x[n]$ 对信号进行缩放。  
$x[n] \ast \delta[n+s] = x[n+s]$ 对信号提前，或者延迟。  
$x[n] \ast (\delta[n]+ \delta[n+s])$ shows an impulse response composed of a delta function plus a shifted and scaled delta function. By superposition, the output of this system is the input signal plus a delayed version of the input signal, i.e., an $echo$. Echoes are important in mant DSP applications. The addition of echoes is a key part in making audio recordings sound natural and pleasant. Radar and sonar analyze echoes to detect aircraft and submarines.  

#### Calculus-like Operations

。。。

#### Low-pass and High-pass Filters

接下来会了解一下低通高通滤波器的核（filter kernels or filter's impulse response）。  
In general, Low-pass filter kernels are composed of a group of adjacent positive points.  
This results in each sample in the output signal being a weighted average of many adjacent points from the input signal.  

**For Low-pass filter kernels**
* Exponential kernel - recursive filter (kalman filter)  
* Square pulse - reducing noise while maintaining edge sharpness  
* Sinc - $sin(x)/(x)$ is used to separate one band of frequencies from another  

滤波器的设计通常是先设计相应的低通滤波器，然后在转换成相应的高通，等等。  
In filter design, the common strategy is devise a low-pass filter, then transfrom it to what you need, high-pass, band-pass, band-reject, etc.  
为了更好理解从低通到 高通的转换，只要记住delta函数的脉冲响应是对整个信号的作用，当然低通滤波器只对信号中低频部分有作用。  
To understand the low-pass to high-pass transform, remember that a delta function impulse response passes the entire signal, while a low-pass impulse response passes only the low-frequency components.  
从叠加的角度来讲，高通内核 = Delta Function - 低通内核。??  
By superposition, a filter kernel consisting of a delta function minus the low-pass filter kernel will pass the entire signal minus the low-frequency components.  
高通内核的特点是在DC成分上没有增益（gain），所以如果把高通内核值相加，会得到0。  

#### Causal and Noncausal Signals

A system is causal if its output depends only on the current input and past inputs (and not on future inputs).  
[reference](https://dsp.stackexchange.com/questions/27143/what-is-causal-signal?answertab=active#tab-top)

#### Zero Phase, Linear Phase, and Nonlinear Phase

Zero Phase: 内核关于0的位置左右对称。  
Linear Phase: 内核关于非0的位置左右对称。  
Nonlinear Phase: 内核不是左右对称。  

### Mathematical Properties

#### Commutative Property

$a[n] \ast b[n] = b[n] \ast a[n]$

#### Associative Property

$(a[n] \ast b[n])\ast c[n] =a[n] \ast ( b[n]\ast c[n]) $

#### Distributive Property

$a[n] \ast b[n] + a[n] \ast c[n] = a[n] \ast (b[n] \ast c[n])$

#### Transference between the Input and Output

$x[n] \ast h[n] =y[n]$
$x[n] \to Linear Change =x'[n]$
$y'[n] = x'[n] \ast h[n]$ and $y'[n] = y[n] \to Same Linear Change$

#### The Central Limit Theorem

如果一个类似脉冲的信号和自己卷积多次，会得到和高斯分布相似的曲线。  
The central Limit Theorem has an interesting implication for convolution. If a pulse-like signal is convoled with itself many times, a Gaussian is product.  

### Correlation

This is the problem: given a signal of some known shape, what is the best way to determine where(or if) the signal occurs in another signal. Correlation is the answer.  

相关也是一种数学方法和卷积类似，由2组信号作为输入，得到1组输出信号（互相关）。如果输入信号是2组相同的信号，得到结果称为自相关。以上关系可以可以看下图。  
Correlation is a mathematical operation that is very similar to convolution. Just as with convolution, correlation uses 2 signals to produce a third signal. The third signal is called the cross-correlation of the 2 input signals. If a signal is correlated with itself, the resulting signal is instead called the autocorrelation. 

![conv_cross](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH6_CONV1.png)

相关的意义在于找出目标信号，而不是在结果中重新塑造一个差不多的目标信号。  
Expect for this noise, the peak generated in the cross-correlation signal is symmetrical between its left and right. This is true even if the target signal isn't symmetrical. In addition, the width of the peak is twice the width of the target signal. Remember, the cross-correlation is trying to **detect** the target signal, not **recreat** it.  

利用相关来找出已知波形的方法称为匹配滤波器。  
Using correlation to detect a known waveform is frequently called **matched filtering**.

相关和卷积的不同之处在于第二个信号是否要翻转。  
卷积$a[n] \ast b[n] =c[n]$，对应的互相关$a[n] \ast b[-n]=c[n]$。  

**Convolution** is the relationship between a system's input signal, output signal, and impulse response.  
**Correaltion** is a way to detect a known waveform in a noisy background.  

关于FFT卷积的内容会在17章中介绍。  
