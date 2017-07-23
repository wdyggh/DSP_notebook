
# CH9 - Application of the DFT

* Calculate a signal's frequency spectrum  
* Find a system's frequency response  
* Used as an intermediate step in more elaborate signal processing techniques  

### Spectral Analysis of Signals

当一组信号的频率，相位变得比波形更加重要时，DFT可以是分析信号的很好工具。  

举例：用一个麦克风收集声音数据，用低通滤波器过滤80Hz以上的声音，再160采样速度，得到一组信号数据。在环境中会有噪音和目标声音同时被录下来，所以为了使信号更加干净，可以使用 Hamming window 在time domain。然后执行DFT，如果原信号中的噪音比较大的话，不管DFT取256，还是2048点，得到的结果会是一样，只是频域上的精度更加精确，噪音并没有变小。  
如果把原信号每隔256长度就切一段，经过Hamming window，再DFT，最后把每段的结果叠加，得到的频域结果的噪音会明显减少。在频域上的振幅被平均化了，相位的信息消失。  
第二种减少噪音的方法是，选择很长的DFT大小（16384）得到的分辨率就会很高，然后在频域上使用低通数字滤波器来平滑曲线，每隔64位取一点，最终得到256个点，也能得到相比较好的结果。  
第一种方法比较简单，计算量比较少，第二种用到数字滤波，所以相对复杂，但是能提供更好的效果，如果有更多的数据就能减少噪音带来的影响。  

影响频域上分辨率的因素：DFT的大小和输入信号的长度。为了区分2个peak的频率，那么采样间隔至少小于2个peak之间的距离。  
一组信号在进行DFT前经过Hamming window的话，会从3方面来影响输出。第一，peak会看起来比较相似；第二，peak的尾巴会比较陡；第三，peak会变宽，影响分辨率。  

How dose this windowing affect the frequency domain? -- when two domain signals are multiplied, the corresponding frequency domains are convolved.  
* Blackman window: widest main lobe(bad), lowest amplitude tails(good).  
* Rectangular window: narrowest main lobe(good), larget tails(bad).  
* Hamming window: sits between Blackman window and Rectangular window.  
* Flat-top window: exactly the same shape as the filter kernel of a low-pass filter, poor frequency resolution.  

[wiki window function](https://zh.wikipedia.org/wiki/窗函数#.E7.9F.A9.E5.BD.A2.E7.AA.97.28Rectangular.29)  

### Frequency Response of Systems

*A system's frequency response is the Fourier Transform of its impulse response.*  

在时域上我们常用卷积来分析系统的表现，在时域上我们也用类似的方法来分析。  

**Notation**  
$h[\;] \iff H[\;]$ 之前用 $h[\;]$表示脉冲响应，$H[\;]$通常用来表示频率响应。  

卷积的表示：convolution in the time domain corresponds to multiplication in the frequency domain.  
时域：$x[n] \ast h[n] = y[n]$  
频域：$X[f] \times H[f] = Y[f]$  

一组长64的脉冲响应，进行64点DFT后就能得到它的频率响应。  
假如进行512点DFT，会得到分辨率更好的频率响应。  
随着DFT的长度变大，分辨率会越来越来，假如长度趋向无穷大，分辨率也会趋向无穷。不管信号的长度为多少。  
尽管脉冲响应是离散信号，但得到频率响应是连续曲线。脉冲响应的N点DFT会得到 N/2+1 的连续曲线样本。  

### Convolution via the Frequency Domain

*An input signal and impulse response, transform the two signals into the frequency domain, multipy them, and then transform the result back into the time domain.*  

$x[n] \ast h[n] = y[n] \\ Re\,X[f] \,Re\,H[f] = Re\,Y[f] \\ Im\, X[f] \,Im\, H[f]=  Im\, Y[f]$  

原本卷积的过程，使用2次DFT，一次乘法，一次IDFT就能代替。尽管中间过程很不一样，但是输出结果是和卷积一样的。用DFT（FFT）代替卷积的好处是，在一定长度以上的信号作为输入时，会减少运算量。  

#### Circular convolution 循环卷积

正常的卷积 $x[n] \ast h[m] = y[l]$ y的长度为 N+M-1。  
但是通过DFT，相乘，IDFT得到的结果只跟 IDFT的长度有关。  
比较2个结果是明显不一样的，这时通过频域转换来的卷积称为循环卷积。  
假设，信号长度N=256，脉冲响应M=51，卷积后的长度为306。  
256点DFT的话结果还是256，同样IDFT后也是256点，问题在于306个样本怎么就会变成256个。  
为了更好理解DFT，可以把N点DFT的结果看成是N点为周期的循环信号，频域卷积把正确的306点信号，稍加重叠，变成256点长度的循环信号，重叠部分（overlap）为49点。  
知道了原因后就能了解如何避免，增大DFT的大小到512，输入信号长度不够时在最后加0，经过频域卷积后，会得到305点非0信号，和206点0信号。  
**总结一下，N点DFT决定了卷积是否循环。**  