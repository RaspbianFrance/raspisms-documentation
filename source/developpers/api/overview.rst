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
    - ``/api/list/{entry_type}/after/{after_id}/``
    - ``/api/list/{entry_type}/before/{before_id}/``

Arguments :
    - **entry_type** (*str*) -- Le type de ressource à lister (``sended``, ``received``, ``scheduled``, ``contact``, ``group``, ``conditional_group``, ``phone``, ``phone_group`` ou ``media``).
    - **page** (*int*), ``optional`` -- Numéro de page à utiliser, si non défini ``0``.
    - **after_id** (*int*), ``optional`` -- Récupère uniquement les entrées avec un ID > after_id, alternative au paramètre page et offrant de meilleures performances.
    - **before_id** (*int*), ``optional`` -- Récupère uniquement les entrées avec un ID < before_id, alternative au paramètre page et offrant de meilleures performances.
    

Réponse :
    Une collection JSON des ressources demandées.

HTTP Code :
    - Success : ``200``
    - Error : ``404``


Envoyer un SMS à un seul numéro -- ``GET``
============================================

Endpoints :
    - ``/api/send-sms/``

Arguments :
    - **text** (*str*) -- Le texte du SMS à envoyer.
    - **to** (*str*), ``optional`` -- Un numéro de téléphone au format international auquel envoyer le SMS.
    - **id_phone** (*str*), ``optional`` -- Identifiant du téléphone avec lequel envoyer le SMS. Si non défini utilise un téléphone au hasard.
    
    .. note::
        Cette méthode est la façon la plus simple d'envoyer un SMS, mais elle limite les fonctionnalités. Pour une utilisation plus avancée, reportez-vous à la `création d'un SMS programmé <#creer-un-sms-programme-post>`_.


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

.. literalinclude:: /_code_examples/api/get_send-sms.curl
    :language: curl

.. literalinclude:: /_code_examples/api/get_send-sms.json
    :language: json


Obtenir le nombre de SMS envoyés -- ``GET``
============================================

Endpoints :
    - ``/api/usage``

Arguments :
    - **start** (*str*), ``optional`` -- La date à patir de laquelle lire le nombre de SMS envoyés, au format ``Y-m-d H:i:s``. Si non défini pas de date de début.
    - **end** (*str*), ``optional`` -- La date jusqu'à laquelle lire le nombre de SMS envoyés, au format ``Y-m-d H:i:s``. Si non défini pas de date de fin.
    - **tag** (*str*), ``optional`` -- Filtre les SMS pour garder uniquement les SMS envoyés avec un tag correspondant. Le tag sera comparé via une requête SQL ``LIKE``. Vous pouvez donc cherchez des tags partiels en utilisant les caractères ``%`` et ``_``. Si non défini tous les tags seront conservés.

Réponse :
    Un objet JSON avec le nombre total de SMS et le nombre de SMS par téléphone avec en clé l'ID du téléphone.

HTTP Code :
    - Success : ``200``
    - Error : ``404``

Exemple de requête CURL
#################################
On va récupérer le nombre de SMS envoyés entre le 1 février 2023 et le 28 février 2023, avec un tag commençant par ``campaign``.

.. literalinclude:: /_code_examples/api/usage.curl
    :language: curl

.. literalinclude:: /_code_examples/api/usage_response.json
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
    - **numbers** (*array | str*), ``optional`` -- Un numéro de téléphone au format international auquel envoyer le SMS ou un tableau de numéros. Pour chaque numéro il est possible de passer à la place un tableau avec les clés ``number`` et ``data``, où ``number`` est le numéro cible, et ``data`` une chaine JSON de données au format ``clé => valeur`` (voir `contacts enrichies <extended_contacts>`) à utiliser au sein du message `via le templating <templating>`.
    - **contacts** (*array | int*), ``optional`` -- L'ID du contact auquel envoyer le SMS ou un tableau d'IDs.
    - **groups** (*array | int*), ``optional`` -- L'ID du groupe auquel envoyer le SMS ou un tableau d'IDs.
    - **conditional_groups** (*array | int*), ``optional`` -- L'ID du groupe conditionnel auquel envoyer le SMS ou un tableau d'IDs.
    - **at** (*str*), ``optional`` -- Date à laquelle envoyer le SMS au format ``Y-m-d H:i:s``. Si non défini utilise la date actuelle.
    - **id_phone** (*str*), ``optional`` -- Identifiant du téléphone avec lequel envoyer le SMS. Si non défini et que ``id_phone_group`` est aussi non défini, utilise un téléphone au hasard.
    - **id_phone_group** (*str*), ``optional`` -- Identifiant du groupe de téléphone avec lequel envoyer le SMS.Si non défini et que ``id_phone`` est aussi non défini,  utilise un téléphone au hasard.
    - **flash** (*bool*), ``optional`` -- ``TRUE`` s'il s'agit d'un SMS flash. Si non défini ``FALSE``.
    - **mms** (*bool*), ``optional`` -- ``TRUE`` si le message est un MMS. Si non défini ``FALSE``.
    - **tag** (*string*), ``optional`` -- Un tag qui sera associé à tous les SMS. Utile pour associer une campagne à un identifiant unique de votre système. Si non défini ``NULL``.
    - **medias** (*multipart-form file array | multipart-form file*) ``optional`` -- Un ou plusieurs fichiers à inclure dans le MMS. Toute requête contenant des medias doit être au format ``multipart/form-data``. Les medias ne seront utilisés que si le paramètre **mms** est défini à ``TRUE``. Pour envoyer plusieurs fichiers, utilisez une série de paramètres ``medias[]``.
    - **numbers_csv** (*multipart-form file*) ``optional`` -- Un fichier CSV de numéros de téléphones qui auxquels seront envoyer le SMS. Le fichier doit respecter le format décrit dans la page sur `l'envoi de SMS à un fichier CSV <sms_csv>`

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

