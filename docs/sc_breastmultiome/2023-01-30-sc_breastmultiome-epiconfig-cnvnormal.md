---
layout: post
title:  "23-01-30, CNV Normal Calls"
date:   2023-01-30
parent: sc_breastmultiome
nav_order: 2
---

Dir: `/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

Travis uploaded "Normal" cell calls per sample using consensus CNV calls and a dual cutoff of total number of CNVs and max consecutive CNVs for the labels. These calls were projected onto the Epiconfig UMAPs constructed from topics. Three resolutions were used: full epithelial data, downsampled epithelial data (to adjust for samples with 100x more cells than other samples), and a subset of the epithelial data using only samples/cells called "Normal" or "LumA" by Genefu PAM50 and/or SSPBC (same subset by either or both).

Normal-like cells by sample:
```
Sample	Normal-Like	Epithelial
sample 3	0	2729
sample 4	72	1937
sample 5	0	165
sample 6	0	64
sample 7	449	703
sample 8	1	54
sample 9	1	249
sample 10	280	3718
sample 11	19	2571
sample 12	3	649
sample 15	547	547
sample 16	78	105
sample 19	258	416
sample 20	3	118
```

Sample 7 and 16 by these calls and previous topic modeling have the most evidence for a mix of "Normal" and "Non-Normal" cells. Samples 15 and 19 are off by themselves in all UMAPs and are not annotated by Genefu PAM50 or SSPBC (messy data?). Other samples with "Normal" cells by CNV have relatively small fractions compared to the total cells in the dataset.

"Normal" cells do not separate from "Non-Normal" cells classified by CNV calls in UMAP clusters or differential topic enrichment at the level of epithelial and downsampled epithelial cells. The normal and LumA subset also shows no cluster seperation but does have specific enriched topics for "Normal" cells for Sample 7. Sample 7 is enriched for topic 20, 21, and 50 and "Normal" cells are relatively more enriched for 20 and 50. However, topic 20 and 50 contain overwhelmingly ATAC features and topic 21 is overwhelmingly RNA features. This may imply a bias for "Normal" cells by CNV for ATAC data though this is only from a extremely granular look at a single sample.

The following folders have additional figures with the prefix 'pass3':
<br>[Epithelial Only - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXO2dfK0MtFv-_rkp0u6LcBu2A26hnjo07nHpPILcsYUQ?e=RzsVaH)
<br>[Downsampled Epithelial - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXBQObLCdVDn4HtPRepbW4BjOB6SIfEy_tWNv4RVY5HHg?e=kB1IC9)
<br>[Normal and LumA Only - Concatenate, Full RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Ehq5vPkfII5Gn1ok69Q7SxwBy8g2MHXTfmHBFqHg336gJA?e=0pZAwg)

Link to Relevant Presentations:
<br>[230130.epiconfig_sc_bcmultiome.inferncnv](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/EegnUcRStVhBjgymcrjkAj4BDxQ_qGlfTDUeiWtS08DV2g?e=mQLUcs)
