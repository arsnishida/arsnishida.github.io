---
layout: post
title:  "23-02-07, Feature Sets"
date:   2023-02-07
parent: sc_klf4bc
nav_order: 2
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# 0. What is Feature Sets?
Identification of chromatin features in a dataset can easily generate 100K-1M features. To understand the biological relevancy of features and for practical purposes, features will almost always be treated into subset groups. Generally in the single-cell context, subsets of the full feature list attempt to represent variablity within cells and describing features within and between a collection of feature sets is meant to capture biological differences in data. Internally, the only available information of a feature is the region itself usually represented in a BED format of chromosome, start, and stop. Biologically relevancy of the features must be derived from the overlap of these features to outside references. Additionally, in the context of creating subsets of features via topic modeling, biological enrichment must be evaluated relative to the full set of features and other topics. Topic modeling is further complicated by the choice of lamba and number of features to look at per topic. An additional step with Epiconfig is the incorporation of multiomic data and the relationships of this data with topics can further be probed.

This is an example of parsing the topics from the multiomic MCF7 KLF data. Reference files will always need to be gathered, curated, and processed differently for different data sets. Where possible, the global feature list is used leveraging the fact that any other feature set (features within the vocab object or features per topic) will be a subset of the global feature list. Specifically, a global feature list is used, subset to a set of variable features used as the full vocab list for Epiconfig, and then features are attributed to topics using different lambdas.

# 1. Set Environment

## 1a. Software Modules
Any versions should be work. PLACEHOLDER - add more downstream software.
```sh
# software modules
module load bedops/2.4.41
module load R/4.2.2
```

## 1b. Libraries
PLACEHOLDER - print a list and move to exacloud.
```sh
export R_LIBS_USER='/home/groups/ravnica/env/R_library_backups/R-4.2.2_copy'	
```

## 1c. Populate Working Directory
```sh
cd /home/groups/ravnica/users/nishida/temp_features
mkdir figures; mkdir matrices; mkdir refs; mkdir scripts; mkdir tmp
cd matrices/
mkdir 1_distance; mkdir 2_geneinfo; mkdir 3_correlations; mkdir 4_enrichment_subset; mkdir 5_enrichment_set; mkdir 6_enrichment_superset
cd ..
```

---

# 2. Populate Data, References, and Scripts

## 2a. Populate Data
Data includes the full R object of the complete Seurat multiomics dataset split and fed into Epiconfig, the vocab matrix, and the LDA model with desired number of topics.
```sh
cp /home/groups/CEDAR/nishida/epiconfig/MCF7_KLF4_wTopics_SO.rds .
cp /home/groups/CEDAR/nishida/epiconfig/klf4/CLRNorm_100kb_2000Feats_TFIDFvar75_Model/vocab_matrix.rds .
cp /home/groups/CEDAR/nishida/epiconfig/MCF7/CLRNorm_100kb_2000Feats_TFIDFvar75_Model/default_lda_out/lda_65topics.rds .
```

