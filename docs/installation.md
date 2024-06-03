# Setup 

Biologists and bioinformaticians often struggle with software installation. To avoid this issue, we have containerized all the software environments in an Apptainer/Singularity image. This image, along with all the required scripts, can be cloned from our Methylator GitHub repository, available [here](https://github.com/parisepigenetics/WGBSflow). If you're using the Jupyter Hub on IFB, you can open a **Terminal** by clicking on the corresponding icon. 

![jupyterHub](img/JupyterHub.png)

Before clonning Methylator, go to your project using the `cd` command.

```
[username @ cpu-node-12 ]$ cd /shared/projects/YourProjectName
```
Now you can clone the repository (use `-b v0.1` to specify the version). 

```bash
[username@clust-slurm-client YourProjectName]$ git clone git@github.com:parisepigenetics/Methylator.git
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
├── configs
│   ├── cluster_config_ifb.yaml
│   ├── cluster_config_ipop.yaml
│   ├── cluster_config.yaml
│   ├── config_nanopore.yaml
│   ├── config_wgbs.yaml
│   ├── metadata_annot.tsv
│   └── metadata.tsv
├── LICENSE
├── my_bank
├── README.md
├── scripts
│   ├── Annotatr.R
│   ├── build_DAG_graphes.sh
│   ├── check_config_path.py
│   ├── colors.yaml
│   ├── DMR.Rmd
│   ├── edc_workflows.py
│   ├── edmr.R
│   ├── final_report_comp.Rmd
│   ├── final_report.Rmd
│   ├── getquota2.sh
│   ├── images
│   ├── main_cluster.py
│   ├── MKit_BedgraphDiff.R
│   ├── MKit_Bedgraph.R
│   ├── MKit_BSMAP.R
│   ├── MKit_diff_bed.R
│   ├── Mkit_differential.Rmd
│   ├── MKit_diff_fig.R
│   ├── MKit_Exploration_all.Rmd
│   ├── MKit_Exploration.Rmd
│   ├── MKit_prep_differential.R
│   ├── MKit_prep_nanopore.R
│   ├── MKit_prep_WGBS.R
│   ├── parse_yaml.sh
│   ├── parsinglog_flow.py
│   ├── parsinglog.py
│   ├── reporting.py
│   ├── run_rule.sh
│   ├── test_bam.R
│   └── Unlock.sh
├── TestDataset
│   ├── bam_nanopore
│   ├── configs
│   ├── fastq
│   └── my_bank
├── WGBSworkflow.sh
└── workflow
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

