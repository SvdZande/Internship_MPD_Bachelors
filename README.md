# Internship_MPD_Bachelors
This is code I made in my Bachelors internship (April-July 2022).
This internship was entitled "Identification of cell fate conversion upon immortalization of epithelial cells using RNA-seq analysis".
It was performed in the group of Jo Zhou, department of Molecular Developmental Biology at the Radboud Institute of Molecular Life Sciences in Nijmegen.

The directory consists of the following R scripts:
deseq2_wt_vs_TERT_LSC_v1.4.Rmd : an R script that takes count tables for primary and immortalised limbal stem cells as input and performs differential gene analysis, as well as KEGG, GO-term and oncogene analysis among others.

deseq2_wt_vs_TERT_KC_v1.0.Rmd : an R script that does the same as the script mentioned above, but now taking keratinocyte primary and immortalised samples as input.

deseq2_wt_vs_TERT_overall_v1.0.Rmd : an R script that again does the same things as mentioned above, but it takes primary and immortalised samples of both limbal stem cells and keratinocytes and does an overall primary vs TERT differential gene analysis.

deseq2_wt_vs_TERT_inhouse_v1.0.Rmd : an R cript that does the same analysis as above, but instead compares gene expression between primary limbal stem cells and keratinocytes.

seurat_scRNA_analysis_v1.0.Rmd : an R script that accept a Seurat object of single-cell RNA sequencing data and perform statistical analyses, as well as normalisation, visualisation, principal component analysis and conversion to pseudobulk (UMAP-clsutering based and random assignment).

splicing_analysis.v1.0.Rmd : an R script that can run differential splice analysis on a single gene (using SGSeq) or on the entire genome (using DEXSeq). This script requires indexed BAM files (2 replicates for each conditions) that have been aligned using a splice-sensitive programme (in this project, we used STAR).

coupling_gene_names.Rmd : a simple R script for coupling a table of counts + ENSEMBL identifiers to their gene names.

r_env_4_1.yaml : a yaml file to create the conda environment I used to run all analyses except the splicing analysis.

r_env_4_1_splicing.yaml : a yaml file to create the conda environment I used to run the splicing analysis.

If any problems with the code occur, contact me at sidney.vanderzande@ru.nl.
