---
title: 空间解析几何
date: 2023-11-04 22:25:30 +0800
categories: [Learning, Math]
tags: [Analytic-Geometry]
math: true
excerpt: 总结了一些常见的空间解析几何的相关问题
---

### 直线的方向向量
+ 对于由

$$\dfrac{x-x_0}{A}=\dfrac{y-y_0}{B}=\dfrac{z-z_0}{C}$$

确定的直线，其方向向量为：

$$\boldsymbol{s}=(A,B,C)$$

+ 对于由方程组

$$\begin{cases}
    x=At+x_0\\[8pt]
    y=Bt+y_0\\[8pt]
    z=Ct+z_0
\end{cases}$$

确定的直线，其方向向量为：

$$\boldsymbol{s}=(A,B,C)$$

+ 对于由方程组

$$\begin{cases}
    A_1x+B_1y+C_1z+D_1=0\\[8pt]
    A_2x+B_2y+C_2z+D_2=0
\end{cases}$$

确定的直线，其方向向量为：

$$\begin{aligned}\boldsymbol{s}&=(A_1,B_1,C_1)\times(A_2,B_2,C_2)\\[8pt]
&=\begin{bmatrix}
\boldsymbol{i}&\boldsymbol{j}&\boldsymbol{k}\\[8pt]
A_1&B_1&C_1\\[8pt]
A_2&B_2&C_2
\end{bmatrix}\end{aligned}$$

### 平面的法向量

对于由方程

$$Ax+By+Cz+D=0$$

确定的平面，其法向量为：

$$\boldsymbol{n}=(A,B,C)$$


### 点到平面的距离
设 $P_0(x_0,y_0,z_0)$ 为空间中一点，$\varGamma:Ax+By+Cz+D=0$ 为空间中一平面，则 $P_0$ 到 $\varGamma$ 的距离 $d$ 为：

$$d=\dfrac{\left|Ax_0+By_0+Cz_0+D\right|}{\sqrt{A^2+B^2+C^2}}$$

推导过程如下：
+ 由平面方程，可得平面 $\varGamma$ 的法向量 $\boldsymbol{n}$ 为：

$$\boldsymbol{n}=(A,B,C)$$

+ 设 $P_1(x_1,y_1,z_1)$ 为平面 $\varGamma$ 上任一点，则有：

$$Ax_1+By_1+Cz_1+D=0$$

+ 设 $\theta$ 为向量 $\overrightarrow{P_1P_0}$ 与 $\boldsymbol{n}$ 的夹角，则有：

$$\cos\theta=\dfrac{\overrightarrow{P_1P_0}\cdot\boldsymbol{n}}{\left|\overrightarrow{P_1P_0}\right|\cdot\left|\boldsymbol{n}\right|}$$

+ 则 $P_0$ 到 $\varGamma$ 的距离 $d$ 可以表示为：

$$\begin{aligned}
    d&=\left|\left|\overrightarrow{P_1P_0}\right|\cdot\cos\theta\right|\\[8pt]
    &=\left|\dfrac{\overrightarrow{P_1P_0}\cdot\boldsymbol{n}}{\left|\boldsymbol{n}\right|}\right|\\[8pt]
    &=\dfrac{\left|A(x_0-x_1)+B(y_0-y_1)+C(z_0-z_1)\right|}{\sqrt{A^2+B^2+C^2}}\\[8pt]
    &=\dfrac{\left|Ax_0+By_0+Cz_0-(Ax_1+By_1+Cz_1)\right|}{\sqrt{A^2+B^2+C^2}}\\[8pt]
    &=\dfrac{\left|Ax_0+By_0+Cz_0+D\right|}{\sqrt{A^2+B^2+C^2}}
\end{aligned}$$

### 点到直线的距离
设 $P_0(x_0,y_0,z_0)$ 为空间中一点， $L:\dfrac{x-x_1}{A}=\dfrac{y-y_1}{B}=\dfrac{z-z_1}{C}$ 为空间中一直线，则 $P_0$ 到 $L$ 的距离 $d$ 的求解过程如下：

+ 由直线方程，可知点 $P_1(x_1,y_1,z_1)$ 在直线上，且直线的方向向量 $\boldsymbol{s}$ 为：

$$\boldsymbol{s}=(A,B,C)$$

+ 根据向量外积的含义：两个向量外积的模长与以这两个向量为边的平行四边形的面积相等，即可得到 $P_0$ 到 $L$ 的距离 $d$ 的表达式为：

$$d=\dfrac{\left|\overrightarrow{P_1P_0}\times\boldsymbol{s}\right|}{\left|\boldsymbol{s}\right|}$$

