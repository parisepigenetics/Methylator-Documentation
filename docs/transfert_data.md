# Transfer your data
If you want to use your own data, you should transfer the FASTQ files into your project folder `/shared/projects/YourProjectName` before doing your analysis. Alternatively the workflow allows you to download data from [SRA](https://www.ncbi.nlm.nih.gov/sra/docs/sradownload/) simply giving the `SRRxxx` IDs, see below [metadata.tsv](preparing_run.md#metadata.tsv). 

## FASTQ names

The workflow is expecting **gzip-compressed FASTQ files** with names formatted as   
- `SampleName_R1.fastq.gz` and `SampleName_R2.fastq.gz` for pair-end data,   
- `SampleName.fastq.gz` for single-end data. 

If your files are not fitting this format, please see [how to correct the names of a batch of FASTQ files](extra_help.md#quickly-change-fastq-names). 

## Generate md5sum

It is highly recommended to check the [md5sum](https://en.wikipedia.org/wiki/Md5sum) for big files. If your raw FASTQ files are on your computer in `PathTo/MethylProject/Fastq/`, you type in a terminal: 

```
You@YourComputer:~$ cd PathTo/MethylProject
You@YourComputer:~/PathTo/MethylProject$ md5sum Fastq/* > Fastq/fastq.md5
```

## Copy to the cluster

You can then copy the `Fastq` folder to the cluster using `rsync`, replacing `username` by your login: 

```
You@YourComputer:~/PathTo/MethylProject$ rsync -avP  Fastq/ username@core.cluster.france-bioinformatique.fr:/shared/projects/YourProjectName/Raw_fastq
```

In this example the FASTQ files are copied from `PathTo/MethylProject/Fastq/` on your computer into a folder named `Raw_fastq` in your project folder on IFB core cluster. On iPOP-UP cluster, only the address is different: 

```
You@YourComputer:~/PathTo/MethylProject$ rsync -avP  Fastq/ username@ipop-up.rpbs.univ-paris-diderot.fr:/shared/projects/YourProjectName/Raw_fastq
```

Feel free to name your folders as you want! 
You will be asked to enter your password, and then the transfer will begin. If it stops before the end, rerun the last command, it will only add the incomplete/missing files. 

## Check md5sum

After the transfer, connect to the cluster ([IFB](https://parisepigenetics.github.io/bibs/cluster/ifb/#/cluster/), [iPOP-UP](https://parisepigenetics.github.io/bibs/cluster/ipopup/#/cluster/)) and check the presence of the files in `Raw_fastq` using `ls` or `ll` command.  

```
[username@clust-slurm-client YourProjectName]$ ll Raw_fastq
```

Check that the transfer went fine using `md5sum`.

```
[username@clust-slurm-client YourProjectName]$ cd Raw_fastq
[username@clust-slurm-client Raw_fastq]$ md5sum -c fastq.md5
```
