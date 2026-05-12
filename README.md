# 🧬 BioTriton
### Triton-Powered GPU Bioinformatics Acceleration Framework

BioTriton is a research-oriented, GPU-native bioinformatics framework built to accelerate foundational sequence computations using OpenAI Triton and PyTorch. 

Unlike enterprise HPC pipelines (which focus on distributed SLURM clusters or MPI orchestration), BioTriton is specifically engineered for **single-GPU optimization** (e.g., RTX 40-series laptops, Google Colab). It serves as a masterclass in GPU systems engineering, memory bandwidth optimization, and hardware-level algorithmic parallelization.

---

## 🚀 Key Features

* **Memory-Safe Sequence Batching:** Process massive sequences (e.g., full human chromosomes) on limited VRAM (≤ 6GB) using mathematically safe overlapping window generators.
* **Deep Systems Optimization (Kernel Fusion):** Bypass PCIe VRAM bottlenecks. Execute operations like Hamming Distance calculation directly in the streaming multiprocessor (SM) L1 cache/registers.
* **Anti-Diagonal Wavefront Parallelism:** Needleman-Wunsch dynamic programming matrices are notorious for serializing GPUs. This engine forces threads to traverse the DP matrix diagonally, unlocking massive parallel speedups without race conditions.
* **Bitwise De Bruijn Graphs:** Extract $K$-mer nodes and edges for genome assembly instantly using hyper-fast Base-4 integer bit-shifting instead of slow string parsing.

---

## ⚡ Performance Benchmarks

*Hardware Context: CPU: AMD Ryzen 7 7840HS | GPU: NVIDIA RTX 4050 (6GB VRAM)*

| Operation | CPU Baseline | BioTriton Lite (GPU) | Speedup |
| :--- | :--- | :--- | :--- |
| **K-mer Extraction (K=31)** | 7.830s | 0.075s | **~104x** |
| **Hamming Distance (Fused)** | 6.938ms | 3.410ms | **~2x (over Unfused GPU)** |
| **Global Alignment (NW)** | 16.69s | 0.242s | **~68x** |

*(Benchmarks calculated against a 5,000,000 bp synthetic sequence, or a 25,000,000 cell DP matrix for alignment).*

---
