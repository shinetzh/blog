# 1. Chamfer Distance
>CD用来衡量两个点云的差异，距离越小两个点云越相似。计算点云P到点云Q的平均最近距离，并且反过来计算点云P到点云Q的平均距离。再进行相加，对前后顺序不敏感，是对称的。

$$
C D(P, Q)=\frac{1}{|P|} \sum_{p \in P} \min _{q \in Q}\|p-q\|^2+\frac{1}{|Q|} \sum_{q \in Q} \min _{p \in P}\|q-p\|^2
$$
$\min _{q \in Q}\|p-q\|^2$对点$p$计算它到点云Q中所有点的距离，并取最小值，$\min _{p \in P}\|q-p\|^2$同理。
# 2. F-Score
>用来衡量分类算法的好坏，F-Score越大表示分类效果越好。
### 2.1. 分类模型中的应用
经常用在分类模型准确率的评价标准上面。是对准确率和召回率进行加权之后的评价指标。


$$
\begin{aligned}
Accuracy &= \frac{TP + TN} {TP + FP + TN + FN}
\end{aligned} \tag{1}\\
$$
$$
\begin{aligned}
Precision &= \frac {TP} {TP+FP}
\end{aligned}) \tag{2}
$$

$$
\begin{aligned}
Recall &= \frac {TP} {TP+FN}
\end{aligned} \tag{3}
$$
- $Accuracy$ 表示模型预测的准确率，包括正样本和负样本。
- $Precision$ 表示所有预测为正的样本中真实正样本的比例。
- $Recall$ 表示所有真实的正样本中有多少被预测为正样本。
$$
\begin{aligned}
\frac {2} {F_{score}} &= \frac {1} {precision} + \frac {1} {recall} \\
F_{score} &= \frac {2 \times precision \times recall} {precision + recall} \\
&= \frac {2TP} {2TP + FP + FN}
\end{aligned} \tag{4}
$$

通用的$F_{score}$可以表示如下：
$$
\begin{aligned}
F_{score} = \frac {(1 + \alpha ^ 2) \times precision \times recall} {\alpha^2 \times precision + recall}
\end{aligned} \tag{5}
$$

- 当$\alpha == 1$时， $F_{score}$ 被叫做$F1_{score}$， 是$Precision$ 和 $Recall$ 的调和平均值，既表示两者同等重要。
- 当$\alpha > 1$时 ，我们认为$Recall$ 更重要。
- 当$\alpha<1$ 时， 我们认为$Precision$ 更重要。

### 2.2. 三维重建中的应用
在三维重建中， $Precision$ 表示在重建模型中的点云能够被参考模型中的点云匹配的概率，也就是重建模型点云中能够被参考模型匹配的点的数量除以重建模型中所有点的个数，如公式(6) 所示，其中$|P|$ 表示重建模型中所有的点云数量:
$$
Precision =\frac{1}{|P|} \sum_{p \in P} 1\left(\min _{q \in Q}\|p-q\|<\tau\right) \tag{6}
$$
$Recall$ 表示参考模型中所有点云能够被重建模型的点云匹配的概率，如公式 (7)所示:
$$
Recall=\frac{1}{|Q|} \sum_{q \in Q} 1\left(\min _{p \in P}\|q-p\|<\tau\right) \tag{7}
$$
$F_{score}$计算公式和（5）式相同。

# 3. Normal Consistency
>用来评估两个模型表面法向量的一致性，值越接近1，表明两个模型越相似。

$$
N C=\frac{1}{|P|} \sum_{p \in P} \max _{q \in Q}\left(n_p \cdot n_q\right)
$$
$\max _{q \in Q}\left(n_p \cdot n_q\right)$ 表示对于点云$P$中的一个点$p$，找到点云$Q$ 中 和其最相近的点$q$，并计算这两个点的法线$n_p, n_q$的点积。Normal Consistency等于所有法线点积的加和平均。其中 $-1 < n_p \cdot n_q < 1$，当$n_p \cdot n_q == 1$ 表示两个法线方向完全一致。当$n_p \cdot n_q == -1$表示两个法线方向完全相反。