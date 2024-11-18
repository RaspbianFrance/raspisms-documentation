Surveillance de la fiabilité des téléphones
============================================

.. note::
   Le calcul de la fiabilité des téléphones n'a pas lieu en temps réel mais est effectué automatiquement par le système toutes les 15 minutes.


La fonctionnalité de surveillance de la fiabilité des téléphones permet d'alerter l'utilisateur lorsque l'un de ses téléphones atteint un certain taux de SMS avec un statut d'échec ou inconnu. 

Vous pouvez configurer des alertes par e-mail ou via le déclenchement d'un webhook HTTP (:ref:`voir <webhook_phone_reliability>`). La fonctionnalité offre également la possibilité de désactiver automatiquement le téléphone concerné en cas d’alerte.

Cette fonctionnalité de surveillance permet de maintenir un haut niveau de fiabilité des services SMS en identifiant rapidement les téléphones potentiellement défaillants, pour prendre des mesures correctives en temps opportun.


Configuration de la Surveillance
--------------------------------

Pour activer et configurer la surveillance de la fiabilité des téléphones :

- Rendez-vous dans la section **"Réglages"**.
- Dans l'encadré **"Supervision des téléphones"**, activez la fonctionnalité pour les statuts **"Échec"** et/ou **"Inconnu"**.

Pour chaque type de statut, vous pouvez définir les paramètres suivants :

- **Taux de SMS échoués/inconnus :** seuil de pourcentage au-delà duquel un téléphone est considéré comme non fiable.
- **Volume minimum de SMS :** nombre minimum de SMS envoyés pour valider la statistique. Cela assure que les alertes ne se déclenchent pas sur un échantillon trop faible ou à cause d'une erreur ponctuelle.
- **Période de surveillance (en minutes) :** période de temps sur laquelle le taux d’échec est calculé. Par exemple, une période de 120 minutes signifie que les statistiques se baseront sur les deux dernières heures.
- **Durée d'ignorance des SMS récents :** période pendant laquelle les SMS récents sont exclus du calcul, permettant aux statuts d'être mis à jour avant d'être pris en compte dans l’analyse.


Alertes et Actions
------------------

Dans la section des réglages de l'alerte, configurez les options suivantes :

- **Recevoir un e-mail :** cochez cette option pour être averti par e-mail dès qu'un téléphone dépasse le seuil de fiabilité.
- **Déclencher un webhook :** activez cette option pour recevoir une alerte via un webhook lorsque le seuil est atteint.
- **Désactiver automatiquement le téléphone :** si activée, cette option désactive automatiquement le téléphone concerné lorsqu’une alerte est émise.

