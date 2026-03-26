# A Deep Dive into VLA Models: Architecture, Edge Deployment, and Optimization
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/yourusername/awesome-VLA-inference/pulls)

<p align="center">
  <img src="./assets/vla01.gif" width="500px">
</p>

## 🔥🔥🔥 Vision-Language-Action Models: Inference & Optimization
Vision-Language-Action (VLA) models have emerged as the foundational brains for embodied agents and robotics. However, deploying these massive models in real-time robotic control systems introduces severe latency and computational bottlenecks. 

This repository provides a comprehensive and up-to-date collection of papers, frameworks, and resources focused on **VLA Model Inference Optimization**. We hope this resource serves as a valuable reference for both the Embodied AI and System/SysML research communities.

*If you are interested in this project, you can contribute to this repo by pulling requests 😊😊😊*

*If you find this repository helpful, please consider citing our related survey (Replace with your own BibTeX later):*
```bibtex
@misc{awesome-vla-inference,
  author = {Jia Luo}, 
  title = {Awesome-VLA-Inference: A Comprehensive Survey on Vision-Language-Action Model Inference Optimization},
  year = {2026},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/gacn2890356890-rgb/VLA/tree/main/Awesome-VLA-Inference}}
}
```

📢 News
🚀 What's New in This Update :


# 🌈 Table of Contents

