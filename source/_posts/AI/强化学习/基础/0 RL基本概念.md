---
categories:
  - AI
  - 强化学习
  - 基础
---
## Terminologies(名词)

**1、智能体(Agent)**

强化学习的主体被称为**智能体 (agent)**。

**2、环境(Environment)**

**环境 (environment)** 是与智能体进行交互的对象，可以抽象地理解为交互过程中的规则或机制。

**3、状态(State)/观测(Observation)**

在每个时刻，环境有一个**状态 (state)**，可以理解为对当前时刻环境的概括。

**状态(State)** 有时也被称为**观测(Observation)**，因为有时智能体并不能观测到环境改变后的全部，只能观测到部分。

**4、状态空间(State Space)**

**状态空间 (state space)** 是指所有可能存在状态的集合，记作花体字母 $\mathcal{S}$ 。

状态空间可以是离散的，也可以是连续的。状态空间可以是有限集合，也可以是无限可数集合。

**5、动作(Action)**

**动作 (action)** 是智能体基于当前状态所做出的决策。

**7、动作空间(Action Apace)**

**动作空间 (action space)** 是指所有可能动作的集合，记作花体字母 $\mathcal{A}$ 。

动作空间可以是离散集合或连续集合，可以是有限集合或无限集合。

在超级玛丽例子中，动作空间是 $\mathcal{A}$ = {左, 右, 上}。在围棋例子中，动作空间是 $\mathcal{A}$ = {1, 2, 3, · · · , 361}。

**8、奖励(Reward)**

**奖励 (reward)** 是指在智能体执行一个动作之后，环境返回给智能体的一个数值。奖励往往由我们自己来定义，奖励定义得好坏非常影响强化学习的结果。

**9、策略(Policy)**

**策略 (policy)** 的意思是根据观测到的状态，如何做出决策，即如何从动作空间中选取一个动作。

强化学习的目标就是得到一个策略函数 (policy function)，也叫 $\pi$ 函数 ( $\pi$ function) ，在每个时刻根据观测到的状态做出决策。策略可以是确定性的，也可以是随机性的，两种都非常有用。

**10、状态转移(State transition)**

**状态转移 (state transition)** 是指智能体从当前 t 时刻的状态 s 转移到下一个时刻状态为 $s^\prime$ 的过程。

在超级玛丽的例子中，基于当前状态（屏幕上的画面），玛丽奥向上跳了一步，那么环境（即游戏程序）就会计算出新的状态（即下一帧画面）。

在中国象棋的例子中，基于当前状态（棋盘上的格局），红方让“车”走到黑方“马”的位置上，那么环境（即游戏规则）就会将黑方的“马”移除，生成新的状态（棋盘上新的格局）。

我们用**状态转移概率函数 (state transition probability function)** 来描述状态转移，记作

$p_t{(s^\prime \mid s,a)} = P(S^\prime_{t+1} = s\prime \mid S_t = s, A_t = a)$

表示这个事件的概率: 在当前状态 $s$ ，智能体执行动作 $a$ ，环境的状态变成 $s^\prime$ 。

**11、马尔可夫决策过程 (Markov decision process, MDP)**

强化学习的数学基础和建模工具是**马尔可夫决策过程 (Markov decision process，MDP)**。

一个 **MDP** 通常由状态空间、动作空间、状态转移函数、奖励函数、折扣因子等组成。

**12、模型**

通常指**与智能体交互的环境模型**，即对环境的**状态转移概率**和**奖励函数**进行建模

## Return and Value

**1、回报(Return)**

**回报 (return)** 是从当前时刻开始到本回合结束的所有奖励的总和，所以回报也叫做**累计奖励 (cumulative future reward)**。

把 t 时刻的回报记作随机变量 $U_t$ 。如果一回合游戏结束，已经观测到所有奖励，那么就把回报记作 $u_t$ 。设本回合在时刻 n 结束。定义回报为:

$U_t = R_t + R_{t+1} + R_{t+2} + R_{t+3} + ... + R_n$ 

