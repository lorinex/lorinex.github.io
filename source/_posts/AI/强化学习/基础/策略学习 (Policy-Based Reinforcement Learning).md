---
categories:
  - AI
  - 强化学习
  - 基础
---
本文的主要内容是 **策略学习(policy-based reinforcement learning)** 和 **策略梯度(policy gradient)** 。策略学习的意思是通过求解一个优化问题，学出最优策略函数或它的近似函数(比如，策略网络 policy network)。

## 策略函数近似

### 1.1 策略函数 $\pi(a \mid s)$

假设动作空间是离散的，我们知道**策略函数** $\pi$($a \mid s$) 是一个条件概率密度函数(probability density function): $\pi(a \mid s) = P(A=a \mid S=s)$ 
它的输入是状态 s ，输出是动作空间$\mathcal{A}$ 中每个动作的概率值。

举个例子，我们把超级玛丽游戏当前屏幕上的画面作为 s ，策略函数会输出每个动作的概率值：$\pi(左 \mid s) = 0.5$,  $\pi(右 \mid s) = 0.2$,  $\pi(上 \mid s) = 0.3$. 如果有这样一个策略函数 $\pi(a \mid s)$ ，我们就可以用它控制智能体。每当观测到一个状态 s ，我们就可以根据策略函数做随机抽样，抽取一个动作 a ，让智能体执行动作 a 。

但是我们能够直接学习到**策略函数** $\pi(a \mid s)$吗？

如果状态和动作都是有限的，那么我们可以画一个表格学习策略函数。

