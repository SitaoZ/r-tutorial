## ggpubr 

[ggpubr](https://rpkgs.datanovia.com/ggpubr/)用于修饰图形，便于发表。它是配套**ggplot**使用的。


## 提供的统计分析方法

| 方法 | R函数 | 描述  |
| :------:|:------:|:-----:|
| T-test | 	t.test()	| 比较两组(参数) |
| Wilcoxon test |	wilcox.test()	| 比较两组(非参数) |
| ANOVA	| aov()或anova()	| 比较多组(参数) |
| Kruskal-Wallis	| kruskal.test()	| 比较多组(非参数) |


## 安装

```r
install.packages("ggpubr")

or

# Install
if(!require(devtools)) install.packages("devtools")
devtools::install_github("kassambara/ggpubr")

```

## 教程
- Boxplot

```r
# Load data
data("ToothGrowth")
df <- ToothGrowth
head(df, 4)

# Box plots with jittered points
# :::::::::::::::::::::::::::::::::::::::::::::::::::
# Change outline colors by groups: dose
# Use custom color palette
# Add jitter points and change the shape by groups
 p <- ggboxplot(df, x = "dose", y = "len",
                color = "dose", palette =c("#00AFBB", "#E7B800", "#FC4E07"),
                add = "jitter", shape = "dose")
 p

 
 # Add p-values comparing groups
 # Specify the comparisons you want
my_comparisons <- list( c("0.5", "1"), c("1", "2"), c("0.5", "2") )
p + stat_compare_means(comparisons = my_comparisons)+ # Add pairwise comparisons p-value
  stat_compare_means(label.y = 50)                   # Add global p-value



 
# Violin plots with box plots inside
# :::::::::::::::::::::::::::::::::::::::::::::::::::
# Change fill color by groups: dose
# add boxplot with white fill color
ggviolin(df, x = "dose", y = "len", fill = "dose",
         palette = c("#00AFBB", "#E7B800", "#FC4E07"),
         add = "boxplot", add.params = list(fill = "white"))+
  stat_compare_means(comparisons = my_comparisons, label = "p.signif")+ # Add significance levels
  stat_compare_means(label.y = 50)                                      # Add global the p-value

```


- Dot charts

```r


```
