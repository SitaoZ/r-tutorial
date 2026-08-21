
## 1 安装软件
[参考该教材](https://zhuanlan.zhihu.com/p/663656839)

```bash
install.packages("survminer")
install.packages("survival")
```

## 2.数据

```bash
library(survminer) 
library(survival) 
data(lung)
head(lung)
summary(lung)# 查看数据集
#  inst time status age sex ph.ecog ph.karno pat.karno meal.cal wt.loss
#1    3  306      2  74   1       1       90       100     1175      NA
#2    3  455      2  68   1       0       90        90     1225      15
#3    3 1010      1  56   1       0       90        90       NA      15
#4    5  210      2  57   1       1       90        60     1150      11
#5    1  883      2  60   1       0      100        90       NA       0
lung$status <- lung$status - 1#转换成常见的0，1
lung$sex <- ifelse(lung$sex == 1,"Male","Female") 
surdata <- lung
surdata$Group <- surdata$sex
surdata$Group <- factor(surdata$Group,levels = c("Female","Male"))

```

首先使用Surv()函数创建生存对象，生存对象是将事件时间和删失信息合并在一起的数据结构。输出的生存对象中，带"+"号的表示删失数据。

```bash
attach(surdata) 
Surv(time,status) # 创建生存对象
```

接着再用survfit()函数根据生存对象拟合生存函数，这里用性别因子进行分组。

```bash
fit <- survfit(Surv(time,status) ~ sex,  data = lung) 
fit
#Call: survfit(formula = Surv(time, status) ~ sex, data = lung)
#             n events median 0.95LCL 0.95UCL
#sex=Female  90     53    426     348     550
#sex=Male   138    112    270     212     310
summary(fit)#使用summary()函数输出更多详细信息。
```


lung数据集包括男性138例，女性90例；男性和女性发生目标结局（死亡）事件分别有112例和53例，中位生存时间分别为270天和426天。

使用survdiff函数计算生存分析的p值；分组过多时，可以使用 pairwise_survdiff 函数进行两两组间比较计算p值：

```bash
head(colon)
#  id study      rx sex age obstruct perfor adhere nodes status differ extent surg node4 time etype
#1  1     1 Lev+5FU   1  43        0      0      0     5      1      2      3    0     1 1521     2
#2  1     1 Lev+5FU   1  43        0      0      0     5      1      2      3    0     1  968     1
#3  2     1 Lev+5FU   1  63        0      0      0     1      0      2      3    0     0 3087     2
#4  2     1 Lev+5FU   1  63        0      0      0     1      0      2      3    0     0 3087     1
#5  3     1     Obs   0  71        0      0      1     7      1      2      2    0     1  963     2
#6  3     1     Obs   0  71        0      0      1     7      1      2      2    0     1  542     1
table(colon$rx)
#    Obs     Lev Lev+5FU 
#    630     620     608
data.survdiff = survdiff(Surv(time, status)~rx, data = colon)
p.val = 1 - pchisq(data.survdiff$chisq, length(data.survdiff$n) - 1)   #计算P值
p.val#[1] 4.990735e-08

restest <- pairwise_survdiff(Surv(time,status) ~ rx,  data = colon)
restest
#Pairwise comparisons using Log-Rank test 
#data:  colon and rx 
#        Obs     Lev    
#Lev     0.78    -      
#Lev+5FU 3.3e-07 9.5e-07
restest$p.value
#                 Obs          Lev
#Lev     7.842255e-01           NA
#Lev+5FU 3.287592e-07 9.454331e-07

```

### 3.绘制生存曲线

使用ggsurvplot函数绘制前文生存函数相应的生存曲线，常见的需求包括添加p值、中位生存时间、置信区间与风险表等。

```bash
ggsurvplot(fit, # 创建的拟合对象
           data = lung,  # 指定变量数据来源
           conf.int = TRUE, # 显示置信区间
           pval = TRUE, # 添加P值
           surv.median.line = "hv",  # 添加中位生存时间线
           risk.table = TRUE, # 添加风险表
           risk.table.height=.25,
           xlab = "Time in days",
           ylab = "Overall Survival Probility",
           legend = c(0.8,0.75), # 指定图例位置
           legend.title = "", # 设置图例标题，这里设置不显示标题，用空格替代
           legend.labs = c("Female","Male"), # 指定图例分组标签
           break.x.by = 200,  # 设置x轴刻度间距
           palette = "hue")  # 自定义调色板 "grey","npg","aaas","lancet","jco", "ucscgb","uchicago","simpsons"和"rickandmorty"
```

添加总患者生存曲线：

```bash
ggsurvplot(fit, # 创建的拟合对象
           data = lung,  # 指定变量数据来源
           conf.int = TRUE, # 显示置信区间
           pval = TRUE, # 添加P值
           surv.median.line = "hv",  # 添加中位生存时间线
           risk.table = TRUE, # 添加风险表
           risk.table.height = .25,
           xlab = "Time in days",
           ylab = "Overall Survival Probility",
           #legend = c(0.8,0.75), # 指定图例位置
           legend.labs = c("All","Female","Male"), # 指定图例分组标签
           legend.title = "", # 设置图例标题，这里设置不显示标题，用空格替代
           break.x.by = 200,  # 设置x轴刻度间距
           add.all = TRUE) # 添加总患者生存曲线
```

### 4.生存曲线微调
ggsurvplot函数包括大量可调参数，更多的参数见文末：

```bash
ggsurv1 <- ggsurvplot(fit = fit,
                      pval = T, pval.size = 8, pval.coord = c(200, .25), # p-value的字体大小和位置.
                      conf.int = TRUE, # 显示生存曲线置信区间
                      conf.int.alpha = c(0.1), # 生存曲线置信区间透明度
                      surv.median.line = "hv", # 添加中位生存时间线；其中v绘制垂直线，h绘制水平线。
                      palette = "nejm", # 设置生存曲线的颜色，这里用的nejm配色
                      #xlim = c(0,3000), # 自定义x轴（时间）显示范围
                      #ylim = c(0, 1.02), # 自定义y轴（生存概率）显示范围
                      axes.offset = TRUE, # 坐标轴原点是否交叉
                      xlab = "Time in days", # x轴标签
                      ylab = "Overall Survival probability", # y轴标签
                      break.time.by = 200, # x轴（时间）间隔
                      surv.scale = "percent", # 生存概率用百分比显示
                      censor.shape="|", censor.size = 2, # 删失线形状和大小
                      legend = c(0.8, 0.8), # 改变legend的位置为c(0.8, 0.85),也可以设为"top","right"
                      legend.labs = c("Female","Male"), # 图例标签
                      legend.title = "",          # 图例标题
                      font.legend = c(18, "plain"),      # 图例字体设置
                      font.x = c(18, "plain", "black"),  # x轴标签字体设置
                      font.y = c(18, "plain", "black"),  # y轴标签字体设置
                      font.xtickslab = c(18, "plain", "black"),  # x轴刻度标签字体设置
                      font.ytickslab = c(18, "plain", "black"),  # y轴刻度标签字体设置
                      risk.table = T, # 显示risk.table
                      risk.table.title = "Number at risk", # risk.table标题
                      risk.table.fontsize = 4, # risk.table字体大小
                      risk.table.col = "strata", # risk.table文字颜色，strata与曲线图一致
                      risk.table.y.text = TRUE, # risk.table y轴显示文字，FALSE则显示色块
                      tables.y.text = FALSE,
                      risk.table.height = 0.25, # risk.table占图片比例
                      surv.plot.height = 0.75, # 生存图占图片比例
                      title = NULL, # 自定义主标题
                      font.main = c(18, "bold")) # 主标题字体设置
ggsurv1$table
ggsurv1$plot
ggsurv1
```

查看ggsurv1结构可以发现，生存曲线本身由多个图片（ggplot2格式）拼接而成，因此可以直接提取子图修改theme：
```bash
str(ggsurv1,1)
#List of 4
# $ plot          :List of 9
#  ..- attr(*, "class")= chr [1:2] "gg" "ggplot"
#  ..- attr(*, "parameters")=List of 14
# $ table         :List of 9
#  ..- attr(*, "class")= chr [1:2] "gg" "ggplot"
# $ data.survplot :'data.frame': 206 obs. of  10 variables:
#  ..- attr(*, "table")='data.frame':    2 obs. of  9 variables:
# $ data.survtable:'data.frame': 12 obs. of  10 variables:
# - attr(*, "class")= chr [1:3] "ggsurvplot" "ggsurv" "list"
# - attr(*, "heights")=List of 4
# - attr(*, "y.text")=List of 3
# - attr(*, "y.text.col")=List of 3
# - attr(*, "legend.position")= num [1:2] 0.8 0.8
# - attr(*, "legend.labs")= chr [1:2] "Male" "Female"
# - attr(*, "cumcensor")= logi FALSE
# - attr(*, "risk.table.pos")= chr "out"
# - attr(*, "axes.offset")= logi TRUE
ggsurv1$table#下层表格
ggsurv1$plot#上层曲线图
ggsurv1
```

这就不奇怪了，为什么生存曲线拼图与众不同：

```bash
splots <- list()
splots[[1]] <- ggsurv1
splots[[2]] <- ggsurv2
ggsurv <- arrange_ggsurvplots(splots, ncol = 2, nrow = 1, risk.table.height = 0.25, print = FALSE)
ggsurv

```


利用ggplot2格式修改theme
实际修改图片时我们发现，KM图下层表格很多地方很难调整，比如侧面分组标签的字体、大小，还是直接用theme修改方便。这里我们原图不显示p值，修改参数后添加HR与log-rank p值：

```bash
OS <- ggsurvplot(fit,palette = c("#2E9FDF","red"),
                 risk.table = TRUE,
                 risk.table.col = "strata", # Change risk table color by groups
                 risk.table.fontsize = 6, # risk.table字体大小
                 linetype = "strata", # Change line type by groups
                 pval = FALSE,conf.int = T,#pval.coord = c(0.15, 0.08),pval.size = 7,#fontsize=8,
                 legend = c(0.7, 0.8),
                 font.x = 16,font.y = 16,font.legend = 14,font.tickslab = 15, font.title = 18, 
                 surv.median.line = "hv",
                 break.time.by = 200,ggtheme = theme_bw(),
                 legend.title = "",
                 legend.labs= c(paste0("Male (N = ",as.numeric(table(surdata$Group)["Male"]),")"),
                                paste0("Female (N = ",as.numeric(table(surdata$Group)["Female"]),")")),
                 xlab ="Time in days",risk.table.height=.25,
                 ylab="Overall Survival Probility",
     title = NULL)
OS

###Cox回归计算HR
res.cox <- coxph(Surv(time,status)~Group, data = surdata)
coxSummary <- summary(res.cox)
coxSummary
#Call:
#coxph(formula = Surv(time, status) ~ Group, data = surdata)
#  n= 228, number of events= 165 
#            coef exp(coef) se(coef)     z Pr(>|z|)   
#GroupMale 0.5310    1.7007   0.1672 3.176  0.00149 **
#Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
#          exp(coef) exp(-coef) lower .95 upper .95
#GroupMale     1.701      0.588     1.226      2.36
#Concordance= 0.579  (se = 0.021 )
#Likelihood ratio test= 10.63  on 1 df,   p=0.001
#Wald test            = 10.09  on 1 df,   p=0.001
#Score (logrank) test = 10.33  on 1 df,   p=0.001
###计算log-rank P值
data.survdiff = survdiff(Surv(time, status)~Group, data = surdata)
p.val = 1 - pchisq(data.survdiff$chisq, length(data.survdiff$n) - 1)   #计算P值
p.val

library(ggplot2)
theme <- theme_bw() + 
         theme(plot.title = element_text(hjust = 0.5,size=20),
         axis.text.x = element_text(hjust = 0.5,size=16), 
         axis.text.y = element_text(size = 16),
         axis.title.x = element_text(size = 18), 
         axis.title.y = element_text(size = 18), 
         axis.line = element_line(colour = "black",size = 1), 
         legend.text = element_text(colour = "black",size = 16),
         legend.title = element_blank(),
         legend.position = c(0.65, 0.9))
OS$plot <- OS$plot + theme + 
           #在图上添加HR等信息：
           annotate("text", x = max(surdata$time)/50, y = 0.1,
                    label= bquote("HR = 1.7 (1.23-2.36)\t, " * italic(p) * " = 0.00149"),size = 6,hjust = 0) 
OS$table <- OS$table + theme + theme(plot.title = element_blank(),
                                     legend.position = "none")
```

拼接时个人习惯去除上半部分X轴的label与刻度线：



```bash
library(patchwork)
theme2 <- theme(axis.ticks.x = element_blank(), ## 删去所有刻度线
                axis.text.x = element_blank(),
                axis.title.x = element_blank())
plot.up <- OS$plot + theme2
plot.down <- OS$table
plot.up/plot_spacer()/plot.down + plot_layout(heights = c(5,-0.25,1))##调整中间空白

```

### 5.其他参数总结

```bash
linetype = 1 # 设置曲线线型。可以按"strata"设置线型；或按数字向量c(1, 2)、字符向量c("solid", "dashed")设置
conf.int # 逻辑词；默认FASLE；为TRUE则绘制曲线置信区间
pval = FALSE # 逻辑词；为TRUE则将统计检验计算的p值添加到图上；若为数字，则直接指定P值大小，如pval = 0.03；若为字符串，则添加字符串到图上，如pval = "p-value: 0.01"
pval.method  # 逻辑词，是否添加计算p值的统计方法的文本；只有当 pval = TRUE时, 才会在图上添加检验方法文本
surv.median.line # 在中位生存时间点处绘制水平或垂直线的字符向量；包括"none"、"hv"、"h"、"v"；其中v绘制垂直线，h绘制水平线。
risk.table = FALSE  # 逻辑词，图上是否添加风险表；
cumevents # 逻辑词，是否添加累积事件表
cumcensor # 逻辑词，是否添加累积删失表
tables.height = 0.25 # 生存曲线图下所有生存表的高度，数值在0-1之间
group.by  # 包含分组变量名称的字符向量，向量长度 ≤ 2
facet.by # 字符向量，指定绘制分面生存曲线的分组变量的名称
ggtheme=theme_survminer() # 设置ggplot2主题，如theme_bw()
tables.theme # 作用于生存表的ggplot2主题名称，有theme_survminer、theme_cleantable()等
add.all = FALSE # 逻辑词；是否添加总患者生存曲线到主生存图中
surv.plot.height # 生存图的高度，默认为 0.75；当risk.table = FALSE时忽略
title  # 图表标题
xlab, ylab # 分别指x轴和y轴标签
```

图例标题和位置：

```bash
legend # 指定图例位置："top","bottom","left","right","none"等。也可用数字向量c(x，y)指定位置，x和y的值应在0到1之间。
legend.title  # 图例标题，例如legend.title = "Sex"。
legend.labs # 指定图例标签的字符向量, 替换fit中strata的名称，顺序应与strata一致。 如 legend.labs = c("Male","Female")
```

置信区间：

```bash
##以下只有在conf.int = TRUE时才生效
conf.int.fill # 设置置信区间填充的颜色
conf.int.style # 设置置信区间的类型，有"ribbon"(默认),"step"两种。
conf.int.alpha # 数值，指定置信区间填充颜色的透明度；数值在0-1之间，0为完全透明，1为不透明。

```

p值文本大小和位置：
``bash 
##以下只有在pval = TRUE时才生效
pval.size # 指定p值文本大小的数字，默认为 5。
pval.coord # 长度为2的数字向量，指定p值位置x、y，如pval.coord=c(x,y)。
pval.method.size # 指定检验方法 log.rank 文本的大小
pval.method.coord # 指定检验方法 log.rank 文本的坐标
log.rank.weights # 计算log-rank检验p值的权重类型的名称。
```
