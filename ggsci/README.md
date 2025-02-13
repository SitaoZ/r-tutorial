## ggsci 

- 安装
```r
install.packages(ggsci)
library(ggsci)
```

- 显示npg配色
```r
#显示配色方案，以npg为例
install.packages("scales")
library("scales")
pal= pal_npg("nrc")(10)
show_col(pal)
```
