### R

```bash
conda create -n RTCGA
conda install conda-forge::r-base==4.5.3
```
## RTCGA install

```bash
conda install \
    --override-channels \
    -c conda-forge \
    -c bioconda \
    --strict-channel-priority \
    bioconductor-rtcga=1.32.0


conda install \
    --override-channels \
    -c conda-forge \
    -c bioconda \
    --strict-channel-priority \
    bioconductor-rtcga=1.40.0


```

## RTCGA.clinical

```bash
# 成功
install.packages("RTCGA.clinical_20151101.42.0.tar.gz")
```

## RTCGA.mRNA
```bash
# 成功
install.packages("RTCGA.mRNA_1.40.0.tar.gz")
conda install \
    --override-channels \
    -c conda-forge \
    -c bioconda \
    --strict-channel-priority \
    bioconductor-rtcga.mrna=1.38.0

```
