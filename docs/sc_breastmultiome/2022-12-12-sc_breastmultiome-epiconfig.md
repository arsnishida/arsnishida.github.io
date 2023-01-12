---
layout: post
title:  "22-12-12, Epiconfig Runs on Full and Subset Data"
date:   2022-12-12
parent: sc_breastmultiome
nav_order: 2
---

Dir: `/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

'Peaks' and 'RNA' matrices were obtained from input provided by Ryan Mulqueen. The hybrid method was used on the full dataset and hybrid, combinatorial, and concatenate methods were used on the filtered subset data. Data is also further subset into an epithelial-only subset. In each case, Epiconfig was run using 10, 20, 30, 40, 50, 60, 65, 70, 75, 80, 85, 90, 95, 100, 150, and 200 topics. A window size of 100bp was used to hybridize features, ATACseq data was transformed with TFIDF and filtered for 0.75 variable regions, and 2000 features were chosen from the RNAseq data after using the log normalization and not the Seurat standard clrNormalization due to memory errors going from sparse to dense.

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

Model selection was performed through selection on the elbow plots. UMAPs and heatmaps were made by grouping the molecularTypes (DCIS, ER+/PR-/HER2-, etc.), sample, newly called Seurat clusters, and EMBO labels. Topics are associated to clusters which are mostly clustering by sample.

The 'phase2' filtered subset data run on both all the cells and the epithelial cells using the concatenation method is the basis for downstream analysis. For comparison, the hybrid method is also used in the downstream analyses.

OneDrive Links to Figures:
<br>[All Data - Hybrid](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Eo-YR5utL4NOt5Sh8e0e49IBFNSQ_2H3fXNk_-4jehJMuw)
<br>[Epithelial Only - Hybrid](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EjrnkLrqukZEsl3bVvh3_pAB1kdIRRz3NG6FD7a3iI80zQ)
<br>[Filtered All Data - Hybrid](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EuUax-tXBi1Eo6Jfhk254toBJd1gSnfA-20-uu4dDxcBMQ)
<br>[Filtered Epithelial Only - Hybrid](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Eis4yIERrLhAlnsz2HUSelIBtshBmJ8dm8GUBXnBIJjL6w)
<br>[Filtered All Data - Combinatorial](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Ek7iPwmRunNOlATYRRnW1v8BOqUP6D51cYzrtvVY6DDD8Q)
<br>[Filtered Epithelial Only - Combinatorial](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EkjDVYeAmf9PkjlKZOIRMqgB6e9QAFxOZm9h0gt-MPNSxA)
<br>[Filtered All Data - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EtnqwCDFdCxJmxDntpW0SGQBvZuKTBxpD_1FZ51srl0TqQ?e=O6jwL9)
<br>[Filtered Epithelial Only - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXO2dfK0MtFv-_rkp0u6LcBu2A26hnjo07nHpPILcsYUQ?e=RzsVaH)

Link to Relevant Presentations:
<br>[Epiconfig 221103.epiconfig_sc_bcmultiome](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/EV_2HRANyjhIqGg-1o1xDmIBywtHlZRjFvfNJpgfprdUqA?e=eIzAi4)