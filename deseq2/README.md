## DEseq2
RNAseq中差异表达分析常用的软件，[bioconductor tutorial](https://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html#how-to-get-help-for-deseq2)


## 安装
```bash
install.packages('BiocManager') 
BiocManager::install('DESeq2')
library("DESeq2")
```

## 使用
```bash
############# LYF 
library(DESeq2)
library(dplyr)

# ewat
all_count <- read.csv('gene_count2.csv', sep = ',')
head(all_count)
#                  Ewat1_1 Ewat1_2 Ewat2_1 Ewat2_2
# gene-Gm40198            0       4       5       0
# gene-Gm26708            7       2      12       3
# gene-Gm4601             5       0       0       0
# gene-LOC118567982       0       0       0       8
# gene-Mtfp1            164     793    1031    1207
# gene-LOC118567987       1       1       5       9

ewat12 <- dplyr::select(all_count, c("gene_id", "Ewat1_1", "Ewat1_2", "Ewat2_1", "Ewat2_2"))
head(ewat12)
rownames(ewat12) <- ewat12$gene_id
ewat12$gene_id <- NULL
head(ewat12)
dim(ewat12)
ewat12 <- ewat12[which(rowSums(ewat12) > 0),] # 数据预处理，去除在全部样本皆表达为0的基因
head(ewat12)


group_list <- c(rep("control",times = 2),rep("treat15",times = 2))        # 指定样本分组
coldata <- data.frame(row.names = colnames(ewat12),group_list=group_list) # 每一行对应一个样本，行名与countData的样本顺序一一对应，列为各种分组信息。
coldata

dds <- DESeqDataSetFromMatrix(countData=ewat12, colData=coldata, design=~group_list) # 构建DESeqDataSet对象
dds <- DESeq(dds)   # 标准化
res <- results(dds) # 将结果用results()函数来获取，赋值给res变量
# res <- results(dds,contrast = c("group_list","treat15","control"))
class(res)                # 查看res数据类型
res <- as.data.frame(res) # 用data.frame转化res格式为表格形式
res <- cbind(rownames(res),res)
head(res)
res0rdered <- res[order(res$padj),] # 对p值重新进行排序
head(res0rdered)                    # 查看res0rdered
DEG <- as.data.frame(res0rdered)
DEG <- na.omit(DEG)                 # 去除缺失值NA
DEG <- cbind(rownames(DEG), DEG) 
head(DEG)
colnames(res) <- c('gene_id',"baseMean" ,"log2FoldChange","lfcSE" ,"stat","pvalue","padj" )                    # res中，包含了基因id、标准化后的基因表达值（baseMean）、log2转化后的差异倍数（Fold Change）值、显著性p值以及校正后p值（默认FDR校正）等主要信息。
write.table(DEG,"ewat-control-treat15min.xls", sep = '\t', col.names = T, row.names = F, quote = FALSE, na='') # 文件保存，得到完整表格

# 获取padj（p值经过多重校验校正后的值）小于0.05，表达倍数取以2为对数后大于1或者小于-1的差异表达基因。
resSig <- res[which(res$padj < 0.05 & abs(res$log2FoldChange) > 1),]
head(resSig)
dim(resSig)
resSig[which(resSig$log2FoldChange > 0),'up_down'] <- 'up'   # 表达量显著上调的基因
resSig[which(resSig$log2FoldChange < 0),'up_down'] <- 'down' # 表达量显著下调的基因
head(resSig)
write.table(resSig, "ewat-control-treat15-0.05.xls", sep='\t',col.names=T,row.names=F,quote = FALSE,na='')     # 输出差异表达分析结果
```

## Count matrix input
```bash
library(DESeq2)
# BiocManager::install("pasilla")
library(pasilla)

directory <- system.file("extdata", package="pasilla",
                         mustWork=TRUE)

list.files(directory)
dataFiles <- file.path(directory,"pasilla_gene_counts.tsv")

dataPath <- system.file("extdata", package="DESeq2",
                        mustWork=TRUE)
file.copy(dataFiles, dataPath, overwrite = TRUE)

pasCts <- system.file("extdata",
                      "pasilla_gene_counts.tsv",
                      package="DESeq2", mustWork=TRUE)

pasAnno <- system.file("extdata",
                       "pasilla_sample_annotation.csv",
                       package="pasilla", mustWork=TRUE)
cts <- as.matrix(read.csv(pasCts,sep="\t",row.names="gene_id"))
coldata <- read.csv(pasAnno, row.names=1)
coldata <- coldata[,c("condition","type")]
coldata$condition <- factor(coldata$condition)
coldata$type <- factor(coldata$type)

head(cts,2)
col.order <- c("treated1","treated2","treated3","untreated1","untreated2", "untreated3", "untreated4")

cts <- cts[, col.order]

head(cts)
coldata

rownames(coldata) <- sub("fb", "", rownames(coldata))
all(rownames(coldata) %in% colnames(cts))

all(rownames(coldata) == colnames(cts))
library("DESeq2")
dds <- DESeqDataSetFromMatrix(countData = cts,
                              colData = coldata,
                              design = ~ condition)
dds

featureData <- data.frame(gene=rownames(cts))
mcols(dds) <- DataFrame(mcols(dds), featureData)
mcols(dds)

dds <- DESeq(dds)
res <- results(dds)
res

res <- results(dds, name="condition_treated_vs_untreated")
res <- results(dds, contrast=c("condition","treated","untreated"))

resultsNames(dds)

resLFC <- lfcShrink(dds, coef="condition_treated_vs_untreated", type="apeglm")
resLFC
```
