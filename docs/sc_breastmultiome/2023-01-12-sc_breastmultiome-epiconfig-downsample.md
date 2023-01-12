---
layout: post
title:  "23-01-12, Epiconfig Run on Downampled Epithelial"
date:   2023-01-12
parent: sc_breastmultiome
nav_order: 2
---

Dir: `/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

Relevant scripts are designated with the phrase, 'downsampled_epi'.

In general, clusters in UMAP space for the data has been overwhelmingly being driven by the cell's sample of origin. For the epithelial subset of cells, there are massive differences in number of cells from each sample. We are attempting to take the data and downsample the number of cells to be more equivalent across samples.

The data used is from filtered subset of epithelial cells using both the hybrid and concatenate methods with the rest being run similar to the previous full run. In each case, Epiconfig was run using 10, 20, 30, 40, 50, 60, 65, 70, 75, 80, 85, 90, 95, 100, 150, and 200 topics. A window size of 100bp was used to hybridize features, ATACseq data was transformed with TFIDF and filtered for 0.75 variable regions, and 2000 features were chosen from the RNAseq data after using the log normalization and not the Seurat standard clrNormalization due to memory errors going from sparse to dense.

Cells are downsampled to have a maximum of 530 cells per sample derived from the median number of cells per sample. The lowest sample had 78 cells and the highest had 12,764 cells making a median the simplest choice as to not eliminate extreme amounts of data. A full list looks like:
```
> table(input$sample)
     RM_1      RM_2      RM_3      RM_4  sample_1 sample_10 sample_11 sample_12 
      601       135       164       104       643     12764      3051      1438 
sample_15 sample_16 sample_19 sample_20  sample_3  sample_4  sample_5  sample_6 
      969       142       530       158      5040      8595       255        78 
 sample_7  sample_8  sample_9 
      918        85       338 
> median(table(input$sample))
[1] 530
> mean(table(input$sample))
[1] 1895.158
> table(input$sample)/530
      RM_1       RM_2       RM_3       RM_4   sample_1  sample_10  sample_11 
 1.1339623  0.2547170  0.3094340  0.1962264  1.2132075 24.0830189  5.7566038 
 sample_12  sample_15  sample_16  sample_19  sample_20   sample_3   sample_4 
 2.7132075  1.8283019  0.2679245  1.0000000  0.2981132  9.5094340 16.2169811 
  sample_5   sample_6   sample_7   sample_8   sample_9 
 0.4811321  0.1471698  1.7320755  0.1603774  0.6377358
```

The downsampling makes many of the lower represented samples cleanly separate from the rest of the samples. Clustering my molecular breast cancer type also improves and finds sample-specific topics within molecular type clusters.

OneDrive Links to Figures:
<br>[Downsampled Epithelial - Hybrid](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Epoiz4ApATRCm2uUMHxEur0B5WBmy2k6gZMJcLJMYgJN4w?e=OVTwTp)
<br>[Downsampled Epithelial - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXBQObLCdVDn4HtPRepbW4BjOB6SIfEy_tWNv4RVY5HHg?e=kB1IC9)

Link to Relevant Presentations:
<br>[230112.epiconfig_sc_bcmultiome.downsample](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/Efvjw8srqjBCpIpaoi1lLLwBaynv1CoEDlgXaWYFBzMtGg?e=yj6BYa)
