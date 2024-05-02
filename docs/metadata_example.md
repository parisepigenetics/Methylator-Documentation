### Metadata

|           sample            |      group      |
| --------------------------- | --------------- |
| SRR9016926_sub500000_chr19  |       WT        |
| SRR9016927_sub500000_chr19  |       1KO       |
| SRR9016928_sub500000_chr19  |       DKO       |
| SRR11806587_sub500000_chr19 |       WT        |
| SRR11806588_sub500000_chr19 |       1KO       |
| SRR11806589_sub500000_chr19 |       DKO       |

Ici le nom des échantilons correspond à leur numéro SRR.
Il est possible d'utiliser la fonctionalité SRA du worklfow (voir ...) 


### Metadata for custom annotation 

|    annotation_tracks   |  group |      name     | 
| ---------------------- | ------ | ------------- | 
| hg19.simplerepeat.txt  | repeat | tandem_repeat |
| hg19.nestedRepeats.txt | repeat | nested_repeat |
| hg19.atacseq.txt       |  atac  |      atac     |

Ce est nécessaire uniquement si vous sélectionner la fonctionnalité "customs annotations" 
dans le fichier de configuration. Dans ce cas le worklfow produira en plus des figures liées
aux annotations par défaults les mêmes figures mais pour des annotations personnelles fournis 
par l'utilisateur. `annotation_tracks`correspond aux noms des fichiers contenant les annotations, 
`group` aux différentes catégories d'annotations et `name` aux noms qui sera données aux annotations dans les rapports. 

``` yaml 
# ===== Customs Annotations ===== #
CUSTOM_ANNOT: yes # yes or no 
METAFILE_ANNOT: configs/metadata_annot.tsv
CUSTOM_ANNOT_PATH: my_bank/
MERGE_WITH_BASICS_ANNOT: yes # yes or no
```
Il faut également indiquer le chemin vers le dossier contenant les fichiers d'annotations et le chemin vers le metadata. 

Attention , le format des fichiers attendu pour réaliser les annotations customs est le suivant :

|  chr | start |  end  |                                 metadata                                   |
| ---- | ----- | ----- | -------------------------------------------------------------------------- |
| chr1 | 10000 | 10468 | trf	6	77.2	6	95	3	789	33	51	0	15	1.43	TAACCC                        | 
| chr1 | 10627 | 10800 | trf	29	6	29	100	0	346	13	38	47	0	1.43	AGGCGCGCCGCGCCGGCGCAGGCGCAGAG | 
| chr1 | 10757 | 10997 | trf	76	3.2	76	95	2	434	17	30	45	6	1.73	GGCGCAGGCGCAGAGAGGCGCGCC    | 
| chr1 | 11225 | 11447 | trf	117	1.9	121	80	14	273	12	32	33	20	1.9	CGCCCCCTGCTGGCGAC         | 

Avec toujours les coordonnées chromosmiques (chromsomes | start | end) plus d'éventuelles métadata. 










