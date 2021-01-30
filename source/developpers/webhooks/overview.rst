.. _webhooks:

=================================
Les Webhooks
=================================


À quoi ça sert ?
=================================
Les webhooks sont un moyen de signaler un évènement à une application tierce sans que celle-ci ait besoin d'intérroger l'application qui émet l'évènement.

Pour cela, lorsqu'un évènement donné se produit, l'application émetrice émet une requête HTTP vers une URL définie et appartenant à l'application réceptrice, laquelle peut alors analyser la requête reçue et adapter son comportement en conséquence.

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

   * - webhook_timestamp
     - Timestamp auquel le webhook a été généré, vous pouvez vous en servir pour ignorer les webhook trop anciens
   
   * - webhook_random_id
     - Identifiant aléatoire du webhook, basé sur le timestamp et une chaine aléatoire. L'unicité de l'identifiant n'est pas garantie mais très hautement probable. Vous pouvez vous en servir sans risque pour identifier un webhook de façon unique.

   * - webhook_signature
     - Signature du webhook. Il s'agit d'une signature ``HMAC SHA-256`` du ``webhook_random_id``, signé avec votre clé API. Vous pouvez utiliser cette signature pour vérifier l'authenticité du webhook reçu.

   * - webhook_type
     - Type de webhook, ``receive_sms`` ou ``send_sms``

   * - id
     - Identifiant unique du SMS

   * - at
     - Date de réception/envoi du SMS, format ``Y-m-d h:i:s``

   * - origin
     - Numéro de téléphone (si `received_sms`) ou ID du téléphone (si `send_sms`) depuis lequel le SMS a été envoyé (format international)

   * - destination
     - Numéro de téléphone (si `send_sms`) ou ID du téléphone (si `received_sms`) auquel le SMS a été envoyé (format international)
   
