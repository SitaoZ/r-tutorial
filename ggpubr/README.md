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

- 分布图
```r
library(ggpubr)
# 创建数据格式
set.seed(1234)
wdata = data.frame(
   sex = factor(rep(c("F", "M"), each=200)),
   weight = c(rnorm(200, 55), rnorm(200, 58)))
head(wdata, 4)

# 带有平均线和边际地毯的密度图
# 边框和填充颜色使用("sex")
# 使用特定的颜色代码
ggdensity(wdata, x = "weight",
   add = "mean", rug = TRUE,
   color = "sex", fill = "sex",
   palette = c("#00AFBB", "#E7B800"))
```
<img width="1017" height="960" alt="image" src="https://github.com/user-attachments/assets/201c8240-95ab-4fbf-89d4-e1a50cce9d81" />


```r
# 带有平均线和边际地毯的**直方图**
# 边框和填充颜色使用("sex")
gghistogram(wdata, x = "weight",
   add = "mean", rug = TRUE,
   color = "sex", fill = "sex",
   palette = c("#00AFBB", "#E7B800"))

```
<img width="1017" height="960" alt="image" src="https://github.com/user-attachments/assets/f34af178-de24-480a-8d42-3f140051d133" />



- Boxplot

```r
# 导入数据
data("ToothGrowth")
df <- ToothGrowth
head(df, 4)

# Box plots with jittered points
# 箱线图和抖动点
# :::::::::::::::::::::::::::::::::::::::::::::::::::
# 指定box外框的颜色: dose
# 使用自定义的颜色代码 palette
# 添加抖动点同时指定点的形状
 p <- ggboxplot(df, x = "dose", y = "len",
                color = "dose", palette =c("#00AFBB", "#E7B800", "#FC4E07"),
                add = "jitter", shape = "dose")
 p

```
<img width="1017" height="960" alt="image" src="https://github.com/user-attachments/assets/2f78d54b-1cce-4776-8737-4dd47a32d196" />


```r

 # 添加组间比较的p值
 # 指定你要的比较组合
 my_comparisons <- list( c("0.5", "1"), c("1", "2"), c("0.5", "2") )
 p + stat_compare_means(comparisons = my_comparisons)+ # Add pairwise comparisons p-value
   stat_compare_means(label.y = 50)                   # Add global p-value


```

<img width="1017" height="960" alt="image" src="https://github.com/user-attachments/assets/a5257c0c-053a-49f9-8d42-bb83b375c024" />


```r
 
# 小提琴图，且内部添加点
# :::::::::::::::::::::::::::::::::::::::::::::::::::
# 填充的颜色: dose
# 同时小提琴内部添加boxplot,且使用白色填充
ggviolin(df, x = "dose", y = "len", fill = "dose",
         palette = c("#00AFBB", "#E7B800", "#FC4E07"),
         add = "boxplot", add.params = list(fill = "white"))+
  stat_compare_means(comparisons = my_comparisons, label = "p.signif")+ # Add significance levels
  stat_compare_means(label.y = 50)                                      # Add global the p-value

```
<img width="1017" height="960" alt="image" src="https://github.com/user-attachments/assets/33abb8ec-5a9f-4c9f-8229-840b6d92d14a" />



- Bar plots

```r
# 导入数据
data("mtcars")
dfm <- mtcars
# 将cyl变量转变成因子变量factor
dfm$cyl <- as.factor(dfm$cyl)
# 添加新列（名称）
dfm$name <- rownames(dfm)
# 检查数据
head(dfm[, c("name", "wt", "mpg", "cyl")])

# 全局排序
ggbarplot(dfm, x = "name", y = "mpg",
          fill = "cyl",               # change fill color by cyl
          color = "white",            # Set bar border colors to white
          palette = "jco",            # jco journal color palett. see ?ggpar
          sort.val = "desc",          # Sort the value in dscending order
          sort.by.groups = FALSE,     # Don't sort inside each group
          x.text.angle = 90           # Rotate vertically x axis texts
          )
```
<img width="1152" height="960" alt="image" src="https://github.com/user-attachments/assets/ec46bfcb-6895-434d-a8b8-5c9d31df6f8b" />


```r
# 组内排序
ggbarplot(dfm, x = "name", y = "mpg",
          fill = "cyl",               # change fill color by cyl
          color = "white",            # Set bar border colors to white
          palette = "jco",            # jco journal color palett. see ?ggpar
          sort.val = "asc",           # Sort the value in dscending order
          sort.by.groups = TRUE,      # Sort inside each group
          x.text.angle = 90           # Rotate vertically x axis texts
          )

```
<img width="1152" height="1152" alt="image" src="https://github.com/user-attachments/assets/79cf24fb-306e-40bd-896f-33b13cbf0287" />



- Deviation graphs 偏差图

