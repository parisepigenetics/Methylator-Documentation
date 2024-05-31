# Requirements

## Cluster account 

Create an account and a project on your favorite cluster, please follow instructions using the links below :
- [IFB core cluster](https://parisepigenetics.github.io/bibs/cluster/ifb/#/cluster/)  
- [iPOP-UP cluster](https://parisepigenetics.github.io/bibs/cluster/ipopup/#/cluster/)

Methylator has been developed on the the IPOP-UP cluster and tester on IFB cluster.
Should works on every HPC administré par [slurm](https://slurm.schedmd.com/quickstart.html) et sur lequel est installé [Apptainer](https://apptainer.org/). 

## Storage 

En fonction de vos analyses l'espace disque nécessaire à l'exécution du workflow peut fortement changer. 
Pour des données RRBS nous conseillons d'avoir à disposition un minimum de 500 Go. 
Pour des données WGBS nous conseillons d'avoir à disposition un minimum de 1.5 To. 

!!! warning
  Pour des jeux de données Whole Genome avec beaucoup d'échantillons, plusieurs To peuvent être nécessaire. 

Le tableau suivant est fournit à titre indicatif pour avoir une idée de l'espace nécessaire en fonction du volume du jeu de données initiale.  
