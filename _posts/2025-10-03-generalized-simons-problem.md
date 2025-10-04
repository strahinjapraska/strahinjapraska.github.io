# Generalized Simon's Problem
 
The motivation for writing this blog was the lack of accessible resources on this topic that present solutions in the similar way as the 'regular' [Simon's problem](https://en.wikipedia.org/wiki/Simon%27s_problem). Knowledge about the regular Simon's problem and some abstract algebra is assumed. 

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

## Amplitude Analysis 
For $z$ to be observed, we need a non-zero amplitude, and the most important part is to analyse what happens to the $\sum_{v \in V}(-1)^{v \cdot z}$ term. Let's denote it by $S(z)$. We consider the following two cases 
1. $z \in V^\bot = \\{z \in \mathbb{Z}^n_2 \lvert, \lvert z \cdot i = 0 \forall v \in V  \\}$, i.e. $z$ is orthogonal to every vector in $v$ 
2. $z \notin V^\bot$, which means that there are some $v$ for which $z \cdot v = 1$

In the first case above, this would mean that every $(-1)^{z \cdot v} = (-1)^0$, so we would add up $\lvert V \rvert$ ones, so $S(z) = \lvert V \rvert$ 

Now what we are interested is how many of the $v$ there are such that $z \cdot v = 1$, as we would like not to be in a case where we have for example $(-1)^1 + (-1)^0 + (-1)^1$, we would like to show that they all cancel out.  

Let us consider the mapping $f_z: V \mapsto \mathbb{Z}_2$ defined as $f_z(v) = z \cdot v$. This mapping is a homomorphism. 
$$f(u \oplus v) = z \cdot (u \oplus v) = z \cdot u \oplus z \cdot v = f(u) \oplus f(v)$$

Now the kernel of this mapping $K = ker(f) = \\{ v \in V \lvert z \cdot v = 0 \\}$. 

**Theorem 1(First Isomorphism Theorem)** Let $G$ and $H$ be groups, and let $G \mapsto H$ be a homomorphism. Then 

$$G/ker(f) \cong Im(f)$$ 

Now, if we apply **Theorem 1** tour our setting: 

$$V/ker(f) \cong Im(f) = \mathbb{Z}_2$$

$V/ker(f)$ is a **quotient group**, I won't go too much into details, there is an awesome [blog](https://www.math3ma.com/blog/the-first-isomorphism-theorem-intuitively) about quotient groups and First Isomorphism Theorem, but quotient group basically represents **all** of the cosets of $ker(f)$ in V.   

Since $V/ker(f) \cong  \mathbb{Z}_2 \implies \lvert V/ker(f) \rvert = \lvert \mathbb{Z}_2 \rvert = 2$. So, there are only two cosets, and a fact from abstract algebra is that the cosets **paritition** the group V and that all the cosets are of the same size. 

<img width="715" height="508" alt="Untitled-2025-10-04-1415" src="https://github.com/user-attachments/assets/a5ebf8bc-ae58-4f0e-8c66-b56e95a03ee2" />

This means that the half of them will have property that $z \cdot v = 1$ and half of them will have $z \cdot v = 0$ . Which means that there are equal number $-1$ and $1$ terms in our sum $\implies S(z) = 0$. 

