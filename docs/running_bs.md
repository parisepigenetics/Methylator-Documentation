# Running your BS-seq pre-processing step by step

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

You should also check SLURM output files. See [Description of the log files](#description-of-the-log-files).


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
[username@clust-slurm-client Methylator]$ wget ???????
[username@clust-slurm-client index]$ tar -zxvf ???? 
```

- **GTF** files can be downloaded from [GenCode](https://www.gencodegenes.org/) (mouse and human), [ENSEMBL](https://www.ensembl.org/info/data/ftp/index.html), [NCBI](https://www.ncbi.nlm.nih.gov/assembly/) (RefSeq, help [here](https://www.ncbi.nlm.nih.gov/genome/doc/ftpfaq/#files)), ...
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
# ================== Control of the workflow ==================
??????????

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
