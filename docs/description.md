
# Workflow Description 

## Utilisation 

Methylator permet d'analyser les données de méthylation de l'ADN depuis deux sources principales :  
- Les données BS-seq (après traitement au Bisulfite de Sodium). Soit en Whole Genome (WGBS), soit en régions enrichie en GC (RRBS). 
- Les données de séquencage nanopore. Soit en Whole Genome, soit

Pour le moment Methylator n'est pas compatible avec les données de puces. 

Dans le cas des données BSseq, le workflow démarre avec les fichiers au format FATSQ. Il réalise l'ensemble des étapes classques permettant d'obtenir
les tables de comptes de méthylation. (Voir analyses step by steps). Dans le cas des données nanopore, le workflow démarre avec les fichiers .BAM
issue du basecalling. La seconde partie du workflow, qui se concentre sur l'analyses de la méthylation est commune aux deux type de données. 

Ceci fait de Methylator un outil de choix lorsque l'on souhaite comparer ces deux méthodes. 



## Folders organisation 

![folders_organisation](img/folder_organisation_worklfow.png)

Le dossier `configs`contient l'ensemble des fichiers de configurations.  

Le dossier ` scripts ` contient l'ensemble des scripts nécessaire au fonctionnement du worklow, à l'execption des script Snakemake.  

Le dossier ` worklfow ` contient l'ensemble des scripts Snakemake ".rules".   

Le dossier ` TestDataset ` contient l'ensemble des fichiers nécessaire pour tester le workflow avec un petit jeu de données.   

Le dossier ` my_bank ` est un dossier vide. Il permet de stocker des génomes de références et des fichiers d'annotations (FASTA, GTF, BED ...) pour différentes espèces lorsque les fichiers en question ne sont pas disponibles dans les banques présentes sur votr cluster. Il existe un scipt search_bank.sh qui lorsqu'il est exécuté télécharge automatiquement (lorsque c'est possible) tous les fichiers d'annotations nécessaire pour le lancement du workflow from UCSC Golden Path. Il prend comme argument le nom du génome de référence de l'espèce en question. par exemple :  

``` sh
srun scripts/search_bank.sh mm39 
```
![search_banks](img/search_banks_example.png)


## Main scripts 

Methylator is launched as a python script named `main_cluster.py` which calls the workflow manager named [Snakemake](https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html). 
Snakemake will execute rules that are defined in `workflow/xxx.rules` and distribute the corresponding jobs to the computing nodes via [SLURM](https://ifb-elixirfr.gitlab.io/cluster/doc/slurm/slurm_user_guide/). 

![cluster_chart](img/cluster_chart.pdf.png)

On the cluster, the main python script is launched via the shell script `Workflow.sh`,
which basically contains only one command `python main_cluster.py` (+ loading of basic modules and information about the run).
