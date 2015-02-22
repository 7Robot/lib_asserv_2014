.. LibAsserv 2014 documentation master file, created by
   sphinx-quickstart on Sat Feb 21 17:39:08 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

################################
Documentation de lib_asserv_2014
################################

Bienvenu sur documentation de la bibliothèque utilisée pour
l'asservissement du robot Fatman pour la coupe de France de robotique 2014.

Vous trouverez le code source sur le dépot Github suivant :
https://github.com/7Robot/lib_asserv_2014

Vous trouverez la version pdf de cette documentation ici :
https://github.com/7Robot/lib_asserv_2014/raw/documentation/docs/LibAsserv2014.pdf
(elle n'est pas nécessairement à jour par rapport à la version web).

.. toctree::
   :maxdepth: 3
   :hidden:

   user
   motion
   asserv
   odo
   pid
   docu

*********************
Plateforme à asservir
*********************

Cette bibliothèque répond à un besoin précis :
**pouvoir contrôler le déplacement d'un robot, en vitesse ou en position**.
Ce robot doit être assimilable à une plateforme différentielle,
considérée comme holonome.

Plateforme différentielle
   Une plateforme différentielle est constituée de 2 roues commandées
   indépendamment ce qui permet souvent de la considérer holonome.

Plateforme holonome
   Pour faire simple, on peut dire qu'une plateforme est dite holonome
   si elle peut se déplacer dans toutes les directions depuis
   sa position courante.

*****************************
Objectifs de l'asservissement
*****************************

Le robot est utilisé pour participer à la `coupe de France de robotique 2014
<http://www.eurobot.org/>`_.
Il doit pouvoir se déplacer de manière autonome sur un plateaux
au dimensions suivantes : 2m x 3m.
Il n'est pas seul sur le plateaux, il peut y avoir jusque 4 robots.
Il y a aussi des éléments de jeu fixe et mobiles sur le plateau.

Compte tenu de ces conditions, il est inutile de chercher la vitesse.
Par contre il peut être très intéressant d'avoir des bonnes capacités
d'accélération et de frein pour naviguer le plus efficacement possible.

La limite de l'\ **intelligence** de l'asservissement est par contre assez
difficile à définir. Il peut être intéressant de le rendre autonome
(évitement des obstacles intégré par exemple) mais plus complexe.
Mais a contrario il peut être aussi plus souple de garder le contrôle
global de la trajectoire par un composant plus au niveau du robot
(IA sur Raspberry Pi par exemple) et de ne contrôler avec cette
bibliothèque d'asservissement que les déplacement de **bas niveau**.

J'ai opté pour ce 2\ |ieme| choix aussi parce que ça simplifie
la bibliothèque d'asservissement.

**************************************************
Implémentation de la bibliothèque d'asservissement
**************************************************

L'objectif étant de faire une bibliothèque réutilisable et indépendante
de la configuration du robot, on a choisi de la développer en C pur.
Ceci nous permet de l'utiliser avec n'importe qu'elle plateforme
de programmation supportant la compilation de langage C.

A `7Robot <http://7robot.fr>`_, nous utilisons une plateforme basée sur des
microcontroleurs PIC (Microchip) pour contrôler les moteurs du robot.
Nous utilisons le logiciel
`MPLAB X <http://www.microchip.com/pagehandler/en-us/family/mplabx/>`_
avec des PICkit pour compiler notre code et le charger sur des dsPIC33f.

.. |ieme| replace:: :sup:`e`
