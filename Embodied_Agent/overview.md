# Overview
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/yourusername/awesome-VLA-inference/pulls)

## Survey

- [The Rise and Potential of Large Language Model Based Agents: A Survey](https://arxiv.org/abs/2309.07864)

- [A Survey on Large Language Model based Autonomous Agents](https://arxiv.org/abs/2308.11432)

- [Large Language Models for Robotics: A Survey](https://arxiv.org/abs/2311.07226)

- [Automatically Correcting Large Language Models: Surveying the Landscape of Diverse Automated Correction Strategies](https://direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00660/120911/Automatically-Correcting-Large-Language-Models)

**Key Points**

- 从单一模型走向复合系统架构
- 多智能体协同与人机共融: 人类在系统中扮演信息提供者、反馈者和监督者的角色（如通过纠正性反馈、指导性反馈或隐式干预）
- 告别手动奖励工程：[EUREKA](https://arxiv.org/abs/2310.12931)、[CARD](https://arxiv.org/abs/2410.14660)（LLM 可以直接将自然语言目标转化为可执行的代码奖励函数）、[RLVR](https://arxiv.org/abs/2501.12948)
- Neuro-symbolic AI
- 记忆机制的进化：RAG（无状态的向量检索）进化为连续体记忆架构（[Continuum Memory Architectures, CMA](https://arxiv.org/abs/2601.09913)）

## Agentic Workflows

**Agentic Workflows**

- Core Modules: Perception, Reasoning & Planning, Memory, Action/Tools
- Reason-Act-Reflect / ReAct
- Prompt Chaining
- Routing
- Parallelization / Scatter-Gather
- Orchestrator & Evaluator

## Agent Memory

- [Agentic Memory: Learning Unified Long-Term and Short-Term Memory Management for Large Language Model Agents](https://arxiv.org/abs/2601.01885)

构建一个能自主、动态管理短期与长期记忆的智能体架构，摆脱对人工预设记忆规则的依赖

- Unified Memory Management（通过学习来决定信息的生命周期：先前方案通常将记忆简单划分为STM和LTM，利用固定的规则如简单的滑动窗口或向量检索）进行管理）
- Dynamic Retrieval
- Learning to Manage

## Agent Planning

- [Trajectory-Informed Memory Generation for Self-Improving Agent Systems](https://arxiv.org/abs/2603.10600)

构建一种Self-Improving机制：如何让智能体在不进行昂贵的参数微调，仅通过管理自己的执行轨迹，就能像人类专家一样“吃一堑长一智”

- Structured Experience Injection：三类经验（Strategy：从成功轨迹中学习高效路径，Recovery：从失败轨迹中学习如何从特定错误中恢复，Optimization：识别哪些“成功的步骤”其实是可以更简练的）
- Fine-grained Decision Attribution：强调出处（Provenance）：记录这条建议是基于哪次具体的执行案例生成的
- 动态检索与上下文对齐

- [ICPRL: Acquiring Physical Intuition from Interactive Control](https://arxiv.org/abs/2603.13295)

- [Code as Policies: Language Model Programs for Embodied Control](https://arxiv.org/abs/2209.07753)

利用 LLM 内置的代码生成能力，绕过复杂的数据采集和端到端训练

- 支持复杂逻辑流：支持循环和判断
- 代码作为通用中间表示：比自然语言更加清晰，可溯源
- Few-shot Learning：通过在 Prompt 中提供 3-5 个“任务-代码”，LLM可以学会API

- [Learning from Trials and Errors: Reflective Test-Time Planning for Embodied LLMs](https://arxiv.org/abs/2602.21198)

构建一种通用的、无需重新训练即可提升具身智能体鲁棒性的框架，动态物理环境中的自我修正能力

- Trajectory-Aware Reflective Prompting：多模态观测序列，与逻辑推理链进行对齐，生成具有物理意义的归因结论
- Active Failure Trigger：预防性反思
- Experience Retrieval-Augmentation：在反射记忆中进行语义检索

### Research Papers

| Title | Introduction | Date | Code |
|:---|:---|:---|:---|
| [RoboTracer: Mastering Spatial Trace with Reasoning in Vision-Language Models for Robotics](https://arxiv.org/abs/2512.13660) |  | 2025-12-15 | [GitHub](https://github.com/Zhoues/RoboTracer) |

