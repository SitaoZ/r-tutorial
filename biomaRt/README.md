# biomaRt

- 查看基因的结构
```r
# 使用biomaRt查看ADAM21的所有转录本
library(biomaRt)

mart <- useMart("ensembl", dataset = "hsapiens_gene_ensembl", 
                host = "https://grch37.ensembl.org")

transcripts <- getBM(
  attributes = c("ensembl_transcript_id", "transcript_version",
                 "refseq_mrna", "cds_length"),
  filters = "hgnc_symbol",
  values = "ADAM21",
  mart = mart
)

print(transcripts)

```

- 查看SNP信息
```r
library(biomaRt)

head(rsid_list)
# [1] "rs10145740" "rs10162642" "rs10176901" "rs102275"   "rs10233329" "rs10474658"

mart_snp <- useMart(
  host = "https://grch37.ensembl.org",  # GLGC数据通常基于hg19/GRCh37
  biomart = "ENSEMBL_MART_SNP",
  dataset = "hsapiens_snp"
)
listAttributes(mart_snp)
bm_results <- getBM(
  attributes = c(
    "refsnp_id",           # rsID
    "chr_name",            # 染色体
    "chrom_start",         # 起始位置
    "chrom_end",           # 结束位置
    "allele",              # 等位基因变化
    "consequence_type_tv", # 【核心】变异类型：missense, synonymous, intron等
    "ensembl_gene_stable_id", # Ensembl基因ID
    "ensembl_gene_name"   # 基因名称
  ),
  filters = "snp_filter",    # 按rsID筛选
  values = rsid_list,
  mart = mart_snp
)
# 有 consequence_type_tv信息
print(bm_results)


```

