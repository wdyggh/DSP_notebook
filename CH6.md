
# CH6 - Convolution

卷积是把2组信号结合，产生第三组信号。  
卷积在数字信号处理中有着非常重要的意义，  
卷积不光涉及到了输入输出信号，还有脉冲响应（impulse response）。  

### The Delta Function and Impulse Response

CH5 中讲到了脉冲分解（将信号分解乘N组，每组在N的位置上值为原值，其他都为0），  
脉冲分解其实是提供了一次分析一个样本的方法，  
当使用这方法时分析时，这过程在数学上叫卷积。  

Delta函数在DSP中是重要成分之一，  
Delta函数通常是用$\delta[n]$表示，  
在$\delta[n=0]=1,\; \delta[n\neq0]=0$。  
Delta函数也被叫做单位脉冲（unit impulse）。  

脉冲响应（通常用$h[n]$表示）是一个单位脉冲经过一个系统得到输出。  
$x[n](\delta[n])\to |System|\to y[n](h[n])$  
所有脉冲都能用delta函数的位移（shifted）缩放（scaled）来表示。  
一组信号$a[n]$，在位置8上的值为-3，可以看成Delta函数经过位移放大，  
用Delta函数表示为 $a[n]=-3\delta[n-8]$。  

假如一个系统的输入为脉冲（$-3\delta[n-8]$）,那么输出可以用什么表示呢？  
已知输入为$\delta[n]$时，输出为$h[n]$，那么输入为$-3\delta[n-8]$，输出为$-3h[n-8]$。  
换句话说，输出可以看成脉冲响应（$h[n]$），经过位移和缩放的结果。  
如果知道一个系统的脉冲响应，那么就了解了系统对所有脉冲的响应。  

### Convolution

1. 信号能被拆分成N组脉冲信号，这些脉冲信号能用Delta函数的位移和缩放来表示。  
2. 输出结果是每个脉冲响应（$h[n]$）的位移和缩放。  
3. 输出信号能用脉冲响应（$h[n]$）的位移和缩放的和。  

如果知道一个系统的脉冲响应，那么就可以计算对所有可能输入的输出，也就知道了这个系统的所有。  

卷积的算法很简单，只有乘法，加法，积分。  
在线性系统中，常用卷积来描述输入信号，脉冲响应，还有输出信号的关系。  
在线性系统中卷积可以表示乘这样  
$x[n]\to|Linear\;System\;h[n]|\to y[n]$  
$x[n]\ast h[n]=y[n]$，卷积是用$\ast$表示  

卷积能作为高低通滤波器，反向衰减器，discrete derivative(first difference) related to the first slop of the input signal。  
卷积输出信号长度 = 2组输入信号长度和-1。  
卷积的理解可以从2个角度来看， 输入信号 和输出信号。  

### The Input Side Algorithm

$x[n]\ast h[n]=y[n]$，可以看成是$x[n]$卷积$h[n]$得到$y[n]$。  
从DSP的角度来理解的话，先分解输入信号，得到的多个结果合成。  
举例，$x[n]$可以分解成脉冲信号$x_{k,\; k\in (0,n)}[k]$。  
当$k=4$，$x[4]=1.4$ 对应的单位脉冲信号为 $1.4\,\delta[n-1]$，  
经过系统后，得到的脉冲响应为 $1.4\,h[n-4]$。  
基本脉冲响应乘1.4，再向右平移4个单位。  
其他脉冲信号也是如此。  
如果把$x[n]$和$h[n]$调换位置，得到的结果也是相同的。  
所以$a[n]\ast b[n]=b[n]\ast a[n]$这就是conv的一个重要性质之一。  

~~~ c
x[30];
h[10];
y[39];  // length = 30+10-1

//init y[]=0;
for(i=0:30)
    for(j=0:10)
        y[i+j] = y(i+j) + x[i]*h[j]
    end
end
~~~

### The Output Side Algorithm

在输出端的第n个值，是多组输入信号和脉冲响应的结合。  
举例：$y[6]=x[3]h[3]+x[4]h[2]+x[5]h[1]+x[6]h[0]$  

这里的卷积可以看出脉冲响应$h[n]$，在计算时经过了翻转。  
如果不翻转的话就是 cross-correlation。  
下图就能清楚的表示2者之间的关系。  
![conv_cross](http://7xifyp.com1.z0.glb.clouddn.com/DSP_CH6_CONV1.png)

在数学上，我们可以用公式来定义卷积  
如果 $x[n]$是长度为 $N$的信号，其索引从$0\to N-1$，  
并且$h[n]$是长度为 $M$的信号，其索引从$0\to M-1$，  
那么两者的卷积可表示为$y[n]=x[n]\ast h[n]$，是长度为 $N+M-1$的信号，其索引从$0 \to N+M-2$。  
卷积和的公式可以写为：$y[i]=\sum_{j=0}^{M-1}h[j]x[i-j]\;\;i\in(0,\;N+M-2)$  
如果$i-j<0$，也就是$x[-1],x[-2]...$，此时需要zero padding。  
如果$i-j>N$，也就是$x[N+1],x[N+2]...$，此时也需要zero padding。  

### The Sum of Weighted Inputs

在卷积中用weighting coefficients 代替 脉冲响应的话，  
在输出端的结果是，输入信号经过weighted的和。  

