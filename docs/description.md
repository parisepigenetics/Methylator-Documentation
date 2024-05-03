
# Description 

Les dossiers sont organisés de la manière suivante : 
![folders_organisation](img/folder_organisation_worklfow.png)

Methylator is launched as a python script named `main_cluster.py` which calls the workflow manager named [Snakemake](https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html). 
Snakemake will execute rules that are defined in `workflow/xxx.rules` and distribute the corresponding jobs to the computing nodes via [SLURM](https://ifb-elixirfr.gitlab.io/cluster/doc/slurm/slurm_user_guide/). 

![cluster_chart](img/cluster_chart.pdf.png)

On the cluster, the main python script is launched via the shell script `Workflow.sh`,
which basically contains only one command `python main_cluster.py` (+ loading of basic modules and information about the run).
