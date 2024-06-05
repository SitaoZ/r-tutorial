## Reshape2
[reshape2](https://seananderson.ca/2013/10/19/reshape/)是用来实现数据的长宽格式之间相互转化的。

- 宽格式
```r
names(airquality) <- tolower(names(airquality))
head(airquality)
##    ozone solar.r wind temp month day
## 1    41     190  7.4   67     5   1
## 2    36     118  8.0   72     5   2
## 3    12     149 12.6   74     5   3
## 4    18     313 11.5   62     5   4
## 5    NA      NA 14.3   56     5   5
## 6    28      NA 14.9   66     5   6
```

- 长格式  
melt默认情况下，该函数会将所有的数字类型的列视作被转化的列。
```r
aql <- melt(airquality) # [a]ir [q]uality [l]ong format
head(aql)
##   variable value
## 1    ozone    41
## 2    ozone    36
## 3    ozone    12
## 4    ozone    18
## 5    ozone    NA
## 6    ozone    28
```

如果想要自己制定相应的列，则需要melt的时候指定id

```r
aql <- melt(airquality , id.vars = c("mouth", "day"))
head(aql)
##   month day variable value
## 1     5   1    ozone    41
## 2     5   2    ozone    36
## 3     5   3    ozone    12
## 4     5   4    ozone    18
## 5     5   5    ozone    NA
## 6     5   6    ozone    28
```

如果想在场格式中指定新变量和值的名称，则需要指定variable.name 和 value.name
```r
aql <- melt(airquality, id.vars = c("month", "day"),
            variable.name = "cirmate_variable",
            value.name = "climate_value")
head(aql)
##  month day cirmate_variable climate_value
##    1     5   1            ozone            41
##    2     5   2            ozone            36
##    3     5   3            ozone            12
##    4     5   4            ozone            18
##    5     5   5            ozone            NA
##    6     5   6            ozone            28
```

## cast 长格式转化为宽格式
reshape2中的cast有多种类型，其中dcast是使用的最多的类型，作用于dataframe

```r
aql <- melt(airquality, id.vars = c("month", "day")) # 宽格式变成长格式
aqw <- dcast(aql, month + day ~ variable) # 复原数据
                  -----------   ---------
                       |             |
ID variables(left side of formula)   Variable to swing into column names (right side of the formula)
head(aqw)

```
