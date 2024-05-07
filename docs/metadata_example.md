
## Annotations

Au cours de l'analyse de la méthylation par Methylator, de nombreuses figures s'appuie sur l'annotation du génome. 
Pour réaliser ces figures, Methylator construit ses propres annotations à l'aide de deux fichiers : 
- un fichier au format GTF pour les annotations génomiques classiques (genes, intergéniques, TSS ...)
- un fichier au format .txt contenant des informations sur les ilôts CpG

**Schema de la partie du workflow en charge de construire les annotations**
[annotations_scheme](img/part_annotation_workflow.png)

Ces annotations sont appelés "standards annotations". 
Pour construire ces annotations Methylator s'appuie sur le package genomicrange de Bioconductor. 

Nous avons fait le choix de reconstuire les annotations plutôt que d'utiliser les bases de données d'annotations pour
éviter d'être dépendent de celle-çi mais également pour s'assurer que les annotations sont toujours construites de manière identique. 

### Construction des annotations 

**Genes**
**Intergenic**
**Introns**
**Exons**
**Promoters**
**Cpg islands**
**CpG Shores**
**CpG Shelves**
**TSS**

L'un des avantages de cette approche elle qu'elle permet à un utilisateur avertie et à l'aise avec le language de programmation R de modifier ou d'ajouter des annotations à sa convenance. 

!!! warning
    Lorsque l'on annotate les CpG, DMC/DMT/DMR avec les annotations. Chaque fois qu'un de ces éléments overlappe avec une annotation elle est associée à cette annotation. 
    De ce fait, un même CpG peut par exemple être annoté comme étant localisé dans un gène, un îlot CpG et un intron. Ce qui explique pourquoi dans les figures de compte
    des annotations certaines catégories ont beaucoup d'éléments que d'autres. C'est pour cette raison que les figures en compte relatifs sont en générales plus informatives.



### Metadata

|           sample            |      group      |
| --------------------------- | --------------- |
| SRR9016926_sub500000_chr19  |       WT        |
| SRR9016927_sub500000_chr19  |       1KO       |
| SRR9016928_sub500000_chr19  |       DKO       |
| SRR11806587_sub500000_chr19 |       WT        |
| SRR11806588_sub500000_chr19 |       1KO       |
| SRR11806589_sub500000_chr19 |       DKO       |

The "sample" column corresponds to the names of your samples, and the "group" column corresponds to the biological conditions.
Here, the name of the samples corresponds to their SRR number.
It is possible to use the SRA function of the worklfow (see [here](runing.md) )


### Metadata for custom annotation 

|    annotation_tracks   |  group |      name     | 
| ---------------------- | ------ | ------------- | 
| hg19.simplerepeat.txt  | repeat | tandem_repeat |
| hg19.nestedRepeats.txt | repeat | nested_repeat |
| hg19.atacseq.txt       |  atac  |      atac     |

This is necessary only if you select the 'customs annotations' feature in the configuration file. 
In this case, the workflow will produce, in addition to the figures linked to default annotations, 
the same figures but for user-provided personal annotations. 'Annotation_tracks' corresponds to 
the names of the files containing the annotations, 'group' to the different annotation categories, 
and 'name' to the names that will be given to the annotations in the reports.

``` yaml 
# ===== Customs Annotations ===== #
CUSTOM_ANNOT: yes # yes or no 
METAFILE_ANNOT: configs/metadata_annot.tsv
CUSTOM_ANNOT_PATH: my_bank/
MERGE_WITH_BASICS_ANNOT: yes # yes or no
```
Additionally, you need to specify the path to the folder containing the annotation files and the path to the metadata

!!! warning
    The expected file format for creating custom annotations is as follows : 
    
|  chr | start |  end  |                                 metadata                                   |
| ---- | ----- | ----- | -------------------------------------------------------------------------- |
| chr1 | 10000 | 10468 | trf	6	77.2	6	95	3	789	33	51	0	15	1.43	TAACCC          | 
| chr1 | 10627 | 10800 | trf	29	6	29	100	0	346	13	38	47	0	1.43	AGGCGCGCCGCGCCGGCGC | 
| chr1 | 10757 | 10997 | trf	76	3.2	76	95	2	434	17	30	45	6	1.73	GGCGCAGGCGCAGAGAGGC | 
| chr1 | 11225 | 11447 | trf	117	1.9	121	80	14	273	12	32	33	20	1.9	CGCCCCCTGCTGGCGAC       | 

With chromosomal coordinates (chromosomes | start | end) along with any potential supplementary columns. 










