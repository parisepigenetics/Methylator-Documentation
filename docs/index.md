# Methylator, a DNA methylation analysis workflow

<small>Maintained by [BiBs](mailto:bibsATparisepigenetics.com). Last update : 25/04/2024. Methylator v0.1. </small>  

Methylator is a complete Snakemake workﬂow to analyse DNA methylation data. Methylator runs in a dedicated Apptainer image to allow for reproducibility and was optimized to compute effciently the data on HPC clusters. We aim to make those complex analyses do able by biologists with no or little bioinformatics background.

!!! warning
    This is a BETA version of the workflow, and of this documentation ! There is no guarantee !   
    If you use this workflow to analyse your data, don't forget to **acknowledge BiBs** in all your communications ! 

!!! quote "Acknowledge BiBs example"   
    **For EDC people :** We thank the Bioinformatics and Biostatistics Core Facility, Paris Epigenetics and Cell Fate Center for bioinformatics support.   
    **For External users :** We thank the Bioinformatics and Biostatistics Core Facility, Paris Epigenetics and Cell Fate Center for sharing their analysis workflows.

## Table of content 
- [Before starting](before_start.md)
- [Installation](installation.md)
- [Description](description.md)
- [Adapt cluster](adapt_cluster.md)
- [Extra Help !](extra_help.md)
- [Singularity](singularity_image.md)
- [Resources](resources.md)

## Pipeline scheme 
![Methylator Schema](img/poster_methylator.svg)

Implemented by [BiBs-EDC](https://parisepigenetics.github.io/bibs/), this workflow for DNA methylation data analysis runs effectively on both IFB and iPOP-UP clusters. If you encounter troubles or need additional tools or features, you can create an issue on the [GitHub repository](https://github.com/parisepigenetics/Methylator/issues), email directly [BiBs](mailto:bibsATparisepigenetics.com).
