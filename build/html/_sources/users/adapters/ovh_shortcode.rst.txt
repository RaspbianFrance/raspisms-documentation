=============================
Téléphone OVH SMS Shortcode
=============================
Le téléphone OVH SMS Shortcode permet l'utilisation de l'offre SMS de OVH avec RaspiSMS en se basant sur l'utilisation de l'API SMS en utilisant des numéros courts, ou un nom alphanumérique affiché à la place du numéro réel.

L'utilisation d'un numéro court vous permet de recevoir des réponses aux messages envoyés, mais uniquement pendant 48 heures. L'utilisation d'un nom alphanumérique ne permet pas de recevoir des réponses. Ces solutions sont plus adaptées à l'envoi de SMS commerciaux ou techniques.

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

1. Souscrire à une offre SMS chez OVH
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour utiliser un téléphone OVH avec un Shortcode vous devez prendre un abonnement adapté chez OVH.

Pour cela, rendez-vous sur `L'offre SMS OVH`_, cliquez sur le bouton **"Commander"** et validez la procédure d'achat/inscription.

1.2 Utiliser un nom personnalisé
################################
.. warning::
    Si vous ne souhaitez pas utiliser un nom personnalisé, vous pouvez passer cette partie.

Si vous le souhaitez il est possible d'utiliser un nom d'expéditeur qui sera affiché à la place du numéro de téléphone. Pour cela, OVH exige que vous fassiez vérifier le nom de cet expéditeur pour éviter les usurpations d'identité (par exemple qu'une personne envoie un message en se faisant passer pour une entreprise connue).

Pour cela, vous devez vous rendre dans `La partie SMS de votre manager OVH`_, puis cliquer sur le nom de votre abonnement. Rendez-vous dans la partie **"Expéditeurs"**, cliquez sur le bouton **"Action"** au dessus de la liste des expéditeurs et ajoutez un nouvel expéditeur. Remplissez la demande selon vos besoin et validez. Votre demande de nom fera alors l'objet d'une validation manuelle. Une fois le nom validé vous pourrez commencer à l'utiiser pour envoyer des messages. Vous devrez attendre la validation du nom pour passer à la suite de ce guide.

2. Ajouter un téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''
Dans RaspiSMS, rendez-vous dans la partie **"Téléphones"**, **"Ajouter un téléphone"** et dans la liste **"Type de téléphone"** choisissez **"OVH SMS Shortcode"**.

3. Récupérer les informations nécessaires à la configuration du téléphone dans RaspiSMS
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Pour configurer le téléphone dans RaspiSMS nous allons avoir besoin de quelques informations qui se trouvent dans votre compte OVH.

3.1 Le nom du téléphone
#######################
Dans le champ **"Nom"** donnez un nom unique au téléphone que vous allez créer. Ce nom n'apparaît que pour vous, jamais pour les destinataires du message.

3.2 Trouver le nom de votre service SMS
#######################################
Commencez par vous connecter à votre compte client OVH et rendez-vous dans `La partie SMS de votre manager OVH`_, puis cliquez sur le nom de votre abonnement.
Copiez la valeur présente dans la colonne **"Nom"** de la liste de vos services et copiez-la dans le champ **"Service Name"** de RaspiSMS.

3.3 Utiliser un nom d'expéditeur
##############################################
Si vous souhaitez utiliser un nom alphanumérique à la place du shortcode vous devrez l'indiquer dans le champ **"Nom de l'expéditeur"**. Ce nom doit au préalable avoir été validé par OVH.

Si vous ne souhaitez pas utiliser de nom à la place du shortcode, laissez ce champ vide.

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

.. note::
    Si vous utilisez un expéditeur nommé, OVH ajoutera automatiquement une clause STOP SMS à la fin de vos message. Si vous souhaitez retirer cette clause vous pouvez cochez la case **"Désactiver la clause 'STOP SMS' automatique"**. 

Retournez sur RaspiSMS et cliquez sur le bouton **"Enregistrer le téléphone"**.



4. Activer le suivi du statut des SMS
''''''''''''''''''''''''''''''''''''''
Au point ou nous en sommes nous avons un téléphone OVH fonctionnel dans RaspiSMS, mais celui-ci n'est pas encore capable de suivre le statut des SMS envoyés.
Le suivi du statut d'un SMS se fait par une `callback`, c'est-à-dire une adresse URL qui sera appelée par le fournisseur (dans notre cas OVH) quand le statut d'un SMS évolue.

En vous rendant sur la liste de vos téléphones dans RaspiSMS, vous devriez voir celui que vous venez de créer. Copiez l'adresse URL affichée sous le label **"Changement de statut d'un SMS"** dans la colonne **"Callbacks"** correspondant au téléphone que vous venez de créer.

Rendez-vous dans votre manager OVH, sur votre offre de SMS et dans la partie **"Options"**, **"Options générales"**. Cliquez sur le bouton **"Modifier"** et dans le champ **"URL de callback changements d’état"** collez l'adresse précédemment copiée et cliquez sur **"Valider"**.


Et voilà, vous pouvez maintenant envoyer des SMS via OVH avec RaspiSMS !




.. _L'offre SMS OVH: https://www.ovhtelecom.fr/sms/#order-SMS
.. _La partie SMS de votre manager OVH: https://www.ovhtelecom.fr/manager/#/sms/
.. _La page de création des identifiants API de OVH: https://eu.api.ovh.com/createToken/index.cgi?GET=/sms&GET=/sms/*&POST=/sms/*&PUT=/sms/*&DELETE=/sms/*&
