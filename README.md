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
  - [3.4 Profile-Guided and Learning-Driven Scheduling](#34-Profile-Guided-and-Learning-Driven-Scheduling)
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
| [Real-Time Multitasking of Deep Neural Networks With Nvidia Tensorrt ](https://ieeexplore.ieee.org/abstract/document/11315110) |  Aromolo et al. <br/>RTSS 2025  | Chunk-based execution partitions long inference jobs into schedulable units, creating preemption points to improve schedulability. |  ○   |  ♡♢  |    ♣\|✤    |  ⒸⒶ  |  ✫   |  **Heuristic**   |                       -                        |
| [Pantheon: Preemptible Multi-DNN Inference on Mobile Edge GPUs](https://dl.acm.org/doi/abs/10.1145/3643832.3661878) |   Han et al.<br/>MobiSys 2024   | Operator-level control of GPU execution; dynamically schedules segments by runtime urgency under multi-DNN contention. |  ○   |  ♢   |   ♣♦\|♣    |  ⒸⒶ  |  ✫   |  **Heuristic**   |  [Code](https://zenodo.org/records/11094058)   |
| [Miriam: Exploiting Elastic Kernels for Real-time Multi-DNN Inference on Edge GPU](https://dl.acm.org/doi/abs/10.1145/3625687.3625789) |   Zhao et al.<br/>SenSys 2023   | Elastic kernel scheduling; reallocates GPU resources during execution rather than reserving them statically. |  ○   |  ♡♢  |   ♣♠\|♣    |  ⒸⒶ  |  ✪   |  **Heuristic**   |                       -                        |
| [DynaMIX: Resource Optimization for DNN-Based Real-Time Applications on a Multi-Tasking System](https://arxiv.org/abs/2302.01568) |    Cho et al.<br/>arXiv 2023    | Runtime precision adaptation; trades computational cost for latency to avoid underutilization. |  ○   |  ♡   |   ♣♦\|♣▲   |  ⒸⓂ  |  ✫   | **Optimization** |                       -                        |
| [DREAM: A Dynamic Scheduler for Dynamic Real-time Multi-model ML Workloads ](https://dl.acm.org/doi/abs/10.1145/3623278.3624753) |   Kim et al.<br/>ASPLOS 2023    | Workload admission control + adaptive model selection; compute-aware model switching to cut deadline violations under overload. |  ●   |  ♧   |   ♣♥\|♣♥   | ⒸⒶⒻ  |  ✦   |  **Heuristic**   |                       -                        |
| [ODMDEF: On-Device Multi-DNN Execution Framework Utilizing Adaptive Layer-Allocation on General Purpose Cores and Accelerators](https://ieeexplore.ieee.org/abstract/document/9453793) | Lim et al.<br/>IEEE Access 2021 | Layer-level CPU–GPU co-execution guided by latency prediction. |  ○   | ♡♢♧  |     ♣      | ⒸⒶⓂ  |  ✫   |   **Learning**   |                       -                        |
| [AxoNN: energy-aware execution of neural network inference on multi-accelerator heterogeneous SoCs](https://dl.acm.org/doi/abs/10.1145/3489517.3530572) |    Dagli et al.<br/>DAC 2022    | Energy-aware mapping of compute across heterogeneous accelerators. |  ○   | ♡♢♧  |    ♣\|♥    | ⒸⒶⒻ  |  ✫   | **Optimization** |                       -                        |
| [Shared Memory-contention-aware Concurrent DNN Execution for Diversely Heterogeneous System-on-Chips](https://dl.acm.org/doi/abs/10.1145/3627535.3638502) |   Dagli et al.<br/>PPoPP 2024   | Incorporates shared-memory contention into the mapping process; couples compute orchestration with system-level resource interactions. |  ○   | ♡♢♧  |    ♣♠▲     | ⒸⒶⓂ  |  ✫   | **Optimization** | [code](https://github.com/ismetdagli/HaX-CoNN) |
| [GCAPS: GPU Context-Aware Preemptive Priority-based Scheduling for Real-Time Tasks](https://arxiv.org/abs/2406.05221) |   Wang et al.<br/>arXiv 2024    | Driver-level preemptive GPU scheduling; improves schedulability for latency-sensitive inference. |  ○   |  ♡♢  |   ♣\|♣✤    |  ⒸⒶ  |  ✪   |  **Heuristic**   |                       -                        |
| [DARIS: An Oversubscribed Spatio-Temporal Scheduler for Real-Time DNN Inference on GPUs](https://ieeexplore.ieee.org/abstract/document/11132423) |   Babaei et al.<br/>DAC 2025    | Spatio-temporal, deadline-aware scheduling to raise throughput while keeping real-time guarantees. |  ❖   |  ♡♢  |    ♣\|▲    |  ⒸⓂ  |  ✦   |  **Heuristic**   |                       -                        |
| [DNN-SAM: Split-and-Merge DNN Execution for Real-Time Object Detection ](https://ieeexplore.ieee.org/abstract/document/9804671) |    Kang et al.<br/>RTAS 2022    | Mandatory-first execution to guarantee timely completion of safety-critical inference. |  ○   |  ♢   |   ♣♦\|♣♦   |  Ⓒ   |  ✦   |  **Heuristic**   |                       -                        |
| [CF-DETR: Coarse-to-Fine Transformer for Real-Time Object Detection ](https://ieeexplore.ieee.org/abstract/document/11315056) |    Shin et al.<br/>RTSS 2025    | Coarse-to-fine execution; adjusts computational effort to available runtime slack. |  ●   |  ♢♧  |   ♣♦\|♣♦   |  ⒸⒶ  |  ✦   |  **Heuristic**   |                       -                        |



### 2.2 Memory Resource Orchestration

| Title                                                        |     Author / Year / Venue      | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |             Policy              |                         Code                          |
| :----------------------------------------------------------- | :----------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :-----------------------------: | :---------------------------------------------------: |
| [MCUNet: Tiny Deep Learning on IoT Devices](https://proceedings.neurips.cc/paper/2020/hash/86c51678350f656dcc7f490a43946ee5-Abstract.html) |  Lin et al.<br/>NeurIPS 2020   | TinyNAS + TinyEngine co-design with globally planned buffers; cuts peak SRAM for MCU-scale inference. |  ○   |  ♡   |   ♣♦\|▲    |  ⒸⓂ  |  ✦   |        **Optimization**         |     [Code](https://github.com/mit-han-lab/mcunet)     |
| [Memory-efficient Patch-based Inference for Tiny Deep Learning](https://proceedings.neurips.cc/paper_files/paper/2021/hash/1371bccec2447b5aa6d96d2a540fb401-Abstract.html) |  Lin et al.<br/>NeurIPS 2021   | Patch-by-patch inference; processes small spatial regions to reduce peak activation memory. |  ○   |  ♡   |   ♣♦\|▲    |  ⒸⓂ  |  ✫   |        **Optimization**         |                           -                           |
| [StreamNet: Memory-Efficient Streaming Tiny Deep Learning Inference on the Microcontroller](https://proceedings.neurips.cc/paper_files/paper/2023/hash/7526508f11bbe0a123af62b9dab1fbe1-Abstract-Conference.html) | Zheng et al.<br/>NeurIPS 2023  | Stream buffers / streaming execution; execution-aware memory reuse without redundant patch computation. |  ○   |  ♡   |   ♣▲\|▲    |  ⒸⓂ  |  ✫   |        **Optimization**         |                           -                           |
| [FlexNN: Efficient and Adaptive DNN Inference on Memory-Constrained Edge Devices ](https://dl.acm.org/doi/abs/10.1145/3636534.3649391) |   Li et al.<br/>MobiCom 2024   | Jointly coordinates tensor partitioning, loading, and execution; reshapes execution to available memory. |  ○   |  ♡   |   ♣▲\|▲    |  ⒸⓂ  |  ✫   |        **Optimization**         |       [Code](https://github.com/xxxxyu/FlexNN)        |
| [vMCU: Coordinated Memory Management and Kernel Optimization for DNN Inference on MCUs](https://proceedings.mlsys.org/paper_files/paper/2024/hash/d5a655b8b373737b4f2aea8f78e5e754-Abstract-Conference.html) |  Zheng et al.<br/>MLSys 2024   | Virtualizes on-chip memory and couples kernel execution with allocation for fine-grained SRAM use. |  ○   |  ♡   |   ▲♥\|▲    |  ⒸⓂ  |  ✪   | **Heuristic<br />Optimization** |                           -                           |
| [FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness](https://proceedings.neurips.cc/paper_files/paper/2022/hash/67d57c32e20fd0a7a302cb81d36e40d5-Abstract-Conference.html?utm_source=chatgpt.com) |  Dao et al.<br/>NeurIPS 2022   | IO-aware tiling reduces HBM↔SRAM traffic; memory locality as important as capacity. |  ○   |  ♡   |   ♣▲\|▲    |  ⒸⓂ  |  ✪   |        **Optimization**         | [Code](https://github.com/Dao-AILab/flash-attention)  |
| [HeteGen: Efficient Heterogeneous Parallel Inference for Large Language Models on Resource-Constrained Devices](https://proceedings.mlsys.org/paper_files/paper/2024/hash/5431dca75a8d2abc1fb51e89e8324f10-Abstract-Conference.html) | Zhao et al. [49]<br/>MLSys 202 | Heterogeneous memory coordination across CPUs/GPUs; memory-aware workload placement. |  ❖   |  ♡♢  |    ♣\|▲    | ⒸⒶⓂ  |  ✫   |        **Optimization**         |                           -                           |
| [Peak-memory-aware partitioning and scheduling for multi-tenant DNN model inference - ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S1383762126000147) |    2Lee et al.<br/>JSA 2026    | Peak-memory-aware partitioning/scheduling for multi-tenant inference; Tailor-and-Stitch + shared tensor pool + yield-based scheduling. |  ○   |  ♢   |    ♣\|▲    |  ⒸⓂ  |  ✫   |          **Heuristic**          |                           -                           |
| [AWTO: A latency-optimized task offloading scheme for LLM-driven agentic workflows on heterogeneous edge - ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0167739X2600049X) |    Yu et al.<br/>FGCS 2026     | Migration of compact LMs/VLMs/multimodal assistants to edge; need to manage persistent runtime/context memory. |  ❖   |  ♡♢  |    ♣\|▲    |  ⒸⓂ  |  ✦   |          **Learning**           |                           -                           |
| [Demand Layering for Real-Time DNN Inference with Minimized Memory Usage](https://ieeexplore.ieee.org/abstract/document/9984745) |    Ji et al.<br/>RTSS 2022     | Layer-activation strategy minimizing memory footprint while preserving schedulability. |  ○   |  ♡♢  |   ♣▲\|▲    |  Ⓜ   |  ✫   |        **Optimization**         |  [Code](https://github.com/aveeslab/demand-layering)  |
| [RT-Swap: Addressing GPU Memory Bottlenecks for Real-Time Multi-DNN Inference](https://www.computer.org/csdl/proceedings-article/rtas/2024/584100a373/1Y5EZtFnZ2o) |   Kang et al.<br/>RTAS 2024    | Priority-driven GPU memory swapping of inactive model states while preserving real-time guarantees. |  ○   |  ♢   |   ♣\|▲✤    |  Ⓜ   |  ✫   |        **Optimization**         | [Code](https://github.com/fredrickang/Public-RT-Swap) |



### 2.3 Accelerator Resource Orchestration

| Title                                                        |        Author / Year / Venue        | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |       Policy        |                            Code                            |
| :----------------------------------------------------------- | :---------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :-----------------: | :--------------------------------------------------------: |
| [CoDL: Efficient CPU-GPU Co-execution for Deep Learning Inference on Mobile Devices](https://www.microsoft.com/en-us/research/wp-content/uploads/2022/05/mobisys22-CoDL__Efficient_CPU_GPU_Co_execution_for_DL_Model_Inference_on_Mobile_Devices-4.pdf) |     Jia et al.<br/>MobiSys 2022     | Operator-level CPU–GPU co-execution within a single model via concurrency-aware latency prediction and data-sharing control. |  ○   |  ♡♢  |     ♣♥     | ⒸⒶⓂ  |  ✫   |    **Heuristic**    |          [Code](https://github.com/csu-eis/CoDL)           |
| [Band: coordinated multi-DNN inference on heterogeneous mobile processorss](https://dl.acm.org/doi/abs/10.1145/3498361.3538948) |    Jeong et al.<br>MobiSys 2022     | Coordinated multi-DNN inference across CPU/GPU/DSP/NPU via dependency- and support-aware subgraph scheduling. |  ●   | ♡♢♧  |   ♣♠\|♣    |  ⒸⒶ  |  ✫   |    **Heuristic**    |           [Code](https://github.com/mrsnu/band)            |
| [BlastNet: Exploiting Duo-Blocks for Cross-Processor Real-Time DNN Inference](https://dl.acm.org/doi/abs/10.1145/3560905.3568520) |     Ling et al.<br/>SenSys 2022     | Duo-blocks couple CPU/GPU execution into analyzable block pairs for real-time cross-processor inference. |  ○   |  ♡♢  |   ♣\|♣♦    |  ⒸⒶ  |  ✫   |    **Heuristic**    |                             -                              |
| [NN-Stretch: Automatic Neural Network Branching for Parallel Inference on Heterogeneous Multi-Processors ](https://dl.acm.org/doi/abs/10.1145/3581791.3596870) |     Wei et al.<br>MobiSys 2023      | Reshapes a sequential model into a multi-branch model; structure-aware spatial scheduling on CPU/GPU/DSP. |  ○   | ♡♢♧  |   ♣♥\|♣♦   |  ⒸⒶ  |  ✫   |  **Optimization**   | [Code](https://github.com/caoting-dotcom/multiBranchModel) |
| [CARIn: Constraint-Aware and Responsive Inference on Heterogeneous Devices for Single- and Multi-DNN Workloads](https://dl.acm.org/doi/full/10.1145/3665868) | Panopoulos et al.<br/>ACM TECS 2024 | SLO-driven, constraint-aware configuration selection over model–processor pairs with runtime switching. |  ○   | ♡♢♧  |  ♣♠♦\|♣▲   |  ⒸⒶ  |  ✦   |    **Heuristic**    |                             -                              |
| [Klotski: DNN Model Orchestration Framework for Dataflow Architecture Accelerators](https://ieeexplore.ieee.org/abstract/document/10323893) |      Bai et al.<br/>ICCAD 2023      | Micro-operation orchestration for dataflow accelerators; decouples scheduling from mapping to cut makespan and NoC overhead. |  ○   |  ♧   |     ♣      |  ⒸⒶ  |  ✪   |  **Optimization**   |                             -                              |
| [XSched: Preemptive Scheduling for Diverse XPUs](https://www.usenix.org/conference/osdi25/presentation/shen-weihang) |      Shen et al.<br/>OSDI 2025      | XQueue preemptible command-queue abstraction; hardware-agnostic priority/bandwidth policies across GPUs/NPUs/ASICs/FPGAs. |  ○   |  ♢♧  |   ♣♠\|♣    |  ⒸⒶ  |  ✦   |    **Heuristic**    |          [Code](https://github.com/XpuOS/xsched)           |
| [Fast On-device LLM Inference with NPUs](https://dl.acm.org/doi/abs/10.1145/3669940.3707239) |      Xu et al.<br>ASPLOS 2025       | On-device LLM: NPU offloading for the prefill stage while keeping accuracy-sensitive ops on CPU/GPU. |  ❖   | ♡♢♧  |   ♣♥\|▲    | ⒸⒶⓂ  |  ✫   |    **Heuristic**    |       [Code](https://github.com/QingweiJi/PowerNPU)        |
| [ARIA: Optimizing Vision Foundation Model Inference on Heterogeneous Mobile Processors for Augmented Reality](https://lovelyzzkei.github.io/assets/pdf/mobisys25-aria.pdf) |     Jung et al.<br>MobiSys 2025     | Vision foundation models in AR: GPU high-fidelity prediction + NPU low-latency updates. |  ○   | ♡♢♧  |    ♦\|♣    |  ⒸⒶ  |  ✦   |    **Heuristic**    |                             -                              |
| [Time-Predictable Acceleration of Deep Neural Networks on FPGA SoC Platforms ](https://ieeexplore.ieee.org/abstract/document/9622368) |   Restuccia et al.<br/>RTSS 2021    | Time-predictable DNN acceleration on FPGA-SoC to reduce execution variability for schedulability analysis. |  ○   |  ♡♧  |    ♣\|✤    |  Ⓐ   |  ✫   | **4.3Optimization** |                             -                              |



### 2.4 Processor Frequency and Energy Orchestration

| Title                                                        |         Author / Year / Venue         | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |           Policy            |                     Code                     |
| :----------------------------------------------------------- | :-----------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :-------------------------: | :------------------------------------------: |
| [zTT: Learning-Based DVFS with Zero Thermal Throttling for Mobile Devices](https://dl.acm.org/doi/abs/10.1145/3529706.3529714) |     Kim et al.<br/>GetMobile 2022     | Learning-based, thermal-aware DVFS that avoids throttling for stable long-term performance. |  ○   |  ♡♢  |   ♣♥\|♣    |  Ⓕ   |  ✦   |        **Learning**         |    [Code](https://github.com/ztt-21/zTT)     |
| [Coordinated Batching and DVFS for DNN Inference on GPU Accelerators ](https://ieeexplore.ieee.org/abstract/document/9689937) | Nabavinejad et al.<br/>IEEE TPDS 2022 | Jointly regulates batch size and GPU frequency for energy-efficient inference. |  ○   |  ♢   |     ♠      |  ⒸⒻ  |  ★   |        **Heuristic**        |                      -                       |
| [A Workload-Aware DVFS Robust to Concurrent Tasks for Mobile Devices](https://dl.acm.org/doi/abs/10.1145/3570361.3592524) |      Lin et al.<br/>MobiCom 2023      | Incorporates workload characteristics into frequency selection for stable latency/energy under concurrent tasks. |  ●   |  ♡♢  |     ♣♥     |  Ⓕ   |  ✦   |         **Control**         | [Code](https://github.com/geardvfs/GearDVFS) |
| [MOC: Multi-Objective Mobile CPU-GPU Co-Optimization for Power-Efficient DNN Inference](https://ieeexplore.ieee.org/abstract/document/10323882) |        Wu et al.<br>ICCAD 2023        | Multi-objective CPU–GPU frequency management over latency, energy, and accuracy. |  ○   |  ♡♢  |     ♣♥     |  Ⓕ   |  ✦   |        **Learning**         |                      -                       |
| [Thermal-Aware Scheduling for Deep Learning on Mobile Devices With NPU ](https://ieeexplore.ieee.org/abstract/document/10478860) |     Tan et al.<br/>IEEE TMC 2024      | Adds thermal state into scheduling to curb thermal accumulation from persistent DNN execution. |  ○   |  ♢♧  |   ♣♦\|♥    |  ⒶⒻ  |  ✦   | **Learning<br />Heuristic** |                      -                       |
| [MapFormer: Attention-based multi-DNN manager for throughout & power co-optimization on embedded devices ](https://dl.acm.org/doi/abs/10.1145/3676536.3676724) |    Karatzas et al.<br/>ICCAD 2024     | Transformer-based predictor + search to partition concurrent DNNs and jointly optimize allocation and power. |  ○   |  ♢♧  |    ♠\|♥    | ⒸⒶⒻ  |  ✫   |        **Learning**         |                      -                       |
| [Tango: Low Latency Multi-DNN Inference on Heterogeneous Edge Platforms](https://ieeexplore.ieee.org/abstract/document/10817997) |     Taufique et al.<br/>ICCD 2024     | PPO-based runtime agent jointly optimizing cluster selection, accuracy config, and DVFS. |  ○   |  ♡♢  |   ♣♥\|♣    | ⒸⒶⒻ  |  ✦   |        **Learning**         |                      -                       |
| [NeuroBalancer: Balancing System Frequencies With Punctual Laziness for Timely and Energy-Efficient DNN Inferences](https://ieeexplore.ieee.org/abstract/document/10819653) |     Bin et al.<br/>IEEE TMC 2025      | System-wide frequency coordination ("punctual laziness") across CPU/GPU/memory for energy efficiency. |  ○   |  ♡♢  |   ♣♥\|♣    |  Ⓕ   |  ✪   |      **Optimization**       |                      -                       |
| [E4: Energy-Efficient DNN Inference for Edge Video Analytics via Early Exiting and DVFS ](https://ojs.aaai.org/index.php/AAAI/article/view/32104) |      Zhang et al.<br/>AAAI 2025       | Edge video analytics: early-exit + DVFS to scale compute with input difficulty. |  ●   |  ♡♢  |   ♣♥\|♦    |  Ⓕ   |  ✫   |      **Optimization**       |                      -                       |
| [Twill: Scheduling Compound AI Systems on Heterogeneous Mobile Edge Platforms ](https://ieeexplore.ieee.org/abstract/document/11240767) |    Taufique et al.<br/>ICCAD 2025     | Compound AI (CNN/Transformer/LLM): affinity-aware mapping, priority-driven freezing/unfreezing, adaptive DVFS. |  ◎   | ♡♢♧  |    ♣\|♥    | ⒸⒶⒻ  |  ✫   |        **Heuristic**        |                      -                       |



## 3. Runtime Decision-Making Strategies

### 3.1 Rule-Based and Priority-Driven Scheduling

| Title                                                        |      Author / Year / Venue      | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |    Policy     |                      Code                       |
| :----------------------------------------------------------- | :-----------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :-----------: | :---------------------------------------------: |
| [LaLaRAND: Flexible Layer-by-Layer CPU/GPU Scheduling for Real-Time DNN Tasks ](https://ieeexplore.ieee.org/abstract/document/9622325) |    Kang et al.<br/>RTSS 2021    | Layer-by-layer CPU/GPU scheduling; fine-grained priority assignment without sacrificing real-time guarantees. |  ○   |  ♡♢  |    ♣\|✤    |  ⒸⒶ  |  ✫   | **Heuristic** | [Code](https://github.com/fredrickang/LaLaRAND) |
| [RT-LM: Uncertainty-Aware Resource Management for Real-Time Inference of Language Models](https://arxiv.org/abs/2309.06619) |     Li et al.<br/>RTSS 2023     | Uncertainty-aware resource management for LMs; estimates execution variability and provisions for worst case. |  ❖   |  ♢   |     ♣      |  Ⓒ   |  ✦   | **Heuristic** |                        -                        |
| [Partitioned Scheduling and Parallelism Assignment for Real-Time DNN Inference Tasks on Multi-TPU ](https://dl.acm.org/doi/abs/10.1145/3649329.3655979) |     Sun et al.<br/>DAC 2024     | Jointly determines task partitions and accelerator-level parallelism under strict deadlines. |  ◎   |  ♧   |    ♣\|✤    |  Ⓒ   |  ✫   | **Heuristic** |                        -                        |
| [FLASH: Deadline-Aware Flexible LLC Arbitration and Scheduling for Hardware Accelerators](https://dl.acm.org/doi/full/10.1145/3757742) | Agarwal et al.<br>ACM TECS 2025 | Deadline-aware last-level-cache arbitration among accelerators based on timing urgency. |  ○   | ♡♢♧  |   ♣♠\|♣    |  Ⓜ   |  ✦   | **Heuristic** |                        -                        |
| [SGPRS: Seamless GPU Partitioning Real-Time Scheduler for Periodic Deep Learning Workloads ](https://ieeexplore.ieee.org/abstract/document/10546669) |  Babaei et al.<br/>arXiv 2024   | Formulates GPU partitioning as a schedulability optimization to limit interference among periodic DL tasks. |  ○   |  ♢   |   ♣♠\|✤    |  Ⓒ   |  ✫   | **Heuristic** |                        -                        |
| [TensorRT-Based Framework and Optimization Methodology for Deep Learning Inference on Jetson Boards ](https://dl.acm.org/doi/full/10.1145/3508391) | Jeong et al.<br/>ACM TECS 2022  | Jointly explores precision, engine configuration, and deployment parameters to reduce latency. |  ○   | ♡♢♧  |     ♠      |  ⒸⒶ  |  ✫   | **Heuristic** |     [Code](https://github.com/cap-lab/jedi)     |
| [IceEdge: Thermal-Aware Machine Learning Inference Serving for Emerging Edge Applications](https://dl.acm.org/doi/full/10.1145/3801093) |  Shafi et al.<br>ACM TOSN 2026  | Thermal-aware feedback loop adapting inference-serving policy to temperature. |  ○   | ♡♢♧  |   ♣♠♥\|♣   |  ⒸⒻ  |  ✦   | **Heuristic** |                        -                        |
| [TinyMem: Boosting Multi-DNN Inference on Tiny AI Accelerators with Weight Memory Virtualization](https://dl.acm.org/doi/abs/10.1145/3708468.3711888) |  Jeon et al.<br>HotMobile 2025  | Weight-memory virtualization knowledge to raise multi-DNN concurrency on tiny accelerators. |  ○   |  ♧   |    ♠\|▲    |  Ⓜ   |  ✫   | **Heuristic** |                        -                        |
| [ARISE: High-Capacity AR Offloading Inference Serving via Proactive Scheduling](https://dl.acm.org/doi/abs/10.1145/3643832.3661894) |  Kong et al.<br/>MobiSys 2024   | Proactive scheduling that predicts future inference demand and pre-provisions resources. |  ○   |  ♢   |    ♠\|♦    |  Ⓒ   |  ✦   | **Heuristic** |                        -                        |
| [Time-Sensitive Multi-DNN Inference on CPU-GPU Edge Platforms ](https://ieeexplore.ieee.org/abstract/document/11268909) |    Ling et al.<br/>TMC 2026     | Introduces NAS-optimized duo-blocks for urgency-driven CPU-GPU scheduling in time-sensitive multi-DNN inference. |  ○   |  ♡♢  |   ♣\|♣♦▲   |  ⒸⓂ  |  ✫   | **Heuristic** |                        -                        |
| [AdaKnife: Flexible DNN Offloading for Inference Acceleration on Heterogeneous Mobile Devices ](https://ieeexplore.ieee.org/abstract/document/10700984) |   Liu et al.<br>IEEE TMC 2025   | Enables graph-based mixed-granularity partitioning and cross-framework offloading for faster distributed DNN inference. |  ○   |  ♡♢  |    ♣\|♣    |  Ⓒ   |  ✫   | **Heuristic** |                        -                        |
| [NetCut: Real-Time DNN Inference Using Layer Removal](https://ieeexplore.ieee.org/abstract/document/9474052) | Zandigohar et al.<br>DATE 2021  | Removes selected layers as latency nears the deadline; control via model complexity. |  ○   |  ♢   |    ♦\|♣    |  Ⓒ   |  ✫   | **Heuristic** |                        -                        |



### 3.2 Optimization-Based Scheduling

| Title                                                        |    Author / Year / Venue     | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      | Code |
| :----------------------------------------------------------- | :--------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :--: |
| [RT-mDL: Supporting Real-Time Mixed Deep Learning Tasks on Edge Platforms](https://dl.acm.org/doi/abs/10.1145/3485730.3485938) | Ling et al.<br/>SenSys 2021  | Jointly optimizes model scaling and task scheduling under storage and deadline constraints. |  ○   |  ♡♢  |   ♣♦\|♣    |  Ⓒ   |  ✦   | **Optimization** |  -   |
| [SPET: Transparent SRAM Allocation and Model Partitioning for Real-time DNN Tasks on Edge TPU ](https://ieeexplore.ieee.org/abstract/document/10247661) |   Han et al.<br/>DAC 2023    | Jointly optimizes SRAM allocation and model partitioning on Edge TPU. |  ○   |  ♧   |    ♣\|✤    |  ⒸⓂ  |  ✫   | **Optimization** |  -   |
| [RT-MDM: Real-Time Scheduling Framework for Multi-DNN on MCU Using External Memory](https://dl.acm.org/doi/abs/10.1145/3649329.3655681) |   Kang et al.<br/>DAC 2024   | Optimizes segmentation and segment-group mapping to cut external memory accesses on MCUs. |  ○   |  ♡   |   ♣\|▲✤    |  ⒸⓂ  |  ✫   | **Optimization** |  -   |
| [DNN Inference Acceleration Based on Adaptive Task Partitioning and Offloading in Embedded VEC ](https://dl.acm.org/doi/full/10.1145/3725734) | Li et al.<br/>ACM TECS 2025  | Joint adaptive task partitioning and offloading optimization under dynamic network/resource conditions. |  ○   |  ♡♢  |     ♣♥     |  Ⓒ   |  ✫   | **Optimization** |  -   |
| [SCENIC: Capability and Scheduling Co-Design for Intelligent Controller on Heterogeneous Platforms](https://ieeexplore.ieee.org/abstract/document/10844810) |  Chen et al.<br/>RTSS 2024   | Jointly optimizes resource allocation and controller-capability configuration on heterogeneous platforms. |  ○   |  ♡♢  |    ♦\|✤    |  Ⓒ   |  ✦   | **Optimization** |  -   |
| [EFFECT-DNN: Energy-efficient Edge Framework for Real-time DNN Inference](https://ieeexplore.ieee.org/abstract/document/10195765) | Zhang et al.<br>WoWMoM 2023  | Nested feedback loop tracking joint QoS and energy targets under fluctuating workloads. |  ○   |  ♡♢  |   ♣♥\|♣    |  Ⓒ   |  ✫   | **Optimization** |  -   |
| [EdgeAdaptor: Online Configuration Adaption, Model Selection and Resource Provisioning for Edge DNN Inference Serving at Scale](https://ieeexplore.ieee.org/abstract/document/9817634) | Zhao et al.<br>IEEE TMC 2022 | Jointly adapts configuration, model selection, and resource provisioning online. |  ○   |  ♤   |    ♦\|♣    |  Ⓒ   |  ✦   | **Optimization** |  -   |
| [Robust DNN Partitioning and Resource Allocation Under Uncertain Inference Time ](https://ieeexplore.ieee.org/abstract/document/11197924) | Nan et al.<br>IEEE TMC 2026  | formulates chance-constrained DNN partitioning and resource allocation under uncertain inference time. |  ○   |  ♡♢  |    ♥\|♣    |  ⒸⒻ  |  ✫   | **Optimization** |  -   |
| [Elastic Execution of Multi-Tenant DNNs on Heterogeneous Edge MPSoCs ](https://ieeexplore.ieee.org/abstract/document/10817905) |  Heidari et al.<br>SEC 2024  | elastically scales DNN inputs with greedy-ILP scheduling to meet deadlines under scene variability. |  ◎   | ♡♢♧  |   ♣♦\|♣♦   |  Ⓒ   |  ✦   | **Optimization** |  -   |

### 3.3 Control-Theoretic Scheduling

| Title                                                        |        Author / Year / Venue         | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |   Policy    |                       Code                        |
| :----------------------------------------------------------- | :----------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :---------: | :-----------------------------------------------: |
| [Towards QoS-Based Embedded Machine Learning](https://www.mdpi.com/2079-9292/11/19/3204) | Springer et al.<br>Electronics 2022  | Feedback controller adjusts compute resources to observed QoS deviations. |  ○   |  ♡♢  |   ♣♥\|✤    |  ⒸⒻ  |  ✦   | **Control** |                         -                         |
| [Future aware Dynamic Thermal Management in CPU-GPU Embedded Platforms](https://ieeexplore.ieee.org/abstract/document/9984747) |      Maity et al.<br>RTSS 2022       | Predicts temperature evolution and proactively adjusts CPU–GPU operation. |  ○   |  ♡♢  |   ♣♥\|♣    |  ⒸⒻ  |  ✪   | **Control** |                         -                         |
| [FC-GPU: Feedback Control GPU Scheduling for Real-time Embedded Systems](https://dl.acm.org/doi/full/10.1145/3761812) | Subramaniyan et al.<br>ACM TECS 2025 | Measures scheduling errors and dynamically adjusts GPU allocation via control theory. |  ○   |  ♢   |     ♣      |  Ⓒ   |  ✪   | **Control** |                         -                         |
| [Budget RNNs: Multi-Capacity Neural Networks to Improve In-Sensor Inference Under Energy Budgets ](https://ieeexplore.ieee.org/abstract/document/9470487) |     Kannan et al.<br/>RTAS 2021      | Energy-budgeted model selection; picks inference configs by available energy. |  ○   |  ♡   |   ♦♥\|♥    |  Ⓒ   |  ✦   | **Control** | [Code](https://github.com/tejaskannan/budget-rnn) |
| [AccuMO: Accuracy-Centric Multitask Offloading in Edge-Assisted Mobile Augmented Reality](https://dl.acm.org/doi/abs/10.1145/3570361.3592531) |     Kong et al.<br/>MobiCom 2023     | Accuracy-aware multitask offloading for mobile AR; models resource–accuracy relationship. |  ○   |  ♢   |    ♦\|♣    |  Ⓒ   |  ✦   | **Control** |    [Code](https://github.com/JonnyKong/AccuMO)    |

### 3.4 Profile-Guided and Learning-Driven Scheduling

| Title                                                        |      Author / Year / Venue      | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |           Policy            |                     Code                      |
| :----------------------------------------------------------- | :-----------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :-------------------------: | :-------------------------------------------: |
| [nn-Meter: towards accurate latency prediction of deep-learning model inference on diverse edge devices](https://dl.acm.org/doi/abs/10.1145/3458864.3467882) |  Zhang et al.<br>MobiSys 2021   | Operator-level latency predictors from profiling; estimates DNN latency across edge devices. |  ○   | ♡♢♧  |     ♣      |  Ⓒ   |  ✪   |        **Learning**         | [Code](https://github.com/microsoft/nn-Meter) |
| [Mediator: Characterizing and Optimizing Multi-DNN Inference for Energy Efficient Edge Intelligence](https://ieeexplore.ieee.org/abstract/document/10763713) |    Choi et al.<br>IISWC 2024    | Characterizes inter-model interference to guide energy-efficient multi-DNN execution. |  ◎   | ♡♢♧  |   ♣♦♥\|♣   | ⒸⒶⒻ  |  ✦   |        **Learning**         |                       -                       |
| [A Performance Prediction-based DNN Partitioner for Edge TPU Pipelining](https://ieeexplore.ieee.org/abstract/document/10773756) |    Zou et al.<br>MILCOM 2024    | Predicts partition performance to select Edge TPU pipeline configurations before deployment. |  ○   |  ♧   |   ♣♠\|▲    |  ⒸⓂ  |  ✫   | **Heuristic<br />Learning** |                       -                       |
| [Flex: Fast, Accurate DNN Inference on Low-Cost Edges Using Heterogeneous Accelerator Execution](https://dl.acm.org/doi/abs/10.1145/3689031.3696067) |   Sen et al.<br>EuroSys 2025    | Uses performance knowledge to select execution paths across heterogeneous accelerators. |  ○   |  ♡♢  |   ♣♦\|♣    |  ⒸⒶ  |  ✫   |        **Learning**         |                       -                       |
| [AdaDrone: Quality of Navigation Based Neural Adaptive Scheduling for Edge-Assisted Drones](https://ieeexplore.ieee.org/abstract/document/9912195) |    Chen et al.<br>ICDCS 2022    | Uses navigation quality as a learning objective to adapt edge-assisted drone inference. |  ○   |  ♡♢  |   ♣♦\|♣    |  Ⓒ   |  ✦   |        **Learning**         |                       -                       |
| [TapFinger: Task Placement and Fine-Grained Resource Allocation for Edge Machine Learning](https://ieeexplore.ieee.org/abstract/document/10229031) |    Li et al.<br>INFOCOM 2023    | Learns task placement and resource allocation across distributed edge ML resources. |  ○   |  ♡♢  |     ♠      |  Ⓒ   |  ✦   |        **Learning**         | [Code](https://github.com/nooblyh/TapFinger)  |
| [BCEdge: SLO-Aware DNN Inference Services With Adaptive Batch-Concurrent Scheduling on Edge Devices](https://ieeexplore.ieee.org/abstract/document/10549973) | Zhang et al.<br/>IEEE TNSM 2024 | Adaptive batch-concurrent scheduling that adapts to SLOs from runtime observations. |  ○   | ♡♢♧  |   ♣♠\|♣    |  ⒸⓂ  |  ✦   |        **Learning**         |                       -                       |
| [Reinforcement Learning-Based Edge-Assisted Inference With Multimodal Data](https://ieeexplore.ieee.org/abstract/document/11223761) |  Liu et al.<br/>IEEE TMC 2025   | RL to optimize inference across heterogeneous modalities and distributed edge resources. |  ○   | ♡♢♧  |    ♣♥♦     |  Ⓒ   |  ✫   |        **Learning**         |     [Code](https://github.com/Yucj7/MMCI)     |
| [Adaptive scheduling of online inference pipelines at the edge: A post-hoc request-oriented approach - ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S1383762125002693) |     Sun et al.<br>JSA 2025      | Dynamically selects models and batches for queued inference-pipeline requests using graph paths and MAB profiling. |  ◎   |  ♢♧  |    ♦\|♣    |  Ⓒ   |  ✦   |        **Learning**         |                       -                       |
| [Uncertainty-Aware RL-Based Scheduling of Multi-DNN Workloads on Edge MPSoCs](https://dl.acm.org/doi/abs/10.1145/3769102.3770621) |   Heidari et al.<br>SEC 2025    | Learns GNN-based multi-DNN scheduling on edge MPSoCs under workload and runtime uncertainty |  ◎   | ♡♢♧  |     ♣      |  Ⓒ   |  ✦   |        **Learning**         |                       -                       |

## 4. Design Tradeoffs and System Insights

### 4.1 Granularity Tradeoffs: Control Precision vs. Management Overhead

| Title                                                        |        Author / Year / Venue         | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                             Code                             |
| :----------------------------------------------------------- | :----------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :----------------------------------------------------------: |
| [Enabling Latency-Sensitive DNN Inference via Joint Optimization of Model Surgery and Resource Allocation in Heterogeneous Edge ](https://dl.acm.org/doi/abs/10.1145/3545008.3545071) |      Huang et al.<br/>ICPP 2022      | Model-level adaptation: low overhead but coarse control over model variants; joint model-surgery and resource-allocation optimization. |  ○   |  ♡♢  |     ♣      |  Ⓒ   |  ✫   | **Optimization** |                              -                               |
| [Throughput Maximization of Delay-Aware DNN Inference in Edge Computing by Exploring DNN Model Partitioning and Inference Parallelism](https://ieeexplore.ieee.org/abstract/document/9606540) |     Li et al.<br/>IEEE TMC 2021      | Partition-level control for delay-aware throughput; exploits pipeline parallelism at partition-point cost. |  ●   |  ♡   |    ♠\|♣    |  Ⓒ   |  ✫   | **Optimization** |                              -                               |
| [PArtNNer: Platform-Agnostic Adaptive Edge-Cloud DNN Partitioning for Minimizing End-to-End Latency ](https://dl.acm.org/doi/full/10.1145/3630266) |    Ghosh et al.<br/>ACM TECS 2024    | Platform-agnostic adaptive edge–cloud partitioning; portable but must reason about device/comm/latency. |  ○   |  ♡♢  |     ♣      |  Ⓒ   |  ✫   |  **Heuristic**   |                              -                               |
| [Inferencing on Edge Devices: A Time- and Space-aware Co-scheduling Approach ](https://dl.acm.org/doi/full/10.1145/3576197) |  Pereira et al.<br/>ACM TODAES 2023  | Task/pass-level co-scheduling reasoning about passes and GPU space; balances flexibility and analyzability; concurrency enlarges memory footprint. |  ○   |  ♢   |    ♣\|▲    |  ⒸⓂ  |  ✪   | **Optimization** |                              -                               |
| [AdaMEC: Towards a Context-adaptive and Dynamically Combinable DNN Deployment Framework for Mobile Edge Computing ](https://dl.acm.org/doi/full/10.1145/3630098) |    Pang et al.<br/>ACM TOSN 2023     | Operator-level atoms assembled per mobile-edge context; rich adaptation but combinatorial profiling cost. |  ○   |  ♡♢  |    ♣\|▲    |  ⒸⓂ  |  ✪   |  **Heuristic**   |                              -                               |
| [Crane: Inter-Layer Scheduling Framework for DNN Inference and Training Co-Support on Tiled Architecture ](https://dl.acm.org/doi/full/10.1145/3725843.3756023) |      Gong et al.<br/>MICRO 2025      | Inter-layer scheduling for tiled architectures with inference/training co-execution. |  ○   |  ♧   |     ♣♥     |  ⒸⓂ  |  ✫   | **Optimization** |                              -                               |
| [Stream: Design Space Exploration of Layer-Fused DNNs on Heterogeneous Dataflow Accelerators](https://ieeexplore.ieee.org/abstract/document/10713407) |    Symons et al.<br/>IEEE TC 2024    | Layer-fused execution on dataflow accelerators; cuts data movement but reduces scheduling flexibility. |  ○   |  ♧   |     ♣      |  ⒸⓂ  |  ✪   | **Optimization** |       [Code](https://github.com/KULeuven-MICAS/stream)       |
| [Real-time, Work-conserving GPU Scheduling for Concurrent DNN Inference](https://dl.acm.org/doi/full/10.1145/3768622) |     Han et al.<br/>ACM TOCS 2025     | Kernel/command-level real-time work-conserving GPU scheduling; reallocates unused GPU resources; high precision/utilization at higher bookkeeping cost and more interference. |  ●   |  ♢   |     ♣♠     |  Ⓒ   |  ✪   |  **Heuristic**   |                              -                               |
| [Improving DNN Inference Throughput Using Practical, Per-Input Compute Adaptation](https://dl.acm.org/doi/abs/10.1145/3694715.3695978) | Padmanabha Iyer et al.<br/>SOSP 2024 | Request-level per-input compute adaptation; efficient but adds runtime variability. |  ●   |  ♢   |    ♠\|♣    |  Ⓒ   |  ✫   | **Optimization** |                              -                               |
| [ZeroSwap: Minimizing Swap Overhead for Real-Time Multi-DNN Inference via SSD-based GPU Memory Extension](https://rtcl.dgist.ac.kr/index.php/zeroswap) |      Kang et al.<br/>RTAS 2026       | Memory-level control via SSD-based GPU memory extension; enables infeasible workloads with swap overhead. |  ○   |  ♡♢  |   ♣\|♣▲    |  Ⓜ   |  ✫   | **Optimization** | [Code](https://github.com/fredrickang/zeroswap_public/tree/main) |



### 4.2 Compute-Memory Tradeoffs: Compute Throughput vs. Memory Efficiency

| Title                                                        |      Author / Year / Venue       | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                        Code                        |
| :----------------------------------------------------------- | :------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :------------------------------------------------: |
| [Memory-efficient DNN training on mobile devices s](https://dl.acm.org/doi/abs/10.1145/3498361.3539765) |   Gim et al.<br/>MobiSys 2022    | Discards/regenerates activations (selective recomputation) to cut peak memory at extra computation (compute-for-memory). |  ○   |  ♤   |   ♣♠\|▲    |  Ⓜ   |  ✪   |  **Heuristic**   |                         -                          |
| [Occamy: Memory-efficient GPU Compiler for DNN Inference ](https://ieeexplore.ieee.org/abstract/document/10247839) |      Lee et al.<br>DAC 2023      | Compiler-stage memory-aware graph transformations to reduce GPU memory in inference. |  ○   |  ♢   |   ♣▲\|▲    |  Ⓜ   |  ✫   |  **Heuristic**   |   [Code](https://github.com/corelab-src/occamy)    |
| [MIME: adapting a single neural network for multi-task inference with memory-efficient dynamic pruning](https://dl.acm.org/doi/abs/10.1145/3489517.3530473) | Bhattacharjee et al.<br>DAC 2022 | Dynamic pruning of a shared network in multitask inference; balances memory and compute. |  ○   |  ♧   |    ♠♥♦▲    |  Ⓜ   |  ✫   |   **Learning**   |                         -                          |
| [Parallelfusion: towards maximum utilization of mobile gpu for dnn inference](https://dl.acm.org/doi/abs/10.1145/3469116.3470014) |     Lee et al.<br>EMDL 2021      | Parallel execution + kernel fusion to raise mobile-GPU utilization at higher memory residency. |  ○   |  ♢   |    ♠\|♣    |  Ⓒ   |  ✪   |  **Heuristic**   |                         -                          |
| [ElasticRoom: Multi-Tenant DNN Inference Engine via Co-design with Resource-constrained Compilation and Strong Priority Scheduling ](https://dl.acm.org/doi/abs/10.1145/3625549.3658654) |      Ma et al.<br>HPDC 2024      | Resource-constrained compilation + priority-aware scheduling for throughput with bounded memory. |  ○   |  ♢   |     ♣♠     |  Ⓒ   |  ✪   |  **Heuristic**   |                         -                          |
| [SEEB-GPU: Early-Exit Aware Scheduling and Batching for Edge GPU Inference](https://dl.acm.org/doi/abs/10.1145/3769102.3772715) | Subramaniyan et al.<br>SEC 2025  | Early-exit DNN with batching on edge GPUs; balances throughput vs. activation-memory growth. |  ●   |  ♢   |    ♣♠♦     |  Ⓐ   |  ★   |  **Heuristic**   |                         -                          |
| [PASK: Cold Start Mitigation for Inference with Proactive and Selective Kernel Loading on GPUs ](https://ieeexplore.ieee.org/abstract/document/11132809/) |     Huang et al.<br>DAC 2025     | Proactively loads GPU kernels to cut cold-start latency at higher memory residency. |  ○   |  ♢   |     ♣      |  Ⓒ   |  ✫   |  **Heuristic**   |                         -                          |
| [H2O: Heterogeneity-Aware Hierarchical Orchestration for Memory-Efficient On-Device LLM Inference ](https://ieeexplore.ieee.org/abstract/document/11224632) |   Zeng et al.<br>IEEE TMC 2026   | Orchestrates weights across heterogeneous memory hierarchies; lower footprint, more data movement. |  ❖   |  ♤   |   ♣▲\|▲    |  Ⓜ   |  ✫   | **Optimization** |      [Code](https://github.com/ccfeiker/H2O)       |
| [RAMS: Runtime Adaptive Memory Scaling for Tiny Deep Learning on IoT Devices](https://ieeexplore.ieee.org/abstract/document/11341897) |   Chu et al.<br>IEEE TMC 2026    | Runtime memory virtualization adjusting allocation to workload; efficiency vs. management cost. |  ○   |  ♡   |   ♣♥▲\|▲   |  Ⓜ   |  ✫   |  **Heuristic**   |                         -                          |
| [Breaking the Edge: Enabling Efficient Neural Network Inference on Integrated Edge Devices](https://ieeexplore.ieee.org/abstract/document/10959707) |  Zhang et al.<br>IEEE TCC 2025   | Shows bottleneck arises from compute/memory-capacity/bandwidth mismatch; argues for joint optimization. |  ○   |  ♤   |     ♣      |  Ⓒ   |  ✫   |  **Heuristic**   | [Code](https://github.com/ChenyangZhang-cs/EdgeNN) |



### 4.3 Predictability-Adaptivity Tradeoffs: Guarantees vs. Flexibility

| Title                                                        |         Author / Year / Venue          | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                             Code                             |
| :----------------------------------------------------------- | :------------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :----------------------------------------------------------: |
| [Real-Time Scheduling and Analysis of Processing Chains on Multi-threaded Executor in ROS 2](https://ieeexplore.ieee.org/abstract/document/9984791) |    Jiang et al. [121]<br>RTSS 2022     | Analyzable execution model for multi-threaded ROS 2 executors with end-to-end guarantees. |  ◎   |  ♡   |    ♣\|✤    |  Ⓒ   |  ✪   |  **Heuristic**   |                              -                               |
| [End-To-End Timing Analysis in ROS2](https://ieeexplore.ieee.org/abstract/document/9984789) |       Teper et al.<br>RTSS 2022        | End-to-end timing analysis deriving provable bounds across the ROS 2 pipeline. |  ◎   |  ♡   |    ♣\|✤    |  Ⓒ   |  ✪   |  **Heuristic**   | [Code](https://github.com/HarunTeper/ros2-end-to-end-distributed) |
| [Worst-Case Time Disparity Analysis of Message Synchronization in ROS](https://ieeexplore.ieee.org/abstract/document/9984711) |         Li et al.<br>RTSS 2022         | Analytical bounds on worst-case synchronization delay in ROS message streams. |  ◎   |  ♡   |    ♣\|✤    |  Ⓒ   |  ✪   |  **Heuristic**   |                              -                               |
| [SEAM: An Optimal Message Synchronizer in ROS with Well-Bounded Time Disparity](https://ieeexplore.ieee.org/abstract/document/10406013) |        Sun et al.<br>RTSS 2023         | Optimal message synchronization with bounded timing disparity (trades flexibility for guarantees). |  ◎   |  ♡   |    ♣\|✤    |  Ⓒ   |  ✪   |  **Heuristic**   | [Code](https://github.com/tianyiWangGithub/seam_synchronizer) |
| [Prophet: Realizing a Predictable Real-time Perception Pipeline for Autonomous Vehicles](https://ieeexplore.ieee.org/abstract/document/9984807) |        Liu et al.<br>RTSS 2022         | Predictable perception pipeline for AVs; prioritizes deterministic, bounded-latency execution. |  ◎   |  ♢   |    ♣\|✤    |  Ⓒ   |  ✦   |  **Heuristic**   |                              -                               |
| [CPT: A Configurable Predictability Testbed for DNN Inference in AVs](https://ieeexplore.ieee.org/abstract/document/10676407) | Liu et al.<br>Tsinghua Sci. Tech. 2024 | Configurable predictability testbed for DNN inference; quantifies cost of timing guarantees. |  ◎   |  ♢   |   ♣♦\|✤    |  Ⓒ   |  ✦   |  **Heuristic**   |                              -                               |
| [RT-BEV: Enhancing Real-Time BEV Perception for Autonomous Vehicles ](https://ieeexplore.ieee.org/abstract/document/10844833) |        Liu et al.<br>RTSS 2024         | Real-time BEV perception via architecture-level optimization while preserving timing awareness. |  ◎   |  ♢   |   ♣♦\|✤    |  Ⓒ   |  ✦   |  **Heuristic**   |       [Code](https://github.com/Torreskai0722/RT-BEV)        |
| [Jigsaw: Taming BEV-centric Perception on Dual-SoC for Autonomous Driving ](https://ieeexplore.ieee.org/abstract/document/10844764) |        Sun et al.<br>RTSS 2024         | Coordinates perception across dual-SoC AV platforms; intermediate predictability–adaptivity point. |  ◎   |  ♤   |    ♣\|✤    |  Ⓒ   |  ✦   |  **Heuristic**   |                              -                               |
| [Enabling Low Latency Edge Intelligence based on Multi-exit DNNs in the Wild ](https://ieeexplore.ieee.org/abstract/document/9546491) |       Huang et al.<br>ICDCS 2021       | Dynamically selects inference depth; improves average latency but complicates WCET analysis. |  ●   |  ♡   |    ♣\|✤    |  Ⓒ   |  ✫   | **Optimization** |                              -                               |
| [LOTUS: learning-based online thermal and latency variation management for two-stage detectors on edge devices](https://dl.acm.org/doi/abs/10.1145/3649329.3657310) |        Gong et al.<br>DAC 2024         | Online learning to manage latency/thermal variation in edge object detection. |  ○   |  ♤   |   ♣♥\|✤    |  Ⓕ   |  ✦   |   **Learning**   |   [Code](https://github.com/wuyushuwys/LOTUS/tree/master)    |
| [FLEX: Adaptive Task Batch Scheduling with Elastic Fusion in Multi-Modal Multi-View Machine Perception](https://ieeexplore.ieee.org/abstract/document/10844787) |         Xu et al.<br>RTSS 2024         | Adaptive batch scheduling + elastic fusion for multimodal perception; efficiency vs. verifiability. |  ◎   |  ♤   |   ♣♠♦\|✤   |  Ⓒ   |  ✦   |  **Heuristic**   |                              -                               |
| [Dělen: Enabling Flexible and Adaptive Model-serving for Multi-tenant Edge AI](https://dl.acm.org/doi/abs/10.1145/3576842.3582375) |       Liang et al.<br>IoTDI 2023       | Flexible model serving via dynamic resource allocation/workload adaptation (most adaptive end). |  ●   |  ♤   |    ♣♦♥     |  Ⓒ   |  ✦   |  **Heuristic**   |                              -                               |



### 4.4 Isolation-Sharing Tradeoffs: Safety Guarantees vs. System Efficiency

| Title                                                        |     Author / Year / Venue      | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                       Code                       |
| :----------------------------------------------------------- | :----------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :----------------------------------------------: |
| [AegisDNN: Dependable and Timely Execution of DNN Tasks with SGX](https://ieeexplore.ieee.org/abstract/document/9622390) |   Xiang et al.<br>RTSS 2021    | SGX enclaves isolate DNN execution; strong safety at the cost of sharing opportunities. |  ○   |  ♢   |    ♣\|♦    |  Ⓒ   |  ✫   | **Optimization** |                        -                         |
| [ROSGM: A Real-Time GPU Management Framework with Plug-In Policies for ROS 2](https://ieeexplore.ieee.org/abstract/document/10155690) |     Li et al.<br>RTAS 2023     | Plug-in GPU management for ROS 2 with policy-driven, controlled sharing boundaries. |  ◎   |  ♤   |     ♣♠     |  Ⓒ   |  ✪   |  **Heuristic**   |                        -                         |
| [Hardware Compute Partitioning on NVIDIA GPUs for Composable Systems ](https://par.nsf.gov/biblio/10652973) |  Bakita et al.<br>ECRTS 2025   | Spatial partitions on NVIDIA GPUs giving composability guarantees for concurrent workloads. |  ○   |  ♢   |    ♠\|✤    |  Ⓐ   |  ✦   |  **Heuristic**   | [Code](https://github.com/tanzelin430/libsmctrl) |
| [Real-Time Task Mapping for CPU-GPU Heterogeneous Platforms: Spatial GPU Partitioning and Utilization Bound ](https://dl.acm.org/doi/abs/10.1145/3803800) |  Han et al.<br>ACM TECS 2026   | Quantifies efficiency loss of isolation; shows excess partitioning hurts utilization. |  ○   |  ♢   |    ♣\|✤    |  Ⓒ   |  ✦   |  **Heuristic**   |                        -                         |
| [SPLIT: QoS-Aware DNN Inference on Shared GPU via Evenly-Sized Model Splitting](https://dl.acm.org/doi/abs/10.1145/3605573.3605627) |    Luo et al.<br>ICPP 2023     | Evenly-sized model splitting for QoS-aware inference on shared GPUs (interference-aware sharing). |  ●   |  ♤   |     ♣      |  Ⓒ   |  ✫   |  **Heuristic**   |   [Code](https://github.com/Arantir1028/SPLIT)   |
| [IasRT: Interference-Aware and SLO-Driven GPU Scheduling for Real-Time DNN Inference](https://ieeexplore.ieee.org/abstract/document/11311099) |   Zhong et al.<br>ICCD 2025    | SLO-driven GPU scheduling that models cross-workload interference at runtime. |  ●   |  ♢   |   ♣♠\|♣    |  Ⓐ   |  ✪   |  **Heuristic**   |                        -                         |
| [SGDRC: Software-Defined Dynamic Resource Control for Concurrent DNN Inference on NVIDIA GPUs ](https://dl.acm.org/doi/abs/10.1145/3710848.3710863) |   Zhang et al.<br>PPoPP 2025   | Software-defined dynamic GPU resource control; adaptive sharing vs. weaker analytical guarantees. |  ●   |  ♢   |    ♠\|♣    |  Ⓐ   |  ✪   |  **Heuristic**   |                        -                         |
| [Analysis and Mitigation of Shared Resource Contention on Heterogeneous Multicore: An Industrial Case Study ](https://ieeexplore.ieee.org/abstract/document/10494679) | Bechtel et al.<br>IEEE TC 2024 | Shows uncontrolled interference degrades temporal guarantees on heterogeneous multicore. |  ◎   |  ♤   |   ♣♦\|✤    |  Ⓜ   |  ✫   |  **Heuristic**   |                        -                         |
| [Fat Block: Narrowing the Boundary of pWCET Analysis on GPU](https://dl.acm.org/doi/full/10.1145/3807211.3807227) |   Zheng et al.<br>ADIST 2026   | Narrows gap between observed execution and probabilistic WCET on GPUs via interference modeling. |  ○   |  ♢   |    ♣\|✤    |  Ⓐ   |  ✪   |  **Heuristic**   |                        -                         |



### 4.5 Latency-Energy-Accuracy Tradeoffs: Multi-Objective Tension

| Title                                                        |      Author / Year / Venue       | Key Mechanism / Contribution                                 |  W   |  H   | Obj \| Con | Orc  | Gran |      Policy      |                        Code                        |
| :----------------------------------------------------------- | :------------------------------: | ------------------------------------------------------------ | :--: | :--: | :--------: | :--: | :--: | :--------------: | :------------------------------------------------: |
| [EdgeBERT: Sentence-Level Energy Optimizations for Latency-Aware Multi-Task NLP Inference](https://dl.acm.org/doi/abs/10.1145/3466752.3480095) |    Tambe et al.<br>MICRO 2021    | Sentence-level model adaptation for multi-task NLP; energy savings at fixed latency. |  ●   |  ♧   |    ♥\|♣    |  Ⓕ   |  ★   |  **Heuristic**   |    [Code](https://github.com/AYUSHMIT/EdgeBERT)    |
| [Fast-Inf: Ultra-Fast Embedded Intelligence on the Batteryless Edge ](https://dl.acm.org/doi/abs/10.1145/3666025.3699335) |  Custode et al.<br>SenSys 2024   | Ultra-lightweight inference pipelines for batteryless devices under tight energy budgets. |  ○   |  ♡   |   ♣♥▲\|♣   |  Ⓒ   |  ✦   |  **Heuristic**   | [Code](https://github.com/DIOL-UniTN/Fast-Inf-FFF) |
| [Multi-Exit DNN Inference Acceleration Based on Multi-Dimensional Optimization for Edge Intelligence](https://ieeexplore.ieee.org/abstract/document/9769868) |   Dong et al.<br>IEEE TMC 2022   | Dynamic exit-point selection; latency reduction via early termination with controlled accuracy loss. |  ●   |  ♤   |     ♣      |  Ⓒ   |  ✦   | **Optimization** |                         -                          |
| [DNN Surgery: Accelerating DNN Inference on the Edge Through Layer Partitioning](https://ieeexplore.ieee.org/abstract/document/10076802) |  Liang et al.<br>IEEE TCC 2023   | Adaptive layer partitioning/structural simplification to shorten latency while preserving accuracy. |  ○   |  ♢   |   ♣♦♥\|♣   |  Ⓒ   |  ✫   | **Optimization** |                         -                          |
| [Panopticus: Omnidirectional 3D Object Detection on Resource-constrained Edge Devices](https://dl.acm.org/doi/abs/10.1145/3636534.3690688) |    Lee et al.<br>MobiCom 2024    | Model simplification to sustain real-time 3D object detection on constrained edge perception. |  ●   |  ♤   |    ♦\|♣    |  Ⓒ   |  ✦   | **Optimization** |                         -                          |
| [Energy-Efficient Approximate Edge Inference Systems ](https://dl.acm.org/doi/full/10.1145/3589766) |  Ghosh et al.<br>ACM TECS 2023   | Approximation that lowers compute intensity for energy savings within bounded accuracy loss. |  ○   |  ♦   |    ♥\|♦    |  Ⓒ   |  ✦   | **Optimization** |                         -                          |
| [CANNON: Communication-Aware Sparse Neural Network Optimization ](https://ieeexplore.ieee.org/abstract/document/10171170) | Goksoy et al.<br>IEEE TETC 2023  | Communication-aware sparsification for distributed inference; joint transmission/compute reduction. |  ○   |  ♧   |   ♣♥\|♦    |  Ⓜ   |  ✫   |  **Heuristic**   |                         -                          |
| [Energy-Efficient and Accuracy-Aware DNN Inference With IoT Device-Edge Collaboration ](https://ieeexplore.ieee.org/abstract/document/10858448) |  Jiang et al.<br>IEEE TSC 2025   | Collaborative device–edge execution jointly optimizing energy and prediction quality. |  ○   |  ♤   |    ♥\|♦    |  Ⓒ   |  ✫   | **Optimization** |                         -                          |
| [Exploring the Boundaries of On-Device Inference: When Tiny Falls Short, Go Hierarchical](https://ieeexplore.ieee.org/abstract/document/11066245) | Behera et al.<br>IEEE IoT-J 2025 | Hierarchical inference + adaptive offloading across latency/energy/accuracy requirements. |  ●   |  ♡   |   ♣♥\|♦    |  Ⓒ   |  ★   |  **Heuristic**   |                         -                          |



## 5. Open Challenges and Future Directions

### 5.1 LLM-Aware Runtime Systems

| Title                                                        |    Author / Year / Venue    | Key Mechanism / Contribution |  W   |  H   | Obj \| Con | Orc  | Gran | Policy |                         Code                          |
| :----------------------------------------------------------- | :-------------------------: | ---------------------------- | :--: | :--: | :--------: | :--: | :--: | :----: | :---------------------------------------------------: |
| [MobileLLM: Optimizing Sub-billion Parameter Language Models for On-Device Use Cases](https://openreview.net/forum?id=EIGbXbxcUQ) |  liu et al.<br/>ICML 2024   |                              |      |      |            |      |      |        | [Code](https://github.com/facebookresearch/MobileLLM) |
| [EdgeLLM: Fast On-Device LLM Inference With Speculative Decoding](https://ieeexplore.ieee.org/abstract/document/10812936) | Xu et al.<br/>IEEE TMC 2025 |                              |      |      |            |      |      |        |                           -                           |
| [CLONE: Customizing LLMs for Efficient Latency-Aware Inference at the Edge](https://www.usenix.org/conference/atc25/presentation/tian) |  Tian et al.<br/>ATC 2025   |                              |      |      |            |      |      |        |       [Code](https://github.com/qxpBlog/CLONE)        |



### 5.2 Memory-Centric Resource Management

| Title | Author / Year / Venue | Key Mechanism / Contribution |  W   |  H   | Obj \| Con | Orc  | Gran | Policy | Code |
| :---- | :-------------------: | ---------------------------- | :--: | :--: | :--------: | :--: | :--: | :----: | :--: |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |



### 5.3 AI-Native Runtime Systems

| Title | Author / Year / Venue | Key Mechanism / Contribution |  W   |  H   | Obj \| Con | Orc  | Gran | Policy | Code |
| :---- | :-------------------: | ---------------------------- | :--: | :--: | :--------: | :--: | :--: | :----: | :--: |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |



### 5.4 Verified and Explainable Scheduling

| Title | Author / Year / Venue | Key Mechanism / Contribution |  W   |  H   | Obj \| Con | Orc  | Gran | Policy | Code |
| :---- | :-------------------: | ---------------------------- | :--: | :--: | :--------: | :--: | :--: | :----: | :--: |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |
|       |                       |                              |      |      |            |      |      |        |      |

