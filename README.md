# Mathematical Modeling & Simulation of the Repressilator

## Overview
This repository contains the numerical simulation and mathematical analysis of the **Repressilator**, a synthetic oscillatory network of transcriptional regulators. The model explores the dynamics of a genetic circuit consisting of three repressor proteins (LacI, TetR, and CI) arranged in a cyclic inhibitory feedback loop. 


![Circuit Diagram](figures/3-gene/png)

## Background
The repressilator was originally designed and implemented in *Escherichia coli* by Elowitz and Leibler (2000). By utilizing a negative feedback loop with an odd number of components, the system avoids stable steady states under certain parameter regimes, leading to sustained oscillations in protein concentration. This project mathematically proves the conditions for these oscillations and simulates them using Python.

## The Mathematical Model

The network is modeled using a system of six coupled ordinary differential equations (ODEs)—three for mRNA transcripts ($m_i$) and three for their corresponding proteins ($p_i$):

$$\frac{dm_i}{dt} = -m_i + \frac{\alpha}{1 + p_j^n} + \alpha_0$$

$$\frac{dp_i}{dt} = -\beta(p_i - m_i)$$

Where:
* $i \in \{lacI, tetR, cI\}$ and $j$ is the preceding repressor.
* $\alpha$: Maximum transcription rate.
* $\alpha_0$: Basal leakiness of the promoter.
* $\beta$: Ratio of protein decay rate to mRNA decay rate.
* $n$: Hill coefficient (cooperativity).

### Steady-State Analysis
At steady state, the derivatives are zero ($\frac{dm_i}{dt} = \frac{dp_i}{dt} = 0$). Assuming symmetry across the three genes, we get $m_i^* = m^*$ and $p_i^* = p^*$. 

From the protein equation, this gives $p^* = m^*$. Substituting this into the mRNA equation yields the steady-state condition:

$$p^* = \frac{\alpha}{1 + p^{*n}} + \alpha_0$$

### Linear Stability & The Jacobian
To determine the stability of this steady state, we perform linear analysis. The $6 \times 6$ Jacobian matrix can be transformed using the cube roots of unity ($\lambda_k$) into three simpler $2 \times 2$ block matrices for each mode $k \in \{0, 1, 2\}$:

$$J_k = \begin{pmatrix} -1 & f'\lambda_k \\ \beta & -\beta \end{pmatrix}$$

Here, $f'$ is the derivative of the repression function evaluated at the steady state:

$$f' = \frac{-\alpha n p^{*n-1}}{(1 + p^{*n})^2}$$

* For **mode $k = 0$**, $\lambda_0 = 1$. The Routh-Hurwitz criterion shows this mode is always unconditionally stable.
* For **modes $k = 1, 2$**, the eigenvalues are complex conjugates. Under specific parameter thresholds for $\alpha$ and $\beta$, the real parts of these eigenvalues cross zero and become positive. This crossing represents a **Hopf bifurcation**, causing the steady state to become unstable and giving rise to a stable limit cycle (sustained oscillations).


> *(Note for you: If you write the code to create an animated GIF of the 3D phase space from your simulation, this is the perfect place to put it! Link it using `![Simulation GIF](assets/animation.gif)`)*

## Simulation Results

The Jupyter Notebook (`IDC401_Project.ipynb`) included in this repository numerically integrates the ODEs to visualize these dynamics. 

### Key Visualizations in the Notebook:
1.  **Time Series Simulation:** Demonstrates the out-of-phase, sustained oscillations of the three repressor proteins.
2.  **Bifurcation Diagram:** Maps the local extrema of the protein concentrations against the control parameter $\alpha$, visually identifying the exact point of the Hopf bifurcation where oscillations begin.

> *(Note for you: You can take a screenshot of your time-series plot or bifurcation diagram from your notebook and embed it here using `![Time Series](assets/timeseries.png)`)*

## Repository Structure
* `IDC401_Project.ipynb`: The main Jupyter Notebook containing the ODE definitions, SciPy numerical integration, and matplotlib visualizations.
* `Presentation.pptx`: The summary presentation slides detailing the project for IDC 401.
* `A synthetic oscillatory network of transcriptional regulators.pdf`: The original 2000 *Nature* paper used as the primary reference.

## How to Run

1.  Clone this repository to your local machine:
    ```bash
    git clone [https://github.com/yourusername/repressilator-dynamics.git](https://github.com/yourusername/repressilator-dynamics.git)
    ```
2.  Ensure you have the required Python libraries installed:
    ```bash
    pip install numpy scipy matplotlib jupyter
    ```
3.  Open the Jupyter Notebook and run the cells sequentially:
    ```bash
    jupyter notebook IDC401_Project.ipynb
    ```
**Authors:** Bhavya (MS21112) & Kumar (MS21266)  
*Course: IDC 401 (Theoretical Biology)*

## References
* Elowitz, M. B., & Leibler, S. (2000). A synthetic oscillatory network of transcriptional regulators. *Nature*, 403(6767), 335-338.
