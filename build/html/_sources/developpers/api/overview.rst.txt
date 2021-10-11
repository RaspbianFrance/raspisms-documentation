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
- Créer une ressource (sms programmé, téléphone), verbe HTTP ``POST``.
- Supprimer une ressource (sms programmé, téléphone), verbe HTTP ``DELETE``.

.. note::
    Actuellement seul les SMS programmés et les téléphones peuvent être créés et supprimés via l'API.


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
    - **entry_type** (*str*) -- Le type de ressource à lister (``sended``, ``received``, ``scheduled``, ``contact``, ``group``, ``conditional_group``, ``phone`` ou ``media``).
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

.. literalinclude:: /_code_examples/api/delete_scheduled.curl
    :language: curl


Créer un SMS programmé -- ``POST``
============================================

Endpoints :
    - ``/api/scheduled/``

Arguments :
    - **text** (*str*) -- Le texte du SMS à envoyer.
    - **numbers** (*array | str*), ``optional`` -- Un numéro de téléphone au format international auquel envoyer le SMS ou un tableau de numéros.
    - **contacts** (*array | int*), ``optional`` -- L'ID du contact auquel envoyer le SMS ou un tableau d'IDs.
    - **groups** (*array | int*), ``optional`` -- L'ID du groupe auquel envoyer le SMS ou un tableau d'IDs.
    - **conditional_groups** (*array | int*), ``optional`` -- L'ID du groupe conditionnel auquel envoyer le SMS ou un tableau d'IDs.
    - **at** (*str*), ``optional`` -- Date à laquelle envoyer le SMS au format ``Y-m-d H:i:s``. Si non défini utilise la date actuelle.
    - **id_phone** (*str*), ``optional`` -- Identifiant du téléphone avec lequel envoyer le SMS. Si non défini utilise un téléphone au hasard.
    - **flash** (*bool*), ``optional`` -- ``TRUE`` s'il s'agit d'un SMS flash. Si non défini ``FALSE``.
    - **mms** (*bool*), ``optional`` -- ``TRUE`` si le message est un MMS. Si non défini ``FALSE``.
    - **medias** (*multipart-form file array | multipart-form file*) ``optional`` -- Un ou plusieurs fichiers à inclure dans le MMS. Toute requête contenant des medias doit être au format ``multipart/form-data``. Les medias ne seront utilisés que si le paramètre **mms** est défini à ``TRUE``. Pour envoyer plusieurs fichiers, utilisez une série de paramètres ``medias[]``.

    .. note::
        Les arguments ``numbers``, ``contacts``, ``groups`` et ``conditional_groups`` sont tous optionnels individuellement, mais vous devez nécessairement renseigner **au moins** un de ces arguments.

        Tous ces arguments peuvent être transmis avec une seule valeur ou comme un tableau de valeurs.



Réponse :
    L'ID du SMS programmé créé.

HTTP Code :
    - Success : ``200``
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

Exemple 3
~~~~~~~~~
Création d'un MMS **"Mon MMS d'exemple"** à envoyer immédiatement au **"+33612345678"** avec les fichiers **"example1.png"** et **"example2.png"**.

.. literalinclude:: /_code_examples/api/post_scheduled3.curl
    :language: curl

.. literalinclude:: /_code_examples/api/post_scheduled3_response.json
    :language: json


Supprimer un téléphone -- ``DELETE``
============================================
Endpoints :
    - ``/api/phone/{id}/``

Arguments :
    - **id** (*str*) -- L'identifiant unique du téléphone à supprimer.

Réponse :
    ``True`` on success.    

HTTP Code :
    - Success : ``204``
    - Error : ``400``

     
Exemple de requête CURL
##################################
On va supprimer le téléphone avec l'id ``42``.

.. literalinclude:: /_code_examples/api/delete_phone.curl
    :language: curl


Créer un téléphone -- ``POST``
============================================

Endpoints :
    - ``/api/phone/``

Arguments :
    - **name** (*str*) -- Le nom du téléphone (doit être unique).
    - **adapter** (*str*) -- Le nom de l'adaptateur logiciel du téléphone. C'est la classe PHP de l'adaptateur, avec son espace de nom.
    - **adapter_data** (*array*), ``optional`` -- Les données de configuration nécessaires pour le téléphone (clés API, fichier de conf, etc.). Le contenu change selon l'adaptateur logiciel du téléphone (voir `la fonction meta_data_fields <developpers_adapters_overview>`).

    .. note::
        Pour voir les données **adapter_data** à fournir, le plus simple est de lire le code de la méthode ``meta_data_fields`` de l'adaptateur logiciel pour lequel vous souhaitez créer un téléphone.



Réponse :
    L'ID du téléphone créé.

HTTP Code :
    - Success : ``200``
    - Error : ``400``


Exemple de requêtes CURL
##################################

Exemple 1
~~~~~~~~~
Création d'un téléphone **"Test phone"** avec l'adaptateur **Test**. Aucune configuration supplémentaire n'est nécessaire pour ce type d'adaptateur.

.. literalinclude:: /_code_examples/api/post_phone1.curl
    :language: curl

.. literalinclude:: /_code_examples/api/post_phone1_response.json
    :language: json


Exemple 2
~~~~~~~~~
Création d'un téléphone **"My Octopush Phone""** avec l'adaptateur **"`Octopush Shortcode <users_adapters_octopush_shortcode>`"**. Cet adaptateur nécessite de configurer le ``login`` et l'``api_key`` du compte Octopush à utiliser.

.. literalinclude:: /_code_examples/api/post_phone2.curl
    :language: curl

.. literalinclude:: /_code_examples/api/post_phone2_response.json
    :language: json
