---
author: "陈坤葳 (Kunwei Chen)"
title: "核心技术栈与研究方向概览"
date: "2026-04-07"
description: "集成 VSLAM、SAM3 与 ROS 2 的异构感知系统开发记录"
tags: ["AI", "Robotics", "SLAM", "Edge Computing"]
categories: ["Technology"]
---

作为一名专注于 **AI 与机器人** 交叉领域的开发者，我致力于将先进的感知算法转化为实际的工程应用。以下是我目前的研究矩阵与核心技术架构。

## 🛠 核心技术栈 (Core Tech Stack)

通过下表可以直观了解我在不同层级的技术积累：

| 维度 | 核心工具/框架 | 应用场景 |
| :--- | :--- | :--- |
| **感知算法** | `PyTorch`, `OpenCV`, `SAM3` | 语义分割、LoRA 微调、特征提取 |
| **定位导航** | `ORB-SLAM3`, `ROS 2 Jazzy` | 视觉里程计、闭环检测、IMU 融合 |
| **硬件/部署** | `NVIDIA Jetson`, `TensorRT`, `Orbbec` | 模型加速、实时数据流采集 |
| **底层开发** | `C++ 17`, `Python 3.10`, `CUDA` | 驱动开发、高性能计算优化 |



---

## 🧠 深度学习与大模型 (AI & LLM)

我目前正在进行的 **SAM3 (Segment Anything Model 3)** 微调实验，旨在提升其在复杂动漫场景下的语义边缘检测精度。

#### LoRA 训练脚本配置 (示例)
利用极简的代码风格展示硬核内容：

```python
# SAM3 LoRA 微调参数片段
config = {
    "model_type": "sam3_vit_h",
    "lora_rank": 8,
    "target_modules": ["q_proj", "v_proj"],
    "learning_rate": 1e-4,
    "dataset": "anime_character_256x256"
}
print(f"🚀 初始化训练任务: {config['model_type']}")