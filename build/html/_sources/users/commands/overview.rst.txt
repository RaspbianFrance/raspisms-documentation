.. _commands:

============================================
Commandes par SMS
============================================

.. note::
    Pour des raisons de sécurité les commandes par SMS ne sont disponibles qu'en auto-hébergement. Ce service n'est pas accessible en mode SaaS.

Les commandes par SMS c'est quoi ?
==================================
Les commandes par SMS sont un moyen de déclencher l'execution de commandes système par SMS.

Cette fonctionnalité peut être utilisée par exemple pour contrôler une machine distante n'ayant pas d'accès à internet.

Créer une commande par SMS
============================================
Pour créer une commande executable par SMS vous devez dans un premier temps créer le script qui devra être lancé à la réception de la commande et le placer dans le dossier des scripts de RaspiSMS (par défaut ``/usr/share/raspisms/scripts/``).

.. warning::
    Attention, le script sera exécuté avec l'utilisateur ``root``.

Une fois le script créé, vous pouvez ajouter la commande en vous rendant dans la partie "Commandes", puis "Ajouter une commande".

Renseignez le nom de la commande, celui-ci sera utilisé dans le SMS de déclenchement. Renseignez également le nom de fichier du script à déclencher (il sera appelée depuis le chemin du dossier des scripts de RaspiSMS).

Choisissez si le niveau administrateur est requis pour exécuter la commande. Si le niveau administrateur est requis la commande ne pourra être lancée que si le compte RaspiSMS est administrateur.

Il ne vous reste plus qu'à valider en cliquant sur "Enregistrer la commande".


Déclencher une commande
=======================
Pour déclencher une commande précedemment créée vous devez envoyer un SMS dont le corps est un message au format JSON tel que décrit ci-dessous.

.. list-table:: Description du SMS de déclenchement d'une commande
   :header-rows: 1

   * - Clé
     - Description

   * - login
     - Login du compte auquel la commande est associée.

   * - password
     - Mot de passe du compte auquel la commande est associée.

   * - command
     - Nom de la commande a déclencher.

   * - args
     - Les arguments à passer au script lancé.


Exemple de commande
===================

.. warning::
     Pour des raisons de sécurité, votre mot de passe sera supprimé du SMS reçu lors de son enregistrement en base de données. Vous devriez cependant penser à supprimer le SMS envoyé de votre téléphone. 

Pour mieux comprendre la syntaxe d'un SMS de commande, voici un exemple concret d'utilisation.

Dans cet exemple, nous allons envoyer un SMS à RaspiSMS afin de commander l'allumage ou l'extinction d'une lumière.

La gestion de la lampe se fait à travers le script ``allumerEteindreLampe.sh``, lequel est identifié dans RaspiSMS par la commande ``lampe``. Ce script peut être appelé sans paramètre, auquel cas il se contentera d'inverser la position de la lampe, ou avec un paramètre, ``--on`` ou ``--off``, permettant de définir si l'on souhaite allumer ou éteindre la lampe.

Dans cet exemple, nous allons utiliser le compte RaspiSMS ``documentation@raspisms.fr`` dont le mot de passe est ``p4ssw0rd``.

Commande avec argument
######################
Afin d'allumer la lampe via les commandes RaspiSMS, vous devrez donc envoyer le SMS suivant :

.. literalinclude:: /_code_examples/commands/argument.json
    :language: json

.. note::
     Les paramètres supplémentaires passés à la commande sont échappés via la fonction ``escapeshellcmd()`` de php. Vous pouvez donc utiliser ces paramètres pour passer des arguments comme ``-t "du texte" -l 10``, mais pas pour exécuter des opérations comme par exemple ``&& reboot``. 

Commande sans argument
######################
Afin de simplement inverser la position de la lampe (l'allumer si elle est éteinte, l'éteindre si elle est allumée), vous devrez envoyer le SMS suivant.

.. literalinclude:: /_code_examples/commands/no_argument.json
    :language: json