Exemple 4
~~~~~~~~~
Création d'un SMS à envoyer immédiatement aux numéros listés dans le fichier CSV **"sms_csv.csv"** ci-dessous.

.. literalinclude:: /_code_examples/sms/sms_csv.csv
    :language: none

.. literalinclude:: /_code_examples/api/post_scheduled4.curl
    :language: curl

.. literalinclude:: /_code_examples/api/post_scheduled4_response.json
    :language: json

Exemple 5
~~~~~~~~~
Création d'un SMS **"Bonjour {{contact.firstname}}"** envoyé immédiatement aux numéro **"+33612345678"** et **"+33612345679"** ou ``{{contact.firstname}}`` sera remplacé par les données liées aux numéros, à savoir ``John`` et ``Jane``.

.. literalinclude:: /_code_examples/api/post_scheduled5.curl
    :language: curl

.. literalinclude:: /_code_examples/api/post_scheduled5_response.json
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
    - **priority** (*int*), ``optional`` -- La priorité d'utilisation du téléphone (:ref:`voir <users_phones_priority>`). Si non défini ``0``.
    - **limits** (*array*), ``optional`` -- Les limites d'envois pour le téléphone (:ref:`voir <users_phones_limits>`). Si non défini ``[]``. C'est un tableau de limites, ou chaque limite est représentée par un tableau avec les clés : 
    
      - **volume** (*int*) -- Le nombre de SMS max sur la période.
      - **startpoint** (*str*) -- Une chaine de caractère représentant `une date relative PHP <https://www.php.net/manual/en/datetime.formats.relative.php>`_ qui sera utilisée comme date de début à partir de laquelle calculer le nombre de SMS envoyés par rapport à la limite (valeures conseillées : ``today``, ``-24 hours``, ``this week midnight``, ``-7 days``, ``this week midnight -1 week``, ``-14 days``, ``this month midnight``, ``-1 month``, ``-28 days``, ``-30 days``, ``-31 days``).
    
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


Exemple 3
~~~~~~~~~
Création d'un téléphone **"Test phone"** avec l'adaptateur **Test**, une priorité de ``2``, une limite de ``100`` sms par jour et une deuxième limite de ``500`` sms dans la semaine.

.. literalinclude:: /_code_examples/api/post_phone3.curl
    :language: curl

.. literalinclude:: /_code_examples/api/post_phone3_response.json
    :language: json



Mettre à jour le status d'un téléphone -- ``POST``
==================================================
Cette requête un peu particulière vous permet de forcer la vérification et la mise à jour du status d'un téléphone.

Endpoints :
    - ``/api/phone/{id}/status/``

Arguments :
    - **id** (*int*) -- L'identifiant du téléphone dont on veut mettre à jour le status.


Réponse :
    Le nouveau status du téléphone parmis ``available``, ``unavailable``, ``no_credit``, ``limit_reached``.

HTTP Code :
    - Success : ``200``
    - Error : ``400``


Exemple de requêtes CURL
##################################

Mise à jour du status du téléphone avec l'id ``50``.

.. literalinclude:: /_code_examples/api/update_phone_status.curl
    :language: curl

.. literalinclude:: /_code_examples/api/update_phone_status_response.json
    :language: json


Obtenir le nombre de SMS envoyés par un téléphone et leur status -- `GET`
=========================================================================

Endpoints :
    - ``/api/stats/sms-status``

Arguments :
    - **start** (*str*) -- La date à patir de laquelle lire le nombre de SMS envoyés, au format ``Y-m-d H:i:s`` (inclus).
    - **end** (*str*) -- La date jusqu'à laquelle lire le nombre de SMS envoyés, au format ``Y-m-d H:i:s`` (non inclus).
    - **id_phone** (*int*), ``optional`` -- Si présent retourne uniquement les infos sur le SMS envoyés par ce téléphone. Si non défini tous les téléphones seront inclus.

Réponse :
    Un objet JSON avec les SMS envoyés groupés par date, status et ID du téléphone.

HTTP Code :
    - Success : ``200``
    - Error : ``404``

Exemple de requête CURL
#################################
On va récupérer le nombre de SMS envoyés entre le 1 février 2023 et le 2 février 2023 pour le téléphone avec l'ID ``1``.

.. literalinclude:: /_code_examples/api/sms_status.curl
    :language: curl

.. literalinclude:: /_code_examples/api/sms_status_response.json
    :language: json

