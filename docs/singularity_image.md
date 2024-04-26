# Singularity image

Methylator uses a Singularity image to run on HPC clusters. 
Singularity is a technology that allows you to build environments isolated from the rest of your machine,
that depend solely on the machine's Linux kernel. These isolated environments are called **containers**. 
An image is a photograph, an exaustive and static description of a container. It enables sharing,
instantiation and execution of containers. Methylator uses this technology to run in an isolated 
isolated environment containing all the tools (with the right versions) required for its operation. The aim of this approach is to improve the reproducibility of analyses carried out by the workflow and increase its durability. 

For more information you can look the Singularity container [documentation](https://docs.sylabs.io/guides/3.0/user-guide/quick_start.html).

Scheme of a singularity container 

![sheme_sing](img/sing1.png)
