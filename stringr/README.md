## stringr
字符串并不是 R 中引人注目的组件，但它们在许多数据清理和准备任务中发挥着重要作用。stringr 包提供了一组功能齐全的函数，旨在让字符串的使用尽可能简单。如果您不熟悉字符串，最好的入门书是 R 数据科学中的字符串章节。
[北京大学数院](https://www.math.pku.edu.cn/teachers/lidf/docs/Rbook/html/_Rbook/regex.html)

### Install

```r
install.packages("tidyverse") # 最方便的安装方式就是安装整个tidyverse包

install.packages("stringr")   # 直接安装
```

### Usage

- 模式匹配
```r
str_detect(x, pattern) # 字符匹配
```


- str_view
可以在Rstudio中高亮显示匹配的部分
```r
x <- c("New theme", "Old times", "In the present theme")
str_view(x, "the")
```

- gsub
R中默认的匹配函数，需要添加`perl = TRUE`
```r
x <- c("New theme- ", "Old times", "In the present theme+-", "MO.6CII-")
str_view(x, "[+-]")
gsub(pattern = "[+\\-()]", replacement = "", x = x, perl = T)
```
