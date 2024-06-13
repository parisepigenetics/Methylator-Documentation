
# Running your analysis step by step

When the configuration files are ready, you can start the run by `sbatch Workflow.sh wgbs`.

```
[username@clust-slurm-client Methylator]$ sbatch Workflow.sh wgbs
```

Please see below detailed explanation. 


## FASTQ quality control

Prerequisite:   
- Using your own data: your FASTQ files are on the cluster, in our example in `/shared/projects/YourProjectName/Raw_fastq` (but you can name your folders as you want, as long as you adjust the `READSPATH` parameter in `config_main.yaml`). 
- You have modified `config/metadata.tsv` according to your experimental design (with sample names or SRR identifiers).

Now you have to check in `config/config_main.yaml` that: 

- you gave a project name

```yaml
# Project name
PROJECT: EXAMPLE
```

- In `Control of the workflow`, `QC` or `SRA` is set to `yes`:

If you want to download data from SRA, set `SRA` to `yes`. The QC will follow automatically. If you use your own data, put `QC` to `yes` and `SRA` to `no`.  

```yaml
## Do you want to download FASTQ files from public from Sequence Read Archive (SRA) ? 
SRA: no  # "yes" or "no". If set to "yes", the workflow will stop after the QC to let you decide whether you want to trim your raw data or not. In order to run the rest of the workflow, you have to set it to "no".

## Do you need to do quality control?
QC: yes  # "yes" or "no". If set to "yes", the workflow will stop after the QC to let you decide whether you want to trim your raw data or not. In order to run the rest of the workflow, you have to set it to "no".
```

The rest of the part `Control of the workflow` will be **ignored**. The software will stop after the QC to give you the opportunity to decide if trimming is necessary or not. 

- The shared parameters are correct (paths to the FASTQ files, metadata.tsv, result folders, single or paired-end data). 

```yaml
## the path to fastq files
READSPATH: /shared/projects/YourProjectName/Raw_fastq

## the meta file describing the experiment settings
METAFILE: /shared/projects/YourProjectName/configs/metadata.tsv

## paths for intermediate and final results
BIGDATAPATH: /shared/projects/YourProjectName/data # for big files
RESULTPATH: /shared/projects/YourProjectName/results
```

When this is done, you can start the QC by running:

```
[username@clust-slurm-client Methylator]$ sbatch Workflow.sh wgbs
```

You can check if your job is running using squeue.

```
[username@clust-slurm-client Methylator]$ squeue --me
```

