---
layout: post
title:  "23-02-03, Normal Samples with Normal CNV Calls"
date:   2023-02-03
parent: sc_breastmultiome
nav_order: 2
---

Dir: `/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

The "Normal" called cells provided by Travis were added to the "Normal" defined samples by Ryan. These are samples RM_4, sample 15, and sample 19. Travis also supplied "Low" and "High" CNV categories for cells which were projected on the previous UMAPs.

"Low" and "High" CNV called cells seperately cleanly per sample in the Epiconfig UMAP relative to the Seurat UMAP though they aren't showing distinct topic enrichment. "Normal" called cells combined with the normal samples still cluster by sample and do not split by CNV category.

The following folders have the new subset of called normal samples and cells:
<br>[Called Normal Epithelial, 2000 RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/ElLC0hvGfQBOrdzGXekcJOsBDNhibkaWgumRb-_VDrAVlA?e=YVnb42)
<br>[Called Normal Epithelial, Full RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Eg03ZlrYwbRFrYgZULmSSEcB88fF6_Q48LHMONOWYSPRAg?e=2sY6Jm)

The following folders have additional figures with the prefix 'pass3':
<br>[Epithelial Only - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXO2dfK0MtFv-_rkp0u6LcBu2A26hnjo07nHpPILcsYUQ?e=RzsVaH)
<br>[Downsampled Epithelial - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXBQObLCdVDn4HtPRepbW4BjOB6SIfEy_tWNv4RVY5HHg?e=kB1IC9)
<br>[Normal and LumA Only - Concatenate, Full RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Ehq5vPkfII5Gn1ok69Q7SxwBy8g2MHXTfmHBFqHg336gJA?e=0pZAwg)

Link to Relevant Presentations:
<br>[230130.epiconfig_sc_bcmultiome.inferncnv](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/ER1ynfz0xlpGquCqKUdRNUwBsJ-7fdRG0ttKXZd5DOXzCQ?e=eeopWw)
