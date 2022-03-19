.. _sms_csv:

============================================
Envoyer des SMS à un fichier CSV
============================================


Comment ça fonctionne ?
========================

Lors de l'envoi d'un SMS, vous pouvez uploader fichier CSV contenant une liste de numéros auxquels sera envoyé le SMS. Pour chacun des numéros vous pouvez également définir un ensemble de données associées (voir :ref:`contacts enrichies <extended_contacts>`) qui pourront être utilisés dans le SMS via :ref:`le système de SMS dynamiques<templating>`.


Format du fichier CSV
#####################
Le format CSV (Comma-Separrated Value) est très utilisé pour le transfert de données avec des logiciels tableur. Dans un fichier CSV, chaque ligne du fichier correspond à une nouvelle entrée, et les valeures de chaque entrée, c'est à dire les colonnes du tabelau, sont séparées par des virgules. La première ligne quand à elle sert généralement à indiquer le nom de chaque colonne.

Le fichier CSV utilisé pour l'envoi des messages doit respecter un format suivant :

* La première ligne sert à indiquer le nom des colonnes, les lignes suivantes contiennent les numéros de téléphones cibles et éventuellement les données à associer, à raison d'un numéro par ligne.
* La première colonne doit contenir le numéro de téléphone (au format international, ``+33XXXXXXXXX``).
* Les colonnes suivantes peuvent contenir les données à associer au contact, ou êtres laissées vides.
* Le nom donné à la première colonne est sans importance, il sera automatiquement écrasé par RaspiSMS, par conséquent, vous pouvez très bien la nommée "0", seul impératif ce nom ne peux plus être ré-utilisé pour une autre colonne. Le nom des colonnes suivantes ne doit-être constitué que de caractères alphanumériques, tirets et underscore. Ces noms seront utilisés comme clé de la donnée associé au numéro. 

Exemple de fichier CSV
'''''''''''''''''''''''
Pour rendre cela plus clair, rien ne vaut au bon exemple. Voici donc un fichier CSV valide permettant d'envoyer un SMS aux deux numéros suivants :

.. list-table:: Le numéro de téléphone "+33 6 12 34 56 78"
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

.. list-table:: Le numéro de téléphone "+33 6 12 34 56 79"
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

.. literalinclude:: /_code_examples/sms/sms_csv.csv
    :language: none

.. note::
    Vous noterez que les caractères utilisés pour la séparation sont la virgule ``,``, pour l'encadrement les doubles guillemets ``"``, et que ces derniers ne sont nécessaires que sur les chaînes comportant des espaces.

.. warning::
    Attention, lors de la lecture d'un fichier CSV avec des logiciels tableur ceux-ci ont tendance à faire disparaitre le "+" au début des numéros, lequel est requis !

