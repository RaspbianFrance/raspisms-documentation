.. _api:

=================================
Utiliser l'API HTTP
=================================


À quoi ça sert ?
=================================
L'API HTTP de RaspiSMS vous permet d'intégrer facilement les fonctionnalitées d'envoi et de récéption de SMS de RaspiSMS au sein d'une autre application. Vous pouvez par exemple utiliser l'API de RaspiSMS afin d'envoyer des SMS de confirmation ou d'alerte depuis une application tierce.


Organisation de l'API
======================

Fonctionnalités
###############
L'API de RaspiSMS est découpée en trois principales fonctionnalités, chacune d'entre elle correspondant à un verbe HTTP dédié :

- Lister des ressources (sms envoyés, reçus et programmés, contacts, etc.), verbe HTTP ``GET``.
- Créer une ressource (sms programmé), verbe HTTP ``POST``.
- Supprimer une ressource (sms programmé), verbe HTTP ``DELETE``.

.. note::
    Actuellement seul les SMS programmés peuvent être créés et supprimés.


Format des réponses
###################
Toutes les réponses des API sont un tableau associatif au format JSON. Les réponses respectent toutes le format ci-dessous.

.. list-table:: Format des réponses
   :header-rows: 1

   * - Clé
     - Description

   * - error
     - Code d'erreur, ``0`` si pas d'erreur.
       
   * - message
     - Message décrivant l'erreur si une erreur est survenue. Sinon ``NULL``.
       
   * - response
     - Réponse de la requête, différent pour chaque requête.
       
   * - next
     - Lien vers la page suivante s'il reste des résultats à affichées pour la requête. Sinon ``NULL``.
       
   * - prev
     - Lien vers la page précédente si la requête actuelle utilise la pagination. Sinon ``NULL``.

Codes d'erreurs
~~~~~~~~~~~~~~~~
L'API utilise les codes d'erreurs suivants.

.. list-table:: Format des réponses
   :header-rows: 1

   * - Code
     - Description

   * - 0
     - Pas d'erreur.
       
   * - 1
     - Identifiant invalide (vous devez fournir une clé API valide).
       
   * - 2
     - Un paramètre non valide a été fourni.
       
   * - 4
     - Un paramètre manquant n'a pas été fourni.
       
   * - 8
     - Impossible de créer la ressource demandée.

   * - 16
     - Le compte lié à la clé API est suspendu.

   * - 32
     - Impossible de supprimer la ressource indiquée.


Pagination
~~~~~~~~~~
Pour les requêtes susceptibles d'employer de la pagination (c'est-à-dire les requêtes de type liste), les résultats sont renvoyés par groupe de 25. Si des résultats supplémentaires sont disponibles un champ ``next`` avec l'URL à intérroger pour obtenir les résultats suivants est indiqué dans la réponse. Sinon le champ est défini à ``NULL``.


Obtenir et utiliser une clé API
================================
Pour effectuer une requête sur l'API, vous devez fournir une clé unique permettant de vous authentifier.

.. warning::
    Votre clé API est un identifiant à part entière, vous ne devez en aucun cas la rendre publique !

    Si cette clé a été rendue publique vous devriez l'invalider et en générer une nouvelle via le bouton "Générer une nouvelle clef API" dans la partie "Mon profil" de RaspiSMS.

Trouver la clé API
##################
Vous pouvez trouver votre clé API dans la partie "Mon profil" de RaspiSMS, dans l'encadré "Mes données".

Transmettre la clé API
######################
Vous devez transmettre votre clé API avec chaque requête sans quoi celle-ci sera rejetée. Pour cela il vous suffit de renseigner votre clé API sous la forme d'un paramètre de ``GET`` ou ``POST`` nommé ``api_key``, ou bien d'un header HTTP nommé ``X-Api-Key``.


Lister des ressources -- ``GET``
=================================

Endpoints :
    - ``/api/list/{entry_type}/``
    - ``/api/list/{entry_type}/{page}/``

Arguments :
    - **entry_type** (*str*) -- Le type de ressource à lister (``sended``, ``received``, ``scheduled``, ``contact``, ``group``, ``conditional_group`` ou ``phone``).
    - **page** (*int*), ``optional`` -- Numéro de page à utiliser, si non défini ``0``.

Réponse :
    Une collection JSON des ressources demandées.

HTTP Code :
    - Success : ``200``
    - Error : ``404``

Exemple de requête CURL
#################################
On va lister les 25 premiers contacts.

.. literalinclude:: /_code_examples/api/list.curl
    :language: curl

.. literalinclude:: /_code_examples/api/list_response.json
    :language: json


Supprimer un SMS programmé -- ``DELETE``
============================================
Endpoints :
    - ``/api/scheduled/{id}/``

Arguments :
    - **id** (*str*) -- L'identifiant unique du SMS programmé à supprimer.

Réponse :
    ``True`` on success.    

HTTP Code :
    - Success : ``204``
    - Error : ``400``

     
Exemple de requête CURL
##################################
On va supprimer le SMS programmé avec l'id ``13``.

.. literalinclude:: /_code_examples/api/delete.curl
    :language: curl


Créer un SMS programmé -- ``POST``
============================================

Endpoints :
    - ``/api/scheduled/``

Arguments :
    - **text** (*int*) -- Le texte du SMS à envoyer.
    - **numbers** (*array | str*), ``optional`` -- Un numéro de téléphone au format international auquel envoyer le SMS ou un tableau de numéros.
    - **contacts** (*array | int*), ``optional`` -- L'ID du contact auquel envoyer le SMS ou un tableau d'IDs.
    - **groups** (*array | int*), ``optional`` -- L'ID du groupe auquel envoyer le SMS ou un tableau d'IDs.
    - **conditional_groups** (*array | int*), ``optional`` -- L'ID du groupe conditionnel auquel envoyer le SMS ou un tableau d'IDs.
    - **at** (*str*), ``optional`` -- Date à laquelle envoyer le SMS au format ``Y-m-d H:i:s``. Si non défini utilise la date actuelle.
    - **id_phone** (*str*), ``optional`` -- Identifiant du téléphone avec lequel envoyer le SMS. Si non défini utilise un téléphone au hasard.
    - **flash** (*bool*), ``optional`` -- Défini s'il s'agit d'un SMS flash. Si non défini ``FALSE``.

    .. note::
        Les arguments ``numbers``, ``contacts``, ``groups`` et ``conditional_groups`` sont tous optionnels individuellement, mais vous devez nécessairement renseigner **au moins** un de ces arguments.

        Tous ces arguments peuvent être transmis avec une seule valeur ou comme un tableau de valeurs.


Réponse :
    L'ID du SMS programmé créé.

HTTP Code :
    - Success : ``204``
    - Error : ``400``


Exemple de requêtes CURL
##################################

Exemple 1
~~~~~~~~~
Création d'un SMS **"Mon SMS d'exemple"** envoyé immédiatement au numéro **"+33612345678"**.

.. literalinclude:: /_code_examples/api/post_scheduled1.curl
    :language: curl

.. literalinclude:: /_code_examples/api/post_scheduled1_response.json
    :language: json


Exemple 2
~~~~~~~~~
Création d'un SMS **"Mon SMS d'exemple"** à envoyer le **"Jeudi 17 juin 2020 à 15 heures, 30 minutes, 25 secondes"**, aux contacts avec l'ID ``11`` et ``12``, avec le téléphone avec l'ID ``3``.

.. literalinclude:: /_code_examples/api/post_scheduled2.curl
    :language: curl

.. literalinclude:: /_code_examples/api/post_scheduled2_response.json
    :language: json
