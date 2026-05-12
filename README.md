# 🧬 BioTriton
### Triton-Powered GPU Bioinformatics Acceleration Framework

BioTriton is a research-oriented, GPU-native bioinformatics framework built to accelerate foundational sequence computations using OpenAI Triton and PyTorch. 

Unlike enterprise HPC pipelines (which focus on distributed SLURM clusters or MPI orchestration), BioTriton is specifically engineered for **single-GPU optimization** (e.g., RTX 40-series laptops, Google Colab). It serves as a masterclass in GPU systems engineering, memory bandwidth optimization, and hardware-level algorithmic parallelization.

---

## 🛠️ Key Features
* **Memory-Safe Sequence Batching:** Processes massive DNA sequences on limited VRAM by automatically splitting them into overlapping chunks, ensuring no $K$-mers are destroyed at chunk boundaries.
* **Kernel Fusion Optimization:** Bypasses PCIe bandwidth bottlenecks by fusing sequence encoding directly into computational kernels, performing math entirely within the GPU's L1 cache.
* **Wavefront Dynamic Programming:** Solves the notorious GPU serialization problem in Sequence Alignment by forcing thread execution across anti-diagonals, computing millions of cells simultaneously without race conditions.
* **Zero-Overhead Graph Edges:** Generates De Bruijn graph nodes instantly using pure Base-4 bitwise operations (right-shifts and masks) instead of slow string manipulation.

---

## 🏗️ Architecture Overview
BioTriton operates on a hybrid orchestrator model:
1. **Python Control Layer:** Manages sequence ingestion, VRAM limits, and kernel dispatch logic.
2. **PyTorch Tensor Bridge:** Facilitates high-speed transfer of raw ASCII bytes over the PCIe bus directly to GPU memory.
3. **OpenAI Triton Kernels:** Custom-compiled GPU kernels execute math-heavy primitives (hashing, reductions, wavefront DP) directly on the Streaming Multiprocessors (SMs).
4. **Visualization Engine:** Decodes integer tensors back to sequence strings for rendering via NetworkX and Matplotlib.

---

## ⚡ Performance Benchmarks

*Hardware Context: CPU: AMD Ryzen 7 7840HS | GPU: NVIDIA RTX 4050 (6GB VRAM)*

| Operation | CPU Baseline | BioTriton (GPU) | Speedup |
| :--- | :--- | :--- | :--- |
| **K-mer Extraction (K=31)** | 7.830s | 0.075s | **~104x** |
| **Hamming Distance (Fused)** | 6.938ms | 3.410ms | **~2x (over Unfused GPU)** |
| **Global Alignment (NW)** | 16.69s | 0.242s | **~68x** |

*(Benchmarks calculated against a 5,000,000 bp synthetic sequence, or a 25,000,000 cell DP matrix for alignment).*

---

## Technical Highlights
* **Beating the Garbage Collector:** Transitioned from Python string parsing to raw byte array transfers to avoid memory overhead bottlenecks.
* **Algorithmic Parallelization:** Implemented a diagonal traversal mechanism for $O(N^2)$ dynamic programming matrices, effectively parallelizing Needleman-Wunsch algorithm execution.
* **Bitwise Representation:** Mapped genomic data (A, C, G, T) to 2-bit integers, allowing sequence extraction and graph overlap logic to be resolved in single clock cycles using bitwise `>>` and `&` operators.

---

## Current Scope
BioTriton is designed as a **single-node, local environment** for algorithmic research, education, and rapid prototyping. It serves as a proving ground for testing custom hardware-accelerated bioinformatics primitives before scaling them up to enterprise infrastructure.

---

## Future Roadmap
* **Eulerian Path Assembly:** Implement traversal algorithms to stitch De Bruijn graph components back into full contiguous sequences (contigs).
* **Block-Level Wavefronts:** Move DP diagonal orchestration entirely into Triton using block-level synchronization to handle ultra-massive matrices.
* **Heuristic Alignment Search:** Introduce seed-and-extend functionality (similar to BLAST) to rapidly query large sequence databases.

---

## Current Limitations
* Optimized exclusively for single-GPU CUDA systems.
* Alignment kernels currently target moderate matrix sizes.
* Graph infrastructure is experimental.
* No distributed execution support.
* No production-scale genome assembly orchestration.
* Benchmarked primarily on synthetic and small biological datasets.

## License
Distributed under the MIT License. See LICENSE for more information.
