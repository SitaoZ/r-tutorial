## ClusterProfiler

### GO
[Gene Ontology（GO）](www.geneontology.org)是一个在生物信息学中广泛应用的知识体系，它通过一套标准化的词汇来描述基因和蛋白质在细胞中的功能。GO的目标是构建一个动态且受控的词汇表，以统一和规范基因及蛋白质功能的描述方式。GO将基因与蛋白质的功能划分为以下三个本体论类别：

- 1.分子功能（Molecular Function, MF）：指单个基因产物（如蛋白质或RNA）或其复合物在分子层面的活性，例如“催化活性”或“转运蛋白活性”。

- 2.细胞组分（Cellular Component, CC）：描述基因产物在细胞内的具体位置，如线粒体、核糖体等。

- 3.生物过程（Biological Process, BP）：指由多个分子活动协同完成的生物学过程，如信号转导或代谢过程。

GO中的每个术语都具有唯一的标识符（GO ID）和明确的名称，并通过“is_a”或“part_of”等关系与其他术语相互关联，共同构成一个有向无环图（DAG）结构。GO注释是将基因产物与相应GO术语相关联的过程，这一步骤对于理解基因功能以及开展基因表达分析至关重要。基于GO注释结果，可进行基因本体富集分析——这是一种统计方法，用于识别在一组基因中哪些GO术语的出现频率显著高于随机预期，从而帮助揭示该基因集合所涉及的生物学功能。

### KEGG
[KEGG](https://www.kegg.jp)（京都基因与基因组百科全书）是一个综合数据库资源，致力于从基因组和分子层面理解生物系统的高层次功能，涵盖细胞、生物体及生态系统等层次。其中，KEGG PATHWAY是该数据库最重要且使用最广泛的部分，它收录了大量基于文献研究、由人工绘制的通路图，涵盖代谢过程、环境信息处理、细胞过程、生物系统、人类疾病及药物开发等领域。

### 富集分析
富集分析（Enrichment Analysis）是一种统计方法，用于确定在一组感兴趣的基因、蛋白质或其他类型的生物分子中，哪些生物学过程、分子功能或细胞组分（如GO术语）显著地过度表达或富集。  
富集分析的主要步骤包括：
- 1.定义感兴趣的基因集：这通常是实验数据中显著变化的基因，或者是根据某种生物学标准选择的基因  
- 2.选择参考基因集：这是整个基因组或蛋白质组的背景集，用于与感兴趣的基因集进行比较  
- 3.选择适当的统计测试：常用的方法包括超几何分布测试、卡方检验、Fisher精确检验等  
- 4.计算富集比：这是感兴趣的基因集中特定类别的基因数与参考基因集中该类别基因数的比例  
- 5.调整多重比较：由于富集分析通常涉及多个假设测试，因此需要调整多重比较以控制假阳性率，常用的方法包括Bonferroni校正、FDR（False Discovery Rate）控制等  
- 6.解释结果：分析哪些生物学过程或功能类别显著富集，这有助于揭示基因集背后的生物学意义  

### emapplot 
```bash
library(DOSE)
data(geneList)
de <- names(geneList)[1:100]
x <- enrichDO(de)
x2 <- pairwise_termsim(x)
emapplot(x2)
```

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

keytypes(org.Mm.eg.db) # 查看数据的中的keytypes
genes_entrezid <- mapIds(org.Mm.eg.db, keys = genes, keytype="ENSEMBL", column = "ENTREZID")
names(genes_entrezid)
#genes_entrezid = as.data.frame(as.matrix(genes_entrezid))
genes_entrezid = as.data.frame(genes_entrezid)

# ID转换
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
ekegg_mmu <- enrichKEGG(entrezid_gene_list ,
                        organism="mmu",
                        pvalueCutoff =0.05,
                        pAdjustMethod = 'BH')
head(ekegg_mmu)
dotplot(ekegg_mmu)
kegg_out <- data.frame(ekegg_mmu)
write.csv(kegg_out, file='kegg672.csv', row.names = FALSE)

# GO
go_analysis_mouse <- enrichGO(entrezid_gene_list,
                              ont = "BP",
                              OrgDb = "org.Mm.eg.db",
                              pvalueCutoff = 0.05)
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

edo <- gseGO(our_geneList,
              ont = "ALL",
              OrgDb = "org.Mm.eg.db",
              keyType = "ENTREZID")
# 气泡图
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


## MSigDB + clusterProfiler


- MSigDB: Molecular Signatures Database
分子特征数据库 (MSigDB) 是一个包含数万个注释基因集的资源库，可与 GSEA 软件配合使用，分为人类和小鼠两类。
```bash
# human
H: hallmark gene sets
C1: positional gene sets
C2: curated gene sets
C3: motif gene sets
C4: computational gene sets
C5: GO gene sets
C6: oncogenic signatures
C7: immunologic signatures
C8	cell type signature gene sets
```

```bash
# mouse
MH	mouse-ortholog hallmark gene sets
M1	positional gene sets
M2	curated gene sets
M3	regulatory target gene sets
M5	ontology gene sets
M8	cell type signature gene sets
```
[msigdbr](https://igordot.github.io/msigdbr/articles/msigdbr-intro.html)
msigdbr R 包提供分子特征数据库 (MSigDB) 基因集，通常与基因集富集分析 (GSEA) 软件一起使用。

```r
library(msigdbr)
msigdbr_species() #

m_df <- msigdbr(species = "Homo sapiens") # 需要联网下载
head(m_df, 2) %>% as.data.frame

all_gene_sets <- msigdbr(species = "Mus musculus")
head(all_gene_sets)


h_gene_sets <- msigdbr(species = "mouse", collection = "H")
head(h_gene_sets)

cgp_gene_sets <- msigdbr(species = "mouse", collection = "C2", subcollection = "CGP")
head(cgp_gene_sets)

all_gene_sets |>
  dplyr::filter(gs_collection == "H") |>
  head()

# NCBI/Entrez IDs
msigdbr_t2g <- dplyr::distinct(msigdbr_df, gs_name, ncbi_gene)
enricher(gene = gene_ids_vector, TERM2GENE = msigdbr_t2g, ...)

# gene symbols
msigdbr_t2g <- dplyr::distinct(msigdbr_df, gs_name, gene_symbol)
enricher(gene = gene_symbols_vector, TERM2GENE = msigdbr_t2g, ...)

# fgsea
msigdbr_list <- split(x = msigdbr_df$gene_symbol, f = msigdbr_df$gs_name)
fgsea(pathways = msigdbr_list, ...)

# gsva
msigdbr_list <- split(x = msigdbr_df$gene_symbol, f = msigdbr_df$gs_name)
gsvapar <- gsvaParam(geneSets = msigdbr_list, ...)
gsva(gsvapar)
```