```r
# 计算mpg数据的z-score
dfm$mpg_z <- (dfm$mpg - mean(dfm$mpg))/sd(dfm$mpg)
# 根据z值将数据分成两份，赋予因子变量
dfm$mpg_grp <- factor(ifelse(dfm$mpg_z < 0, "low", "high"), 
                     levels = c("low", "high"))
# 检查数据
head(dfm[, c("name", "wt", "mpg", "mpg_z", "mpg_grp", "cyl")])


ggbarplot(dfm, x = "name", y = "mpg_z",
          fill = "mpg_grp",           # change fill color by mpg_level
          color = "white",            # Set bar border colors to white
          palette = "jco",            # jco journal color palett. see ?ggpar
          sort.val = "asc",           # Sort the value in ascending order
          sort.by.groups = FALSE,     # Don't sort inside each group
          x.text.angle = 90,          # Rotate vertically x axis texts
          ylab = "MPG z-score",
          xlab = FALSE,
          legend.title = "MPG Group"
          )

```
<img width="1152" height="960" alt="image" src="https://github.com/user-attachments/assets/a17d9a53-71f8-4423-b345-b34cf0b6074e" />


```r
# 旋转图形: 使用 rotate = TRUE 和 sort.val = “desc”
ggbarplot(dfm, x = "name", y = "mpg_z",
          fill = "mpg_grp",           # change fill color by mpg_level
          color = "white",            # Set bar border colors to white
          palette = "jco",            # jco journal color palett. see ?ggpar
          sort.val = "desc",          # Sort the value in descending order
          sort.by.groups = FALSE,     # Don't sort inside each group
          x.text.angle = 90,          # Rotate vertically x axis texts
          ylab = "MPG z-score",
          legend.title = "MPG Group",
          rotate = TRUE,
          ggtheme = theme_minimal()
          )

```
<img width="1248" height="1152" alt="image" src="https://github.com/user-attachments/assets/f9b1dcf1-94c9-4d4a-b6c4-60242d8e6f5d" />


- Dot charts   
lollipop（棒棒糖）图形是bar图的一种变体，用于可视化大批的数据。
```r
ggdotchart(dfm, x = "name", y = "mpg",
           color = "cyl",                                # Color by groups
           palette = c("#00AFBB", "#E7B800", "#FC4E07"), # Custom color palette
           sorting = "ascending",                        # Sort value in descending order
           add = "segments",                             # Add segments from y = 0 to dots
           ggtheme = theme_pubr()                        # ggplot2 theme
           )

```
<img width="1440" height="960" alt="image" src="https://github.com/user-attachments/assets/9c111a9f-14de-495e-8978-0c6f10a05b26" />


```r
降序排布. sorting = “descending”.
旋转图形, using rotate = TRUE.
组内部降序排布，使用mpg排布，组别用cyl区分
设置点的大小 dot.size = 6
使用mpg的值标记在点内label = round(dfm$mpg).

ggdotchart(dfm, x = "name", y = "mpg",
           color = "cyl",                                # Color by groups
           palette = c("#00AFBB", "#E7B800", "#FC4E07"), # Custom color palette
           sorting = "descending",                       # Sort value in descending order
           add = "segments",                             # Add segments from y = 0 to dots
           rotate = TRUE,                                # Rotate vertically
           group = "cyl",                                # Order by groups
           dot.size = 6,                                 # Large dot size
           label = round(dfm$mpg),                        # Add mpg values as dot labels
           font.label = list(color = "white", size = 9, 
                             vjust = 0.5),               # Adjust label parameters
           ggtheme = theme_pubr()                        # ggplot2 theme
           )


```
<img width="960" height="1440" alt="image" src="https://github.com/user-attachments/assets/f322f79b-f0fd-4071-a648-a8d521c388e3" />


```r
# 偏差图

# y = “mpg_z”
# 改变棒子的颜色和大小 add.params = list(color = “lightgray”, size = 2)

ggdotchart(dfm, x = "name", y = "mpg_z",
           color = "cyl",                                # Color by groups
           palette = c("#00AFBB", "#E7B800", "#FC4E07"), # Custom color palette
           sorting = "descending",                       # Sort value in descending order
           add = "segments",                             # Add segments from y = 0 to dots
           add.params = list(color = "lightgray", size = 2), # Change segment color and size
           group = "cyl",                                # Order by groups
           dot.size = 6,                                 # Large dot size
           label = round(dfm$mpg_z,1),                        # Add mpg values as dot labels
           font.label = list(color = "white", size = 9, 
                             vjust = 0.5),               # Adjust label parameters
           ggtheme = theme_pubr()                        # ggplot2 theme
           )+
  geom_hline(yintercept = 0, linetype = 2, color = "lightgray")

```
<img width="1440" height="960" alt="image" src="https://github.com/user-attachments/assets/12f87eaa-f7d8-425c-90a1-0eaf82b46e3b" />


```r
# Cleveland’s dot plot
# Color y text by groups. Use y.text.col = TRUE.
ggdotchart(dfm, x = "name", y = "mpg",
           color = "cyl",                                # Color by groups
           palette = c("#00AFBB", "#E7B800", "#FC4E07"), # Custom color palette
           sorting = "descending",                       # Sort value in descending order
           rotate = TRUE,                                # Rotate vertically
           dot.size = 2,                                 # Large dot size
           y.text.col = TRUE,                            # Color y text by groups
           ggtheme = theme_pubr()                        # ggplot2 theme
           )+
  theme_cleveland()                                      # Add dashed grids

```
<img width="960" height="1440" alt="image" src="https://github.com/user-attachments/assets/e975ee65-5414-46c6-a90e-073523d69920" />
