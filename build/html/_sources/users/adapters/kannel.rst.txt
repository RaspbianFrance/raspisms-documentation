=============================
Téléphone Kannel
=============================
Le téléphone Kannel permet de connecter RaspiSMS à une instance du logiciel `Kannel`_. Kannel est un logiciel de passerelle SMS vous permettant aussi bien d'utiliser un modem GSM équipé d'une carte SIM, que de vous brancher directement au SMSC d'un opérateur.

Grâce au téléphone Kannel, vous profitez de l'interface utilisateur et des fonctionnalités avancées de RaspiSMS, tout en vous appuyant sur la puissance de Kannel, un standard de l'industrie des télécoms.

Cette méthode vous permet donc aussi bien d'utiliser un forfait téléphonique classique avec RaspiSMS, que de vous connecter directement au SMS Center d'un opérateur, ou bien d'utiliser u n groupe SMPP.

Fonctionnalités supportées
--------------------------
=============================== =========
 Fonctionnalité                 Support
=============================== =========
Envoi de SMS                    Oui
Réception de SMS                Oui
Status du téléphone             Non
Envoi de MMS                    Non
Réception de MMS                Non
Suivi de l'envoi                Oui
SMS Flash                       Non
Alerte sur réception d'un appel Non
Alerte sur fin d'un appel       Non
=============================== =========



1. Configurer le serveur Kannel
-------------------------------
Avant de pouvoir créer un téléphone Kannel dans RaspiSMS, vous devrez avoir installer et configurer le logiciel Kannel sur un serveur disposant d'une IP public, laquelle sera ensuite renseignée dans RaspiSMS.


1.1 Comment configurer Kannel ?
''''''''''''''''''''''''''''''''''''''''''''''''
Pour permettre l'utilisation de Kannel avec RaspiSMS, vous devrez configurer les groupes suivants :

* Un groupe ``sms-box`` pour activer les fonctionnalités SMS de Kannel.
* Un groupe ``sendsms-user`` pour permettre l'envoi des SMS.
* Un groupe ``smsc`` pour transmettre les SMS au SMSC opérateur ou au modem GSM.
* Un groupe ``smsbox-route`` pour recevoir les SMS depuis le réseau opérateur ou le modem GSM.
* Un groupe ``sms-service`` pour transmettre les SMS reçus par Kanel à RaspiSMS.

Pour plus d'informations sur la configuration de ces différents groupes, reportez-vous à la configuration de Kannel, `la documentation de Kannel`_.

1.2.1 Configuration spécifique des groupes Kannel pour relier RaspiSMS
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
Pour pleinement relier RaspiSMS à Kannel, vous devrez adapter la configuration de certains groupes Kannel.

1.2.1.1 Groupe ``sendsms-user``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Vous devez définir un groupe ``sendsms-user``, avec un nom d'utilisateur et un mot de passe pour le groupe. Vous devrez aussi :

* Définir l'option ``max-messages`` à ``10`` ou plus pour vous assurez que les SMS longs envoyés depuis RaspiSMS ne seront pas coupés par Kannel.
* Si vous voulez que les SMS longs soient envoyés en un seul message, définir l'option ``concatenation`` à ``true``.

1.2.1.2 Groupe ``sms-service``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Vous devez définir un groupe ``sms-service``, avec :

* L'option ``keyword`` à ``default`` pour que le groupe se déclenche sur tous les messages.
* L'option ``max-messages`` à ``0`` pour que Kannel ne tente pas de répondre automatiquement aux SMS entrants.
* L'option ``concatenation`` à ``true`` pour que Kannel ré-assemble automatiquement les longs SMS en un seul.
* L'option ``post-url``. Une fois le téléphone Kannel créé dans RaspiSMS, en vous rendant sur la liste des téléphones créés, dans la colonne "Callbacks" du téléphone Kannel que vous venez de créer, vous trouverez l'adresse URL à utiliser pour l'option ``post-url`` sous l'entrée "Réception d'un SMS". 


1.2.2 Exemple de fichier de configuration
""""""""""""""""""""""""""""""""""""""""""
Le fichier de configuration ``/etc/kannel/kannel.conf`` ci-dessous vous offre un exemple de serveur Kannel avec un SMSC ``FAKE`` (un SMSC Fake est utilisé uniquement à des fins de tests) et configuré de façon à pouvoir être utilisé avec RaspiSMS.

.. warning::
    N'oubliez pas de remplacer les variables précédez d'un ``$`` par de vraies valeures, et n'oubliez pas de modifier l'option ``post-url`` à la fin de l'étape 3 de ce document.

.. literalinclude:: /_code_examples/adapters/kannel.conf
    :language: ini


2. Ajouter un téléphone dans RaspiSMS
----------------------------------------
Dans RaspiSMS, rendez-vous dans la partie **"Téléphones"**, **"Ajouter un téléphone"** et dans la liste **"Type de téléphone"** choisissez **"Kannel"**.

