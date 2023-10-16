# lung.project:

#Welcome to Code_Club!

Overarching goal in a nutshell:
Finding molecular features affected by ageing in the lung

Examples of analyses that you could perform:
1.	Which cell types have changes in their transcriptomes in old lungs?
a.	How large are these changes?
b.	What genes 
2.	Do proportions of cell types change during ageing?
a.	Follow up: Do the abundances change with or without co-occurring changes in the gene expression patterns?
Bonus questions:
3.	When integrating epigenetic data (ATAC-seq), are the changes in chromatin accessibility corroborating the results from question 1 & 2?
a.	Which type of features are among the most changed (peaks proximal/distal to gene bodies, located in genomic regions of distinct properties?)
4.	Is there a change in alternative splicing in cell types of aged lungs?
a.	Is it even 


•	Data sources:

o	 RNA: Columbia University/NYP COVID-19 Lung Atlas: 7 control samples ~30k cells
https://singlecell.broadinstitute.org/single_cell/study/SCP1219/columbia-university-nyp-covid-19-lung-atlas#/

o	 LungMAP Human Lung Reference Cell Atlas: 104 lung samples ~350k cells (!)
https://data-browser.lungmap.net/explore/projects/6135382f-487d-4adb-9cf8-4d6634125b68 

Tabula Sapiens: 3 donors ~35k cells
https://tabula-sapiens-cellxgene.ds.czbiohub.org/lung/
https://figshare.com/articles/dataset/Tabula_Sapiens_release_1_0/14267219

o	ATAC:
Lung ATAC-seq was performed in this publication, I didn’t find the exact download link yet:
https://elifesciences.org/articles/62522

 
Conceptual workflow:
•	Unify Metadata:
  o	Make sure only healthy samples are used
  o	Suggestion for main factors to keep and unify: 

Single cell protocol, sex, ethnicity, age
  •	Obtain count matrices
  •	SCRAN normalization: https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0947-7

run using pre-clustering to uphold the assumptions of the algorithm
  •	Integrate Data
    o	What are reasons why we integrate? What factors to perform integration on?
    o	There are many integration algorithms; I suggest that you familiarize yourself with       the linked Nature Biotech paper and the differences in algorithms, eg. in terms of what     you get out from them (eg. only low dimensional data or corrected count tables?)

You can start with Seurats RPCA integration method for this project or explore others.
    •	Label cells using known cell type labels. We can utilize the already nicely annotated     Tabula Sapiens dataset to annotate all cells in our integrated dataset! Typically, this     is an application for machine learning methods.
scANVI (“single-cell ANnotation using Variational Inference”)
Transfer learning using scArches https://www.nature.com/articles/s41587-021-01001-7
    
    •	Cluster cells
    Leiden or Louvain clustering are typically used on a nearest neighbor graph of the   low dimensional space (eg. PCA or integrated low dimensional space). Think about coarse and fine groupings, how can we come up with them?

Questions that you should keep in mind:
    •	Remember to always take a look at the output of every step – does the output make         sense?
    •	How do we evaluate the performance / sensibility of analysis steps?
Example: Shannon entropy calculation on the cell type label distribution per cluster     to validate whether our cell type labels correspond to data driven grouping of cells we     produced.


Details and links
•	SCRAN example implementation: https://github.com/bvieth/powsimR/blob/d9e49ace330214513761e4be37396e4afed96e86/R/utils_normalise.R#L434
•	Data integration algorithms and performance metrics: https://www.nature.com/articles/s41592-021-01336-8
•	Seurat RPCA tutorial:
https://satijalab.org/seurat/articles/integration_rpca
•	Interesting concept of “geometric sketching” to remove the factor of the size (ie number of cells) of the datasets being used in data integration https://cb.csail.mit.edu/cb/geosketch/
