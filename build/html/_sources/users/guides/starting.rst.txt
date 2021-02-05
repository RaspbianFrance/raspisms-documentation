.. _starting_guide:

============================================
Commencer à utiliser RaspiSMS
============================================

Ce guide a pour objectif de vous aider lors de votre première prise en main de RaspiSMS et de vous guider à travers les différentes étapes.

1. Connectez-vous à votre compte RaspiSMS
=========================================

.. raw:: html
    :file: ../../_videos/connect.html

Rendez-vous sur `la page de connection de RaspiSMS <https://app.raspisms.fr/>`_ et connectez-vous avec les identifiants que vous avez fournis lors de votre inscription en mode SaaS.

.. note::
    En auto-hébergement rendez-vous sur `la page de connection <https://127.0.0.1/raspisms>`_, les identifiants ont été générés lors de l'installation et sont disponibles dans le fichier ``/usr/share/raspisms/.credentials``.

2. Créez un premier téléphone
==============================

.. raw:: html
    :file: ../../_videos/phone_ovh_virtual.html

Afin de pouvoir recevoir et envoyer des SMS depuis l'application vous devez dans un premier temps créer un téléphone qui sera utilisé pour envoyer et recevoir des messages. Pour cela vous pouvez utiliser différents fournisseurs de services (:ref:`voir Les types de téléphones <adapters>`), mais nous vous recommandons l'utilisation :ref:`d'OVH avec un numéro virtuel <users_adapters_ovh_virtual_number>` qui, à notre sens, fournit le service le plus accessible et efficace.

3. Envoyer un premier SMS
=========================

.. raw:: html
    :file: ../../_videos/send_sms.html

Maintenant que vous avez créé un premier téléphone, il ne vous reste plus qu'à envoyer votre premier SMS. Pour cela, rendez-vous dans la partie "SMS", "Nouveau SMS" et cliquez sur le bouton "Créer un nouveau SMS".

Tapez le texte du SMS que vous souhaitez envoyer dans la partie **"Texte du SMS"**.

Laissez la date d'envoi inchangée pour envoyer le SMS immédiatement.

Dans le champs **"Numéros cibles"**, renseignez le numéro auquel vous souhaitez envoyer le SMS. Il est possible d'ajouter d'autres numéros en cliquant sur le "+" au-dessous du champs.

Enfin, dans le champs **"Numéro à employer"**, séléctionnez le nom du téléphone que vous venez de créer. Si vous laisser le champs à sa valeur par défaut "N'importe lequel", un téléphone sera choisi au hasard parmi ceux que vous avez créés (avec un seul téléphone cela n'a donc pas d'importance).

Il ne vous reste plus qu'à cliquer sur le bouton **"Enregistrer le SMS"**.

4. Vérifier le statut du SMS
============================

Vous pouvez maintenant vérifier si le SMS a bien été envoyé et éventuellement reçu, en vous rendant dans la partie **"SMS"**, **"SMS Envoyés"**.

En cliquant sur le numéro de téléphone du contact cible vous pourrez également accéder à tous les échanges avec ce numéro via l'interface de discussion.

5. Aller plus loin
==================

RaspiSMS propose de nombreuses fonctionnalités, pour en savoir plus, reportez-vous à :ref:`la documentation utilisateur<users>`.
