.. _mms:

============================================
Envoyer des MMS
============================================

Quels téléphones peuvent envoyer/recevoir des MMS ?
===================================================
Tous les téléphones ne sont pas en mesure d'envoyer ou de recevoir des MMS. Pour savoir si un téléphone supporte ou nous l'envoi et/ou la réception de MMS, repportez vous à l'entrée dédiée au téléphone dans :ref:`la liste des téléphones<adapters>`.

Activer/désactiver le support des MMS
============================================
Le support des MMS peut être activé/désactivé. Pour cela, rendez-vous dans la partie "Réglages" de RaspiSMS, modifiez la valeur dans le champ "Support des MMS" et cliquez sur "Mettre à jour les données".

Si vous désactivez le support des MMS il sera toujours possible de recevoir des MMS, mais plus d'en envoyer.


Comment ça fonctionne ?
========================

Si les MMS sont activés, lors de la rédaction de votre SMS vous pouvez sélectionner un ou plusieurs fichiers à inclure dans votre SMS. Si vous incluez des fichiers dans un SMS, celui-ci deviens automatiquement un MMS.

Si vous ne choisissez pas un téléphone en particulier pour envoyer le message, RaspiSMS utilisera en priorité les téléphones supportant les MMS, si aucun téléphone ne supporte les MMS il utilisera un téléphone au hasard.

.. note::
    RaspiSMS n'impose pas de règles particulières quand aux types, poids ou nombres des fichiers qui peuvent êtres inclus dans un MMS. Néanmoins, selon les téléphones et les opérateurs, de telles restrictions peuvent exister. Pour cette raison nous vous conseillons de ne pas envoyer de fichiers trop volumineux, et autant que possible de vous limiter à un seul fichier par MMS.


Que se passe-t-il si le téléphone ne supporte pas les MMS ?
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
Si vous recevez un MMS sur un téléphone qui ne supporte pas la réception des MMS, le message ne sera pas reçu.

Si vous envoyez un MMS depuis un téléphone qui ne supporte pas l'envoi des MMS, le message sera envoyé sous forme d'un ou plusieurs SMS, et les médias à inclures dans le MMS (photos, vidéos, etc.) seront ajoutés au corps du SMS sous forme d'adresses URL pointant vers les fichiers sur le serveur RaspiSMS.

.. note::
    Lors de l'envoi d'un SMS contenant une plusieures adresse(s) URL vers le(s) média(s) associé(s) au SMS, la taille du SMS peut être amenée à augmenter rapidement, ce qui peut provoquer des consommations importantes de crédit.
