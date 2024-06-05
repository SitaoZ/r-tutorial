## Reshape2
[reshape2](https://www.rdocumentation.org/packages/reshape2/versions/1.4.4)是用来实现数据的长宽格式之间相互转化的。

- 宽格式
```r
names(airquality) <- tolower(names(airquality))
head(airquality)
 ozone solar.r wind temp month day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6
```
