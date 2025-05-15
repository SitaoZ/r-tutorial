## org.Hs.eg.db
Genome wide annotation for Human, primarily based on mapping using `Entrez Gene identifiers`.


## org.Mm.eg.db
Genome wide annotation for Mouse, primarily based on mapping using `Entrez Gene identifiers`.


## org.Rn.eg.db
Genome wide annotation for Rat, primarily based on mapping using `Entrez Gene identifiers`.

## org.Sc.sgd.db
Genome wide annotation for Yeast, primarily based on mapping using `ORF identifiers from SGD`.
酵母的全基因组注释，主要基于使用来自 SGD 的 ORF 标识符的映射。 [PDF](https://www.bioconductor.org/packages/release/data/annotation/manuals/org.Sc.sgd.db/man/org.Sc.sgd.db.pdf)

```bash
# Load the Annotation Package:
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("org.Sc.sgd.db")
library(org.Sc.sgd.db)


# Check Valid Keytypes and Columns
columns(org.Sc.sgd.db)  # Lists all retrievable data types
keytypes(org.Sc.sgd.db) # Lists valid keytypes (identifier types)

# Verify Your Keys:
# Suppose your keys are gene names (e.g., "CDC42"), use keytype="GENENAME".

# If using ORF IDs (e.g., "YLR256W"), use keytype="ORF".

# Check if "CDC42" is a valid key in GENENAME
"CDC42" %in% keys(org.Sc.sgd.db, keytype="GENENAME")

# Map GENENAME to ORF ID
mapIds(org.Sc.sgd.db, 
       keys = "CDC42", 
       column = "ORF", 
       keytype = "GENENAME")

# Use select() for More Details:
select(org.Sc.sgd.db, 
       keys = "YLR256W", 
       columns = c("GENENAME", "DESCRIPTION"), 
       keytype = "ORF")

```

```bash
"ORF" = c("gene2systematic","systematic_name"),
"DESCRIPTION" = c("chromosome_features","feature_description"),
"COMMON" = c("gene2systematic","gene_name"),
"INTERPRO" = c("interpro","interpro_id"),
"SMART" = c("smart","smart_id"),
"SGD" = c("sgd","sgd_id") )
```
