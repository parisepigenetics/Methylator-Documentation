
# Workflow architecture

## Folders organisation 

![folders_organisation](img/folder_organisation_worklfow.png)

The `configs` folder contains all the configurations files.
Files present here are the only ones you need to modify to use the workflow.

```bash
.
├── cluster_config_ifb.yaml
├── cluster_config_ipop.yaml
├── cluster_config.yaml
├── config_nanopore.yaml
├── config_wgbs.yaml
├── config.yaml
├── metadata_annot.tsv
└── metadata.tsv
```

The `scripts` folder contains all the scripts necessary for the workflow's operation, except for the Snakemake scripts. Please do not touch anything here. 

```bash
.
├── Annotatr.R
├── build_DAG_graphes.sh
├── check_config_path.py
├── colors.yaml
├── DMR.Rmd
├── DMR_RRBS.Rmd
├── edc_workflows.py
├── final_report_comp.Rmd
├── final_report.Rmd
├── getquota2.sh
├── images
│   ├── bibs_logo_.png
│   ├── cpg_annot.jpeg
│   └── gene.jpeg
├── main_cluster.py
├── MKit_BedgraphDiff.R
├── MKit_Bedgraph.R
├── MKit_BSMAP.R
├── MKit_diff_bed.R
├── Mkit_differential.Rmd
├── MKit_diff_fig.R
├── MKit_Exploration_all.Rmd
├── MKit_Exploration.Rmd
├── MKit_prep_differential.R
├── MKit_prep_nanopore.R
├── MKit_prep_WGBS.R
├── ORA.py
├── parse_yaml.sh
├── parsinglog_flow.py
├── parsinglog.py
├── prep_array.R
├── reporting.py
├── run_rule.sh
├── search_bank.sh
└── test_bam.R
```

The  `workflow ` folder contains all the Snakemake scripts ".rules". Please do not touch anything here.

```bash
.
├── config_main_schema.yaml
├── config_mapping_schema.yaml
├── config_methylator_schema.yaml
├── config_QC_schema.yaml
├── config_trim_schema.yaml
├── differential.rules
├── exploration.rules
├── fastq_dump_QC.rules
├── mapping.rules
├── nanopore.yml
├── report.rules
├── samples.schema.yaml
├── Singularity_ncbi
├── trim.rules
└── wgbsflow.yaml
```

The  ` TestDataset `  folder contains all the files necessary to test the workflow with a small dataset.    

```bash
.
├── bam_nanopore
│   ├── RRMS_2marks_NP95
│   └── RRMS_2marks_WT
├── configs
│   ├── config_nanopore.yaml
│   ├── config_wgbs.yaml
│   ├── metadata_nano2.tsv
│   ├── metadata_nano.tsv
│   └── metadata_wgbs.tsv
├── fastq
│   ├── select_sam.sh
│   ├── SRR11806587_sub500000_chr19_R1.fastq.gz
│   ├── SRR11806587_sub500000_chr19_R2.fastq.gz
│   ├── SRR11806588_sub500000_chr19_R1.fastq.gz
│   ├── SRR11806588_sub500000_chr19_R2.fastq.gz
│   ├── SRR11806589_sub500000_chr19_R1.fastq.gz
│   ├── SRR11806589_sub500000_chr19_R2.fastq.gz
│   ├── SRR9016926_sub500000_chr19_R1.fastq.gz
│   ├── SRR9016926_sub500000_chr19_R2.fastq.gz
│   ├── SRR9016927_sub500000_chr19_R1.fastq.gz
│   ├── SRR9016927_sub500000_chr19_R2.fastq.gz
│   ├── SRR9016928_sub500000_chr19_R1.fastq.gz
│   └── SRR9016928_sub500000_chr19_R2.fastq.gz
└── my_bank
    ├── cpgIslandExt.mm39.bed
    ├── cpgIslandExt.mm39_mini.bed
    ├── gencode.vM27.annotation_chr19.gtf
    ├── gencode.vM27.annotation_chr19_mini.gtf
    ├── mm39_chr19_mini.fa
    └── rrms_mm39_mini.bed
```


The `my_bank` folder is an empty directory. It is used to store reference genomes and annotation files (FASTA, GTF, BED, etc.) for different species when the required files are not available in the banks present on your cluster (refer to [annotation](annotations.md) ).  

## Folders organisation after run 

After launching the workflow, new folders are created. One folder for easily usable results, which you can name as you wish in the configuration file. One folder
for 'heavy' results, also nameable as you wish. A log folder, and a slurm_output folder.

### Exemple of folders organisation after launching 
![folders_organisation_results](img/folder_organisation_worklfow_results.png)

Le dossier Results contient les résultas de chaque projet. Dans chaque dossiers projets, les résultats sotn organisées dand des dossiers distincts corrspondants aux différentes étapes du workflow : fastq download + QC, trimming + QC, mapping + QC et analyse de la methylation.

### Exemple de l'organisation des résultats 
![results_folders](img/results_folders.png)

De plus, à la fin de l'exécution du workflow, si vous avez choisie de générer un rapport, un dossier zippé et horodaté (unique, permet de versionner vos analyses) est généré afin de partager facilement vos résultats, ce dossier contient l'ensemble des fichiers HTML (QC + analyses statistiques). Ce dossier est accompagné d'un fichier HMTL (final report), également horodaté, contenant des liens redirigeant vers les différents rapports HTML afin de facilité la navigation dans les résultats. 

### Exemple liste des fichiers contenue dans final_report_(project)_(horodatage).tar.gz
![final_report_Test_WGBS_20240828T1023.tar.gz](img/final_report_Test_WGBS_20240828T1023.tar.gz.png)

### Exemple de fichier final_report_(projet)_(horodatage).html
![final_report_Test_WGBS_20240828T1023.html](img/final_report_Test_WGBS_20240828T1023.png)


Dans la partie Methylation_analysis des résultats, afin de faciliter la possibilité de tester de nombreuses paramétrisations sur un même jeu de données, si vous relancé le workflow (pour le même project) après avoir modifié un paramètre dans la partie Methylation Analysis du fichier de configuration, une nouvelle analyse de la méthylation est réalisé. Cette analyse est stocké dans le dossier last_analysis. A chaque nouvelle analyse un dossier last_analys est généré et les anciens dossiers sont renommé analysis_(numero).

![Methylation_analysis](img/Methylation_analysis.png)


Pour conserver les informations sur la parmétrisation de vos analyse, chaque dossier last_analys ou analysis_(numero) contient un clone du fichier de configuration ayant servie à généer les résultats. Ainsi, il est possible de réaliser un nombre d'analyse de la méthylation sans se perdre. Les analyses de Methylation sont organisées selon les différentes étapes des analyses à savoir : exploration, analyse diféfrentielle en CpG et Tiles (Differential) , analyse différentielle en régions (DMR) et analys ed'enrichissement (ORA). Ainsi qu'un fichier BigWig contenant des bigwig de la différence de méthylation (en CpG) pour chaque comparaison. 

![Last_analysis](img/Last_analysis.png)











