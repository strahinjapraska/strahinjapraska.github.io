# Generalized Simon's Problem

The motivation to write this blog was because of the lack of resources on this topic that are accessible and solved in a way the "regular" [Simon's problem](https://en.wikipedia.org/wiki/Simon%27s_problem) is usually solved. Knowledge about the regular Simon's problem is assumed. 

## Problem Statement 
Given some unknown function $f: \mathbb{Z}_2^n \mapsto X$, where X is a finite set, and a positive integer $k<n$. 

There exists an unknown subgroup $V \leq \mathbb{Z}_2^n$ of rank $k$ such that for any $x, y \in \mathbb{Z}^n_2, f(x) = f(y)$ iff $x \oplus y \in V$

Regular Simon's problem is a special case where $V =$ \{ 0, s \}. 

**Note:** This is also known as a special case of [Hidden Subgroup Problem](https://en.wikipedia.org/wiki/Hidden_subgroup_problem). 


## Quantum Algorithm 
The quantum algorithm is given in the following figure 

We start with the state: 

$\ket{\psi_0} = \ket{0}^{\otimes n} \ket{0}^{\otimes n}$

After applying Hadamard to the first $n$ qubits: 

$\ket{\psi_1} = H^{\otimes n}(\ket{0}^{\otimes n})\ket{0}^{\otimes n} = (\frac{1}{\sqrt{2^n}} \sum_{x \in \mathbb{Z}_2^n}\ket{x})\ket{0}^{\otimes n}$

We then query $f$ using XOR oracle: 

$\ket{\psi_2} =(\frac{1}{\sqrt{2^n}} \sum_{x \in \mathbb{Z}_2^n}\ket{x})\ket{f(x)}$

Now here is the first difference to the regular Simon's problem, we measure answer bits, in regular Simon's problem we would be left with state 

$$\frac{\ket{x} + \ket{y}}{\sqrt{2}}$$


$\ket{\psi_3} =$ 