## 2b. Populate References
These will always be genome and data dependent. In the future we will start curating these in a centralized location but for now pull them from whenever. Here I am copying across drives on purpose (exacloud to ravnica) but normally these should be symbolic links `ln -s`.
```sh
cd refs/
#################
# Genome Specific
#################
# GTF, this is an old HAVANA/ENSEMBL GTF default packaged with cellranger missing annotations (like 5' and 3' UTR)
cp /home/groups/CEDAR/anno/CellRanger/refdata-cellranger-arc-GRCh38-2020-A-2.0.0/genes/genes.gtf.gz .
# GTF, more recent GTF with more discrete annotation
cp /home/groups/CEDAR/nishida/refs/hg38/hg38_ucsc/hg38.ensGene.gtf .
# ENSEMBL TSS
cp /home/groups/CEDAR/nishida/refs/hg38/hg38_ensembl_tss/ensembl.hg38.tss.chr.bed . 
# DHS Index
cp /home/groups/CEDAR/nishida/refs/hg38/hg38_DHS_index/DHS_Index_and_Vocabulary_hg38_WM20190703.bed .
# Motifs
cp /home/groups/CEDAR/nishida/refs/fimo_scans/cat_hg38.bed .
#################################
# Cell-Type/Tissue/Data Specific:
#################################
# active enhancer mark, H3K27ac
cp /home/groups/CEDAR/nishida/SC_KLF4BC/refs/H3K27Ac_intersect_all3.bed .
# superenhancers
cp /home/groups/CEDAR/nishida/SC_KLF4BC/refs/Homer_SE_MCF7_sg_NT_1.bed .
# CCANs
cp /home/groups/CEDAR/nishida/SC_KLF4BC/refs/MCF7.CCANs.txt .
# relevant cistrome data
# here I'm pulling out relevant cistrome subsets by the "Tissue_type" column matching "Breast"
# and then collapsing into new names by "Factor", "Cell_line", and "Cell_type" columns
# change these based on your desired subset and query
# I'm removing spaces and backslashes but other annotations might need more character changes
mkdir cistrome
cat /home/groups/CEDAR/mulqueen/ref/cistrome/human_factor_full_QC.txt | cut -f 1,4-7 | awk '$5 == "Breast"' | cut -f 1-4 | sed -e 's/\t/_/2g' | sed -e 's/\//-/g' | sed -e 's/ //g' > cistrome/cistrome_subset.txt
while IFS= read -r line
do
	a=$(echo "$line" | cut -f 1)
	b=$(echo "$line" | cut -f 2)
	cp /home/groups/CEDAR/mulqueen/ref/cistrome/human_factor/${a}_sort_peaks.narrowPeak.bed cistrome/${a}_${b}.bed
done < cistrome/cistrome_subset.txt
cd ..
```

---

# 3. Create Feature Lists

## 3a. Features from All
Pass the input path and output prefix. Seurat object assay names may not be standardized, adjust accordingly.
```sh
Rscript scripts/pull_features.all.Rscript MCF7_KLF4_wTopics_SO.rds features.all
```

Content of `scripts/pull_features.vocab.Rscript`:
```R
# !/usr/bin/R

library(Signac)

args <- commandArgs(trailingOnly = T)
input_path <- args[1]
output_prefix <- args[2]

input_model <- readRDS(input_path)
rna <- rownames(input_model@assays$RNA)
atac <- rownames(input_model@assays$ATAC)

write.table(rna,paste(output_prefix,".rna.txt",sep=""),sep="\t",quote=F,row.names=F,col.names=F)
write.table(atac,paste(output_prefix,".atac.txt",sep=""),sep="\t",quote=F,row.names=F,col.names=F)
```

## 3b. Features from Vocab
Pass the vocab matrix input path and the output prefix.
```sh
Rscript scripts/pull_features.vocab.Rscript vocab_matrix.rds features.vocab
```

Content of `scripts/pull_features.vocab.Rscript`:
```R
# !/usr/bin/R

library(Matrix)

args <- commandArgs(trailingOnly = T)
input_path <- args[1]
output_prefix <- args[2]

input_model <- readRDS(input_path)
features <- row.names(input_model)

# split features
atac_features <- features[which(grepl("^chr",features))]
hybrid_features <- features[which(grepl("_ON$",features))]
rna_features <- features[! features %in% c(atac_features,hybrid_features)]

# check feature counts
sum_features <- length(atac_features) + length(hybrid_features) + length(rna_features)
if (sum_features != length(features)) {
	print("Feature splitting by feature names does not add to the total number of features. Check feature names.")
	quit()
}

write.table(atac_features,paste(output_prefix,".atac.txt",sep=""),row.names=F,col.names=F,quote=F,sep="\t")
write.table(rna_features,paste(output_prefix,".rna.txt",sep=""),row.names=F,col.names=F,quote=F,sep="\t")
write.table(hybrid_features,paste(output_prefix,".hybrid.txt",sep=""),row.names=F,col.names=F,quote=F,sep="\t")
```

