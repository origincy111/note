滤波
====
## 一、简介
### 1. 定义
滤波（Filtering）是信号处理中的一个重要概念，它的目的是**去除**数据中的**噪声**，**提取**有用的**信息**。在很多应用场景中（如传感器数据处理、室内定位、图像处理等），原始数据通常会受到外部环境的干扰，导致数据波动较大，影响后续计算和分析。滤波技术能够减少这些误差，使数据更加平滑、稳定、精准。
### 2. 作用
- **去除噪声**：减少数据中的随机波动，使信号更加稳定。
- **平滑噪声**：在轨迹计算、姿态估计等应用中，滤波可以消除突变点，使曲线更平滑。
- **提高定位精度**：在 GNSS、UWB、WiFi、蓝牙等定位系统中，滤波可以减少信号的干扰，提高定位准确性。
- **优化传感器数据**：如加速度计、陀螺仪的数据通常有噪声，滤波可以使数据更接近真实值
## 二、最简单的滤波：均值滤波
### 1. 原理
它的基本思想是 **取某个时间窗口（或区域）内的数据的平均值**，然后用这个均值代替当前值，从而减少数据中的高频噪声，使数据更平滑。
#### 数学表达式：

$$
y(i)=\frac{1}{N}\sum^{i+k}_{j=i-k}x(j)
$$
**其中**：
$\quad N$是窗口大小（如 3、5、7）。
$\quad x(j)$ 是原始数据。
$\quad y(i)$ 是滤波后的数据。

### 优点
- 计算简单，适用于大多数数据平滑场景
- 在信号变化平稳环境下效果好
  
### 缺点
- 对突变噪声效果不好，会变极端值拉偏
- 会导致信号延迟，滤波后数据滞后于真实值
  
## 三、最简单的滤波：均值滤波




---
卡尔曼滤波
---
## 一、入门
### 1. 适用系统：线性高斯系统
- 线性：$y=kx+b \qquad z=ax+by \qquad \omega=u^2+v^2$
  $线性\begin{cases}    叠加性\\  
    齐次性
  \end{cases}$
- 高斯：噪声满足正态分布
 
### 2. 宏观意义
滤波即**加权**
理想状态下：$信号\times 1+噪声\times 0$
低通滤波：$低频信号 \times 1+高频信号 \times 0$
卡尔曼滤波：$估计值\times p + 观测值\times (1-p)$

## 二、进阶
### 1. 状态空间表达式

$$
\begin{aligned}
&\textbf{状态方程} \qquad x_k=Ax_{k-1}+Bu_k+\omega_k \\
&\begin{array}{ll}
\textbf{其中：} & x_k为当前状态当前值 \\
 & x_{k-1}为上一个时刻该状态的值 \\
 & u_k为输入值 \\
 & \omega_k为过程噪声 \\
 & A为一个状态转移矩阵\\
 & B为一个控制矩阵
\end{array}\\\\
&\textbf{观测方程} \qquad y_k=Cx_k+v_k \\
&\begin{array}{ll}
\textbf{其中：} & y_k为观测量 \\
 & x_{k}为当前状态当前值 \\
 & v_k为观测噪声
\end{array}
\end{aligned}
$$
完整的空间转态方程如下图所示
![](FLTR_IMG/状态方程.png)

### 2. 高斯分布
参数分析
- 1. $\omega_k \in N(x|0, Q_k)\\
    v_k \in N(x|0, R_k)$
- 2. 方差
    - $一维方差\delta$
    - $ 二维协方差cov(X,Y) \qquad cov(X,Y)=\begin{bmatrix}  \delta_{11}&\delta_{12}\\\delta_{21}&\delta_{22}  \end{bmatrix}$  
    - $多维协方差矩阵C\\C=\begin{bmatrix}  cov(X_1,X_1)&cov(X_1,X_2)&\dots&cov(X_1,X_n)\\  cov(X_2,X_1)&cov(X_2,X_2)&\dots&cov(X_2,X_n)\\  \vdots&\vdots&\ddots&\vdots\\  cov(X_n,X_1)&cov(X_n,X_2)&\dots&cov(X_2,X_n)  \end{bmatrix}$
  
- 3. 超参数
    超参数是手动设置的系统参数，用于控制模型的行为和性能，超参数的选择和优化对模型性能有重要影响。
    PID控制算法的kp,ki,kd和卡尔曼滤波的Q,R即超参数

## 三、核心
### 1. 卡尔曼公式理解
- 实现过程：**使用上一次的最优结果预测当前的值**  
$\qquad\qquad$**同时使用观测值修正当前值，得到最优结果**
- 公式
  $$
  \begin{array}{lclc}
    预测：&&更新：\\[4pt]
    \widehat{x}^-_t=F\widehat{x}_{t-1}+Bu_{t-1}&(1)&
    K_t=P^-_tH^T(HP_t^-H^T+R)^{-1}&(1)\\[4pt]
    P^-_t=FP_{t-1}F^T+Q&(2)&
    \widehat{x}_t=\widehat{x}_t^-+K_t(z_t-H\widehat{x}_t^-)&(2)\\[4pt]&&
    P_t=(I-K_tH)P_t^-&(3)
  \end{array}
  $$
  $$
  \begin{array}{lll}
  \textbf{其中：} & \widehat{x}^-_t为t时刻的先验估计 & K_t为卡尔曼增益 \\
  & \widehat{x}_{t}为t时刻的最优估计 & R为观测噪声的方差\\ 
  & P^-_t为t时刻的先验协方差矩阵 & \widehat{x}_t为最优估计值\\
  & Q为过程噪声协方差矩阵 & P_t为t时刻的协方差矩阵
  \end{array}
  $$
