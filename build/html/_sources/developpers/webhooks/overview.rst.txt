.. _webhooks:

=================================
Les Webhooks
=================================


À quoi ça sert ?
=================================
Les webhooks sont un moyen de signaler un évènement à une application tierce sans que celle-ci ai besoin d'intérroger l'application qui emet l'évènement.

Pour cela, lorsqu'un évènement donné se produit, l'application émetrice emet une requête HTTP vers une URL définie et appartenant à l'application réceptrice, laquelle peut alors analyser la requête reçue et adapter son comportement en conséquence.

Créer un Webhook
========================
Pour créer un Webhook il vous suffit de vous rendre dans la partie "Webhooks", et de cliquer sur le bouton "Ajouter un webhook" pour accéder au formulaire de création des webhooks.

Vous devrez alors définir l'adresse URL à appeler lors du déclenchement du webhook. Il doit obligatoirement s'agir d'une adresse en ``http://`` ou ``https://``. Vous devrez également choisir le type de webhook à créer, c'est-à-dire le type d'évènement qui déclenchera le webhook.

Contenu de la requête
======================
Lors du déclenchement d'un webhook, la requête HTTP contient certains paramètres décrivant l'évènement survenu.

.. list-table:: Protocole de la requête

    * - Verbe HTTP
      - POST

.. list-table:: Paramètres de la requête
   :header-rows: 1

   * - Nom du paramètre
     - Description

   * - webhook_type
     - Type de webhook, ``receive_sms`` ou ``send_sms``

   * - id
     - Identifiant unique du SMS

   * - at
     - Date de réception/envoi du SMS, format ``Y-m-d h:i:s``

   * - origin
     - Numéro de téléphone depuis lequel le SMS a été envoyé (format international)

   * - destination
     - Numéro de téléphone auquel le SMS a été envoyé (format international)
   
