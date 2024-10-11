## readxl

[readxl](!https://readxl.tidyverse.org/)R中用于处理Excel信息的包。

### Install 
```r
install.packages("readxl")
```

### Usage

```r
library(readxl)
readxl_example()                             # readxl 提供了一些数据集
readxl_example("clippy.xls")                 # 实例数据地址

read_excel("a.xlsx")                         # 读取xls和xlsx文件，可以根据文件的后缀进行匹配。
excel_sheets("a.xlsx")                       # 列出sheet名称

read_excel(xlsx_example, sheet = "chickwts") # 使用sheet name 读取某一个sheet
read_excel(xls_example, sheet = 4)           # 使用索引读取某一个sheet
read_excel(xlsx_example, n_max = 3)          # 指定读取的行数
read_excel(xlsx_example, range = "C1:E4")    # 指定去读的列数
read_excel(xlsx_example, na = "setosa")      # 若果NA表示某种信息而不是纯粹的空包，则设置na参数

```


## openxlsx

```r
library(openxlsx)
openxlsx::write.xlsx(dataframe, file = "example.xlsx") # 写入Excel
```
