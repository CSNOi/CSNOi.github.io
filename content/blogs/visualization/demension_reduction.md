---
title: "高维数据可视化方法：PCA、t-SNE 与 UMAP" 
date: 2026-04-01T10:00:00+08:00 
description: "深度解析主流降维算法 PCA、t-SNE 与 UMAP 的数学原理及可视化对比。" 
image: "/img/blogs/visualization/interpretability.jpg" 
tags: ["AI", "XAI", "Research", "可视化"] 
categories: ["Deep Learning", "技术笔记"] 
draft: false 
mathjax: true
---

# **降维算法详尽指南：PCA、t-SNE 与 UMAP**

在数据科学中，降维是将高维特征空间映射到低维（通常为 2D 或 3D）空间的过程，旨在保留数据的关键结构。本文将深入探讨三种主流算法。

## **1\. PCA (主成分分析 \- Principal Component Analysis)**

<iframe src="/img/blogs/visualization/pca_animation.html" width="100%" height="700px" frameborder="0" style="border-radius: 8px; border: 1px solid #475569; background-color: #1e1e1e;"></iframe>

### **历史背景与论文**

* **提出者**：Karl Pearson (1901) / Harold Hotelling (1933)。  
* **核心论文**：*On lines and planes of closest fit to systems of points in space* (Pearson, 1901)。  
* **背景**：PCA 是最早的线性降维技术，主要用于数据压缩和去除噪声。

### **数学推导**

PCA 的目标是找到一组正交轴（主成分），使得数据在这些轴上的投影方差最大。

1. **数据去中心化**：设数据集为 $ X \in \mathbb{R}^{n \times d} $，计算均值并减去：  
   $$\bar{X} = X - \mu$$
2. **计算协方差矩阵**：  
   $$\Sigma = \frac{1}{n-1} \bar{X}^T \bar{X}$$ 
3. **特征值分解**：  
   对 $\Sigma$ 进行分解，找到特征值 $\lambda$ 和特征向量 $v$：  
   $$\Sigma v = \lambda v$$ 
4. **降维投影**：  
   选取前 $k$ 个最大特征值对应的特征向量组成矩阵 $W_k$。降维后的数据 $Z$ 为：  
   $$Z = \bar{X} W_k$$

## **2\. t-SNE (t-分布邻域嵌入 \- t-Distributed Stochastic Neighbor Embedding)**

### **历史背景与论文**

* **提出者**：Laurens van der Maaten, Geoffrey Hinton (2008)。  
* **核心论文**：*Visualizing Data using t-SNE*。  
* **背景**：为解决 SNE 算法的“拥挤问题”（Crowding Problem）而提出，是当前聚类可视化的标准工具。

### **数学推导**

t-SNE 通过匹配高维和低维空间中的概率分布来保留局部结构。

1. **高维空间相似度（高斯分布）**：  
   点 $x_j$ 相对于 $x_i$ 的条件概率：  
   $$p_{j|i} = \frac{\exp(-\|x_i - x_j\|^2 / 2\sigma_i^2)}{\sum_{k \neq i} \exp(-\|x_i - x_k\|^2 / 2\sigma_i^2)}$$
   定义对称联合概率 $p_{ij} = \frac{p_{j|i} + p_{i|j}}{2n}$。  
2. **低维空间相似度（t-分布）**：  
   为了缓解拥挤问题，使用自由度为 1 的 t-分布（柯西分布）：  
   $$q_{ij} = \frac{(1 + \|y_i - y_j\|^2)^{-1}}{\sum_{k \neq l} (1 + \|y_k - y_l\|^2)^{-1}}$$
3. **目标函数（KL 散度）**：  
   通过梯度下降最小化两个分布之间的差异：  
   $$C = KL(P \| Q) = \sum_{i} \sum_{j} p_{ij} \log \frac{p_{ij}}{q_{ij}}$$

## **3\. UMAP (一致流形近似与投影 \- Uniform Manifold Approximation and Projection)**

### **历史背景与论文**

* **提出者**：Leland McInnes, John Healy, James Melville (2018)。  
* **核心论文**：*UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction*。  
* **背景**：基于黎曼几何和代数拓扑理论，旨在比 t-SNE 更快且能更好地保留全局结构。

### **数学推导**

UMAP 假设数据均匀分布在流形上，并计算模糊单纯复形（Fuzzy Simplicial Sets）。

1. **高维权边计算**：  
   $$w(x_i, x_j) = \exp\left(-\frac{\max(0, d(x_i, x_j) - \rho_i)}{\sigma_i}\right)$$ 
   其中 $\rho_i$ 是点 $i$ 到其最近邻的距离。  
2. **低维权边模拟**：  
   $$q_{ij} = (1 + a\|y_i - y_j\|^{2b})^{-1}$$ 
3. **目标函数（交叉熵）**：  
   UMAP 同时关注“吸引”和“排斥”：  
   $$\mathcal{L}_{CE} = \sum_{i \neq j} \left[ p_{ij} \log \left(\frac{p_{ij}}{q_{ij}}\right) + (1 - p_{ij}) \log \left(\frac{1 - p_{ij}}{1 - q_{ij}}\right) \right]$$

## **4\. 可视化对比代码 (Python)**
```

import numpy as np  
import matplotlib.pyplot as plt  
from sklearn import datasets  
from sklearn.decomposition import PCA  
from sklearn.manifold import TSNE  
import umap  \# pip install umap-learn

\# 1\. 加载数据 (MNIST 手写数字采样)  
digits \= datasets.load\_digits()  
X, y \= digits.data, digits.target

\# 2\. 执行降维  
\# PCA  
pca\_res \= PCA(n\_components=2).fit\_transform(X)

\# t-SNE  
tsne\_res \= TSNE(n\_components=2, random\_state=42).fit\_transform(X)

\# UMAP  
umap\_res \= umap.UMAP(n\_neighbors=15, min\_dist=0.1, random\_state=42).fit\_transform(X)

\# 3\. 绘图对比  
fig, axes \= plt.subplots(1, 3, figsize=(18, 5))  
titles \= \['PCA', 't-SNE', 'UMAP'\]  
results \= \[pca\_res, tsne\_res, umap\_res\]

for i, ax in enumerate(axes):  
    scatter \= ax.scatter(results\[i\]\[:, 0\], results\[i\]\[:, 1\], c=y, cmap='Spectral', s=5)  
    ax.set\_title(titles\[i\])  
    plt.colorbar(scatter, ax=ax)

plt.tight\_layout()  
plt.show()

```

## **5\. 总结对比**

| 特性 | PCA | t-SNE | UMAP |
| :---- | :---- | :---- | :---- |
| **基础** | 线性代数 (协方差) | 概率论 (t-分布) | 拓扑学 (单纯复形) |
| **速度** | 极快 | 较慢 | 快 |
| **重点** | 全局方差 | 局部结构 | 局部+全局平衡 |
| **场景** | 初步探索、降噪 | 聚类可视化 | 大规模数据集、保留宏观结构 |

