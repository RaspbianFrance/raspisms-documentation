.. _import_contacts:

=================================
Importer et exporter des contacts
=================================

À quoi ça sert ?
=================================
L'import et l'export de contacts vous permettent de facilement créer de nombreux contacts ainsi que de sauvegarder votre répertoire, par exemple pour le transférer sur une nouvelle instance de RaspiSMS.

Il s'agit aussi d'un moyen simple d'importer une base de donnée de contacts déjà existante au sein de RaspiSMS.

Exporter des contacts
========================
Pour exporter vos contacts, rien de plus simple, il vous suffit de vous rendre sur la page de gestion de vos contacts et de cliquer sur le bouton "Exporter la liste des contacts" en haut à droite. Une fenêtre apparait alors pour vous demander le format à employer, actuellement les format "JSON" et "CSV" sont supportés.

Importer des contacts
========================
Importer des contacts n'est pas beaucoup plus compliqué, mais cela demande en revanche de préter une certaine attention au format des données afin de créer des contacts valides.

Pour importer des contacts, vous devrez donc créer un fichier selon les règles indiquées ci-dessous, puis cliquer sur le bouton "Importer une liste des contacts" en haut à droite dans la partie dédiée à la gestion des contacts, cliquer sur "Choisir le fichier" dans la popup ouverte, séléctionner le fichier précédemment créé et enfin cliquer sur "Valider".

.. note::
    Lors de l'import d'un contact, si un contact avec un nom similaire existe déjà le nouveau contact ne sera pas importé.


Format des données
####################
Afin de pouvoir importer des contacts, il important de respecter un format précis, lequel varie selon que vous souhaitez importer des contacts via un fichier "JSON" ou "CSV".

Pour la partie qui suit nous prendrons en exemple la création par importation des deux contacts suivants :

.. list-table:: Le contact nommé "John Doe", numéro de téléphone "+33 6 12 34 56 78"
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

.. list-table:: Le contact nommé "Jane Doe", numéro de téléphone "+33 6 12 34 56 79"
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


Importer un fichier CSV
~~~~~~~~~~~~~~~~~~~~~~~
Le format CSV (Comma-Separrated Value) est très utilisé pour le transfert de données avec des logiciels tableur. Dans un fichier CSV, chaque ligne du fichier correspond à une nouvelle entrée, et les valeures de chaque entrée, c'est à dire les colonnes du tabelau, sont séparées par des virgules. La première ligne quand à elle sert généralement à indiquer le nom de chaque colonne. 

Dans notre cas la première ligne servira donc à indiquer les colonnes, et les lignes suivantes seront les contacts à importer.

Il est très important de respecter un certain ordre quand aux colonnes. En effet, les deux premières colonnes doivent contenir dans l'ordre, le nom du contact à créer, puis son numéro de téléphone (au format international, ``+33XXXXXXXXX``). Les colonnes suivantes contiendront les données enrichies du contacts (voir :ref:`contacts enrichies <extended_contacts>`).

Le nom donné aux deux premières colonnes est sans importance, il sera automatiquement écrasé par RaspiSMS, par conséquent, vous pouvez très bien les nommées "0" et "1", seul impératif ce nom ne peux plus être ré-utilisé pour une autre colonne. Le nom des colonnes suivantes ne doit-être constitué que de caractères alphanumériques, tirets et underscore. Ces noms seront utilisés comme clé de la donnée enrichie correspondante du contact.

Exemple de fichier CSV
'''''''''''''''''''''''
Pour rendre cela plus clair, rien ne vaut au bon exemple. Voici donc un fichier CSV valide permettant d'importer les deux contacts d'exemples.

.. literalinclude:: /_code_examples/contacts_groups/import.csv
    :language: none

.. note::
    Vous noterez que les caractères utilisés pour la séparation sont la virgule ``,``, pour l'encadrement les doubles guillemets ``"``, et que ces derniers ne sont nécessaires que sur les chaînes comportant des espaces.

.. warning::
    Attention, lors de la lecture d'un fichier CSV avec des logiciels tableur ceux-ci ont tendance à faire disparaitre le "+" au début des numéros, lequel est requis !


Importer un fichier JSON
~~~~~~~~~~~~~~~~~~~~~~~~~
Le format JSON (JavaScript Object Notation) est un format plus récent d'avantage utilisé par les API et les systèmes informatiques. Les fichiers JSON peuvent représenter des collections ou des tableaux avec des clés nommées. Plus complexes à lire pour les humains, ils sont en revanche plus simple à analyser pour des machines et peuvent donc êtres utilisés pour facilement exporter des données de systèmes informatiques plus récents et/ou complexes que de simples tableurs ou bases de données.

Pour importer des contacts via un fichier JSON, vous devrez créer une collection de tableaux avec des clés nommées. Chaque tableau de la collection sera un contact.

Les tableaux doivent posséder les clés "name" et "number", qui contiendrons le nom à donné au contact et son numéro, toujours au format international.

Pour définir les données étendues du contact il faudra créer une clé supplémentaire nommée "data". Cette clé sera associée à un tableau dans lequel chaque donnée enrichie sera renseignée avec en clé son nom (là aussi la clé devrait-être uniquement composé de caractères alphanumériques, tirets et underscore), et en valeure associée la valeure souhaitée pour la donnée.

Exemple de fichier JSON
'''''''''''''''''''''''
Comme pour le CSV, voici un fichier JSON valide permettant d'importer les deux contacts d'exemples.

.. literalinclude:: /_code_examples/contacts_groups/import.json
    :language: json

.. note::
    Pour des raisons de lisibilités des espaces et indentations ont été ajoutés, mais ils ne sont pas nécessaires.


