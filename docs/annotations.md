
## Annotations

Au cours de l'analyse de la méthylation par Methylator, de nombreuses figures s'appuie sur l'annotation du génome. 
Pour réaliser ces figures, Methylator construit ses propres annotations à l'aide de deux fichiers :    
- un fichier au format GTF pour les annotations génomiques classiques (genes, intergéniques, TSS ...)    
- un fichier au format .txt contenant des informations sur les ilôts CpG

**Schema de la partie du workflow en charge de construire les annotations**
![annotations_scheme](img/part_annotation_workflow.png)

Ces annotations sont appelés "standards annotations". C'est le script R `Annotatr.R` . 
Pour construire ces annotations Methylator s'appuie sur les package
[annotar](https://www.bioconductor.org/packages/devel/bioc/vignettes/annotatr/inst/doc/annotatr-vignette.html), 
[GenomicRanges](https://bioconductor.org/packages/release/bioc/vignettes/GenomicRanges/inst/doc/GenomicRangesIntroduction.html)
et [GenomicFeatures](https://kasperdanielhansen.github.io/genbioconductor/html/GenomicFeatures.html). 

Nous avons fait le choix de reconstuire les annotations plutôt que d'utiliser les bases de données d'annotations pour
éviter d'être dépendent de celle-çi mais également pour s'assurer que les annotations sont toujours construites de manière identique. 

### Construction des annotations 

**Genes**    
Utilise la fonction genes de Genomicfeatures
```R
GenomicFeatures::genes(txdb)
```


**Intergenic**   
Intersection de l'ensemble des annotations avec les annotations génomiques.
Tous ce qui n'est pas annoté comme étant un gène est automatiquement annoté comme étant intergenetic. 

**Introns**    


**Exons**    
Utilise la fonction exonsBy de genomicFeatures
```R
GenomicFeatures::exonsBy(txdb, by = 'gene')
```   

**Promoters**
-2000 paires de bases en amonts du Start et + 2000 paires de bases en aval du Start.    

**TSS**
```R
tss_gr = IRanges::promoters(genic_gr, upstream = 0, downstream = 1)
GenomicRanges::mcols(tss_gr)$type = sprintf('%s_tss', ORG)
``` 
**Near TSS**
```R
near_tss_gr = IRanges::promoters(genic_gr, upstream = 5000, downstream = 500)
GenomicRanges::mcols(near_tss_gr)$type = sprintf('%s_near_tss', ORG)
``` 

![genes](img/gene.jpeg)


**Cpg islands**    

**CpG Shores**    
+ 2kb en amont et en aval des  CpG Islands

**CpG Shelves**    
+ 4kb en amont et en aval des CpG Islands


![cpg](img/cpg_annot.jpeg)


L'un des avantages de cette approche elle qu'elle permet à un utilisateur avertie et à l'aise avec le language de programmation R de modifier ou d'ajouter des annotations à sa convenance. 

!!! warning
    When annotating CpGs, DMCs, DMTs, or DMRs, whenever one of these elements overlaps with an annotation, it is associated with that annotation. As a result, the same CpG can be annotated as being located in a gene, a CpG island, and an intron simultaneously. This explains why in the annotation count figures, some categories have more elements than others (e.g., genes). That's why it's also interesting to look at the figures in relative count.

### Methylation status in absolut count
![meth_stat_abs](img/methylation_status_absolut.png)

### Methylation status in relative count
![meth_stat_rel](img/methylation_status_relative.png)



## Metadata for custom annotation 

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
Vous pouvez également choisir de merger les annotations customs aux annotations standards pour avoir un unique plot à chaque fois.

!!! warning
    The expected file format for creating custom annotations is as follows : 
    
|  chr | start |  end  |                                 metadata                                   |
| ---- | ----- | ----- | -------------------------------------------------------------------------- |
| chr1 | 10000 | 10468 | trf	6	77.2	6	95	3	789	33	51	0	15	1.43	TAACCC          | 
| chr1 | 10627 | 10800 | trf	29	6	29	100	0	346	13	38	47	0	1.43	AGGCGCGCCGCGCCGGCGC | 
| chr1 | 10757 | 10997 | trf	76	3.2	76	95	2	434	17	30	45	6	1.73	GGCGCAGGCGCAGAGAGGC | 
| chr1 | 11225 | 11447 | trf	117	1.9	121	80	14	273	12	32	33	20	1.9	CGCCCCCTGCTGGCGAC       | 

With chromosomal coordinates (chromosomes | start | end) along with any potential supplementary columns. 










