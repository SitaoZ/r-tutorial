## NOIseq

NOIseq是RNA-Seq中差异表达分析的一种软件，可以适用于有重复的样本，也可以适用于无重复的样本。
[NOISeq](https://www.bioconductor.org/packages/release/bioc/vignettes/NOISeq/inst/doc/NOISeq.pdf)包括三个模块: 
(1) Quality control of count data; 
(2) Normalization and low-count filtering; 
(3) Differential expression analysis.

## 安装

```bash 
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("NOISeq")

browseVignettes("NOISeq")
```

## 使用

```
library(NOISeq)

mycounts <- read.csv('gene_count.csv')
rownames(mycounts) <- mycounts$gene_id
mycounts <- mycounts %>% filter(gene_id %in% gene_51790)
dim(mycounts)
head(mycounts)
mycounts <- mycounts[order(mycounts$gene_id),]

g_ids <- mycounts$gene_id
mycounts$gene_id <- NULL
head(mycounts)
dim(mycounts)
head(mycounts)

# 注意准备其他信息时，注意顺序，最好都按照基因名排布
library(biomaRt)
mouse <- useMart("ensembl", dataset="mmusculus_gene_ensembl")
# https://www.biostars.org/p/370234/
gene_coords <- getBM(attributes=c("ensembl_gene_id", "start_position","end_position","percentage_gene_gc_content", "chromosome_name", "gene_biotype"), filters="ensembl_gene_id", values=g_ids, mart=mouse)
gene_coords$length <- gene_coords$end_position - gene_coords$start_position
head(gene_coords)
dim(gene_coords)
gene_51790 <- gene_coords$ensembl_gene_id

# 基因长度
mylength <- dplyr::select(gene_coords, c("ensembl_gene_id", "length")) 
head(mylength)
mylength <- mylength[order(mylength$ensembl_gene_id),]
head(mylength)
head(mylength)
rownames(mylength) <- mylength$ensembl_gene_id
names_specific <- mylength$ensembl_gene_id
mylength$ensembl_gene_id <- NULL
#colnames(mylength) <- NULL
head(mylength)

mylength <- as.numeric(mylength$length)
head(mylength)
names(mylength) <- names_specific
class(mylength)

# 基因的GC含量
mygc <- dplyr::select(gene_coords, c("ensembl_gene_id", "percentage_gene_gc_content"))
head(mygc)
mygc <- mygc[order(mygc$ensembl_gene_id),]
rownames(mygc) <- mygc$ensembl_gene_id
mygc$ensembl_gene_id <- NULL
#colnames(mygc) <- NULL
#mygc <- t(mygc)
dim(mygc)
mygc <- mygc$percentage_gene_gc_content
head(mygc)
class(mygc)
names(mygc) <- names_specific

# 基因的类型
mybiotypes <- dplyr::select(gene_coords, c("ensembl_gene_id", "gene_biotype"))
mybiotypes
mybiotypes <- mybiotypes[order(mybiotypes$ensembl_gene_id),]
rownames(mybiotypes) <- mybiotypes$ensembl_gene_id
mybiotypes$ensembl_gene_id <- NULL
# colnames(mybiotypes) <- NULL
# mybiotypes <- t(mybiotypes)
dim(mybiotypes)
mybiotypes <- mybiotypes$gene_biotype
names(mybiotypes) <- names_specific

# 基因的位置
mychroms <- dplyr::select(gene_coords, c("ensembl_gene_id", "chromosome_name", "start_position","end_position"))
head(mychroms)
mychroms <- mychroms[order(mychroms$ensembl_gene_id),]
rownames(mychroms) <- mychroms$ensembl_gene_id
mychroms$ensembl_gene_id <- NULL
colnames(mychroms) <- c("Chr", "GeneStart", "GeneEnd")
dim(mychroms)

# 因子信息
myfactors = data.frame(Tissue = c("control", "15min", "60min", "2h"))
myfactors

# 查看准备好的数据
head(mycounts)
head(mylength)
head(mygc)
head(mybiotypes)
head(mychroms)

# 读取
mydata <- readData(data = mycounts, length = mylength, gc = mygc, biotype = mybiotypes,
                   chromosome = mychroms, factors = myfactors)


# 差异分析；注意conditions的顺序，前面是分子，后面是分母
myresults <- noiseq(mydata, factor = "Tissue", k = NULL, norm = "n", pnr = 0.2, conditions = c("15min", "control"),
                    nss = 5, v = 0.02, lc = 1, replicates = "no")

myresults
head(myresults@results[[1]])
# 无重复，q的值推荐使用0.9
# 上下调基因
myresults.deg = degenes(myresults, q = 0.9, M = NULL)
head(myresults.deg)

myresults.deg1 = degenes(myresults, q = 0.9, M = "up")
head(myresults.deg1)

myresults.deg2 = degenes(myresults, q = 0.9, M = "down")

head(myresults.deg2)

# 自带作图函数
DE.plot(myresults, q = 0.9, graphic = "expr", log.scale = TRUE)
DE.plot(myresults, q = 0.8, graphic = "MD")
DE.plot(myresults, chromosomes = c(1, 2), log.scale = TRUE, join = FALSE,
        q = 0.9, graphic = "chrom")
DE.plot(myresults, chromosomes = NULL, q = 0.9, graphic = "distr")

# 结果输出
write.csv(myresults.deg, "No_replicate_15min_control.csv")
write.csv(myresults.deg1, "No_replicate_15min_control_Up.csv")
write.csv(myresults.deg2, "No_replicate_15min_control_Down.csv")

# Please remember that, when using NOISeq, the probability of differential expression is not equivalent to
# 1 − pvalue. We recommend for q to use values around 0.8. If NOISeq-sim has been used because no replicates
# are available, then it is preferable to use a higher threshold such as q = 0.9. However, when using NOISeqBIO,
# the probability of differential expression would be equivalent to 1 − F DR, where F DR can be considered as an
# adjusted p-value. Hence, in this case, it would be more convenient to use q = 0.95.
```