+ 将其展开，可得：

$$\begin{aligned}
    d&=\dfrac{\left|\overrightarrow{P_1P_0}\times\boldsymbol{s}\right|}{\left|\boldsymbol{s}\right|}\\[8pt]
    &=\dfrac{\begin{vmatrix}
    \boldsymbol{i}&\boldsymbol{j}&\boldsymbol{k}\\
    x_0-x_1&y_0-y_1&z_0-z_1\\
    A&B&C
    \end{vmatrix}}{\sqrt{A^2+B^2+C^2}}
\end{aligned}$$

### 两直线的夹角
已知两条直线方程分别为：

$$\begin{aligned}L_1&:\dfrac{x-x_1}{A_1}=\dfrac{y-y_1}{B_1}=\dfrac{z-z_1}{C_1}\\[8pt]
L_2&:\dfrac{x-x_2}{A_2}=\dfrac{y-y_2}{B_2}=\dfrac{z-z_2}{C_2}\end{aligned}$$

则其夹角 $\theta$ 的求解方法如下：

+ 根据两直线方程，得到 $L_1$ 、$L_2$ 的方向向量分别为 $\boldsymbol{s}_1$ 、$\boldsymbol{s}_2$ ：

$$\begin{aligned}
    \boldsymbol{s}_1&=(A_1,B_1,C_1)\\[8pt]
    \boldsymbol{s}_2&=(A_2,B_2,C_2)
\end{aligned}$$

+ 则夹角 $\theta$ 的余弦值可以表示为：

$$\cos\theta=\dfrac{\boldsymbol{s}_1\cdot\boldsymbol{s}_2}{\left|\boldsymbol{s}_1\right|\cdot\left|\boldsymbol{s}_2\right|}$$

### 两直线的公垂线
已知两条直线方程分别为：

$$\begin{aligned}L_1&:\dfrac{x-x_1}{A_1}=\dfrac{y-y_1}{B_1}=\dfrac{z-z_1}{C_1}\\[8pt]
L_2&:\dfrac{x-x_2}{A_2}=\dfrac{y-y_2}{B_2}=\dfrac{z-z_2}{C_2}\end{aligned}$$

则其公垂线 $L_3$ 的求解方法如下：

+ 根据两直线方程，得到 $L_1$ 、$L_2$ 的方向向量分别为 $\boldsymbol{s}_1$ 、$\boldsymbol{s}_2$ ：

$$\begin{aligned}
    \boldsymbol{s}_1&=(A_1,B_1,C_1)\\[8pt]
    \boldsymbol{s}_2&=(A_2,B_2,C_2)
\end{aligned}$$

+ 则 $L_3$ 的方向向量 $\boldsymbol{s}_3$ 为：

$$\boldsymbol{s}_3=\boldsymbol{s}_1\times\boldsymbol{s}_2$$

+ 由 $L_1$ 、$L_3$ 确定的平面 $\varGamma_1$ 的法向量 $\boldsymbol{n}_1$ 为：

$$\boldsymbol{n}_1=\boldsymbol{s}_1\times\boldsymbol{s}_3$$

+ 由 $L_2$ 、$L_3$ 确定的平面 $\varGamma_2$ 的法向量 $\boldsymbol{n}_2$ 为：

$$\boldsymbol{n}_2=\boldsymbol{s}_2\times\boldsymbol{s}_3$$

+ 不妨设 $\boldsymbol{n}_1=(a_1,b_1,c_1)$ ，$\boldsymbol{n}_2=(a_2,b_2,c_2)$，则公垂线 $L_3$ 即为平面 $\varGamma _1$ 与平面 $\varGamma _2$ 的交线，其方程为：

$$\begin{cases}
    a_1(x-x_1)+b_1(y-y_1)+c_1(z-z_1)=0\\[8pt]
    a_2(x-x_2)+b_2(y-y_2)+c_2(z-z_2)=0
\end{cases}$$

### 过直线的平面
在空间中，经过一条给定直线的平面有无数个，可以采用平面束方程的方式将它们表示出来。

比如，对于以如下形式给出的直线 $L$ ：

$$\begin{cases}
    A_1x+B_1y+C_1z+D_1=0\\[8pt]
    A_2x+B_2y+C_2z+D_2=0
\end{cases}$$

那么，所有经过 $L$ 的平面均可以用下式表示：

$$\lambda(A_1x+B_1y+C_1z+D_1)+\mu(A_2x+B_2y+C_2z+D_2)=0\qquad(\lambda,\mu\in\mathbb R)$$