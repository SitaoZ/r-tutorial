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
在 R 中被赋值的所有内容都是对象。我们通常将 R 对象视为方法（或函数）可以对其执行操作的东西；但是，R 函数也是 R 对象。R 对象是分配给 R 内存的内容，属于特定类型或类。对象包括向量、列表、矩阵、数组、因子和数据框等。不要被术语所困扰。为了分配给内存，必须创建一个 r 对象。
```r
# Ris case sensitive， 不要使用R已存在的函数名作为对象名称
# <- 和 = 均可以
> ABC <- 'ATCG'
> abc = 'ATCG'
```