## 3c. Features from Topics
For every lambda in the range of 0 to 1 in increments of 0.05, pull top words/features. Pass the lda model, lambda, max words, and output path. Number of max words here is 2000 but adjust accordingly to length of full vocab.
```sh
array=(0.05 0.10 0.15 0.20 0.25 0.30 0.35 0.40 0.45 0.50 0.55 0.60 0.65 0.70 0.75 0.80 0.85 0.90 0.95 1.00)
for lambda in "${array[@]}"
do
	mkdir topwords_${lambda}
	Rscript scripts/lambda_top_words.Rscript lda_65topics.rds ${lambda} 2000 topwords_${lambda}
done
```

Content of `scripts/lambda_top_words.Rscript`:
```R
# !/usr/bin/R

args <- commandArgs(trailingOnly = T)
input_path <- args[1]
lambda <- args[2]
maxwords <- args[3]
output_path <- args[4]

selected_model <- readRDS(input_path)
num_topics <- nrow(selected_model$topic_word_distribution)

# print topwords per topic
for (i in 1:num_topics) {
	top_words_ldavis <- selected_model$get_top_words(n = length(selected_model$.__enclos_env__$private$vocabulary), #61299,
		topic_number = as.numeric(i), # 1L:selected_model$.__enclos_env__$private$n_topics,
		lambda = as.numeric(lambda))[1:maxwords]
	filename <- paste(output_path, "/topic_",i,".txt",sep="")
	write.table(top_words_ldavis,filename,sep="\t",quote=F,row.names=F,col.names=F)
}
```

---

# 4. Distance Analysis

## 4a. Hybrid Matrix
Hybrid words are based on distance. Due to their lower number, just count them up per topic/lambda. Makes a matrix, `matrix.hybwords.txt`, with columns 1. number topics, 2. lambda, 3. number of hybrid words.
```sh
# variables
OUT_PATH="/home/groups/ravnica/users/nishida/temp_features/matrices/1_distance"
VOCAB_PATH="features.vocab.hybrid.txt"

# create the header
echo -e "TOPIC\tLAMBDA\tCOUNT" > $OUT_PATH/matrix.hybwords.txt

# number of hybrid words matrix
array=(0.05 0.10 0.15 0.20 0.25 0.30 0.35 0.40 0.45 0.50 0.55 0.60 0.65 0.70 0.75 0.80 0.85 0.90 0.95 1.00)
for n_topic in $(seq 1 65)
do
	for lambda in "${array[@]}"
	do
		numhyb=$(cat topwords_${lambda}/topic_${n_topic}.txt | grep "ON$" | wc -l)
		echo -e "${n_topic}\t${lambda}\t$numhyb"
	done
done >> $OUT_PATH/matrix.hybwords.txt

# add the vocab total at the end
numhyb=$(cat $VOCAB_PATH | wc -l)
echo -e "VOCAB_TOTAL\tVOCAB_TOTAL\t$numhyb" >> $OUT_PATH/matrix.hybwords.txt
```

## 4b. Distance Distributions, Global TSS

