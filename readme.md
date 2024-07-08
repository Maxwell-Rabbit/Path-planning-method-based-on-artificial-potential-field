> 人工势场法路径规划是由Khatib提出的一种虚拟力法（Oussama Khatib，Real-Time Obstacle Avoidance for Manipulators and Mobile Robots. Proc of The 1985 IEEE.）。它的基本思想是将机器人在周围环境中的运动，设计成一种抽象的人造引力场中的运动，目标点对移动机器人产生“引力”，障碍物对移动机器人产生“斥力”，最后通过求合力来控制移动机器人的运动。应用势场法规划出来的路径一般是比较平滑并且安全，但是这种方法存在局部最优点问题。
> ——百度百科

利用matlab建立m文件编写程序利用人工势场法进行规划路径，首先将障碍物分布图转化为一张图像模式为索引颜色、格式为png格式的图片，然后使用matlab打开这张图片，在图片上选择起点和终点，matlab将根据终点计算引力场，根据图片求得障碍物边界，根据障碍物边界求得斥力场，再将引力场和斥力场相结合得出人工势场，并根据人工势场结合起点计算路径。
引力场的产生只和终点以及到终点的距离有关，引力场的计算公式为
$$f=\frac{1}{2}\times k\times d^{2}$$
其中k为引力系数，d为到终点的距离。
斥力场的计算只和障碍物边界以及到障碍物边界的最小距离有关。在求斥力时需要设定一个阈值，当到达障碍物距离的最小距离小于阈值时则按斥力场公式进行计算，当到障碍物边界最小距离大于阈值时则认为没有斥力。斥力场的计算公式为
$$f=\frac{1}{2}\times k\times (\frac{1}{d}-\frac{1}{t})^{2}$$
其中k为斥力系数，d为到障碍物边界的最小距离，t为阈值。
在求障碍物边界时，采用的是原图减去原图膨胀后图像再进行反相的方法。原图黑色为障碍区域，白色为通行区域。对原图进行膨胀处理后后障碍物区域缩小，即黑色区域缩小，白色区域扩大，用原图减去膨胀完成的区域后只有障碍物边界处为白色，其余区域为黑色，再进行反相将黑色和白色转换，实现了图像障碍物边界区域为黑色，其余地方为白色，成功提取障碍物边界。
路径规划最好的方法为梯度法，但为了方便程序编写使用的是局部区域最小值法，即先设定一个步长，以当前点为中心，步长为距离提取矩阵，在矩阵中寻找最小值，则最小值的坐标为路径的下一处坐标，并将最小值点设置为当前点，循环往复，直到到达人工势场中场最小的点，也就是设置的终点附近。在使用此方法时，若步长过大，有可能导致规划的路径穿过障碍物，若步长过小，则可能导致路径规划陷入局部最小值，也就是当前点为提取矩阵中的最小值的点。为了解决这个问题，在编写程序时增加了若当前点为提取矩阵的最小值点，即局部最小值情况时增加步长，但此种方法在有些场景下使用时会产生路径穿过障碍物的情况。

此程序运行后会显示障碍物图片，在图片中选择起点和终点后按回车，会出现引力场、斥力场、人工势场和路径规划图。运行结果如下所示。

![轨迹](https://github.com/Maxwell-Rabbit/Path-planning-method-based-on-artificial-potential-field/blob/main/photo/%E8%BD%A8%E8%BF%B9.png)

上图为人工势场法规划的路径，下图为人工势场法求得的引力和斥力叠加后的效果。

![势场](https://github.com/Maxwell-Rabbit/Path-planning-method-based-on-artificial-potential-field/blob/main/photo/%E5%8A%BF%E5%9C%BA.png)

引力场图像如下图所示。

![引力](https://github.com/Maxwell-Rabbit/Path-planning-method-based-on-artificial-potential-field/blob/main/photo/%E5%BC%95%E5%8A%9B.png)

斥力场图像如下图所示。

![斥力png](https://github.com/Maxwell-Rabbit/Path-planning-method-based-on-artificial-potential-field/blob/main/photo/%E6%96%A5%E5%8A%9Bpng.png)
