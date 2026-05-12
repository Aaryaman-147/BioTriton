# 🧬 BioTriton
### Triton-Powered GPU Bioinformatics Acceleration Framework

BioTriton is a research-oriented, GPU-native bioinformatics framework built to accelerate foundational sequence computations using OpenAI Triton and PyTorch. 

Unlike enterprise HPC pipelines (which focus on distributed SLURM clusters or MPI orchestration), BioTriton is specifically engineered for **single-GPU optimization** (e.g., RTX 40-series laptops, Google Colab). It serves as a masterclass in GPU systems engineering, memory bandwidth optimization, and hardware-level algorithmic parallelization.

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
