---
layout: post
title: Classical Shadow
author: Gaurav Gyawali
date: 2021-12-13 12:20:00 -0500
description: 
comments: true
categories: scienceblog
---

Obtaining any useful information from a quantum computer requires performing measurement on the quantum state. 
Since the dimension of the Hilbert space increases exponentially in the number of qubits, a naive strategy to 
measure the expectation values involves exponentially large number of measurements. Huang et. al [1,2] have
developed an approximate classical description of a quantum state using very few measurements on a quantum state, 
called the classical shadow, which allows us to accurately predict $$M$$ different functions of the state with 
high success probability with just order $$log(M)$$ measurements. 

The authors have also shown the effectiveness of this approach in machine-learning applications for quantum many-body problems.



## Definition
To construct a classical shadow representation, a unitary $$U$$ chosen at random from a fixed ensemble, 
is used to rotate a quantum state $$\rho$$ to $$U \rho U^\dagger$$. Measurement on the computational basis is 
then performed which collapses $$\rho$$ to $$U^\dagger \ket{\hat{b}} \bra{\hat{b}} U$$ with probability $$\bra{\hat{b}} U \rho U^\dagger \ket{\hat{b}}$$,
 where $$\hat{b} \in \{0,1\}^n$$. The state $$U^\dagger \ket{\hat{b}} \bra{\hat{b}} U$$ is a classical description of $$\rho$$ that we can store in a 
 classical memory, and its expectation value over both the  unitary ensemble $$\mathcal{U}$$ and outcomes $$b$$ describes a quantum channel $$\rho \rightarrow \mathcal{M}(\rho)$$:
$$
    \label{measurement_channel}
    \mathrm{E} \left[ U^\dagger \ket{\hat{b}} \bra{\hat{b}} U \right] = \mathrm{E}_{U \in \: \mathcal{U}} \sum_{b \in\{0,1\}^{n}} \bra{b}U \rho U^{\dagger}\ket{b} U^{\dagger}\ket{b} \bra{b} U=\mathcal{M}(\rho).
$$

The ensemble $$\mathcal{U}$$ is tomographically complete if for any given two states, we can always find some rotation 
which can distinguish between them. Tomographic completeness ensures the existence of a unique inverse $$\mathcal{M} ^{-1}$$ 
and we can define $$\hat{\rho}=\mathcal{M}^{-1}\left(U^{\dagger}|\hat{b}\rangle\langle\hat{b}| U\right)$$. 
The array $$(\rho;N) = {\hat{\rho}_1, \cdots \hat{\rho}_N}$$ is called the classical shadow of $$\rho$$ with size $$N$$. 
$$\hat{\rho}$$ has a unit trace, but it is not necessarily positive semi-definite. 
Thus, it is not a physical density matrix although it reproduces $\rho$ in expectation i.e. $$\mathrm{E}[\hat{\rho}] = \rho$$ in principle. In practice, we desire to predict various linear (eg. $$tr(O\rho)$$) as well as non linear  (eg. $$tr(\rho \text{log}(\rho)$$) functions of $$\rho$$, or predict its phase. 

## Choice of unitary ensembles
A practical realization of the above technique requires a unitary ensemble that allows us to predict the properties of 
a state $$\rho$$, ideally with just a few copies. It is not surprising that the Clifford unitaries, which are ubiquitous 
in quantum information/computing, appear here as well. Huang et. al. [1] propose two kinds of unitary ensembles- 
i) random Clifford unitaries on the entire Hilbert space, ii) tensor products of random 2-qubit Clifford unitaries (or Pauli matrices). 
They show that predicting linear functions with error $$\epsilon$$ requires at least $$\text{log}(M) \text{max}_i \text{tr}(O_i^2)/\epsilon^2$$ 
and  $$ \text{log}(M)3^k \epsilon^2 $$ copies of the state $$\rho$$ using random Clifford and random Pauli measurements respectively

The authors also show that the classical shadow representation is capable of predicting nonlinear functions such as entanglement entropy, 
but the size of the classical shadow scales exponentially in the subsystem size.  

## Machine Learning Application
In Ref [2], Huang et. al. prove that the classical machine learning algorithms can efficiently predict the 
ground state properties of gapped Hamiltonians in finite spatial dimensions, after learning the classical shadows 
of other ground states in the same phase of matter. An interesting example is classifying the quantum phases of matter. 
Classifying the symmetry-breaking phases is conceptually simple because it involves calculating $$\text{tr}(\rho O)$$, 
for some k-local observable $O$ such that $$\text{tr}(\rho O) \geq 1 \: \forall \: \rho \in \text{ phase A}$$, 
and  $$\text{tr}(\rho O) \leq -1 \: \forall \: \rho \in \text{ phase B}$$.

In contrast to the symmetry-breaking phases, topological phases cannot be distinguished by measuring some local observable $$O$$. 
It involves measuring a nonlinear function of $\rho$, for example the entanglement entropy. Learning the nonlinear function requires a more expressive ML model. 
The authors thus devise a mapping from classical shadow $$S(\rho, N)$$ to a highly expressive feature space that includes the polynomial expansion of many-body reduced density matrices. 
Using this feature space, they establish a rigorous guarantee that a classical ML can efficiently classify the topological phases of matter as well. 
The provided examples include the topological phases of the toric code and bond-alternating XXZ Hamiltonians. 

Classical shadow, thus, serves as an efficient yet powerful classical representation of a quantum state with a potential to play a key role in harnessing the NISQ devices for simulating many-body systems.

## Citations

[1] Hsin-Yuan Huang, Richard Kueng, and John Preskill. Predicting many prop-
erties of a quantum system from very few measurements. Nature Physics,
16(10):1050–1057, Jun 2020.

[2] Hsin-Yuan Huang, Richard Kueng, Giacomo Torlai, Victor V Albert, and
John Preskill. Provably efficient machine learning for quantum many-body
problems. arXiv preprint arXiv:2106.12627, 2021.
4

