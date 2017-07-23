
# CH8 - The Discrete Fourier Transform

### The Family of Fourier Transform

Fourier 是由 Jean Baptiste Joseph Fourier 命名而来。  
Fourier分析的基础是把信号拆成正弦、余弦成分。  

Fourier为什么用正弦信号来分解，而不是其他方波，三角波，等等。事实上分解一组信号有很多种方法，但是分解的目的是为了能够找到一种比处理原信号跟简单的方法。  

sinusoidal fidelity(正弦保真度): If the input to a linear system is a sinusoidal wave, the output will also be a sinusoidal wave, and at exactly the same frequency as the input. Sinusoids are the only waveform that have this property. Although a sinusoid on the input guarantees a sinusoid on the output, the two may be different in amplitude and phase. The frequency and wave shape must remain the same.  

Fourier Transform 能分成4大类，根据信号的连续或离散，周期或非周期。  
* Aperiodic-Continuous $\to$ Fourier Transform
* Periodic-Continuous $\to$ Fourier Series
* Aperiodic-Discrete $\to $ Discrete Time Fourier Transform
* Periodic-Discrete $\to$ Discrete Fourier Transform

4种Fourier变换都能细分成实数或者复数。实数的版本计算量比较简单，复数的话考虑到使用复数，会相对的复杂。Complex mathematics can quickly become overwhelming, even to those that specialize in DSP.  
在数学中变换（Transform）作为一个很重要的部分存在，同样在数字信号处理领域：Fourier transform, Laplace transform, Z transform, Hilbert transform, Discrete Cosine transform, etc. 那么什么是变换（Transform）？简单举例来说 $y=2x+1$方程，给定的x，就有对应的y作为输出。稍复杂些$y=2a+3b+4c$，输出也可由几个输入同时决定。变换就是这个的拓展，允许同时多个输入多个输出。举例，有长度为100的信号，经过你写的算法，输出也有100（或者其它长度）的长度，那么就有你的变换了。  

### Notation and  Format of the Real DFT

Discrete Fourier transform 的$N$点输入信号，拥有$N/2+1$点的输出信号。

![DSP_CH8_RealDFT1.png](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH8_RealDFT1.png)  

The term frequency domain is used to describe the amplitudes of the sine and cosine waves.  
在时域中代表信号长度的 N，N首先是正数，通常是2的次方，一方面方便使用二进制读写，第二在FFT中，通常使用2的次方作为FFT的大小。通常N从32~4096中选取。  

### The Frequency Domain's Independent Variable

假如，有一个长度为128 的信号进行DFT运算，在时域上的信号表示为$x[0]\; to\; x[127]$，在频域上的信号能表示为$Re\,X[0] \; to\; Re\,X[64]$ 和 $Im\,X[0] \; to\; Im\,X[64]$，注意的是长度为128的输入，得到长度为 $N/2+1 =65$ 的输出，就是 $0~64$。  

在DSP中有4中表示Fourier transform后的数据，k 代表了数据的索引，从0~N/2；f是从0~0.5；w 从0~\pi，或者用hz代表频率。  

### DFT Basic Functions

DFT的基本公式是由以下2个方程而来，$c_k[i]=cos(2\pi ki/N)$，$s_k[i]=sin(2\pi ki/N)$。$c_k[\;]$是cosine波的振幅在$Re\,X[k]$，$s_k[\;]$是sine波的振幅在$Im\,X[k]$。$c_0[\;]$是cosine波频率为0的值，也表示$Re\,X[0]$拥有整个信号的平均值（DC offset）在时域上。  

### Synthesis, Calculating the Inverse DFT

Synthesis equation: $x[i] = \sum_{k=0}^{N/2}\,Re \,\bar{X}[k]\,cos(2 \pi ki/N) + \sum_{k=0}^{N/2}\,Im \,\bar{X}[k]\,sin(2 \pi ki/N)$  
...

### Analysis, Calculating the DFT

two methods:  
* simultaneous equations  
* correlation (if DFT has less than about 32 points)  
* Fast Fourier Transform(will discuss in CH12)  

### Duality

### Polar Notation

Magnitude of X[] $\to \; Mag\,X[\;]$  
Phase of X[] $\to \; Phase\,X[\;]$  

$Mag\,X[k]=(ReX[k]^2+ImX[k]^2)^{1/2}$  
$Phase\,X[k] = arctan(\frac{ImX[k]}{ReX[k]})$  
$ReX[k] = Mag\,X[k]\;cos(Phase\,X[k])$  
$ImX[k] = Mag\,X[k]\;sin(Phase\,X[k])$  

### Polar Nuisances

Radians vs. Degrees
