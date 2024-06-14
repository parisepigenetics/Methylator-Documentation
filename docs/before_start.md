# Requirements

## Cluster account 

Create an account and a project on your favorite cluster, please follow instructions using the links below :    
- [IFB core cluster](https://parisepigenetics.github.io/bibs/cluster/ifb/#/cluster/)  
- [iPOP-UP cluster](https://parisepigenetics.github.io/bibs/cluster/ipopup/#/cluster/)

Methylator has been developed on the IPOP-UP cluster and tested on the IFB cluster. It should work on any HPC managed by [slurm](https://slurm.schedmd.com/quickstart.html) and with [Apptainer](https://apptainer.org/) installed. 

## Storage 

Depending on your analyses, the disk space required to run the workflow can vary significantly. For RRBS data, we recommend having at least 500 GB available. For WGBS data, we recommend having at least 1.5 TB available.

!!! warning
    For whole genome datasets with many samples, several terabytes may be necessary.

**The following table is provided as a guideline to estimate the required disk space based on the volume of the initial dataset (fastq.gz):**

| fastq.gz | Big data | Result data | Disk space requirement | Data type |
| -------- | -------- | ----------- | ---------------------- | --------- |
| 120 Go   | 530 Go   |             |                        |    WGBS   |
| 178 Go   |          |             |                        |    WGBS   |
| 676 Go   |          |             |          2 To ?        |    WGBS   | 

Note: The disk space required is greater than the data volume written at the end of the workflow execution. This is because the mapping step (Bismark) requires writing a large volume of temporary data files, which are deleted at the end of this step.

## Required Knowledge for Quick Start

!!! warning
    Methylator was designed to be as user-friendly as possible; however, it is a tool dedicated to complex biological analyses and requires certain prerequisites to ensure a quick start and understanding of the analyses performed.

- Be comfortable with the UNIX/Linux operating system, particularly the file structure and main commands.
- Know how to use a computing cluster (HPC) with the SLURM scheduler.
-  Understand the fundamental steps of DNA methylation analysis and the associated file formats. The [biological context](biological_context.md) section can be used for this.
-  Have general knowledge in statistics/biostatistics.
