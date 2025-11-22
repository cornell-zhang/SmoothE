<!--- SPDX-License-Identifier: Apache-2.0  -->
<!-- <img src="smoothe-icon.png" width=128/>  -->
<img src="smoothe-icon.png" width="60"/> SmoothE: Differentiable E-Graph Extraction
==============================================================================

## 📙 Overview

SmoothE is a novel approach for e-graph extraction that handles complex cost models through a probabilistic perspective.
By relaxing the original discrete optimization problem into a continuous differentiable form,
SmoothE minimizes the expected cost via gradient descent to optimize the probability distribution utilizing GPUs.

For a deep dive into the methodology, please refer to our paper: [SmoothE: Differentiable E-Graph Extraction](https://www.csl.cornell.edu/~zhiruz/pdfs/smoothe-asplos2025.pdf)

🚀 **Optimized Implementation**
> Note: This repository contains an optimized implementation of SmoothE, distinct from the original [ASPLOS'25 artifact version](https://github.com/yaohuicai/smoothe-artifact).

Key improvements in this version include:
* **Enhanced GPU Efficiency**: Significant speedups and improved GPU memory usage, especially for large graphs.
* **Improved Algorithm**: Improved stability and convergence of the SmoothE algorithm.
* **Usability**: Includes an automated launch script with hyper-parameter tuning for solving an entire benchmark.


## 🔥 Installation 
### Prerequisites
**Hardware**
To achieve optimal performance, high-end GPU hardware is required.
* Recommended: NVIDIA A100 or newer.
* Minimum: A CUDA-capable GPU with significant VRAM.

### Environment Setup
We recommend using Conda to manage dependencies.
1. Clone the repository:
```
git clone https://github.com/your-username/smoothe.git
cd smoothe
```
2. Create and activate the environment:

```
conda env create -f env.yaml
conda activate smoothe
```

## 🏁 Quick Start
This library includes a launch script (`script/solve_benchmark.py`) that handles auto-tuning of hyper-parameters for you.
**Running a Benchmark**
This will solve all instances in the specified benchmark using SmoothE.
```
python script/solve_benchmark.py ${PATH_TO_BENCHMARK}
```
**Arguments:**
* ${PATH_TO_BENCHMARK}: The relative path to your target benchmark file.

**Optional Arguments:**
* --time_limit: Set the maximum runtime per instance in seconds (Default: 120).

**Example:**
```
python script/solve_benchmark.py dataset/tensat --time_limit 60
```

**Output:**
Results are automatically saved to the `logs/` directory:
* `logs/${PATH_TO_BENCHMARK}.json`

<!-- dataset/set: 996738, 104632
dataset/maxsat: 3851, 3781
dataset/circuits: 109885, 47817
dataset/herbie: 51525, 9274
dataset/rover/box: 12537, 2852
dataset/rover/fir: 13037, 1604
dataset/rover/mcm: 16960, 2694
dataset/flexc: 19830, 4892
dataset/esyn: 32022, 15102
dataset/tensat: 57800, 34800
dataset/diospyros: 15384, 1671
dataset/impress: 102030, 90312
dataset/boole/mapped: 303327, 154812
dataset/boole/nonmapped: 416269, 163586
dataset/emorphic: 190310, 146160 -->
## 💿 Dataset
This repository also includes an extensive benchmark dataset for testing.
The sources and statistics of the dataset are as follows:
| Benchmark | # Instances | # E-nodes | # E-classes |
|:---|:---|:---|:---|
| [rover/box](https://ieeexplore.ieee.org/abstract/document/10579443/) | 3 | 12,537 | 2,852 |
| [rover/fir](https://ieeexplore.ieee.org/abstract/document/10579443/) | 4 | 13,037 | 1,604 |
| [rover/mcm](https://ieeexplore.ieee.org/abstract/document/10579443/) | 2 | 16,960 | 2,694 |
| [flexc](https://arxiv.org/abs/2309.091121) | 14 | 19,830 | 4,892 |
| [tensat](https://proceedings.mlsys.org/paper_files/paper/2021/hash/cc427d934a7f6c0663e5923f49eba531-Abstract.html) | 5 | 57,800 | 34,800 |
| [diospyros](https://dl.acm.org/doi/abs/10.1145/3445814.3446707) | 10 | 15,384 | 1,671 |
| [impress](https://ieeexplore.ieee.org/abstract/document/9786123) | 3 | 102,030 | 90,312 |
| [set](https://dl.acm.org/doi/10.1145/3669940.3707262) | 4 | 996,738 | 104,632 |
| [maxsat](https://dl.acm.org/doi/10.1145/3669940.3707262) | 6 | 3,851 | 3,781 |
| [herbie](https://dl.acm.org/doi/10.1145/2737924.2737959) | 18 | 51,525 | 9,274 |
| [circuits](https://ieeexplore.ieee.org/document/11168886) | 28 | 109,885 | 47,817 |
| [esyn](https://dl.acm.org/doi/abs/10.1145/3649329.3656246) | 14 | 32,022 | 15,102 |
| [emorphic](https://arxiv.org/abs/2504.11574) | 9 | 190,310 | 146,160 |
| [boole/mapped](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=11132728) | 4 | 303,327 | 154,812 |
| [boole/nonmapped](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=11132728) | 6 | 416,269 | 163,586 |

Note that only a subset of the [*boole*](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=11132728) benchmark is included in this repository due to the GitHub file size limit.
The complete dataset can be downloaded from their [Hugging Face page](https://huggingface.co/datasets/SeaSkysz/eboost_dataset/tree/main/boole).

## 🤼 Comparing with SOTA 
With this optimized implementation, SmoothE achieves the state-of-the-art results.
Comparing with state-of-the-art extraction method, [*e-boost*](https://arxiv.org/abs/2508.13020) using ILP, 
SmoothE is comparable or even better, including on their own benchmarks.
For example, on *boole* and *e-morphic* benchmarks, SmoothE outperforms *e-boost* with CPLEX by 7.8% and 1.7% respectively in terms of geometric mean cost reduction, under the same time limit of 60 seconds per instance.

Note that *e-boost* can also be seen as an orthogonal technique to SmoothE.
By feeding the e-graphs pruned using *e-boost* into SmoothE,
sometimes results can be further improved, especially for large e-graphs.

## 📜 Citation 
If you use SmoothE in your research, please cite our ASPLOS '25 paper:
```
@inproceedings{cai2025smoothe,
  title={Smoothe: Differentiable e-graph extraction},
  author={Cai, Yaohui and Yang, Kaixin and Deng, Chenhui and Yu, Cunxi and Zhang, Zhiru},
  booktitle={International Conference on Architectural Support for Programming Languages and Operating Systems (ASPLOS)},
  pages={1020--1034},
  year={2025}
}
```

