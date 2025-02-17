## 记录常用的画图脚本


### 分组柱状图 + P值 + error bar

```r
# 导入包
library(ggplot2)
library(readxl)
library(ggsci)
library(dplyr)
library(ggpubr)

# 读取数据
P4 <- read_excel('qPCR_expression.xlsx', sheet = 'P4')
head(P4)
dim(P4)

# 将数字分组变成因子变量，便于画图 float to string 
P4$Concentration <- factor(P4$Concentration, levels = c("0", "0.03", "0.3", "1"))

# 分组计算均值和标准差
P4.summary2 <- P4 %>%
  group_by(Gene, Source, Concentration) %>%
  summarise(
    sd = sd(Count),
    Count = mean(Count)
  )
P4.summary2

# 画图，指定颜色
ggplot(data = P4, aes(x = Concentration, y = Count, color = Source)) +
  geom_col(data = P4.summary2, 
           position = position_dodge(0.8), 
           width = 0.7, fill = "white") +
  geom_jitter(
    position = position_jitterdodge(jitter.width = 0.2, dodge.width = 0.8)
  ) + 
  geom_errorbar(
    data = P4.summary2,
    aes(ymin = Count-sd, ymax = Count+sd),  
    width = 0.2, position = position_dodge(0.8)
  ) +
  theme_classic() + 
  stat_compare_means(aes(group = Source), method = "t.test", paired = FALSE) +
  labs(y = "The relative mRNA level") + 
  facet_wrap(~Gene, ncol = 3) +
  scale_color_manual(values = c("#4DBBD5FF", "#E64B35FF"))
```

### 柱状图按值倒序排列
```bash
library(ggplot2)

gtex <- read.csv('gtex.txt', sep = '\t')
head(gtex)

ggplot(data = gtex, aes(x = reorder(Tissue, -nTPM), y = nTPM, fill = Tissue)) + 
  geom_bar(stat = 'identity')  + theme_classic() 
```
