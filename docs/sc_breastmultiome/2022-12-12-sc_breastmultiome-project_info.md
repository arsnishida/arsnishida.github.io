---
layout: post
title:  "22-12-12, Project Info"
date:   2022-12-12
parent: sc_breastmultiome
nav_order: 2
---

# Ryan's OneDrive:
[hyperlink to OneDrive](https://mdandersonorg-my.sharepoint.com/personal/rmulqueen_mdanderson_org/_layouts/15/onedrive.aspx?ga=1&id=%2Fpersonal%2Frmulqueen%5Fmdanderson%5Forg%2FDocuments%2FMultimodal%20BC)

# Ryan's Directory:
`/home/groups/CEDAR/mulqueen/projects/multiome/220715_multiome_phase2/`

# ARSN's Directory:
`/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

# Input:
There are three main input files from Ryan.
<br>Original/Full - `/home/groups/CEDAR/mulqueen/projects/multiome/220715_multiome_phase2/phase2.QC.SeuratObject.rds`
<br>Filtered Subset - `/home/groups/CEDAR/mulqueen/projects/multiome/220715_multiome_phase2/phase2.QC.filt.SeuratObject.rds`
<br>Epithelial Subset (from Filtered Subset currently) - `/home/groups/CEDAR/mulqueen/projects/multiome/220715_multiome_phase2/epithelial.SeuratObject.rds`

# Output:
There are four data groups that were analyzed with Epiconfig.
<br>all - epiconfig run on the full dataset
<br>epithelial - epiconfig run on the original epithelial calls (the original R object is replaced by the new version, this version is obsolete)
<br>phase2_filt_sub - epiconfig run on the filtered dataset
<br>phase2_epithelial - epiconfig run on the filtered dataset's epithelial subset

# Refs:
GTF - `/home/groups/CEDAR/mulqueen/ref/refdata-cellranger-arc-GRCh38-2020-A-2.0.0`

# Scripts:
Text files with commands for data processing are labeled like, 'run.1_preprocessing.txt', starting with the prefix, 'run'.
<br>1 - Preprocessing and preparation of the matrices for Epiconfig.
<br>2 - Running Epiconfig.
<br>3 - Generating the elbow plot for model selection.
<br>4 - Plotting preliminary UMAP and heatmaps.
