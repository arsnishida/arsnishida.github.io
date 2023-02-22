---
layout: post
title:  "23-02-21, IGV PAM50"
date:   2023-02-21
parent: sc_breastmultiome
nav_order: 2
---

IGV browser tracks in bigwig format were made for each sample for ATAC and RNA. Shots were taken for each of the 50 PAM50 genes and a few extra as a reference. The only gene with a mildly changed name from the PAM50 list is ORC6L to ORC6 which was confirmed by Gene Cards to have changed. Tracks are ordered by Genefu PAM50 designations. Though single-cell, PAM50 classification is performed with bulked samples so these bulk tracks are reflecting the same information.

Tracks and IGV shots are shared [at this link here](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/ErW8ElcyhtBDub9DJCEFCJEB6fPhuWh0C0ivq9cFzX8W5w?e=YNISch)

Raw BAMs are scaled to 1M reads into a bedgraph, sorted, and transformed into a bigwig.
```sh
# variables
PREFIX=$1
REF="chrNameLength.txt"

# modules
module load bedtools
module load samtools
module load ucscGenomeBrowserKent

# calculate scaling factor
counts=$(samtools view -c ${PREFIX}.bam)
scale=1000000
scalingfactor=$(awk -v a=$scale -v b=$counts "BEGIN {print a/b}")

# make scaled bedgraph, sort, transform to bigwig
genomeCoverageBed -ibam ${PREFIX}.bam -bg -scale ${scalingfactor} -g ${REF} > temp.${PREFIX}.norm.bedgraph
sort-bed temp.${PREFIX}.norm.bedgraph > temp.${PREFIX}.norm.sorted.bedgraph
bedGraphToBigWig temp.${PREFIX}.norm.sorted.bedgraph ${REF} ${PREFIX}.bigwig
rm temp.${PREFIX}.norm.bedgraph
rm temp.${PREFIX}.norm.sorted.bedgraph
```

IGV tracks exist on the same scale (the scale vanishes at this resolution). Regions are zoomed out to the same relative scale though the number of bases per site will always vary per gene size.

Specific shots are presented here to show systemic issues with certain samples and expected trends:
<br>[230221.igv_sc_bcmultiome.pam50](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/ESvzmSLyoxVIvltPwtEzGVUBjflNFld_z4RC8OY77_SFZg?e=JRYYqf)

List of genes:
```
CONTROL: ACTB, MUC1, NECTIN2, CDH1
RYAN: ACTA2, KIT
PAM50: ACTR3B, ANLN, BAG1, BCL2, BIRC5, BLVRA, CCNB1, CCNE1, CDC20, CDC6, CDH3, CENPF, CEP55, CXXC5, EGFR, ERBB2, ESR1, EXO1, FGFR4, FOXA1, FOXC1, GPR160, GRB7, KIF2C, KRT14, KRT17, KRT5, MAPT, MDM2, MELK, MIA, MKI67, MLPH, MMP11, MYBL2, MYC, NAT1, NDC80,  NUF2, ORC6, PGR, PHGDH, PTTG1, RRM2, SFRP1, SLC39A6, TMEM45B, TYMS, UBE2C, UBE2T
```