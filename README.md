<h1 align="center">Han Xiaoyu</h1>

<p align="center">
  <b>声音大模型 · Sound Foundation Models</b><br>
  南京大学研究生 · 声学场景与事件理解 ↔ 可控音效与声音生成
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Focus-Sound%20Foundation%20Models-4c6ef5?style=flat-square" alt="focus">
  <img src="https://img.shields.io/badge/Understanding-Scene%20%26%20Event-15aabf?style=flat-square" alt="understanding">
  <img src="https://img.shields.io/badge/Generation-Foley%20%26%20Ambience-e8590c?style=flat-square" alt="generation">
  <img src="https://img.shields.io/badge/Principle-Reproducible%20%2F%20Offline--first-2f9e44?style=flat-square" alt="principle">
</p>

---

## 关于

我是**南京大学研究生**，研究方向为**声音大模型（sound foundation models）**。我关注一条完整的链路：从**环境声与声学场景的理解**（这是什么场景、里面有哪些声音事件、如何用语言描述与检索），到**可控音效与声音生成**（把文本与时序条件映射为可听的波形）。

在实现取向上，我偏好**可复现**与**离线优先**：核心算法尽量只依赖 `numpy` / `scipy`，把深度模型与大语言模型作为**可选后端**，让整条流程在 CPU 上即可秒级跑通、结果可确定复现——既方便审阅，也方便在没有预训练大权重的环境里做研究实验。

## 研究方向

| 维度 | 关注点 |
| --- | --- |
| 🎧 **声音理解** | 声学场景分类（ASC）、声音事件检测（SED）、音频-文本跨模态检索、结合大模型的场景描述 |
| 🎛 **声音生成** | 文本 / 时序条件驱动的可控 Foley 与环境声合成、扩散与流匹配声学建模、离线声码器还原 |
| 🧪 **方法与工程** | 从零实现的可训练模型与梯度校验、可复现基准、离线优先的最小依赖工程 |

## 代表项目

以下均为本账号下的**真实开源仓库**，两者是理解侧与生成侧的姊妹项目：

### 🎧 [soundscape-lm](https://github.com/isla-brooks/soundscape-lm) · 环境声音理解工具包

把一段音频转成「**这是什么场景 + 里面有哪些声音事件 + 一句自然语言描述**」，并支持音频-文本检索。

- **声学场景分类（ASC）**：基于 clip 级嵌入的最近原型分类器，输出场景标签与概率。
- **声音事件检测（SED）**：帧级概率 → 中值滤波 → 阈值 → `(onset, offset, label)` 事件序列。
- **评估指标**：分段指标（P/R/F、S/D/I 错误率）、带时间容差的事件级最优匹配、检索指标（R@1/5/10、mAP@10）。
- **音频-文本检索 + LLM 场景描述**：同一嵌入空间的余弦排序；场景/事件结果可离线渲染成中英文描述，也可接入 OpenAI 兼容接口。
- 核心链路仅依赖 `numpy` / `scipy`，深度模型与 LLM 均为可选后端。

### 🎛 [foley-flow](https://github.com/isla-brooks/foley-flow) · 可控 Foley / 环境声生成框架

把「**文本描述 + 事件时间轴**」映射为音效波形，在归一化梅尔谱上建模，纯 `numpy` / `scipy` 实现、离线优先。

- **统一骨干，双范式**：同一个向量场既能预测扩散噪声 `ε`（DDPM / DDIM），也能预测流匹配速度场 `v`（rectified flow，Euler / Heun / 中点 ODE）。
- **时序对齐**：用 `EventTimeline` 声明「某时刻发生某事件」，采样期通过能量对齐引导把声学事件对到时间轴上。
- **文本 / 标签条件 + CFG**：轻量标签嵌入配合无分类器引导，引导强度可调。
- **纯 numpy 音频前端**：STFT / 梅尔滤波 / Griffin-Lim 声码器 / WAV 读写，无 `librosa`、无 `torch`。
- **手写反向传播**：`numpy` 实现的可训练模型，附有限差分梯度校验，CPU 上秒级确定性复现。

## 工程原则

- **可复现优先**：确定性的演示与基准，尽量避免隐藏的随机性与外部服务依赖。
- **离线优先 / 最小依赖**：核心不绑定重型框架与预训练大权重，深度与 LLM 后端可插拔。
- **完备工程**：CI 矩阵、`ruff` / `mypy`、`pytest` 覆盖率、示例、文档与发布流程，MIT 许可证。

## 技术栈

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="python">
  <img src="https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white" alt="numpy">
  <img src="https://img.shields.io/badge/SciPy-8CAAE6?style=flat-square&logo=scipy&logoColor=white" alt="scipy">
  <img src="https://img.shields.io/badge/pytest-0A9EDC?style=flat-square&logo=pytest&logoColor=white" alt="pytest">
  <img src="https://img.shields.io/badge/Ruff-D7FF64?style=flat-square&logo=ruff&logoColor=black" alt="ruff">
  <img src="https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white" alt="actions">
</p>

## 目前在做

- 打磨 `soundscape-lm` 与 `foley-flow` 两侧的评估与文档，让理解→描述→生成形成闭环。
- 探索环境声表征在场景理解与可控生成之间的共享与迁移。
