.. _adapters:

=======================
Les types de téléphones
=======================

Pourquoi plusieurs types de téléphones ?
========================================
Les différents types de téléphones ont pour objectif de permettre la prise en charge simple de nombreux fournisseurs de SMS et modes d'envoi.

En tant qu'utilisateur, le support de différents types de téléphones, et donc de différents fournisseurs de SMS vous permet d'utiliser RaspiSMS avec l'offre commerciale la plus adaptée à vos besoins.

Par ailleurs, en proposant une intégration unifiée au sein de l'application de différents fournisseurs de SMS, RaspiSMS vous permet de ne pas être dépendant d'un seul vendeur de SMS et de pouvoir changer de solution d'envoi facilement, sans avoir à modifier vos habitudes ou votre système d'information.

Si vous êtes développeur, vous pouvez trouvez plus d'information sur la création d'un nouveau type dé téléphone dans :ref:`la partie "Créer un nouveau type de téléphone"<developpers_adapters_overview>`.

Actuellement les types de téléphones disponibles sont les suivants :

Types de téléphones disponibles
===============================
.. toctree::
    :maxdepth: 1

    raspisms_relay
    ovh_virtual_number
    ovh_shortcode
    twilio_virtual_number
    octopush_virtual_number
    octopush_shortcode
    odyssey_messaging
    gammu
    kannel