- 实例:
  假设有一作匀加速直线运动的小汽车，我们讨论其两个状态，$Position$和$Velocity$,将其构成一个二维矢量$x_t=\begin{bmatrix}
    p\\v
  \end{bmatrix}$  
  #### (1). 预测
  对此我们可以推断出一个**预测模型**：
  $$
  \left. 
    \begin{array}{l}
    P_i=P_{i-1}+v_{i-1} \cdot \triangle t+\displaystyle\frac{a}{2}\triangle t^2\\
    v_i=v_{i-1}+a\cdot\triangle t
    \end{array}
   \right\}\Longrightarrow\begin{bmatrix}
    p_i\\v_i
   \end{bmatrix}=
   \begin{bmatrix}
      1&\triangle t\\
      0&1
    \end{bmatrix}\begin{bmatrix}
    p_{i-1}\\v_{i-1}
   \end{bmatrix}+
   \begin{bmatrix}
    \displaystyle \frac{\triangle t^2}{2}\\[8pt]\triangle t
   \end{bmatrix}a_i
  $$
  将右侧矩阵模型与预测公式(1)对应，可得：
  $$
  \begin{bmatrix}
    p_i\\v_i
   \end{bmatrix}\rightarrow\widehat{x}_t^-
  \quad
  \begin{bmatrix}
      1&\triangle t\\
      0&1
    \end{bmatrix}\rightarrow F
  \quad
  \begin{bmatrix}
    p_{i-1}\\v_{i-1}
   \end{bmatrix}\rightarrow x_t
  \quad
  \begin{bmatrix}
    \displaystyle\frac{\triangle t^2}{2}\\[8pt]\triangle t
   \end{bmatrix}\rightarrow B
  \quad
  a_i\rightarrow u_{t-1}
  $$
  $
  \textcircled{1}先验估计\\ 
  \qquad \Rightarrow \widehat{x}_t^- = F\widehat{x}_{t-1}+Bu_{t-1}\\  
  \textcircled{2}先验估计协方差\\ 
  \qquad \Rightarrow \widehat{P}_t^- = F\widehat{P}_{t-1}F^T+Q\\
  $
  #### (2). 测量
  假设有一个卫星仅观测汽车的$Position$，类似地可以推出一个**测量模型**：
  $$
  \left. 
    \begin{array}{l}
    z_p=P_t+\triangle P_t\\
    z_v=0
    \end{array}
   \right\}\Longrightarrow\begin{bmatrix}
    z_p\\z_v
   \end{bmatrix}=
   \begin{bmatrix}
      1&0\\
      0&0
    \end{bmatrix}\begin{bmatrix}
    p_{t}\\v_{t}
   \end{bmatrix}+
   \begin{bmatrix}
    1&0\\
    0&0
   \end{bmatrix}\begin{bmatrix}
    \triangle P_t\\\triangle v_{t}
   \end{bmatrix}
  $$
  同样地，将右侧矩阵模型与测量方程对应，可得：
  $$
  \begin{bmatrix}
    p_i\\v_i
   \end{bmatrix}\rightarrow 测量值z_t
  \quad
  \begin{bmatrix}
      1&\triangle t\\
      0&1
    \end{bmatrix}\rightarrow 观测矩阵H
  \quad
  \begin{bmatrix}
    p_{t}\\v_{t}
   \end{bmatrix}\rightarrow 观测值x_t
  \quad
  \begin{bmatrix}
    \triangle P_t\\\triangle v_{t}
   \end{bmatrix}\rightarrow 观测噪声v
  $$
  $\textcircled{3}测量方程\\
  \qquad \Rightarrow z_t=Hx_t+v（z_t的维数不一定与\widehat{x}_t相同）
  $
  #### (3). 状态更新
  $\textcircled{4}修正估计\\
  \qquad \Rightarrow \widehat{x}_t=\widehat{x}_t^-+K_t(z_t-H\widehat{x}^-_t) \qquad\qquad \textcolor{red}{(最终滤波结果)}\\
  \textcircled{5}更新卡尔曼增益\\
  \qquad \Rightarrow K_t=\displaystyle\frac{P_t^-H^T}{HP^-_tH^T+R}\\
  \textcircled{6}更新后验估计协方差\\
  \qquad \Rightarrow P_t=(I-K_tH)P^-_t
  $
### 2. 调节超参数
- #### (1). Q与R的取值
  - I. 公式层面的理解
  $$
  \left.
    \begin{array}{l}
     P_t^-=FP_{t-1}F^T+Q\\
     K_t=\displaystyle\frac{P_t^-H^T}{HP^-_tH^T+R}
     \end{array}
     \right\}\xrightarrow{令F,H=1(即一维状态下)可化简为}K=\displaystyle\frac{P_{t-1}+Q}{P_{t-1}+Q+R}
  $$
  $
  再根据预测公式:\widehat{x}_t=\widehat{x}_t^-+K(\widehat{z}_t-H\widehat{x}_t^-)\\
  \textcircled{1}当预测值更可信（模型更精确），K更小，\frac{R}{P_{t-1}+Q}更大\\
  \textcircled{2}当观测值更可信（传感器精度更高），K更大，\frac{R}{P_{t-1}+Q}更小 
  $
  - II. 其他层面的理解
  $
  \textcircled{1}Q为过程噪声的方差，模型越理想越小，反之越大\\
  \textcircled{2}R为观测噪声的方差,传感器越精确越小，反之越大
  $
- #### (2). $P_0$与$\widehat{x}_0$的取值
  - 习惯取$\widehat{x}_0=0，P $ 往小了取,方便收敛(一般取1，不可为0)
### 3. 卡尔曼滤波的使用
#### (1). 选择状态量，观测量
#### (2). 构建方程
#### (3). 初始化参数
#### (4). 代入公式迭代
#### (5). 调节超参数

## 四、精通
