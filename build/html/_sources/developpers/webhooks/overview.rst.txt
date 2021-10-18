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
     - Type de webhook, ``receive_sms`` ou ``send_sms``, ``send_sms_status_change``, ``inbound_call``

   * - body
     - Le corps du webhook, un objet JSON qui dépendra du type de webhook, voir `Corps des webhooks`_.


Corps des webhooks
------------------

.. list-table:: Body des webhook ``receive_sms``

   * - id
     - Identifiant unique du SMS

   * - at
     - Date de réception du SMS, format ``Y-m-d h:i:s``

   * - origin
     - Numéro de téléphone depuis lequel le SMS a été envoyé (format international)

   * - destination
     - ID du téléphone auquel le SMS a été envoyé

   * - text
     - Texte du SMS

   * - mms
     - ``1`` si le SMS est un MMS, ``0`` sinon.

   * - medias ``optional``
     - Un tableau des médias liés au message, si aucun média n'est lié, le paramètre n'existe pas.

       .. list-table:: ``Contenu d'un média``

          * - id
            - Id du média

          * - id_user
            - Id de l'utilisateur auquel appartient le média
          
          * - path
            - Chemin relatif du média depuis le dossier ``PWD_DATA`` de RaspiSMS (par défaut ``/usr/share/raspisms/data/``).
   
.. list-table:: Body des webhook ``send_sms``

   * - id
     - Identifiant unique du SMS

   * - at
     - Date d'envoi du SMS, format ``Y-m-d h:i:s``
   
   * - status
     - Statut d'envoi du SMS, ``failed`` si l'envoi a échoué, ``unknown`` sinon. En raison de la nature asynchrone du protocole SMS, on ne sait pas au moment de l'envoi si le SMS a été reçu ou non.
       
       Par conséquent, dans un webhook ``send_sms``, un statut ``failed`` indique uniquement que le serveur RaspiSMS n'a pas pu contacter le service fournisseur de SMS (API, modem, etc.), ou que celui-ci a refusé la demande d'envoi.

       Le statut ``unknown`` indique uniquement que la demande d'envoi a bien été transmise et acceptée. Il ne dit **rien** du fait que le SMS ai, été reçu ou non par le destinataire.

   * - origin
     - ID du téléphone depuis lequel le SMS a été envoyé

   * - destination
     - Numéro de téléphone auquel le SMS a été envoyé (format international)

   * - text
     - Texte du SMS

   * - mms
     - ``1`` si le SMS est un MMS, ``0`` sinon.

   * - medias ``optional``
     - Un tableau des médias liés au message, si aucun média n'est lié, le paramètre n'existe pas ou contient un tableau vide.

       .. list-table:: ``Contenu d'un média``

          * - id
            - Id du média

          * - id_user
            - Id de l'utilisateur auquel appartient le média
          
          * - path
            - Chemin relatif du média depuis le dossier ``PWD_DATA`` de RaspiSMS (par défaut ``/usr/share/raspisms/data/``).
   
   * - originating_scheduled
     - Identifiant unique du SMS programmé à l'origine de l'envoi. À noter qu'il est très probable que la ressource ``scheduled`` associée ait été détruite avant la réception du webhook.


.. list-table:: Body des webhook ``send_sms_status_change``

   * - id
     - Identifiant unique du SMS

   * - status
     - Nouveau statut du SMS. Le statut est une chaine parmis les suivantes :

       .. list-table:: ``Types de statuts``

          * - ``failed``
            - L'envoi du SMS a échoué. Soit RaspiSMS n'a pas pu transmettre la demande d'envoi au fournisseur télécom. Soit celui-ci à refusé l'envoi. Soit il n'a pas été possible de transmettre le message à l'utilisateur (numéro non attribué, réseau télécom indisponible, etc.).

          * - ``success``
            - Le SMS a bien été reçu par le destinataire. Cela ne signifie pas forcément que le SMS a été lu, mais au minimum qu'il a été reçu par le mobile. Pour qu'un statut ``success`` soit possible, il faut que le téléphone utilisé pour envoyer le SMS supporte le suivi d'envoi (voir la partie "Fonctionnalités supportées" de :ref:`la documentation sur les types de téléphones<adapters>`.).
          
          * - ``unknown``
            - RaspiSMS a pu transmettre la demande d'envoi, mais aucune autre information n'est disponible, le SMS peut aussi bien : Avoir été reçu sans accusé de réception ; Être en attente d'envoi ou de réception ; Avoir échoué sans suivi du statut d'envoi.

   

.. list-table:: Body des webhook ``inbound_call``

   * - id
     - Identifiant unique de l'appel.
   
   * - id_user
     - Identifiant unique de l'utilisateur qui a passé ou reçu l'appel.
   
   * - id_phone
     - Identifiant unique du téléphone qui a passé ou reçu l'appel.
   
   * - uid
     - Identifiant unique de l'appel sur la plateforme de l'opérateur. Utilisé notamment pour mettre à jour un appel lors de la réception d'un signal de fin.
   
   * - direction
     - Direction de l'appel, ``inbound`` si c'est un appel entrant, ou ``outbound`` si c'est un appel sortant.

   * - start
     - Date de début de l'appel, au format ``Y-m-d h:i:s``.
   
   * - end ``optional``
     - Date de fin de l'appel, au format ``Y-m-d h:i:s``. Disponible uniquement si on connais la date de fin de l'appel, sinon ``NULL``.

   * - origin ``optional``
     - Numéro de téléphone qui a passé l'appel si c'est un appel entrant, sinon ``NULL``.

   * - destination ``optional``
     - Numéro de téléphone qui a reçu l'appel si c'est un appel sortant, sinon ``NULL``.


