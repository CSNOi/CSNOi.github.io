---
title: "高维数据可视化方法"
date: 2026-04-01T10:00:00+08:00
description: "PCA、tSNE、UMAP"
image: "/img/blogs/interpretability.jpg"
tags: ["AI", "XAI", "Research"]
categories: ["Deep Learning"]
draft: false
---

# 降维算法详尽指南：PCA、t-SNE 与 UMAP

在数据科学中，降维是将高维特征空间映射到低维（通常为 2D 或 3D）空间的过程，旨在保留数据的关键结构。本文将深入探讨三种主流算法。

## 1. PCA (主成分分析 - Principal Component Analysis)

### 历史背景与论文

* **提出者**：Karl Pearson (1901) / Harold Hotelling (1933)。

* **核心论文**：*On lines and planes of closest fit to systems of points in space* (Pearson, 1901)。

* **背景**：PCA 是最早的线性降维技术，主要用于数据压缩和去除噪声。

### 数学推导

PCA 的目标是找到一组正交轴（主成分），使得数据在这些轴上的投影方差最大。

<comment-tag id="1">1. **数据去中心化**：设数据集为 $X \in \mathbb{R}^{n \times d}$，计算均值并减去：</comment-tag id="1" text="建议改为：'**中心化处理（Zero-centering）**：设原始数据集为 $X \in \mathbb{R}^{n \times d}$，通过减去每个维度的均值 $\mu$ 使得数据的中心位于坐标原点。' 这样表述更加专业，且明确了该操作的几何意义。" type="suggestion">
   

   $$
   \bar{X} = X - \mu
   $$

2. **计算协方差矩阵**：
   

   $$
   \Sigma = \frac{1}{n-1} \bar{X}^T \bar{X}
   $$

3. **特征值分解**：
   对 $\Sigma$ 进行分解，找到特征值 $\lambda$ and 特征向量 $v$：
   

   $$
   \Sigma v = \lambda v
   $$

4. **降维投影**：
   选取前 $k$ 个最大特征值对应的特征向量组成矩阵 $W_k$。降维后的数据 $Z$ 为：
   

   $$
   Z = \bar{X} W_k
   $$

## 2. t-SNE (t-分布邻域嵌入 - t-Distributed Stochastic Neighbor Embedding)

### 历史背景与论文

* **提出者**：Laurens van der Maaten, Geoffrey Hinton (2008)。

* **核心论文**：*Visualizing Data using t-SNE*。

* <comment-tag id="2">**背景**：为解决 SNE 算法的“拥挤问题”（Crowding Problem）而提出</comment-tag id="2" text="建议补充对拥挤问题的说明：'，即在高维空间中距离较远的点在投影到低维空间后往往会挤在一起，无法区分不同的簇。' 这能解释 t-SNE 引入 t-分布的根本动力。" type="suggestion">，是当前生物信息学和图像处理中聚类可视化的标准工具。

### 数学推导

t-SNE 通过匹配高维和低维空间中的概率分布来保留局部结构。

1. **高维空间相似度（高斯分布）**：
   点 $x_j$ 相对于 $x_i$ 的条件概率：
   

   $$
   p_{j|i} = \frac{\exp(-\|x_i - x_j\|^2 / 2\sigma_i^2)}{\sum_{k \neq i} \exp(-\|x_i - x_k\|^2 / 2\sigma_i^2)}
   $$

   
   定义对称联合概率 $p_{ij} = \frac{p_{j|i} + p_{i|j}}{2n}$。

<comment-tag id="3">2. **低维空间相似度（t-分布）**：
   为了长尾效应，使用自由度为 1 的 t-分布（柯西分布）：</comment-tag id="3" text="建议明确说明为何使用 t-分布。可以改为：'为了缓解拥挤问题，低维空间使用自由度为 1 的 t-分布（即柯西分布）。利用其长尾特性，中等距离的点在低维空间会比在高斯分布下被排斥得更远，从而更好地展示簇间结构。' " type="suggestion">
   

   $$
   q_{ij} = \frac{(1 + \|y_i - y_j\|^2)^{-1}}{\sum_{k \neq l} (1 + \|y_k - y_l\|^2)^{-1}}
   $$

