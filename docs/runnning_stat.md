
# Running your statistical analysis step by step

At this stage, you need to select the global parameters for the statistical analysis.

```yaml
LEVEL: CpG # "CpG" or "Tiles" study by Tiles or by CpG 
TILESIZE: 250 
STEPSIZE: 1  # Tiles relative step size
NB_CPG_TILES: 1 # minimal number of CpG to keep a tile in the analysis
```
If **Tiles** was selected, define tile size, step size, and minimal number of CpG by tile. 
**STEPSIZE** corresponds to the relative size of the gap between two tiles. If it is set to 1, the tiles are non-overlapping, if it is set to 0.5, the tiles overlap by half.

## Exploratory analysis 

```yaml
# ===== Exploratory analysis ===== #
## params 
MINCOV: 5  # int, minimum coverage depth for the analysis
COV.PERC: 99.9 # to the coverage filter, choose the percentile for remove top ..% 
MINQUALI: 20  # int, minimum quality to keep a CPG for the analysis
DESTRAND: yes # combine information from F and R strands reads
UNITE: all # 'all' or 'one' (at least one per group)
```
**COV.PERC** corresponds to the percentage of the most covered CpGs or tiles to retain. For example, if COV.PERC = 99.9, only the top 1% most covered are removed. This parameter helps eliminate biases of mapping, for example in repetitive regions. Please note, in the case of RRBS, set this argument to 100 to avoid removing intentionally enriched regions.

**DESTRAND**  if TRUE, reads covering both strands of a CpG dinucleotide will be merged, do not set to TRUE if not only interested in CpGs (default: FALSE). If the methylRawList object contains regions rather than bases setting destrand to TRUE will have no effect. Setting DESTRAND to "true" can be useful for "artificially" increasing the coverage of your data in the case of low-covered samples. Indeed, with this parameter, the coverage per site is doubled.

**UNITE** allows you to choose how you want to unify your replicates. Minimum number of samples per replicate needed to cover a region/base. By default only regions/bases that are covered in all samples are united as methylBase object, however by supplying an integer for this argument users can control how many samples needed to cover region/base to be united as methylBase object. For example, if min.per.group set to 2 and there are 3 replicates per condition, the bases/regions that are covered in at least 2 replicates will be united and missing data for uncovered bases/regions will appear as NAs. 

## Differential methylation analysis

Finally you have to set the parameters for the differential methylation analysis. You have to define the comparisons you want to do (pairs of conditions). 

```yaml
COMPARISON : [["WT","1KO"], ["WT","DKO"], ["1KO","DKO"]] 
```
And set up your analysis 

```yaml
# ===== Differential analysis ===== #

DMR: yes # if yes, run DMR

## params (CPG or TILES)
LIST_SIGNIDIF: [10, 20, 25, 30] # SigDiffMeth en %, used in MKit_diff_bed.R
LIST_QVALUE : [0.001, 0.01, 0.05]  # used in MKit_diff_bed.R
QVALUE: 0.05  # QValue (used in Mkit_differential.Rmd)
SIGNIDIF: 10

## params (only DMR)
DMR_TYPE: regions  # regions or blocks, by default regions 
MIN_CPG: 5  # minimum number of CpGs to consider for a candidate DMR, by default 5, minimum 3
MAX_GAP: 1000 # maximum number of bp in between neighboring CpGs to be included in the same DMR, by default 1000
CUTOFF: 0.1 # cutoff of the single CpG methylation difference that is used to discover candidate DMR. by default 0.1
FDR: 0.05 # QVALUE for select significant DMR
```
**LIST_SIGNIDIF** and **LIST_QVALUE** are used for generating bedgraphs. For each threshold of differences and for each threshold of q-value, a bedgraph is generated.

If **DMR** is turn to "YES", you perform a DMR analysis with the same comparison pairs as in CpG or Tiles analysis. Please note that the package used to infer the DMRs was designed for WGBS analysis. If you want to perform an analysis in RRBS, you can try removing the smoothing or modifying certain parameters: ??

The default parameters are optimized to focus on local DMRs (**regions**), typically in the range of hundreds to thousands of base pairs. If you choose **blocks**, the range increases to hundreds of thousands to millions of base pairs. In this case, it's advisable to decrease the cutoff.

!!! warning 
    The DMR analysis relies on the DMC analysis. It is not possible to perform the DMR analysis without first running the workflow with the "CpG" LEVEL. In the case where you choose to perform an analysis in "Tiles" and in DMR, this will not generate an error message, but only the DMT analysis will be performed.. 

### BedGraphe 

In addition to the HTML report, for each comparison, .bed files are generated. For each methylation difference threshold in LIST_SIGNIDIF and for each QVALUE in LIST_QVALUE, a bedgraph is generated in the folder: . 

!!! warning
    In the case where overlapping tiles are chosen, the conversion of bedgraphs to Bigwig is delicate. To enable the conversion, we have used the following method:
    
``` 
module load bedopts 
bedops --chop 500 PBS_EtOH_5mC_MethDiffperc.bedGraph > test.bed
bedtools intersect -a PBS_EtOH_5mC_MethDiffperc.bedGraph -b test.bed > result.bed
bedtools sort -i result.bed > result_sort.bed
bedtools merge -i result_sort.bed -d -1 -c 4 -o mean > final.bed
```

bedops --chop 500 merges all overlapping tiles and then splits the region into fragments of identical sizes (500 bp).    
bedtools intersect is used to intersect all tiles with the 500 bp fragments created by bedops --chop 500.    
Finally, bedtools merge -c 4 -o mean merges the intersecting tiles with the 500 bp fragments and calculates the average methylation percentage. The -d -1 prevents adjacent fragments from being merged, meaning that there must be at least a 1 bp overlap for them to be merged.


## ORA : Over-representation analysis

After the differential analysis, files containing lists of genes for each comparison are generated. If certain genes have been identified as associated with differentially methylated regions (DMTs/DMRs), it is possible to use these gene lists to perform an Over-representation analysis.

ORA, or Over-Representation Analysis, is a statistical method used to determine if a gene in your data is over-represented compared to a predefined dataset (such as GO, KEGG, etc.). Methylator utilizes the Python tool [GSEApy](https://gseapy.readthedocs.io/en/latest/index.html)  to conduct this analysis.

To do this, you need to provide an Ensembl dataset (depending on the organism under study) and a gene set library (depending on the analyses you wish to perform). All available gene set libraries are accessible on the [Enrichr](https://maayanlab.cloud/Enrichr/#libraries) website.

``` yaml 
# ORA : Over-representation analysis
ORA: yes # if yes, perform a Over-representation analysis with genes who overlap with DMRs or tiles 
GENE_SET: DisGeNET # gene set library (Enrichr)
ENSEMBL_DATASET: hsapiens_gene_ensembl # dataset fo convert ensembl gene ID
```

## Start the workflow

When the configuration files are fully adapted to your experimental set up and needs, you can **start the workflow** by running:

```
[username@clust-slurm-client Methylator]$ sbatch Workflow.sh wgbs
```
