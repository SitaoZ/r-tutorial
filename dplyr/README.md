## dplyr tutorial
 dplyr
tidyverse中用与数据框操作的工具[dplyr](!https://dplyr.tidyverse.org/articles/dplyr.html)。
tidy data数据格式为 每列为变量，每行为一个样本。

* [group](#group)
* [mutate](#mutate)  
* [transmute](#transmute)
* [select](#select)
* [filter](#filter)
* [summarise](#summarise)
* [arrange](#arrange) 


### group 
```r
mtcars %>% group_by(cyl) %>% summarise(mean = mean(mpg), n = n())
# A tibble: 3 × 3
    cyl  mean     n
  <dbl> <dbl> <int>
1     4  26.7    11
2     6  19.7     7
3     8  15.1    14
```
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

```r
# 逻辑操作符号
==  等于
!=  不等于
>   大于
<   小于
<=  小于等于
>=  大于等于
is.na()  缺失值
!is.na() 非缺失值
%in%     在
!        非
|        或
&        与
xor()    只满足一个条件（|减去&）
```

```r
filter(starwars, species == "Human") # 选择其中species == "Human"的数据
filter(starwars, mass > 1000)        # 选择大于1000
filter(starwars, hair_color == "none" & eye_color == "black") # 多个条件
filter(starwars, hair_color == "none", eye_color == "black")

filter(starwars, hair_color == "none" | eye_color == "black") # 任意一个条件
starwars %>% filter(mass > mean(mass, na.rm = TRUE)) # 选择大于平均值
starwars %>% group_by(gender) %>% filter(mass > mean(mass, na.rm = TRUE)) # 选择大于特定类别平均值的列

# 使用变量名来直接选择
vars <- c("mass", "height")
cond <- c(80, 150)
starwars %>% filter (.data[[vars[[1]]]] > cond[[1]],
                    .data[[vars[[2]]]] > cond[[2]])

```

### summarise
减少变量

```r
mtcars %>%
  summarise(mean = mean(disp), n = n())  # 返回平均值和样本数

mtcars %>%
  group_by(cyl) %>%
  summarise(mean = mean(disp), n = n()) # 按组别分类，返回均值和样本数

mtcars %>%
  group_by(cyl) %>%
  summarise(disp = mean(disp), sd = sd(disp)) # 按组别求sd mean

```
### arrange
改变排序,使用某一列的内容对文件进行排序，也可以指定分组
```r
arrange(mtcars, cyl, disp)  # 对mtcars数据框，按照cyl和disp内容进行排序
arrange(mtcars, desc(disp)) # 按照disp倒序排列 Descending order

by_cyl <- mtcars %>% group_by(cyl) # 获取某一列的数据
by_cyl %>% arrange(desc(wt))       # 对某一列的数据进行排序
```
