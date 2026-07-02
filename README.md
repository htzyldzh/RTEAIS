<div align="center">

# Real-Time Embedded AI Systems

## Scheduling and Resource Orchestration for Multi-DNN Edge Intelligence

</div>

## News

- **[Jun. 2026]** Initial release of the literature collection and taxonomy.
- Paper and repository status will be updated after publication.

## Overview

This repository accompanies the survey paper **“Real-Time Embedded AI Systems: Scheduling and Resource Orchestration for Multi-DNN Edge Intelligence.”** It maintains a structured and continuously updated collection of research on runtime scheduling and resource orchestration for embedded AI systems.

The survey focuses on systems that execute one or more AI workloads under timing and resource constraints on heterogeneous embedded platforms. Rather than organizing prior work only by application or hardware type, we analyze each system along the paper's seven-dimensional taxonomy:


$$
\mathcal{S}=(W,H,C,O,G,\Gamma,\Pi),
$$


where \(W\) denotes workload characteristics, \(H\) hardware and resource characteristics, \(C\) runtime constraints, \(O\) orchestration actions, \(G\) optimization objectives, $\Gamma$ orchestration granularity, and $\Pi$ decision-making policies.

The repository serves three purposes:

1. provide a searchable collection of representative systems and their artifacts;
2. expose common mechanisms across compute execution, memory, accelerator, and processor-frequency/energy orchestration; and
3. summarize design tradeoffs and open problems for future real-time embedded AI runtimes.

## Scope

### Included

- Real-time and latency-sensitive DNN inference on embedded and edge platforms.
- Multi-DNN, pipeline, streaming, and generative AI workloads.
- Heterogeneous processors, including CPUs, GPUs, NPUs, DSPs, FPGAs, TPUs, and MCU-class devices.
- Compute, memory, accelerator, power, thermal, and cross-device orchestration.
- Rule-based, optimization-based, control-theoretic, profile-guided, and learning-driven runtime policies.
- Timing analysis, schedulability, deadline-aware adaptation, and resource-interference management.

### Outside the Primary Scope

- Model compression methods that do not involve runtime resource management.
- Standalone accelerator microarchitectures without a scheduling or orchestration component.
- Cloud datacenter training schedulers unless their mechanisms directly inform embedded AI runtimes.
- Edge AI applications that do not study runtime scheduling, allocation, placement, or adaptation.

## Contents

