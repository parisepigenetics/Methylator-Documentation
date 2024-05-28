
# Running your nanopore pre-processing step by step

##  Process BAM to BED 

If you launch the workflow with nanopore data, the first step is to convert the BAM files to BED using the modbam2bed tool recommended by Oxford Nanopore Technology (ONT). If you have already completed this step, set it to yes. If you already have BED files from modbam2bed, you can specify the path to the folder containing them. The BED files should be named {Sample}_Merged.cpg.bed.

```yaml
# # ===================== Configuration for process BAM files  ================== #
# =========================================================================== #

STARTFROMBED: no # put no if you start from BAM files
```

```sh
sbatch WGBSflow.sh nanopore
```
