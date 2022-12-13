---
layout: post
title:  "22-12-12, Epiconfig on Full and Original Epithelial Subsets"
date:   2022-12-12
parent: sc_breastmultiome
nav_order: 2
---

Dir: `/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

'Peaks' and 'RNA' object were obtained from input provided by Ryan Mulqueen. Epiconfig was run on the data in a hybrid fashion using 10, 20, 30, 40, 50, 60, 65, 70, 75, 80, 85, 90, 95, 100, 150, and 200 topics. A window size of 100bp was used to hybridize features, ATACseq data was transformed with TFIDF and filtered for 0.75 variable regions, and 2000 features were chosen from the RNAseq data after using the log normalization and not the Seurat standard clrNormalization due to memory errors going from sparse to dense.

A full example of the used options for the call is below:
```sh
/home/groups/CEDAR/nishida/epiconfig/epiconfig/1_epiconfig.R \
    --method=hybrid \
    --workDir=/home/groups/CEDAR/nishida/SC_BREASTMULTIOME/all \
    --modelsOut=/home/groups/CEDAR/nishida/SC_BREASTMULTIOME/all \
    --rna=count_matrix.rna.rds \
    --atac=count_matrix.peak.rds \
    --geneAnno=/home/groups/CEDAR/anno/CellRanger/refdata-cellranger-arc-GRCh38-2020-A-2.0.0/genes/genes.gtf.gz \
    --windowSize=100 \
    --nFeatures=2000 \
    --nTopics=$ntopics \
    --filterName=TFIDF_var \
    --filterThresh=0.75 \
    --clrNorm=F
```

Model selection was performed through selection on the elbow plots. UMAPs and heatmaps were made by grouping the molecularTypes (DCIS, ER+/PR-/HER2-, etc.), sample, and newly called Seurat clusters. Topics are associated to clusters which are mostly clustering by sample.

A filtered version of this data was created which will be moved forward in lieu of this full dataset.

OneDrive Links to Figures:
<br>[All Data](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Eo-YR5utL4NOt5Sh8e0e49IBFNSQ_2H3fXNk_-4jehJMuw?e=aI4N4k)
<br>[Epithelial Only](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EjrnkLrqukZEsl3bVvh3_pAB1kdIRRz3NG6FD7a3iI80zQ?e=XQMwtA)