Calculates a matrix with the distribution of distances per topic with every lambda. The global distances can be calculated on the full set of features and subset from that. Point TMP to wherever and clean up at your leisure. Output is a matrix for global, vocab, and each topic with distances and counts per lambda.
```sh
# output path
OUT_PATH="/home/groups/ravnica/users/nishida/temp_features/matrices/1_distance"
TMP="tmp/"

# calculate all distances
# reformat to bed, sort bed, find distances relative to TSS, save relevant columns
cat features.all.atac.txt  | tr '-' '\t' | sort-bed - | closest-features --closest --delim "\t" --dist - refs/ensembl.hg38.tss.chr.bed | cut -f 1,2,3,7,8,9 > features.all.atac.sorted_distances.txt

# calculate vocab distance by overlap to all features
cat features.vocab.atac.txt | tr '-' '\t' | sort-bed - | bedops -e features.all.atac.sorted_distances.txt - > features.vocab.atac.sorted_distances.txt

# summarize all feature distances into count distributions
INPUT="features.all.atac.sorted_distances.txt"
OUTPUT="matrix.distance_all.txt"
# header
echo -e "DISTANCE\tCOUNT_ALL" > $OUT_PATH/$OUTPUT
# grab distances, sort numerically, collapse to counts, reformat white space, swap columns, reformat to tabs
cat $INPUT | cut -f 6 | sort -n | uniq -c | sed -e 's/^[ ]*//g' | awk '{print $2,$1}' | tr ' ' '\t' >> $OUT_PATH/$OUTPUT

# summarize all vocab distances into count distributions
INPUT="features.vocab.atac.sorted_distances.txt"
OUTPUT="matrix.distance_vocab.txt"
# header
echo -e "DISTANCE\tCOUNT_VOCAB" > $OUT_PATH/$OUTPUT
# grab distances, sort numerically, collapse to counts, reformat white space, swap columns, reformat to tabs
cat $INPUT | cut -f 6 | sort -n | uniq -c | sed -e 's/^[ ]*//g' | awk '{print $2,$1}' | tr ' ' '\t' >> $OUT_PATH/$OUTPUT
# prep topic matrixes seeding with vocab since topics are a subset of vocab, save to TMP
cat $OUT_PATH/$OUTPUT | cut -f 1,2 | sed 1d > ${TMP}/tmp.vocabdist.txt

# get padded distance counts per topic and lambda, save to TMP
REF="features.vocab.atac.sorted_distances.txt"
array=(0.05 0.10 0.15 0.20 0.25 0.30 0.35 0.40 0.45 0.50 0.55 0.60 0.65 0.70 0.75 0.80 0.85 0.90 0.95 1.00)
for n_topic in $(seq 1 65)
do
	for lambda in "${array[@]}"
	do
		# get atac features, format to bed, sort, compare overlap, save distance, sort, count, clean whitespace, swap columns, reformat, pad with 0's via key, reformat, save relevant column
		cat topwords_${lambda}/topic_${n_topic}.txt | grep "^chr" | tr '-' '\t' | sort-bed - | bedops -e $REF - | cut -f 6 | sort -n | uniq -c | sed -e 's/^[ ]*//g' | awk '{print $2,$1}' | tr ' ' '\t' | awk 'NR == FNR { key[$1] = $2; next } { $2 = ($1 in key) ? key[$1] : 0 }; 1' - ${TMP}/tmp.vocabdist.txt | tr ' ' '\t' | cut -f 2 > ${TMP}/${n_topic}_${lambda}.txt
		
		# if the file is completely empty (no ATAC features) pad it entirely with 0's
		if [ ! -s ${TMP}/${n_topic}_${lambda}.txt ]; then
			cat ${TMP}/tmp.vocabdist.txt | awk '{print 0}' > ${TMP}/${n_topic}_${lambda}.txt
		fi
	done
done

# PLACEHOLDER - turn this into a better loop rather than explicity calling each lambda
for num in $(seq 1 65)
do
	# header
	echo -e "DISTANCE\tCOUNT_VOCAB\tCOUNT_L0.05\tCOUNT_L0.10\tCOUNT_L0.15\tCOUNT_L0.20\tCOUNT_L0.25\tCOUNT_L0.30\tCOUNT_L0.35\tCOUNT_L0.40\tCOUNT_L0.45\tCOUNT_L0.50\tCOUNT_L0.55\tCOUNT_L0.60\tCOUNT_L0.65\tCOUNT_L0.70\tCOUNT_L0.75\tCOUNT_L0.80\tCOUNT_L0.85\tCOUNT_L0.90\tCOUNT_L0.95\tCOUNT_L1.00" > $OUT_PATH/matrix.distance.topic_${num}.txt
	paste ${TMP}/tmp.vocabdist.txt ${TMP}/${num}_0.05.txt ${TMP}/${num}_0.10.txt ${TMP}/${num}_0.15.txt ${TMP}/${num}_0.20.txt ${TMP}/${num}_0.25.txt ${TMP}/${num}_0.30.txt ${TMP}/${num}_0.35.txt ${TMP}/${num}_0.40.txt ${TMP}/${num}_0.45.txt ${TMP}/${num}_0.50.txt ${TMP}/${num}_0.55.txt ${TMP}/${num}_0.60.txt ${TMP}/${num}_0.65.txt ${TMP}/${num}_0.70.txt ${TMP}/${num}_0.75.txt ${TMP}/${num}_0.80.txt ${TMP}/${num}_0.85.txt ${TMP}/${num}_0.90.txt ${TMP}/${num}_0.95.txt ${TMP}/${num}_1.00.txt >> $OUT_PATH/matrix.distance.topic_${num}.txt
done
```

