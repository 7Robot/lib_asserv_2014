##############################
Construire cette documentation
##############################

L'objectif est d'avoir une documentation facilement maintenable
tout en étant suffisamment riche en fonctionnalités pour pouvoir
décrire l'ensemble du code. Il est donc important d'avoir
le support de fonctionnalités telles que :

* Une syntaxe simple
* Ecrire du code source
* Inclure des images
* Utiliser des mathématiques
* Une navigation simple dans la documentation

J'avais le choix entre 2 types de documentations :

#. Une documentation générée à partir de fichiers
   `Markdown <http://fr.wikipedia.org/wiki/Markdown>`_.
#. Ou une documentation générée à partir de fichiers rST
   (`reStructuredText <http://fr.wikipedia.org/wiki/ReStructuredText>`_).

J'ai opté pour la 2\ |ieme| car elle gère mieux l'affichage
des formules mathématiques (à la mode LaTeX)
et qu'elle permet d'exporter la documentation vers un pdf.

****************************
La documentation avec Sphinx
****************************

Sphinx est un outil pour générer facilement de la documentation
à base de fichier respectant la syntaxe rST (reStructuredText).

.. |ieme| replace:: :sup:`e`
