+++
title = "计算机视觉复习笔记"
date = 2019-11-27
extra.math = true
+++

# 计算机视觉

## 图像处理

### 图像类型

- binary
- gray scale
- color

### 直方图均衡化

1. 灰度数组a[i32; L]，a[i]代表灰度值为i的像素所占百分比
2. 前缀和suma[i32: L]
3. $b[i]=\lceil a[i]*(L-1) \rceil$
4. 像素值x变为b[x]

### 数学形态学

#### 基本运算

- 膨胀（求绝对元映像范围内最大值）
- 腐蚀（求绝对元范围内最小值）
- 开运算（先腐蚀后膨胀）
- 闭运算（先膨胀后腐蚀）

> 结构元：奇形怪状的东东，比如 `<img src="https://i.loli.net/2021/06/30/E1gzVTfWjDoUsaG.png" alt="image-20210630203910970" style="zoom:50%;" />`
>
> 映像：180度旋转

### 滤波

#### 降噪

- 盒式滤波器
- 高斯滤波器`<img src="https://i.loli.net/2021/07/01/SosrWDkcY4G6h89.png" alt="image-20210701142324557" style="zoom:50%;" />`，可以将二维变成一维`<img src="https://i.loli.net/2021/07/01/R5JQDIp2s1zVbj8.png" alt="image-20210701144659994" style="zoom:50%;" />`
- 中值滤波（用于噪点多）

#### 锐化

> $\sqrt {x^2+y^2}通常近似为\left|x\right|+\left|y\right|$
>
> 锐化就是原图值加上 卷积结果(原图的梯度图)
> 一阶微分近似为 $f(x+1)-f(x)$
>
> 二阶偏微分近似为$f(x+1,y)+f(x-1,y)-2f(x,y)$

- 一阶微分算子 `<img src="https://i.loli.net/2021/07/01/t8HPxlqinCGO5JK.png" alt="image-20210701155644631" style="zoom:50%;" />`

  交叉Roberts![image-20210701155808222](https://i.loli.net/2021/07/01/TCt2rRvZ7J9M3Yq.png)![image-20210701160731563](https://i.loli.net/2021/07/01/k2YRpHWrLhtnmBU.png)

  Sobel![image-20210701155921754](https://i.loli.net/2021/07/01/PdmVUh3zvckb2ei.png)![image-20210701160705872](https://i.loli.net/2021/07/01/9ftJYpQ64GLH3ai.png)
- 二阶微分算子Laplace

  ![image-20210701160804948](https://i.loli.net/2021/07/01/MnjhNbAugZfxR4V.png)![image-20210701160812712](https://i.loli.net/2021/07/01/9TqApBZYbJirFWG.png)

## 局部图像特征

### 边缘检测

#### 基本滤波器

边缘检测+原始图像==锐化（

- Sobel
- Prewitt![image-20210701165536737](https://i.loli.net/2021/07/01/235QOLZGBxqNwAb.png)
- Roberts

#### 差分高斯滤波

> 常作为高斯拉普拉斯算子的近似
>
> 高斯拉普拉斯就是先高斯再拉普拉斯，由于卷积符合结合律，可以直接对高斯核进行拉普拉斯。

差分高斯滤波就是选择不同的标准差，把两个高斯相减

![image-20210701170732461](https://i.loli.net/2021/07/01/Fr4PNMx6GCU9RLD.png)

#### Canny

1. 高斯降噪
2. 计算梯度幅值和方向（方向拟合为四个方向![image-20210701171326981](https://i.loli.net/2021/07/01/3HyaRsJdYEKN718.png)
3. 非极大值抑制（只保留局部极大值，（比上下左右大？），使边缘变细）
4. 滞后阈值，设置高低阈值h、l，像素大于h或在l-h之间且连接着一个大于h的值才保留

### 角点检测

1. 求出图像灰度值两个方向的梯度$I_x,I_y$`<img src="https://i.loli.net/2021/07/01/pqU3F1jsN4XGw6g.png" alt="image-20210701214127441" style="zoom: 67%;" />`
2. 求出$I_x^2, I_y^2, I_xI_y$并用高斯核加权
3. 计算每个像素的Harris响应度R，并设置低阈值，`<img src="https://i.loli.net/2021/07/01/d8TPb9jav5WUwhs.png" alt="image-20210701214425872" style="zoom: 67%;" />`
4. 在一个固定大小窗口（例如5x5）内进行非极大值抑制，局部极大值即为角点
