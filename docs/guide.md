# this is guide
# Control Theory for Neuroscience
- 连接组合对线虫运动的影响，
-  使用包含 $N$ 个神经元和 $M$ 个肌肉组织的有向网络来进行分析，这一系统的动力学方程可写作
  $$
	\dot{\mathbf{z}}(t) = \mathbf{f}(\mathbf{z}, \mathbf{v},t) \tag{1}
	$$
	- $\mathbf{z}(t) = [z_{1}(t),z_{2}(t),\dots,z_{N+M}(t)]^{\text{T}}$ 表示在时间 $t$ 时 $M+N$ 个节点的状态 (state)
	- $\mathbf{f}(*) = [f_{1}(*), f_{2}(*),\dots,f_{N+M}(*)]^{\text{T}}$ 用于表示每个节点的非线性动力学过程
	- $\mathbf{v}(t) = [v_{1}(t),v_{2}(t),\dots,v_{S}(t)]^{\text{T}}$ 用于表示外部刺激作用于 $S$ 个触觉感受器
- 假设没有任何外界刺激时，神经系统处于平衡态 $\mathbf{z}^{*}$，此时，$\mathbf{f}(\mathbf{z}^{*},\mathbf{v}^{*},t) = 0$，使用 $\mathbf{x}(t)= \mathbf{z}(t) - \mathbf{z^{*}}$ 以及 $\mathbf{u}(t) = \mathbf{v}(t) - \mathbf{v}^{*}$，$(1)$ 式可被[[Fundamentals of Nonlinear Dynamics#^linearized|线性化]]，得到
  $$
	\begin{cases}
	\dot{\mathbf{x}}(t) = A \mathbf{x}(t) + B \mathbf{u}(t) \\
	\mathbf{y}(t) = C \mathbf{x}(t)
	\end{cases} \tag{2} 
	$$
	- $A \coloneqq \left.\frac{\partial \mathbf{f}}{\partial \mathbf{z}}\right\vert_{\mathbf{z}^{*},\mathbf{v}^{*}}$ 其相当于连接组 (connectome) 的邻接矩阵 (adjacency matrix)，对角线上的 $a_{ii}$ 表示第 $i$ 个节点的动力学 $A \in \mathbb{R}^{(N+M)\times(N+M)}$
	- $B \coloneqq \left.\frac{\partial \mathbf{f}}{\partial \mathbf{v}}\right\vert_{\mathbf{z}^{*},\mathbf{v}^{*}}$ 输入矩阵 $B$ 用于表示接受外部信号的受体神经元 (receptor neurons)。即连接外部信号的神经元节点 $B \in \mathbb{R}^{(N+M)\times S}$
	- $C$ 代表我们希望控制的目标节点 $C \in \mathbb{R}^{M\times(N+M)}$
	- 向量 $\mathbf{y}(t)$ 用于表示 $t$ 时刻 $M$ 个肌肉细胞的状态。

# Controllable
## Definition of Controllable and Uncontrollable
  ![[controllable.png]]
- 只有输入刺激数目 $m \geq k$ 即运动细胞数时，才能算 **Controllable**，因此，那些在感觉运动中有重要作用的神经元，往往会导致可控制肌肉数目的下降。
- Kalman’s criterion for controllable - system is **controllability** if and only if  
  $$
	\text{rank}(CB, CAB, CA^{2}B, \dots, CA^{N+M-1}B) = M \tag{S1} 
	$$
	 - 其中
		- $C \in \mathbb{R}^{M\times(M+N)}$
		- $A\in \mathbb{R}^{(M+X)\times(M+N)}$
		- $B \in \mathbb{R}^{(M +N)\times S}$
	- 但是此方法存在一定缺陷：
		1. 为了计算上述矩阵的秩，我们不仅需要知道网络的连边，还需要知道各个连边的权重。而这些数据往往是很难得到的。（==但是，后面那一篇？==
		2. 此矩阵为病态矩阵 (ill-conditioned matrix) - 即矩阵中单个元素的轻微扰动会导致矩阵从非奇异矩阵转变为奇异矩阵。

## Structural controllability
### The formation of Matrix
- 矩阵中的元素仅有两种情况，0和独立自由参数 (free parameters)。当上述的 $A,B,C$ 矩阵的元素变为上述情况且符合 Kalman’s Criterion for controllable 时，系统被称为结构性可控 (Structural controllability)
- 例如
  $$ \begin{align} A = \begin{bmatrix} 0 & 0 & 0 & 0 \\ a_{21} & 0 & 0 &  0 \\ a_{31} & 0 & 0 & 0 \\ 0 & 0 & a_{43} & 0 \end{bmatrix},\, B = \begin{bmatrix} b_{1} \\ 0 \\ 0 \\ 0 \end{bmatrix}, \text{ and } C = \begin{bmatrix} 0 & c_{12} & 0 & 0 \\ 0 & 0 & 0 & c_{24} \end{bmatrix} \\\\ K = [CB,CAB,CA^{2}B,CA^{3}B] = \begin{bmatrix}0 & c_{12}a_{21}b_{1} & 0 & 0 \\ 0 & 0 & c_{24}a_{43}a_{31}b_{1} & 0 \end{bmatrix} \end{align} $$
	- $\text{Rank}(K) = 2 = M$ 故其是结构性可控的

### The other formation
- 另一种形式  $$K_{q,S\times l + r} = \sum_{\text{P} \in\text{Path}_{q,r}(l+2)} \pi_{\text{P}}$$
	- $q$ - 希望控制的第 $q$ 个节点 $q = 1,2,\dots,M$
	- $r$ - 第 $r$ 个刺激 $r = 1,2,\dots, S$
		- $S$ - 输入刺激总数
	- $l$ - 从第 $r$ 个刺激传导到第 $q$  个目标节点的路径的长度（即经过的连边数目）
	- $\pi_{\text{P}}$ 路径 $\text{P}$ 上各边的权重的积
- 以上面的$A,B,C$ 为例，可得下图
  ```mermaid
  flowchart TD;
  input[s1];
  it1((x1));
  it2((x2));
  it3((x3));
  it4((x4));
  c1([op1]);
  c2([op2]);
  input -."b1".-> it1;
  it1 --"a21"--> it2;
  it1 --"a31"--> it3;
  it3 --"a43"--> it4;
  it2 -."c12".-> c1;
  it4 -."c24".-> c2;
	```
	- 可见从 s1 $(r = 1 = S)$ 至 op1 $(q=1 )$ 的仅有一条长度为 $3 \,(l = 3-2 = 1)$ 的路径，于是可知 $q = 1,\, S\times l + r = 1\times 1 +1 = 2$，于是可知
	  $$
		K_{1,2} =  c_{12}a_{21}b_{1}
		$$
		这与上面通过矩阵计算得出的结果是一致的。

		
## Linking-graph
![[Pasted image 20231022161649.png]]
- 上图中 (c) 为基于 (a) 图构建的链接图 (linking-graph)  ，我们可将其用数学形式表示 $G_{\text{linking}} = G_{\text{linking}}(A,B,C)$ 决定于三个结构矩阵 $\{ A,B,C \}$：
	- $G_{\text{linking}}$ 的节点集合为 $V_{A}\cup V_{B}\cup V_{C}$，其中 
		- $V_{A} = \{ v_{i,t}^{A}\,\vert\, i = 1,2,\dots,N+M;\,t=1,2,\dots,N+M \}$
		- $V_{B} = \{ v_{i,t}^{B}\,\vert\, i = 0,2,\dots,S;\,t=0,1,\dots,N+M-1 \}$
		- $V_{C} = \{ v_{i}^{c}\,\vert\, i = 1,2,\dots,M \}$
	- $G_{\text{linking}}$ 的连边集合为 $E_{A}\cup E_{B}\cup E_{C}$，其中
		- $E_{A} = \{ v_{j,t}^{A} \to v_{i,t+1}^{A}\,\vert\, a_{ij} \neq 0, t = 1,2,\dots,N+M-1 \}$
		- $E_{B} = \{ v_{j,t}^{B} \to v_{i,t+1}^{A}\,\vert\, b_{ij} \neq 0, t = 0,1,\dots,N+M-1 \}$
		- $E_{C} = \{ v_{j,t}^{A} \to v_{i}^{C}\,\vert\, c_{ij}\neq 0;\, t=N+M \}$

## The maximum bound of $z$ (linking size)
- ***linking size*** - 从节点集合 $B$ 到集合 $C$ 之间**不相交** (**disjoint**) 的路线数（即无共同交点)。其可抽象为一个多源多汇 (multi-sources-multi-sinks) 的最大流 (maximum flow) 问题，其每个节点和边的容量均为 $1$ ![[Pasted image 20231022184634.png]]
	-  **注意**：linking size ($z^{*}$) 并不能直接确定实际的结构性可控的节点数 $z$，其仅是 $z$ 的**上限**，即
		- 当 $C = \mathbf{I}_{(N+M)}$ 即为 $N+M$ 阶的单位矩阵（希望控制所有 $N+M$ 节点时），可控制的节点数目等于 linking graph 的 linking size。但若我们仅希望控制其中一部分时，可控节点数就不同于 linking size，实际上有
		  $$
			z \leq z^{*} \tag{S2} 
			$$
		- 即：linking size 仅决定可控制节点数目的上限，所以其无法直接应用于正常线虫的可控制节点数目的判定，也无法直接评估移走某类神经元后，可控制节点的下降数目。
## The minimum bound of $z$
- 当目标节点（希望被控制的节点）集合不受到外界信号 $\mathbf{u}(t)$ 的直接控制时，可将网络分为两个子网络 $G_{\text{O}}$ 和 $G_{\text{M}}$。其中，$G_{\text{O}}$ 包含子网络 $G_{\text{S}}$ 和 $G_{\text{D}}$。（如下图所示 ![[Pasted image 20231024114345.png]]
	- $G_{\text{O}}(V_{\text{O}})$：除希望控制的节点外的所有其他节点
		- $G_{\text{S}}(V_{\text{S}})$ - 其中每个节点直接接受外界的刺激，直接与外界刺激相连
		- $G_{\text{D}}(V_{\text{D}})$ - 其中每个节点直接连接我们希望控制的节点
	- $G_{\text{M}}(V_{\text{M}})$ 我们希望直接控制的节点
1. 考虑子图 $G_{\text{O}} - G_{\text{D}}$，由于现实中此部分的神经网络具有极高的密度，于是我们可以将此部分视为是节点 $V_{S}$ 结构性可控的。
2. 依次向 $G_{\text{O}}-G_{\text{D}}$ 中加入节点 $V_{D}$，直到新的子图不能由 $V_{\text{S}}$ 完全控制。其中被纳入最终子图 $(G_{\text{F}})$ 中的属于集合 $V_{\text{D}}$ 的节点记为 $V_{\text{D}_{-}}$
3. 寻找从 $V_{\text{D}_{-}}$ 到 $V_{\text{M}}$ 的最大匹配数，并获得节点集合 $U \subseteq V_{\text{M}}$ 表示被匹配覆盖的属于 $V_{\text{M}}$ 的节点集合
4. 虽然，$U$ 可能有很多种，但是其的集合的 **势 (Cardinality)**  $z_{*}$是唯一的。（对于有限集合可以使用集合的元素数目来度量），其就是 $z$ 的下限，即在目标集合 $V_{\text{M}}$ 中可控节点的数目 $z$ 满足
   $$
	z \geq z_{*} \tag{S3} 
	$$
	
	 ^eqS3
	- 由 [[#The other formation|Structural Controllability 的第二种形式]] 可知，将任何节点及其连边加入已有图中，我们将会发现，$\text{Path}_{q, r}(l+2)$ 的 Cardinality 只有可能增加或不变，而不可能减少，这意味着 $K$ 中的元素个数不小于原来的 $\pi_{\text{P}}$ 的个数，即不会增加 $\text{rank}(K)$ ，故不等式[[Gang Yan, 2017#^eqS3 | S3]]成立

## Self-loops
- 邻接矩阵 $A$ 中的非零元素 $A_{ii}$ 表示节点 $i$ 的内部动力学，即其自身形成的环。一般认为，节点自身的动力学会影响网络的完全控制性 (full controllability of network)。Zhao *et al.* 发现，但所有的节点具有相同的环（即动力学特征）时，完全控制的最大匹配结果 (the maximum matching result of full controllability) 依然是有效的。显然这对于神经系统而言是成立的，因为在进行神经建模的过程中，我们会假定所有神经元的动力学特征是完全一致的。
- 将矩阵分块 $A = \begin{bmatrix} A_{\alpha\alpha} & A_{\alpha\beta} \\ A_{\beta\alpha} & A_{\beta\beta}  \end{bmatrix},\, B = \begin{bmatrix} B_{\alpha} \\ B_{\beta}\end{bmatrix},\, C = \begin{bmatrix}C_{\alpha} & C_{\beta} \end{bmatrix}$ 
	- $A_{\alpha\alpha}$ 表示子图 $G_{\text{O}}$ 节点集 $V_{\text{O}}$ 内部节点之间的邻接矩阵；$A_{\beta\beta}$ 表示子图 $G_{\text{M}}$ 节点集 $V_{\text{M}}$ 内部节点之间的邻接矩阵；$A_{\alpha\beta}$ 表示从集合 $V_{\text{M}}$ 连向集合 $V_{\text{O}}$ 的连边，$A_{\beta\alpha}$ 表示从集合 $V_{\text{O}}$ 连向集合 $V_{\text{M}}$ 的连边。
	- $B_{\alpha}$ 表示属于 $V_{\text{O}}$ 且与外部刺激之间联系的节点，$B_{\beta}$ 表示属于 $V_{\text{M}}$ 且与外部刺激直接联系的节点。
	- $C$ 表示希望控制的节点（肌肉）
	- 由于
		1. 外部刺激不直接与肌肉相连 → $B_{\beta} = \mathbf{0}_{M\times S})$
		2. 输出节点全属于 $V_{\text{M}}$ → $C_{\alpha} =\mathbf{0}_{M\times N}$
		3. 仅存在由 $V_{\text{O}}$ 向 $V_{\text{M}}$ 的单向连接，而不存在反向连接；且肌肉之间相互独立 → $A_{\alpha\beta} = \mathbf{0}_{N\times M}, A_{\beta\beta} = \mathbf{0}_{M\times M}$，于是有
		   $$
			\begin{cases}
			A = \begin{bmatrix}
			A_{\alpha\alpha} & \mathbf{0}_{N\times M} \\
			A_{\beta\alpha} & \mathbf{0}_{M\times M}
			\end{bmatrix}\\ \\
			B = \begin{bmatrix}
			B_{\alpha} \\
				\mathbf{0}_{M\times S}
			\end{bmatrix} \\\\
			C = \begin{bmatrix}
			\mathbf{0}_{M\times N}  & C_{\beta}
			\end{bmatrix}
			\end{cases}
			$$
	- 于是可得 $CA^{k}B  = C_{\beta}A_{\beta\alpha}A_{\alpha\alpha}^{k-1}B_{\alpha}$，于是有
	  $$
	  \begin{align}
		K & = [CB,CAB,CA^{2}B, \dots, CA^{N+M-1}B] \\
		&=[C_{\beta}B_{\alpha}, C_{\beta}A_{\beta\alpha}A_{\alpha\alpha}B_{\alpha}, C_{\beta}A_{\beta\alpha}A_{\alpha\alpha}^{2}B_{\alpha},\dots,C_{\beta}A_{\beta\alpha}A_{\alpha\alpha}^{N+M-2}B_{\alpha}] \tag{S4} 
		\end{align}
		$$
- 此外，我们假设：神经元的动力学过程不同于肌肉细胞的动力学过程 ($\ell_{1}\neq \ell_{2} \neq 0$)，于是带有自身动力学过程的邻接矩阵变为
  $$
	\hat{A} = 
	\begin{bmatrix}
	A_{\alpha\alpha}+\ell_{1} I_{\alpha} & \mathbf{0}  \\
	A_{\beta\alpha}  & \ell_{2}I_{\beta}
	\end{bmatrix} \tag{S5} 
	$$
	- 于是有 $\hat{K} = [CB, C\hat{A}B,C\hat{A}^{2}B, \dots,C\hat{A}^{N+M-1}B]$
	- 其并==不会改变一个可控矩阵的秩==，即 ==$\text{rank}(K) = \text{rank}(\hat{A})$==
	- 证明
		- $\hat{K}$ 中任意一个子矩阵可写作 $(k\geq 2)$
		  $$
			\begin{align}
			C \hat{A}^{k}B &= C_{\beta}\left( \sum_{i=1}^{k}\ell_{2}^{i-1}A_{\beta\alpha}(A_{\alpha\alpha} + \ell_{1}I_{\alpha})^{k-i} \right)B_{\alpha}\\
			&= \underbrace{C_{\beta}A_{\beta\alpha}(A_{\alpha\alpha}+\ell_{1})^{k-1}B_{\alpha}}_{\text{ta(k)}} + \underbrace{C_{\beta}\left( \sum_{i=2}^{k} \ell_{2}^{i-1}A_{\beta\alpha}(A_{\alpha\alpha} + \ell_{1}I_{\alpha})^{k-i}\right)B_{\alpha}}_{\text{tb(k)}} \\
			& = C_{\beta}A_{\beta\alpha}(A_{\alpha\alpha}+\ell_{1})^{k-1}B_{\alpha} + \ell_{2}C\hat{A}^{k-1}B
			\end{align}
			$$
		- 于是可将 $\hat{K}$ 写作
		  $$
			\hat{K} = [C_{\beta}B_{\alpha},\,C_{\beta}A_{\beta\alpha}B_{\alpha},\,\text{ta}(2) + \ell_{2}C\hat{A}B,\, \dots, \text{ta}(N+M-1) + \ell_{2}C \hat{A}^{N+M-2}B]
			$$
		- 于是我们从第 $3$ 个子矩阵开始，将第 $k+1$ 个子矩阵减去第 $k$ 个字矩阵乘以 $\ell_{2}$ 的结果，即 $C\hat{A}^{k+1}B - \ell_{2}C\hat{A}^{k}B$ 得到结果为
		  $$
		  \hat{K}' = [C_{\beta}B_{\alpha}, C_{\beta}A_{\beta\alpha}B_{\alpha}, C_{\beta}A_{\beta\alpha}(A_{\alpha\alpha}+\ell_{1}I_{\alpha})B_{\alpha},\dots,C_{\beta}A_{\beta \alpha}(A_{\alpha\alpha}+\ell_{1}I_{\alpha})^{N+M-2}B_{\alpha}]
			$$
		- 由于$(A_{\alpha\alpha}+\ell_{1}I_{\alpha})^{k} = f(A_{\alpha\alpha},A_{\alpha\alpha}^{2},\dots ,A_{\alpha\alpha}^{k})$ ，于是，我们使用合适的常数乘以 $\hat{K}'$ 的第 $2$ 个子矩阵，并让第 $3$ 个子矩阵减去该结果，我们可以得到 $C_{\beta}A_{\beta\alpha}A_{\alpha\alpha}B_{\alpha}$
		- 在根据上面的结果，我们让第 $4$ 个子矩阵减去：更新后的第 $3$ 个子矩阵乘以合适的数值和第 $2$ 个子矩阵乘以合适的数值，不断重复迭代上面的过程，我们可以得到
		  $$
			\hat{K}''
			=[C_{\beta}B_{\alpha}, C_{\beta}A_{\beta\alpha}A_{\alpha\alpha}B_{\alpha}, C_{\beta}A_{\beta\alpha}A_{\alpha\alpha}^{2}B_{\alpha},\dots,C_{\beta}A_{\beta\alpha}A_{\alpha\alpha}^{N+M-2}B_{\alpha}]  = K
			$$
		- 由于矩阵的基本变换不会改变矩阵的秩，因此有 $\text{rank}(K) = \text{rank}(\hat{K})$



## How to describe the motion of C. Elegans? — Eigenworms
- 线虫的移动描述 (**eigenworms**)，分为四部分
	1. 第一部份表示线虫整体的弯曲 (A large body bend)
	2. 第二部分和第三部份表示线虫身体进行正弦摆动，以使得其向前运动的行为。
	3. 第四部分：表示在头部和尾部的细小动作。
> 参考文献 [Dimensionality and Dynamics in the Behavior of _C. elegans](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000028)



