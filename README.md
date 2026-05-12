# 🧬 BioTriton
### Triton-Powered GPU Bioinformatics Acceleration Framework

BioTriton is a research-oriented, GPU-native bioinformatics framework built to accelerate foundational sequence computations using OpenAI Triton and PyTorch. 

Unlike enterprise HPC pipelines (which focus on distributed SLURM clusters or MPI orchestration), BioTriton is specifically engineered for **single-GPU optimization** (e.g., RTX 40-series laptops, Google Colab). It serves as a masterclass in GPU systems engineering, memory bandwidth optimization, and hardware-level algorithmic parallelization.

---

## Key Features
* **Memory-Safe Sequence Batching:** Processes massive DNA sequences on limited VRAM by automatically splitting them into overlapping chunks, ensuring no $K$-mers are destroyed at chunk boundaries.
* **Kernel Fusion Optimization:** Bypasses PCIe bandwidth bottlenecks by fusing sequence encoding directly into computational kernels, performing math entirely within the GPU's L1 cache.
* **Wavefront Dynamic Programming:** Solves the notorious GPU serialization problem in Sequence Alignment by forcing thread execution across anti-diagonals, computing millions of cells simultaneously without race conditions.
* **Zero-Overhead Graph Edges:** Generates De Bruijn graph nodes instantly using pure Base-4 bitwise operations (right-shifts and masks) instead of slow string manipulation.

---

## Architecture Overview
BioTriton Lite operates on a hybrid orchestrator model:
1. **Python Control Layer:** Manages sequence ingestion, VRAM limits, and kernel dispatch logic.
2. **PyTorch Tensor Bridge:** Facilitates high-speed transfer of raw ASCII bytes over the PCIe bus directly to GPU memory.
3. **OpenAI Triton Kernels:** Custom-compiled GPU kernels execute math-heavy primitives (hashing, reductions, wavefront DP) directly on the Streaming Multiprocessors (SMs).
4. **Visualization Engine:** Decodes integer tensors back to sequence strings for rendering via NetworkX and Matplotlib.

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
