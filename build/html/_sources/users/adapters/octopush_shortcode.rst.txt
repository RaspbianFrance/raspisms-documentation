===================================
Adaptateur Octopush Shortcode
===================================
.. warning::
    Actuellement l'offre d'Octopush nous semble moins aboutie que celles d'autres fournisseurs comme OVH ou Twilio, nous vous recommandons donc de privilégier ces derniers.

L'adaptateur Octopush Shortcode permet l'utilisation de la plateforme Octopush avec RaspiSMS en se basant sur l'utilisation de l'API et un numéro court aléatoire.

L'utilisation d'un numéro court aléatoire vous permet d'envoyer un message à vos utilisateurs mais pas de recevoir des réponses. Vous pouvez aussi utiliser un nom alphanumérique à la place du numéro.

Fonctionnalités supportées
--------------------------
================ =========
 Fonctionnalité   Support
================ =========
Envoi de SMS     Oui
Réception de SMS Oui
Suivi de l'envoi Oui
SMS Flash        Non
================ =========



Configurer l'adaptateur Octopush SMS
-------------------------------------
Dans cette partie nous allons voir comment configurer l'adaptateur Octopush Shortcode pour lier RaspiSMS et Octopush.

1. Souscrire à une offre chez Octopush
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour utiliser l'adaptateur Octopush vous devez prendre un abonnement adapté chez Octopush.

Pour cela, rendez-vous sur `Le site d'octopush`_.

2. Ajouter le téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''
Dans RaspiSMS, rendez-vous dans la partie **"Téléphones"**, **"Ajouter un téléphone"** et dans la liste **"Adaptateur logiciel du téléphone"** choisissez **"Octopush Shortcode"**.

3. Récupérer les informations nécessaires à la configuration du téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour configurer le téléphone dans RaspiSMS nous allons avoir besoin de quelques informations qui se trouvent dans votre compte Octopush.

3.1 Le nom du téléphone
#######################
Dans le champ **"Nom"** donnez un nom unique au téléphone que vous allez créer. Ce nom n'apparaît que pour vous, jamais pour les destinataires du message.

3.2 Récupérer les identifiants API Octopush
##############################################
Rendez-vous sur `le site d'octopush`_, connectez-vous à votre compte et rendez-vous dans la partie `identifiants API`_ d'Octopush.
Copiez la valeur dans le cadre "Login" dans le champ **"Octopush Login"** de RaspiSMS, et la valeur dans le cadre "Clé API" dans le champ **"API Key"** de RaspiSMS.

3.3 Définir le nom d'expéditeur
###############################################
.. warning::
    Si vous ne souhaitez pas utiliser d'expéditeur personnalisé, laissez le champ vide.

Si vous le souhaitez vous pouvez utiliser un nom d'expéditeur qui sera affiché au destinataire à la place du numéro de téléphone.
Pour cela, entrez le nom que vous souhaitez utiliser dans le champ **"Nom de l'expéditeur"** dans RaspiSMS.

4. Activer la réception de messages et le suivi du statut des SMS
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Au point où nous en sommes nous avons un téléphone Octopush capable d'envoyer des SMS depuis RaspiSMS, mais celui-ci n'est pas encore capable de suivre le statut des SMS envoyés ou de recevoir des messages.
Pour activer le suivi de statut d'un SMS et la réception de messages vous devez transmettre à Octopush les adresses URL que leurs serveurs devront contacter pour transmettre l'information à RaspiSMS.

Pour cela, vous devez envoyer un mail à Octopush en leur transmettant ces adresses dites de `callback`. Vous pouvez trouver ces adresses en vous rendant sur la liste de vos téléphones dans RaspiSMS ; vous devriez voir celui que vous venez de créer. Copiez l'adresse URL affichée sous le label **"Changement de statut d'un SMS"** dans la colonne **"Callbacks"** correspondant au téléphone que vous venez de créer.


Le suivi du statut d'un SMS se fait par une `callback`, c'est-à-dire une adresse URL qui sera appelée par le fournisseur (dans notre cas Octopush) quand le statut d'un SMS évolue.

En vous rendant sur la liste de vos téléphones dans RaspiSMS, vous devriez voir celui que vous venez de créer. Dans la colonne **"Callbacks"** vous trouverez les deux adresses à transmettre à Octopush.

Et voilà, vous pouvez maintenant envoyer et recevoir des SMS via Octopush avec RaspiSMS !




.. _Le site d'octopush: https://www.octopush.com/
.. _identifiants API: https://www.octopush-dm.com/api-logins
