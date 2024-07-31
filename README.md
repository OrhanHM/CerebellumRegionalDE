# Identification of Regionally Variant Genes using Spatial Transcriptomics Data

### Motivations

Working on filling this in :-)
&nbsp;  
&nbsp;  

### Setup

For this project, I use spatial transcriptomics data collected using [Slide-seq v2 technology](https://www.nature.com/articles/s41587-020-0739-1) from mouse cerebellum tissue by [Cable, et. al](https://www.nature.com/articles/s41587-021-00830-w). The researchers already performed cell type clustering, and I used the cell type assignments they derived from this process.  

I used [concordex](https://www.biorxiv.org/content/10.1101/2023.06.28.546949v2) to break the dataset into regions based on cell type composition. Then, the aims of the project were to find DE genes between regions (across cell types), as well as DE genes between region *within a single cell type*.  

See the rest of this README 

&nbsp;  

### Process

The workflow of Jupyter Notebooks roughly follows as so:
1. **Filtering.ipynb**
   - Initial dataset exploration used to determine thresholds for filtering out low expression genes and faulty cells
2. **Gene-RegionComparisons.ipynb**
   - Perform statistical tests to compare gene expression within each cell type, pairwise between regions. Tabulate all results and select those with T-test p-values that pass the Bonferroni Correction
3. **GeneratingPlots.ipynb**
    - Generate and save to file a combined Stripbox+ECDF plot for every gene + cell type combo with at least one statistically significant pairwise comparison
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
- **passes_bonferroni**
    - Identical to comparisondf, but only contains comparisons for which the T-test p-value passes the Bonferroni Correction
- **marker_genes**

- **minidf**
    - Used in multiple instances when a temporary dataframe needs to be generated (often in the context of plotting)

&nbsp;  

### Variables, filenames, column titles, etc.:

- cell_type: The cell type assignment of a certain cell. These were given by the researchers
- celltype_marker_genes.csv: List of genes highlighted by [Cable, et. al](https://www.nature.com/articles/s41587-021-00830-w) to be DE for a specific cell type
- DE: 'Differential expression', 'Differentially expressed'
- gene_name: name of the gene
- GOI: 'Gene of interest.' Used as a variable to hold a single gene name
- n_cells: number of cells fitting a certain criteria
- n_exprs: number of unique cells with nonzero expression of a given gene
- n_genes_by_counts: number of unique genes with nonzero expression in a given cell
- plotname_handle: Non-unique identifier for **df** rows in the form *gene*_*celltype*
- pct_counts_mt: NOTE, this is really the *fraction* of pre-normalization counts corresponding to mitochondria genes for each cell
- region: Region assignment by [concordex](https://www.biorxiv.org/content/10.1101/2023.06.28.546949v2) for a specific cell. See qualitative correspondence between region numbers and cerebellar biological regions
    - 0: Granule layer
    - 1: Purkinje-Bergmann layer
    - 2: Oligodendrocyte (white matter) region
    - 3: Molecular layer
- 



