---
layout: post
title:  "23-02-13, CancerSEA Plots w/ Epiconfig Clustesr"
date:   2023-02-13
parent: sc_breastmultiome
nav_order: 2
---

Dir: `/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

CancerSEA values across 14 categories were added to the data by Ryan. Violin plots were made against the three current Epiconfig models (full epithelial, downsampled epithelial, subset epithelial to Normal and LumA). These provide different clusters and subset by which the data is divided for potentially different distributions of CancerSEA values. In addition to the Epiconfig clusters, Travis' categories created by InfernCNV were also used for grouping.

The magnitudes in differences across CancerSEA values is slight though observable. Lingering questions I have are what do negative values represent, what is an expected magnitude of differences for which we should be looking, and are the values scaled similarly across the 14 categories? From eyeballing the figures, I pulled out some of them showing the most pronounced differences. Broadly, 'normal' epithelial cells have higher values relative to 'non-normal' cells in expected categories like epithelial-mesenchymal transition, quiescence, and stemness. After subsetting, intermediary clusters emerge and have higher scores in differentiation, cell cycle, and DNA repair.

The following folders have additional figures with the prefix 'pass4':
<br>[Epithelial Only - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXO2dfK0MtFv-_rkp0u6LcBu2A26hnjo07nHpPILcsYUQ?e=RzsVaH)
<br>[Downsampled Epithelial - Concatenate](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/EsXBQObLCdVDn4HtPRepbW4BjOB6SIfEy_tWNv4RVY5HHg?e=kB1IC9)
<br>[Normal and LumA Only - Concatenate, Full RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Ehq5vPkfII5Gn1ok69Q7SxwBy8g2MHXTfmHBFqHg336gJA?e=0pZAwg)

Link to Relevant Presentations:
<br>[230213.epiconfig_sc_bcmultiome.cancersea](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/EYxAMKfiiRZJnImSxDD6izoBn7rAEU0fG3Huw5ftQl2UXg?e=roXmvD)
