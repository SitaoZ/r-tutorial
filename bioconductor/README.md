# Bioconductor 



##  org.Hs.eg.db
基因本体（GO）是基因集富集分析的标准来源。GO 包含三个命名空间：生物过程（BP）、细胞成分（CC）和分子功能（MF），从不同角度描述生物实体。
GO 术语和基因之间的关联集成在 Bioconductor 标准包中：生物注释包。在当前的 Bioconductor 版本（3.17）中，有以下生物包：

| Package      | Organism | Package | Organism |
| :--------  | :-----: | :----: | ----:|
| org.Hs.eg.db   |  Human       | org.Mm.eg.db    |  Mouse    |
| org.Rn.eg.db	 |  Rat	        | org.Dm.eg.db	  |  Fly       | 
| org.At.tair.db |  Arabidopsis	| org.Dr.eg.db	  |  Zebrafish |
| org.Sc.sgd.db  |	Yeast	      | org.Ce.eg.db	  |  Worm      |  
| org.Bt.eg.db	 | Bovine	      | org.Gg.eg.db	  |  Chicken   |
| org.Ss.eg.db	 | Pig	        | org.Mmu.eg.db	  |  Rhesus    |
| org.Cf.eg.db	 | Canine	      | org.EcK12.eg.db	| E coli strain K12 |
| org.Xl.eg.db	 | Xenopus	    | org.Pt.eg.db	  | Chimp | 
| org.Ag.eg.db	 | Anopheles	  | org.EcSakai.eg.db	| E coli strain Sakai |

生物体包的名称由四个部分组成。命名约定是：   
- org 仅表示“生物体”。    
- 第二部分对应于特定生物体，例如 Hs 表示人类，Mm 表示小鼠。   
- 第三部分对应于包中使用的主要基因 ID 类型，通常使用 eg，表示“Entrez genes”，因为数据主要从 NCBI 数据库中检索。  
  但是，对于某些生物体，主要 ID 可以来自其自己的主要数据库，例如酵母的 sgd 对应于酵母菌基因组数据库(Saccharomyces Genome Database)，即酵母的主要数据库。   
- 最后一部分始终是“db”，这仅表示它是一个数据库包。   
