# 🥈 Q-volution Hackathon: Quantum Harmonic Oscillator Solver

**Team:** PlanQtons  
**Achievement:** 2nd Place | Track C: Solving Linear Differential Equations  
**Organizers:** [Girls In Quantum](https://girlsinquantum.com/) & [Classiq](https://www.classiq.io/) (Hosted on [Aqora](https://aqora.io/))

## Overview

This repository contains Team PlanQtons' submission for the Q-volution Hackathon. Our challenge was to implement a functional quantum algorithm capable of solving a classical Linear Differential Equation (LDE), specifically the harmonic oscillator. And finally analyze how resource-efficient our implementation is by comparing circuit depth and width under different optimization settings.

Building upon the theoretical framework of *"A Quantum Algorithm for Solving Linear Differential Equations: Theory and Experiment"* (Tao Xin et al., 2020), we engineered a highly optimized quantum circuit using the **Classiq** platform. 

### The Problem
We modeled the quantum harmonic oscillator defined by:
$$ y'' + \omega^2 y = 0 \quad \text{where } \omega=1 $$
**Initial Conditions:** $y(0)=1, \quad y'(0)=1$

Our goal was to solve this over the time interval $t \in[0, 1]$, calculate the Kinetic and Potential energies, and analyze the algorithm's resource efficiency.

---

## Our Journey & Key Innovations

Translating a theoretical paper into a practical, optimized quantum circuit requires bridging a significant gap. Here is how Team PlanQtons tackled the challenge:

### 1. Vectorization & Mathematical Formulation
We converted the 2nd-order differential equation into a 1st-order system:
$$ \frac{dX}{dt} = \begin{bmatrix} x_2 \\ -x_1 \end{bmatrix} = \mathcal{M}X $$
We identified that the transformation matrix $\mathcal{M}$ acts like a bit-flip with a phase, allowing us to map it directly to the Pauli-Y matrix: **$\mathcal{M} = iY$**. Because $Y$ is unitary, $\mathcal{M}$ preserves the norm, and the solution takes the form $X(t) = \exp(iYt)X(0)$.

> **Want to see our raw math?** Check out our original[Handwritten Mathematical Derivations & Circuit Blueprints](PlanQtons_Mathematical_Derivations.pdf) detailing exactly how we went from the classical differential equation to proving $M$ is unitary and calculating our energy sanity checks!

### 2. Standard LCU Implementation ($k=3$)
We initially approximated the exponential using a Taylor series expansion:
$$ X(t) \approx \sum_{m=0}^{k} \frac{(iYt)^m}{m!} X(0) $$
Using the Linear Combination of Unitaries (LCU) method for $k=3$, we utilized **1 work qubit and 2 ancilla qubits**, successfully simulating the system and computing the energy fractions. 

### 3. The "Eureka!" Moment: The Grouping Method 💡 (Our Winning Optimization)
While analyzing the cyclic properties of $\mathcal{M}^m$ on paper, we noticed a profound mathematical simplification. By grouping the Taylor series terms, the expansion naturally resolves into trigonometric functions:
$$ X(t) \approx (\cos t \cdot I + i \sin t \cdot Y) X(0) $$
*(You can see the exact moment we realized this on page 5 of our [handwritten notes](PlanQtons_Mathematical_Derivations.pdf)!)*

**The Impact:** This realization allowed us to design a custom circuit that mathematically represents the exact solution using **only a single ancilla qubit**. This drastically reduced circuit depth and represented the most resource-efficient solution possible for this problem.

### 4. Advanced Generalization ($k$-term expansion)
To fulfill the hackathon's requirement of analyzing parameter trade-offs, we built a generalized circuit capable of taking any Taylor expansion order $k$. We successfully proved and visualized the exponential error reduction (on a logarithmic scale) against the linear growth in circuit depth as $k$ increases.

---

## 📊 Results & Deliverables

Our repository includes the following major analytical deliverables:

1. **[Handwritten Mathematical Formulation](PlanQtons_Mathematical_Derivations.pdf):** Our raw, 10-page step-by-step derivation of the state matrix, Taylor expansion groupings, and LCU circuit blueprint.
2. **Energy Conservation:** Simulated measurement of the Work Qubit dynamically plotted the Potential Energy (P.E.) and Kinetic Energy (K.E.) fractions over time, proving that $E_{total} = 1$ is conserved.
3. **Resource vs. Accuracy Trade-off:** By sweeping Classiq's `error_bound` parameter, we visualized the exact trade-off between **Circuit Depth** (Gate Count) and **Relative Physics Error**.
4. **Advanced $k$-Scaling Analysis:** Demonstrated the relationship between Taylor expansion depth, absolute error, and the required quantum hardware overhead.
---

## 💻 How to Run the Code

### Prerequisites
The code is built using the `classiq` Python SDK. You will need an active Classiq account to authenticate and synthesize the circuits.

```bash
pip install classiq numpy matplotlib
```
### Execution
1. Clone the repository.
2. Run the Jupyter Notebook.

Note: A browser window will pop up asking you to authenticate with Classiq upon the first run. If a separate window does not pop up you would have to click the link shown in the output directly below the codeblock.

---

### Team PlanQtons
We are incredibly proud of our collaboration, late-night debugging sessions, and the mathematical breakthrough that secured our place on the podium.

**Nathalia Rivera Orellana**

[Linkedin](https://www.linkedin.com/in/nathalia-rivera-orellana-8b8945368/) | [Github](https://github.com/FishyyyCataclysmicFish)

**Chelsie Hadley** 

[Linkedin](https://www.linkedin.com/in/chelsie-hadley/) | [Github](https://github.com/chelsiehadley)

**Animesh Banik** 

[Linkedin](https://www.linkedin.com/in/animesh-banik/) | [Github](https://github.com/AnimeshBanik144)

---

### References
Xin, T., et al. "A Quantum Algorithm for Solving Linear Differential Equations: Theory and Experiment." (2020). ([Physical Review A](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.101.032307)) ([arXiv](https://doi.org/10.48550/arXiv.1807.04553))

[Classiq Documentation & Library](https://docs.classiq.io/latest/)