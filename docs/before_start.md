# Requirements

## Cluster account 

Create an account and a project on your favorite cluster, please follow instructions using the links below :    
- [IFB core cluster](https://parisepigenetics.github.io/bibs/cluster/ifb/#/cluster/)  
- [iPOP-UP cluster](https://parisepigenetics.github.io/bibs/cluster/ipopup/#/cluster/)

Methylator has been developed on the the IPOP-UP cluster and tester on IFB cluster.
Should works on every HPC administré par [slurm](https://slurm.schedmd.com/quickstart.html) et sur lequel est installé [Apptainer](https://apptainer.org/). 

## Storage 

Depending on your analyses, the disk space required to run the workflow can vary significantly. For RRBS data, we recommend having at least 500 GB available. For WGBS data, we recommend having at least 1.5 TB available.

!!! warning
    For whole genome datasets with many samples, several terabytes may be necessary.

**The following table is provided as a guideline to estimate the required disk space based on the volume of the initial dataset:**


## Required Knowledge for Quick Start

!!! warning
    Methylator was designed to be as user-friendly as possible; however, it is a tool dedicated to complex biological analyses and requires certain prerequisites to ensure a quick start and understanding of the analyses performed.

- Be comfortable with the UNIX/Linux operating system, particularly the file structure and main commands.
- Know how to use a computing cluster (HPC) with the SLURM scheduler.
-  Understand the fundamental steps of DNA methylation analysis and the associated file formats. The [biological context](biological_context.md) section can be used for this.
-  Have general knowledge in statistics/biostatistics.
