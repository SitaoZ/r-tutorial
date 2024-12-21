## UpSet
集合处理最常见的可视化方法——维恩图， 但是维恩图不能扩展到三到四个集合之外。相比之下，[UpSet](https://upset.app/)非常适合对包含三个以上集合的数据进行定量分析。


## 图形含义
UpSet图以矩阵形式绘制集合的交集，如下图所示。
每一列对应一个集合，顶部的条形图显示了集合的大小。每一行对应一个可能的交集：填充的单元格显示了哪个集合是交集的一部分。
还要注意连接填充单元格的线条：它们显示了你应该按哪个方向阅读图表。
<div align=center>
<img src="https://github.com/user-attachments/assets/263f1c61-38f7-4740-b9e9-b1d07acf1f6a" alt="fig1">
</div>
这里你可以看到这些交点如何与维恩图中的线段相对应。
图中的第一行完全是空的——它对应于不属于任何集合的所有元素。
绿色（第三）行对应于仅存在于集合 B 中的元素（不存在于 A 或 C 中）。
橙色（第五）行表示集合 A 和 B 共享但不与 C 共享的元素。
最后，最后一行（紫色）表示所有集合之间共享的元素。
<div align=center>
  <img src="https://github.com/user-attachments/assets/b7f11c00-b5c1-47d2-97e0-0b241b0eaea0" alt="fig2">
</div>
这种布局很棒，因为我们可以将交叉点的大小（cardinality “基数”）绘制为矩阵旁边的条形图，如下面的示例所示

<div align=center>
  <img src="https://github.com/user-attachments/assets/cdcb19e8-7d5e-4da5-9ff1-5eb424888b6a" alt="fig3">
</div>


矩阵也非常有用，因为它可以按各种方式排序。一种常见的方式是按基数（cardinality大小）排序，如下图所示，但也可以按度、集合或任何其他所需的排序方式排序。
<div align=center>
  <img src="https://github.com/user-attachments/assets/ed23af73-33a5-483b-a51e-09acf629eb5e" alt="fig4">
</div>

最后，UpSet 在水平和垂直方向上都表现良好。垂直布局更适合可滚动的交互式 UpSet 图，而水平布局最适合论文中的图形。
<div align=center>
  <img src="https://github.com/user-attachments/assets/d066e93a-8677-450c-b3a1-2540a7b2730c" alt="fig5">
</div>

- 注意，UpSet通常易于可视化。但有一个重要警告：在解释集合大小差异很大的数据时，应小心谨慎。

## UpSetR 安装
```r
install.packages("UpSetR")
```
## UpSetR代码
[UpSetR](!https://github.com/hms-dbmi/UpSetR)
```r
# 读取软件提供的自带数据集
mutations <- read.csv( system.file("extdata", "mutations.csv", package = "UpSetR"), header=T, sep = ",")
# 画图
upset(mutations, sets = c("PTEN", "TP53", "EGFR", "PIK3R1", "RB1"), sets.bar.color = "#56B4E9",
order.by = "freq", empty.intersections = "on")
```
