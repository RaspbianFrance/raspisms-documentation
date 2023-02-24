.. _users_adapters_odyssey_messaging:

===================================
Téléphone Odyssey Messaging
===================================
Le téléphone Odyssey Messaging permet l'utilisation de la plateforme Odyssey Messaging avec RaspiSMS en se basant sur l'utilisation de l'API.

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



Configurer un téléphone Odyssey Messaging
------------------------------------------
Dans cette partie nous allons voir comment configurer un téléphone Odyssey Messaging pour lier RaspiSMS et Odyssey Messaging.

1. Souscrire à une offre chez Odyssey Messaging
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour utiliser un téléphone Odyssey Messaging vous devez prendre un abonnement adapté chez Odyssey Messaging.

Pour cela, rendez-vous sur `Le site d'Odyssey Messaging`_.

2. Ajouter le téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''
Dans RaspiSMS, rendez-vous dans la partie **"Téléphones"**, **"Ajouter un téléphone"** et dans la liste **"Type de téléphone"** choisissez **"Odyssey Messaging"**.

3. Récupérer les informations nécessaires à la configuration du téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour configurer le téléphone dans RaspiSMS nous allons avoir besoin de quelques informations qui se trouvent dans votre compte Odyssey Messaging.

3.1 Le nom du téléphone
#######################
Dans le champ **"Nom"** donnez un nom unique au téléphone que vous allez créer. Ce nom n'apparaît que pour vous, jamais pour les destinataires du message.

3.2 Définir les identifiants API Odyssey Messaging
##################################################
Dans le champ **"Odyssey login"** renseignez votre login Odyssey Messaging (format 12345.user), dans le champ **"Mot de passe"** renseignez votre mot de passe Odyssey Messaging.

3.3 Définir le nom d'expéditeur
###############################################
.. warning::
    Si vous ne souhaitez pas utiliser d'expéditeur personnalisé, laissez le champ vide.

Si vous le souhaitez vous pouvez utiliser un nom d'expéditeur qui sera affiché au destinataire à la place du numéro de téléphone.
Pour cela, entrez le nom que vous souhaitez utiliser dans le champ **"Nom de l'expéditeur"** dans RaspiSMS.

4. Activer la réception de messages et le suivi du statut des SMS
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Au point où nous en sommes nous avons un téléphone Odyssey Messaging capable d'envoyer des SMS depuis RaspiSMS, mais celui-ci n'est pas encore capable de suivre le statut des SMS envoyés ou de recevoir des messages.

La réception de SMS et le suivi du statut d'un SMS se fait par une `callback`, c'est-à-dire une adresse URL qui sera appelée par le fournisseur (dans notre cas Odyssey Messaging) quand le statut d'un SMS évolue.

Pour activer le suivi de statut d'un SMS et la réception de messages vous devez transmettre à Odyssey Messaging les adresses URL que leurs serveurs doivent contacter pour transmettre l'information à RaspiSMS.

4.1 La réception de messages
############################

En vous rendant sur la liste de vos téléphones dans RaspiSMS, vous devriez voir celui que vous venez de créer. Copiez l'adresse URL affichée sous le label **"Réception d'un SMS"** dans la colonne **"Callbacks"** correspondant au téléphone que vous venez de créer.

Rendez-vous dans la partie **"Configurer"**, **"CallBack API"** de Odyssey Messaging, et ajoutez une nouvelle callback. Choisissez le type ``InboundSMS``, selectionnez le protocole ``PostHttp``, dans le champs **"Adresse"**, collez l'URL précédemment copiée depuis RaspiSMS, enfin sélectionnez le paramètre ``JSON``.

4.2 Le suivi du status d'un SMS
####################################

En vous rendant sur la liste de vos téléphones dans RaspiSMS, vous devriez voir celui que vous venez de créer. Copiez l'adresse URL affichée sous le label **"Changement de statut d'un SMS"** dans la colonne **"Callbacks"** correspondant au téléphone que vous venez de créer.

Rendez-vous dans la partie **"Configurer"**, **"CallBack API"** de Odyssey Messaging, et ajoutez une nouvelle callback. Choisissez le type ``ItemStatusChanged``, selectionnez le protocole ``PostHttp``, dans le champs **"Adresse"**, collez l'URL précédemment copiée depuis RaspiSMS, enfin sélectionnez le paramètre ``JSON``.

Et voilà, vous pouvez maintenant envoyer et recevoir des SMS via Odyssey Messaging avec RaspiSMS !




.. _Le site d'Odyssey Messaging: https://www.odyssey-messaging.com/