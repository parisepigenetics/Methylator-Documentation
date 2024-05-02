
## Configuration file for Bisuflite Sequencing (WGBS or RRBS) data

```yaml
# ============================================================================= #
# =========== Methylator Workflow configuration file (BS-seq data) ============= #
# ============================================================================= #

# Please check the parameters, and adjust them according to your circumstance

# Project name
PROJECT: AGE_LIVER

## paths for intermediate final results
BIGDATAPATH: Big_Data # for big files
RESULTPATH: Results


# ===================== Configuration for BS-seq data =================== #
# ===================================================================== #
DATATYPE: "WGBS" # "WGBS" (Whole genome) or "RRBS" (Reduced representation) 
METAFILE: configs/metadata_age_liver.tsv  # the meta file describing the experiment settings
READSPATH: fastq_age_liver/ # the path to fastq files
COMPARISON : [["2M","8M"], ["8M","16M"], ["2M","16M"]] 
# the comparison(s) you want to do. If multiple comparisons, specify each pair (CONTROL & TREAT) in order respectively
# for testing purpose we put here the relative paths, but we recommend to always use full paths such as /shared/projects/YourProjectName/Raw_Fastq


# ========================= Control of the workflow ========================= #
# =========================================================================== #

## Do you want to download FASTQ files from public from Sequence Read Archive (SRA) ? 
SRA: no # "yes" or "no". 
# If set to "yes", the workflow will stop after the QC to let you decide whether you want to trim your raw data or not. 
# In order to run the rest of the workflow, you have to set it to "no".

## Do you need to do quality control ?
QC: no # "yes" or "no". 
# If set to "yes", the workflow will stop after the QC to let you decide whether you want to trim your raw data or not. 
# In order to run the rest of the workflow, you have to set it to "no".

## Do you need to do trimming ?
TRIMMED: no # "yes" or "no"

## Do you need to do mapping ?
MAPPING: no # "yes" or "no"

## Do you want to do the globale exploration ?
EXPLORATION: yes # "yes" or "no"

## Do you need to do differentially methylation analysis ?
DIFFERENTIAL: yes # "yes" or "no"

## Do you need to use MetyLasso tool ? 
METHYLASSO: no

## Do you need a final report ?
REPORT: yes # "yes" or "no"


# ================== Shared parameters for some or all of the sub-workflows ================== #
# ============================================================================================ # 

## is the sequencing paired-end or single-end?
END: pair  # "pair" or "single"


# ================== Configuration for Quality Control ================== # 
## All required params have already been defined in the public params


# ================== Configuration for trimming ================== # 

## Quality of the trimming
## Trim low-quality ends from reads in addition to adapter removal.
QUALITY: 28 #  Default : 20.

## Number of trimmed bases
## put "no" for TRIM3 and TRIM5 if you don't want to trim a fixed number of bases. 
TRIM5: 10 #  integer or "no", remove N bp from the 5' end of reads. This may be useful if the qualities were very poor, or if there is some sort of unwanted bias at the 5' end.
TRIM3: 9 # integer or "no", remove N bp from the 3' end of reads AFTER adapter/quality trimming has been performed.


# ================== Configuration for alignment to genome ================== # 

## aligner
ALIGNER: BOWTIE2 # "BOWTIE2"

## genome files
GENOMEPATH: my_bank/mm10.fa #path to the reference genome's folder 

## genome
ASSEMBLY: mm10 # short name of the assembly used for the analysis

## Library type
LIBRARY_TYPE_OPT: non_directional   # "directional", "non_directional" or "pbat" sequencing library 

## params in Bismark alignment (--score_min {function},{val1},{val2}). 
# More information about Bismark options: https://felixkrueger.github.io/Bismark/options/alignment/

TYPEFUNC: L # 'L' or 'G' for linear or logarithmic fonction
VARFUNCT1: 0 # first value for the function 0 is default
VARFUNCT2: -0.6 # second value for the function -0.2 is default
MISMATCH: 0 # nomber of mismatch wanted
ADVANCE_OPT:    # for advanced bismark user (mapping only), IMPLEMENTED IN THE CODE BUT NOT TESTED


# =================== Configuration for Statistical analysis ================= #
# ============================================================================ #

LEVEL: Tiles # "CpG" or "Tiles", study by CpG or by Tiles

# if Tiles was selected, define tile size, step size, and minimal number of CpG /tile
TILESIZE: 300
STEPSIZE: 1  # Tiles relative step size
NB_CPG_TILES: 3 # minimal number of CpG to keep a tile in the analysis

# if CpG was selected, your can choose to merge the reads from both strands
DESTRAND: yes # if yes, reads on both strands of a CpG dinucleotide will be merged. This provides better coverage, but only advised when looking at CpG methylation (for CpH methylation this will cause wrong results in subsequent analyses). This have no effect working on Tiles. 


# ===== Exploratory analysis ===== #
## params 
MINCOV: 5  # int, minimum coverage depth for the analysis
COV.PERC: 99.9 # to the coverage filter, choose the percentile for remove top ..% (MKit_diff_bed.R and MKit_Exploration.Rmd)
MINQUALI: 20  # int, minimum quality to keep a CPG for the analysis
UNITE: all # 'all' or 'one' (at least one per group)
MIN_PER_GROUP: 2 

# ===== Differential analysis ===== #

# designed for WGBS analysis 
# To use dmrseq, you need to have at least 2 samples in each condition
DMR: no # if yes, run DMR
# warning, it's possible to run DMR only if we choose CpG as LEVEL

## params (CPG or TILES)
LIST_SIGNIDIF: [30] # SigDiffMeth en %, used in MKit_diff_bed.R
LIST_QVALUE : [0.001]  # used in MKit_diff_bed.R
QVALUE: 0.05  # QValue (used in Mkit_differential.Rmd)
SIGNIDIF: 20 #corriger pour MINDIFF 
COVARIATE: yes # yes or no, do you want add covariables, if yes, please add a column for each covariable in the metadata 

## params (only DMR)
DMR_TYPE: regions  # regions or blocks, by default regions 
MIN_CPG: 3  # minimum number of CpGs to consider for a candidate DMR, by default 5, minimum 3
MAX_GAP: 1000 # maximum number of bp in between neighboring CpGs to be included in the same DMR, by default 1000 (for blocks : 5e3) 
CUTOFF: 0.1 # cutoff of the single CpG coefficient (methylation difference) that is used to discover candidate DMR. by default 0.1
FDR: 0.05 # QVALUE for select significant DMR

# ORA : Over-representation analysis
ORA: no # if yes, perform a Over-representation analysis with genes who overlap with DMRs or tiles 
GENE_SET: DisGeNET # gene set library (Enrichr)
ENSEMBL_DATASET: hsapiens_gene_ensembl # dataset fo convert ensembl gene ID


# ==================== Configuration for annotations ==================== #
# ======================================================================= # 

# ==== Standard Annotations ==== # 
## GTF 
GTFPATH: my_bank/gencode.vM25.annotation.gtf

## CpG island Bed 
BEDPATH: my_bank/cpgIslandExt.mm10.txt

# ===== Customs Annotations ===== #
CUSTOM_ANNOT: no 
METAFILE_ANNOT: configs/metadata_annot.tsv
CUSTOM_ANNOT_PATH: my_bank/
MERGE_WITH_BASICS_ANNOT: yes # yes or no
```

