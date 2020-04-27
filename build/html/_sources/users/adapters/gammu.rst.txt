=============================
Adaptateur Gammu
=============================
.. warning::
    L'adaptateur Gammu n'est disponible qu'en mode auto-hébergement car il nécessite un équipement physique relié à la machine.

L'adaptateur Gammu permet l'utilisation d'un modem GSM et d'une carte SIM branchée physiquement au serveur hébergeant RaspiSMS, en se basant sur l'utilisation du logiciel Gammu.

Cette méthode permet d'utiliser un forfait téléphonique classique avec RaspiSMS et vous permet d'envoyer et de recevoir des messages.

Fonctionnalités supportées
--------------------------
================ =========
 Fonctionnalité   Support
================ =========
Envoi de SMS     Oui
Reception de SMS Oui
Suivi de l'envoi Non
SMS Flash        Non
================ =========



1. Configurer Gammu
-------------------------------
Pour pouvoir utiliser l'adaptateur Gammu, les logiciels ``gammu`` et ``python3-gammu`` doivent êtres installés sur le serveur. Vous devrez également avoir un modem GSM avec une carte SIM reliée au serveur.

1.1 Trouver un modem GSM compatible
'''''''''''''''''''''''''''''''''''
Pour commencez vous aurez besoin d'un modem GSM compatible avec Gammu, pour cela reportez-vous à `la liste des modems supportés par gammu`_.

Nous vous conseillons d'utiliser directement `un modem SIM800L`_, plus simple à trouver, moins cher et totalement compatible avec Gammu.

1.2 Configurer gammu pour utiliser le modem GSM
''''''''''''''''''''''''''''''''''''''''''''''''
Une fois que vous avez relié votre modem GSM au serveur vous devez encore configurer Gammu pour utiliser ce GSM. Pour cela vous devez créer un fichier de configuration que vous renseignerez par la suite dans RaspiSMS. Pour créer ce fichier de configuration repportez-vous à `la documentation de gammu`_.

Pour générer le fichier de configuration vous pouvez utiliser les commandes ``gammu-config`` et ``gammu-detect``, la encore reportez-vous à la documentation de gammu sur `gammu-detect`_ et `gammu-config`.

1.2.1 Quel fichier de configuration utiliser ?
"""""""""""""""""""""""""""""""""""""""""""""""
Généralement, gammu utilise le fichier ``/etc/gammu.rc`` pour stocker la configuration. Dans notre cas nous avons plutôt intéret à créer un fichier de configuration à part pour chaque téléphone. Le chemin de ce fichier sera ensuite renseigné dans RaspiSMS afin d'être utilisé lors de l'appel interne à Gammu.

Créer donc un nouveau fichier avec un nom au format ``/etc/gammu_<id_unique>.rc``.

1.2.2 Exemple de fichier de configuration
""""""""""""""""""""""""""""""""""""""""""""
Voici un exemple de fichier de configuration que vous pouvez utiliser comme base à adapter pour votre propre modem GSM ::

    [gammu]
    device = /dev/ttyUSB0
    name = Phone on USB serial port HUAWEI_Technologies HUAWEI_Mobile
    connection = at
    log_file = /var/log/raspisms/gammu_uid.log
    log_format = textalldate
    gammu_coding = utf8


2. Ajouter un téléphone dans RaspiSMS
----------------------------------------
Dans RaspiSMS, rendez-vous dans la partie **"Téléphones"**, **"Ajouter un téléphone"** et dans la liste **"Adaptateur logiciel du téléphone"** choisissez **"Gammu"**.

2.1 Le nom du téléphone
''''''''''''''''''''''''
Dans le champs **"Nom"** donnez un nom unique au téléphone que vous allez créer. Ce nom n'apparait que pour vous, jamais pour les destinataires du message.

2.2 Renseigner le fichier de configuration
''''''''''''''''''''''''''''''''''''''''''''''
Il est possible d'utiliser plusieurs téléphones gammu en même temps. Pour cela, RaspiSMS doit savoir exactement quel fichier de configuration utiliser pour chaque téléphone.
Copiez donc le chemin du fichier de configuration que vous venez des créer (``/etc/gammu_<id_unique>.rc``), et collez-le dans le champs **"Fichier de configuration"**.

2.3 Utiliser un code PIN
'''''''''''''''''''''''''''''
.. warning::
    Laissez ce champs vide si votre carte SIM n'utilise pas de code PIN.

Si votre carte SIM utilise un code PIN vous devez rentrer ce code PIN dans le champs **"Code Pin"** dans RaspiSMS. 


Et voilà, vous pouvez maintenant envoyez et recevoir des SMS avec Gammu et RaspiSMS !


.. _un modem SIM800L: https://geni.us/sim800 
.. _gammu-detect: https://wammu.eu/docs/manual/utils/gammu-detect.html
.. _gammu-config: https://wammu.eu/docs/manual/utils/gammu-config.html
.. _la liste des modems supportés par gammu: https://fr.wammu.eu/phones/
.. _la documentation de gammu: https://wammu.eu/docs/manual/config/index.html
