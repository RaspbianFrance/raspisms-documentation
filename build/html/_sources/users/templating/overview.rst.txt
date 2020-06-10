.. _templating:

============================================
SMS dynamiques et templating
============================================

Les SMS dynamiques c'est quoi ?
=================================
Les SMS dynamiques vous permettent d'écrire un SMS dont le contenu sera adapté à chaque contact en fonction de ses données. Cela peut être exploité dans le cadre d'une campagne de communication pour envoyer un SMS personnalisé à chaque utilisateur.

Les SMS dynamiques exploitent les données des contacts enrichis, voir :ref:`la documentation des contacts enrichis <extended_contacts>`.

.. warning::
    Seul les contacts et les groupes peuvent êtres utilisés avec les SMS dynamiques. L'envoi d'un SMS dynamique à un numéro direct fonctionne, mais aucune donnée ne sera insérée, ce qui risque de créer des messages imprévisibles.
    

Activer/désactiver le support du templating
============================================
Le support du templating peut être activé/désactivé. Pour cela, rendez-vous dans la partie "Réglages" de RaspiSMS, modifiez la valeure dans le champs "Support du templating" et cliquez sur "Mettre à jour les données".

Si vous désactivez le support du templating les messages seront considérés comme de simple chaînes de texte et aucune information ne sera remplacée.


Comment ça fonctionne ?
========================
Lors de la rédaction de votre SMS vous pouvez utilisez une notation particulière pour indiquer à RaspiSMS que certaines parties doivent êtres remplacées par une valeure issue des données du contact destinataire du message.

Au moment de l'envoi du SMS, RaspiSMS analysera le message à la recherche de ces notations et les remplacera par les données du contact.

.. note::
    Vous devriez toujours vérifier que votre template fonctionne comme prévu avant d'envoyer votre message. Pour cela, avant d'envoyer votre message, séléctionner un des contacts cible dans la liste déroulante "Prévisualiser pour : " en bas à droite du message, et cliquez sur le bouton "Prévisualiser" à droite. Le message qui sera envoyé sera alors affiché dans un pop-in.

Les bases du templating
=======================

.. note::
    N'hésitez pas à tester vous même les exemplees qui suivent pour pleinement prendre en main le templating. Utilisez la fonctionnalitée de prévisualisation de l'envoi de SMS pour voir le message généré.


Le système de templating s'apparente à un langage de programmation simplifié et permet d'aller assez loin techniquement. Pour commencer, nous allons voir comment insérer les données d'un contact à l'intérieur d'un SMS pour créer un message personnalisé.

