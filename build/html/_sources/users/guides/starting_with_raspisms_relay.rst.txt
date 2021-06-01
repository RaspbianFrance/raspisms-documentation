.. _starting_with_raspisms_relay_guide:

========================================================
Commencer à utiliser RaspiSMS avec un téléphone Android.
========================================================

.. note::
    L'utilisation d'un téléphone Android et de l'application RaspiSMS Relay n'est possible qu'en mode SaaS. Cette fonctionnalitée n'est pas disponible en auto-hébergement.

Ce guide a pour objectif de vous aider lors de votre première prise en main de RaspiSMS avec un téléphone Android.

.. raw:: html
    :file: ../../_videos/raspisms-relay-demo.html

1. Utiliser un téléphone Android dans RaspiSMS, comment ça fonctionne ?
========================================================================

En utilisant l'offre SaaS de RaspiSMS et l'application RaspiSMS Relay, vous pouvez configurer un téléphone Android afin d'utiliser un abonnement mobile classique pour l'envoi et la réception des SMS.

L'application web RaspiSMS vous permet de créer vos SMS en ligne en profitant de nombreuses fonctionnalités, comme la gestion de contacts et les listes de diffusion. Puis l'application mobile RaspiSMS Relay reçoit la demande d'envoi du serveur et utilise la carte SIM du téléphone le réseau mobile opérateur pour envoyer le message au destinataire.

.. image:: ../../_static/img/chart-raspisms-relay-horizontal.svg
  :width: 800
  :alt: Schéma du fonctionnement de RaspiSMS Relay.

2. Installez l'application RaspiSMS Relay sur votre téléphone.
================================================================

Rendez-vous sur `la page playstore de l'application RaspiSMS Relay <https://play.google.com/store/apps/details?id=fr.raspisms.raspismsrelay>`_ et téléchargez l'application.


3. Configurez le relais RaspiSMS.
=================================

Lancez l'application RaspiSMS Relay, validez les demandes d'autorisation pour la lecture et l'envoi des SMS. Ces autorisations sont nécessaires au fonctionnement de l'application.

Une fois les autorisations données, cliquez sur "Configurer le relais", puis suivez les instructions de l'application.

3.1. Connectez-vous à votre compte RaspiSMS.
--------------------------------------------

Connectez-vous à votre compte RaspiSMS, et rendez-vous dans votre compte pour trouvez votre clé API. Une fois votre clé API trouvée, recopiez là dans l'application RaspiSMS Relay puis cliquez sur le bouton "Continuer".

3.2. Définissez le nom du téléphone à créer dans RaspiSMS.
----------------------------------------------------------

Dans l'application RaspiSMS Relay, rentrez un nom personnalisé pour le téléphone à créer dans RaspiSMS, ou bien utilisez le nom par défaut, puis cliquez sur le bouton "Continuer".

3.3. Choisissez si le téléphone doit relayer les SMS reçus.
-----------------------------------------------------------

Choisissez si le téléphone doit relayer au serveur les SMS qu'il a reçu. Si vous choisissez de relayer les SMS reçus, tous les messages qui seront reçus par le téléphones seront transférés au serveur. Cliquez sur le bouton "Continuer".


4. Envoyez votre premier SMS depuis RaspiSMS.
==========================================================

Maintenant que vous avez configuré le relais RaspiSMS, il ne vous reste plus qu'à envoyer votre premier SMS. Pour cela, rendez-vous dans la partie "SMS", "Nouveau SMS" et cliquez sur le bouton "Créer un nouveau SMS".

Tapez le texte du SMS que vous souhaitez envoyer dans la partie **"Texte du SMS"**.

Laissez la date d'envoi inchangée pour envoyer le SMS immédiatement.

Dans le champs **"Numéros cibles"**, renseignez le numéro auquel vous souhaitez envoyer le SMS. Il est possible d'ajouter d'autres numéros en cliquant sur le "+" au-dessous du champs.

Enfin, dans le champs **"Numéro à employer"**, séléctionnez le nom du téléphone que vous avez choisi lors de la configuration du relais. Si vous laisser le champs à sa valeur par défaut "N'importe lequel", un téléphone sera choisi au hasard parmi ceux que vous avez créés (avec un seul téléphone cela n'a donc pas d'importance).

Il ne vous reste plus qu'à cliquer sur le bouton **"Enregistrer le SMS"**.

5. Vérifier le statut du SMS.
=============================

Vous pouvez maintenant vérifier si le SMS a bien été envoyé et éventuellement reçu par le destinataire, en vous rendant sur l'inteface en ligne, dans la partie **"SMS"**, **"SMS Envoyés"**.

Si vous avez configuré le relais pour relayer les SMS reçus, vous pouvez vérifier son bon fonctionnement en vous envoyant un SMS à vous même, et en vous rendant dans l'interface de RaspiSMS, dans la partie **"SMS"**, **"SMS Reçus"**.

6. Vérifier le statut du relais.
================================

Depuis le téléphone équipé de l'application, vous pouvez également voir le statut du relais en démarrant l'application.

La page principale affiche le statut du relais ainsi que les derniers messages relayés (depuis le cloud vers un destinataire et depuis un mobile vers le cloud) et leur statut.

Vous pouvez quitter l'application RaspiSMS Relay sans risques, les messages continuerons d'êtres relayés. Le seul pré-requis est qu'un réseau mobile soit disponible pour envoyer et recevoir les messages, et qu'un réseau internet soit disponible pour synchroniser les données avec le serveur centrale de RaspiSMS.

.. warning::
    Sur certains téléphones ajoutant des surcouches à Android, fermer une application dans la liste des applications ouvertes déclenche abusivement une fermeture forcée de l'application. L'application est totalement arrêtée, et les tâches de fond de ces applications (par exemple recevoir ou envoyer des SMS) peuvent êtres stoppées.

    Nous vous recommandons donc de ne pas fermer totalement l'application dans la liste des applications ouvertes.

    Pour plus d'informations : `<https://dontkillmyapp.com/>`_

En cas de perte de réseau, les messages à envoyer sont mis en attente. En cas d'echec lors des tentatives d'envoi ou de synchronisation, jusqu'à cinq tentatives seront effectuées, à des intervales de plus en plus grands. En cas d'echec définitif, le statut du message est mis à jour localement et sur le serveur dès que possible.


7. Désactiver les limites de SMS sur Android.
================================================

Android intègre nativement une limitation du nombre de SMS qu'une application peut envoyer sur une période donnée. Par défaut cette limite est fixée à 30 SMS sur une période de 30 minutes. Selon le constructeur et le modèle du téléphone, cette limite peut varier.

Si vous devez envoyer plus de 30 SMS en 30 minutes, vous devez donc désactiver cette limite. Certains constructeurs peuvent intégrer une option dans les réglages générals du téléphone pour vous permettre de désactiver ou modifier cette limite, mais cela reste rare. En l'absence d'une telle option la seule solution est alors d'activer `le déboggage USB du téléphone <https://developer.android.com/studio/debug/dev-options>`_, de le connecter à un ordinateur équipé du logiciel `Android Debug Bridge <https://developer.android.com/studio/command-line/adb>`_ et de lancer la ligne de commande ci-dessous :

.. literalinclude:: /_code_examples/adb/disable_sms_limit.sh
    :language: bash

Cette commande va modifier la limite de SMS envoyés à 1000000 toutes les 30 minutes.


8. Aller plus loin
==================

RaspiSMS propose de nombreuses fonctionnalités, pour en savoir plus, reportez-vous à :ref:`la documentation utilisateur<users>`.
