
# Running your nanopore pre-processing step by step

## Configuration 



``` yaml

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

```





##  Process BAM to BED 

If you launch the workflow with nanopore data, the first step is to convert the BAM files to BED using the modbam2bed tool recommended by Oxford Nanopore Technology (ONT). If you have already completed this step, set it to yes. If you already have BED files from modbam2bed, you can specify the path to the folder containing them. The BED files should be named {Sample}_Merged.cpg.bed.

```yaml
# # ===================== Configuration for process BAM files  ================== #
# =========================================================================== #

STARTFROMBED: no # put no if you start from BAM files
```

## Start the workflow 

You can start the workflow by running:

```sh
sbatch WGBSflow.sh nanopore
```