2.1 Le nom du téléphone
''''''''''''''''''''''''
Dans le champ **"Nom"** donnez un nom unique au téléphone que vous allez créer. Ce nom n'apparaît que pour vous, jamais pour les destinataires du message.

2.2 Définir l'adresse du script d'envoi de Kannel
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Dans le champ **"Adresse URL du groupe kannel sendsms"**, vous devez renseignez l'adresse URL du groupe ``send-sms`` de Kannel qui sera utilisé par RaspiSMS pour déclencher l'envoi d'un SMS par Kannel.

Si vous utilisez le fichier de configuration de Kannel précédemment indiqué, cette adresse devrait être de la forme ``http://IP_OU_URL_DE_VOTRE_SERVEUR_KANNEL:13013/cgi-bin/sendsms``.

2.3 Définir l'utilisateur Kannel pour l'envoi des SMS
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour envoyer un message, Kannel demande les identifiants de l'utilisateur lié au groupe ``send-sms`` de Kannel.

Copiez-vous la valeure de l'option ``username`` du groupe ``send-sms`` et collez-le dans le champ **"Nom de l'utilisateur"**. Faites de même pour l'option ``username`` avec le champ **"Mot de passe de l'utilisateur"**.

2.4 Définir le numéro de téléphone/nom transmis un destinataire
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Kannel permet (voir nécessite) de définir le numéro de téléphone ou le nom qui sera transmis au destinataire du SMS.

Même si ce paramètre est considéré comme obligatoire par Kannel (paramètre ``from``), dans la très vaste majorité des cas, ce numéro ou ce nom sera remplacé par un numéro/nom qui vous aura été attribué par l'opérateur mobile gérant le SMSC auquel vous vous connectez. Dans certains cas néanmoins, il se pourrait qu'il s'agisse du nom/numéro qui sera affiché au destinataire lors de la réception d'un SMS.

Vous devez donc renseigner ce numéro de téléphone ou nom dans le champ **"Numéro de téléphone émetteur ou nom de l'émetteur"**. Si vous renseigner un nom, sa longueur devrat être comprise entre 3 et 11 caractères.

2.5 Activer le suivi du statut d'un SMS
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
.. warning::
    La modification de ce champs est réservée à des usages spécifiques et des utilisateurs avancés.

Lors du déclenchement de l'envoi d'un SMS, il est possible de spécifier à Kannel une adresse URL à laquelle sera transmise une requête HTTP lors de certains évènements au cours de la vie du SMS (refus par l'opérateur, réception par le téléphone final, échec de livraison, etc.).

Pour cela et par défaut, le champs **"Adresse URL de livraison du Delivery Report du SMS"** est pré-rempli avec l'adresse de base utilisée par RaspiSMS pour recevoir la mise à jour du statut d'un SMS et vous ne devriez pas la modifier. Néanmoins, si vous utilisez RaspiSMS et Kannel sur des sous-réseaux internes, vous pourriez vouloir modifier la partie ``domaine`` de cette adresse pour utiliser un domaine ou une IP interne.

2.6 Définir le SMSC à utiliser pour envoyer les SMS avec ce téléphone
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Si vous utilisez plusieurs SMSC au sein de Kannel et un seul service ``send-sms``, vous pouvez définir dans le champs **"Identifiant unique du SMSC"** l'identifiant du SMSC à utiliser pour envoyer les SMS avec ce téléphone Kannel.

Si vous ne utilisez un seul SMSC, ou bien si vous préférez Kannel s'occuper lui même du routage, laissez simplement ce champs vide.

Enfin, cliquez sur le bouton **"Enregistrer le téléphone"**.


3. Modifier la configuration du seveur Kannel pour transmettre les SMS reçus à RaspiSMS
----------------------------------------------------------------------------------------
Maintenant que le téléphone a été créé au sein de RaspiSMS, la dernière chose à faire est de configurer Kannel pour que celui-ci transmette à RaspiSMS les SMS qu'il reçoit.

Pour cela, rendez-vous dans l'interface listant les téléphones au sein de RaspiSMS. Dans cette liste, cherchez le téléphone que vous venez de créer etopiez l'adresse URL apparaissant sous la partie **"Réception d'un SMS"** dans la colonne **"Callbacks"**.

Enfin, sur le serveur Kannel, modifiez le fichier de configuration ``/etc/kannel/kannel.conf``, et remplacez/ajoutez la valeure de l'option ``post-url`` par l'adresse URL précedemment copiée.

Et voilà, vous pouvez désormais envoyer et recevoir des SMS depuis RaspiSMS en passant par Kannel !

.. _Kannel: https://www.kannel.org/
.. _la documentation de Kannel: https://www.kannel.org/download/kannel-userguide-snapshot/userguide.html