## Configuration file for Nanopore Methylation Sequencing data (MS) (MS or RRMS) 

```  yaml
# ============================================================================= #
# ========= Methylator Workflow configuration file (Nanopore data) ============ #
# ============================================================================= #

# Please check the parameters, and adjust them according to your circumstance

# Project name
PROJECT: Test_nanopore

## paths for intermediate final results
BIGDATAPATH: Big_Data # for big files
RESULTPATH: Results

## genome files
GENOMEPATH: TestDataset/my_bank/mm39_chr19_mini.fa  # path to the reference genome's folder 

## genome
ASSEMBLY: mm39 # mm10 name of the assembly used for the analysis (now use mm39, new version)


# ========================= Control of the workflow ========================= #
# =========================================================================== #

## Do you want to do the globale exploration ?
EXPLORATION: yes # "yes" or "no"

## Do you need to do differentially methylation analysis ?
DIFFERENTIAL: yes # "yes" or "no"

## Do you need a final report ?
REPORT: no # "yes" or "no"

# ===================== Configuration for Nanopore data ==================== #
# ========================================================================== #
DATATYPE: NANOPORE # do not touch!
METAFILE: TestDataset/configs/metadata_nano2.tsv
NANO_BAM_PATH: TestDataset/bam_nanopore
COMPARISON: [["WT","NP95"]]    # [["WT","TKO"], ["WT","WT2"], ["WT2","TKO"]]
# Did you select specific regions during sequencing? 
RRMS: yes 
# If yes, put the path to the BED file used for selection below
BED_RRMS: TestDataset/my_bank/rrms_mm39_mini.bed
# if you have basecalled FAST5 files including 5hmC detection, you can explore this mark
5HMC: yes


# # ===================== Configuration for process BAM files  ================== #
# =========================================================================== #

STARTFROMBED: no # put no if you start from BAM files, if you already have BED files from modbam2bed you can put the path to the folder containing them. The BED should be named Sample_Merged.cpg.bed. 


# =================== Configuration for Statistical Analysis ================ #
# =========================================================================== #


LEVEL: Tiles # "CpG" or "Tiles" study by Tiles or by CpG 

# if Tiles was selected, define tile size, step size, and minimal number of CpG /tile
TILESIZE: 250 
STEPSIZE: 1  # Tiles relative step size
NB_CPG_TILES: 1 # minimal number of CpG to keep a tile in the analysis

# if CpG was selected, your can choose to merge the reads from both strands
DESTRAND: yes # if yes, reads on both strands of a CpG dinucleotide will be merged. This provides better coverage, but only advised when looking at CpG methylation (for CpH methylation this will cause wrong results in subsequent analyses). This have no effect working on Tiles. 

# If CpG was selected, it is possible to perform a differential methylation analysis by region 
# To use dmrseq, you need to have at least 2 samples in each condition
DMR: yes

## params (only DMR)
DMR_TYPE: blocks  # regions or blocks, by default regions 
MIN_CPG: 3  # minimum number of CpGs to consider for a candidate DMR, by default 5, minimum 3
MAX_GAP: 6000 # maximum number of bp in between neighboring CpGs to be included in the same DMR, by default 1000 (for blocks : 5e3) 
CUTOFF: 0.05 # cutoff of the single CpG coefficient (methylation difference) that is used to discover candidate DMR. by default 0.1
FDR: 0.05 # QVALUE for select significant DMR


# ===== Exploratory analysis ===== #
## params 
MINCOV: 20  # int, minimum coverage depth for the analysis
COV.PERC: 99.9 # to the coverage filter, choose the percentile for remove top ..% (MKit_diff_bed.R and MKit_Exploration.Rmd)
MINQUALI: 20  # int, minimum quality to keep a CpG for the analysis
UNITE: all # 'all' or 'one' (at least one per group)
MIN_PER_GROUP: 3 

# ===== Differential analysis ===== #
## params
LIST_SIGNIDIF: [10, 20, 25, 30] # SigDiffMeth en %, used in MKit_diff_bed.R
LIST_QVALUE : [0.001, 0.01, 0.05]  # used in MKit_diff_bed.R
QVALUE: 0.05  # QValue (used in Mkit_differential.Rmd)
SIGNIDIF: 10


# ======================= Configuration for annotations ===================== # 
# =========================================================================== #

# ===== Standard annotations  ===== #
## GTF 
GTFPATH: TestDataset/my_bank/gencode.vM27.annotation_chr19_mini.gtf

## CPG Bed 
BEDPATH: TestDataset/my_bank/cpgIslandExt.mm39_mini.bed

# ===== Customs Annotations ===== #
CUSTOM_ANNOT: no 
METAFILE_ANNOT: configs/metadata_annot.tsv
CUSTOM_ANNOT_PATH: "/shared/projects/wgbs_flow/Elouan/Custom_tracks/"
```