![](https://pic4.zhimg.com/80/v2-d7d47a49e68a1fdb1f69c1af694e4e7b_720w.webp)

图 3.1 通过表格学习策略函数

但如果状态和动作是无限的呢？那么我们可以使用神经网络近似策略函数，即**策略网络(policy network)**。

### 1.2 策略网络(Policy Network)

想要得到这样一个策略函数，当前最有效的方法是用神经网络 $\pi(a \mid s;\theta)$ 近似策略函数 $\pi(a \mid s)$ 。我们把这个神经网络 $\pi(a \mid s;\theta)$ 称为**策略网络**， $\theta$ 表示神经网络的参数。一开始随机初始化 $\theta$ ，随后利用收集的状态、动作、奖励更新 $\theta$ 。

![](https://pic1.zhimg.com/80/v2-334b4bfd5f86a80cd11927e873e55e0c_720w.webp)

图 3.2 策略网络的结构

策略网络的结构如上图 3.2 所示，策略网络的输入是状态 s ，经过卷积神经网络或全连接神经网络处理输入。策略网络输出层的激活函数是 softmax ，所以输出的向量(记作 f )所有元素都是正数，且相加等于 1，即 $\sum_{a \in \mathcal{A}}\pi(a \mid s; \theta) = 1$ 。动作空间 $\mathcal{A}$ 的大小是多少，向量 f 的维度就是多少。

## 二、策略学习的目标函数

我们知道**状态价值函数(State-value function)** $V_{\pi}(s_t) = E_A\left [Q_{\pi}(s_t,A) \right ] = \sum_a\pi(a \mid s_t)\cdot Q_{\pi}(s_t,a)$ . 如果用策略网络 $\pi(a \mid s_t;\theta)$ 去近似策略函数 $\pi(a \mid s_t)$ ，那么我们可以得到状态价值函数的近似： $V(s_t;\theta) = \sum_a\pi(a \mid s_t; \theta)\cdot Q_\pi(s_t,a)$ 近似状态价值既依赖于当前状态 $s_t$ ，也依赖于策略网络 $\pi$ 的参数 $\theta$ 。

*_Approximate state-value function:_*
$$V(s;\theta) = \sum_a\pi(a \mid s; \theta)\cdot Q_\pi(s,a)$$
如果一个策略很好，那么状态价值函数的近似 $V(s;\theta)$ 的均值应当很大。
因此我们定义**目标函数**： $J(\theta) = E_S\left [V(S;\theta) \right]$ 
目标函数 $J(\theta)$ 排除了状态 S 的因素，只依赖于策略网络 $\pi$ 的参数 $\theta$ 。
策略越好，则 $J(\theta)$ 越大，
所以策略学习可以被看作是这样一个优化问题： $max_\theta{J(\theta)}$ 通过学习参数 $\theta$ ，使得目标函数 $J(\theta)$ 越来越大，也就意味着策略网络越来越好。

怎样提升参数 $\theta$ 呢？答案是可以使用策略梯度上升更新 $\theta$ ，使得 $J(\theta)$ 增大。设当前策略网络的参数为 $\theta_{now}$ ，做梯度上升更新参数，得到新的参数
$$\theta_{new}:\theta_{new} \leftarrow \theta_{now} + \beta\cdot\nabla_\theta{J(\theta_{now})}$$
此处的 $\beta$ 是学习率，需要手动调。
其中的梯度： $\nabla_\theta{J(\theta_{now})} = \frac{\partial J(\theta)}{\partial \theta} \mid _{\theta=\theta_{now}}$ 称为**策略梯度(policy gradient)**。

## 三、策略梯度(Policy Gradient)

### 3.1 策略梯度定理（不严谨的表述） 
$$\frac{\partial J(\theta)}{\partial \theta} = E_S\left [E_{A\sim{\pi(\cdot\mid S;\theta)}}\left [\frac{\partial \ln{\pi(A\mid S;\theta)}}{\partial \theta}\cdot Q_\pi(S,A) \right] \right]$$ 
>注：上述策略梯度定理不严谨，严格地说，这个定理只有在”状态 S 服从马尔可夫链的稳态分布 $d(\cdot)$ “这个假设下才成立。期望前面应该有一项系数 $1 + \gamma + \cdots + \gamma^{n-1} = \frac{1-\gamma^n}{1-\gamma}$ ，其中 $\gamma$ 是折扣率， n 是一个回合的长度。因此，策略梯度应该是： $$\frac{\partial J(\theta)}{\partial \theta} = \frac{1-\gamma^n}{1-\gamma}\cdot E_S\left [E_{A\sim{\pi(\cdot\mid S;\theta)}}\left [\frac{\partial \ln{\pi(A\mid S;\theta)}}{\partial \theta}\cdot Q_\pi(S,A) \right] \right]$$ 在实际应用中，系数 $\frac{1-\gamma^n}{1-\gamma}$ 可以忽略，原因是在做梯度上升的时候，系数 $\frac{1-\gamma^n}{1-\gamma}$会被学习率 $\beta$ 吸收。

### 3.2 策略梯度定理证明

有些复杂，这里不证，感兴趣的可以看王树森老师的深度强化学习书籍 101~106 页。

### 3.3 近似策略梯度

我们知道**策略梯度**公式为： $$\nabla_\theta J(\theta) = E_S\left [E_{A\sim{\pi(\cdot\mid S;\theta)}}\left [Q_\pi(S,A)\cdot \nabla_\theta \ln{\pi(A\mid S;\theta)} \right] \right]$$ ，但是求出这个期望是不可能的，因为我们并不知道状态 S 的概率密度函数；即使知道，能够通过连加或定积分求出期望，我们也不愿意这样做，应为连加或定积分的计算量非常大。

我们可以通过期望的蒙特卡洛方法去近似策略梯度，每次从环境中观测到一个状态 s ，它相当于随机变量 S 的观测值。然后再根据当前的策略网络（策略网络的参数必须是最新的）随机抽样得出一个动作： $a\sim \pi(\cdot \mid s;\theta)$ 
计算随机梯度： $g(s,a;\theta) = Q_\pi(s,a)\cdot \nabla_\theta \ln{\pi(a\mid s;\theta)}$ 
很显然， $g(s,a;\theta)$ 是策略梯度 $\nabla_\theta J(\theta)$ 的无偏估计： $\nabla_\theta J(\theta) = E_S\left [E_{A\sim{\pi(\cdot\mid S;\theta)}}\left [g(S,A;\theta) \right] \right]$ 
然后我们就可以使用近似策略梯度做随机梯度上升来更新 $\theta$ ，使得目标函数 $J(\theta)$ 逐渐增大： 
$\theta \leftarrow \theta + \beta\cdot g(s,a;\theta)$ 

## 四、使用策略梯度更新策略网络

**算法：**

1、在 t 时刻观测到状态 $s_t$ ；

2、根据策略网络 $\pi(\cdot \mid s_t;\theta)$ 随机抽样一个动作 $a_t$ ；

3、计算动作价值 $q_t \approx Q_\pi(s_t,a_t)$ ；

4、计算策略网络关于参数 $\theta$ 的微分 $$\mathrm{d}\theta = \frac{\partial \ln\pi(a_t\mid s_t;\theta)}{\partial \theta}\mid_{\theta=\theta_t}$$

5、计算近似策略梯度 $g(a_t,\theta_t) = q_t \cdot \mathrm{d}\theta$ ；

6、更新策略网络： $\theta_{t+1} \leftarrow \theta_t + \beta\cdot g(a_t,\theta_t)$ 。

**问题：**

在第 3 步中，怎么计算 $q_t$ ？

**方法：**

在后面章节中，我们用两种方法对 $Q_\pi(s,a)$ 做近似。

1、REINFORCE 算法

用实际观测的回报 u 近似 $Q_\pi(s,a)$ 。

2、actor-critic 算法

用神经网络 $q(s,a;w)$ 近似 $Q_\pi(s,a)$ 。