## R-tutorial

1. RStudio 快捷键
```r
# 多行注释
Command + shift + c # Mac, Windows Ctrl
```

2. 检查安装包
```R
> .libPaths()                    # 显示library库所在的位置
> .packages(all.available=TRUE)  # 显示已经安装的包
> library()                      # 显示library库中的包
> search()                       # 显示当前环境已经导入的包
> R.version.string               # 显示当前的R版本
> R.Version()                    # R.Version
> sessionInfo()                  # 返回R会话的信息
> install.packages(package, repos="https://mirrors.tuna.tsinghua.edu.cn/CRAN") # 指定源安装包
```

- R环境的批量移动
```r
> # 保存
> installed.packages()     # 显示所有已经安装的包
> installed.packages()[,1] # 显示第一列，只显示安装包的名称
> Rpackages <- installed.packages()[,1]   # 赋予变量
> save(Rpackages, file="Rpackages.Rdata") # 保存变量

> # 导入
> data = load("Rpackages.Rdata") # 导入变量
> head(Rpackages)                # 查看变量
> for (i in Rpackages) install.packages() # 安装包
```

3. 管理R工作空间的函数
```r
> getwd('path')  # 显示当前工作目录
> setwd('path')  # 修改当前的工作目录为path
> ls()           # 列出当前工作目录的对象
> rm(objectlist) # 删除一个或者多个对象
> help(options)  # 显示可用选项的说明
> history(#)     # 显示最近使用过的#个命令
> (.packages())  # 列出已经导入的R包
> detach("package:dplyr") # 删除当前导入的dplyr包
> data(package="dplyr")   # 列出dplyr中的数据集
> ls("package:dplyr")     # 列出R包中的全部函数
> ls("package:stats")     # 列出R中的stats包的全部函数
> length(ls("package:stats")) # 查看stats中有多少函数

```
4. R中常用的帮助函数
```r
> help.start()  # 打开帮助文档首页
> help("foo")   # 查看函数foo的帮助信息
> ?foo          # 同上
```

5. 常用函数
```r
> View() # 以电子表格的形式查看数据集

```
6. 包安装
```r
> install.packages("package_name")
> install.packages("readr")
> install.packages(c("readr", "ggplot2"))

> if (!requireNamespace("devtools"))
    install.packages("devtools")
> devtools::install_github("TomKellyGenetics/leiden", ref = "master")
```


7. R-grammar
- 管道符
```r
> # R 中的管道符 %>%可以由dplyr包导入，也可以使用magrittr包导入
> library(dplyr)
> starwars %>% head()
```
- 条件判断
```r
> # 两个ifelse 叠加
> df$UP_DOWN <- ifelse(df$P_value <= 0.05, 
                     ifelse(df$`log2 FC` >= 2,ifelse(df$Gene == 'Malrd1','Bait','Interactor'), 'Unspecific'),
                     "Unspecific")
```
- header 重复解决
```bash
> # Method 1
> # 或者去读直接，df <- read.csv('a.csv', check.names = TRUE)


> # Method 2
> df <- read.csv('a.csv', check.names = FALSE)
> df
> # ID A A A B B B
> # 1 gene1 1 2 3 4 5 6
> # 2 gene2 2 3 4 5 6 7
> names(df) <- make.unique(names(df))
> df
> # ID A A.1 A.2 B B.1 B.2
> # 1 gene1 1   2   3 4   5   6
> # 2 gene2 2   3   4 5   6   7
```

8. 查看颜色
```r
> library("scales")
> show_col(pal_npg("nrc")(10))
> show_col(pal_npg("nrc", alpha = 0.6)(10))
```

9. 设置BioManager镜像
```r
getOption("repos") # 查看当前的镜像
install.packages("ABC", repos="http://mirror.tuna.tsinghua.edu.cn")
options("repos" = c(CRAN="http://mirrors:.tuna.tsinghua.edu.cn/CRAN"))
options(BioC_mirror = "http://mirrors.tuna.tsinghua.edu.cn.bioconductor")
```

10. R基本语法
- R 对象
在 R 中被赋值的所有内容都是对象。我们通常将 R 对象视为方法（或函数）可以对其执行操作的东西；但是，R 函数也是 R 对象。R 对象是分配给 R 内存的内容，属于特定类型或类。
对象包括向量、列表、矩阵、数组、因子和数据框等。不要被术语所困扰。为了分配给内存，必须创建一个 r 对象。
```r
# 创建对象 <- 和 = 均可以
# Ris case sensitive， 不要使用R已存在的函数名作为对象名称
> ABC <- 'ATCG'
> abc = 'ATCG'

# 删除对象
> rm(abc)

# 对象的类型查询
> mode(abc)
> typeof(abc)
> class(abc)
```
- 1-based
```r
# 向量 vector
> transcript_names <- c("TSPAN6","TNMD","SCYL3","GCLC")
> length(transcript_names)
> transcript_names[1] # 第一个元素 1-based
# TSPAN6
> transcript_names[length(transcript_names)] # 最后一个元素
# GCLC
```
- 向量 vector
```r
# 合并函数 combine function c()
> transcript_names <- c(transcript_names,"ANAPC10P1","ABCD1")
# "TSPAN6"    "TNMD"      "SCYL3"     "GCLC"      "ANAPC10P1" "ABCD1"

# 重命名 rename
> transcript_names[3]<-"NNAME"
> transcript_names[transcript_names == "ABCD1"]  <- "NEW"

# 过滤
> transcript_counts[transcript_counts > 260]

# 过滤掉 NAs
> transcript_counts[!is.na(transcript_counts)]   # 返回非NAs的值 
> which(is.na(transcript_counts))                # 返回NAs的索引 

#  操作 %in%
> transcript_names <- c("TSPAN6", "TNMD", "NNAME", "ANAPC10P1", "NEW")
> find_transcripts<-c("NNAME","ANAPC10P1")
> transcript_names[transcript_names %in% find_transcripts]
# "NNAME"     "ANAPC10P1"

# 保存
> saveRDS(transcript_counts,"transcript_counts.rds")
```

