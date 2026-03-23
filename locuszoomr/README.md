# locuszoomr
The locuszoomr package allows users to produce publication ready gene locus plots very similar to those produced by the web interface ‘locuszoom’ (http://locuszoom.org), but running purely locally in R. Plots can easily be customised, labelled and stacked.

These gene annotation plots are produced via R base graphics or ‘ggplot2’. A ‘plotly’ version can also be generated.

[manual](https://cran.r-project.org/web/packages/locuszoomr/vignettes/locuszoomr.html)

# install 
```r
# 依赖的包
if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")
BiocManager::install("ensembldb")
BiocManager::install("EnsDb.Hsapiens.v75")
BiocManager::install("AnnotationHub")


# 安装
install.packages("locuszoomr")

```

## 
```bash
library(locuszoomr)
data(SLE_gwas_sub)  ## limited subset of data from SLE GWAS
head(SLE_gwas_sub)


library(EnsDb.Hsapiens.v75)
loc <- locus(data = SLE_gwas_sub, gene = 'UBE2L3', flank = 1e5,
             ens_db = "EnsDb.Hsapiens.v75")
summary(loc)
locus_plot(loc)

# 使用最新的ENSEMBL 106数据库
library(AnnotationHub)
ah <- AnnotationHub()
query(ah, c("EnsDb", "Homo sapiens"))

ensDb_v106 <- ah[["AH100643"]]

# built-in mini dataset
data("SLE_gwas_sub")
loc <- locus(data = SLE_gwas_sub, gene = 'UBE2L3', fix_window = 1e6,
             ens_db = ensDb_v106)
locus_plot(loc)


```
