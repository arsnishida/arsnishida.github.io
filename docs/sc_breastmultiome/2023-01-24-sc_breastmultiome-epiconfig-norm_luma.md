---
layout: post
title:  "23-01-24, Normal and LumA Subsets"
date:   2023-01-24
parent: sc_breastmultiome
nav_order: 2
---

Dir: `/home/groups/CEDAR/nishida/SC_BREASTMULTIOME`

For further probing of topics related to the classifications of the multiomic breast cancer samples, the full set of RNA features was used along with redoing the analysis with the subset of Normal and LumA cells only. Both of these classifications should be the closest to each other relative to the other classifications and we would ideally hope to capture the differences and transitions between these two states. From the subsetting, samples 7 and 12 look like the only samples split into separate discrete clusters and sample 16 is interesting with its close proximity to the other Normal cells and because the alternative classifier suggests it is also of the Normal subtype.

List of GENEFU and SSPBC Classifications per Sample:
```
NUMCELLS	EPICELLS	GENEFU	SSPBC	SAMPLE
1030	135	Basal	Her2	RM_2
2392	643	Basal	Her2	sample_1
5961	5040	Basal	Her2	sample_3
10866	8595	Her2	LumB	sample_4
719	601	LumA	LumA	RM_1
208	164	LumA	LumA	RM_3
2139	1438	LumA	LumA	sample_12
162	78	LumA	LumA	sample_6
208	85	LumA	LumA	sample_8
454	338	LumA	LumA	sample_9
230	142	LumA	Normal	sample_16
13081	12764	LumB	LumB	sample_10
3218	3051	LumB	LumB	sample_11
356	158	LumB	LumB	sample_20
575	255	LumB	LumB	sample_5
111	104	NA	NA	RM_4
984	969	NA	NA	sample_15
944	530	NA	NA	sample_19
1391	918	Normal	LumA	sample_7
```

The following folders have figures generated showing their UMAPs and heatmaps:
<br>[Filtered Epithelial Cells - Concatenate, Full RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/ErJi4kT0b-hJpIBkGLwiqD8BEtgNzHSSCnwFJ8bpLSdWGQ?e=ZBnKgM)
<br>[Filtered Normal and LumA Only - Concatenate, 2000 RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/ErtLGma-IyhJkkJQmbR7oiAB69hp7dFuwssTGEsb2sh5Zw?e=78M7HU)
<br>[Filtered Normal and LumA Only - Concatenate, Full RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Ehq5vPkfII5Gn1ok69Q7SxwBy8g2MHXTfmHBFqHg336gJA?e=0pZAwg)

The Normal and LumA subset using all RNA features seems most useful for further analysis. Its relevant model files are here:
<br>[MODEL of Filtered Normal and LumA Only - Concatenate, Full RNA Features](https://ohsuitg-my.sharepoint.com/:f:/g/personal/nishidaa_ohsu_edu/Epq4aETLH39Nlyq1i8AhzO0ByzPg86LsYOeIsZOlkSwL9g?e=c7HEv1)

Link to Relevant Presentations:
<br>[230124.epiconfig_sc_bcmultiome.luma_normal.pdf](https://ohsuitg-my.sharepoint.com/:b:/g/personal/nishidaa_ohsu_edu/ERqB_PY6thdAvdDJ9Dfurb4BNC928WHhVtUhCeeBEC_3ew)