Notre contact d'exemple
'''''''''''''''''''''''
Pour la suite de ce tutoriel nous allons considérer que nous envoyons un message à un contact nommé "John Doe" et qui possède les données enrichies suivantes :

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



Insérer les données du contact dans le SMS
''''''''''''''''''''''''''''''''''''''''''''
Pour commencer, nous allons créer un SMS qui affiche le texte ``Bonjour John Doe, comment allez-vous aujourd'hui ?``, en utilisant les données ``lastname`` et ``firstname`` plutôt que d'écrire le nom directement.

Toutes les données d'un contact sont accessibles sous le nom ``contact.nom_de_la_donnée``. Pour afficher une variable on l'entour de ``{{`` et ``}}``. Pour afficher cette donnée vous devez donc écrire ``{{contact.nom_de_la_donnée}}`` là où vous souhaitez afficher la donnée.

Si nous voulons écrire le message précedemment vu, nous devrons donc écrire le SMS suivant :

.. literalinclude:: /_code_examples/sms_templating/base.twig
    :language: twig


Comme vous pouvez le constatez en utilisant la fonction de prévisualisation, les variables ``{{contact.firstname}}`` et ``{{contact.lastname}}`` ont été remplacées par ``John`` et ``Doe``.

Modifier le contenu en utilisant des conditions
''''''''''''''''''''''''''''''''''''''''''''''''
Pour l'instant nous avons vu comment insérer des données à l'intérieur d'un message, mais il est possible d'aller plus loin. Par exemple, imaginons que nous voulions écrire ``M.`` ou ``Mme.`` avant le prénom du contact, selon qu'il s'agisse d'un homme ou d'une femme. Dans notre exemple le sexe du contact est stocké dans la donnée ``gender``. Seulement, elle stock ``male`` ou ``female`` plutôt que la civilité. Il nous faudrait donc un moyen de modifier le contenu du message **selon** la valeure d'une donnée, plutôt que de simplement afficher cette donnée.

Pour cela nous allons utiliser des conditions (on parle de **"structure de contrôle"**). Il s'agit d'une notation particulière qui va indiquer à RaspiSMS qu'il doit afficher un message ou un autre selon une condition.

Pour utiliser une structure, on utilise son nom et une condition, c'est-à-dire une comparaison qui peut être vraie ou fausse. Ceci est fait en utilisant la notation ``{% nom_structure condition %}``.


Les structures de contrôle
~~~~~~~~~~~~~~~~~~~~~~~~~~
Il existe trois structures principales, ``if``, ``else`` et ``elseif``, qui correspondent à ``SI``, ``SINON`` et ``SINON SI``. ``if`` s'utilise toujours en premier, ``else`` toujours en dernier, et ``elseif`` forcément après ``if`` et avant ``else`` si ``else`` est utilisé. Enfin, pour indiquer que nous avons atteind la fin du texte conditionnel, on écrit ``{% endif %}`` !

Les opérateurs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Pour exprimer la condition on dispose d'opérateurs permettant d'effectuer des comparaisons. On compare alors deux opérandes, comme ceci ``opérande_1 opérateur opérande_2``. Voici la liste des principaux opérateurs de comparaison.

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


Vous pouvez également combiner plusieurs conditions en utilisant les opérateurs logiques, en faisant ``condition opérateur condition``.

.. list-table:: Les opérateurs logiques 
   :header-rows: 1
   :widths: 10 90

   * - Opérateur
     - Effets

   * - ``and``
     - Vrai si les deux opérations sont vraies

   * - ``or``
     - Vrai si au moins une des deux opérations est vraie
       
   * - ``not``
     - Cas particulier, elle s'utilise avec une seule condition et devant, par exemple ``not 1==1``. Inverse la condition, si vrai -> faux, si faux -> vrai.



Comparer à une chaîne de caractères
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Il est possible de comparer à la fois à des chiffres et à du texte. Cependant, si vous voulez faire une comparaison avec du texte, vous devrez entourer celui-ci avec des guillemets simples (``'``). On parle alors d'une chaîne de caractère.


Par exemple, si vous voulez vérifier si le sexe de l'utilisateur est ``male``, vous utiliserez la condition ``contact.gender == 'male'``.

.. note::
    Et si vous devez écrire un guillement simple, par exemple pour faire une apostrophe, comment faire ? Et bien il vous suffit de **"l'échapper"**, c'est à dire indiquer qu'il s'agit d'un simple caractère et pas d'un caractère spéciale. Pour cela il vous suffit d'écrire un ``\`` devant le ``'``. Par example, pour écrire ``aujourd'hui``, vous écrirez ``'aujourd\'hui'``. 

    Vous devrez d'ailleurs faire pareil pour écrire un ``\``, vous devrez alors écrire ``\\``.


Notre SMS dynamique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
C'est un peu dense, mais vous devriez commencer à comprendre comment nous pouvons faire varier notre SMS selon le sexe du contact. Puisque nous souhaitons modifier notre SMS selon la variable ``contact.gender``, nous pouvons écrire le message suivant :

.. literalinclude:: /_code_examples/sms_templating/condition_if_else.twig
    :language: twig

En utilisant la prévisualisation vous pouvez constater que vous obtenez bien le message ``Bonjour M. John Doe, comment allez-vous aujourd'hui ?``.

Améliorons notre message
~~~~~~~~~~~~~~~~~~~~~~~~
Nous pouvons allez encore plus loin, par exemple pour gérer le cas où nous ne connaitrions pas le sexe de l'utilisateur.

.. literalinclude:: /_code_examples/sms_templating/condition_if_elseif_else.twig
    :language: twig

Autre piste d'amélioration, plutôt que de ré-écrire le message complet pour chaque condition, nous pourrions simplement changer la civilité.

.. literalinclude:: /_code_examples/sms_templating/condition_if_elseif_else_inline.twig
    :language: twig


Vérifier si les données du contact existent
'''''''''''''''''''''''''''''''''''''''''''
Comme les données du contact sont entièrement définies par l'utilisateur et ne sont pas obligatoires, elles peuvent varier d'un utilisateur à l'autre. Hélas, si un message est envoyé à un utilisateur et utilise des données qui n'existent pas chez cet utilisateur, cela risque d'entrainer des comportement incohérents, notamment des messages avec des variables remplacées par des chaines vides.

Pour éviter ce genre de comportement, une bonne solution est de vérifier en amont si toutes les variables utilisées sont disponibles. Pour cela, il vous suffit d'entourer votre message d'un block ``if`` vérifiant que les variables sont définies avec ``is defined``.

Dans notre cas, nous devrions donc modifier notre message comme ceci.


.. literalinclude:: /_code_examples/sms_templating/condition_if_elseif_else_inline_defined.twig
    :language: twig

Si une variable n'est pas définie, alors le SMS sera vide et ne sera pas envoyé !

.. note::
    Pour la suite de ce tutoriel, nous n'incluerons pas cette vérification à chaque fois pour des raisons de lisibilité, mais nous vous encourageons vivement à l'utiliser systématiquement !


Aller plus loin avec le templating
==================================
Le templating est un outil très puissant qui vous permet d'aller très loin, il s'agit en fait d'un véritable petit langage de programmation.

.. note::
    Pour proposer autant de fonctionnalité, RaspiSMS utilise le moteur de templating libre Twig (version 3.x). Si vous souhaitez aller encore plus loin avec le moteur de templating de RaspiSMS, nous vous invitons à vous reporter à `la documentation de Twig`_. Vous y trouverez de nombreuses informations.


Modifier les données avec les filtres
'''''''''''''''''''''''''''''''''''''
Parfois, on doit modifier les données à afficher, par exemple pour modifier leur format. Pour cela on peut utiliser les **"filtres"**. Il s'agit d'un moyen simple de modifier une donnée.

Pour utiliser un filtre, il suffit d'écrire ``votre_donnée|nom_du_filte``. Certains filtres peuvent également prendre des arguments qui modifient leur comportement. Dans ce cas on utilise la notation ``votre_donnée|nom_du_filtre(argument1, argument2, ...)``.

Un exemple d'utilisation de filtre pourrait être d'écrire le nom de l'utilisateur entièrement en majuscule. Ça tombe bien, un filtre existe pour ça, ``upper``. Il nous suffit donc de modifier le message précedent en :

.. literalinclude:: /_code_examples/sms_templating/condition_if_elseif_else_inline_upper.twig
    :language: twig

On pourrait aussi utiliser les filtres pour vérifier si c'est l'anniversaire de notre contact :

.. literalinclude:: /_code_examples/sms_templating/filter_birthdate.twig
    :language: twig



Utiliser un filtre sur un bloc de texte
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Il est même possible d'appliquer un filtre à tout un bloc de texte plutôt qu'à une donnée. Pour cela vous devrez utiliser la structure ``{% apply filtre %}{% endapply %}``. Par exemple, pour mettre tout le SMS précédent en majuscule :

.. literalinclude:: /_code_examples/sms_templating/condition_if_elseif_else_inline_full_upper.twig
    :language: twig


Liste des filtres disponibles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
De nombreux filtres sont disponibles au sein de RaspiSMS.

.. list-table:: Liste des filtres disponibles dans RaspiSMS

   * - abs
     - capitalize
     - country_name
     - currency_name
     - currency_symbol
   
   * - date
     - date_modify
     - default
     - escape
     - first
   
   * - format
     - format_currency
     - format_datetime
     - format_number
     - join
       
   * - json_encode
     - keys
     - language_name
     - last
     - length

   * - locale_name
     - lower
     - number_format
     - replace
     - reverse

   * - round
     - slice
     - sort
     - spaceless
     - split

   * - timezone_name
     - title
     - trim
     - upper
     - url_encode


Pour en apprendre plus sur chacun de ces filtres, consultez `la page des filtres de la documentation de Twig`_. 

Pour en apprendre plus sur les filtres de façon générale, repportez vous à `cette page de la documentation Twig <https://twig.symfony.com/doc/3.x/templates.html#filters>`_.


Générer des données avec fonctions
'''''''''''''''''''''''''''''''''''''
En plus de permettre la modification des données avec les filtres, le moteur de templating vous permet de générer des données avec les fonctions. Par exemple, nous pouvons utiliser les fonctions ``random`` et ``range`` avec l'opérateur de boucle ``for`` afin de générer un message avec un identifiant aléatoire que l'utilisateur devra présenter lors d'un rendez-vous.

.. literalinclude:: /_code_examples/sms_templating/function_random_id.twig
    :language: twig

Liste des fonctions disponibles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Seules quelques fonctions sont disponibles au sein de RaspiSMS.

.. list-table:: Liste des fonctions disponibles dans RaspiSMS

   * - date
     - max
     - min
     - random
     - range

Pour en apprendre plus sur chacun de ces fonctions, consultez `la page des fonctions de la documentation de Twig`_. 

.. _la documentation de Twig: https://twig.symfony.com/doc/3.x/templates.html
.. _la page des filtres de la documentation de Twig: https://twig.symfony.com/doc/3.x/filters/index.html
.. _la page des fonctions de la documentation de Twig: https://twig.symfony.com/doc/3.x/functions/index.html
