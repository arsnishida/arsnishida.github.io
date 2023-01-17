---
layout: post
title:  "23-01-17, PAM50 UMAPs"
date:   2023-01-17
parent: sc_breastmultiome
nav_order: 2
---

Dir: `/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

Using the RNA portion of the data, Ryan used two methods, genefu PAM50 and sspbc, to classify cells into breast cancer types. The sspbc method switches some classification of Normal/LumA cells and does not try to classify Basal cells. The classifiations are projected onto the UMAP. Epiconfig and downsampling may not be doing better in clustering relative to Seurat's WNN but it does make more interpretable topics.

The following folders and versions of running Epiconfig were updated with additional sets of figures represented here labeled with the prefix: 'pass2'.
<br>[Filtered All Data - Hybrid](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EuUax-tXBi1Eo6Jfhk254toBJd1gSnfA-20-uu4dDxcBMQ)
<br>[Filtered Epithelial Only - Hybrid](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Eis4yIERrLhAlnsz2HUSelIBtshBmJ8dm8GUBXnBIJjL6w)
<br>[Filtered All Data - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EtnqwCDFdCxJmxDntpW0SGQBvZuKTBxpD_1FZ51srl0TqQ?e=O6jwL9)
<br>[Filtered Epithelial Only - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXO2dfK0MtFv-_rkp0u6LcBu2A26hnjo07nHpPILcsYUQ?e=RzsVaH)
<br>[Downsampled Epithelial - Hybrid](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Epoiz4ApATRCm2uUMHxEur0B5WBmy2k6gZMJcLJMYgJN4w?e=OVTwTp)
<br>[Downsampled Epithelial - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXBQObLCdVDn4HtPRepbW4BjOB6SIfEy_tWNv4RVY5HHg?e=kB1IC9)

Link to Relevant Presentations:
<br>[230117.epiconfig_sc_bcmultiome.pam50umap.pdf](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/EatjBmFLWY9OlF2tjiUZet0BHyFsPFvl7n4d_wKE0Z4TqA?e=EEurTJ)
