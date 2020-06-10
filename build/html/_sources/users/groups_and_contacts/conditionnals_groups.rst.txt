.. _conditionnals_groups:

============================================
Groupes conditionnels
============================================

Les groupes conditionnels c'est quoi ?
=======================================
Les groupes conditionnels sont un moyen de créer de regrouper des contacts en utilisant :ref:`les données des contacts enrichis <extended_contacts>`. C'est donc un moyen de créer des groupes dont la liste des membres n'est pas fixe mais calculée dynamiquement selon les critères de votre choix. C'est un excellent moyen de faire des listes de diffusions dynamiques.

Lors de l'envoi d'un SMS les groupes conditionnels s'utilisent de la même façon que les groupes classiques.


Activer/désactiver le support des groupes conditionnels
=======================================================
Le support des groupes conditionnels peut être activé/désactivé. Pour cela, rendez-vous dans la partie "Réglages" de RaspiSMS, modifiez la valeure dans le champs "Support des groupes conditionnels" et cliquez sur "Mettre à jour les données".


Comment ça fonctionne ?
========================
Lors de la création d'un groupe conditionnel, au lieu d'écrire la liste des contacts faisant parti du groupe, vous écrivez une règle qui sera évaluée pour chaque contact avec les données de celui-ci. Si la règle est évaluée comme ``VRAIE``, alors le contact est inclus dans le groupe, s'il elle est évaluée comme ``FAUSSE``, le groupe n'est pas inclus.

Au moment de l'envoi d'un SMS ayant parmis ses cibles un groupe conditionnel, la liste de tous les contacts est récupérée et la règle du groupe conditionnelle est évaluée pour chacun.

.. note::
    Lors de la création d'un groupe conditionnel vous pouvez prévisualiser la liste des membres du groupes à l'instant T en cliquant sur le bouton "Prévisualiser les contacts" en bas à droite.

Les bases des règles
=======================

.. note::
    N'hésitez pas à tester vous même les exemplees qui suivent pour pleinement prendre en main les groupes conditionnels. Utilisez la fonctionnalitée de prévisualisation pour voir la liste des contacts dans votre groupe.

Le système de règle utilisé par les groupes conditionnel est une sorte de langage de programmation, assez proche dans l'idée :ref:`du système de templating <templating>`. Il s'agit en fait d'une "expression booléenne", comme lors de l'utilisation d'un ``IF`` en programmation.


Nos contacts d'exemple
'''''''''''''''''''''''
Pour la suite de ce tutoriel nous allons considérer que nous possédons deux contact, "John Doe" et "Jane Doe" avec les données enrichies suivantes :

.. list-table:: Données de John Doe
   :header-rows: 1

   * - Nom de la donnée
     - Valeure de la donnée

   * - lastname
     - Doe

   * - firstname
     - John

   * - birthdate
     - 1990-10-22

   * - gender
     - male

   * - vip
     - 1

.. list-table:: Données de Jane Doe
   :header-rows: 1

   * - Nom de la donnée
     - Valeure de la donnée

   * - lastname
     - Doe

   * - firstname
     - Jane

   * - birthdate
     - 1992-08-15

   * - gender
     - female

   * - vip
     - 0


Notre première règle
''''''''''''''''''''''''''''''''''''''''''''
Pour commencer, nous allons créer une règle très simple qui sera toujours ``VRAIE``. À quoi une telle règle peut bien servir ? Et bien par exemple à faire un groupe qui contient **tous** vos contacts. En effet, puisque la règle sera toujours ``VRAIE``, à chaque contact testé celui-ci sera considéré comme devant faire parti du groupe. C'est donc un moyen simple d'envoyer un message à tous vos contacts sans avoir à mettre le groupe à jour à chaque ajout d'un nouveau contact.

Pour écrire une règle, on fait en fait une comparaison entre deux "opérandes" et un "opérateur", selon la forme suivante ``opérateur_gauche opérateur opérande_droite``. Il existe plusieurs opérateurs que nous verrons plus tard, mais pour commencer regardons l'opérateur d'égalité, celui-ci s'écrit ``==`` et reviens à dire ``EST ÉGAL À``.

Par conséquent, nous pouvons écrire une règle (ou "condition") qui sera **toujours** ``VRAIE`` comme ceci :

.. literalinclude:: /_code_examples/conditionnals_groups/true.php
    :language: php

Si vous cliquez sur la fonctionnalité de pré-visualisation vous verrez que tous nos contacts sont inclus dans le groupe.

Utiliser les données des contacts
''''''''''''''''''''''''''''''''''
C'est bien gentil tout ça, mais comment peut-on utiliser ces règles pour modifier le comportement en fonction des données d'un contact ? Et bien tout simplement en comparant non pas deux opérateurs fixes comme précédemment, mais un opérateur fixe et une variable contenant la donnée du contact qui nous intéresse.

