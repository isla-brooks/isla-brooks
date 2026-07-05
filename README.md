## Han Xiaoyu · 韩晓雨

南京大学研究生 · 声音大模型 —— 听懂声音，也生成声音。

**我在研究什么？**
两件事：让模型 **听懂** 环境声音（场景、事件），也让它 **生成** 声音（音效、环境声）。一进一出，都围绕「声音」这件事做大模型。

**做过什么？**

*听懂* → [soundscape-lm](https://github.com/isla-brooks/soundscape-lm)
声学场景分类 + 声音事件检测，接入大语言模型做场景描述与音频–文本检索。

*生成* → [foley-flow](https://github.com/isla-brooks/foley-flow)
文本 / 条件驱动的音效（Foley）与环境声生成，扩散 / 流匹配声学模型，含时序对齐。

**怎么写代码？**
numpy / scipy 内核，`import` 不拉深度学习框架；无 GPU、无网络也能跑核心与单测；需要训练时再接可选 PyTorch。离线优先，CI 全绿。

<sub>Nanjing University · 声音大模型 · 听懂 + 生成 · she/her</sub>
