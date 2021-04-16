.. _users_adapters_ovh_virtual_number:

==================================
Téléphone OVH SMS Numéro Virtuel
==================================
Le téléphone OVH SMS permet l'utilisation de l'offre SMS de OVH avec RaspiSMS en se basant sur l'utilisation de l'API SMS avec un numéro virtuel.

L'utilisation d'un numéro virtuel vous permet de communiquer ce numéro à vos utilisateurs pour qu'ils puissent vous envoyer des messages, même si vous ne les avez pas vous-même contactés auparavant.

.. raw:: html
    :file: ../../_videos/phone_ovh_virtual.html

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


Configurer un téléphone OVH SMS
-------------------------------
Dans cette partie nous allons voir comment configurer un téléphone OVH SMS pour lier RaspiSMS et OVH.

1. Souscrire à une offre SMS avec numéro virtuel chez OVH
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour utiliser un téléphone OVH avec numéro virtuel vous devez prendre un abonnement adapté chez OVH.

Pour cela, rendez-vous sur `L'offre numéro virtuel`_, cliquez sur le bouton **"Commander"** et validez la procédure d'achat/inscription.


2. Ajouter un téléphone OVH dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''
Dans RaspiSMS, rendez-vous dans la partie **"Téléphones"**, **"Ajouter un téléphone"** et dans la liste **"Type de téléphone"** choisissez **"OVH SMS Numéro Virtuel"**.

3. Récupérer les informations nécessaires à la configuration du téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour configurer le téléphone dans RaspiSMS nous allons avoir besoin de quelques informations qui se trouvent dans votre compte OVH.

3.1 Le nom du téléphone
#######################
Dans le champ **"Nom"** donnez un nom unique au téléphone que vous allez créer. Ce nom n'apparaît que pour vous, jamais pour les destinataires du message.

3.2 Trouver le nom de votre service SMS
#######################################
Commencez par vous connecter à votre compte client OVH et rendez-vous dans `La partie SMS de votre manager OVH`_, puis cliquez sur le nom de votre abonnement.
Copier la valeur présente dans la colonne **"Nom"** de la liste de vos services et copiez-la dans le champ **"Service Name"** de RaspiSMS.

3.3 Trouver le numéro de téléphone virtuel OVH
##############################################
Nous allons maintenant avoir besoin de votre numéro de téléphone virtuel chez OVH. Pour cela, cliquez sur le nom de votre service dans la liste, et rendez-vous sur l'onglet **"Expéditeurs"**. Votre numéro apparaît dans la colonne **"Expéditeur"**, recopiez ce numéro dans le champ **"Numéro de téléphone"** de RaspiSMS.

3.4 Créer et récupérer les identifiants API OVH
###############################################
Dernière étape, nous allons créer et récupérer les identifiants API de OVH. Pour cela, rendez-vous sur `La page de création des identifiants API de OVH`_ et entrez les informations suivantes :

- **Account ID or email address** : Votre identifiant OVH
- **Password** : Votre mot de passe OVH
- **Script name** : RaspiSMS
- **Script description** : RaspiSMS Application
- **Validity** : Unlimited

La partie **"Rights"** permet de définir les droits de l'application sur le compte OVH. Laissez-la telle que définie lors de votre arrivée sur la page.

Cliquez sur **"Create keys"** pour créer vos identifiants API OVH.

.. warning::
    Si vous avez l'erreur **"An application with that name already exists"**, modifiez **"Script Name"**, vous pouvez y mettre la valeur de votre choix, c'est sans importance.

Vous arrivez alors sur une page vous affichant les identifiants API créés. Il vous suffit de recopier les valeurs dans **"Application Key"**, **"Application Secret"** et **"Consumer Key"** dans les champs correspondant dans RaspiSMS.

Retournez sur RaspiSMS et cliquez sur le bouton **"Enregistrer le téléphone"**.

4. Activer le suivi du statut des SMS
''''''''''''''''''''''''''''''''''''''
Au point où nous en sommes nous avons un téléphone OVH fonctionnel dans RaspiSMS, mais celui-ci n'est pas encore capable de suivre le statut des SMS envoyés.
Le suivi du statut d'un SMS se fait par une `callback`, c'est-à-dire une adresse URL qui sera appelée par le fournisseur (dans notre cas OVH) quand le statut d'un SMS évolue.

En vous rendant sur la liste de vos téléphones dans RaspiSMS, vous devriez voir celui que vous venez de créer. Copiez l'adresse URL affichée sous le label **"Changement de statut d'un SMS"** dans la colonne **"Callbacks"** correspondant au téléphone que vous venez de créer.

Rendez-vous dans votre manager OVH, sur votre offre de SMS et dans la partie **"Options"**, **"Options générales"**. Cliquez sur le bouton **"Modifier"** et dans le champ **"URL de callback changements d’état"** collez l'adresse précédemment copiée et cliquez sur **"Valider"**.


Et voilà, vous pouvez maintenant envoyer des SMS via OVH avec RaspiSMS !




.. _L'offre numéro virtuel: https://www.ovhtelecom.fr/sms/reponse/numeros-virtuels.xml
.. _La partie SMS de votre manager OVH: https://www.ovhtelecom.fr/manager/#/sms/
.. _La page de création des identifiants API de OVH: https://eu.api.ovh.com/createToken/index.cgi?GET=/sms&GET=/sms/*&POST=/sms/*&PUT=/sms/*&DELETE=/sms/*&
