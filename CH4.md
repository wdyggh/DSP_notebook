
# CH4 - DSP Software

### Computer Numbers

### Fixed Point (Integers)

### Floating Point (Real Numbers)

### Number Precision

### Execution Speed: Program Language

### Execution Speed: Hardware

### Execution Speed: Programming Tips

尽可能使用整数计算，来代替小数。  
尽量少用$sin(x),\; log(x),\; y^x,\; etc$  

$sin(x)=x-\frac{x^3}{3!}+ \frac{x^5}{5!}- \frac{x^7}{7!}+ \frac{x^9}{9!}- ...$  

$cos(x) = 1- \frac{x^2}{2!}+ \frac{x^4}{4!}- \frac{x^6}{6!}+ \frac{x^8}{8!}- ...$  

$e^x= 1+ x + \frac{x^2}{2!}+ \frac{x^3}{3!}+ \frac{x^4}{4!}+ \frac{x^5}{5!}+...$  

这就解释了为什么这些函数计算的比较慢。  