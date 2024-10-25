## clusterProfiler


## Mouse example
```r
BiocManager::install('clusterProfiler')
# https://guangchuangyu.github.io/cn/2018/07/clusterprofiler-ath/
# https://guangchuangyu.github.io/cn/2018/07/clusterprofiler-ath/
library(clusterProfiler)
library(enrichplot)
library(org.Mm.eg.db)
library(readxl)

setwd('C:/Users/zhusitao/Documents/R_workshop/')
gene672 <- read_excel('gene672.xlsx')
search_kegg_organism('ath', by='kegg_code')
search_kegg_organism('mmu', by='kegg_code')
search_kegg_organism('hsa', by='kegg_code')
#search_kegg_organism('osa', by='kegg_code')

genes <- c("ENSMUSG00000031073","ENSMUSG00000049124","ENSMUSG00000091721","ENSMUSG00000026614","ENSMUSG00000076934","ENSMUSG00000094006","ENSMUSG00000105906","ENSMUSG00000047517","ENSMUSG00000095041","ENSMUSG00000024222","ENSMUSG00000076563","ENSMUSG00000096410","ENSMUSG00000094546","ENSMUSG00000094797","ENSMUSG00000056836","ENSMUSG00000068452","ENSMUSG00000026928","ENSMUSG00000003955","ENSMUSG00000042808","ENSMUSG00000029561","ENSMUSG00000029084","ENSMUSG00000046688","ENSMUSG00000030017","ENSMUSG00000071356","ENSMUSG00000095630","ENSMUSG00000026880","ENSMUSG00000035692","ENSMUSG00000025203")
#genes <- c("Fgf15","Gm8186","Gimd1","Slc30a10","Iglv1","Igkv4-59","Iglc1","Dmbt1","AC149090.1","Fkbp5","Igkv5-48","Ighv1-19","Ighv1-26","Igkv6-15","Gm6851","Duox2","Card9","Fam162a","Gpx2","Oasl2","Cd38","Tifa","Reg3g","Reg3b","Igkv6-23","Stom","Isg15","Scd2")
genes <- gene672$gene_id


genes_entrezid <- mapIds(org.Mm.eg.db, keys = genes, keytype="ENSEMBL", column = "ENTREZID")
names(genes_entrezid)
#genes_entrezid = as.data.frame(as.matrix(genes_entrezid))
genes_entrezid = as.data.frame(genes_entrezid)


genes_alias <- mapIds(org.Mm.eg.db, keys = genes, keytype="ENSEMBL", column = "ALIAS")
genes_alias <- as.data.frame(genes_alias)
alias <- as.data.frame(genes_entrezid)

total_id <- merge(genes_alias, alias , by = 'row.names', all = TRUE)
write.csv(total_id, file = 'MAP_ID.csv')

library(dplyr)

genes_entrezid_no_NA <- filter(genes_entrezid, genes_entrezid != "NA")
entrezid_gene_list <- genes_entrezid_no_NA$genes_entrezid
entrezid_gene_list
is.vector(entrezid_gene_list)
# KEGG
ekegg_mmu <- enrichKEGG(entrezid_gene_list , organism="mmu", pvalueCutoff =0.05,pAdjustMethod = 'BH')
head(ekegg_mmu)
dotplot(ekegg_mmu)
kegg_out <- data.frame(ekegg_mmu)
write.csv(kegg_out, file='kegg672.csv', row.names = FALSE)

# GO
go_analysis_mouse <- enrichGO(entrezid_gene_list, ont = "BP", OrgDb = "org.Mm.eg.db", pvalueCutoff = 0.05)
head(go_analysis_mouse)
barplot(go_analysis_mouse)


# GSEA
library(readxl)
df <- read_excel("gene672.xlsx")
head(df)
dim(df)
df <- data.frame(df)
head(df)
rownames(df) <- df$gene_id
head(df)
df_merge <- merge(df, genes_entrezid, by = 'row.names', all = TRUE)
dim(df_merge)
df_merge <- filter(df_merge, genes_entrezid != "NA")
dim(df_merge)
total_id_no_NA <- filter(total_id, genes_entrezid != "NA")
our_geneList = df_merge$log2FoldChange.HCD_KO.HCD_WT.
names(our_geneList)= as.character(df_merge$genes_entrezid)

head(our_geneList)
our_geneList <- sort(our_geneList, decreasing = T)
head(our_geneList)
edo <- gseGO(our_geneList, ont = "ALL", OrgDb = "org.Mm.eg.db", keyType = "ENTREZID")
dotplot(edo, showCategory=30) + ggtitle("dotplot for ORA")
dotplot(edo2, showCategory=30) + ggtitle("dotplot for GSEA")

edo2 <- gseDO(our_geneList, organism = "mm")

# gene concept network 
## convert gene ID to Symbol
edox <- setReadable(edo, 'org.Mm.eg.db', 'ENTREZID')
categorys <- c("immune response", "production of molecular mediator of immune response",
               "immunoglobulin production", "immune system process")
p1 <- cnetplot(edox, foldChange=geneList)
cnetplot(edox, foldChange=geneList, showCategory = categorys)
## categorySize can be scaled by 'pvalue' or 'geneNum'
p2 <- cnetplot(edox, categorySize="pvalue", foldChange=geneList)
p3 <- cnetplot(edox, foldChange=geneList, circular = TRUE, colorEdge = TRUE) 
cowplot::plot_grid(p1, p2, p3, ncol=3, labels=LETTERS[1:3], rel_widths=c(.8, .8, 1.2))


p1 <- cnetplot(edox, node_label="category", 
               cex_label_category = 1.2) 
p2 <- cnetplot(edox, node_label="gene", 
               cex_label_gene = 0.8) 
p3 <- cnetplot(edox, node_label="all") 
p4 <- cnetplot(edox, node_label="none", 
               color_category='firebrick', 
               color_gene='steelblue') 
cowplot::plot_grid(p1, p2, p3, p4, ncol=2, labels=LETTERS[1:4])


# Tree plot
edox2 <- pairwise_termsim(edox)
p1 <- treeplot(edox2)
p2 <- treeplot(edox2, hclust_method = "average")
aplot::plot_list(p1, p2, tag_levels='A')


# Enrichmen analysis 
edox3 <- pairwise_termsim(edo)
p1 <- emapplot(edox3)
p2 <- emapplot(edox3, cex_category=1.5)
p3 <- emapplot(edox3, layout="kk")
p4 <- emapplot(edox3, cex_category=1.5,layout="kk") 
cowplot::plot_grid(p1, p2, p3, p4, ncol=2, labels=LETTERS[1:4])

# UpSet Plot 
upsetplot(edo)


#ridgeline
ridgeplot(edo)


#edo2 <- gseDO(our_geneList, organism = 'mm')

# GSEA
gseaplot2(edo, geneSetID = 1, title = edo$Description[1])
gseaplot2(edo, geneSetID = 2, title = edo$Description[2])

```