- 因子 factor   
它们是用于存储分类数据的专用向量（有序或无序）
```r
> sex <- factor(c("M","F","F","M","M","M"))
> levels(sex)
# [1] "F" "M"
```

- 列表 list
列表可以包含不同类型的多个元素
```r
# 创建列表list()
> My_exp <- list(c("N052611", "N061011", "N080611", "N61311" ), 
               c("SRR1039508", "SRR1039509", "SRR1039512",
                 "SRR1039513", "SRR1039516", "SRR1039517",
                 "SRR1039520", "SRR1039521"),c(100,200,300,400))
# 命名列表 names函数
names(My_exp)<-c("cell_lines","sample_id","counts")

# 列表循环 lapply()
# 创建一个函数（从每个向量中删除第一个索引）
> lapply(My_exp,function(x){x[-1]})  # 变量前面放置“-”以将其排除
```

- 数据框 dataframe
```r
# R 基础函数
> read.csv()
> read.table()
> read.delim() 
> readRDS() # 读取已存在的R对象

# 行数
> nrow()
# 行名
> rownames()

# 列数
> ncol()
# 列名
> colnames()

# 前几行
> head()
# 后几行
> tail()
```
- 数据框索引
```r
# 可以使用 [] [[]] $ 三个符号

# 使用 $
> head(df$sample)
# 使用 []
> head(df["sample"])

# 使用 [[]]
> head(df[["sample"]])
```

- 数据库子集 subsetting 
```r
> iris[2,4] # 返回第四列第二行的值

> iris[2, ] # 返回第两行的值

> iris[-1, ]  # 返回除了第一行之外的数据框; - 表示去除

> iris[1:4,1] # 返回第一列的1至4行，作为一个向量返回
> iris[1:4, ] # 返回数据框的1至4行，作为一个数据框返回

> iris[1:10,c("Sepal.Length","Sepal.Width")] # 直接用列名
> iris[iris$Species == "setosa",]            # 使用比较运算符

```

- %in%
%in% “返回一个逻辑向量，指示其左操作数是否匹配”。然后可以使用此逻辑向量将数据帧过滤为仅匹配的值。
```r
> keep_t<-c("CPD","EXT1","MCL1","LASP1")
> head(sscaled$transcript %in% keep_t)

```

- 使用tidyverse整理数据
tidy 数据有三条规则：（1）每个变量形成自己的列，（2）各个观测值形成一行，（3）每个值都有自己的单元格。
处理数据使用tidyverse集合包，它包括dplyr, ggplot2, forcats, tibble, readr, stringr, tidyr, and purr
```r
library(tidyverse)
# ── Attaching core tidyverse packages ────── tidyverse 2.0.0 ──
# ✔ dplyr     1.1.4     ✔ readr     2.1.5
# ✔ forcats   1.0.0     ✔ stringr   1.5.1
# ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
# ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
# ✔ purrr     1.0.2
```

- dplyr
dplyr 包是一个相当新的（2014 年）包，它试图为最常见的数据操作任务提供简单的工具。它被构建为直接处理数据框。它背后的想法很大程度上受到 plyr 包的启发，该包已经使用了一段时间，但在某些情况下速度很慢。dplyr 通过将大部分计算移植到 C++ 来解决这个问题。另一个功能是能够处理直接存储在外部数据库中的数据。这样做的好处是，数据可以在关系数据库中本地管理，可以对该数据库进行查询，并且只返回查询的结果。这解决了 R 的一个常见问题，即所有操作都在内存中进行，因此您可以处理的数据量受到可用内存的限制。数据库连接基本上消除了这一限制，因为您可以拥有一个超过 100 GB 的数据库，直接对其进行查询并提取 R 中分析所需的数据。--- datacarpentry.com

```r
> install.packages("dplyr") 
> library("dplyr")

# 取数据框子集
# 选取 gene/transcript, logFC, and FDR corrected p-value
# 第一个参数是数据框，后面参数为选择的列
> dexp_s <- select(dexp, transcript, logFC, FDR) 

# 我们还可以选择所有列，使用 - 或 ! 删除那些我们不感兴趣的列
> df_exp<-select(dexp, -feature)

# : 区间
> df_exp<-select(df_exp, transcript:FDR,albut)   

```
