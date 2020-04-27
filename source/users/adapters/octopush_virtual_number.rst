===================================
Adaptateur Octopush Numéro Virtuel
===================================
.. warning::
    Actuellement l'offre d'Octopush nous semble moins aboutie que celle d'autres fournisseurs comme OVH ou Twilio, nous vous recommandons donc de privilégier ces fournisseurs.

L'adaptateur Octopush Numéro Virtuel permet l'utilisation de la plateforme Octopush avec RaspiSMS en se basant sur l'utilisation de l'API avec un numéro virtuel.

L'utilisation d'un numéro virtuel vous permet de communiquer ce numéro à vos utilisateurs pour qu'ils puissent vous envoyer des messages, même si vous ne les avez pas vous même contactés auparavant.

Fonctionnalités supportées
--------------------------
================ =========
 Fonctionnalité   Support
================ =========
Envoi de SMS     Oui
Reception de SMS Oui
Suivi de l'envoi Oui
SMS Flash        Non
================ =========



Configurer l'adaptateur Octopush SMS
-------------------------------------
Dans cette partie nous allons voir comment configurer l'adaptateur Octopush Numéro Virtuel pour lier RaspiSMS et Octopush.

1. Souscrire à une offre SMS avec numéro virtuel chez Octopush
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour utiliser l'adaptateur Octopush avec numéro virtuel vous devez prendre un abonnement adapté chez Octopush.

Pour cela, rendez-vous sur `Le site d'octopush`_, souscrivez à une offre chez eux et passez par la page de contact pour demander un devis afin d'obtenir un numéro virtuel.

Vous devez attendre d'avoir obtenu votre numéro pour pouvoir passer à la suite de ce guide.

2. Ajouter le téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''
Dans RaspiSMS, rendez-vous dans la partie **"Téléphones"**, **"Ajouter un téléphone"** et dans la liste **"Adaptateur logiciel du téléphone"** choisissez **"Octopush Numéro Virtuel"**.

3. Récupérer les informations nécessaires à la configuration du téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour configurer le téléphone dans RaspiSMS nous allons avoir besoin de quelques informations qui se trouvent dans votre compte Octopush.

3.1 Le nom du téléphone
#######################
Dans le champs **"Nom"** donnez un nom unique au téléphone que vous allez créer. Ce nom n'apparait que pour vous, jamais pour les destinataires du message.

3.2 Récupérer les identifiants API Octopush
##############################################
Rendez-vous sur `le site d'octopush`_, connectez-vous à votre compte et rendez-vous dans la partie `identifiants API`_ d'Octopush.
Copiez la valeure dans le cadre "Login" dans le champs **"Octopush Login"** de RaspiSMS, et la valeure dans le cadre "Clé API" dans le champs **"API Key"** de RaspiSMS.

3.3 Définir le numéro de téléphone virtuel
###############################################
Dernière étape, rentrez votre numéro de téléphone virtuel Octopush dans le champs "Numéro de téléphone virtuel" de RaspiSMS.

4. Activer la réception de messages et le suivi du statut des SMS
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Au point ou nous en sommes nous avons un téléphone Octopush capable d'envoyer des SMS depuis RaspiSMS, mais celui-ci n'est pas encore capable de suivre le statut des SMS envoyés ou de recevoir des messages.
Pour activer le suivi de status d'un SMS et la réception de messages vous devez transmettre à Octopush les adresses URL que leurs serveurs devront contacter pour transmettre l'information à RaspiSMS.

Pour cela, vous devez envoyer un mail à Octopush en leur transmettant ces addresses dites de `callback`. Vous pouvez trouver ces adresses en vous rendant sur la liste de vos téléphone dans RaspiSMS, vous devriez voir celui que vous venez de créer. Copiez l'adresse URL affichée sous le label **"Changement de status d'un SMS"** dans la colonne **"Callbacks"** correspondant au téléphone que vous venez de créer.


Le suivi du status d'un SMS se fait par une `callback`, c'est-à-dire une adresse URL qui sera appelé par le fournisseur (dans notre cas OVH) quand le statut d'un SMS évolu.

En vous rendant sur la liste de vos téléphone dans RaspiSMS, vous devriez voir celui que vous venez de créer. Dans la colonne **"Callbacks"** vous trouverez les deux adresses à transmettre à Octopush.

Et voilà, vous pouvez maintenant envoyez et recevoir des SMS via Octopush avec RaspiSMS !




.. _Le site d'octopush: https://www.octopush.com/
.. _identifiants API: https://www.octopush-dm.com/api-logins