Pour accéder à une donnée d'un contact il nous suffit d'utiliser la notation ``contact.nom_de_la_donnée``. Si nous prenons nos contact d'exemple, nous pouvons par exemple accéder à leur sexe en utilisant la notation ``contact.gender``.

Faire un groupe avec tous les contacts femmes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nous allons créer un groupe qui contiendra tous les contacts dont la donnée ``gender`` est égale à ``female``. Pour cela il y a encore une chose que vous devez apprendre, la comparaison avec des chaînes de caractères. En effet, jusqu'à présent nous avons comparé des chiffres (``1`` et ``1`` entre eux), mais les données d'un contact sont **toujours** des chaînes de caractères.

Pour faire une comparaison avec du texte, vous devrez entourer celui-ci avec des guillemets simples (``'``), ou doubles (``"``). On parle alors d'une chaîne de caractère. Sans cela, le moteur de règle ne pourrais pas savoir si vous parlez d'une variable (par exemple ``contact.gender``) ou d'un texte !
    
Dans notre cas, nous allons vouloir comparer le sexe de notre contact avec la chaîne ``'female'``. 

.. warning::
    Attention, les chaînes de caractères sont "sensibles à la casse", donc ``'female'`` est différent de ``'FEMALE'``.

.. note::
    Et si vous devez écrire un guillemet simple, par exemple pour faire une apostrophe, au milieu d'une chaîne délimitée par des guillemets simples, comment faire ? Et bien il vous suffit de **"l'échapper"**, c'est à dire indiquer qu'il s'agit d'un simple caractère et pas d'un caractère spéciale. Pour cela il vous suffit d'écrire un ``\`` devant le ``'``. Par example, pour écrire ``aujourd'hui``, vous écrirez ``'aujourd\'hui'``.

    Le fonctionnement est le même pour les guillemets doubles.

    Vous devrez d'ailleurs faire pareil pour écrire un ``\``, vous devrez alors écrire ``\\``.

Pour créer un groupe avec tous les contacts femmes, nous pouvons donc écrire.
 
.. literalinclude:: /_code_examples/conditionnals_groups/female.php
    :language: php


Comme vous pouvez le constatez en utilisant la fonction de prévisualisation, cette fois seule "Jane Doe" est dans le groupe.

Utiliser d'autres opérateurs et combiner des conditions
========================================================
Jusqu'à présent nous avons seulement vu l'opérateur d'égalité, mais ce n'est pas le seul. En effet, non seulement il existe d'autres opérateurs de comparaison, mais il existe aussi des opérateurs "logiques" qui permettent de combiner plusieurs conditions.

Les opérateurs de comparaison et mathématiques
''''''''''''''''''''''''''''''''''''''''''''''''
En plus de l'opérateur d'égalité, des opérateurs d'inégalité, d'infériorité, etc. sont utilisables, ainsi que des opérateurs mathématiques.

.. list-table:: Les opérateurs de comparaison
   :header-rows: 1
   :widths: 10 90

   * - Opérateur
     - Effets

   * - ``==``
     - Vrai si les deux opérandes ont la même valeure.

   * - ``!=``
     - Vrai si les deux opérandes n'ont pas la même valeure.

   * - ``<``
     - Vrai si la valeure de l'opérande gauche est plus petite que celle de l'opérande droite.

   * - ``>``
     - Vrai si la valeure de l'opérande gauche est plus grande que celle de l'opérande droite.

   * - ``<=``
     - Vrai si la valeure de l'opérande gauche est plus petite ou égale à celle de l'opérande droite.

   * - ``>=``
     - Vrai si la valeure de l'opérande gauche est plus grande ou égale que celle de l'opérande droite.


En voyons ces opérateurs, on se rend par exemple compte que l'on aurait pu écrire d'autres forme de notre première règle toujours vraie, comme ``1 < 10`` ou encore ``'vraie' != 'faux'``.

En plus des opérateurs de comparaison, les opérateurs mathématiques classiques sont aussi utilisables.

.. list-table:: Les opérateurs mathématiques
   :header-rows: 1
   :widths: 10 90

   * - Opérateur
     - Effets

   * - ``+``
     - Addition.

   * - ``-``
     - Soustraction.

   * - ``*``
     - Multiplication.

   * - ``/``
     - Division.

   * - ``**``
     - Puissance.
   
   * - ``%``
     - Modulo.


Avec ces opérateurs on pourrait encore reprendre la règle précédente comme suit, ``1 < 5*2`` ou ``4 == 2*2``.


Combiner plusieurs conditions
''''''''''''''''''''''''''''''''''''''''''''''''
Pour aller plus loin que la simple comparaison, vous pouvez combiner plusieurs conditions en utilisant les opérateurs logiques, en faisant ``condition opérateur condition``.

.. list-table:: Les opérateurs logiques 
   :header-rows: 1
   :widths: 10 90

   * - Opérateur
     - Effets

   * - ``&&``
     - Vrai si les deux opérations sont vraies

   * - ``||``
     - Vrai si au moins une des deux opérations est vraie
       
   * - ``!``
     - Cas particulier, elle s'utilise avec une seule condition et devant, par exemple ``!(1==1)``. Inverse la condition, si vrai -> faux, si faux -> vrai.


.. note::
    Comme vous le voyez avec le dernier cas, on peux aussi utiliser les parenthèses, ``(`` et ``)``, comme en mathématique, pour regrouper des expressions.

Nous pourrions donc choisir de faire un groupe avec seulement les femmes qui sont VIP (ce groupe sera vide puisque "Jane Doe" n'est pas VIP).
 
.. literalinclude:: /_code_examples/conditionnals_groups/female_vip.php
    :language: php

.. note::
    Pour la suite de ce tutoriel, nous n'incluerons pas cette vérification à chaque fois pour des raisons de lisibilité, mais nous vous encourageons vivement à l'utiliser systématiquement !


Aller plus loin avec les groupes conditionnels
================================================
Le moteur est un outil très puissant et il s'agit techniquement d'un véritable petit langage de programmation.

.. note::
    Pour proposer autant de fonctionnalité, RaspiSMS utilise le moteur règle "ExpressionLanguage" du projet Symfony (version 5.x). Si vous souhaitez aller encore plus loin avec le moteur de règle, nous vous invitons à vous reporter à `la documentation du composant ExpressionLanguage de Symfony`_. Vous y trouverez de nombreuses informations.


Utiliser des fonctions
'''''''''''''''''''''''''''''''''''''
En plus des opérateurs de base, certaines fonctionnalités supplémentaires sont accessibles à travers les "fonctions". Pour utiliser une fonction on utilise la notation ``nom_fonction(parametre1, parametre2...)``. Les fonctions peuvent retourner des chaînes de caractères, des chiffres ou même ``VRAIE`` ou ``FAUX``.

Nous pourrions par exemple utiliser la fonction ``upper`` sur la donnée ``lastname`` de notre contact pour la mettre en majuscule, afin de supprimer les risques d'erreures liées à la casse.

.. literalinclude:: /_code_examples/conditionnals_groups/upper.php
    :language: php

Vous pouvez tentez de modifier la casse de ``'DOE'``, par exemple en ``'DoE'`` et constater qu'après modification des contacts ne sont pas inclus. À l'inverse, modifier la casse de la donnée ``lastname`` d'un des contacts n'aura aucun effet. C'est un bon moyen de vous prémunir contre les erreures de casse lors de la saisie des données.

Liste des fonctions disponibles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Seules les fonctions suivantes sont disponibles au sein de RaspiSMS.

.. list-table:: Liste des fonctions disponibles dans RaspiSMS
   :header-rows: 1

   * - Nom dans RaspiSMS
     - Équivalent PHP

   * - exists
     - !is_null

   * - lower
     - mb_strtolower

   * - upper
     - mb_strtoupper

   * - substr
     - mb_substr

   * - strlen
     - mb_strlen

   * - abs
     - abs

   * - date
     - strtotime

Pour en apprendre plus sur chacun de ces fonctions, consultez `la page correspondante sur la documentation PHP`_. 


Vérifier si les données du contact existent
'''''''''''''''''''''''''''''''''''''''''''
Comme les données du contact sont entièrement définies par l'utilisateur et ne sont pas obligatoires, elles peuvent varier d'un utilisateur à l'autre. Cela peut parfois créer des comportement imprévu, pour cette raison nous vous conseillons de toujours vérifier si les données que vous utilisez sont disponibles. Pour cela, il vous suffit de commencer votre condition par une conditon de la forme ``(exists(contact.var1) && exists(contact.var2)) && (votre_vraie_condition)``.

Dans notre cas, nous devrions donc modifier notre dernier exemple comme ceci.

.. literalinclude:: /_code_examples/conditionnals_groups/upper_exists.php
    :language: php

Si une variable n'est pas définie, alors la condition retournera ``FAUX`` et le contact ne sera pas inclus.

.. note::
    Pour des raisons de lisibilité nous n'avons pas inclus cette vérification dans tous nos exemples, mais vous êtes fortement encouragés à toujours vérifier la présence des données.

.. _la documentation du composant ExpressionLanguage de Symfony: https://symfony.com/doc/current/components/expression_language.html
.. _la page correspondante sur la documentation PHP: https://www.php.net/manual/fr/intro-whatis.php