- [Introduction to the VLA Paradigm](#introduction-to-the-vla-paradigm)
    - [The Challenge of Action Tokenization](#the-challenge-of-action-tokenization)
    - [Evolution of Classic VLA Models](#evolution-of-classic-vla-models)
- [Deep Dive into the Hierarchical Dual-System Architecture](#deep-dive-into-the-hierarchical-dual-system-architecture)
    - [Dual-System Architecture Core Elements](#dual-system-architecture-core-elements)
- [Edge Deployment Bottlenecks and Optimization Strategies](#edge-deployment-bottlenecks-and-optimization-strategies)
    - [Mainstream Acceleration and Compression Techniques](#mainstream-acceleration-and-compression-techniques)
    - [Borrowing from LLM Acceleration](#borrowing-from-llm-acceleration)
- [Industry Trends and Strategic Conclusions](#industry-trends-and-strategic-conclusions)
    - [Final Outlook](#final-outlook)
- [Awesome Papers](#awesome-papers)


## Introduction to the VLA Paradigm

The field of Embodied Artificial Intelligence is undergoing a profound transformation, driven largely by the evolution of Vision-Language-Action (VLA) models. Initially catalyzed by breakthroughs like **Robotics Transformer 2 (RT-2)**, which demonstrated the power of co-fine-tuning vision-language models with robotic trajectory data, VLAs have become the standard for translating abstract semantic goals into concrete physical movements.

At its core, the standard **VLA Pipeline** operates by transforming multimodal inputs into a unified token space:

- **Vision:** Vision Inputs -> Vision Transformer / SigLIP -> Vision Tokens
- **Text:** Natural Language Instruction -> Text Transformer -> Text Tokens
- **Execution:** Prefix Tokens -> LLM Backbones -> Autoregressive Generation

The fundamental mathematical formulation governing this autoregressive action generation is:

$$a_j = \arg\max_{a_j} P(a_j | a_{0:j-1}, O, P, W)$$

Where $O$ represents multi-view visual observations, $P$ represents proprioception (e.g., robotic arm end-effector pose, gripper open/close state), and $W$ represents language instructions.

### The Challenge of Action Tokenization

A significant hurdle in early VLA development was converting LLM outputs into a probability distribution for discrete physical vocabularies. Early models relied on direct discretization (e.g., 256 Bins), which severely lacked the precision required for dexterous manipulation. This limitation paved the way for a new generation of sophisticated models.

### Evolution of Classic VLA Models

- **Octo (2024):** Introduced memory-augmented Transformers. Trained on four million trajectories from the Open X-Embodiment dataset, it championed multi-embodiment zero-shot generalization.
- **OpenVLA (2024):** The first open-source 7B-scale VLA model (built on Llama-2 and SigLIP), surpassing closed-source baselines in multi-task processing and offering a robust LoRA fine-tuning ecosystem.
- **RoboMamba (2024):** Bypassed traditional Transformer attention bottlenecks by utilizing the Mamba State Space Model (SSM), enabling ultra-long context reasoning with linear computational complexity.
- **3D-VLA (2024):** Pioneered generative world models by predicting actions alongside 3D point clouds and scene representations of future frames, deepening physical dynamic understanding.
- **$\pi_0$ (Pi-0) (2024):** Abandoned discrete autoregressive generation for Flow Matching, realizing high-frequency, smooth, and continuous action trajectory generation.

------

## Deep Dive into the Hierarchical Dual-System Architecture

Drawing inspiration from Cognitive Psychology's Dual Process Theory, modern embodied control is increasingly decoupled into two distinct cognitive layers:

- **System 1 (Intuitive Cerebellum / Fast Thinking):** Composed of lightweight Convolutional Neural Networks, Diffusion Policies, or MLPs. It handles high-frequency, reactive tasks.
- **System 2 (Deliberative Brain / Slow Thinking):** Powered by large-scale Vision-Language Models (e.g., LLaVA, Qwen-VL). It processes abstract instructions, breaks down logic, and performs spatial topology planning. Operating at low frequencies (0.5Hz–5Hz), it intervenes only during significant state changes.

While early research cascaded these systems physically—resulting in severe bandwidth limits—recent top-tier research fuses them into a single, end-to-end architecture:

- **Fast-in-Slow (FiS-VLA):** Nests System 1 inside System 2. System 2 provides continuous "Latent Conditions," allowing System 1 to perform real-time Action Chunking at up to 117.7Hz.
- **DP-VLA & HAMSTER:** Utilize VLMs for low-frequency semantic decisions while relying on lightweight networks to decompose high-order tasks into low-order Motor Primitives, ensuring fast and interpretable execution.

### Dual-System Architecture Core Elements

| **Feature**            | **System 2 (Slow / Deliberative)**                           | **System 1 (Fast / Intuitive)**                              |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Responsibilities**   | Semantic understanding, common-sense reasoning, goal decomposition | Real-time kinematic control, obstacle avoidance, smooth trajectory generation |
| **Typical Algorithms** | Large Multimodal Transformers (OpenVLA, LLaVA)               | CNN, DiT, State-Space Models                                 |
| **Frequency**          | Ultra-low frequency (0.5Hz - 5Hz)                            | High frequency (50Hz - 500Hz)                                |
| **Inputs**             | Global high-res images, long text sequences                  | Cropped image streams, high-freq joint states                |

------

## Edge Deployment Bottlenecks and Optimization Strategies

Deploying massive VLA models on physical robots encounters the "Impossible Triangle" of Compute, Memory, and Power. Edge devices (often limited to 4GB–8GB RAM) face severe Memory Bandwidth Walls, exacerbated by two primary issues:

1. **The Latency Disaster:** Autoregressive decoding of dozens of continuous tokens per action causes fatal accumulated latency in closed-loop systems.
2. **KV Cache Explosion:** Multi-view, multi-frame video inputs cause KV Cache memory to grow linearly or quadratically, exhausting VRAM and starving compute units.

### Mainstream Acceleration and Compression Techniques

To bridge the gap between cloud-level intelligence and edge-level hardware constraints, the industry has adopted several rigorous optimization strategies:

- **Kinematic-Rectified Speculative Decoding (KERV):** Mitigates "token selection ambiguity" by using small draft models and large verification models. KERV introduces Kalman Filters to dynamically adjust draft acceptance thresholds based strictly on physical kinematic constraints.
- **Action-Aware Pruning & Extreme Quantization:** Standard compression destroys spatial geometry. Models like **CogVLA** and **SP-VLA** dynamically prune background tokens based on semantic relevance and robotic velocity. **BitVLA** and **SQAP-VLA** push boundaries with 1-bit quantization and Hadamard transforms to smooth feature distributions under extreme compression.
- **Flow Matching & Tokenless Paradigms:** Models like **FLOWER** bypass autoregression entirely. Using an 18-layer Flow Transformer (under 1B parameters), it samples directly in continuous action spaces, eliminating tokenization precision loss and achieving ultra-low latency.

### Borrowing from LLM Acceleration

Techniques originating in generative text are being successfully adapted for robotics:

| **Transfer Technology**  | **VLA Modification & Application**                           | **Estimated Impact**                      |
| ------------------------ | ------------------------------------------------------------ | ----------------------------------------- |
| **FlashAttention & GQA** | Efficient attention for multi-camera high-res inputs         | >70% reduction in VRAM read/write latency |
| **Adaptive KV Cache**    | Retains static background frames; recalculates only dynamic objects | 2x-4x speedup, sharp drop in VRAM usage   |
| **MoE Dynamic Routing**  | Dynamically drops redundant background visual tokens         | >50% reduction in activation parameters   |

------

## Industry Trends and Strategic Conclusions

The VLA field has officially crossed the "pioneering phase" of crafting single-task demonstration videos. The current engineering battlefield is focused entirely on **Cross-Embodiment generalization** and **Zero-Shot physical interactions**.

Two major paradigm shifts are defining the future of Embodied AI:

1. **The Shift to Embodied Reinforcement Learning:** Pure Behavior Cloning (imitation learning) leads to out-of-distribution error accumulation. The industry is rapidly moving toward RLHF (Reinforcement Learning from Human Feedback) within generative World Models. Frameworks like **$\pi_{0.6}$** utilize interactive sandboxes for massive trial-and-error, establishing industry-leading self-correction capabilities.
2. **Explicit Injection of Physical Priors:** Relying on black-box neural networks to implicitly learn physics from data triggers the "Composition Paradox" in resource-scarce robotics. The 2026 release of **HoloBrain-0** shattered this approach by explicitly embedding URDF kinematic trees and precise camera extrinsic/intrinsic matrices into the LLM's computational graph. By forcing visual data into a unified SE(3) 3D motion manifold, HoloBrain-0 proved that embedding classical robotics theory into AI architectures yields exponential leaps in generalization with minimal compute.

### Final Outlook

The path to Artificial General Intelligence in robotics requires a profound compromise with physical reality. The ultimate digital soul for physical hardware—from dexterous grippers to general-purpose humanoids—will be a hierarchical VLA that marries the deep semantic reasoning of modern LLMs with the rigorous spatial geometry and control theory of classical robotics.

## Awesome Papers

### Research Papers

| Title | Introduction | Date | Code |
|:---|:---|:---|:---|
| [RT-2: New model translates vision and language into action](https://deepmind.google/blog/rt-2-new-model-translates-vision-and-language-into-action/) | | 2023-07-28 | |
| [EfficientVLA: Training-Free Acceleration and Compression for Vision-Language-Action Models](https://arxiv.org/abs/2506.10100) | A training-free framework systematically reducing VLA redundancies across language, vision, and action modules for efficient real-time inference.  | 2025-06-11 |  |
| [SP-VLA: A Joint Model Scheduling and Token Pruning Approach for VLA Model Acceleration](https://arxiv.org/abs/2506.12723) | Accelerates VLA inference by jointly scheduling between lightweight generators and target VLMs while dynamically pruning visual tokens based on task uncertainty. | 2025-06-15 | |
| [Spec-VLA: Speculative Decoding for Vision-Language-Action Models with Relaxed Acceptance](https://arxiv.org/abs/2507.22424v2) | Introducing the first speculative decoding framework for VLAs with a relaxed acceptance mechanism based on action token distances to achieve 1.42x speedup. | 2025-07-30 | [GitHub](https://github.com/PineTreeWss/SpecVLA)|
| [VLA-Pruner: Temporal-Aware Dual-Level Visual Token Pruning for Efficient Vision-Language-Action Inference](https://arxiv.org/abs/2511.16449) |  | 2026-11-20 | [GitHub](https://github.com/MINT-SJTU/VLA-Pruner) |
| [KERV: Kinematic-Rectified Speculative Decoding for Embodied VLA Models](https://arxiv.org/html/2603.01581v1) | Accelerates VLA inference by rectifying draft actions with kinematic constraints to improve speculative decoding acceptance rates and ensure physical consistency. | 2026-03-02 | |
| [HeiSD: Hybrid Speculative Decoding for Embodied Vision-Language-Action Models with Kinematic Awareness](https://arxiv.org/abs/2603.17573v1) | | 2026-03-18 | |

### Survey and Review Papers

| Title | Introduction | Date | Code |
|:---|:---|:---|:---|
| [Vision-Language-Action (VLA) Models: Concepts, Progress, Applications and Challenges](https://arxiv.org/abs/2505.04769v2) | | 2025-05-07 | |
| [Pure Vision Language Action (VLA) Models: A Comprehensive Survey](https://arxiv.org/abs/2509.19012v1) | | 2025-09-23 | |

