===============
Les Adaptateurs
===============

Objectif des adapateurs
=======================
Les adaptateurs ont pour objectif de permettre la prise en charge simple de nombreux fournisseurs de SMS.
Pour cela, les adaptateurs offrent une interface standardisée entre le code de RaspiSMS et le code des fournisseurs, lequel change d'un fournisseur à un autre.

Grâce à cette organisation il est facile d'ajouter le support de nouveaux fournisseurs de SMS, puisqu'il suffit de créer un nouvel adaptateur et qu'il n'est pas nécessaire de toucher aux autres parties de RaspiSMS.

Si vous souhaitez ajouter un nouvel adaptateur vous devez donc implémenter l'interface ``adapters/AdapterInterface``, laquelle impose un certain nombre de méthodes que nous allons voir ci-dessous.

Les méthodes de la classe ``AdapterInterface`` et leurs rôles
==============================================================

TODO
