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
```

2. 管理R工作空间的函数
```r
> getwd('path')  # 显示当前工作目录
> setwd('path')  # 修改当前的工作目录为path
> ls()           # 列出当前工作目录的对象
> rm(objectlist) # 删除一个或者多个对象
> help(options)  # 显示可用选项的说明
> history(#)     # 显示最近使用过的#个命令
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

```
