<h1><center>随机算法</center></h1>

<center>BY  唐志鹏  SA23011068</center>

## 题目 1

随机算法 NEWALG: 

- 运行算法 $\mathcal{A}$ 直到 $100T(n)$ 时间
  - 如果算法 $\mathcal{A}$ 在 $100T(n)$ 时间内运行结束，返回算法 $\mathcal{A}$ 的输出
  - 否则，终止，并返回拒绝

**正确性**：

设 $T_{\mathcal{A}}$ 为算法 $\mathcal{A}$ 的运行时间，

- 如果输入是 “是” 实例，则
  $$
  \begin{align}
  \text{Pr}(\text{“是” 实例被接受}) &= \text{Pr}(\mathcal{A}\ \text{在}\  100T(n)\ 内结束) \\
  &= \text{Pr}(T_{\mathcal{A}} \leq 100 T(n)) \\
  &= \text{Pr}(T_{\mathcal{A}} \leq 100\rm{E}[T_{\mathcal{A}}]) \\
  &\geq 1 - \frac{1}{100}\ (\text{Markov 不等式}) \\
  &\geq \frac{9}{10}
  \end{align}
  $$

- 如果输入是 “否“ 实例，则，无论算法 $\mathcal{A}$ 是否在 $100T(n)$ 时间内结束，都会被拒绝

## 题目 2

- 使用 Markov 不等式
  $$
  \text{Pr}(R \geq 150) \leq \frac{{\rm{E}}[R]}{150} = \frac{2}{3}
  $$

- 使用 Chebyshev 不等式
  $$
  \text{Pr}(R\geq 150) \leq \text{Pr}(|R-100| \geq 50) \leq \frac{{\rm{var}}[R]}{50^2} = \frac{4}{125}
  $$

因此，找到的人智商至少为 $150$ 概率不超过 $\frac{4}{125}$

## 题目 3

### (a)

设 $X_i$ 为 $1$ 当且仅当第 $i$ 个 CPU 恰好在一轮中被分配了 $1$ 个进程，否则 $X_i$ 为 $0$，则
$$
X_i = \begin{cases}
1,\ 第i个 {\rm{CPU}} 恰好被分配1个进程 \\
0,\ \rm{otherwise}
\end{cases}
$$
计算第 $i$ 个 CPU 恰好在一轮中被分配了 $1$ 个进程的概率，
$$
\text{Pr}(X_i=1) = \tbinom{b}{1} \cdot\frac{1}{n} \cdot \left(1-\frac{1}{n}\right)^{b-1} = b\cdot\frac{1}{n} \cdot \left(1-\frac{1}{n}\right)^{b-1}
$$
则 $X_i$ 的期望为，
$$
{\rm{E}}[X_i] = \text{Pr}(X_i=1) = b\cdot\frac{1}{n} \cdot \left(1-\frac{1}{n}\right)^{b-1}
$$
设 $Y$ 为下一轮开始时的进程数，则
$$
\begin{align}
{\rm{E}}[Y] &= b - {\rm{E}} \left [\sum_{i=1}^n X_i \right] \\
&= b - \sum_{i=1}^n {\rm{E}}[X_i] \\
&= b - b \cdot \left(1-\frac{1}n\right)^{b-1}
\end{align}
$$

### (b)

根据上述结果，
$$
x_{j+1} = x_j - x_j \cdot \left(1 - \frac{1}{n} \right)^{x_j-1} \leq x_j - x_j \cdot \left( 1 - \frac{x_j-1}{n} \right) = \frac{x_j(x_j-1)}{n} \leq \frac{{x_j}^2}{n}
$$
在第 $1$ 轮，$x_0=n$，
$$
x_1 = n - n \left( 1 - \frac{1}{n} \right)^{n-1} \leq n - n\cdot \frac{1}{e} = n \left( 1 - \frac{1}{e} \right)
$$
令 $\frac{1}{c} = 1 - \frac{1}{e}$，则 $x_1 \leq \frac{n}{c}$

下面用归纳法证明：$x_j \leq \frac{n}{c^{2^{j-1}}},\ j \geq 1$

当 $j=1$ 时，$x_1 \leq \frac{n}{c} = \frac{n}{c^{2^0}}$

假设 $j=i$ 时结论成立，则 $j=i+1$ 时，
$$
x_{i+1} \leq \frac{{x_i}^2}{n} \leq \frac{{\left(\frac{n}{c^{2^{i-1}}}\right)}^2}{n} = \frac{n}{c^{2^{i}}}
$$

结论成立！

若 $k$ 轮后没有剩余进程，则 $x_k = 0$，这意味着 $\frac{n}{c^{2^{k-1}}} < 1$

因此，$k = \log_2 \log_c n + 1 = O(\log \log n)$

## 题目 4

使用一个随机函数 $h\in \mathcal{H}$ 哈希流 $\mathcal{S}$，设 $C$ 为流中的碰撞总数，对任意的不同的 $x,x'\in \mathcal{S}$，定义 $\mathcal{X}_{h(x)=h(x')}$: 
$$
\mathcal{X}_{h(x)=h(x')} \begin{cases}
1,\ {\rm if}\ h(x)=h(x') \\
0,\ \rm otherwise
\end{cases}
$$
于是，$C = \sum_{x\neq x'} \mathcal{X}_{h(x)=h(x')}$，
$$
{\rm E}[C] = {\rm E}\left[ \sum_{x\neq x'} \mathcal{X}_{h(x)=h(x')} \right] = \sum_{x\neq x'} {\rm E}[\mathcal{X}_{h(x)=h(x')}]
$$
根据强 2-全域的定义，
$$
\begin{align}
{\rm E}[\mathcal{X}_{h(x)=h(x')}] &= {\rm Pr}(h(x)=h(x')) \\
&\leq \sum_{y} {\rm Pr}(h(x)=y \and h(x')=y) \\
&= \sum_{y} \frac{1}{|Y|^2} \\
&= \frac{1}{|Y|} \\
&= \frac{1}{cs^2}
\end{align}
$$
流 $\mathcal{S}$ 中至多只有 $\tbinom{s}{2}$ 对不同的 $x,x'$，因此，
$$
{\rm E}[C] \leq \frac{\tbinom{s}{2}}{cs^2} = \frac{1}{2c}
$$
根据 Markov 不等式，
$$
{\rm Pr}[C \geq 1] \leq \frac{{\rm E}[C]}{1} = \frac{1}{2c}
$$
因此，碰撞的概率至多为 $\frac{1}{2c}$