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

`Sphinx <http://sphinx-doc.org/index.html>`_ est un outil
pour générer facilement de la documentation
à base de fichiers respectant la syntaxe rST (reStructuredText).
Vous pourrez trouver une introduction à la syntaxe rST à cette adresse :
http://sphinx-doc.org/rest.html. Elle est à la fois simple et puissante !

**********************
Installation de Sphinx
**********************

Vous pouvez trouver tous les détails de l'installation pour votre configuration ici : http://sphinx-doc.org/install.html.
Pour moi ça a été extrêmement simple, j'ai utilisé le gestionnaire de paquets
de python (`pip <https://pip.pypa.io/en/latest/>`_) :

.. code-block:: bash

   $ sudo pip install sphinx

Et c'est tout !

****************************
Initialiser la documentation
****************************

Les sources de la documentation vont se trouver dans un dossier ``docs``.
On commence donc par créer ce dossier.
Ensuite il suffit d'utiliser la commande ``sphinx-quickstart``
qui permet de configurer la documentation :

.. code-block:: bash

   $ mkdir docs
   $ cd docs/
   $ sphinx-quickstart

Généralement vous pouvez laisser la configuration par défaut qui vous est proposée.
Attention, il y a quand même certaines questions pour lesquelles il faut changer la réponse :

.. code-block:: rst

   > Separate source and build directories (y/n) [n]: y
   > autodoc: automatically insert docstrings from modules (y/n) [n]: y
   > mathjax: include math, rendered in the browser by MathJax (y/n) [n]: y

A la fin du scrip Sphinx aura initialisé la configuration en générant
2 dossiers : ``build`` et ``source`` ainsi qu'un makefile
pour faciliter la génération de la doc avec une commande du type :

.. code-block:: bash
   
   $ make html

****************************************
Configuration de la documentation Sphinx
****************************************

L'utilisation du script ``sphinx-quickstart`` a généré plusieurs fichiers
permettant d'agir sur la configuration de la documentation dans le dossier ``docs/``.
Je vais détailler maintenant les quelques manip à faire pour améliorer cette configuration.

Héberger sa documentation sur Github
====================================

Si vous travaillez sur du code avec un dépot Git (j'espère !),
il y a quelques manip à faire.

Une documentation qui n'est visible par personne n'est pas très utile.
Du coup il faut héberger quelquepart la documentation générée.
Si vous hébergez votre code sur `Github <https://github.com/>`_ c'est parfait !
Github peut aussi héberger la documentation dans ce que l'on appelle
les "pages Github". Vous pouvez avoir une explication ici :
https://help.github.com/articles/what-are-github-pages/

Pour activer les pages Github de votre dépot il suffit de créer
un nouvelle branche vierge intitulée ``gh-pages`` et de la push :

.. code-block:: guess

   $ git checkout --orphan gh-pages
   $ git rm -rf .
   $ echo "My page" > index.html
   $ git add index.html
   $ git commit -am "Premier commit sur les pages Github"
   $ git push origin gh-pages -u

Configurer la compilation vers HTML
===================================

On veut pouvoir build la documentation dans la branche ``gh-pages``.
Or ca ne va pas être pratique de devoir changer constamment de branche
pour la documentation. Ce que je suggère donc c'est d'avoir un deuxième
dossier qui est un clone du même dépot mais dans lequel on reste
toujours sur la branche ``gh-pages`` :

.. code-block:: guess

   $ cd ../
   $ mkdir mondepot_gh-pages
   $ cd mondepot_gh-pages/
   $ git clone -b gh-pages --single-branch git@github.com:moi/mondepot.git .

Maintenant il faut changer la configuration pour build dans ce dossier.
On repart dans le dossier original et on modifie le fichier ``Makefile`` :

.. code-block:: guess

   BUILDDIR = ../../mondepot_gh-pages

   html:
      $(SPHINXBUILD) -b html $(ALLSPHINXOPTS) $(BUILDDIR)

Utiliser le thème rtd (readTheDocs)
===================================

C'est tout simple. Il suffit d'installer le theme via pip :

.. code-block:: guess

   $ sudo pip install sphinx_rtd_theme

Ensuite dans le fichier ``conf.py`` :

.. code-block:: guess

   import sphinx_rtd_theme
   html_theme = "sphinx_rtd_theme"
   html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]

Pour que le theme marche une fois la documentation poussée sur Github,
il faut faire une dernière petite manip :
il faut ajouter un fichier vide intitulé ``.nojekyll``
(dans la branche gh-pages) et le push (add et commit avant biensûr).
Sinon, Github va ignorer les dossiers commençant par _ dont le dossier
_static/ qui contient les styles CSS et images et donc le theme
ne marchera pas (vous aurez une vielle page html toute moche).

Configurer la compilation vers PDF (Latex)
==========================================

La compilation de la doc vers pdf (Latex) est un outils puissant
mais qui demande un petit peu de réglages.
J'ai choisi de compiler en français (babel french),
dans un dossier temporaire et de ne garder que les pdf à la fin
(supprimer directement les fichiers Latex générés automatiquement).
Pour le type de document Latex on a le choix entre 2 types :
``manual`` qui correspond à une sorte de ``report``,
ou ``howto`` plus proche du style de ``article``.
J'ai choisi ``howto`` pour avoir une documentation plus compacte.

Modifs à faire dans le fichier ``Makefile`` :

.. code-block:: guess
   
   # ajouter au début : le dossier temporaire pour la compilation Latex
   PDFBUILDDIR = build

   # nettoyage des fichiers générés
   clean:
      rm -rf $(BUILDDIR)/*
      rm -rf $(PDFBUILDDIR)

   # builds pour latex et latexpdf (compile le pdf en plus de générer le Latex)
   latex:
      $(SPHINXBUILD) -b latex $(ALLSPHINXOPTS) $(PDFBUILDDIR)/latex
      @echo "Build finished; the LaTeX files are in $(PDFBUILDDIR)/latex."
      @echo "Run \`make' in that directory to run these through (pdf)latex" \
            "(use \`make latexpdf' here to do that automatically)."

   latexpdf:
      $(SPHINXBUILD) -b latex $(ALLSPHINXOPTS) $(PDFBUILDDIR)/latex
      @echo "Running LaTeX files through pdflatex..."
      $(MAKE) -C $(PDFBUILDDIR)/latex all-pdf
      cp $(PDFBUILDDIR)/latex/*.pdf .
      rm -rf $(PDFBUILDDIR)
      @echo "pdflatex finished"

Et pour le fichier ``conf.py`` :

.. code-block:: python

   language = 'fr'

   'papersize': 'a4paper',
   'pointsize': '12pt',
   
   # The language
   'babel': '\\usepackage[french]{babel}'

   # choisir 'howto' au lieu de 'manual' si vous le souhaitez
   latex_documents = [
      ('index', 'LibAsserv2014.tex', 'LibAsserv 2014 Documentation',
      'Matthieu Pizenberg', 'howto'),
   ]


.. |ieme| replace:: :sup:`e`
