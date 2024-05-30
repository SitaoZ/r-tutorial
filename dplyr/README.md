## dplyr tutorial
 dplyr
tidyverse中用与数据框操作的工具[dplyr](!https://dplyr.tidyverse.org/articles/dplyr.html)。
* [mutate](#mutate)  
* [transmute](#transmute)
* [select](#select)
* [filter](#filter)
* [summarise](#summarise)
* [arrange](#arrange) 


### mutate
生成新的变量，使用原有的变量进行计算或生成新的变量，在原数据框的基础上返回新的数据框。
可以进行数学运算。

```r
iris %>% mutate(Sepal.Squre = Sepal.Length * Sepal.Width, .keep="used") %>% head()

starwars %>%
  select(name, mass, species) %>%
  group_by(species) %>%
  mutate(mass_norm = mass / mean(mass, na.rm = TRUE))

starwars %>%
  select(name, mass) %>%
  mutate(
    mass2 = mass * 2,
    mass2_squared = mass2 * mass2
  )

```

### transmute
添加新的变量，但是返回仅保留新的变量的数据框。
### select
选择变量

```r
starwars %>% select(homeworld, height, mass)    # 取三列
iris %>% select(!c(Sepal.Length, Petal.Length)) # 取剩余的几列
starwars %>% select(name:mass)    # : 表示选择的跨度区间，包括首位
starwars %>% select(!(name:mass)) # ! 表示取反

iris %>% select(starts_with("Petal") & ends_with("Width")) # 选择以..开头和以..结尾的列
iris %>% select(starts_with("Petal") | ends_with("Width"))
iris %>% select(starts_with("Petal") & !ends_with("Width"))
```
### filter
过滤
### summarise
减少变量
### arrange
改变排序
