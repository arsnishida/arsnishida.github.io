---
layout: post
title:  "22-12-13, KLF4 BC Initial Processing"
date:   2022-12-13
parent: sc_klf4bc
nav_order: 2
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Data Locations
Single-cell KLF sequence data was processed by Ryan Mulqueen as described [at this link here](https://mulqueenr.github.io/10xmultiome_cellline/). For reference, the names of the core experiment and sequence runs are:
```
/home/groups/CEDAR/mulqueen/sequencing_data/EXP211227HM/211229_A01058_0202_AHT33MDRXY
/home/groups/CEDAR/mulqueen/sequencing_data/EXP211228HM/211229_A01058_0201_BHT55FDRXY
```
The main directory for his analysis is `/home/groups/CEDAR/mulqueen/projects/multiome/220111_multi`. Cellranger-arc was run against the references in `/home/groups/CEDAR/mulqueen/ref/refdata-cellranger-arc-GRCh38-2020-A-2.0.0`. Annotations within R were from EnsDb.Hsapiens.v86.

# Seurat Pre-Processing
A Seurat object was created for each of the four groups creating the initial RNA and ATAC assays and then merged together: "YW_si_Control_plus_E2", "YW_si_Control_plus_Veh", "YW_si_KLF4_plus_E2", and "YW_si_KLF4_plus_Veh". The resulting merged object is saved as `yw_merged.SeuratObject.rds` and the following QC and filtering was performed below thresholding for cells with between 500 to 25000 RNA counts and 500 to 100000 ATAC counts, nucleosome signal less than 2, and TSS enrichment greater than 1:
```
# before
table(dat$sample)
# YW_si_Control_plus_E2 YW_si_Control_plus_Veh     YW_si_KLF4_plus_E2
#                  7357                   6435                   7727
#   YW_si_KLF4_plus_Veh
#                  6785

# filter out low quality cells
dat <- subset(
  x = dat,
  subset = nCount_ATAC < 100000 &
    nCount_RNA < 25000 &
    nCount_ATAC > 500 &
    nCount_RNA > 500 &
    nucleosome_signal < 2 &
    TSS.enrichment > 1
)

# after
table(dat$sample)
# YW_si_Control_plus_E2 YW_si_Control_plus_Veh     YW_si_KLF4_plus_E2
#                  7030                   6023                   7552
#   YW_si_KLF4_plus_Veh
#                  6350
```

# Seurat Multimodality
New peaks are called within Seurat with macs2 in a new assay called 'peaks' (a default call with no extra parameters). RNA undergoes sctransform normalization and turned into a UMAP with PCA reduction. Peaks are filtered with FindTopFeatures with a minimum cutoff of 5 occurences, transformed via TFIDF and decomposed using SVD creating the LSI matrix and discarding the first dimension. The pca and lsi reductions are used to find multi-modal neighbors in Seurat creating its weighted nearest neighbors for the multimodal UMAP. Clusters are called at 0.8 resolution on the WNN UMAP.

[Multimodal UMAP Comparisons](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/EVCENXD5HTJEppXNYLG4hz8B-cln_e070ejdR7oGYlK_gw?e=7XasZs)
<br>[Merged QC Info by Condition](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/EUzDLaV3RwNFnY5NVsJ3wV8BceL_vC0RLHH5XV90r82ORQ?e=cOR4um)

# InferCNV

InfernCNV is used to call CNVs using a gene list re-ordered by chr and start positions. The call is made multiple times due to hanging:
```
#run infercnv
infercnv_obj = CreateInfercnvObject(raw_counts_matrix="YW_inferCNV.counts.txt",
    annotations_file="YW_inferCNV.annotation.txt",
    delim="\t",
    gene_order_file="YW_inferCNV.gene_order.txt",
    ref_group_names=NULL)
saveRDS(infercnv_obj,file="YW_inferCNV.Rds")

#read in where inferCNV keeps hanging (step 14)
infercnv_obj<-readRDS("YW_inferCNV.Rds")
infercnv_obj = infercnv::run(infercnv_obj,
    cutoff=0.1, # cutoff=1 works well for Smart-seq2, and cutoff=0.1 works well for 10x Genomics
    out_dir="./YW_inferCNV", 
    denoise=TRUE,
    HMM=FALSE,
    num_threads=10,
    cluster_references=FALSE,
    resume_mode=FALSE
)
#other options (removed for now)
#k_obs_groups=2,
#cluster_by_groups=FALSE, 
#HMM_type="i6",
#output_format="pdf")
saveRDS(infercnv_obj,file="YW_inferCNV.Rds")
```
At this point, the results from inferCNV are saved back into `yw_merged.SeuratObject.rds` and split by cell-type into `yw_mcf7.SeuratObject.rds` and `yw_t47d.SeuratObject.rds`. Additionally, a control-only subset is created (presumably for Travis).

[InferCNV Heatmap](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/EZ1dN-EaIMxGgkiLBsBdAV4BEevhXC7lJkYrvzBdl1OzSQ?e=4jHuAX)
<br>[InferCNV Celltype UMAP](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/Ebg35uxpRttEnMVP9cOHlLcBi9YEROm1zwcN4ZETZl-Kzw?e=EYPdRe)
<br>[InferCNV Celltype UMAP, control only](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/EQSIBYvmHgdHnJfoLZvjGsABA5Jn4u9M2AcGjRQdnx19LQ?e=GBPEsp)

# XOXO
