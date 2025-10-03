# Generalized Simon's Problem

The motivation to write this blog was because of the lack of resources on this topic that are accessible and solved in a way the "regular" [Simon's problem](https://en.wikipedia.org/wiki/Simon%27s_problem) is usually solved. Knowledge about the regular Simon's problem is assumed. 

## Problem Statement 
Given some unknown function $f: \mathbb{Z}_2^n \mapsto X$, where $X$ is a finite set, and a positive integer $k<n$. 

There exists an unknown subgroup $V \leq \mathbb{Z}_2^n$ of rank $k$ such that for any $x, y \in \mathbb{Z}^n_2, f(x) = f(y)$ iff $x \oplus y \in V$

Goal is to find $V$

Regular Simon's problem is a special case where $V =$ \{ 0, s \}. 

**Note:** This is also known as a special case of [Hidden Subgroup Problem](https://en.wikipedia.org/wiki/Hidden_subgroup_problem). 


## Quantum Algorithm 
The quantum algorithm is given in the Figure 1. 

<img width="827" height="278" alt="The-quantum-circuit-of-Simons-algorithm-The-measurement-in-the-dotted-box-could-be" src="https://github.com/user-attachments/assets/95bbd475-2f0e-4826-88b4-5552e23b4df9" />

**Figure 1.** Taken from [https://www.researchgate.net/figure/The-quantum-circuit-of-Simons-algorithm-The-measurement-in-the-dotted-box-could-be_fig12_350180612](https://www.researchgate.net/figure/The-quantum-circuit-of-Simons-algorithm-The-measurement-in-the-dotted-box-could-be_fig12_350180612)

We start with the state: 

$\ket{\psi_0} = \ket{0}^{\otimes n} \ket{0}^{\otimes n}$

After applying Hadamard to the first $n$ qubits: 

$\ket{\psi_1} = H^{\otimes n}(\ket{0}^{\otimes n})\ket{0}^{\otimes n} = (\frac{1}{\sqrt{2^n}} \displaystyle \sum_{x \in \mathbb{Z}_2^n}\ket{x})\ket{0}^{\otimes n}$

We then query $f$ using XOR oracle: 

$\ket{\psi_2} =(\frac{1}{\sqrt{2^n}} \displaystyle \sum_{x \in \mathbb{Z}_2^n}\ket{x})\ket{f(x)}$

Now here is the first difference to the regular Simon's problem, we measure answer bits, in regular Simon's problem we would be left with state 

$$\frac{\ket{x} + \ket{y}}{\sqrt{2}} = \frac{\ket{x \oplus 0} + \ket{x \oplus s}}{\sqrt{2}}$$ 

In other words, when we measure $w$ it coincides with two values, $0$ and $s$, now we just generalize this for all $v \in V$ and we shouldn't forget to normalize by $\lvert V \rvert$ since that's how many such $v$ there are, i.e.

$\ket{\psi_3} = \frac{1}{\sqrt{\lvert V \rvert}} \displaystyle \sum_{v \in V}\ket{x \oplus v}$  

Now we apply Hadamard again: 

$$
\begin{align}
    \ket{\psi_4} &= H^{\otimes n}(\frac{1}{\lvert V \rvert} \sum_{v \in V} \ket{x \oplus v}) \\
                 &= \frac{1}{\sqrt{\lvert V \rvert}} \frac{1}{\sqrt{2^n}}  \sum_{z \in \mathbb{Z}_2^n}\sum_{v \in V}(-1)^{(x \oplus v) \cdot z}\ket{z} \\ 
                &= \frac{1}{\sqrt{\lvert V \rvert}} \frac{1}{\sqrt{2^n}}  \sum_{z \in \mathbb{Z}_2^n}\sum_{v \in V}(-1)^{x \cdot z}(-1)^{v \cdot z}\ket{z} \\ 
                 &= \frac{1}{\sqrt{\lvert V \rvert}} \frac{1}{\sqrt{2^n}} \sum_{z \in \mathbb{Z}_2^n} (-1)^{x \cdot z}\sum_{v \in V}(-1)^{v \cdot z}\ket{z}
\end{align} 
$$ 

