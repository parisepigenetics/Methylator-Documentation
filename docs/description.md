
# Workflow Description 

## Utilisation 



## Folders organisation 

![folders_organisation](img/folder_organisation_worklfow.png)

Le dossier `configs`contient l'ensemble des fichiers de configurations.  

Le dossier ` scripts ` contient l'ensemble des scripts nécessaire au fonctionnement du worklow, à l'execption des script Snakemake.  

Le dossier ` worklfow ` contient l'ensemble des scripts Snakemake ".rules".   

Le dossier ` TestDataset ` contient l'ensemble des fichiers nécessaire pour tester le workflow avec un petit jeu de données.   

Le dossier ` my_bank ` est un dossier vide. Il permet de stocker des génomes de références et des fichiers d'annotations (FASTA, GTF, BED ...) pour différentes espèces lorsque les fichiers en question ne sont pas disponibles dans les banques présentes sur votr cluster. Il existe un scipt search_bank.sh qui lorsqu'il est exécuté télécharge automatiquement (lorsque c'est possible) tous les fichiers d'annotations nécessaire pour le lancement du workflow. Il prend comme argument le nom du génome de référence de l'espèce en question. par exemple :  

``` sh
srun scripts/search_bank.sh mm39 
``` 
## Main scripts 

Methylator is launched as a python script named `main_cluster.py` which calls the workflow manager named [Snakemake](https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html). 
Snakemake will execute rules that are defined in `workflow/xxx.rules` and distribute the corresponding jobs to the computing nodes via [SLURM](https://ifb-elixirfr.gitlab.io/cluster/doc/slurm/slurm_user_guide/). 

![cluster_chart](img/cluster_chart.pdf.png)

On the cluster, the main python script is launched via the shell script `Workflow.sh`,
which basically contains only one command `python main_cluster.py` (+ loading of basic modules and information about the run).
