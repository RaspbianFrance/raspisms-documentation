===============
Les Adaptateurs
===============

Objectif des adapateurs
=======================
Les adaptateurs ont pour objectif de permettre la prise en charge simple de nombreux fournisseurs de SMS.
Pour cela, les adaptateurs offrent une interface standardisée entre le code de RaspiSMS et le code des fournisseurs, lequel change d'un fournisseur à un autre.

Grâce à cette organisation il est facile d'ajouter le support de nouveaux fournisseurs de SMS, puisqu'il suffit de créer un nouvel adaptateur et qu'il n'est pas nécessaire de toucher aux autres parties de RaspiSMS.

Actuellement les adaptateurs disponibles sont les suivants :


.. toctree::
    :maxdepth: 1
    :caption: Adaptateurs disponibles

    ovh_virtual_number
    ovh_shortcode
    twilio_virtual_number
    octopush_virtual_number
    octopush_shortcode
    gammu