## 4c. Distance Distributions, Within Topic TSS
Calculates a matrix with the distrubution of distances within the realm of a topic. Here I am using the original GTF from Cell Ranger to specifically match names. In the future, we should probably switch to accession ID's. This GTF is a combination of ENSEMBL and HAVANA, is a bit older, and lacks some discrete categories (like splitting 5' and 3' UTR annotations). The output is a bit wide for my liking but with max words of 2000 it isn't obscene. Columns are 1. Topic, 2. Lambda, 3. Number of ATAC features, 4. Number of RNA features, 5. Number of RNA features found, 6. Number of ATAC features with no match (not on the chromosome) and 7. comma separated distances.
```sh
OUT_PATH="/home/groups/ravnica/users/nishida/temp_features/matrices/1_distance"
REF="refs/genes.gtf.gz"
TMP="tmp/"

# pull out a reference of start codons and common gene names
zcat $REF | awk '$3 == "start_codon"' | cut -f 1,4,5 > ${TMP}/tmp.gtf.coords.bed
zcat $REF | awk '$3 == "start_codon"' | cut -f 9 | sed -e 's/.*gene_name \"//g' | sed -e 's/\".*//g' > ${TMP}/tmp.gtf.gnames.bed

# strangely some of the 'start_codons' here are not 3bp, pad these with by one base to the stop
paste ${TMP}/tmp.gtf.coords.bed ${TMP}/tmp.gtf.gnames.bed | awk '{if ($2 == $3) {printf ("%s\t%s\t%s\t%s\n",$1,$2,$3+1,$4)} else {print}}' | sort-bed - > ${TMP}/tmp.gtf_starts.sorted.bed

echo "TOPIC\tLAMBDA\tNUM_ATAC\tNUM_RNA\tNUM_RNA_FOUND\tNUM_ATAC_NA\tDISTANCE_STRING" > ${OUT_PATH}/matrix.distance.within.txt
array=(0.05 0.10 0.15 0.20 0.25 0.30 0.35 0.40 0.45 0.50 0.55 0.60 0.65 0.70 0.75 0.80 0.85 0.90 0.95 1.00)
for n_topic in $(seq 1 65)
do
	for lambda in "${array[@]}"
	do
		
		# per topic and per lambda, write a list of comma-separated sorted distances
		cat topwords_${lambda}/topic_${n_topic}.txt | grep -v "_ON$" | grep "^chr" > tmp/tmp.atac.txt
		cat topwords_${lambda}/topic_${n_topic}.txt | grep -v "_ON$" | grep -v "^chr" > tmp/tmp.rna.txt
		
		# subset rna bed
		while IFS= read -r line
		do
			cat tmp/tmp.gtf_starts.sorted.bed | awk -v name="$line" '$4 == name'
		done < tmp/tmp.rna.txt | sort-bed - > tmp/tmp.rna.gtf_starts.sorted.bed
		
		# calculate values
		num_atac=$(cat tmp/tmp.atac.txt | wc -l)
		num_rna=$(cat tmp/tmp.rna.txt | wc -l)
		num_rna_found=$(cat tmp/tmp.rna.gtf_starts.sorted.bed | cut -f 4 | sort | uniq | wc -l)
		num_atac_na=$(cat tmp/tmp.atac.txt | tr '-' '\t' | sort-bed - | closest-features --dist --closest --no-query --delim '\t' - tmp/tmp.rna.gtf_starts.sorted.bed | cut -f 5 | grep -c "NA")
		dist_str=$(cat tmp/tmp.atac.txt | tr '-' '\t' | sort-bed - | closest-features --dist --closest --no-query --delim '\t' - tmp/tmp.rna.gtf_starts.sorted.bed | cut -f 4 | grep -v "NA" | tr '\n' ',' | sed 's/.$//')
		
		echo -e "$n_topic\t$lambda\t$num_atac\t$num_rna\t$num_rna_found\t$num_atac_na\t$dist_str" >> ${OUT_PATH}/matrix.distance.within.txt
		
	done
done
```