3. **目标函数（KL 散度）**：
   通过梯度下降最小化两个分布之间的差异：
   

   $$
   C = KL(P \| Q) = \sum_{i} \sum_{j} p_{ij} \log \frac{p_{ij}}{q_{ij}}
   $$

## 3. UMAP (一致流形近似与投影 - Uniform Manifold Approximation and Projection)

### 历史背景与论文

* **提出者**：Leland McInnes, John Healy, James Melville (2018)。

* **核心论文**：*UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction*。

* **背景**：基于黎曼几何和代数拓扑理论，旨在比 t-SNE 更快且能更好地保留全局结构。

### 数学推导

UMAP 假设数据均匀分布在流形上，并计算模糊单纯复形（Fuzzy Simplicial Sets）。

1. **高维权边计算**：
   

   $$
   w(x_i, x_j) = \exp\left(-\frac{\max(0, d(x_i, x_j) - \rho_i)}{\sigma_i}\right)
   $$

   
   其中 $\rho_i$ 是点 $i$ 到其最近邻的距离。

<comment-tag id="4">2. **低维权边模拟**：
   

   $$
   q_{ij} = (1 + a\|y_i - y_j\|^{2b})^{-1}
   $$</comment-tag id="4" text="建议补充对超参数 $a$ 和 $b$ 的简要说明：'其中 $a$ 和 $b$ 是由参数 min_dist 和 spread 决定的超参数，用于控制低维嵌入中点的紧致程度。' 这有助于读者将公式与库中的参数设置联系起来。" type="suggestion">

3. **目标函数（交叉熵）**：
   不同于 t-SNE 只关注“吸引”，UMAP 同时关注“吸引”和“排斥”：
   

   $$
   \mathcal{L}_{CE} = \sum_{i \neq j} \left[ p_{ij} \log \left(\frac{p_{ij}}{q_{ij}}\right) + (1 - p_{ij}) \log \left(\frac{1 - p_{ij}}{1 - q_{ij}}\right) \right]
   $$

## 4. 可视化对比代码 (Python)

<comment-tag id="5">```
import numpy as np</comment-tag id="5" text="建议在代码块开头指明语言类型，即使用 ```python。这样在 Markdown 渲染中可以启用语法高亮，极大提升代码块的可读性。" type="suggestion">
import matplotlib.pyplot as plt
from sklearn import datasets
from sklearn.decomposition import PCA
from sklearn.manifold import TSNE
import umap # pip install umap-learn

# 1. 加载数据 (MNIST 手写数字采样)
digits = datasets.load_digits()
X, y = digits.data, digits.target

# 2. 执行降维
# PCA
pca_res = PCA(n_components=2).fit_transform(X)

# t-SNE
tsne_res = TSNE(n_components=2, random_state=42).fit_transform(X)

# UMAP
umap_res = umap.UMAP(n_neighbors=15, min_dist=0.1, random_state=42).fit_transform(X)

# 3. 绘图对比
fig, axes = plt.subplots(1, 3, figsize=(18, 5))
titles = ['PCA', 't-SNE', 'UMAP']
data = [pca_res, tsne_res, umap_res]

for i, ax in enumerate(axes):
    scatter = ax.scatter(data[i][:, 0], data[i][:, 1], c=y, cmap='Spectral', s=5)
    ax.set_title(titles[i])
    plt.colorbar(scatter, ax=ax)

plt.tight_layout()
plt.show()

```

## 5. 总结对比

| 特性 | PCA | t-SNE | UMAP | 
 | ----- | ----- | ----- | ----- | 
| **基础** | 线性代数 (协方差) | 概率论 (t-分布) | 拓扑学 (单纯复形) | 
| **速度** | 极快 | 较慢 | 快 | 
| **重点** | 全局方差 | 局部结构 | 局部+全局平衡 | 
| **场景** | 初步探索、降噪 | 聚类可视化 | 大规模数据集、保留宏观结构 | 


*建议已添加*