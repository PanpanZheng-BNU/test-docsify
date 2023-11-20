# this is guide

###### [[Papers_MOC | Papers MOC]]
# Pareto law in a kinetic model of market with random saving propensity
<small> (2004) - Arnab Chatterjee, Bikas K. Chakrabarti, S. S. Manna</small>

**Title**:: Pareto law in a kinetic model of market with random saving propensity
**Link**:: 
**DOI**:: 10.1016/J.PHYSA.2003.11.014
**Tags**:: #paper


```ad-quote
title: Abstract
We have numerically simulated the ideal-gas models of trading markets, where each agent is identified with a gas molecule and each trading as an elastic or money-conserving two-body collision. Unlike in the ideal gas, we introduce (quenched) saving propensity of the agents, distributed widely between the agents $(0 \leq \lambda<1)$. The system remarkably self-organizes to a critical Pareto distribution of money $P(m)\sim m^{-(\nu+1)}$ with $\nu\simeq 1$. We analyze the robustness (universality) of the distribution in the model. We also argue that although the fractional saving ingredient is a bit unnatural one in the context of gas models, our model is the simplest so far, showing self-organized criticality, and combines two century-old distributions: Gibbs (1901) and Pareto (1897) distributions. © 2003 Elsevier B.V. All rights reserved.
```

### Notes
#### The Distribution of Wealth
- **Pareto law**-个人具有收入或财富 $m$ 的概率 $P(m)$ 随着财富 $m$ 的增加而减少，切服从幂律分布，被称为 
  $$
	P(m)\propto m^{-(1+\nu)} \tag{1} 
	$$
	- 此处的指数 $\nu$ 介于 $1,2$ 之间，其会随着经济体的变化而变化，如对于美国 $\nu = 1.6$，而对于日本 $\nu = [1.8,2.2]$
	- Pareto law相对而言更适合用于描述富人群体的财富分布，对于广大的低收入群体，使用Gibbs分布貌似更合适。
- 储蓄倾向 (saving propensity) 的意义

#### Agent-based model
- 一个**封闭经济系统**的理想气体模型，其具有经济总量 $M$，一共有 $N$ 个个体；不存在生产或迁移的活动，仅限于交易活动。每个主体 $i$，其拥有的金钱数目 $m_{i}(t)$ 在时刻 $t$。在每次交易的过程中，$i$ 和 $j$ 随机交换他们的金钱，他们的金钱总额是一顶的，且不会有个体得到小于零的金钱数（负债）（即 $m_{i}\geq 0$
  $$
	m_{i}(t) + m_{j}(t) = m_{i}(t+1) + m_{j}(t+1) \tag{2} 
	$$
	- 其中 $t$ 的单位是交易的次数。其金钱的稳定分布 $(t\to \infty)$，是一个 Gibbs Distribution
	  $$
		P(m) = (1 / T) \exp(-m /T);\quad T = M/N \tag{3} 
		$$
	- 因此，无论初始分布的均匀程度，最终的结果都是吉布斯分布，其表示大多数个体只有较少的金钱，下式符合金钱的守恒以及熵的可加性：
	  $$
		P(m_{1})P(m_{2}) = P(m_{1}+m_{2}) \tag{4} 
		$$
	- 这种稳定分布十分鲁棒。无论交易形式如何变异，其最终的吉布斯分布几乎保持不变。
- 引入储蓄倾向因子 $\lambda$，每个交易者在时刻 $t$ 会存入其所有财富 $m_{i}(t)$ 的 $\lambda$，并用剩余的去做随机的交易
  $$
	m_{i}(t+1)  = m_{i}(t)+\Delta m;\quad m_{j}(t+1) = m_{j}(t)- \Delta m; \tag{5} 
	$$
	- **同质性** $\lambda$，即所有主体的储蓄率 $\lambda$ 是一致的
	  $$
		\Delta m = (1-\lambda)[\varepsilon \{ m_{i}(t) + m_{j}(t) \} - m_{j}(t)] \tag{5.1} 
		$$
		$\varepsilon$ 表示来自于贸易本身随机性的随机比例
		- 模拟结果如下图所示 $(N = 200, m_{i}(0)= 1)$
		  ![[H05_01.svg]]
	- **异质性** $\lambda$，不同主体的储蓄率 $\lambda$ 不同，如个体 $i$ 的储蓄率为 $\lambda_{i}$ 而个体 $j$ 的储蓄率为 $\lambda_{j}$，于是 $(5.1)$ 变为
	  $$
		\Delta m = \varepsilon(1-\lambda_{j})m_{j}(t)  - (1-\lambda_{i})(1-\varepsilon)m_{i}(t) \tag{5.2} 		$$
		- 模拟结果如下图所示
		  ![[H05_02.svg]]
		  







