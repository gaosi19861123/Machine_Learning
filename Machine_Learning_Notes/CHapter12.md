# 机器学习系统设计

## 确定执行的优先级

例子：垃圾邮件分类器，统计邮件内容的词频，并与过滤关键词特征表进行关联
> $x_j=\begin{cases}1 &if \ word \ j \ appears \ in \ e-mail \\0 &otherwise \end{cases}$
> 实际实现过程中，将挑选垃圾邮件中出现频次最多的10000~500000个单词

实现方法：
* 收集数据 ('honeypot' 项目收集垃圾邮件)
* 根据邮件标题使用复杂的特征
* 根据邮件内容使用复杂的特征
* 强化鲁棒性(例如错误拼写单词)

> 目前没有绝对有效的方法，大多数都是随机使用，或者混合使用

## 误差分析

推荐的方法：
1. 使用一个相对简单粗暴的算法快速启动项目，然后使用交叉验证集测试；
2. 绘制学习曲线(learning curves)，然后根据曲线决定优化方案(更多数据/更多特征);
3. 误差分析: 观察错误的结果，总结出特征，然后优化系统；
   * 注意频率多的错误，并找出对应方案
4. 数值估计
   * 使用一些方法，完善算法 ( 词干统计 etc. )
   * 利用 交叉验证机 低成本试错 (观察 误差变化) 

## 不对称性分类的误差分析

### 准确率 Precision / 召回率 Recall

针对一个类别 ( 例如 $y=1$ ) 进行预测

| 预测 \ 实际 |  实际 1   |  实际 0   |
| :---------: | :-------: | :-------: |
|   预测 1    | 真正类 TP | 假正类 FP |
|   预测 0    | 假负类 FN | 真负类 TN |

**准确率**  $=\dfrac{TP}{TP+FP}$     ( 又名 查准率 )
> Precision 准确率 = 真实的正类 / 所有**预测的正类**  

**召回率**  $=\dfrac{TP}{TP+FN}$     ( 又名 查全率 )
> Recall 召回率 = 真实的正类 / 所有**正类** 

## 准确率与召回率的权衡

调节阈值 $threshold$ 可以改变 $h_\theta(x) \geqslant \ threshold$,然后调节 **准确率** 与 **召回率**

* 阈值低 - 高召回率，低准确率
* 阈值高 - 高准确率，低召回率

利用 **召回率-准确率曲线** 找到平衡点

### $F_1$ 参数
比较 准确率/召回率 的方法:
* 平均 : $\dfrac{Precision + Recall}{2}$ （ 不是一个好方案 ）
* $F_1$ 参数 : $F_1=2 \dfrac{Precision \times Recall}{Precision + Recall}$
  * 如果  $Precision = 0$ 或者 $Recall = 0$ , 则 $F_1 = 0$
  * 如果  $Precision = 1$ 同时 $Recall = 1$ , 则 $F_1 = 1$ 

|       | 准确率 | 召回率 | 平均(不推荐) | $F_1$ 分数(推荐) |
| :---: | :----: | :----: | :----------: | :--------------: |
| 算法1 |  0.5   |  0.4   |     0.45     |    **0.444**     |
| 算法2 |  0.7   |  0.1   |     0.4      |      0.175       |
| 算法3 |  0.02  |  1.0   |   **0.51**   |      0.0392      |

## 机器学习数据

数据量的增加会提高模型的准确性
> 有足够的特征信息用于预测 (用人的思维来考虑是否可以预测)
> 有大量的训练数据 ($J_{train}(\theta) \approx J_{test}(\theta)$ , 同时 $J_{test}(\theta)$ 会比较小，模型相对低方差 )