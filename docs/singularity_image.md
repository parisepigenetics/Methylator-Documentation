## Singularity image

Methylator utilise une image Singularity pour s'exécuter sur les cluster HPC. 
Singularity est une technologie qui permet de construire des environnements isolé du reste de votre machine,
qui ne dependeront uniquement du noyaux linux de celle-çi. On appele ces environnements isolés des **containers**. 
Une image est une photographie, une description exaustive et statique d'un container. Elle permet le partage,
l'instanciation et l'exécution de containers. Methylator utilise cette technologie pour s'exécuter dans un 
environnement isolé contenant l'intégralité des outils (avec les bonnes versions) nécessaire à son fonctionnement. L'objectif de cette démarche est d'améliorer la reproductibilitée des analyses conduite par le workflow et d'augmenter sa pérénité. 

For more information you can look the Singularity container [documentation](https://docs.sylabs.io/guides/3.0/user-guide/quick_start.html).
