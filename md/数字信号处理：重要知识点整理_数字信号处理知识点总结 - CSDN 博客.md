> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/Dabie_haze/article/details/105853124?spm=1001.2014.3001.5506)

#### 文章目录

*   [0 最重要！DFT 和 FFT 的区别](#0_DFTFFT_4)
*   [1 连续时间信号频域分析](#1__7)
*   [2 通过离散时间信号的 Z 变换表达式 X(z) 直接写出时域离散信号（序列）x(n) 的方法](#2_ZXzxn_30)
*   [3 部分分式法的 MATLAB 实现（求 X(z) 的部分展开式）](#3_MATLABXz_44)
*   [4 稳定系统](#4__57)
*   [5 求频响特性（系统函数 H 与对应的频点 w）](#5_Hw_65)
*   [6 求离散系统的响应（求出某输入信号通过该系统得到的输出）](#6__71)
*   [7 求系统的单位冲激响应](#7__76)
*   [8 求系统的零极点](#8__81)
*   [9 将零极点增益表示的 H(z) 转换成基本二阶形式](#9_Hz_86)
*   [10 周期 / 非周期、连续 / 离散信号的傅里叶变换 / 傅里叶级数 / DFT 小结](#10_DFT_97)
*   [11 加窗对信号频谱分析的影响](#11__100)
*   [12 fft 中的点数 L（即频域抽样的点数）的大小对频谱分析的影响](#12_fftL_105)
*   [13 各种窗函数的产生](#13__108)
*   [14 窗函数法设计 FIR 数字滤波器的步骤](#14_FIR_121)
*   [15 频率采样法设计 FIR 滤波器的步骤](#15_FIR_123)
*   [16 比较 FIR 滤波器的两种设计步骤的优缺点](#16_FIR_125)
*   [19 简述常用的 IIR 数字滤波器的设计方法](#19_IIR_135)
*   [21 简述由模拟滤波器转换为 IIR 数字滤波器的两种常用变换方法的优缺点](#21_IIR_140)
*   [17 脉冲响应不变法设计 IIR 数字滤波器的步骤](#17_IIR_154)
*   [18 双线性变换法设计 IIR 数字滤波器的步骤](#18_IIR_157)
*   [19 简述模拟滤波器转换为数字滤波器的要求和步骤](#19__160)
*   [20 简述巴特沃斯滤波器和切比雪夫滤波器的比较](#20__166)
*   [21 简述巴特沃斯型模拟低通滤波器设计步骤](#21__170)

> 写在前面：本文中涉及的函数的使用场景为 matlab

0 最重要！DFT 和 FFT 的区别
-------------------

[https://www.vfe.cc/NewsDetail-765.aspx](https://www.vfe.cc/NewsDetail-765.aspx)

1 连续时间信号频域分析
------------

1）**周期信号的傅里叶级数**  
① 三角形式的傅里叶级数：  
![](https://i-blog.csdnimg.cn/blog_migrate/cab5d8dd629d48f8335f17253f9c5254.png#pic_center)

② 指数形式的傅里叶级数：  
![](https://i-blog.csdnimg.cn/blog_migrate/19311cf147373913c828fb0537c4bf34.png#pic_center)

其中，傅里叶级数为 ![](https://i-blog.csdnimg.cn/blog_migrate/3b7dbf0631bffcb038ea6bc1c5541ffd.png#pic_center)

傅里叶级数 ak 其实也是权重，可以用于合成信号（合成效果和谐波次数 N 有关，N 越大越接近原信号），合成公式： ![](https://i-blog.csdnimg.cn/blog_migrate/6a2a61e1971748d30bdcef8615b07b28.png)

2）**非周期信号的傅里叶变换**  
① 傅里叶变换：  
![](https://i-blog.csdnimg.cn/blog_migrate/2e549f8afba5da749852949aec8bc4e5.png#pic_center)

② 傅里叶逆变换：  
![](https://i-blog.csdnimg.cn/blog_migrate/848f1518e5749b394024dc06807765ff.png#pic_center)

3）**周期信号的频谱是离散的，非周期信号的频谱是连续的；  
离散信号的频谱是周期的，连续信号的频谱是非周期的。**

2 通过离散时间信号的 Z 变换表达式 X(z) 直接写出时域离散信号（序列）x(n) 的方法
-----------------------------------------------

![](https://i-blog.csdnimg.cn/blog_migrate/733dc639809b6fda7b9c8a768223ce4a.png#pic_center)

因此 X(z) 的系数即序列 x(n) 的值（只有离散时间信号即序列可以不用计算直接写！）

> 例：  
> ![](https://i-blog.csdnimg.cn/blog_migrate/a96ed75032c9d1c284a26da1b0b1bc1e.png#pic_center)
> 
> 对应的 x1(n) 为：![](https://i-blog.csdnimg.cn/blog_migrate/b82c0b3f5e9a169f32e746e1e6a7486f.png#pic_center)
> 
> 注意是从高次幂到低次幂排列，原点即 z^0 的系数。但是对应的 n1 = [0:2] 是从小到大排列（注意原点位置）

3 部分分式法的 MATLAB 实现（求 X(z) 的部分展开式）
---------------------------------

1）函数与格式：[r,p,k] = **residuez**(b,a)  
2）用法：设返回参数 r,p,k 分别是：  
![](https://i-blog.csdnimg.cn/blog_migrate/d067d8540dbb8ac172462a3dcd004f4c.png#pic_center)

则  
![](https://i-blog.csdnimg.cn/blog_migrate/3ce6be23729eae580a6be4231e6ec5de.png#pic_center)

**注意同一个极点 p3 出现了两次，说明是二重极点**  
3）用于数字滤波器的并联型是通过系统函数（传递函数）H(z) 的部分展开式实现的，所以该函数也可以用于实现数字滤波器的并联型  
4）用 residuez 实现数字滤波器的并联型时，由于输出的 r,p 可能会有共轭复系数，需要转换成实数：  
[b1,a1] = resideuz(R1,P1,0)

4 稳定系统
------

1）定义：当输入序列是有界的，则输出序列也有界，称系统是稳定的。  
2）判断：  
① 通过**零极点的分布**来判断：（对于因果系统）  
**稳定**：H(z) 的全部极点都落在单位圆内，即收敛域应该包含单位圆在内  
**临界稳定**：一阶极点位于单位圆上（若有其他阶的，都在单位圆内），单位圆外无极点  
**不稳定**：有极点落在单位圆外，或者单位圆上有重极点

5 求频响特性（系统函数 H 与对应的频点 w）
------------------------

1）函数与格式：[H,w] = **freqz**(b,a,N)  
2）用法：输入 b 和 a 分别为系统函数 H(z) 的分子和分母系数矩阵，N 为正整数，默认为 512；  
输出 w 包含了 0-pi 范围内的 N 个频率等分点，H 是 w 对应的值  
3）也可以通过手算出系统函数 H(exp(j*w)) 实现

6 求离散系统的响应（求出某输入信号通过该系统得到的输出）
-----------------------------

1）函数与格式：y = **filter**(b,a,Xn)  
2）用法：实现差分方程的求解，因此这个只能用于离散系统（连续系统对应微分方程）求响应。其中 b 和 a 分别是差分方程的输出 y 和输入 x 的系数，Xn 是输入信号，y 是通过该系统的输出信号  
3）当输入信号 Xn 为单位冲激信号（即单位脉冲信号）或单位阶跃信号时，可以用该函数求系统的单位冲激响应（即单位脉冲响应）或单位阶跃响应。

7 求系统的单位冲激响应
------------

1）可以通过上面的 **filter** 函数求  
2）函数与格式：y = **impz**(b,a,N)  
3）用法：b 和 a 同上，N 表示冲激响应输出的序列个数，输出 y 是 N 个时域点对应的响应值。如果直接输出 impz(b,a,N) 可以直接画图，不用 stem([0:N-1],y)

8 求系统的零极点
---------

1）函数与格式：[z,p,k] = **tf2zp**(b,a)  
2）用法：其中输入参数 b 是系统函数 H(z) 中分子的系数向量，a 是分母的系数向量。输出的 z 为零点，p 为极点，k 为常数  
3）也可以用 roots 函数分别求分子和分母的方程根，来求出系统的零极点

9 将零极点增益表示的 H(z) 转换成基本二阶形式
--------------------------

1）函数与格式：sos = **zp2sps**(z,p,k)  
2）用法：其中 z 是零点，p 是极点，k 是常数项，输出的 sos 是矩阵：  
![](https://i-blog.csdnimg.cn/blog_migrate/1d14fb8653322fc0a62c1ce9d7e3e74f.png#pic_center)

对应的系统函数为（这只写出了某一行，多行的要相乘）：  
![](https://i-blog.csdnimg.cn/blog_migrate/36bc51072d0e93c4943cfacb13d15c32.png#pic_center)

（注意这里的 a 就是分母的系数，写成系统函数时直接代数相加即可，但是在画图时级联型中的 a 是需要改变符号的）  
3）由于数字滤波器的级联型是通过 H(z) 的基本二阶形式实现的，所以该函数也可以用于实现级联型

10 周期 / 非周期、连续 / 离散信号的傅里叶变换 / 傅里叶级数 / DFT 小结
--------------------------------------------

![](https://i-blog.csdnimg.cn/blog_migrate/27bb2f2ff84fa33da8aa2338f93b3b6a.png#pic_center)

11 加窗对信号频谱分析的影响
---------------

1）如果窗的宽度越大，即时间序列截取的越长，其频谱的旁瓣占的比例越小。当窗口宽度无限大时，即截取所有的时间序列，则只有主瓣，没有旁瓣。  
2）**频谱泄露**是不可避免的，因为任何窗函数都不可能满足宽度无限大。但是选择好的窗函数，可以尽可能减少能量的泄露。  
3）好的窗函数，是窗函数的频谱尽可能衰减的快，即主瓣和旁瓣的比例尽可能大。

12 fft 中的点数 L（即频域抽样的点数）的大小对频谱分析的影响
----------------------------------

补零对原信号来说并**没有增加任何信息**，但是补零相当于对原信号的频谱做插值（时域增加采样点的个数，频域中频谱分辨率减小），**能够减少频谱泄露**。

13 各种窗函数的产生
-----------

1）三角窗：bartlett、triang  
2）布莱克曼窗：blackman  
3）矩形窗：boxcar  
4）汉明窗：hamming  
5）汉宁窗：hanning  
6）切比雪夫窗：chebwin  
7）凯塞窗：kaiser

![](https://i-blog.csdnimg.cn/blog_migrate/175844a1f130e426594dd4216bb157b1.png#pic_center)

14 窗函数法设计 FIR 数字滤波器的步骤
----------------------

_（待补充）_

15 频率采样法设计 FIR 滤波器的步骤
---------------------

_（待补充）_

16 比较 FIR 滤波器的两种设计步骤的优缺点
------------------------

1）**窗函数法**  
![](https://i-blog.csdnimg.cn/blog_migrate/5ec54ff205d0754a96d4b7cb242a2e8e.png#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/c42aac641a573c230adb1dcc697c64f6.png#pic_center)  
![](https://i-blog.csdnimg.cn/blog_migrate/b64a796f1ebe85d4e53dead9e1ec8645.png#pic_center)

2）**频率采样法**  
![](https://i-blog.csdnimg.cn/blog_migrate/eaf3c71b02fb8f7460f7a2738c9b8542.png#pic_center)

19 简述常用的 IIR 数字滤波器的设计方法
-----------------------

![](https://i-blog.csdnimg.cn/blog_migrate/236f779a5d72968fb5ab69d6ce288ca0.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/2104901d62d0ab66be8270f3a5909997.png)

21 简述由模拟滤波器转换为 IIR 数字滤波器的两种常用变换方法的优缺点
-------------------------------------

1）**脉冲响应不变法**：![](https://i-blog.csdnimg.cn/blog_migrate/a90767e50efff670313b7625d8a751ce.png#pic_center)

补充：脉冲响应不变法存在**频谱混叠**现象的原因是：数字滤波器频响是模拟滤波器频响的周期延拓。解决办法：  
![](https://i-blog.csdnimg.cn/blog_migrate/5f18af1b235c11fa844e0f549b9caefe.png#pic_center)

2）**双线性变换法**：  
优点：通过实现 Z 平面到 S 平面的映射，解决了脉冲响应不变法的混叠失真问题  
缺点：频率之间的非线性变换问题，会产生新的问题：  
1）一个线性相位的模拟滤波器经双线性变换后得到非线性相位的数字滤波器，不再保持原有的线性相位了。  
2）这种非线性关系要求模拟滤波器的幅频响应必须是分段常数型的，不然变换所产生的数字滤波器幅频响应相对于原模拟滤波器的幅频响应会有畸变。

17 脉冲响应不变法设计 IIR 数字滤波器的步骤
-------------------------

![](https://i-blog.csdnimg.cn/blog_migrate/cf46b4851eba4f15dcff812670d3826c.png#pic_center)

18 双线性变换法设计 IIR 数字滤波器的步骤
------------------------

![](https://i-blog.csdnimg.cn/blog_migrate/5b84044f0dd1847ee82204138d7fc6f0.png#pic_center)

19 简述模拟滤波器转换为数字滤波器的要求和步骤
------------------------

![](https://i-blog.csdnimg.cn/blog_migrate/f1cf67f7b72f7dd37c90047174d53114.png)  
![](https://i-blog.csdnimg.cn/blog_migrate/e7112298251b23befbcf9070c1d8ad51.png)

20 简述巴特沃斯滤波器和切比雪夫滤波器的比较
-----------------------

![](https://i-blog.csdnimg.cn/blog_migrate/f5e6e39f269857a2aeaebf82cfa1ef33.png#pic_center)

21 简述巴特沃斯型模拟低通滤波器设计步骤
---------------------

巴特沃斯型：  
![](https://i-blog.csdnimg.cn/blog_migrate/050db0d793318b4bc5d4d9676a80cb83.png#pic_center)

切比雪夫型：

![](https://i-blog.csdnimg.cn/blog_migrate/1ea53358925fb6c607d07535e861ac72.png#pic_center)