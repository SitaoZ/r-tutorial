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
```r
library(ggplot2)

gtex <- read.csv('gtex.txt', sep = '\t')
head(gtex)
# reorder 绘制条形图（按 nTPM 降序）
ggplot(data = gtex, aes(x = reorder(Tissue, -nTPM), y = nTPM, fill = Tissue)) + 
  geom_bar(stat = 'identity')  + theme_classic() 
```

### Heatmap 按指定的排序

```r
# 导入包
library(pheatmap)
library(dplyr)

setwd('C:/Users/zhusitao/Documents/R_workshop/project/QXX/species_table/')

df <- read.csv('taxa_table_RC.tsv', sep = '\t')
head(df)

# 列名重命名 
df <- df %>% rename("A_L1" = L1_A, "B_L1" = L1_B, "A_L4" = L4_A, "B_L4"=L4_B, "A_L7" = L7_A, "B_L7" = L7_B, 
                    "B_H1" = H1_B, "A_H2" = H2_A, "B_H2" = H2_B, "A_H3" = H3_A, "B_H3" = "H3_B", "B_H4" = H4_B,
                    "B_H5" = H5_B, "A_H7" = H7_A, "B_H7" = H7_B, "A_aL1" = aL1_A, "B_aL1" = aL1_B, "B_aL3" = aL3_B,
                    "A_aL4" = aL4_A, "B_aL4" = aL4_B, "A_aL6" = aL6_A, "B_aL6" = aL6_B, "B_aL7" = aL7_B, "B_aH1" = aH1_B, "A_aH2" =aH2_A,
                    "B_aH2" = aH2_B, "A_aH3" = aH3_A, "B_aH3" = aH3_B, "B_aH4" = aH4_B, "B_aH6" = aH6_B, "B_aH7" = aH7_B)
head(df)
dim(df)

# 去除不用的列
df$taxonID <- NULL

# 拆分旧列成新列
library(tidyr)
df <- df %>%
  separate(
    taxonomy,           # 要拆分的列
    into = c("Domain", "Phylum", "Class", "Order", "Family", "Genus", "Species"),  # 新列名
    sep = ";",          # 拆分依据的分隔符（可以是正则表达式）
    remove = TRUE       # 是否删除原始列（默认TRUE）
  )
# c("Domain", "Kingdom", "Phylum", "Class", "Order", "Family", "Genus", "Species")

                        
head(df)
df$taxonomy<- NULL
df <- filter(df, df$Species != "s_Unclassified")
dim(df)
head(df)

rownames(df) <- df$Species
df$Domain <- NULL
df$Phylum <- NULL
df$Class <- NULL
df$Order <- NULL
df$Family <- NULL
df$Genus <- NULL
df$Species <- NULL
head(df)
dim(df)

# 过滤行
df <- df[rowSums(abs(df)) > 10000, ]
dim(df)
head(df)

# 按行名排序
df_sorted <- df[order(rownames(df)), ]
# 按列名排序
df_sorted <- df_sorted[, order(colnames(df_sorted))]
dim(df_sorted)
pheatmap(df_sorted, scale = 'row', cluster_rows = F, cluster_cols = F)

```
