# Identification of Regionally Variant Genes using Spatial Transcriptomics Data


I highly suggest glancing at the **Process** and **Dataframes** sections to get a feel for the project. The last section is an index of variable names and column titles for user reference. 

&nbsp;  

### Setup and Data

For this project, I use spatial transcriptomics data collected using [Slide-seq v2 technology](https://www.nature.com/articles/s41587-020-0739-1) from mouse cerebellum tissue by [Cable, et. al](https://www.nature.com/articles/s41587-021-00830-w). The researchers already performed cell type clustering, and I used the cell type assignments they derived from this process.  

I used [concordex](https://www.biorxiv.org/content/10.1101/2023.06.28.546949v2) to break the dataset into regions based on cell type composition. Then, the aims of the project were to find DE genes between regions (across cell types), as well as DE genes between region *within a single cell type*.  

Most of the data I was working with is contained in this repo, however, the matrix containing all of the count data is too large to store on github. The original data can be downloaded [here](https://singlecell.broadinstitute.org/single_cell/study/SCP948/robust-decomposition-of-cell-type-mixtures-in-spatial-transcriptomics#study-download) (the file is called "Cerebellum_MappedDGEForR.csv") but I used a repackaged numpy file containing the same data that loads much faster into python, which I have uploaded to Google Drive [here](https://drive.google.com/file/d/1-bPFZfneGkXVb2yClsgSD8wyGBkCfR5I/view?usp=sharing).  


&nbsp;  

### Process

The workflow of Jupyter Notebooks roughly follows as so:
1. **Filtering.ipynb**
   - Initial dataset exploration used to determine thresholds for filtering out low expression genes and faulty cells
2. **Gene-RegionComparisons.ipynb**
   - Perform statistical tests to compare gene expression within each cell type, pairwise between regions. Tabulate all results and select those with T-test p-values that pass the Bonferroni Correction
3. **GeneratingPlots.ipynb**
    - Generate and save to file a combined Stripbox and ECDF plot for every gene + cell type combo with at least one statistically significant pairwise comparison
4. **SpatialVisualization.ipynb**
    - Dedicated notebook for making spatial plots
4. **FollowupAnalysis.ipynb**
    - Notebook with follow-up code to investigate possible trends surfacing from plot analysis

&nbsp;  

### Dataframes:
I use multiple pandas dataframes in this project, so here are breif descriptions of each one. 
&nbsp;  
- **df**
    - Contains a row for every unique gene + cell type + region combination, along with information about the expression of the given gene in that collection of cells
- **comparisondf**
    - Contains a row for every statistical comparison made to test for differences in expression of a given gene within a cell type, between two regions. *Conceptually, comparisons are being made between rows of **df***. Each row includes T-test, Wilcoxon Rank Sum, and Komolgorov-Smirnov p-values. 
- **passes_bonferroni**
    - Identical to comparisondf, but only contains comparisons for which the T-test p-value passes the Bonferroni Correction.
- **marker_genes**
    - A list of genes highlighted by the researchers who collected the data as "cell type specific genes." In this context, these are genes with log-fold change of 3 or greater in a certain cell type compared to all other cell types. 
- **minidf**
    - Used in multiple instances when a temporary dataframe needs to be generated, often in the context of plotting

&nbsp;  

### Variables, column titles, etc.:

- cell_type: The cell type assignment of a certain cell. These were given by the researchers
- DE: 'Differential expression', 'Differentially expressed'
- gene_name: name of the gene
- GOI: 'Gene of interest.' Used as a variable to hold a single gene name
- log1p_total_counts: sum all mRNA counts within a cell after the log1p transformation has been applied
- ks_p: p-value result from a Komolgorov-Smirnov test
- n_cells: number of cells fitting a certain criteria
- n_cells_by_counts: number of unique cells with nonzero expression of a given gene
- n_counts: same as log1p_total_counts, but summed over multiple cells
- n_exprs: number of unique cells with nonzero expression of a given gene
- n_genes_by_counts: number of unique genes with nonzero expression in a given cell
- plotname_handle: Non-unique identifier for **df** rows in the form *gene*_*celltype*
- pct_counts_mt: NOTE, this is really the *fraction* of pre-normalization counts corresponding to mitochondrial genes for each cell
- pct_exprs: n_exprs/n_cells for a given gene. NOTE, like pct_counts_mt, this is the *fraction* of cells with nonzero expression of a certain gene, not the percentage, technically 
- region: Region assignment by [concordex](https://www.biorxiv.org/content/10.1101/2023.06.28.546949v2) for a specific cell. See qualitative correspondence between region numbers and cerebellar biological regions
    - 0: Granule layer
    - 1: Purkinje-Bergmann layer
    - 2: Oligodendrocyte (white matter) region
    - 3: Molecular layer
- region1, region2: The two regions being compared in for each row of **comparisondf**
- suff_data: Column of booleans in **df** determining whether comparisons should be made between that row and other rows (see **comparisondf** description). Defined as True if n_exprs > 10, else False
- total_counts: sum of all mRNA counts
- ttest_p: p-value result from a T-test
- wilcoxon_p: p-value result from a Wilcoxon Rank Sum 

