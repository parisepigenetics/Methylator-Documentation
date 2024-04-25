# Installation and description

In order to install Methylator, you  have to clone the Methylator GitHub repository to your cluster project. 
If you're using the Jupyter Hub on IFB, you can open a **Terminal** by clicking on the corresponding icon. 

![jupyterHub](img/jupyterHub.png)

Before clonning Methylator, go to your project using the `cd` command.

```
[username @ cpu-node-12 ]$ cd /shared/projects/YourProjectName
```
Now you can clone the repository (use `-b v0.1` to specify the version). 

```bash
[username@clust-slurm-client YourProjectName]$ git clone https://github.com/parisepigenetics/Methylator
Cloning into 'Methylator'...
remote: Enumerating objects: 670, done.
remote: Counting objects: 100% (42/42), done.
remote: Compressing objects: 100% (41/41), done.
remote: Total 670 (delta 1), reused 31 (delta 1), pack-reused 628
Receiving objects: 100% (670/670), 999.24 MiB | 32.76 MiB/s, done.
Resolving deltas: 100% (315/315), done.
Checking out files: 100% (84/84), done.
```
Enter `Methylator` directory (`cd`) and look at the files using `tree` or `ls`.
```
[username@clust-slurm-client YourProjectName]$ cd Methylator
[username@clust-slurm-client Methylator]$ tree -L 2
.
????????????????
```

Methylator is launched as a python script named `main_cluster.py` which calls the workflow manager named [Snakemake](https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html). Snakemake will execute rules that are defined in `workflow/xxx.rules` and distribute the corresponding jobs to the computing nodes via [SLURM](https://ifb-elixirfr.gitlab.io/cluster/doc/slurm/slurm_user_guide/). 

![cluster_chart](img/cluster_chart.pdf.png)


 On the cluster, the main python script is launched via the shell script `Workflow.sh`, which basically contains only one command `python main_cluster.py` (+ loading of basic modules and information about the run).
