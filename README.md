# Real-Time Embedded AI Systems: Scheduling and Resource Orchestration for Multi-DNN Edge Intelligence

## Objective

This repository accompanies our survey paper:

**Real-Time Embedded AI Systems: Resource Orchestration for Multi-DNN Edge Intelligence**

This survey provides a comprehensive review of runtime scheduling and resource orchestration techniques for embedded AI systems. We systematically analyze existing research from the perspectives of workload characteristics, hardware heterogeneity, runtime constraints, decision policies, orchestration granularity, and optimization objectives. Beyond summarizing existing systems, we identify key design tradeoffs, common architectural patterns, and emerging research opportunities for future AI runtime systems.

## Contents

- [1. Related Survey](#1-Related-Survey)
- [2. Resource Orchestration Mechanisms](#2-Resource-Orchestration-Mechanisms)
  - [2.1 Compute Resource Orchestration](#21-Compute-Resource-Orchestration)
  - [2.2 Memory Resource Orchestration](#22-Memory-Resource-Orchestration)
  - [2.3 Accelerator Resource Orchestration](#23-Accelerator-Resource-Orchestration)
  - [2.4 Processor Frequency and Energy Orchestration](#24-Processor-Frequency-and-Energy-Orchestration)
- [3. Runtime Decision-Making Strategies](#3-Runtime-Decision-Making-Strategies)
  - [3.1 Rule-Based and Priority-Driven Scheduling](#31-Rule-Based-and-Priority-Driven-Scheduling)
  - [3.2 Optimization-Based Scheduling](#32-Optimization-Based-Scheduling)
  - [3.3 Control-Theoretic Scheduling](#33-Control-Theoretic-Scheduling)
  - [3.4 Profile-Guided and Learning-Driven Scheduling](34-Profile-Guided-and-Learning-Driven-Scheduling)
- [4. Design Tradeoffs and System Insights](#4-Design-Tradeoffs-and-System-Insights)
  - [4.1 Granularity Tradeoffs: Control Precision vs. Management Overhead](#41-Granularity-Tradeoffs-Control-Precision-vs-Management-Overhead)
  - [4.2 Compute-Memory Tradeoffs: Compute Throughput vs. Memory Efficiency](#42-Compute-Memory-Tradeoffs-Compute-Throughput-vs-Memory-Efficiency)
  - [4.3 Predictability-Adaptivity Tradeoffs: Guarantees vs. Flexibility](#43-Predictability-Adaptivity-Tradeoffs-Guarantees-vs-Flexibility)
  - [4.4 Isolation-Sharing Tradeoffs: Safety Guarantees vs. System Efficiency](#44-Isolation-Sharing-Tradeoffs-Safety-Guarantees-vs-System-Efficiency)
  - [4.5 Latency-Energy-Accuracy Tradeoffs: Multi-Objective Tension](#45-Latency-Energy-Accuracy-Tradeoffs-Multi-Objective-Tension)

- [5. Open Challenges and Future Directions](#5-Open-Challenges-and-Future-Directions)
  - [5.1 LLM-Aware Runtime Systems](#51-LLM-Aware-Runtime-Systems)
  - [5.2 Memory-Centric Resource Management](#52-Memory-Centric-Resource-Management)
  - [5.3 AI-Native Runtime Systems](#53-AI-Native-Runtime-Systems)
  - [5.4 Verified and Explainable Scheduling](#54-Verified-and-Explainable-Scheduling)

## Citation

If you think this repo is useful, please cite our paper:

```bibtex
@ARTICLE{xxx,
  author={Jing Huang, Zihao Deng and ZongHua Gu},
  journal={ACM Computing Survey}, 
  title={Real-Time Embedded AI Systems: Scheduling and Resource Orchestration for Multi-DNN Edge Intelligence}, 
  year={2026},
  volume={},
  number={},
  pages={},
  keywords={},
  doi={xxx/xxxx}}
```

## Usage

**Workload** :

| Static Workloads | Dynamic Workloads | Pipeline Workloads | Generative Workloads |
| :--------------: | :---------------: | :----------------: | :------------------: |
|        ○         |         ●         |         ◎          |          ❖           |

**Hardware** :

| CPU  | GPU  | NPU  | SoC  |
| :--: | :--: | :--: | :--: |
|  ♡   |  ♢   |  ♧   |  ♤   |

**Constraints and Objectives** :

| latency | Throughput | Accuracy | energy | Utilization | Memory | Predictability |
| :-----: | :--------: | :------: | :----: | :---------: | :----: | :------------: |
|    ♣    |     ♠      |    ♦     |   ♥    |      ✿      |   ▲    |       ✤        |

**Orchestration** :

| Compute | Memory | Accelerator | Frequency-Energy |
| :-----: | :----: | :---------: | :--------------: |
|    Ⓒ    |   Ⓜ    |      Ⓐ      |        Ⓕ         |

**Granularity** :

| model | Batch | Layer | kernel |
| :---: | :---: | :---: | :----: |
|   ✦   |   ★   |   ✫   |   ✪    |

**Policy** :

Heuristic/ Optimization/ Control/ Learning

## 1. Related Survey

| Title                                                        | Venue  | Date    | Code                                                         | Summary                                                      |
| ------------------------------------------------------------ | ------ | ------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [[2501.03265\] Cognitive Edge Computing: A Comprehensive Survey on Optimizing Large Models and AI Agents for Pervasive Deployment](https://arxiv.org/abs/2501.03265) | arXiv  | 2025-11 | -                                                            | This paper systematically reviews the emerging paradigm of cognitive edge computing, which aims to solve the deployment problems of LLMs and AI Agents on resource constrained edge devices. And point out the remaining challenges: lack of modal perception inference benchmark, opaque energy reporting, insufficient edge security/alignment evaluation, and lack of multi-agent testing bed. |
| [Edge intelligence unleashed: a survey on deploying large language models in resource-constrained environments](https://acnsci.org/journal/index.php/jec/article/view/1000) | JEC    | 2025    | -                                                            | This article provides a systematic overview of the techniques for deploying large language models on resource constrained edge devices. The article also identified key research gaps such as multimodal processing, federated learning integration, and hardware software collaborative design, providing a systematic framework for edge intelligence. |
| [Deep Learning Workload Scheduling in GPU Datacenters: A Survey ACM Computing Surveys](https://dl.acm.org/doi/full/10.1145/3638757) | CS     | 2024-1  | [S-Lab-System-Group/Awesome-DL-Scheduling-Papers](https://github.com/S-Lab-System-Group/Awesome-DL-Scheduling-Papers) | This review article provides the first systematic overview of scheduling techniques for deep learning (DL) training and inference tasks in GPU data centers. This review article published in ACM Computing Surveys provides the first systematic overview of scheduling techniques for deep learning (DL) training and inference tasks in GPU data centers. |
| [Deep Learning for Edge Computing Applications: A State-of-the-Art Survey ](https://ieeexplore.ieee.org/document/9044329) | Access | 2020-3  | -                                                            | This overview article systematically combs the fusion status of deep learning (DL) in edge computing applications for the first time. On the whole, this review provides a systematic theoretical basis and practical guidance for understanding and promoting the application of edge computing enabled by deep learning. |
| [Efficient Deep Learning Infrastructures for Embedded Computing Systems: A Comprehensive Survey and Future Envision](https://dl.acm.org/doi/10.1145/3701728) | TECS   | 2024-12 | -                                                            | This article systematically outlines the efficient deep learning infrastructure for embedded computing systems. The article points out that with the tremendous success of deep neural networks in various visual and language tasks, model complexity continues to grow, while embedded system resources are limited, and the computing gap between the two is widening. Efficient infrastructure is urgently needed to promote the popularization of embedded intelligence. |
| [Deep Reinforcement Learning-Based Task Scheduling and Resource Allocation for Vehicular Edge Computing: A Survey](https://ieeexplore.ieee.org/document/11173255) | TITC   | 2025-9  | -                                                            | This review systematically reviews the latest research progress of Deep Reinforcement Learning (DRL) in the field of task scheduling and resource allocation of Internet of Vehicles edge computing (VEC). This review provides a systematic theoretical framework and practical guidance for the research on DRL driven scheduling and resource optimization in VEC for intelligent transportation systems. |

## 2. Resource Orchestration Mechanisms

### 2.1 Compute Resource Orchestration

|                            Title                             |  year<br />Venue  |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                      Code                      |
| :----------------------------------------------------------: | :---------------: | :--: | :--: | :--------: | :--: | :--: | :--------------: | :--------------------------------------------: |
| [DUET: A Compiler-Runtime Subgraph Scheduling Approach for Tensor Programs on a Coupled CPU-GPU Architecture](https://ieeexplore.ieee.org/abstract/document/9460468) |  2021<br />IPDPS  |  ○   |  ♡♢  |     ♣      |  ⒸⒶ  |  ✫   |  **Heuristic**   |                       -                        |
| [Real-Time Multitasking of Deep Neural Networks With Nvidia Tensorrt ](https://ieeexplore.ieee.org/abstract/document/11315110) |  2025<br />RTSS   |  ○   |  ♡♢  |    ♣\|✤    |  ⒸⒶ  |  ✫   | **Optimization** |                       -                        |
| [Pantheon: Preemptible Multi-DNN Inference on Mobile Edge GPUs](https://dl.acm.org/doi/abs/10.1145/3643832.3661878) | 2024<br />MOBISYS |  ○   |  ♢   |   ♣♦\|♣    |  ⒸⒶ  |  ✫   | **Optimization** |  [Code](https://zenodo.org/records/11094058)   |
| [Miriam: Exploiting Elastic Kernels for Real-time Multi-DNN Inference on Edge GPU](https://dl.acm.org/doi/abs/10.1145/3625687.3625789) | 2023<br />SenSys  |  ○   |  ♡♢  |   ♣♠\|♣    |  ⒸⒶ  |  ✪   |  **Heuristic**   |                       -                        |
| [DynaMIX: Resource Optimization for DNN-Based Real-Time Applications on a Multi-Tasking System](https://arxiv.org/abs/2302.01568) |  2023<br />arXiv  |  ○   |  ♡   |   ♣♦\|♣▲   |  ⒸⓂ  |  ✫   | **Optimization** |                       -                        |
| [DREAM: A Dynamic Scheduler for Dynamic Real-time Multi-model ML Workloads ](https://dl.acm.org/doi/abs/10.1145/3623278.3624753) | 2023<br />ASPLOS  |  ●   |  ♧   |   ♣♥\|♣♥   | ⒸⒶⒻ  |  ✦   |  **Heuristic**   |                       -                        |
| [ODMDEF: On-Device Multi-DNN Execution Framework Utilizing Adaptive Layer-Allocation on General Purpose Cores and Accelerators](https://ieeexplore.ieee.org/abstract/document/9453793) | 2021<br />Access  |  ○   | ♡♢♧  |     ♣      | ⒸⒶⓂ  |  ✫   |   **Learning**   |                       -                        |
| [AxoNN nergy-aware execution of neural network inference on multi-accelerator heterogeneous SoCs ](https://dl.acm.org/doi/abs/10.1145/3489517.3530572) |   2022<br />DAC   |  ○   | ♡♢♧  |    ♣\|♥    |  ⒶⒻ  |  ✫   | **Optimization** |                       -                        |
| [Shared Memory-contention-aware Concurrent DNN Execution for Diversely Heterogeneous System-on-Chips ](https://dl.acm.org/doi/abs/10.1145/3627535.3638502) |  2024<br />PPoPP  |  ○   | ♡♢♧  |    ♣♠▲     |  ⒸⒶ  |  ✫   | **Optimization** | [code](https://github.com/ismetdagli/HaX-CoNN) |
| [GCAPS: GPU Context-Aware Preemptive Priority-based Scheduling for Real-Time Tasks](https://arxiv.org/abs/2406.05221) |  2024<br />arXiv  |      |      |            |      |      |                  |                                                |
|                                                              |                   |      |      |            |      |      |                  |                                                |
|                                                              |                   |      |      |            |      |      |                  |                                                |



### 2.2 Memory Resource Orchestration

### 2.3 Accelerator Resource Orchestration

### 2.4 Processor Frequency and Energy Orchestration



## 3. Runtime Decision-Making Strategies

### 3.1 Rule-Based and Priority-Driven Scheduling

### 3.2 Optimization-Based Scheduling

### 3.3 Control-Theoretic Scheduling

### 3.4 Profile-Guided and Learning-Driven Scheduling



## 4. Design Tradeoffs and System Insights

### 4.1 Granularity Tradeoffs: Control Precision vs. Management Overhead

### 4.2 Compute-Memory Tradeoffs: Compute Throughput vs. Memory Efficiency

### 4.3 Predictability-Adaptivity Tradeoffs: Guarantees vs. Flexibility

### 4.4 Isolation-Sharing Tradeoffs: Safety Guarantees vs. System Efficiency

### 4.5 Latency-Energy-Accuracy Tradeoffs: Multi-Objective Tension



## 5. Open Challenges and Future Directions

### 5.1 LLM-Aware Runtime Systems

### 5.2 Memory-Centric Resource Management

### 5.3 AI-Native Runtime Systems

### 5.4 Verified and Explainable Scheduling