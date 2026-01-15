<!-- markdownlint-disable first-line-h1 -->
<!-- markdownlint-disable html -->
<!-- markdownlint-disable no-duplicate-header -->

<div align="center">
  <img src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/logo.svg?raw=true" width="60%" alt="DeepSeek-V3" />
</div>
<hr>
<div align="center" style="line-height: 1;">
  <a href="https://www.deepseek.com/"><img alt="Homepage"
    src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/badge.svg?raw=true"/></a>
  <a href="https://chat.deepseek.com/"><img alt="Chat"
    src="https://img.shields.io/badge/🤖%20Chat-DeepSeek%20V3-536af5?color=536af5&logoColor=white"/></a>
  <a href="https://huggingface.co/deepseek-ai"><img alt="Hugging Face"
    src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-DeepSeek%20AI-ffc107?color=ffc107&logoColor=white"/></a>
  <br>
  <a href="https://discord.gg/Tc7c45Zzu5"><img alt="Discord"
    src="https://img.shields.io/badge/Discord-DeepSeek%20AI-7289da?logo=discord&logoColor=white&color=7289da"/></a>
  <a href="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/qr.jpeg?raw=true"><img alt="Wechat"
    src="https://img.shields.io/badge/WeChat-DeepSeek%20AI-brightgreen?logo=wechat&logoColor=white"/></a>
  <a href="https://twitter.com/deepseek_ai"><img alt="Twitter Follow"
    src="https://img.shields.io/badge/Twitter-deepseek_ai-white?logo=x&logoColor=white"/></a>
  <br>
  <a href="LICENSE" style="margin: 2px;">
    <img alt="License" src="https://img.shields.io/badge/License-Apache 2.0-f5de53?&color=f5de53" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <br>
</div>

## 1. 介绍

此仓库包含论文的官方实现：**[Conditional Memory via Scalable Lookup: A New Axis of Sparsity for Large Language Models](Engram_paper.pdf)**。

> **摘要：** 虽然 Mixture-of-Experts（MoE）通过条件计算扩展模型容量，但 Transformer 缺乏原生的知识查找原语。为此，我们探索将 **条件记忆（conditional memory）** 作为一个互补的稀疏性维度，并通过 **Engram** 模块对经典的 $N$-gram 嵌入进行了现代化，使其支持 $\mathcal{O}(1)$ 查找。

**主要贡献：**

- **稀疏性分配：** 我们对神经计算（MoE）与静态记忆（Engram）之间的权衡进行了形式化，发现了一个 U 形的缩放规律，指导最佳容量分配。
- **实证验证：** 在严格的同参数（iso-parameter）和同 FLOPs（iso-FLOPs）约束下，Engram-27B 模型在知识、推理、代码与数学等领域相较 MoE 基线表现出持续改进。
- **机理分析：** 我们的分析表明，Engram 可以减轻早期层对静态模式重建的负担，从而可能为复杂推理保留更有效的深度。
- **系统效率：** 该模块采用确定性的寻址方式，允许将大规模嵌入表卸载到主机内存，从而在推理时带来极小的额外开销。

## 2. 架构

Engram 模块通过检索静态 $N$-gram 记忆并将其与动态隐状态融合来扩展主干网络。架构如下所示（提供了 [drawio 图](drawio/Engram.drawio)）：

<p align="center">
  <img width="75%" src="figures/arch.png" alt="Engram Architecture">
</p>

## 3. 评估

### 缩放规律

<p align="center">
  <img width="90%" src="figures/scaling_law.png" alt="Scaling Law">
</p>

---

### 大规模预训练

<p align="center">
  <img width="80%" src="figures/27b_exp_results.png" alt="Pre-training Results">
</p>

---

### 长上下文训练

<p align="center">
  <img width="80%" src="figures/long_context_results.png" alt="Long Context Results">
</p>

## 4. Engram 个案研究

<p align="center">
  <img width="80%" src="figures/case.png" alt="Long Context Results">
</p>

## 5. 快速开始

建议使用 Python 3.8+ 和 PyTorch。

```bash
pip install torch numpy transformers sympy
```

我们提供了一个独立实现以演示 Engram 模块的核心逻辑：

```bash
python engram_demo_v1.py
```

> ⚠️ **注意：** 提供的代码是演示版本，旨在说明数据流。它对标准组件（如 Attention/MoE/mHC）做了简化或模拟处理，以便集中展示 Engram 模块。

## 6. 许可

Engram 模型的使用受 [模型许可](LICENSE) 约束。

## 7. 联系方式

如有任何问题，请提交 issue 或通过以下邮箱联系我们： [service@deepseek.com](mailto:service@deepseek.com)。