You should also check SLURM output files. See [Description of the log files](extra_help.md#description-of-the-log-files).


## Trimming

If you put `TRIMMED: no`, there will be no trimming and the original FASTQ sequences will be mapped. 

If you put `TRIMMED: yes`, [Trim Galore](https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim_Galore_User_Guide.md) will remove low quality and very short reads, and cut the adapters. If you also want to remove a fixed number of bases in 5' or 3', you have to configure it. For instance if you want to remove the first 10 bases: 

```yaml
# ================== Configuration for trimming ==================

## Number of trimmed bases
## put "no" for TRIM3 and TRIM5 if you don't want to trim a fixed number of bases.
TRIM5: 10 #  integer or "no", remove N bp from the 5' end of reads. This may be useful if the qualities were very poor, or if there is some sort of unwanted bias at the 5' end. 
TRIM3: no # integer or "no", remove N bp from the 3' end of reads AFTER adapter/quality trimming has been performed. 
```

## Mapping

At this step you have to provide the path to your genome index as well as to a GTF annotation file and a BED file with CpG island coordinates. 

!!! info "Use common banks"
    Some reference files are shared between cluster users. Before downloading a new reference, check what is available at `/shared/bank/` (IFB) or `/shared/banks/` (iPOP-UP).

```bash
[username@clust-slurm-client ~]$ tree -L 2 /shared/bank/homo_sapiens/
/shared/bank/homo_sapiens/
├── GRCh37
│   ├── bowtie2
│   ├── fasta
│   ├── gff
│   ├── star -> star-2.7.2b
│   ├── star-2.6
│   └── star-2.7.2b
├── GRCh38
│   ├── bwa
│   ├── fasta
│   ├── gff
│   ├── star -> star-2.6.1a
│   └── star-2.6.1a
├── hg19
│   ├── bowtie
│   ├── bowtie2
│   ├── bwa
│   ├── fasta
│   ├── gff
│   ├── hisat2
│   ├── picard
│   ├── star -> star-2.7.2b
│   ├── star-2.6
│   └── star-2.7.2b
└── hg38
    ├── bowtie2
    ├── fasta
    ├── star -> star-2.7.2b
    ├── star-2.6
    └── star-2.7.2b

30 directories, 0 files
```

If you don't find what you need, you can ask for it on [IFB](https://community.france-bioinformatique.fr/) or [iPOP-UP](https://discourse.rpbs.univ-paris-diderot.fr/c/ipop-up) community support. In case you don't have a quick answer, you can download (for instance [here](http://refgenomes.databio.org/)) or produce the indexes you need in your folder (and remove it when it's available in the common banks). Copy the path to the file you need and then paste the link to `wget`. When downloading is over, you might have to decompress the file. 

```
[username@clust-slurm-client Methylator]$ wget -O - http://hgdownload.soe.ucsc.edu/goldenPath/mm39/bigZips/mm39.fa.gz | gunzip -c > my_bank/mm39.fa
```

- **GTF** files can be downloaded from [GenCode](https://www.gencodegenes.org/) (mouse and human), UCSC, [ENSEMBL](https://www.ensembl.org/info/data/ftp/index.html), [NCBI](https://www.ncbi.nlm.nih.gov/assembly/) (RefSeq, help [here](https://www.ncbi.nlm.nih.gov/genome/doc/ftpfaq/#files)), ...
Similarly you can download them to the server using `wget`. 

- **CpG Islands** files can be downloaded from [UCSC goldenPath](https://hgdownload.soe.ucsc.edu/goldenPath/hg38/database/). For obtain the bed file, you can use this command (example with hg38): 

```sh
 wget -qO- http://hgdownload.cse.ucsc.edu/goldenpath/hg38/database/cpgIslandExt.txt.gz \
   | gunzip -c  > cpgIslandExt.hg38.txt
```

!!! info "Fill common banks"
    Don't forget to give the links to the new references you made/downloaded to [IFB](https://community.france-bioinformatique.fr/) or to [iPOP-UP](https://discourse.rpbs.univ-paris-diderot.fr/c/      ipop-up) support so that they can add them to the common banks.

Be sure you give the right path to those files and adjust the other settings to your need: 


```yaml
# ================== Configuration for alignment to genome ================== # 

## aligner
ALIGNER: BOWTIE2 # "BOWTIE2"

## genome files
GENOMEPATH: my_bank/mm39.fa #path to the reference genome's folder 

## genome
ASSEMBLY: mm39 # short name of the assembly used for the analysis

## Library type
LIBRARY_TYPE_OPT: non_directional   # "directional", "non_directional" or "pbat" sequencing library 

## params in Bismark alignment (--score_min {function},{val1},{val2}). 
# More information about Bismark options: https://felixkrueger.github.io/Bismark/options/alignment/

TYPEFUNC: L # 'L' or 'G' for linear or logarithmic fonction
VARFUNCT1: 0 # first value for the function 0 is default
VARFUNCT2: -0.6 # second value for the function -0.2 is default
MISMATCH: 0 # nomber of mismatch wanted
ADVANCE_OPT:    # for advanced bismark user (mapping only), IMPLEMENTED IN THE CODE BUT NOT TESTED
```

For an easy visualisation on a genome browser, BigWig files are generated. 


As at the moment the default project quota in 250 Go you might be exceeding the space you have (and may or may not get error messages...). So if the mapping fails, try removing files to get more space, or ask to increase your quota on [IFB Community support](https://community.cluster.france-bioinformatique.fr) or [iPOP-UP Community support](https://discourse.rpbs.univ-paris-diderot.fr/c/ipop-up). To see the space you have you can run:

``` 
[username@clust-slurm-client Methylator]$ du -h --max-depth=1 /shared/projects/YourProjectName/
```

and

```
[username@clust-slurm-client Methylator]$ lfsgetquota YourProjectName
```


##  Process BAM to BED (only for nanopore)

If you launch the workflow with nanopore data, the first step is to convert the BAM files to BED using the modbam2bed tool recommended by Oxford Nanopore Technology (ONT). If you have already completed this step, set it to yes. If you already have BED files from modbam2bed, you can specify the path to the folder containing them. The BED files should be named {Sample}_Merged.cpg.bed.

```yaml
# # ===================== Configuration for process BAM files  ================== #
# =========================================================================== #

STARTFROMBED: no # put no if you start from BAM files
```


## Statistical analysis

At this stage, you need to select the global parameters for the statistical analysis.

```yaml
LEVEL: CpG # "CpG" or "Tiles" study by Tiles or by CpG 
TILESIZE: 250 
STEPSIZE: 1  # Tiles relative step size
NB_CPG_TILES: 1 # minimal number of CpG to keep a tile in the analysis
```
If **Tiles** was selected, define tile size, step size, and minimal number of CpG by tile. 
**STEPSIZE** corresponds to the relative size of the gap between two tiles. If it is set to 1, the tiles are non-overlapping, if it is set to 0.5, the tiles overlap by half.

### Exploratory analysis 

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

### Differential methylation analysis

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

#### BedGraphe 

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
