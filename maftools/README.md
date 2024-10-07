# maftools
随着癌症基因组学的发展，MAF (Mutation Annotation Fromat)格式被广泛用于存储体细胞变异(somatic variants)。TCGA (The Cancer Genome Atlas)
项目启动后已经测序了超过30种癌症，每种癌症的样本数目都超过了200；测序的变异结果都已MAF的格式存储。maftools就是用来高效总结、分析、注释和可视化来至TCGA或者内部研究的MAF文件。

## 0 MAF 格式
[MAF](https://docs.gdc.cancer.gov/Data/File_Formats/MAF_Format/)
- 强制列名   
Hugo_Symbol, Chromosome, Start_Position, End_Position, Reference_Allele, Tumor_Seq_Allele2, Variant_Classification, Variant_Type and Tumor_Sample_Barcode.
- 推荐列名  
non MAF specific fields containing VAF (Variant Allele Frequency) and amino acid change information.

## 1 maftools 
[maftools manual](https://bioconductor.org/packages/release/bioc/vignettes/maftools/inst/doc/maftools.html#1_Introduction)

## 2 生成 MAF files
- VCF to MAF  
使用[vcf2maf](https://github.com/mskcc/vcf2maf)将VCF转变成MAF。新版本的GATK也可以实现。
- ANNOVAR注释文件   
如果VCF使用ANNOVAR注释，则可以使用maftools的annovarToMaf函数，将tabular分隔的文件转化成MAF

## 3 软件安装
```r
if (!require("BiocManager"))
    install.packages("BiocManager")
BiocManager::install("maftools")
```

## 4 maftools概览 

![image](https://github.com/user-attachments/assets/18d6ba4f-7c85-4394-92cb-96689f876712)
