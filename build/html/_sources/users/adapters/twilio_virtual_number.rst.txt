================================
Téléphone Twilio Numéro Virtuel
================================
Le téléphone Twilio Numéro Virtuel permet l'utilisation de l'offre SMS de Twilio avec RaspiSMS en se basant sur l'utilisation de l'API SMS avec un numéro virtuel.

L'utilisation d'un numéro virtuel vous permet de communiquer ce numéro à vos utilisateurs pour qu'ils puissent vous envoyer des messages, même si vous ne les avez pas vous même contactés auparavant.

Fonctionnalités supportées
--------------------------
=============================== =========
 Fonctionnalité                 Support
=============================== =========
Envoi de SMS                    Oui
Réception de SMS                Oui
Envoi de MMS                    Non
Réception de MMS                Non
Suivi de l'envoi                Oui
SMS Flash                       Non
Alerte sur réception d'un appel Non
Alerte sur fin d'un appel       Non
=============================== =========



Configurer un téléphone Twilio
---------------------------------
Dans cette partie nous allons voir comment configurer un téléphone Twilio pour lier RaspiSMS et Twilio.

1. Souscrire à une offre SMS avec numéro virtuel chez Twilio
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour utiliser un téléphone Twilio avec numéro virtuel vous devez prendre un abonnement adapté chez Twilio.

Pour cela, rendez-vous sur `L'inscription de Twilio`_ et validez la procédure d'inscription.


2. Ajouter un téléphone OVH dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''
Dans RaspiSMS, rendez-vous dans la partie **"Téléphones"**, **"Ajouter un téléphone"** et dans la liste **"Type de téléphone"** choisissez **"Twilio Numéro Virtuel"**

3. Récupérer les informations nécessaires à la configuration du téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour configurer le téléphone dans RaspiSMS nous allons avoir besoin de quelques informations qui se trouvent dans votre compte Twilio.

3.1 Le nom du téléphone
#######################
Dans le champ **"Nom"** donnez un nom unique au téléphone que vous allez créer. Ce nom n'apparaît que pour vous, jamais pour les destinataires du message.

3.2 Récupérer les identifiants API Twilio
###############################################
Pour commencer nous allons récupérer les identifiants API de Twilio. Pour cela, rendez-vous sur `L'accueil de la console Twilio`_ vos identifiants API sont indiqués en bas de la partie "Project Info".

Copiez la valeur sous **"Account SID"** dans le champ du même nom dans RaspiSMS, et cliquez sur **"Show"** sous **"Auth Token"** pour afficher la valeur, et recopiez-la dans le champ du même nom dans RaspiSMS.

3.3 Trouver le numéro de téléphone virtuel
############################################
Commencez par vous connecter à votre compte Twilio et rendez-vous dans `La partie numéro de téléphone de la console Twilio`_, puis créez un nouveau numéro de téléphone. Cette opération peut prendre un peu de temps car il peut y avoir une validation manuelle dans certains cas.

Vous devrez attendre que le numéro soit validé pour pouvoir passer à la suite de ce guide.

Une fois le numéro de téléphone virtuel créé, rendez-vous de nouveau dans `La partie numéro de téléphone de la console Twilio`_, le numéro apparaît dans la colonne **"Number"**. Recopiez ce numéro dans le champ **"Numéro de téléphone virtuel"** de RaspiSMS.

Sur RaspiSMS, laissez le champ **"Callback de changement de statut"** tel quel pour que Twilio envoie un requête à RaspiSMS lors du changement de statut d'un message.

Enfin, cliquez sur le bouton **"Enregistrer le téléphone"**.

Et voilà, vous pouvez maintenant envoyez des SMS via Twilio avec RaspiSMS !




.. _L'inscription de Twilio: https://www.twilio.com/try-twilio
.. _La partie numéro de téléphone de la console Twilio: https://www.twilio.com/console/phone-numbers/incoming
.. _L'accueil de la console Twilio: https://www.twilio.com/console
