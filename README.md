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

| Title                                                        | Venue  | Yaer |                             Code                             | Summary                                                      |
| ------------------------------------------------------------ | ------ | :--: | :----------------------------------------------------------: | ------------------------------------------------------------ |
| [Cognitive Edge Computing: A Comprehensive Survey on Optimizing Large Models and AI Agents for Pervasive Deployment](https://arxiv.org/abs/2501.03265) | arXiv  | 2025 |                              -                               | This paper systematically reviews the emerging paradigm of cognitive edge computing, which aims to solve the deployment problems of LLMs and AI Agents on resource constrained edge devices. And point out the remaining challenges: lack of modal perception inference benchmark, opaque energy reporting, insufficient edge security/alignment evaluation, and lack of multi-agent testing bed. |
| [Edge intelligence unleashed: a survey on deploying large language models in resource-constrained environments](https://acnsci.org/journal/index.php/jec/article/view/1000) | JEC    | 2025 |                              -                               | This article provides a systematic overview of the techniques for deploying large language models on resource constrained edge devices. The article also identified key research gaps such as multimodal processing, federated learning integration, and hardware software collaborative design, providing a systematic framework for edge intelligence. |
| [Deep Learning Workload Scheduling in GPU Datacenters: A Survey ACM Computing Surveys](https://dl.acm.org/doi/full/10.1145/3638757) | CS     | 2024 | [S-Lab-System-Group/Awesome-DL-Scheduling-Papers](https://github.com/S-Lab-System-Group/Awesome-DL-Scheduling-Papers) | This review article provides the first systematic overview of scheduling techniques for deep learning (DL) training and inference tasks in GPU data centers. This review article published in ACM Computing Surveys provides the first systematic overview of scheduling techniques for deep learning (DL) training and inference tasks in GPU data centers. |
| [Deep Learning for Edge Computing Applications: A State-of-the-Art Survey ](https://ieeexplore.ieee.org/document/9044329) | Access | 2020 |                              -                               | This overview article systematically combs the fusion status of deep learning (DL) in edge computing applications for the first time. On the whole, this review provides a systematic theoretical basis and practical guidance for understanding and promoting the application of edge computing enabled by deep learning. |
| [Efficient Deep Learning Infrastructures for Embedded Computing Systems: A Comprehensive Survey and Future Envision](https://dl.acm.org/doi/10.1145/3701728) | TECS   | 2024 |                              -                               | This article systematically outlines the efficient deep learning infrastructure for embedded computing systems. The article points out that with the tremendous success of deep neural networks in various visual and language tasks, model complexity continues to grow, while embedded system resources are limited, and the computing gap between the two is widening. Efficient infrastructure is urgently needed to promote the popularization of embedded intelligence. |
| [Deep Reinforcement Learning-Based Task Scheduling and Resource Allocation for Vehicular Edge Computing: A Survey](https://ieeexplore.ieee.org/document/11173255) | TITC   | 2025 |                              -                               | This review systematically reviews the latest research progress of Deep Reinforcement Learning (DRL) in the field of task scheduling and resource allocation of Internet of Vehicles edge computing (VEC). This review provides a systematic theoretical framework and practical guidance for the research on DRL driven scheduling and resource optimization in VEC for intelligent transportation systems. |



## 2. Resource Orchestration Mechanisms

### 2.1 Compute Resource Orchestration

| Title                                                        |      Author / Year / Venue      | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                      Code                      |
| :----------------------------------------------------------- | :-----------------------------: | :----------------------------------------------------------- | :--: | :--: | :--------: | :--: | :--: | :--------------: | :--------------------------------------------: |
| [DUET: A Compiler-Runtime Subgraph Scheduling Approach for Tensor Programs on a Coupled CPU-GPU Architecture](https://ieeexplore.ieee.org/abstract/document/9460468) |   Zhang et al.<br/>IPDPS 2021   | Decomposes inference pipelines into schedulable stages; turns end-to-end latency into a pipeline-scheduling problem with deterministic response-time guarantees. |  ○   |  ♡♢  |     ♣      |  ⒸⒶ  |  ✫   |  **Heuristic**   |                       -                        |
| [Real-Time Multitasking of Deep Neural Networks With Nvidia Tensorrt ](https://ieeexplore.ieee.org/abstract/document/11315110) |  Aromolo et al. <br/>RTSS 2025  | Chunk-based execution partitions long inference jobs into schedulable units, creating preemption points to improve schedulability. |  ○   |  ♡♢  |    ♣\|✤    |  ⒸⒶ  |  ✫   | **Optimization** |                       -                        |
| [Pantheon: Preemptible Multi-DNN Inference on Mobile Edge GPUs](https://dl.acm.org/doi/abs/10.1145/3643832.3661878) |   Han et al.<br/>MobiSys 2024   | Operator-level control of GPU execution; dynamically schedules segments by runtime urgency under multi-DNN contention. |  ○   |  ♢   |   ♣♦\|♣    |  ⒸⒶ  |  ✫   | **Optimization** |  [Code](https://zenodo.org/records/11094058)   |
| [Miriam: Exploiting Elastic Kernels for Real-time Multi-DNN Inference on Edge GPU](https://dl.acm.org/doi/abs/10.1145/3625687.3625789) |   Zhao et al.<br/>SenSys 2023   | Elastic kernel scheduling; reallocates GPU resources during execution rather than reserving them statically. |  ○   |  ♡♢  |   ♣♠\|♣    |  ⒸⒶ  |  ✪   |  **Heuristic**   |                       -                        |
| [DynaMIX: Resource Optimization for DNN-Based Real-Time Applications on a Multi-Tasking System](https://arxiv.org/abs/2302.01568) |    Cho et al.<br/>arXiv 2023    | Runtime precision adaptation; trades computational cost for latency to avoid underutilization. |  ○   |  ♡   |   ♣♦\|♣▲   |  ⒸⓂ  |  ✫   | **Optimization** |                       -                        |
| [DREAM: A Dynamic Scheduler for Dynamic Real-time Multi-model ML Workloads ](https://dl.acm.org/doi/abs/10.1145/3623278.3624753) |   Kim et al.<br/>ASPLOS 2023    | Workload admission control + adaptive model selection; compute-aware model switching to cut deadline violations under overload. |  ●   |  ♧   |   ♣♥\|♣♥   | ⒸⒶⒻ  |  ✦   |  **Heuristic**   |                       -                        |
| [ODMDEF: On-Device Multi-DNN Execution Framework Utilizing Adaptive Layer-Allocation on General Purpose Cores and Accelerators](https://ieeexplore.ieee.org/abstract/document/9453793) | Lim et al.<br/>IEEE Access 2021 | Layer-level CPU–GPU co-execution guided by latency prediction. |  ○   | ♡♢♧  |     ♣      | ⒸⒶⓂ  |  ✫   |   **Learning**   |                       -                        |
| [AxoNN: energy-aware execution of neural network inference on multi-accelerator heterogeneous SoCs](https://dl.acm.org/doi/abs/10.1145/3489517.3530572) |    Dagli et al.<br/>DAC 2022    | Energy-aware mapping of compute across heterogeneous accelerators. |  ○   | ♡♢♧  |    ♣\|♥    | ⒸⒶⒻ  |  ✫   | **Optimization** |                       -                        |
| [Shared Memory-contention-aware Concurrent DNN Execution for Diversely Heterogeneous System-on-Chips](https://dl.acm.org/doi/abs/10.1145/3627535.3638502) |   Dagli et al.<br/>PPoPP 2024   | Incorporates shared-memory contention into the mapping process; couples compute orchestration with system-level resource interactions. |  ○   | ♡♢♧  |    ♣♠▲     | ⒸⒶⓂ  |  ✫   | **Optimization** | [code](https://github.com/ismetdagli/HaX-CoNN) |
| [GCAPS: GPU Context-Aware Preemptive Priority-based Scheduling for Real-Time Tasks](https://arxiv.org/abs/2406.05221) |   Wang et al.<br/>arXiv 2024    | Driver-level preemptive GPU scheduling; improves schedulability for latency-sensitive inference. |  ○   |  ♡♢  |   ♣\|♣✤    |  ⒸⒶ  |  ✪   | **Optimization** |                       -                        |
| [DARIS: An Oversubscribed Spatio-Temporal Scheduler for Real-Time DNN Inference on GPUs](https://ieeexplore.ieee.org/abstract/document/11132423) |   Babaei et al.<br/>DAC 2025    | Spatio-temporal, deadline-aware scheduling to raise throughput while keeping real-time guarantees. |  ❖   |  ♡♢  |    ♣\|▲    |  ⒸⓂ  |  ✦   |   **Learning**   |                       -                        |
| [DNN-SAM: Split-and-Merge DNN Execution for Real-Time Object Detection ](https://ieeexplore.ieee.org/abstract/document/9804671) |    Kang et al.<br/>RTAS 2022    | Mandatory-first execution to guarantee timely completion of safety-critical inference. |  ○   |  ♢   |   ♣♦\|♣♦   |  Ⓒ   |  ✦   | **Optimization** |                       -                        |
| [CF-DETR: Coarse-to-Fine Transformer for Real-Time Object Detection ](https://ieeexplore.ieee.org/abstract/document/11315056) |    Shin et al.<br/>RTSS 2025    | Coarse-to-fine execution; adjusts computational effort to available runtime slack. |  ●   |  ♢♧  |   ♣♦\|♣♦   |  ⒸⒶ  |  ✦   | **Optimization** |                       -                        |



### 2.2 Memory Resource Orchestration

| Title                                                        |     Author / Year / Venue      | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                         Code                         |
| :----------------------------------------------------------- | :----------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :--------------------------------------------------: |
| [MCUNet: Tiny Deep Learning on IoT Devices](https://proceedings.neurips.cc/paper/2020/hash/86c51678350f656dcc7f490a43946ee5-Abstract.html) |  Lin et al.<br/>NeurIPS 2020   | TinyNAS + TinyEngine co-design with globally planned buffers; cuts peak SRAM for MCU-scale inference. |  ○   |  ♡   |   ♣♦\|▲    |  ⒸⓂ  |  ✦   |   **Learning**   |    [Code](https://github.com/mit-han-lab/mcunet)     |
| [Memory-efficient Patch-based Inference for Tiny Deep Learning](https://proceedings.neurips.cc/paper_files/paper/2021/hash/1371bccec2447b5aa6d96d2a540fb401-Abstract.html) |  Lin et al.<br/>NeurIPS 2021   | Patch-by-patch inference; processes small spatial regions to reduce peak activation memory. |  ○   |  ♡   |   ♣♦\|▲    |  ⒸⓂ  |  ✫   |   **Learning**   |                          -                           |
| [StreamNet: Memory-Efficient Streaming Tiny Deep Learning Inference on the Microcontroller](https://proceedings.neurips.cc/paper_files/paper/2023/hash/7526508f11bbe0a123af62b9dab1fbe1-Abstract-Conference.html) | Zheng et al.<br/>NeurIPS 2023  | Stream buffers / streaming execution; execution-aware memory reuse without redundant patch computation. |  ○   |  ♡   |   ♣▲\|▲    |  ⒸⓂ  |  ✫   |  **Heuristic**   |                          -                           |
| [FlexNN: Efficient and Adaptive DNN Inference on Memory-Constrained Edge Devices ](https://dl.acm.org/doi/abs/10.1145/3636534.3649391) |   Li et al.<br/>MobiCom 2024   | Jointly coordinates tensor partitioning, loading, and execution; reshapes execution to available memory. |  ○   |  ♡   |   ♣▲\|▲    |  ⒸⓂ  |  ✫   |  **Heuristic**   |       [Code](https://github.com/xxxxyu/FlexNN)       |
| [vMCU: Coordinated Memory Management and Kernel Optimization for DNN Inference on MCUs](https://proceedings.mlsys.org/paper_files/paper/2024/hash/d5a655b8b373737b4f2aea8f78e5e754-Abstract-Conference.html) |  Zheng et al.<br/>MLSys 2024   | Virtualizes on-chip memory and couples kernel execution with allocation for fine-grained SRAM use. |  ○   |  ♡   |   ▲♥\|▲    |  ⒸⓂ  |  ✪   |  **Heuristic**   |                          -                           |
| [FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness](https://proceedings.neurips.cc/paper_files/paper/2022/hash/67d57c32e20fd0a7a302cb81d36e40d5-Abstract-Conference.html?utm_source=chatgpt.com) |  Dao et al.<br/>NeurIPS 2022   | IO-aware tiling reduces HBM↔SRAM traffic; memory locality as important as capacity. |  ○   |  ♡   |   ♣▲\|▲    |  ⒸⓂ  |  ✪   |  **Heuristic**   | [Code](https://github.com/Dao-AILab/flash-attention) |
| [HeteGen: Efficient Heterogeneous Parallel Inference for Large Language Models on Resource-Constrained Devices](https://proceedings.mlsys.org/paper_files/paper/2024/hash/5431dca75a8d2abc1fb51e89e8324f10-Abstract-Conference.html) | Zhao et al. [49]<br/>MLSys 202 | Heterogeneous memory coordination across CPUs/GPUs; memory-aware workload placement. |  ❖   |  ♡♢  |    ♣\|▲    | ⒸⒶⓂ  |  ✫   | **Optimization** |                          -                           |
| [Peak-memory-aware partitioning and scheduling for multi-tenant DNN model inference - ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S1383762126000147) |    2Lee et al.<br/>JSA 2026    | Peak-memory-aware partitioning/scheduling for multi-tenant inference; Tailor-and-Stitch + shared tensor pool + yield-based scheduling. |  ○   |  ♢   |    ♣\|▲    |  ⒸⓂ  |  ✫   |  **Heuristic**   |                          -                           |
| [AWTO: A latency-optimized task offloading scheme for LLM-driven agentic workflows on heterogeneous edge - ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0167739X2600049X) |    Yu et al.<br/>FGCS 2026     | Migration of compact LMs/VLMs/multimodal assistants to edge; need to manage persistent runtime/context memory. |  ❖   |  ♡♢  |    ♣\|▲    |  ⒸⓂ  |  ✦   |   **Learning**   |                          -                           |



### 2.3 Accelerator Resource Orchestration

| Title                                                        |        Author / Year / Venue        | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                            Code                            |
| :----------------------------------------------------------- | :---------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :--------------------------------------------------------: |
| [CoDL: Efficient CPU-GPU Co-execution for Deep Learning Inference on Mobile Devices](https://www.microsoft.com/en-us/research/wp-content/uploads/2022/05/mobisys22-CoDL__Efficient_CPU_GPU_Co_execution_for_DL_Model_Inference_on_Mobile_Devices-4.pdf) |     Jia et al.<br/>MobiSys 2022     | Operator-level CPU–GPU co-execution within a single model via concurrency-aware latency prediction and data-sharing control. |  ○   |  ♡♢  |     ♣♥     | ⒸⒶⓂ  |  ✫   |  **Heuristic**   |          [Code](https://github.com/csu-eis/CoDL)           |
| [Band: coordinated multi-DNN inference on heterogeneous mobile processorss](https://dl.acm.org/doi/abs/10.1145/3498361.3538948) |    Jeong et al.<br>MobiSys 2022     | Coordinated multi-DNN inference across CPU/GPU/DSP/NPU via dependency- and support-aware subgraph scheduling. |  ●   | ♡♢♧  |   ♣♠\|♣    |  ⒸⒶ  |  ✫   |  **Heuristic**   |           [Code](https://github.com/mrsnu/band)            |
| [BlastNet: Exploiting Duo-Blocks for Cross-Processor Real-Time DNN Inference](https://dl.acm.org/doi/abs/10.1145/3560905.3568520) |     Ling et al.<br/>SenSys 2022     | Duo-blocks couple CPU/GPU execution into analyzable block pairs for real-time cross-processor inference. |  ○   |  ♡♢  |   ♣\|♣♦    |  ⒸⒶ  |  ✫   |  **Heuristic**   |                             -                              |
| [NN-Stretch: Automatic Neural Network Branching for Parallel Inference on Heterogeneous Multi-Processors ](https://dl.acm.org/doi/abs/10.1145/3581791.3596870) |     Wei et al.<br>MobiSys 2023      | Reshapes a sequential model into a multi-branch model; structure-aware spatial scheduling on CPU/GPU/DSP. |  ○   | ♡♢♧  |   ♣♥\|♣♦   |  ⒸⒶ  |  ✫   | **Optimization** | [Code](https://github.com/caoting-dotcom/multiBranchModel) |
| [CARIn: Constraint-Aware and Responsive Inference on Heterogeneous Devices for Single- and Multi-DNN Workloads](https://dl.acm.org/doi/full/10.1145/3665868) | Panopoulos et al.<br/>ACM TECS 2024 | SLO-driven, constraint-aware configuration selection over model–processor pairs with runtime switching. |  ○   | ♡♢♧  |  ♣♠♦\|♣▲   |  ⒸⒶ  |  ✦   | **Optimization** |                             -                              |
| [Klotski: DNN Model Orchestration Framework for Dataflow Architecture Accelerators](https://ieeexplore.ieee.org/abstract/document/10323893) |      Bai et al.<br/>ICCAD 2023      | Micro-operation orchestration for dataflow accelerators; decouples scheduling from mapping to cut makespan and NoC overhead. |  ○   |  ♧   |     ♣      |  ⒸⒶ  |  ✪   | **Optimization** |                             -                              |
| [XSched: Preemptive Scheduling for Diverse XPUs](https://www.usenix.org/conference/osdi25/presentation/shen-weihang) |      Shen et al.<br/>OSDI 2025      | XQueue preemptible command-queue abstraction; hardware-agnostic priority/bandwidth policies across GPUs/NPUs/ASICs/FPGAs. |  ○   |  ♢♧  |   ♣♠\|♣    |  ⒸⒶ  |  ✦   |  **Heuristic**   |          [Code](https://github.com/XpuOS/xsched)           |
| [Fast On-device LLM Inference with NPUs](https://dl.acm.org/doi/abs/10.1145/3669940.3707239) |      Xu et al.<br>ASPLOS 2025       | On-device LLM: NPU offloading for the prefill stage while keeping accuracy-sensitive ops on CPU/GPU. |  ❖   | ♡♢♧  |   ♣♥\|▲    | ⒸⒶⓂ  |  ✫   |  **Heuristic**   |       [Code](https://github.com/QingweiJi/PowerNPU)        |
| [ARIA: Optimizing Vision Foundation Model Inference on Heterogeneous Mobile Processors for Augmented Reality](https://lovelyzzkei.github.io/assets/pdf/mobisys25-aria.pdf) |     Jung et al.<br>MobiSys 2025     | Vision foundation models in AR: GPU high-fidelity prediction + NPU low-latency updates. |  ○   | ♡♢♧  |    ♦\|♣    |  ⒸⒶ  |  ✦   |  **Heuristic**   |                             -                              |



### 2.4 Processor Frequency and Energy Orchestration

| Title                                                        |         Author / Year / Venue         | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                     Code                     |
| :----------------------------------------------------------- | :-----------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :------------------------------------------: |
| [zTT: Learning-Based DVFS with Zero Thermal Throttling for Mobile Devices](https://dl.acm.org/doi/abs/10.1145/3529706.3529714) |     Kim et al.<br/>GetMobile 2022     | Learning-based, thermal-aware DVFS that avoids throttling for stable long-term performance. |  ○   |  ♡♢  |   ♣♥\|♣    |  Ⓕ   |  ✦   |   **Learning**   |    [Code](https://github.com/ztt-21/zTT)     |
| [Coordinated Batching and DVFS for DNN Inference on GPU Accelerators ](https://ieeexplore.ieee.org/abstract/document/9689937) | Nabavinejad et al.<br/>IEEE TPDS 2022 | Jointly regulates batch size and GPU frequency for energy-efficient inference. |  ○   |  ♢   |     ♠      |  ⒸⒻ  |  ★   |  **Heuristic**   |                      -                       |
| [A Workload-Aware DVFS Robust to Concurrent Tasks for Mobile Devices](https://dl.acm.org/doi/abs/10.1145/3570361.3592524) |      Lin et al.<br/>MobiCom 2023      | Incorporates workload characteristics into frequency selection for stable latency/energy under concurrent tasks. |  ●   |  ♡♢  |     ♣♥     |  Ⓕ   |  ✦   |   **Learning**   | [Code](https://github.com/geardvfs/GearDVFS) |
| [MOC: Multi-Objective Mobile CPU-GPU Co-Optimization for Power-Efficient DNN Inference](https://ieeexplore.ieee.org/abstract/document/10323882) |        Wu et al.<br>ICCAD 2023        | Multi-objective CPU–GPU frequency management over latency, energy, and accuracy. |  ○   |  ♡♢  |     ♣♥     |  Ⓕ   |  ✦   |   **Learning**   |                      -                       |
| [Thermal-Aware Scheduling for Deep Learning on Mobile Devices With NPU ](https://ieeexplore.ieee.org/abstract/document/10478860) |     Tan et al.<br/>IEEE TMC 2024      | Adds thermal state into scheduling to curb thermal accumulation from persistent DNN execution. |  ○   |  ♢♧  |   ♣♦\|♥    |  ⒶⒻ  |  ✦   |   **Learning**   |                      -                       |
| [MapFormer: Attention-based multi-DNN manager for throughout & power co-optimization on embedded devices ](https://dl.acm.org/doi/abs/10.1145/3676536.3676724) |    Karatzas et al.<br/>ICCAD 2024     | Transformer-based predictor + search to partition concurrent DNNs and jointly optimize allocation and power. |  ○   |  ♢♧  |    ♠\|♥    | ⒸⒶⒻ  |  ✫   |   **Learning**   |                      -                       |
| [Tango: Low Latency Multi-DNN Inference on Heterogeneous Edge Platforms](https://ieeexplore.ieee.org/abstract/document/10817997) |     Taufique et al.<br/>ICCD 2024     | PPO-based runtime agent jointly optimizing cluster selection, accuracy config, and DVFS. |  ○   |  ♡♢  |   ♣♥\|♣    | ⒸⒶⒻ  |  ✦   |   **Learning**   |                      -                       |
| [NeuroBalancer: Balancing System Frequencies With Punctual Laziness for Timely and Energy-Efficient DNN Inferences](https://ieeexplore.ieee.org/abstract/document/10819653) |     Bin et al.<br/>IEEE TMC 2025      | System-wide frequency coordination ("punctual laziness") across CPU/GPU/memory for energy efficiency. |  ○   |  ♡♢  |   ♣♥\|♣    |  Ⓕ   |  ✪   | **Optimization** |                      -                       |
| [E4: Energy-Efficient DNN Inference for Edge Video Analytics via Early Exiting and DVFS ](https://ojs.aaai.org/index.php/AAAI/article/view/32104) |      Zhang et al.<br/>AAAI 2025       | Edge video analytics: early-exit + DVFS to scale compute with input difficulty. |  ●   |  ♡♢  |   ♣♥\|♦    |  Ⓕ   |  ✫   |   **Learning**   |                      -                       |
| [Twill: Scheduling Compound AI Systems on Heterogeneous Mobile Edge Platforms ](https://ieeexplore.ieee.org/abstract/document/11240767) |    Taufique et al.<br/>ICCAD 2025     | Compound AI (CNN/Transformer/LLM): affinity-aware mapping, priority-driven freezing/unfreezing, adaptive DVFS. |  ◎   | ♡♢♧  |    ♣\|♥    | ⒸⒶⒻ  |  ✫   |  **Heuristic**   |                      -                       |



## 3. Runtime Decision-Making Strategies

### 3.1 Rule-Based and Priority-Driven Scheduling

| Title                                                        |      Author / Year / Venue      | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                         Code                          |
| :----------------------------------------------------------- | :-----------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :---------------------------------------------------: |
| [LaLaRAND: Flexible Layer-by-Layer CPU/GPU Scheduling for Real-Time DNN Tasks ](https://ieeexplore.ieee.org/abstract/document/9622325) |    Kang et al.<br/>RTSS 2021    | Layer-by-layer CPU/GPU scheduling; fine-grained priority assignment without sacrificing real-time guarantees. |  ○   |  ♡♢  |    ♣\|✤    |  ⒸⒶ  |  ✫   | **Optimization** |    [Code](https://github.com/fredrickang/LaLaRAND)    |
| [Time-Predictable Acceleration of Deep Neural Networks on FPGA SoC Platforms ](https://ieeexplore.ieee.org/abstract/document/9622368) | Restuccia et al.<br/>RTSS 2021  | Time-predictable DNN acceleration on FPGA-SoC to reduce execution variability for schedulability analysis. |  ○   |  ♡♧  |    ♣\|✤    |  Ⓐ   |  ✫   | **Optimization** |                           -                           |
| [Demand Layering for Real-Time DNN Inference with Minimized Memory Usage](https://ieeexplore.ieee.org/abstract/document/9984745) |     Ji et al.<br/>RTSS 2022     | Layer-activation strategy minimizing memory footprint while preserving schedulability. |  ○   |  ♡♢  |   ♣▲\|▲    |  Ⓜ   |  ✫   |  **Heuristic**   |  [Code](https://github.com/aveeslab/demand-layering)  |
| [RT-LM: Uncertainty-Aware Resource Management for Real-Time Inference of Language Models](https://arxiv.org/abs/2309.06619) |     Li et al.<br/>RTSS 2023     | Uncertainty-aware resource management for LMs; estimates execution variability and provisions for worst case. |  ❖   |  ♢   |     ♣      |  Ⓒ   |  ✦   |  **Heuristic**   |                           -                           |
| [Partitioned Scheduling and Parallelism Assignment for Real-Time DNN Inference Tasks on Multi-TPU ](https://dl.acm.org/doi/abs/10.1145/3649329.3655979) |     Sun et al.<br/>DAC 2024     | Jointly determines task partitions and accelerator-level parallelism under strict deadlines. |  ◎   |  ♧   |    ♣\|✤    |  Ⓒ   |  ✫   | **Optimization** |                           -                           |
| [RT-Swap: Addressing GPU Memory Bottlenecks for Real-Time Multi-DNN Inference](https://www.computer.org/csdl/proceedings-article/rtas/2024/584100a373/1Y5EZtFnZ2o) |    Kang et al.<br/>RTAS 2024    | Priority-driven GPU memory swapping of inactive model states while preserving real-time guarantees. |  ○   |  ♢   |   ♣\|▲✤    |  Ⓜ   |  ✫   |  **Heuristic**   | [Code](https://github.com/fredrickang/Public-RT-Swap) |
| [SCENIC: Capability and Scheduling Co-Design for Intelligent Controller on Heterogeneous Platforms](https://ieeexplore.ieee.org/abstract/document/10844810) |    Chen et al.<br/>RTSS 2024    | Jointly optimizes resource allocation and controller-capability configuration on heterogeneous platforms. |  ○   |  ♡♢  |    ♦\|✤    |  Ⓒ   |  ✦   | **Optimization** |                           -                           |
| [FLASH: Deadline-Aware Flexible LLC Arbitration and Scheduling for Hardware Accelerators](https://dl.acm.org/doi/full/10.1145/3757742) | Agarwal et al.<br>ACM TECS 2025 | Deadline-aware last-level-cache arbitration among accelerators based on timing urgency. |  ○   | ♡♢♧  |   ♣♠\|♣    |  Ⓜ   |  ✦   |  **Heuristic**   |                           -                           |



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



