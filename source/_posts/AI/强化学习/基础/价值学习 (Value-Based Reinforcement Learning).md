---
categories:
  - AI
  - 强化学习
  - 基础
---
## Deep Q-Network(DQN)

我们希望知道**最优动作价值函数** $Q^\star$ ，因为 $Q^\star$ 就像先知一样，可以在 t 时刻就预见 t 到 n 时刻之间的累计奖励的期望。假如我们知道 $Q^\star$ ，我们就可以根据 $Q^\star$ 的值选择**最优动作(best action)** $a^\star = argmax_{a}Q^\star(s, a)$ ，然后就可以最大化未来的累计奖励。

但是我们并不知道 $Q^\star$ 的函数表达式，那么有没有办法可以近似出 $Q^\star$ 呢？在实践中，我们会近似学习 $Q^\star$ ，最有效的办法是 **Deep Q Network(DQN)**，使用神经网络 $Q(s,a;w)$ 去近似 $Q^\star(s, a)$ ，其结构如下图：

![图 2.1: DQN 的神经网络结构。输入是状态 s；输出是每个动作的 Q 值。](https://pic2.zhimg.com/80/v2-215d5fd667db2a42a0e191253e4db00d_720w.webp)

图 2.1: DQN 的神经网络结构。输入是状态 s；输出是每个动作的 Q 值。

**DQN** 的表达式为 $Q(s,a;w)$ ，其中的 w 为神经网络中的参数。训练 **DQN** 时，首先随机初始化 w ，然后用“经验”去学习 w ，需要对 **DQN** 关于神经网络参数 w 求梯度：
$$\nabla _w{Q(s,a;w)} = \frac{\partial{Q(s,a;w)}}{\partial w}$$ 上式表示函数值 $Q(s,a;w)$ 关于参数 w 的梯度。

**DQN** 的输出是离散动作空间 $\mathcal{A}$ 上的每个动作的 Q 值，即给每个动作的评分，分数越高意味着动作越好。

我们可以将 **DQN** 应用到玩游戏当中，在游戏的一个回合中，我们观测到 t 时刻的状态 $s_t$ ，然后根据 **DQN** 选出能够使 t 时刻 Q 值最大的动作 $a_t$ ，执行动作 $a_t$ ，得到奖励 $r_t$ 和状态转移 $s_{t+1}$ ，依次进行，直到这一回合结束。如下图所示：
![](Pasted%20image%2020230714144458.png)
图 2.2: Apply DQN to Play Game

## 三、时间差分算法(Temporal Difference Learning)

训练 **DQN** 最常用的算法是时间差分 (temporal difference，缩写 TD) 算法。

先举个例子：

我们有一个模型 $Q(s,d;w)$ ，其中 s 是起点， d 是终点， w 是参数，模型 Q 可以预测开车出行的时间开销。假设我从北京开车去广州，模型 Q 预计我会花费 14 个小时，当我到达广州时，我知道自己的实际花费时间是 16 小时。那么我们怎么去更新这个模型 Q 呢？

我们可以在出发前先让模型做预测： `q = Q("北京","广州";w) = 14` ，当我们完成旅途时得到实际时间 y = 16 。我们把这次的旅程作为一组训练数据，然后用梯度下降对模型做一次更新。

我们希望估计值 `q = Q("北京","广州";w)` 尽可能接近真实观测到的 y ，所以用两者的平方作为损失函数：
$$L(w) = \frac{1}{2}\left [Q(s,d;w) - y \right ]^2$$
然后使用链式法则对 L(w) 关于参数 w 求梯度： $$\frac{\partial{L}}{\partial{w}} = \frac{\partial{q}}{\partial{w}} \cdot \frac{\partial{L}}{\partial{q}} = (q-y) \cdot \frac{\partial{Q(w)}}{\partial{w}}$$ ，最后做梯度下降更新模型参数 $$w:w_{t+1} = w_t - \alpha \cdot \frac{\partial{L}}{\partial w} \mid _{w=w_t}$$ 。此处的 $\alpha$ 是学习率，是个超参数，需要手动调。

在完成一次梯度下降之后，如果再让模型做一次预测，那么模型的预测值 `Q("北京","广州";w)`会比原先更接近 y = 16 。

但是，我可以在完成旅程之前就更新这个模型吗？答案是可以的，我们可以用 **TD** 算法在完成旅程之前就更新模型 Q 。

接着这个例子，我从北京出发到广州途中会经过郑州，从北京出发后经过 4 小时，我到达郑州。假设到达郑州时我的车坏了，必须要在郑州修理，我不得不取消行程，那么未完成行程的这组数据能否用来更新模型 Q 呢？答案是可以的，我们可以用 **TD(Temporal Difference)** 算法更新模型 Q 。

出发前模型估计全程时间为` q = Q("北京","广州";w) = 14` ，我从北京出发后经过$r = 4$ 小时到达郑州，此时再让模型做一次预测，模型告诉我 `q = Q("郑州","广州";w) = 11` 。到达郑州后，根据模型最新估计，整个旅程的总时间为： $y = r + q^\prime = 4 + 11 = 15$ ，**TD** 算法将 y = 15 称为 **TD 目标(TD target)**，它比最初的预测 q = 14 更可靠。

所以我们可以用 TD target y 对模型进行更新，我们希望估计值 q 尽量接近 y ，用两者的平方作为损失函数：$$L(w) = \frac{1}{2}\left [Q(s,d;w) - y \right ]^2$$  把 $Q(s,d;w) - y$ 记作 $\delta$ ，也叫 **TD 误差(TD error)**，做梯度下降更新模型参数 $w$ 。

## 四、用 TD 算法训练 DQN(TD Learning for DQN)

我们知道**折扣回报** $U_t = R_t + \gamma R_{t+1} + \gamma ^2R_{t+2} + \gamma ^3R_{t+3} + ...$ ，可以将其写为以下形式：
![](Pasted%20image%2020230714144916.png)

其中括号里的式子则被记作 $U_{t+1}$ ，那么我们会得到如下等式：

$U_t = R_t + \gamma \cdot U_{t+1}$ **DQN** 在 t 时刻的输出： $Q(s_t,a_t;w)$ ，是对 $U_t$ 的估计；在 t+1 时刻的输出 $Q(s_{t+1},a_{t+1};w)$ ，是对 $U_{t+1}$ 的估计。因此，我们可以得到如下等式：

$Q(s_t,a_t;w) \approx r_t + \gamma \cdot Q(s_{t+1},a_{t+1};w)$**使用 TD learning 训练 DQN 流程：**

1、在 t 时刻做预测 Prediction: $Q(s_t,a_t;w_t)$ .

2、计算 TD target: $y_t = r_t + \gamma \cdot Q(s_{t+1},a_{t+1};w_t) = r_t + \gamma \cdot max_aQ(s_{t+1},a_;w_t)$ .

3、计算损失函数 Loss: $L_t = \frac{1}{2}\left [Q(s_t,a_t;w_t) - y_t \right ]^2$ .

4、做梯度下降更新参数 w : $w_{t+1} = w_t - \alpha \cdot \frac{\partial{L_t}}{\partial w} \mid _{w=w_t}$ .

