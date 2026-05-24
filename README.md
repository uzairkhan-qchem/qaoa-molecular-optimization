# QAOA for Chemistry‑Inspired Graph Optimization

> 🚧 **Work in Progress** – This project implements the Quantum Approximate Optimization Algorithm (QAOA) on a weighted Max‑Cut problem that mimics molecular conformation optimization (e.g., torsional angle assignment). Current results show that a single‑layer QAOA (p=1) does not outperform classical brute‑force, but the pipeline is ready for deeper circuits, larger graphs, and noise analysis.

## 🧪 Problem Overview

We consider a small **weighted 4‑cycle graph** where vertices represent binary variables (e.g., two states of a torsional angle) and edge weights represent steric or energetic penalties. The goal is to find the **maximum cut** – a partition of vertices into two sets that maximises the sum of weights of edges crossing the cut. This is equivalent to finding the ground state of an antiferromagnetic Ising model.

- **Graph:** 4 nodes, edges (0‑1):1.0, (1‑2):0.8, (2‑3):1.0, (3‑0):0.8  
- **Classical optimum:** Cut value = 3.6 (partition [1,0,1,0] or [0,1,0,1])

## 🔬 QAOA Implementation

- **Framework:** Qiskit + Aer (ideal and noisy simulation)  
- **Number of qubits:** 4  
- **QAOA depth:** p = 1 (one cost layer + one mixer layer)  
- **Cost Hamiltonian:** `H_C = Σ w_ij (1 - Z_i Z_j)/2`  
- **Mixer Hamiltonian:** `H_M = Σ X_i`  
- **Optimizer:** COBYLA (scipy) – 30 iterations  

## 📊 Results

| Simulation | Expected Max‑Cut | Probability of optimal bitstrings (1010/0101) |
|------------|------------------|------------------------------------------------|
| Ideal (no noise) | 1.066 (suboptimal) | 9.0% |
| Noisy (depolarizing: 1% 1‑qubit, 2% 2‑qubit) | 1.126 | 8.8% |

**Key observations:**
- QAOA with p=1 got stuck in a local minimum, far from the true optimum 3.6.
- Noise slightly degraded performance but the main issue is insufficient circuit depth (`p=1`).
- The probability of measuring the optimal partitions is low (<10%), meaning the algorithm does not reliably solve the problem.

## ❌ Why No Quantum Advantage (Yet)

1. **Low depth** – A 4‑cycle requires at least p=2 to approximate the max cut correctly.  
2. **Local minima** – COBYLA converged prematurely; better optimisers or multiple restarts would help.  
3. **Small problem** – Classical brute‑force is trivial, so quantum advantage is not expected.  
4. **Noise** – Depolarising error reduces solution quality, but the main bottleneck is algorithmic.

## 🚀 Future Directions (WIP)

- [x] Implement QAOA with p=1 on a small graph  
- [x] Add noise model and compare ideal vs noisy  
- [ ] Increase QAOA depth to p=2 and p=3, compare solution quality  
- [ ] Use a more robust optimizer (SPSA, differential evolution)  
- [ ] Map a real molecular problem (e.g., n‑butane torsional angles) to Max‑Cut  
- [ ] Run on real quantum hardware (IBM Quantum, AWS Braket) with error mitigation  

## 📝 License
MIT – free to use, modify, and distribute with attribution.

## 🙏 Acknowledgements
- Qiskit team for the quantum computing framework
- QAOA literature (Farhi, Goldstone, Gutmann) for the original algorithm