- [1. Related Surveys](#1-related-surveys)
- [2. Unified Taxonomy](#2-unified-taxonomy)
- [3. Resource Orchestration Mechanisms](#3-resource-orchestration-mechanisms)
  - [3.1 Compute Execution Orchestration](#31-compute-execution-orchestration)
  - [3.2 Memory Resource Orchestration](#32-memory-resource-orchestration)
  - [3.3 Accelerator Resource Orchestration](#33-accelerator-resource-orchestration)
  - [3.4 Processor-Frequency and Energy Orchestration](#34-processor-frequency-and-energy-orchestration)
- [4. Runtime Decision-Making Strategies](#4-runtime-decision-making-strategies)
  - [4.1 Rule-Based and Priority-Driven Scheduling](#41-rule-based-and-priority-driven-scheduling)
  - [4.2 Optimization-Based Scheduling](#42-optimization-based-scheduling)
  - [4.3 Control-Theoretic Scheduling and Runtime Adaptation](#43-control-theoretic-scheduling-and-runtime-adaptation)
  - [4.4 Profile-Guided Decision Support and Learning-Driven Scheduling](#44-profile-guided-decision-support-and-learning-driven-scheduling)
- [5. Design Tradeoffs and System Insights](#5-design-tradeoffs-and-system-insights)
  - [5.1 Granularity Tradeoffs](#51-granularity-tradeoffs-control-precision-vs-management-overhead)
  - [5.2 Compute--Memory Tradeoffs](#52-compute--memory-tradeoffs-compute-throughput-vs-memory-efficiency)
  - [5.3 Predictability--Adaptivity Tradeoffs](#53-predictability--adaptivity-tradeoffs-guarantees-vs-flexibility)
  - [5.4 Isolation--Sharing Tradeoffs](#54-isolation-sharing-tradeoffs-isolation-guarantees-vs-resource-efficiency)
  - [5.5 Latency--Energy--Accuracy Tradeoffs](#55-latency--energy--accuracy-tradeoffs-multi-objective-tension)
- [6. Open Challenges and Future Directions](#6-open-challenges-and-future-directions)
  - [6.1 LLM-Aware Runtime Systems](#61-llm-aware-runtime-systems)
  - [6.2 Memory-Centric Resource Management](#62-memory-centric-resource-management)
  - [6.3 AI-Native Runtime Systems](#63-ai-native-runtime-systems)
  - [6.4 Timing-Assured and Explainable Orchestration](#64-timing-assured-and-explainable-orchestration)
- [Citation](#citation)
- [Contributing](#contributing)
- [License](#license)

## 1. Related Surveys

Existing surveys cover edge intelligence, embedded deep-learning infrastructure, GPU datacenter scheduling, vehicular edge computing, and resource-constrained LLM deployment. This survey differs by jointly examining **real-time constraints, multi-DNN concurrency, heterogeneous embedded hardware, runtime decision policies, orchestration actions, optimization objectives, orchestration granularity, and system-level tradeoffs**.

| Survey | Venue | Year | Main Scope | Difference from This Survey | Artifact |
|---|---:|:---:|---|---|:---:|
| [Cognitive Edge Computing: A Comprehensive Survey on Optimizing Large Models and AI Agents for Pervasive Deployment](https://arxiv.org/abs/2501.03265) | arXiv | 2025 | Large models and AI agents on pervasive edge platforms | Focuses on deployment and agents rather than real-time multi-DNN orchestration | - |
| [Edge Intelligence Unleashed: A Survey on Deploying Large Language Models in Resource-Constrained Environments](https://acnsci.org/journal/index.php/jec/article/view/1000) | JEC | 2025 | Resource-constrained LLM deployment | Emphasizes LLM deployment techniques rather than general runtime scheduling | - |
| [Deep Learning Workload Scheduling in GPU Datacenters: A Survey](https://dl.acm.org/doi/full/10.1145/3638757) | ACM Computing Surveys | 2024 | DL training and inference scheduling in GPU datacenters | Targets cloud GPU clusters rather than heterogeneous embedded SoCs | [GitHub](https://github.com/S-Lab-System-Group/Awesome-DL-Scheduling-Papers) |
| [Deep Learning for Edge Computing Applications: A State-of-the-Art Survey](https://ieeexplore.ieee.org/document/9044329) | IEEE Access | 2020 | Edge deep-learning applications and enabling techniques | Provides broad application coverage but limited real-time orchestration analysis | - |
| [Efficient Deep Learning Infrastructures for Embedded Computing Systems: A Comprehensive Survey and Future Envision](https://dl.acm.org/doi/10.1145/3701728) | ACM TECS | 2024 | Efficient embedded deep-learning infrastructure | Covers infrastructure broadly but does not center on runtime policies and multi-DNN contention | - |
| [Deep Reinforcement Learning-Based Task Scheduling and Resource Allocation for Vehicular Edge Computing: A Survey](https://ieeexplore.ieee.org/document/11173255) | IEEE TITS | 2025 | DRL-based scheduling in vehicular edge computing | Focuses on DRL and VEC rather than the broader embedded-AI runtime design space | - |

## 2. Unified Taxonomy

The paper represents an embedded AI runtime system with the survey-level abstraction

$$
S=(W,H,C,O,G,\Gamma,\Pi),
$$

where \(W\) denotes workload characteristics, \(H\) hardware and resource characteristics, \(C\) runtime constraints, \(O\) orchestration actions, \(G\) optimization objectives, $\Gamma$ orchestration granularity, and $\Pi$ decision-making policies.

The first three dimensions describe the operating context: what is executed, which resources are available, and which requirements define feasible behavior. The remaining four dimensions describe how the runtime responds: which actions it can take, which outcomes it prefers, at what boundary it intervenes, and how it selects an action.

| Dimension | Question Answered | Representative Values | Runtime Implication |
|---|---|---|---|
| **Workload (W)** | What is executed? | Static, dynamic, pipeline-oriented, and generative workloads | Determines graph stability, request variability, inter-model dependencies, phase changes, and persistent runtime state. |
| **Hardware (H)** | Where can it execute? | CPUs, GPUs, NPUs/DLAs, DSPs, FPGAs, dataflow accelerators, memory hierarchies, and edge-cloud resources | Determines legal execution choices, data-movement cost, concurrency, interference, and available preemption or partitioning support. |
| **Constraints (C)** | What must be satisfied? | Deadlines, memory and bandwidth limits, energy or thermal envelopes, accuracy floors, and predictability requirements | Defines the feasible operating region and excludes actions that violate mandatory requirements. |
| **Orchestration Actions (O)** | What can the runtime change? | Dispatching, admission, batching, partitioning, placement, paging, prefetching, sharing, model adaptation, DVFS, and migration | Defines the runtime control space and the resource bottlenecks that can be acted upon. |
| **Objectives (G)** | What is optimized? | Latency, deadline satisfaction, throughput, utilization, energy, data movement, accuracy, and predictability | Ranks the actions that remain feasible after the constraints in \(C\) have been applied. |
| **Granularity (Γ)** | At what boundary does control occur? | Application, model, request, batch, stage, subgraph, layer, operator, kernel, tensor segment, or cache page | Determines control precision, profiling effort, monitoring cost, and scheduling overhead. |
| **Policy (Π)** | How is an action selected? | Rule- and priority-based, optimization-based, control-theoretic, and profile-guided or learning-driven policies | Determines online decision cost, adaptability, model dependence, and the strength of available timing evidence. |

### Classification Notes

- The taxonomy is a comparison framework rather than a set of mutually exclusive system labels.
- Workload labels describe the dominant execution structure, not mutually exclusive application classes; for example, a pipeline may include static, dynamic, or generative stages.
- Hardware labels capture the complete execution and resource substrate, including processors, accelerators, shared memory, interconnects, storage, and nearby edge or cloud resources.
- **Deadline-aware** and **real-time guaranteed** are not equivalent. The former uses deadlines as scheduling signals; the latter requires analytical or enforced timing evidence.
- **Constraints (C)** define feasibility, while **Objectives (G)** rank actions after feasibility has been established.
- **Orchestration action (O)** is kept separate from **objective (G)**: swapping, batching, or DVFS is an action, while latency, energy, or predictability is an optimized outcome.
- When a system spans multiple control units, **Granularity (Γ)** records the dominant runtime control boundary and may list secondary units when the paper explicitly discusses them.
- Hybrid policies are classified by the dominant mechanism that determines the final runtime action; a learned predictor inside a heuristic or optimization routine does not by itself make the scheduler learning-driven.

## Table Schema

Detailed classification tables use the following fields. Tradeoff and future-direction tables may use a smaller comparison-oriented subset, but the canonical labels remain the same seven dimensions.

| Field | Description |
|---|---|
| **Title** | Paper or system name with publication link |
| **Author / Venue / Year** | Compact bibliographic metadata |
| **Core Mechanism / Contribution** | One-sentence summary of the runtime contribution |
| **Workload (W)** | Static, dynamic, pipeline-oriented, or generative workload |
| **Hardware (H)** | Main processors, accelerators, memory hierarchy, storage, and edge/cloud resource substrate |
| **Constraints (C)** | Mandatory requirements such as deadlines, memory or bandwidth limits, energy or thermal envelopes, accuracy floors, predictability, interference bounds, or policy-overhead limits |
| **Orchestration Action (O)** | Runtime action exposed by the system, such as dispatching, admission, batching, preemption, partitioning, placement, paging, prefetching, sharing, DVFS, migration, or model adaptation |
| **Objectives (G)** | Metrics optimized after constraints are applied |
| **Granularity (Γ)** | Runtime control unit |
| **Decision Policy (Π)** | Rule- or priority-based, optimization-based, control-theoretic, profile-guided, or learning-driven policy |
| **Artifact** | Public code, dataset, project page, or reproducibility package |

## 3. Resource Orchestration Mechanisms

Resource orchestration determines how runtime resources are allocated, shared, and reconfigured. The following groups distinguish the managed resource from the decision policy used to manage it.

### 3.1 Compute Execution Orchestration

Compute orchestration controls when and where inference units execute. Existing systems progressively move from stage- and layer-level scheduling toward preemptible operators, elastic kernels, adaptive models, and spatio-temporal GPU allocation.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Core Mechanism / Contribution | Workload (W) | Hardware (H) | Constraints (C) | Orchestration Action (O) | Objectives (G) | Granularity (Γ) | Decision Policy (Π) | Artifact |
|---|---|---|---|---|---|---|---|---|---|:---:|
| [DUET: A Compiler-Runtime Subgraph Scheduling Approach for Tensor Programs on a Coupled CPU-GPU Architecture](https://ieeexplore.ieee.org/abstract/document/9460468) | Zhang et al., IPDPS 2021 | Decomposes tensor programs into schedulable subgraphs and coordinates their execution on a coupled CPU–GPU architecture. | Static | CPU+GPU | timing constraints | Subgraph partitioning and CPU-GPU dispatch | Latency | Stage / subgraph | Heuristic | - |
| [Real-Time Multitasking of Deep Neural Networks with NVIDIA TensorRT](https://ieeexplore.ieee.org/abstract/document/11315110) | Aromolo et al., RTSS 2025 | Partitions long inference jobs into chunks that expose preemption points and reduce blocking. | Static | CPU+GPU | deadlines | TensorRT chunking and limited preemption | Latency, predictability | Chunk | Heuristic | - |
| [Pantheon: Preemptible Multi-DNN Inference on Mobile Edge GPUs](https://dl.acm.org/doi/abs/10.1145/3643832.3661878) | Han et al., MobiSys 2024 | Slices DNNs into chunks and uses software scheduling with nested redundancy to enable preemption among real-time DNN tasks. | Static multi-DNN | GPU | deadlines | Chunk-level preemption and software scheduling | Latency, accuracy | Chunk / segment | Heuristic | [Artifact](https://zenodo.org/records/11094058) |
| [DynaMIX: Resource Optimization for DNN-Based Real-Time Applications on a Multitasking System](https://arxiv.org/abs/2302.01568) | Cho et al., arXiv 2023 | Adjusts mixed precision across concurrent applications using offline latency and accuracy profiles. | Static | CPU | deadline, memory | Mixed-precision runtime mode selection | Latency, accuracy | Application / precision mode | Optimization | - |
| [DREAM: A Dynamic Scheduler for Dynamic Real-Time Multi-Model ML Workloads](https://dl.acm.org/doi/abs/10.1145/3623278.3624753) | Kim et al., ASPLOS 2023 | Combines admission control and adaptive model selection to reduce deadline violations during overload. | Dynamic multi-model | NPU | deadline, energy budget | Admission control and adaptive model selection | Latency, energy | Task / model / layer | Heuristic | - |
| [DNN-SAM: Split-and-Merge DNN Execution for Real-Time Object Detection](https://ieeexplore.ieee.org/abstract/document/9804671) | Kang et al., RTAS 2022 | Separates mandatory and optional computation so that essential inference completes within its timing requirement. | Static | GPU | deadlines | Mandatory/optional split-and-merge execution | Latency, accuracy | Mandatory / optional subtask | Heuristic | - |
| [CF-DETR: Coarse-to-Fine Transformer for Real-Time Object Detection](https://ieeexplore.ieee.org/abstract/document/11315056) | Shin et al., RTSS 2025 | Exploits DETR patch granularity, attention scope, and coarse-to-fine refinement to trade accuracy for timeliness under runtime slack. | Dynamic | GPU+NPU | deadlines | Coarse-to-fine patch/attention adaptation | Latency, accuracy | Subgraph (secondary: patch / attention scope) | Heuristic (slack-driven) | - |
| [Pipelined Data-Parallel CPU/GPU Scheduling for Multi-DNN Real-Time Inference](https://doi.org/10.1109/RTSS46320.2019.00042) | Xiang & Kim, RTSS 2019 | Pipelined data-parallel CPU–GPU scheduling for multi-DNN real-time inference; discussed in the survey as a taxonomy example (DART). | Pipeline-oriented | Heterogeneous CPU–GPU | Real-time latency / deadlines | Pipeline execution + CPU/GPU scheduling | End-to-end latency (predictable) | Pipeline stage | Analyzable priority-based | - |
| [A Co-Scheduling Framework for DNN Models on Mobile and Edge Devices with Heterogeneous Hardware](https://doi.org/10.1109/TMC.2019.2953525) | Xu et al., IEEE TMC 2021 | Co-scheduling of multi-model workloads across heterogeneous mobile/edge hardware with learning-guided adaptation; discussed in the survey as a taxonomy example (COSREL). | Dynamic multi-model | Mobile/edge heterogeneous hardware | Throughput / fairness targets | Model-level co-scheduling + resource allocation | Throughput and fairness | Model | Learning-guided (learning-driven) | - |

</details>

### 3.2 Memory Resource Orchestration

Memory orchestration evolves from static buffer planning and patch execution to coordinated paging, swapping, recomputation, and heterogeneous-memory placement. The central challenge is to lower peak memory without introducing data movement or recomputation that violates timing constraints.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Core Mechanism / Contribution | Workload (W) | Hardware (H) | Constraints (C) | Orchestration Action (O) | Objectives (G) | Granularity (Γ) | Decision Policy (Π) | Artifact |
|---|---|---|---|---|---|---|---|---|---|:---:|
| [MCUNet: Tiny Deep Learning on IoT Devices](https://proceedings.neurips.cc/paper/2020/hash/86c51678350f656dcc7f490a43946ee5-Abstract.html) | Lin et al., NeurIPS 2020 | Co-designs TinyNAS and TinyEngine with globally planned buffers for MCU-scale inference. | Static | MCU / CPU | SRAM capacity | Global buffer planning | Latency, accuracy | Model / buffer | Optimization | [Code](https://github.com/mit-han-lab/mcunet) |
| [Memory-Efficient Patch-Based Inference for Tiny Deep Learning](https://proceedings.neurips.cc/paper_files/paper/2021/hash/1371bccec2447b5aa6d96d2a540fb401-Abstract.html) | Lin et al., NeurIPS 2021 | Processes small spatial regions sequentially to reduce peak activation memory. | Static | MCU / CPU | SRAM capacity | Patch-based activation scheduling | Latency, accuracy | Patch / layer | Optimization | - |
| [StreamNet: Memory-Efficient Streaming Tiny Deep Learning Inference on the Microcontroller](https://proceedings.neurips.cc/paper_files/paper/2023/hash/7526508f11bbe0a123af62b9dab1fbe1-Abstract-Conference.html) | Zheng et al., NeurIPS 2023 | Uses streaming buffers and execution-aware reuse to avoid redundant patch computation. | Static | MCU / CPU | SRAM capacity | Streaming buffer reuse | Latency, memory | Layer / stream buffer | Optimization | - |
| [FlexNN: Efficient and Adaptive DNN Inference on Memory-Constrained Edge Devices](https://dl.acm.org/doi/abs/10.1145/3636534.3649391) | Li et al., MobiCom 2024 | Jointly coordinates tensor partitioning, loading, and execution according to available memory. | Static | CPU | capacity | Tensor partitioning, loading, and execution | Latency, memory | Tensor / layer | Optimization | [Code](https://github.com/xxxxyu/FlexNN) |
| [vMCU: Coordinated Memory Management and Kernel Optimization for DNN Inference on MCUs](https://proceedings.mlsys.org/paper_files/paper/2024/hash/d5a655b8b373737b4f2aea8f78e5e754-Abstract-Conference.html) | Zheng et al., MLSys 2024 | Virtualizes on-chip memory and coordinates allocation with kernel execution. | Static | MCU / CPU | SRAM capacity | Memory virtualization and kernel allocation | Memory | Kernel / memory segment | Hybrid heuristic and optimization | - |
| [FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness](https://proceedings.neurips.cc/paper_files/paper/2022/hash/67d57c32e20fd0a7a302cb81d36e40d5-Abstract-Conference.html) | Dao et al., NeurIPS 2022 | Uses IO-aware tiling to reduce data movement between high-bandwidth and on-chip memory. | Static / generative | GPU | bandwidth | IO-aware attention tiling | Latency, memory | Operator / tile | Algorithmic optimization | [Code](https://github.com/Dao-AILab/flash-attention) |
| [HeteGen: Efficient Heterogeneous Parallel Inference for Large Language Models on Resource-Constrained Devices](https://proceedings.mlsys.org/paper_files/paper/2024/hash/5431dca75a8d2abc1fb51e89e8324f10-Abstract-Conference.html) | Zhao et al., MLSys 2024 | Coordinates computation and memory placement across CPUs and GPUs for LLM inference. | Generative | CPU+GPU | memory capacity | Heterogeneous layer and tensor placement | Latency | Layer / tensor | Optimization | - |
| [Quilt: Peak-Memory-Aware Partitioning and Scheduling for Multi-Tenant DNN Model Inference](https://www.sciencedirect.com/science/article/abs/pii/S1383762126000147) | Lee et al., JSA 2026 | Partitions models into peak-memory-aware tasks and coordinates them to prevent OOM without layer-level scheduling overhead. | Static multi-tenant | GPU | memory capacity | Peak-memory-aware task partitioning and coordination | Latency, memory | Task / model partition | Heuristic | - |
| [AWTO: A Latency-Optimized Task Offloading Scheme for LLM-Driven Agentic Workflows on Heterogeneous Edge](https://www.sciencedirect.com/science/article/abs/pii/S0167739X2600049X) | Yu et al., FGCS 2026 | Optimizes offloading decisions for heterogeneous LLM-driven agent workflows. | Generative / pipeline | Heterogeneous edge | compute, communication, memory | Task offloading for agentic workflows | Latency | Task / model | Learning-based | - |
| [Demand Layering for Real-Time DNN Inference with Minimized Memory Usage](https://ieeexplore.ieee.org/abstract/document/9984745) | Ji et al., RTSS 2022 | Loads DNN parameters from SSD layer by layer and pipelines reading, copying, and execution. | Static | CPU+GPU+SSD | deadlines, capacity | Demand-driven layer loading | Latency, memory | Layer | Optimization | [Code](https://github.com/aveeslab/demand-layering) |
| [RT-Swap: Addressing GPU Memory Bottlenecks for Real-Time Multi-DNN Inference](https://www.computer.org/csdl/proceedings-article/rtas/2024/584100a373/1Y5EZtFnZ2o) | Kang et al., RTAS 2024 | Swaps inactive model states under priority control while accounting for real-time requirements. | Static multi-DNN | GPU | memory capacity | Priority-aware model-state swapping | Latency, predictability | Layer / model state | Optimization | [Code](https://github.com/fredrickang/Public-RT-Swap) |

</details>

### 3.3 Accelerator Resource Orchestration

Accelerator orchestration coordinates heterogeneous execution engines. The main progression is from CPU–GPU co-execution, through multi-DNN subgraph scheduling, to portable preemptive abstractions and workload-specific use of NPUs and dataflow accelerators.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Core Mechanism / Contribution | Workload (W) | Hardware (H) | Constraints (C) | Orchestration Action (O) | Objectives (G) | Granularity (Γ) | Decision Policy (Π) | Artifact |
|---|---|---|---|---|---|---|---|---|---|:---:|
| [CoDL: Efficient CPU-GPU Co-Execution for Deep Learning Inference on Mobile Devices](https://www.microsoft.com/en-us/research/wp-content/uploads/2022/05/mobisys22-CoDL__Efficient_CPU_GPU_Co_execution_for_DL_Model_Inference_on_Mobile_Devices-4.pdf) | Jia et al., MobiSys 2022 | Enables operator-level CPU–GPU co-execution using concurrency-aware latency prediction and data-sharing control. | Static | CPU+GPU | - | Operator-level CPU-GPU co-execution | Latency, energy | Operator | Learning-assisted heuristic | [Code](https://github.com/csu-eis/CoDL) |
| [BAND: Coordinated Multi-DNN Inference on Heterogeneous Mobile Processors](https://dl.acm.org/doi/abs/10.1145/3498361.3538948) | Jeong et al., MobiSys 2022 | Partitions mobile multi-DNN workloads into subgraphs and schedules unsupported fallback operators across heterogeneous processors. | Dynamic multi-DNN | CPU+GPU+DSP+NPU | deadlines | Subgraph scheduling with fallback-operator coordination | Latency, throughput | Subgraph | Heuristic | [Code](https://github.com/mrsnu/band) |
| [BlastNet: Exploiting Duo-Blocks for Cross-Processor Real-Time DNN Inference](https://dl.acm.org/doi/abs/10.1145/3560905.3568520) | Ling et al., SenSys 2022 | Groups CPU and GPU execution into analyzable duo-blocks for cross-processor real-time inference. | Static | CPU+GPU | deadlines | Duo-block grouping and scheduling | Latency, accuracy | Block / layer | Heuristic | - |
| [Time-Sensitive Multi-DNN Inference on CPU-GPU Edge Platforms](https://ieeexplore.ieee.org/abstract/document/11268909) | Ling et al., IEEE TMC 2026 | Uses duo-block structures and urgency-driven CPU–GPU scheduling for time-sensitive multi-DNN inference. | Static multi-DNN | CPU+GPU | deadlines, memory | Urgency-driven duo-block scheduling | Latency, accuracy | Block / layer | Heuristic | - |
| [NN-Stretch: Automatic Neural Network Branching for Parallel Inference on Heterogeneous Multi-Processors](https://dl.acm.org/doi/abs/10.1145/3581791.3596870) | Wei et al., MobiSys 2023 | Reshapes sequential networks into branches for parallel execution across CPU, GPU, and DSP resources. | Static | CPU+GPU+DSP | - | Network branching and parallel placement | Latency, energy, accuracy | Branch / layer | Optimization | [Code](https://github.com/caoting-dotcom/multiBranchModel) |
| [CARIn: Constraint-Aware and Responsive Inference on Heterogeneous Devices for Single- and Multi-DNN Workloads](https://dl.acm.org/doi/full/10.1145/3665868) | Panopoulos et al., ACM TECS 2024 | Selects model–processor configurations under SLO and resource constraints and switches configurations at runtime. | Static / multi-DNN | CPU+GPU+NPU | deadline, memory | Runtime configuration selection and switching | Latency, throughput, accuracy | Model configuration | Heuristic | - |
| [Klotski: DNN Model Orchestration Framework for Dataflow Architecture Accelerators](https://ieeexplore.ieee.org/abstract/document/10323893) | Bai et al., ICCAD 2023 | Orchestrates micro-operations on dataflow accelerators while decoupling scheduling from mapping. | Static | Dataflow accelerator / NPU | - | Micro-operation partitioning, scheduling, and mapping | Makespan, NoC overhead | Micro-operation | Optimization | - |
| [XSched: Preemptive Scheduling for Diverse XPUs](https://www.usenix.org/conference/osdi25/presentation/shen-weihang) | Shen et al., OSDI 2025 | Provides a preemptible command-queue abstraction and portable priority and bandwidth policies across diverse XPUs. | Static / multi-tenant | GPU+NPU+ASIC+FPGA | deadlines | Preemptible command-queue scheduling | Latency, throughput | Command queue | Heuristic | [Code](https://github.com/XpuOS/xsched) |
| [Fast On-Device LLM Inference with NPUs](https://dl.acm.org/doi/abs/10.1145/3669940.3707239) | Xu et al., ASPLOS 2025 | Offloads suitable prefill computation to NPUs while retaining unsupported or accuracy-sensitive operations on CPU/GPU. | Generative | CPU+GPU+NPU | memory capacity | NPU prefill offloading with CPU/GPU fallback | Latency, energy | Layer / operator | Heuristic | [Code](https://github.com/QingweiJi/PowerNPU) |
| [ARIA: Optimizing Vision Foundation Model Inference on Heterogeneous Mobile Processors for Augmented Reality](https://lovelyzzkei.github.io/assets/pdf/mobisys25-aria.pdf) | Jung et al., MobiSys 2025 | Coordinates GPU-based high-fidelity prediction with NPU-based low-latency updates for AR workloads. | Dynamic | CPU+GPU+NPU | latency | GPU/NPU stage coordination | Accuracy | Model / stage | Heuristic | - |
| [DICTAT: Time-Predictable Acceleration of Deep Neural Networks on FPGA-SoC Platforms](https://ieeexplore.ieee.org/abstract/document/9622368) | Restuccia et al., RTSS 2021 | Models Xilinx DPU execution and improves predictable instruction fetching for response-time analysis. | Static | CPU+FPGA | - | Predictable DPU instruction fetching | Latency, predictability | DPU instruction / layer | Analysis / optimization | - |
| [MIRIAM: Exploiting Elastic Kernels for Real-Time Multi-DNN Inference on Edge GPUs](https://dl.acm.org/doi/abs/10.1145/3625687.3625789) | Zhao et al., SenSys 2023 | Generates elastic GPU kernels and coordinates mixed-critical DNN kernels on edge GPUs. | Static multi-DNN | CPU+GPU | deadlines | Elastic-kernel generation and resizing | Latency, throughput | Kernel | Heuristic | - |
| [ODMDEF: On-Device Multi-DNN Execution Framework Utilizing Adaptive Layer Allocation on General-Purpose Cores and Accelerators](https://ieeexplore.ieee.org/abstract/document/9453793) | Lim et al., IEEE Access 2021 | Uses latency prediction to guide layer-level co-execution across general-purpose cores and accelerators. | Static multi-DNN | CPU+GPU+NPU | - | Adaptive layer allocation | Latency | Layer | Learning-assisted heuristic | - |
| [AxoNN: Energy-Aware Execution of Neural Network Inference on Multi-Accelerator Heterogeneous SoCs](https://dl.acm.org/doi/abs/10.1145/3489517.3530572) | Dagli et al., DAC 2022 | Maps layers across heterogeneous DSAs while modeling inter-accelerator transition costs for energy-performance targets. | Static | Heterogeneous DSA / SoC | - | Energy-aware DSA mapping | Latency, energy | Layer / subgraph | Optimization | - |
| [HaX-CoNN: Shared-Memory-Contention-Aware Concurrent DNN Execution for Diversely Heterogeneous Systems-on-Chip](https://dl.acm.org/doi/abs/10.1145/3627535.3638502) | Dagli et al., PPoPP 2024 | Incorporates shared-memory contention into concurrent DNN mapping on heterogeneous SoCs. | Static multi-DNN | CPU+GPU+NPU | - | Contention-aware concurrent mapping | Latency, throughput, memory | Layer / subgraph | Optimization | [Code](https://github.com/ismetdagli/HaX-CoNN) |
| [GCAPS: GPU Context-Aware Preemptive Priority-Based Scheduling for Real-Time Tasks](https://arxiv.org/abs/2406.05221) | Wang et al., arXiv 2024 | Introduces driver-level preemptive GPU scheduling to improve the schedulability of latency-sensitive tasks. | Static | CPU+GPU | deadlines | Driver-level GPU preemptive scheduling | Latency, predictability | Kernel / command | Heuristic | - |
| [DARIS: An Oversubscribed Spatio-Temporal Scheduler for Real-Time DNN Inference on GPUs](https://ieeexplore.ieee.org/abstract/document/11132423) | Babaei et al., DAC 2025 | Combines MPS/CUDA-stream sharing with staging-based temporal partitioning for soft real-time DNN inference. | Static multi-DNN | GPU | deadlines, memory | Staging-based temporal partitioning with spatial GPU sharing | Latency, throughput | Stage / GPU partition | Heuristic | - |

</details>

### 3.4 Processor-Frequency and Energy Orchestration

Processor-frequency and energy orchestration treats DVFS, batching, model configuration, and accelerator selection as coupled runtime controls. The objective is not merely to minimize energy, but to prevent thermal throttling and preserve timing behavior under sustained workloads.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Core Mechanism / Contribution | Workload (W) | Hardware (H) | Constraints (C) | Orchestration Action (O) | Objectives (G) | Granularity (Γ) | Decision Policy (Π) | Artifact |
|---|---|---|---|---|---|---|---|---|---|:---:|
| [zTT: Learning-Based DVFS with Zero Thermal Throttling for Mobile Devices](https://dl.acm.org/doi/abs/10.1145/3529706.3529714) | Kim et al., GetMobile 2022 | Learns thermal-aware DVFS decisions intended to avoid throttling during sustained mobile workloads. | Static | CPU+GPU | thermal limit | Learning-based DVFS | Latency, energy | Model / control interval | Learning-based | [Code](https://github.com/ztt-21/zTT) |
| [Coordinated Batching and DVFS for DNN Inference on GPU Accelerators](https://ieeexplore.ieee.org/abstract/document/9689937) | Nabavinejad et al., IEEE TPDS 2022 | Jointly adjusts batch size and GPU frequency for energy-efficient inference. | Static | GPU | latency SLO | Batch-size and GPU-frequency control | Throughput, energy | Batch | Heuristic | - |
| [A Workload-Aware DVFS Robust to Concurrent Tasks for Mobile Devices](https://dl.acm.org/doi/abs/10.1145/3570361.3592524) | Lin et al., MobiCom 2023 | Selects frequencies using workload characteristics to stabilize latency and energy under concurrent execution. | Dynamic | CPU+GPU | - | Workload-aware frequency selection | Latency, energy | Control interval | Control / profile-guided | [Code](https://github.com/geardvfs/GearDVFS) |
| [MOC: Multi-Objective Mobile CPU-GPU Co-Optimization for Power-Efficient DNN Inference](https://ieeexplore.ieee.org/abstract/document/10323882) | Wu et al., ICCAD 2023 | Jointly manages CPU–GPU configurations over latency, energy, and accuracy objectives. | Static | CPU+GPU | - | CPU-GPU configuration and frequency tuning | Latency, energy, accuracy | Model configuration | Learning-assisted optimization | - |
| [Thermal-Aware Scheduling for Deep Learning on Mobile Devices with NPU](https://ieeexplore.ieee.org/abstract/document/10478860) | Tan et al., IEEE TMC 2024 | Incorporates thermal state into scheduling across mobile processors and NPUs. | Static | GPU+NPU | thermal limit | Thermal-aware processor selection | Latency, accuracy | Model | Learning-assisted heuristic | - |
| [MapFormer: Attention-Based Multi-DNN Manager for Throughput and Power Co-Optimization on Embedded Devices](https://dl.acm.org/doi/abs/10.1145/3676536.3676724) | Karatzas et al., ICCAD 2024 | Combines a transformer-based predictor with search to partition concurrent DNNs and optimize allocation and power. | Static multi-DNN | GPU+NPU | power budget | DNN partitioning, allocation, and power control | Throughput | Layer / partition | Learning-assisted heuristic | - |
| [Tango: Low-Latency Multi-DNN Inference on Heterogeneous Edge Platforms](https://ieeexplore.ieee.org/abstract/document/10817997) | Taufique et al., ICCD 2024 | Uses a PPO agent to select compute clusters, accuracy configurations, and DVFS settings. | Dynamic multi-DNN | CPU+GPU | deadlines | Cluster selection, accuracy configuration, and DVFS | Latency, energy | Model configuration | Reinforcement learning | - |
| [NeuroBalancer: Balancing System Frequencies with Punctual Laziness for Timely and Energy-Efficient DNN Inferences](https://ieeexplore.ieee.org/abstract/document/10819653) | Bin et al., IEEE TMC 2025 | Coordinates CPU, GPU, and memory frequencies to exploit timing slack while preserving punctual execution. | Static | CPU+GPU | deadlines | CPU/GPU/memory frequency coordination | Latency, energy | System / control interval | Optimization | - |
| [E4: Energy-Efficient DNN Inference for Edge Video Analytics via Early Exiting and DVFS](https://ojs.aaai.org/index.php/AAAI/article/view/32104) | Zhang et al., AAAI 2025 | Couples early exits with DVFS so computation follows input difficulty and energy availability. | Dynamic | CPU+GPU | accuracy | Early-exit and DVFS coupling | Latency, energy | Exit / layer | Optimization | - |
| [Twill: Scheduling Compound AI Systems on Heterogeneous Mobile Edge Platforms](https://ieeexplore.ieee.org/abstract/document/11240767) | Taufique et al., ICCAD 2025 | Uses affinity-aware mapping, priority-driven freezing and unfreezing, and adaptive DVFS for compound AI pipelines. | Pipeline | CPU+GPU+NPU | energy budget | Affinity mapping, task freezing, migration, and DVFS | Latency | Stage / layer | Heuristic | - |

</details>

## 4. Runtime Decision-Making Strategies

The same hardware and orchestration mechanisms can be controlled by different policy families. This section therefore classifies systems by how decisions are produced rather than by which resource is managed.

### 4.1 Rule-Based and Priority-Driven Scheduling

Rule- and priority-driven systems use explicit urgency, priority, slack, affinity, or threshold rules. They are generally lightweight and interpretable, but their behavior depends on how well the predefined rules capture workload variation and interference.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Core Mechanism / Contribution | Workload (W) | Hardware (H) | Constraints (C) | Orchestration Action (O) | Objectives (G) | Granularity (Γ) | Decision Policy (Π) | Artifact |
|---|---|---|---|---|---|---|---|---|---|:---:|
| [LaLaRAND: Flexible Layer-by-Layer CPU/GPU Scheduling for Real-Time DNN Tasks](https://ieeexplore.ieee.org/abstract/document/9622325) | Kang et al., RTSS 2021 | Assigns priorities and processors at layer granularity while preserving analyzable timing behavior. | Static | CPU+GPU | deadlines | Layer-level priority and processor assignment | Latency, predictability | Layer | Priority heuristic | [Code](https://github.com/fredrickang/LaLaRAND) |
| [RT-LM: Uncertainty-Aware Resource Management for Real-Time Inference of Language Models](https://arxiv.org/abs/2309.06619) | Li et al., RTSS 2023 | Accounts for execution uncertainty when allocating resources to language-model inference. | Generative | GPU | deadlines | Uncertainty-aware model/resource selection | Latency | Model / request | Uncertainty-aware rule | - |
| [Partitioned Scheduling and Parallelism Assignment for Real-Time DNN Inference Tasks on Multi-TPU](https://dl.acm.org/doi/abs/10.1145/3649329.3655979) | Sun et al., DAC 2024 | Jointly selects task partitions and TPU-level parallelism under deadline constraints. | Static multi-DNN | TPU / NPU | deadlines | Partitioned scheduling and parallelism assignment | Latency, predictability | Layer / partition | Heuristic | - |
| [FLASH: Deadline-Aware Flexible LLC Arbitration and Scheduling for Hardware Accelerators](https://dl.acm.org/doi/full/10.1145/3757742) | Agarwal et al., ACM TECS 2025 | Arbitrates last-level-cache access among accelerators according to deadline urgency. | Static | CPU+GPU+NPU | deadlines | Deadline-aware LLC arbitration | Latency, throughput | Request / cache allocation | Priority heuristic | - |
| [SGPRS: Seamless GPU Partitioning Real-Time Scheduler for Periodic Deep Learning Workloads](https://ieeexplore.ieee.org/abstract/document/10546669) | Babaei et al., 2024 | Assigns stage priorities and virtual deadlines to support schedulability analysis for periodic DNN workloads. | Static periodic | GPU | deadlines | Stage priority scheduling with virtual deadlines | Latency, predictability | Stage | Heuristic | - |
| [TensorRT-Based Framework and Optimization Methodology for Deep Learning Inference on Jetson Boards](https://dl.acm.org/doi/full/10.1145/3508391) | Jeong et al., ACM TECS 2022 | Explores precision, engine, and deployment configurations for Jetson inference. | Static | CPU+GPU+NPU | latency | TensorRT deployment tuning | Throughput | Layer / engine configuration | Heuristic / deployment rule | [Code](https://github.com/cap-lab/jedi) |
| [IceEdge: Thermal-Aware Machine Learning Inference Serving for Emerging Edge Applications](https://dl.acm.org/doi/full/10.1145/3801093) | Shafi et al., ACM TOSN 2026 | Adapts batching and inference-serving decisions according to deadline and thermal state. | Dynamic | CPU+GPU+NPU | deadlines, thermal limit | Deadline batching and thermal checks | Latency, throughput, energy | Model / request | Feedback heuristic | - |
| [TinyMem: Boosting Multi-DNN Inference on Tiny AI Accelerators with Weight-Memory Virtualization](https://dl.acm.org/doi/abs/10.1145/3708468.3711888) | Jeon et al., HotMobile 2025 | Schedules weight residency using preserve, preload, and pack rules to reduce repeated loading. | Static multi-DNN | NPU | memory capacity | Weight-residency scheduling (preserve/preload/pack) | Throughput | Layer / weight block | Heuristic | - |
| [ARISE: High-Capacity AR Offloading Inference Serving via Proactive Scheduling](https://dl.acm.org/doi/abs/10.1145/3643832.3661894) | Kong et al., MobiSys 2024 | Predicts future inference demand and proactively provisions serving resources. | Dynamic | GPU | accuracy SLO | Proactive AR offloading scheduling | Throughput | Request / model | Predictive heuristic | - |
| [AdaKnife: Flexible DNN Offloading for Inference Acceleration on Heterogeneous Mobile Devices](https://ieeexplore.ieee.org/abstract/document/10700984) | Liu et al., IEEE TMC 2025 | Uses graph-based mixed-granularity partitioning and cross-framework offloading. | Static | CPU+GPU | resource capacity | Flexible DNN offloading | Latency | Subgraph / layer | Heuristic | - |
| [NetCut: Real-Time DNN Inference Using Layer Removal](https://ieeexplore.ieee.org/abstract/document/9474052) | Zandigohar et al., DATE 2021 | Constructs trimmed network variants and selects the variant that fits the deadline. | Dynamic | GPU | deadline | Deadline-aware model-variant selection with layer removal | Latency, accuracy | Model variant / layer | Threshold heuristic | - |

</details>

### 4.2 Optimization-Based Scheduling

Optimization-based schedulers explicitly formulate placement, partitioning, resource allocation, or configuration selection as mathematical programs or structured search problems. They provide a clear objective and constraint model, but online applicability depends on problem size, profiling accuracy, and solver overhead.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Core Mechanism / Contribution | Workload (W) | Hardware (H) | Constraints (C) | Orchestration Action (O) | Objectives (G) | Granularity (Γ) | Decision Policy (Π) | Artifact |
|---|---|---|---|---|---|---|---|---|---|:---:|
| [RT-mDL: Supporting Real-Time Mixed Deep Learning Tasks on Edge Platforms](https://dl.acm.org/doi/abs/10.1145/3485730.3485938) | Ling et al., SenSys 2021 | Jointly optimizes model scaling and task scheduling under storage and deadline constraints. | Static mixed tasks | CPU+GPU | deadlines, storage | Model scaling and priority assignment | Latency, accuracy | Model | Optimization | - |
| [SPET: Transparent SRAM Allocation and Model Partitioning for Real-Time DNN Tasks on Edge TPU](https://ieeexplore.ieee.org/abstract/document/10247661) | Han et al., DAC 2023 | Jointly optimizes SRAM allocation and model partitioning on Edge TPU. | Static | TPU / NPU | SRAM capacity | SRAM allocation and model partitioning | Latency, predictability | Layer / memory region | Optimization | - |
| [RT-MDM: Real-Time Scheduling Framework for Multi-DNN on MCU Using External Memory](https://dl.acm.org/doi/abs/10.1145/3649329.3655681) | Kang et al., DAC 2024 | Optimizes model segmentation and segment-group mapping to reduce external-memory accesses. | Static multi-DNN | MCU | memory capacity | Segmentation and external-memory scheduling | Latency, predictability | Segment / layer | Optimization | - |
| [DNN Inference Acceleration Based on Adaptive Task Partitioning and Offloading in Embedded VEC](https://dl.acm.org/doi/full/10.1145/3725734) | Li et al., ACM TECS 2025 | Optimizes task partitioning and offloading under dynamic network and resource conditions. | Dynamic | Device+edge | - | Adaptive task partitioning and offloading | Latency, energy | Layer / subgraph | Optimization | - |
| [SCENIC: Capability and Scheduling Co-Design for Intelligent Controllers on Heterogeneous Platforms](https://ieeexplore.ieee.org/abstract/document/10844810) | Chen et al., RTSS 2024 | Jointly optimizes resource allocation and controller-capability configurations. | Static / dynamic control | CPU+GPU | schedulability | Capability and scheduling co-design | Accuracy, predictability | Model / control configuration | Optimization | - |
| [EFFECT-DNN: Energy-Efficient Edge Framework for Real-Time DNN Inference](https://ieeexplore.ieee.org/abstract/document/10195765) | Zhang et al., WoWMoM 2023 | Uses Lyapunov optimization and convex allocation to reduce long-term device energy under latency constraints. | Dynamic | CPU+GPU | latency constraint | Cut-point and resource allocation | Latency, energy | Cut point / resource allocation | Lyapunov / convex optimization | - |
| [EdgeAdaptor: Online Configuration Adaptation, Model Selection, and Resource Provisioning for Edge DNN Inference Serving at Scale](https://ieeexplore.ieee.org/abstract/document/9817634) | Zhao et al., IEEE TMC 2022 | Uses convex relaxation and dependent randomized rounding for model selection and resource provisioning at scale. | Dynamic | Heterogeneous edge | latency SLO | Model selection and resource provisioning | Latency, accuracy | Model / request | Convex relaxation / randomized rounding | - |
| [Robust DNN Partitioning and Resource Allocation under Uncertain Inference Time](https://ieeexplore.ieee.org/abstract/document/11197924) | Nan et al., IEEE TMC 2026 | Uses chance constraints for partition, bandwidth, and frequency decisions when inference time is uncertain. | Static / uncertain | CPU+GPU | deadline probability | Partition, bandwidth, and frequency allocation | Energy, deadline probability | Layer / partition | Chance-constrained optimization | - |
| [EMERALD: Elastic Execution of Multi-Tenant DNNs on Heterogeneous Edge MPSoCs](https://ieeexplore.ieee.org/abstract/document/10817905) | Heidari et al., SEC 2024 | Separates fast local input-scale decisions from infrequent global ILP resource-allocation updates under scene variation. | Dynamic multi-tenant | CPU+GPU+NPU | deadlines | Input-scale adaptation with local/global allocation | Latency, accuracy | Input scale / allocation | Local heuristic + ILP | - |

</details>

### 4.3 Control-Theoretic Scheduling and Runtime Adaptation

Control-theoretic methods close the loop between measured runtime state and resource decisions. Their strength is continuous correction under disturbances; their limitation is that stability and responsiveness depend on an adequate system model and sensing interval.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Core Mechanism / Contribution | Workload (W) | Hardware (H) | Constraints (C) | Orchestration Action (O) | Objectives (G) | Granularity (Γ) | Decision Policy (Π) | Artifact |
|---|---|---|---|---|---|---|---|---|---|:---:|
| [QoS-EML: Towards QoS-Based Embedded Machine Learning](https://www.mdpi.com/2079-9292/11/19/3204) | Springer et al., Electronics 2022 | Adjusts compute resources in response to measured deviations from QoS targets. | Static / dynamic | CPU+GPU | QoS target | QoS and DVFS feedback control | Latency, energy | Model / control interval | Feedback control | - |
| [Future-Aware Dynamic Thermal Management in CPU-GPU Embedded Platforms](https://ieeexplore.ieee.org/abstract/document/9984747) | Maity et al., RTSS 2022 | Predicts temperature evolution and proactively adjusts CPU–GPU operation. | Static | CPU+GPU | thermal limit | Prediction-based CPU/GPU thermal mapping | Latency, energy | System / control interval | Predictive control | - |
| [FC-GPU: Feedback-Control GPU Scheduling for Real-Time Embedded Systems](https://dl.acm.org/doi/full/10.1145/3761812) | Subramaniyan et al., ACM TECS 2025 | Controls the response-time ratio by adjusting invocation rate under GPU contention. | Static | GPU | deadlines | Invocation-rate control under GPU contention | Latency | Invocation / control interval | Feedback control | - |
| [Budget RNNs: Multi-Capacity Neural Networks to Improve In-Sensor Inference under Energy Budgets](https://ieeexplore.ieee.org/abstract/document/9470487) | Kannan et al., RTAS 2021 | Regulates energy use through halting and capacity selection, with accuracy as the main risk. | Dynamic | MCU / CPU | energy budget | Energy-budget halting and capacity selection | Energy, accuracy | Halting point / model capacity | Budget-aware control | [Code](https://github.com/tejaskannan/budget-rnn) |
| [AccuMO: Accuracy-Centric Multitask Offloading in Edge-Assisted Mobile Augmented Reality](https://dl.acm.org/doi/abs/10.1145/3570361.3592531) | Kong et al., MobiCom 2023 | Regulates AR freshness through multitask offloading, with stale results as the main risk. | Dynamic | Mobile+edge GPU | AR freshness | Freshness-aware multitask offloading | Accuracy, freshness | Task / offloading decision | Model-based control | [Code](https://github.com/JonnyKong/AccuMO) |

</details>

### 4.4 Profile-Guided Decision Support and Learning-Driven Scheduling

Profile-guided systems rely on measured latency, energy, or interference models; learning-driven systems infer configurations or scheduling policies from data. Their key distinction is whether learning only predicts system behavior or directly produces the runtime decision.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Core Mechanism / Contribution | Workload (W) | Hardware (H) | Constraints (C) | Orchestration Action (O) | Objectives (G) | Granularity (Γ) | Decision Policy (Π) | Artifact |
|---|---|---|---|---|---|---|---|---|---|:---:|
| [nn-Meter: Towards Accurate Latency Prediction of Deep-Learning Model Inference on Diverse Edge Devices](https://dl.acm.org/doi/abs/10.1145/3458864.3467882) | Zhang et al., MobiSys 2021 | Learns operator-level latency predictors from profiling data across diverse edge devices. | Static | CPU+GPU+NPU | - | Latency-prediction support for scheduling | Latency prediction | Operator | Supervised learning (predictor) | [Code](https://github.com/microsoft/nn-Meter) |
| [Mediator: Characterizing and Optimizing Multi-DNN Inference for Energy-Efficient Edge Intelligence](https://ieeexplore.ieee.org/abstract/document/10763713) | Choi et al., IISWC 2024 | Characterizes inter-model interference and uses RL-guided decisions for placement, precision, and power state. | Pipeline / multi-DNN | CPU+GPU+NPU | deadlines | Interference characterization and RL-guided placement/precision/power decisions | Latency, accuracy, energy | Model | Profile-guided + RL | - |
| [A Performance-Prediction-Based DNN Partitioner for Edge TPU Pipelining](https://ieeexplore.ieee.org/abstract/document/10773756) | Zou et al., MILCOM 2024 | Predicts partition performance and selects TPU pipeline configurations before deployment. | Static / pipeline | TPU / NPU | memory | Prediction-based Edge TPU partitioning | Latency, throughput | Layer / partition | Learning-assisted heuristic | - |
| [Flex: Fast, Accurate DNN Inference on Low-Cost Edges Using Heterogeneous Accelerator Execution](https://dl.acm.org/doi/abs/10.1145/3689031.3696067) | Sen et al., EuroSys 2025 | Learns input-aware layer placement between CPU and low-cost accelerators. | Static | CPU+accelerator | deadlines | Input-aware layer placement | Latency, accuracy | Layer / input | Profile-guided / learning-assisted | - |
| [AdaDrone: Quality-of-Navigation-Based Neural Adaptive Scheduling for Edge-Assisted Drones](https://ieeexplore.ieee.org/abstract/document/9912195) | Chen et al., ICDCS 2022 | Adapts edge-assisted inference according to navigation-quality objectives. | Dynamic | Device+edge | deadlines | Quality-aware neural scheduling | Latency, accuracy | Model / task | Learning-based | - |
| [TapFinger: Task Placement and Fine-Grained Resource Allocation for Edge Machine Learning](https://ieeexplore.ieee.org/abstract/document/10229031) | Li et al., INFOCOM 2023 | Learns task placement and fine-grained resource allocation across distributed edge resources. | Dynamic | Distributed edge | - | Task placement and resource allocation | Throughput | Task | Learning-based | [Code](https://github.com/nooblyh/TapFinger) |
| [BCEdge: SLO-Aware DNN Inference Services with Adaptive Batch-Concurrent Scheduling on Edge Devices](https://ieeexplore.ieee.org/abstract/document/10549973) | Zhang et al., IEEE TNSM 2024 | Learns batch size, concurrent instances, and shared-memory configuration under SLO pressure. | Dynamic | CPU+GPU+NPU | SLO | DRL-guided batch and memory scheduling | Latency, throughput, memory | Batch / memory configuration | Deep reinforcement learning | - |
| [Reinforcement Learning-Based Edge-Assisted Inference with Multimodal Data](https://ieeexplore.ieee.org/abstract/document/11223761) | Liu et al., IEEE TMC 2025 | Uses reinforcement learning to coordinate multimodal inference across heterogeneous edge resources. | Dynamic / pipeline | CPU+GPU+NPU | - | RL-based edge-assisted inference | Latency, energy, accuracy | Task / modality | Reinforcement learning | [Code](https://github.com/Yucj7/MMCI) |
| [PRAS: Adaptive Scheduling of Online Inference Pipelines at the Edge: A Post-Hoc Request-Oriented Approach](https://www.sciencedirect.com/science/article/abs/pii/S1383762125002693) | Sun et al., JSA 2025 | Selects models and batches for queued pipeline requests using graph paths and multi-armed-bandit profiling. | Pipeline | GPU+NPU | latency | MAB-assisted pipeline and batch selection | Latency, accuracy | Request / model / batch | MAB-assisted online learning | - |
| [Uncertainty-Aware RL-Based Scheduling of Multi-DNN Workloads on Edge MPSoCs](https://dl.acm.org/doi/abs/10.1145/3769102.3770621) | Heidari et al., SEC 2025 | Learns GNN-based scheduling policies under workload and runtime uncertainty. | Pipeline / multi-DNN | CPU+GPU+NPU | uncertainty | Uncertainty-aware RL scheduling | Latency | Model / task | Reinforcement learning | - |

</details>

## 5. Design Tradeoffs and System Insights

The following sections do not introduce another independent taxonomy. Instead, they compare how systems balance competing goals and identify where improvements in one dimension create costs in another.

### 5.1 Granularity Tradeoffs: Control Precision vs. Management Overhead

Coarse-grained decisions reduce profiling, bookkeeping, and switching overhead, but offer limited control over interference and slack. Fine-grained decisions improve resource utilization and responsiveness, yet increase scheduling complexity, state tracking, and timing-analysis difficulty.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Tradeoff Exposed | Workload | Hardware | Granularity | Decision Policy | Artifact |
|---|---|---|---|---|---|---|:---:|
| [Enabling Latency-Sensitive DNN Inference via Joint Optimization of Model Surgery and Resource Allocation in Heterogeneous Edge](https://dl.acm.org/doi/abs/10.1145/3545008.3545071) | Huang et al., ICPP 2022 | Model restructuring offers low-overhead adaptation but provides coarser runtime control than operator scheduling. | Static | CPU+GPU | Model / layer | Optimization | - |
| [Throughput Maximization of Delay-Aware DNN Inference in Edge Computing by Exploring DNN Model Partitioning and Inference Parallelism](https://ieeexplore.ieee.org/abstract/document/9606540) | Li et al., IEEE TMC 2021 | Partition-level control increases pipeline parallelism but introduces partition and communication overhead. | Dynamic | CPU / edge | Layer / partition | Optimization | - |
| [PArtNNer: Platform-Agnostic Adaptive Edge-Cloud DNN Partitioning for Minimizing End-to-End Latency](https://dl.acm.org/doi/full/10.1145/3630266) | Ghosh et al., ACM TECS 2024 | Portable partitioning improves adaptability but must account for device and network variability. | Static / dynamic | Device+edge | Layer / partition | Heuristic | - |
| [Inferencing on Edge Devices: A Time- and Space-Aware Co-Scheduling Approach](https://dl.acm.org/doi/full/10.1145/3576197) | Pereira et al., ACM TODAES 2023 | Pass-level co-scheduling improves flexibility but expands memory state and scheduling complexity. | Static | GPU | Pass / kernel | Optimization | - |
| [AdaMEC: Towards a Context-Adaptive and Dynamically Combinable DNN Deployment Framework for Mobile Edge Computing](https://dl.acm.org/doi/full/10.1145/3630098) | Pang et al., ACM TOSN 2023 | Operator-level composition supports rich context adaptation at the cost of a larger profiling and search space. | Dynamic | CPU+GPU | Operator | Heuristic | - |
| [Crane: Inter-Layer Scheduling Framework for DNN Inference and Training Co-Support on Tiled Architecture](https://dl.acm.org/doi/full/10.1145/3725843.3756023) | Gong et al., MICRO 2025 | Inter-layer scheduling increases overlap and reuse but requires more detailed dependency management. | Static mixed inference/training | Tiled accelerator | Layer | Optimization | - |
| [Stream: Design-Space Exploration of Layer-Fused DNNs on Heterogeneous Dataflow Accelerators](https://ieeexplore.ieee.org/abstract/document/10713407) | Symons et al., IEEE TC 2024 | Layer fusion reduces data movement but constrains later scheduling flexibility. | Static | Dataflow accelerator | Fused layer / operator | Optimization | [Code](https://github.com/KULeuven-MICAS/stream) |
| [Real-Time, Work-Conserving GPU Scheduling for Concurrent DNN Inference](https://dl.acm.org/doi/full/10.1145/3768622) | Han et al., ACM TOCS 2025 | Kernel- or command-level reallocation improves utilization but increases bookkeeping and interference sensitivity. | Dynamic multi-DNN | GPU | Kernel / command | Heuristic | - |
| [Improving DNN Inference Throughput Using Practical, Per-Input Compute Adaptation](https://dl.acm.org/doi/abs/10.1145/3694715.3695978) | Iyer et al., SOSP 2024 | Per-input adaptation improves average efficiency while increasing runtime variability. | Dynamic | GPU | Request / input | Optimization | - |
| [ZeroSwap: Minimizing Swap Overhead for Real-Time Multi-DNN Inference via SSD-Based GPU Memory Extension](https://rtcl.dgist.ac.kr/index.php/zeroswap) | Kang et al., RTAS 2026 | Memory-block control expands feasible workloads but adds data-movement and scheduling overhead. | Static multi-DNN | CPU+GPU+SSD | Memory block / segment | Optimization | [Code](https://github.com/fredrickang/zeroswap_public/tree/main) |

</details>

**Insight.** No single granularity dominates across platforms. The practical design point is the finest unit for which execution cost, interference, and switching overhead remain predictable enough to support the intended timing requirement.

### 5.2 Compute--Memory Tradeoffs: Compute Throughput vs. Memory Efficiency

Memory-saving techniques often exchange storage for recomputation, data movement, serialization, or lower concurrency. Conversely, throughput-oriented fusion, batching, and parallel execution may increase activation residency and working-set size.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Tradeoff Exposed | Workload | Hardware | Decision Policy | Artifact |
|---|---|---|---|---|---|:---:|
| [Memory-Efficient DNN Training on Mobile Devices](https://dl.acm.org/doi/abs/10.1145/3498361.3539765) | Gim et al., MobiSys 2022 | Selective activation recomputation reduces peak memory at the cost of additional computation. | Static training | Mobile SoC | Heuristic | - |
| [Occamy: Memory-Efficient GPU Compiler for DNN Inference](https://ieeexplore.ieee.org/abstract/document/10247839) | Lee et al., DAC 2023 | Compiler transformations lower GPU memory use but may alter execution order and recomputation cost. | Static | GPU | Heuristic / compiler optimization | [Code](https://github.com/corelab-src/occamy) |
| [MIME: Adapting a Single Neural Network for Multi-Task Inference with Memory-Efficient Dynamic Pruning](https://dl.acm.org/doi/abs/10.1145/3489517.3530473) | Bhattacharjee et al., DAC 2022 | Dynamic pruning shares a model across tasks while balancing compute, memory, and accuracy. | Static multi-task | NPU / accelerator | Learning-based | - |
| [ParallelFusion: Towards Maximum Utilization of Mobile GPU for DNN Inference](https://dl.acm.org/doi/abs/10.1145/3469116.3470014) | Lee et al., EMDL 2021 | Parallel execution and fusion improve GPU utilization but increase concurrent memory residency. | Static | GPU | Heuristic | - |
| [ElasticRoom: Multi-Tenant DNN Inference Engine via Co-Design with Resource-Constrained Compilation and Strong Priority Scheduling](https://dl.acm.org/doi/abs/10.1145/3625549.3658654) | Ma et al., HPDC 2024 | Resource-constrained compilation bounds memory while priority scheduling preserves service responsiveness. | Static multi-tenant | GPU | Heuristic | - |
| [SEEB-GPU: Early-Exit-Aware Scheduling and Batching for Edge GPU Inference](https://dl.acm.org/doi/abs/10.1145/3769102.3772715) | Subramaniyan et al., SEC 2025 | Batching improves throughput but enlarges the activation working set for early-exit models. | Dynamic | GPU | Heuristic | - |
| [PASK: Cold-Start Mitigation for Inference with Proactive and Selective Kernel Loading on GPUs](https://ieeexplore.ieee.org/abstract/document/11132809/) | Huang et al., DAC 2025 | Proactive kernel loading reduces cold-start latency by reserving more memory for sporadic arrivals. | Dynamic (sporadic arrivals) | GPU | Heuristic | - |
| [H2O: Heterogeneity-Aware Hierarchical Orchestration for Memory-Efficient On-Device LLM Inference](https://ieeexplore.ieee.org/abstract/document/11224632) | Zeng et al., IEEE TMC 2026 | Hierarchical weight placement lowers the device footprint but increases transfers across memory levels. | Generative | Heterogeneous SoC | Heuristic + optimization | [Code](https://github.com/ccfeiker/H2O) |
| [RAMS: Runtime Adaptive Memory Scaling for Tiny Deep Learning on IoT Devices](https://ieeexplore.ieee.org/abstract/document/11341897) | Chu et al., IEEE TMC 2026 | Runtime memory virtualization improves capacity use but adds allocation and management overhead. | Static / dynamic | MCU | Heuristic | - |
| [Breaking the Edge: Enabling Efficient Neural Network Inference on Integrated Edge Devices](https://ieeexplore.ieee.org/abstract/document/10959707) | Zhang et al., IEEE TCC 2025 | Identifies performance loss from mismatched compute throughput, memory capacity, and bandwidth, motivating joint optimization. | Static | Integrated edge platform | Heuristic / system co-design | [Code](https://github.com/ChenyangZhang-cs/EdgeNN) |

</details>

**Insight.** Memory efficiency is not an isolated capacity problem. For real-time systems, the relevant objective is to minimize memory pressure while bounding the timing cost of recomputation, paging, and transfer.

### 5.3 Predictability--Adaptivity Tradeoffs: Guarantees vs. Flexibility

Predictable systems restrict execution and communication behavior so that timing can be bounded. Adaptive systems exploit workload, thermal, and input variation, but introduce state-dependent execution paths that complicate worst-case analysis.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Position in the Tradeoff | Workload | Hardware | Decision Policy | Artifact |
|---|---|---|---|---|---|:---:|
| [Real-Time Scheduling and Analysis of Processing Chains on Multi-Threaded Executors in ROS 2](https://ieeexplore.ieee.org/abstract/document/9984791) | Jiang et al., RTSS 2022 | Provides an analyzable executor model and end-to-end timing bounds. | Pipeline | CPU | Analytical scheduling | - |
| [End-to-End Timing Analysis in ROS 2](https://ieeexplore.ieee.org/abstract/document/9984789) | Teper et al., RTSS 2022 | Derives timing bounds across distributed ROS 2 processing chains. | Pipeline | CPU | Timing analysis | [Code](https://github.com/HarunTeper/ros2-end-to-end-distributed) |
| [Worst-Case Time Disparity Analysis of Message Synchronization in ROS](https://ieeexplore.ieee.org/abstract/document/9984711) | Li et al., RTSS 2022 | Bounds synchronization disparity across message streams. | Pipeline | CPU | Timing analysis | - |
| [SEAM: An Optimal Message Synchronizer in ROS with Well-Bounded Time Disparity](https://ieeexplore.ieee.org/abstract/document/10406013) | Sun et al., RTSS 2023 | Enforces bounded synchronization disparity through an optimized synchronizer. | Pipeline | CPU | Optimization | [Code](https://github.com/tianyiWangGithub/seam_synchronizer) |
| [Prophet: Realizing a Predictable Real-Time Perception Pipeline for Autonomous Vehicles](https://ieeexplore.ieee.org/abstract/document/9984807) | Liu et al., RTSS 2022 | Prioritizes bounded execution in a perception pipeline. | Pipeline | GPU | Heuristic / analysis | - |
| [CPT: A Configurable Predictability Testbed for DNN Inference in Autonomous Vehicles](https://ieeexplore.ieee.org/abstract/document/10676407) | Liu et al., Tsinghua Science and Technology 2024 | Quantifies the performance cost of predictability configurations. | Pipeline | GPU | Experimental framework | - |
| [RT-BEV: Enhancing Real-Time BEV Perception for Autonomous Vehicles](https://ieeexplore.ieee.org/abstract/document/10844833) | Liu et al., RTSS 2024 | Optimizes BEV perception while retaining timing awareness. | Pipeline | GPU | Heuristic | [Code](https://github.com/Torreskai0722/RT-BEV) |
| [Jigsaw: Taming BEV-Centric Perception on Dual-SoC for Autonomous Driving](https://ieeexplore.ieee.org/abstract/document/10844764) | Sun et al., RTSS 2024 | Coordinates BEV perception across two SoCs, balancing fixed structure with platform-level flexibility. | Pipeline | Dual SoC | Heuristic | - |
| [Enabling Low-Latency Edge Intelligence Based on Multi-Exit DNNs in the Wild](https://ieeexplore.ieee.org/abstract/document/9546491) | Huang et al., ICDCS 2021 | Dynamically selects inference depth, improving average latency while increasing execution-time variation. | Dynamic | CPU | Optimization | - |
| [LOTUS: Learning-Based Online Thermal and Latency Variation Management for Two-Stage Detectors on Edge Devices](https://dl.acm.org/doi/abs/10.1145/3649329.3657310) | Gong et al., DAC 2024 | Learns online responses to latency and thermal variation, with distribution-shift risk. | Dynamic | Heterogeneous SoC | Learning-based | [Code](https://github.com/wuyushuwys/LOTUS/tree/master) |
| [FLEX: Adaptive Task Batch Scheduling with Elastic Fusion in Multi-Modal Multi-View Machine Perception](https://ieeexplore.ieee.org/abstract/document/10844787) | Xu et al., RTSS 2024 | Uses adaptive batching and elastic fusion, improving efficiency at the cost of a larger dynamic state space. | Pipeline | Heterogeneous SoC | Heuristic | - |
| [Dělen: Enabling Flexible and Adaptive Model Serving for Multi-Tenant Edge AI](https://dl.acm.org/doi/abs/10.1145/3576842.3582375) | Liang et al., IoTDI 2023 | Dynamically reallocates resources and adapts model serving for multi-tenant workloads. | Dynamic | Heterogeneous SoC | Heuristic | - |

</details>

**Insight.** Adaptation is compatible with real-time guarantees only when the adaptation space itself is bounded and analyzable. A promising direction is to certify a finite set of runtime modes and allow online switching only among verified configurations.

### 5.4 Isolation--Sharing Tradeoffs: Isolation Guarantees vs. Resource Efficiency

Isolation reduces interference and simplifies compositional reasoning, but static partitions may leave accelerators and memory underutilized. Sharing improves utilization but requires interference-aware scheduling, resource accounting, and enforcement.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Tradeoff Exposed | Workload | Hardware | Decision Policy | Artifact |
|---|---|---|---|---|---|:---:|
| [AegisDNN: Dependable and Timely Execution of DNN Tasks with SGX](https://ieeexplore.ieee.org/abstract/document/9622390) | Xiang et al., RTSS 2021 | Trusted execution improves isolation and dependability but adds protected-memory and execution overhead. | Static | CPU / SGX platform | Optimization | - |
| [ROSGM: A Real-Time GPU Management Framework with Plug-In Policies for ROS 2](https://ieeexplore.ieee.org/abstract/document/10155690) | Li et al., RTAS 2023 | Provides policy-level GPU access control for ROS 2 pipelines. | Static / pipeline | CPU+GPU | Policy plug-ins | - |
| [Hardware Compute Partitioning on NVIDIA GPUs for Composable Systems](https://par.nsf.gov/biblio/10652973) | Bakita et al., ECRTS 2025 | Uses spatial GPU partitions to improve composability while limiting cross-partition sharing. | Static | GPU | Partitioning heuristic | [Code](https://github.com/tanzelin430/libsmctrl) |
| [Real-Time Task Mapping for CPU-GPU Heterogeneous Platforms: Spatial GPU Partitioning and Utilization Bound](https://dl.acm.org/doi/abs/10.1145/3803800) | Han et al., ACM TECS 2026 | Analyzes the utilization loss introduced by spatial GPU partitioning. | Static | CPU+GPU | Analytical heuristic | - |
| [SPLIT: QoS-Aware DNN Inference on Shared GPU via Evenly Sized Model Splitting](https://dl.acm.org/doi/abs/10.1145/3605573.3605627) | Luo et al., ICPP 2023 | Splits models into balanced units for interference-aware GPU sharing. | Dynamic multi-DNN | GPU | Heuristic | [Code](https://github.com/Arantir1028/SPLIT) |
| [IasRT: Interference-Aware and SLO-Driven GPU Scheduling for Real-Time DNN Inference](https://ieeexplore.ieee.org/abstract/document/11311099) | Zhong et al., ICCD 2025 | Models cross-workload interference and schedules shared GPU resources according to SLOs. | Dynamic multi-DNN | GPU | Heuristic | - |
| [SGDRC: Software-Defined Dynamic Resource Control for Concurrent DNN Inference on NVIDIA GPUs](https://dl.acm.org/doi/abs/10.1145/3710848.3710863) | Zhang et al., PPoPP 2025 | Dynamically controls GPU shares in software, improving adaptability while weakening static isolation. | Dynamic multi-DNN | GPU | Software-defined dynamic control (heuristic) | - |
| [Analysis and Mitigation of Shared-Resource Contention on Heterogeneous Multicore: An Industrial Case Study](https://ieeexplore.ieee.org/abstract/document/10494679) | Bechtel et al., IEEE TC 2024 | Demonstrates how uncontrolled shared-resource contention degrades temporal behavior. | Pipeline | Heterogeneous multicore | Analysis and mitigation | - |
| [Fat Block: Narrowing the Boundary of pWCET Analysis on GPU](https://dl.acm.org/doi/full/10.1145/3807211.3807227) | Zheng et al., ADIST 2026 | Models GPU interference to reduce pessimism in probabilistic WCET analysis. | Static | GPU | Analytical method | - |

</details>

**Insight.** The relevant alternative is not simply isolation or sharing. Efficient real-time systems need **enforced sharing**: resources are shared when slack exists, but interference remains measurable, bounded, and revocable.

### 5.5 Latency--Energy--Accuracy Tradeoffs: Multi-Objective Tension

Embedded AI runtimes often cannot optimize latency, energy, and accuracy independently. Early exits, approximate execution, model scaling, sparsification, and offloading move the operating point along a Pareto frontier rather than improving every metric simultaneously.

<details>
<summary><b>Representative systems</b></summary>

| Title | Author / Venue / Year | Main Control | Workload | Hardware | Decision Policy | Artifact |
|---|---|---|---|---|---|:---:|
| [EdgeBERT: Sentence-Level Energy Optimizations for Latency-Aware Multi-Task NLP Inference](https://dl.acm.org/doi/abs/10.1145/3466752.3480095) | Tambe et al., MICRO 2021 | Sentence-level adaptation under latency constraints. | Dynamic NLP | NPU / accelerator | Heuristic | [Code](https://github.com/AYUSHMIT/EdgeBERT) |
| [Fast-Inf: Ultra-Fast Embedded Intelligence on the Batteryless Edge](https://dl.acm.org/doi/abs/10.1145/3666025.3699335) | Custode et al., SenSys 2024 | Lightweight inference pipelines under intermittent energy. | Static | MCU | Heuristic | [Code](https://github.com/DIOL-UniTN/Fast-Inf-FFF) |
| [Multi-Exit DNN Inference Acceleration Based on Multi-Dimensional Optimization for Edge Intelligence](https://ieeexplore.ieee.org/abstract/document/9769868) | Dong et al., IEEE TMC 2022 | Dynamic exit-point selection. | Dynamic | Heterogeneous SoC | Optimization | - |
| [DNN Surgery: Accelerating DNN Inference on the Edge through Layer Partitioning](https://ieeexplore.ieee.org/abstract/document/10076802) | Liang et al., IEEE TCC 2023 | Adaptive partitioning and structural simplification. | Static / dynamic | GPU | Optimization | - |
| [Panopticus: Omnidirectional 3D Object Detection on Resource-Constrained Edge Devices](https://dl.acm.org/doi/abs/10.1145/3636534.3690688) | Lee et al., MobiCom 2024 | Multi-view branch selection for semantic-quality-aware real-time 3D perception. | Dynamic perception (multi-view) | Heterogeneous SoC | Optimization | - |
| [Energy-Efficient Approximate Edge Inference Systems](https://dl.acm.org/doi/full/10.1145/3589766) | Ghosh et al., ACM TECS 2023 | Approximate execution with bounded accuracy degradation. | Static | Edge platform | Optimization | - |
| [CANNON: Communication-Aware Sparse Neural Network Optimization](https://ieeexplore.ieee.org/abstract/document/10171170) | Goksoy et al., IEEE TETC 2023 | Communication-aware sparsification; arithmetic savings can shift cost to communication. | Static / distributed | Accelerator cluster | Heuristic | - |
| [Energy-Efficient and Accuracy-Aware DNN Inference with IoT Device–Edge Collaboration](https://ieeexplore.ieee.org/abstract/document/10858448) | Jiang et al., IEEE TSC 2025 | Jointly optimizes collaborative execution, energy, and prediction quality. | Dynamic | Device+edge | Optimization | - |
| [Exploring the Boundaries of On-Device Inference: When Tiny Falls Short, Go Hierarchical](https://ieeexplore.ieee.org/abstract/document/11066245) | Behera et al., IEEE IoT Journal 2025 | Uses hierarchical inference and adaptive offloading across latency, energy, and accuracy requirements. | Dynamic | Device+edge | Heuristic | - |

</details>

**Insight.** Multi-objective runtime management should expose application-level utility rather than hard-code one universal metric weighting. The runtime must also distinguish reversible controls, such as DVFS, from quality-changing controls, such as early exit or layer removal.

## 6. Open Challenges and Future Directions

The papers below are not presented as completed solutions to the corresponding future direction. They provide partial mechanisms or adjacent foundations from which broader embedded-AI runtime capabilities may emerge.

### 6.1 LLM-Aware Runtime Systems

On-device LLM systems have begun to expose phase-specific execution, including prefill, decode, speculative verification, adapter routing, and edge-cloud collaboration. Real-time orchestration remains less developed when generative and conventional perception models share accelerators, memory, and energy budgets. Future systems must jointly manage prefill–decode asymmetry, token-level execution variability, KV-cache growth, and interference with periodic workloads.

| Title | Author / Venue / Year | Relevant Mechanism | Artifact |
|---|---|---|:---:|
| [MobileLLM: Optimizing Sub-Billion-Parameter Language Models for On-Device Use Cases](https://openreview.net/forum?id=EIGbXbxcUQ) | Liu et al., ICML 2024 | Shows model-structure choices whose layer depth, attention pattern, operator mix, and weight sharing affect runtime scheduling. | [Code](https://github.com/facebookresearch/MobileLLM) |
| [Fast On-Device LLM Inference with NPUs](https://dl.acm.org/doi/abs/10.1145/3669940.3707239) | Xu et al., ASPLOS 2025 | Offloads suitable prefill subgraphs to mobile NPUs while handling prompt length variation, outliers, and unsupported operators on CPU/GPU. | [Code](https://github.com/QingweiJi/PowerNPU) |
| [EdgeLLM: Fast On-Device LLM Inference with Speculative Decoding](https://ieeexplore.ieee.org/abstract/document/10812936) | Xu et al., IEEE TMC 2025 | Uses width-adaptive token-tree generation, batched verification, fallback, and provisional generation for speculative decoding. | - |
| [CLONE: Customizing LLMs for Efficient Latency-Aware Inference at the Edge](https://www.usenix.org/conference/atc25/presentation/tian) | Tian et al., USENIX ATC 2025 | Couples device-specific model tailoring, LoRA adapters, request-wise MoE routing, and layer-boundary DVFS. | [Code](https://github.com/qxpBlog/CLONE) |
| [Hybrid SLM and LLM for Edge–Cloud Collaborative Inference](https://dl.acm.org/doi/abs/10.1145/3662006.3662067) | Hao et al., MobiSys 2024 | Uses an edge SLM and selective cloud LLM verification or correction as an edge-cloud orchestration problem. | - |

### 6.2 Memory-Centric Resource Management

Future memory managers must coordinate model weights, activations, persistent context, and KV caches across accelerator memory, DRAM, flash, and remote storage. The open problem is to decide when to retain, evict, transfer, compress, or recompute data while preserving token latency and deadline behavior.

| Title | Author / Venue / Year | Relevant Mechanism | Artifact |
|---|---|---|:---:|
| [LLM in a Flash: Efficient Large Language Model Inference with Limited Memory](https://aclanthology.org/2024.acl-long.678.pdf) | Alizadeh et al., ACL 2024 | Uses flash-aware windowing and row–column bundling to execute models that exceed DRAM capacity. | - |
| [POET: Training Neural Networks on Tiny Devices with Integrated Rematerialization and Paging](https://proceedings.mlr.press/v162/patil22b.html) | Patil et al., ICML 2022 | Jointly optimizes rematerialization and paging for memory-constrained training. | [Code](https://github.com/ShishirPatil/poet) |
| [MNN-LLM: A Generic Inference Engine for Fast Large Language Model Deployment on Mobile Devices](https://dl.acm.org/doi/full/10.1145/3700410.3702126) | Wang et al., MMAsia 2024 | Combines quantization, DRAM–flash storage, and hardware-specific operator optimization. | [Code](https://github.com/alibaba/MNN/blob/master/project/android/apps/MnnLlmApp/README_CN.md) |
| [m^2LLM: A Multi-Dimensional Optimization Framework for LLM Inference on Mobile Devices](https://ieeexplore.ieee.org/abstract/document/11075620) | Liu et al., IEEE TPDS 2025 | Optimizes model customization, chunk-wise pipelining, prompt compression, and layer-wise resource scheduling. | [Code](https://github.com/UbiquitousLearning/mLLM) |
| [KVPR: Efficient LLM Inference with I/O-Aware KV-Cache Partial Recomputation](https://aclanthology.org/2025.findings-acl.997/) | Jiang et al., Findings of ACL 2025 | Overlaps partial KV recomputation with host–device KV transfer. | [Code](https://github.com/chaoyij/KVPR) |
| [Kelle: Co-Designing KV Caching and eDRAM for Efficient LLM Serving in Edge Computing](https://dl.acm.org/doi/full/10.1145/3725843.3756071) | Xia et al., MICRO 2025 | Co-designs eDRAM caching, eviction, recomputation, refresh control, and specialized acceleration. | - |

### 6.3 AI-Native Runtime Systems

AI-native runtimes use learned models not only to predict execution cost but also to synthesize or adapt scheduling policies. The main challenge is to combine learning-based decisions with bounded overhead, safe fallback modes, distribution-shift detection, and interpretable constraints.

| Title | Author / Venue / Year | Relevant Mechanism | Artifact |
|---|---|---|:---:|
| [EdgeML: An AutoML Framework for Real-Time Deep Learning on the Edge](https://dl.acm.org/doi/abs/10.1145/3450268.3453520) | Zhao et al., IoTDI 2021 | Searches model-system configurations under real-time constraints for edge AI deployment. | [Code](https://github.com/Kyrie-Zhao/EdgeML) |
| [Automated Runtime-Aware Scheduling for Multi-Tenant DNN Inference on GPU](https://ieeexplore.ieee.org/abstract/document/9643501) | Yu et al., ICCAD 2021 | Uses runtime performance information to update multi-tenant DNN scheduling decisions. | - |
| [EdgeTimer: Adaptive Multi-Timescale Scheduling in Mobile Edge Computing with Deep Reinforcement Learning](https://ieeexplore.ieee.org/abstract/document/10621305) | Hao et al., INFOCOM 2024 | Provides a transferable multi-timescale learning method for coordinating fast dispatch and longer-term resource decisions. | - |

### 6.4 Timing-Assured and Explainable Orchestration

Future embedded-AI runtimes need stronger links among resource orchestration, timing verification, control safety, and runtime assurance. The goal is not to classify all adjacent CPS work as AI scheduling, but to identify methods that could support certified operating modes, verified switching, deterministic accelerators, and explainable constraint violations.

| Title | Author / Venue / Year | Relevance to Future AI Runtimes | Artifact |
|---|---|---|:---:|
| [DECNTR: Optimizing Safety and Schedulability with Multi-Mode Control and Resource-Allocation Co-Design](https://ieeexplore.ieee.org/abstract/document/10568070) | Gifford et al., RTAS 2024 | Demonstrates co-design of switchable controllers, periods, and resources under schedulability and safety requirements. | - |
| [Integrated Real-Time Control and Scheduling for Safety-Critical Cyber-Physical Systems](https://ieeexplore.ieee.org/abstract/document/11018773) | Sudvarg et al., RTAS 2025 | Optimizes controller frequencies online while maintaining control and schedulability constraints. | - |
| [Deadline-Safe Reach-Avoid Control Synthesis for Cyber-Physical Systems with Reinforcement Learning](https://ieeexplore.ieee.org/abstract/document/10844742) | Liu et al., RTSS 2024 | Integrates deadline-aware constraints into reinforcement-learning-based reach–avoid control. | - |
| [Optimal Runtime Assurance via Reinforcement Learning](https://www.computer.org/csdl/proceedings-article/iccps/2024/692700a067/1YdXxNJMt6E) | Miller et al., ICCPS 2024 | Synthesizes performance-oriented runtime-assurance policies with safety constraints. | - |
| [Recovery-Guaranteed Sensor-Attack Detection for Cyber-Physical Systems](https://ieeexplore.ieee.org/abstract/document/11018669) | Xu et al., RTAS 2025 | Shows how detection thresholds can be adapted while retaining recovery guarantees. | - |
| [Accelerating Timing-Specification Verification of Interrupt-Driven Real-Time Systems](https://ieeexplore.ieee.org/abstract/document/11315067) | Shi et al., RTSS 2025 | Uses abstraction and first-order logic to accelerate timing verification. | - |
| [DERCA: Deterministic Cycle-Level Accelerator on Reconfigurable Platforms in DNN-Enabled Real-Time Systems](https://ieeexplore.ieee.org/document/11315054) | Ji et al., RTSS 2025 | Provides cycle-deterministic, intra-layer preemptive accelerator execution. | [Code](https://github.com/arc-research-lab/DERCA) |

## Citation

The paper is currently under preparation. The citation will be updated after a public preprint or final publication becomes available.

```bibtex
@article{huang2026realtimeembeddedai,
  author  = {Huang, Jing and Deng, Zihao and Gu, Zonghua},
  title   = {Real-Time Embedded AI Systems: Scheduling and Resource Orchestration for Multi-DNN Edge Intelligence},
  journal = {Manuscript under preparation},
  year    = {2026}
}
```

## Contributing

Contributions are welcome. You may:

- open an issue to report missing papers, incorrect metadata, broken links, or classification errors;
- submit a pull request to add papers, artifacts, benchmarks, or concise corrections; or
- propose a new category when existing dimensions do not adequately represent an emerging runtime mechanism.

When adding a paper, please provide:

1. the official paper title and publication link;
2. authors, venue, and year;
3. a one-sentence description of the core runtime mechanism;
4. workload, hardware, constraint, orchestration-action, objective, granularity, and policy labels; and
5. an artifact link when publicly available.

## Maintenance Notes

- Prefer the final conference or journal version over a preprint when both are available.
- Use official project, publisher, or author links whenever possible.
- Distinguish measured low latency from analytical or enforced real-time guarantees.
- Mark learning as a supporting technique when it only predicts performance and does not directly produce the scheduling decision.
- Keep summaries factual and limited to the contribution demonstrated by the cited paper.

Last updated: **June 2026**.

## License

The repository metadata and original summaries may be released under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/). Copyright for linked papers, code, and project materials remains with their respective authors and publishers.

