# Internship_MPD_Bachelors
This is code I made in my Bachelors internship (April-July 2022).
This internship was entitled "Identification of cell fate conversion upon immortalization of epithelial cells using RNA-seq analysis".
It was performed in the group of Jo Zhou, department of Molecular Developmental Biology at the Radboud Institute of Molecular Life Sciences in Nijmegen.

## Files
The directory consists of the following R scripts:

_coupling_gene_names.Rmd_ : a simple R script for coupling a table of counts + ENSEMBL identifiers to their gene names.

_deseq2_wt_vs_TERT_KC_v1.3.Rmd_ : an R script that does the same as the script mentioned above, but now taking keratinocyte primary and immortalised samples as input.

_deseq2_wt_vs_TERT_LSC_v1.5B.Rmd_ : an R script that takes count tables for primary and immortalised limbal stem cells as input and performs differential gene analysis, as well as KEGG, GO-term and oncogene analysis among others.

_deseq2_wt_vs_TERT_inhouse_v1.1.Rmd_ : an R script that does the same analysis as above, but instead compares gene expression between primary limbal stem cells and keratinocytes.

_deseq2_wt_vs_TERT_overall_v1.2.Rmd_ : an R script that again does the same things as mentioned above, but it takes primary and immortalised samples of both limbal stem cells and keratinocytes and does an overall primary vs TERT differential gene analysis.

_mart_export.txt_ : a list of gene names and their ENSEMBL identifiers, which can be used to couple count tables with ENSEMBL identifiers to their corresponding gene names.

_r_env_4_1.yml_ : a yml file to create the conda environment I used to run all analyses except the splicing analysis.

_r_env_4_1_splicing.yml_ : a yml file to create the conda environment I used to run the splicing analysis.

_seurat_scRNA_analysis_v1.0.Rmd_ : an R script that accepts a Seurat object of single-cell RNA sequencing data and perform statistical analyses, as well as normalisation, visualisation, principal component analysis and conversion to pseudobulk (UMAP-clsutering based and random assignment).

_splicing_analysis.v1.2.Rmd_ : an R script that can run differential splice analysis on a single gene (using SGSeq) or on the entire genome (using DEXSeq). This script requires indexed BAM files (2 replicates for each conditions) that have been aligned using a splice-sensitive programme (in this project, we used STAR).

## Setup
Running the scripts mentioned in the "Files" section may take large amounts of RAM, especially splicing analysis since BAM files can become very large. Therefore, working on an external server is advised. For this project, I used the MobaXterm application to work on a remote server. To run R studio on an external server, you will need to set up a conda environment.

### Installing conda

First, log on to the server you will be using. Then, navigate to the directory you want to install conda in using the `cd` command, followed by the absolute path of the directory you want to work in. You can type `pwd` to check that you are in the correct directory.

Once you are in the correct working directory, run the following commands:
```{console}
wget repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O
~/conda_install.sh;
```
This should install conda, but if you cannot download the file needed to install conda, you can also click the link, download it to your computer, and then upload it to your working directory. Then, you only have to run the second line of code.

You can now start conda by typing
```{console}
source ~/.bashrc
```
Make sure to run this piece of code every time you start up the server again. If you see (base) to the left of your working directory, then conda is correctly activated.

### Making a conda environment

You will now have to make a conda environment, which includes the packages you will need for your analysis (such as R, or Python).
A conda environment can either be made from scratch, which means you will have to install each package separately, or you can make a conda environment using a .yml file, which tells conda which packages need to be installed. An example of a .yml file can be found under "Files".

A conda environment can easily be made from scratch via the following command:
```{console}
conda create --name <NAME OF ENVIRONMENT>
```
When naming your environment, try to avoid using spaces as this might make things hard later on. Use underscores instead, so "example_environment" instead of "example environment".

If you want to make an environment based on a .yml file, you can use the following command:
```{console}
conda env create -f environment.yml
```
The environment will then have the same name as the first line of the .yml file. For example, the following command:
```{console}
conda env create -f r_env_4_1.yml
```
will create a conda environment called r_env_4_1, and will contain all packages specified in the .yml file.

You can activate your environment using the following command:
```{console}
conda activate <NAME OF ENVIRONMENT>
```
and can be deactivated by
```{console}
conda deactivate
```
You can now activate Rstudio by typing
```{console}
rstudio
```
when your environment is active.

## Workflow

### scRNA pre-processing (KC TERT)
To pre-process scRNA data, you should create a Seurat object out of the count tables of all cells. A tutorial on Seurat can be found [here](https://satijalab.org/seurat/articles/pbmc3k_tutorial.html). Then, you can use the _seurat_scRNA_analysis_v1.0.Rmd_ file present in this repository to perform quality controls on these cells, filter them, and assign them to one of the pseudobulk replicates (based on UMAP clustering or random assignment). This will generate pseudobulk count tables that can be directly used for the analyses below. In the end, I did not use my scRNA data due to poor quality, but I could still generate two pseudobulk replicates from this script.

### Coupling counts tables to gene names
As an input, you should have count tables of genes in all conditions. The genes are given in ENSEMBL identifiers. You should also have _mart_export.txt_  downloaded to your working directory.
To couple the count table to gene names, use the script coupling_gene_identifiers.Rmd. Do this for the count tables of both conditions (LSCs and KCs), unless your count table already includes gene names.

### Analysing primary vs TERT limbal stem cells (LSCs)
Once you have coupled the count table to the gene names, you can use the count tables of primary and TERT immortalised LSCs to compare these conditions to each other. You can do this using the _deseq2_wt_vs_TERT_LSC_v1.5B.Rmd_ file. Make sure to save the differential genes (the output of genes_2 that will export to your results directory) as you will need this for the overall analysis.

### Analysing primary vs TERT keratinocytes (KCs)
You can use the _deseq2_wt_vs_TERT_KC_v1.3.Rmd_ file to perform the same primary vs TERT analysis as done for the LSCs, but now for keratinocytes. The input is the count tables for the primary and TERT immortalized keratinocytes. Again, make sure to save the differential genes to your results directory.

### Analysing primary vs TERT of all cell types
To perform an overall analysis of primary vs TERT conditions, you can use the _deseq2_wt_vs_TERT_overall_v1.2.Rmd_ file. As an input, you provide all count tables you have (primary LSC, primary KC, TERT LSC, and TERT KC).

### Analysing primary inhouse samples (LSC vs KC)
If you perform the analyses above and look at the result of PCA analyses and sample clustering, you might find that the primary inhouse samples (PKC_1/2 and LSC_wt_1/2) are very alike. If you want, you can rule out any sample mixups using the _deseq2_wt_vs_TERT_inhouse_v1.1.Rmd_ script, which compares the primary inhouse samples based on their cell type. 

### Splicing analysis
 Instead of using the environment specified by _r_env_4_1.yml_, make a new environment using the _r_env_4_1_splicing.yml_ file. As an input, you need to provide BAM files of two primary LSC replicates and two TERT LSC replicates. These BAM files need to be aligned using a splice-sensitive programme (STAR, TopHat) and they need to be indexed. Then, you can provide these as input to _splicing_analysis.v1.1.Rmd_. The result will be saved to your results directory and can be used for further analysis in the _deseq2_wt_vs_TERT_LSC_v1.5B.Rmd_ or _deseq2_wt_vs_TERT_overall_v1.2.Rmd_ scripts.

#
I hope this code assists you in performing your differential gene/splice analysis.
If any problems with the code occur, contact me at sidney.vanderzande@ru.nl.