---

# 5. Gene Info

## 5a. Prep Gene Info
Here I'm using a GTF which is more recent and has more annotations (UTR is broken into 5' and 3').
```sh
#GTF2 - hg38.ensGene.gtf
```

---

# 6. Correlations

## 6a. Pearson / Spearman

```sh

```

## 6b. CCAN's

```sh
# CCAN - MCF7.CCANs.txt
```

---

# 7. Enrichment - Subset

```sh
# FIMO - cat_hg38.bed
# hypergeo
cd /home/groups/oroaklab/adey_lab/projects/maga/00_DataFreeze_Cortex_and_Hippocampus/Regions/cortex/cortex.filt.temp40_topic.Rds.topics

# tally backgrounds
cat *.bed | sort-bed - > all.bed
cat all.bed | bedops -e 1 /home/groups/oroaklab/nishida/fimo_scans/cat_hg38.bed - > background.bed
cat background.bed | cut -f 5,6 | tr '\t' '_' | sort | uniq -c | sed -e 's/^[ ]*//g' | tr ' ' '\t' | awk '{print $2,$1}' | tr ' ' '\t' > background_counts.txt

# tally topics and compare to bg with hypergeo
array=(Topic_10 Topic_11 Topic_12 Topic_13 Topic_14 Topic_15 Topic_16 Topic_17 Topic_18 Topic_19 Topic_1 Topic_20 Topic_21 Topic_22 Topic_23 Topic_24 Topic_25 Topic_26 Topic_27 Topic_28 Topic_29 Topic_2 Topic_30 Topic_31 Topic_32 Topic_33 Topic_34 Topic_35 Topic_36 Topic_37 Topic_38 Topic_39 Topic_3 Topic_40 Topic_4 Topic_5 Topic_6 Topic_7 Topic_8 Topic_9)
for i in "${array[@]}"
do
cat $i.bed | sort-bed - | bedops -e 1 /home/groups/oroaklab/nishida/fimo_scans/cat_hg38.bed - > $i.motifs.bed
cat $i.motifs.bed | cut -f 5,6 | tr '\t' '_' | sort | uniq -c | sed -e 's/^[ ]*//g' | tr ' ' '\t' | awk '{print $2,$1}' | tr ' ' '\t' > $i.motifcounts.txt
Rscript /home/groups/oroaklab/nishida/scripts/sc/motifs_hypergeo.Rscript $i.motifcounts.txt background_counts.txt $i.hypergeo.txt
done
```

---

# 8. Enrichment - Similar Set

```sh
#DHSi - DHS_Index_and_Vocabulary_hg38_WM20190703.bed
#CISTROMES - /cistrome/etc
```

---

# 9. Enrichment - Superset

```sh
#SUPERENH - Homer_SE_MCF7_sg_NT_1.bed # SE computed by Aaron in MCF7 WT cells.
#ACTIVEENHMARK - H3K27Ac_intersect_all3.bed # This BED is the "active enhancers" mark in WT_E2 treated MCF7.
```

---

10. Feature Pathway Enrichment

```sh
# placeholder of full GO enrichment with size/shape, multiple topics, etc.
```
