---
author: "陈坤葳 (Kunwei Chen)"
title: "关于我 (About Me)"
date: "2026-04-07"
description: "AI与机器人开发者的自我介绍及技术背景"
tags: [
    "About",
    "Resume",
]
---

## 👋 你好，我是陈坤葳 (Kunwei Chen)

我是一名专注于 **人工智能（AI）** 与 **机器人（Robotics）** 交叉领域的开发者与研究者。我的主要研究方向包括大视觉模型（如 SAM3）的微调、生成式扩散模型开发，以及基于 ROS 2 的视觉同时定位与建图（VSLAM）系统构建。

我热衷于探索计算机视觉算法在底层硬件上的高效部署，致力于让机器人系统拥有更精准的感知与定位能力。

---

## 🔬 核心研究方向

### 1. 视觉大模型与微调 (AI & Vision)
* **SAM3 微调与应用**：利用 LoRA 技术对 Segment Anything Model 3 进行参数高效微调（PEFT），针对特定领域（如动漫角色分割）优化模型边缘检测表现。
* **扩散模型 (Diffusion Models)**：独立开发用于动漫角色生成的扩散模型，涵盖 256x256 数据集构建、训练脚本调试及 GPU 利用率底层优化。

### 2. 机器人视觉定位 (VSLAM)
* **异构传感器融合**：利用 Orbbec RGB-D 相机与 6 轴 IMU（惯性测量单元）数据进行高精度视觉里程计开发。
* **系统优化**：针对复杂环境下的特征点丢失问题，引入闭环检测（Loop Closure）机制，并致力于解决相机底层帧率不稳定等硬件级传输问题。

---

## 💻 技术栈 (Tech Stack)

得益于开源社区的强大力量，以下是我在日常开发与研究中构建的核心技能矩阵：

| 领域 | 核心技术 / 工具 |
| :--- | :--- |
| **编程语言** | Python 3.x, C++ 17 |
| **深度学习框架** | PyTorch, LoRA, TensorRT |
| **机器人底层系统** | ROS 2 Jazzy, Ubuntu 24.04 |
| **计算机视觉** | OpenCV, SAM3, Diffusion Models |
| **硬件与调试** | Meta Quest 3S, Orbbec Camera |

---

## 🚀 近期动态

目前，我正在使用 **Ubuntu 24.04** 和 **ROS 2 Jazzy** 环境，结合 **Meta Quest 3S** 头显，开发一套端到端的 VSLAM 演示与交互系统。同时，我也在持续迭代 SAM3 的训练脚本，解决复杂场景下的 AttributeError 等工程化挑战。

---

## 📫 联系我

我非常期待与志同道合的开发者、研究人员进行技术交流。如果您对我的算法部署感兴趣，或者有合作机会，欢迎通过以下方式与我取得联系：

* **Email**: <WorkKunweiChen@outlook.com>
* **GitHub**: [CSNOi](https://github.com/CSNOi)
* **QQ**: 1226120778

> 保持开源精神，在算法与硬件的边界上持续探索。