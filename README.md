## R-tutorial

0. RStudio 快捷键
```r
# 多行注释
Command + shift + c # Mac, Windows Ctrl
```

1. 检查安装包
```R
> .libPaths()                    # 显示library库所在的位置
> .packages(all.available=TRUE)  # 显示已经安装的包
> library()                      # 显示library库中的包
> search()                       # 显示当前环境已经导入的包
> R.version.string               # 显示当前的R版本
> R.Version()                    # R.Version
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

2. 管理R工作空间的函数
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
3. R中常用的帮助函数
```r
> help.start()  # 打开帮助文档首页
> help("foo")   # 查看函数foo的帮助信息
> ?foo          # 同上
```

4.常用函数
```r
> View() # 以电子表格的形式查看数据集

```
5.包安装
```r
> install.packages("package_name")
> install.packages("readr")
> install.packages(c("readr", "ggplot2"))

> if (!requireNamespace("devtools"))
    install.packages("devtools")
> devtools::install_github("TomKellyGenetics/leiden", ref = "master")
```


5.R-grammar
```r
> # R 中的管道符 %>%可以有dplyr包导入，也可以使用magrittr包导入
> library(dplyr)
> starwars %>% head()
```
