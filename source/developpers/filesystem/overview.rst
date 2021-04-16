.. _developpers_filesystem_overview:

==============================
Système de fichier de RaspiSMS
==============================

Les fichiers de logs
========================
RaspiSMS dispose de différents fichiers de logs correspondant à différentes parties du système.

La pluspart des fichiers de log sont regroupés dans le dossier ``/var/log/raspisms/``. En plus de ces logs, RaspiSMS peut potentiellement émettre des logs dans les fichiers de logs Apache ou PHP, dont les emplacements dépendent de la configuration de votre serveur web.

.. list-table:: Fichiers de logs
   :header-rows: 1

   * - Fichier
     - Description

   * - ``/var/log/raspisms/callback.log``
     - Suivi des callback HTTP.
       
   * - ``/var/log/raspisms/daemons.log``
     - Logs des démons RaspiSMS. Cela inclus notamment les logs des téléphones, des webhook, du mailer.
       

Les fichiers de configuration
=============================

.. note::
    Pour ce paragraphe, nous considérons que nous sommes dans le dossier de RaspiSMS (par défaut ``/usr/share/raspisms``). Les chemins non absolus sont donc à considérés comme partant de ce dossier.

RaspiSMS dispose de plusieurs fichiers de configurations qui permettent de modifier certains réglages et comportements.

Ces fichiers de configurations sont des fichiers PHP contenant un tableau ``$env``. C'est ce tableau qui est transformé en constantes lors de l'éxecution, en utilisant la clé comme nom.

Les fichiers sont indiqués ci-dessous dans l'ordre où ils sont lus par le système. Chaque fois qu'un fichier est lu, les constantes sont créées avant de passer au fichier suivant (sauf pour ``env.descartes.php`` qui est lu avant que ``./descartes/env.php`` ne soit transformé en constantes, ceci afin de pouvoir écraser la configuration par défaut de descartes).

.. list-table:: Fichiers de configuration
   :header-rows: 1

   * - Fichier
     - Description

   * - ``./descartes/env.php``
     - Fichier de configuration du framework Descartes. La seule raison de toucher à ce fichier est de modifier la variable ``$http_dir_path`` pour utiliser un autre sous-dossier que ``/raspisms`` dans l'URL.

   * - ``./descartes.env.php``
     - Fichier de configuration permettant d'écraser les réglages par défaut du framework Descartes. Non utilisé par défaut dans le projet.
       
   * - ``./env.php``
     - Fichier de configuration global de RaspiSMS, il contient essentiellement des constantes nécessaires au bon fonctionnement de RaspiSMS et des réglages qui ne sont pas censés êtres modifiés par l'utilisateur.

   * - ``./env.XXX.php``
     - Si une constante ``ENV`` est définie dans un des fichiers précédents (par défaut elle est définie à ``prod`` dans ``./env.php``), RaspiSMS chargera le fichier ``./env.XXX.php`` (ou ``XXX`` est égal à la valeure de ``ENV``).
       Ce fichier est utilisé pour définir des constantes nécessaires à RaspiSMS mais qui varient d'une installation à une autre. C'est dans ce fichier que sont définis les identifiants de la base de données à employer.


Organisation de l'application
=============================
RaspiSMS est découpé en différents dossiers qui représentent différentes parties de l'application, de sa logique, et de ses données.

Nous ne reviendrons que sur les dossiers les plus importants.

.. list-table:: Principaux dossiers/fichiers de l'application
   :header-rows: 1
   
   * - Fichier
     - Description
   
   * - ``./adapters``
     - Dossier des adaptateurs logiciels pour les différentes types de téléphones.

   * - ``./assets``
     - Dossier des ressources statiques. Ce dossier doit être servi directement par le serveur web sans être interprété.

   * - ``./bin``
     - Dossier des exécutables utilisés par RaspiSMS.

   * - ``./confs``
     - Contient les fichiers de configuration pour les services externes mais nécessaires au fonctionnement de RaspiSMS (systemd et apache notamment).

   * - ``./console.php``
     - Exécutable qui permet de lancer certaines commandes de RaspiSMS en mode CLI (notamment ``controllers\internals\Console.php``).

   * - ``./data``
     - Dossier des données générées par l'application.

   * - ``./data/public``
     - Dossier des données publiques générées par l'application. Pour des raisons de sécurité ce dossier doit **absolument** être servi directement par le serveur web, **sans être interprété et sans être listable**.

   * - ``./index.php``
     - Fichier d'entrée pour toute l'application. Toutes les requêtes qui ne pointent pas vers ``./assets/*`` ou ``./data/public/*`` doivent renvoyer vers ``index.php``.