回报是未来获得的奖励总和，所以智能体的目标就是让回报尽量大，越大越好。强化学习的目标就是寻找一个策略，使得回报的期望最大化。这个策略称为最优策略 (optimum policy)。

**2、折扣回报(Discounted Return)**

在 MDP 中，通常使用**折扣回报 (discounted return)**，给未来的奖励做折扣。折扣回报的定义如下:

$U_t = R_t + \gamma R_{t+1} + \gamma ^2R_{t+2} + \gamma ^3R_{t+3} + ...$ 这里的 $\gamma \in [0,1]$ 叫折扣率。对待越久远的未来，给奖励打的折扣越大。

**3、动作价值函数(Action-value function)** Q

表示在当前状态下选择某个动作，可以获得多大的累积奖励或长期回报。

假设我们已经观测到状态 $s_t$ ，而且做完决策，选中动作 $a_t$ 。那么 $U_t$ 中的随机性来自于 $t+1$ 时刻起的所有的状态和动作: $S_{t+1},A_{t+1},S_{t+2},A_{t+2}, ... , S_n,A_n$ .

对 $U_t$ 关于变量 $S_{t+1},A_{t+1}, ... , S_n,A_n$ 求条件期望，得到

$Q_\pi(s_t,a_t) = E_{S_{t+1},A_{t+1},...,S_n,A_n}\left [U_t \mid S_t=s_t,A_t=a_t \right ]$ 期望中的 $S_t = s_t$ 和 $A_t = a_t$ 是条件，意思是已经观测到 $S_t$ 与 $A_t$ 的值。
条件期望的结果 $Q_\pi(s_t,a_t)$ 被称作**动作价值函数 (action-value function)**。

动作价值函数 $Q_\pi(s_t,a_t)$ 依赖于 $s_t$ 与 $a_t$ ，而不依赖于 t+1 时刻及其之后的状态和动作，因为随机变量 $S_{t+1},A_{t+1}, ... , S_n,A_n$ 都被期望消除了。

**4、状态价值函数(State-value function)** V

$V_\pi(s_t) = E_{A_t \sim \pi(. \mid s_t)}\left [Q_\pi(s_t,A_t) \right ] =\sum_{a \in \mathcal{A}}\pi(a\mid s_t)Q_\pi(s_t,a)$ 公式里把动作 $A_t$作为随机变量，然后关于 $A_t$ 求期望，把 $A_t$ 消掉。得到的状态价值函数 $V_\pi(s_t)$ 只依赖于策略 $\pi$ 与当前状态 $s_t$ ，不依赖于动作。

状态价值函数 $V_\pi(s_t)$ 也是回报 $U_t$ 的期望: $V_\pi(s_t) = E_{A_t,S_{t+1},A_{t+1}, ... , S_n,A_n}\left [U_t\mid S_t=s_t \right ]$ .

期望消掉了 $U_t$ 依赖的随机变量 $A_t,S_{t+1},A_{t+1}, ... , S_n,A_n$ 。状态价值越大，就意味着回报的期望越大。用状态价值可以衡量策略 $\pi$ 与状态 $s_t$ 的好坏。

## 分类

- **基于模型的强化学习 model-based reinforcement learning**
	- 两种动态规划算法，即**策略迭代和价值迭代**，则是基于模型的强化学习方法
	- **Dyna-Q**
- **无模型的强化学习 model-free reinforcement learning**
	- 两种时序差分算法 **Sarsa 和 Q-learning** 算法

两个**评价指标**：
1. 算法收敛后的策略在初始状态下的期望回报
2. 样本复杂度，即算法达到收敛结果需要在真实环境中采样的样本数量。

基于模型的强化学习算法由于具有一个环境模型，智能体可以额外和环境模型进行交互，对真实环境中样本的需求量往往就会减少，因此通常会比无模型的强化学习算法具有更低的样本复杂度。
但是，环境模型可能并不准确，不能完全代替真实环境，因此基于模型的强化学习算法收敛后其策略的期望回报可能不如无模型的强化学习算法。