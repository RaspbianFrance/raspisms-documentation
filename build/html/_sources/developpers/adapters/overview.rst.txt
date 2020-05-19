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

Les méthodes ``meta_*``
'''''''''''''''''''''''
Les méthodes commençant par ``meta_`` permettent de définir les capacités et les informations relatives à l'adaptateur, tel que son nom, les champs supplémentaires permettant la configuration de l'adaptateur, ou encore les fonctionnalités disponibles.
Pour cela ces fonctions doivent retourner des chaines de caractères ou des tableaux. Ces méthodes doivent être envisagées comme de simple propriétés et à ce titre sont toutes statiques. L'objectif est de forcer l'implémentation des méthodes par les classes implémentant l'interface, car PHP ne supporte pas les propriétés abstraites.

meta_classname
""""""""""""""""""
Nom de la classe de l'adaptateur, il devrait uniquement retourner ``__CLASS__``.

meta_uid
""""""""""""
Le nom unique de l'adaptateur, en **snake_case**. Par exemple, si l'adaptateur s'appel **"GammuAdapter"**, un bon ``meta_uid`` est **"gammu_adapter"**.

meta_name
"""""""""""""
Le nom de l'adaptateur tel qu'il sera affiché à l'utilisateur. Par exemple, si vous créez un adaptateur pour un service externe example-sms.com, vous pourriez utiliser "Example SMS".

meta_description
""""""""""""""""""""
La description de l'adaptateur et du service qu'il permet d'utiliser. Cette description sera affichée à l'utilisateur.

meta_datas_fields
"""""""""""""""""""""
Les champs à afficher lors de la création du téléphone et permettant de configurer l'adaptateur, par exemple renseigner les clefs API du service implémenté.

La fonction retourne un tableau où chaque ligne correspond à un ``input`` qui devra être affiché sur la page de création du téléphone.


Chaque ligne est donc elle même un tableau avec les clefs suivantes:
 - ``name`` La clé qui sera utilisé pour accéder à la propriété. Il doit s'agir d'un nom valide pouvant servir de clé à un tableau PHP. Le nom sera aussi utilisé comme nom de l'input HTML.
 - ``title`` Le titre qui sera affiché au dessus de l'input.
 - ``description`` La description qui sera affichée à l'utilisateur avant l'input. Cette description devrait permettre à l'utilisateur de comprendre l'utilité du réglage.
 - ``required`` Booléen permettant de définir si le champs est obligatoire. Si ``TRUE`` l'utilisateur devra obligatoirement remplir le champs pour pouvoir créer un téléphone avec cet adaptateur.
 - ``number`` Booléen permettant de définir si le champs est un numéro de téléphone nécessitant l'utilisation d'un input dédié avec internationalisation des numéros. Par défaut ``FALSE``.


meta_support_read
"""""""""""""""""""""
Booléen définissant si l'adaptateur supporte la lecture directe des SMS reçus, par exemple via un appel à une API, par opposition à la lecture des SMS par appel d'une callback HTTP. Si ``TRUE`` alors ``meta_support_reception`` doit obligatoirement être à ``FALSE``.

À vous de choisir selon les capacités du service implémenté la solution la plus adaptée entre callback HTTP et lecture directe, les deux ayant leurs avantages et leurs inconvénients.

meta_support_reception
""""""""""""""""""""""""""
Booléen définissant si l'adaptateur supporte la réception de SMS par appel à une callback HTTP. Si ``TRUE`` alors ``meta_support_read`` doit être à ``FALSE``.

meta_support_status_change
""""""""""""""""""""""""""""""
Booléen définissant si l'adaptateur supporte la mise à jour du status d'un SMS par l'appel à une callback HTTP.

meta_support_flash
""""""""""""""""""""""
Booléen définissant si l'adaptateur supporte les SMS flash.


Le constructeur de l'adaptateur
''''''''''''''''''''''''''''''''
.. code-block::

    public function __construct(stringdatas)

La fonctions prend en argument une chaine de caractère ``$datas`` qui est une chaîne JSON contenant les données de configuration de l'adaptateur. Il s'agit des données indiqués via les champs générés en utilisant ``meta_datas_fields``.

Àœ vous de décoder ces données et de les utiliser pour générer un client API, un fichier de configuration ou autre.


Les méthodes d'envoi et de lecture
'''''''''''''''''''''''''''''''''''''''
Ces méthodes permettent l'envoi de SMS et la lecture des SMS reçus.

Envoi d'un SMS
""""""""""""""
.. code-block::

    public function send (stringdestination, stringtext, boolflash = false)

La méthode est appelée à chaque SMS envoyé via l'adaptateur et prend trois arguments, détaillés ci-après :
 - ``$destination`` Le numéro auquel envoyer le SMS.
 - ``$text`` Le corps du SMS à envoyer.
 - ``$flash`` Défini si le SMS envoyé doit être un SMS flash. Par défaut ``FALSE``.


La fonction doit retourner un tableau avec trois clés :
 - ``error`` Un booléen à ``TRUE`` si une erreure est survenue et ``FALSE`` sinon.
 - ``error_message`` Le message d'erreur en cas d'echec, ou ``NULL`` en cas de succés.
 - ``uid`` L'identifiant unique du SMS envoyé au sein de la plateforme implémentée par l'adaptateur. Cet identifiant doit permettre de retrouver le SMS sur la plateforme, par exemple lors de la réception d'un appel HTTP de callback indiquant la mise à jour du status d'un SMS.


Lecture d'un SMS
""""""""""""""""
.. code-block::

    public function read (): array

La méthode appelée pour lire les SMS reçus. Cette méthode est appelée **très** souvent (environ 2 fois par seconde), à vous de vous assurez que cela n'entrainera pas de dépassement des capacité du service implémenté, et potentiellement de mettre en place des mécanisme de temporisation.

La fonction retourne un tableau tel que suit :
 - ``error`` Un booléen à ``TRUE`` si une erreure est survenue et ``FALSE`` sinon.
 - ``error_message`` Le message d'erreur en cas d'echec, ou ``NULL`` en cas de succés.
 - ``smss`` Un tableau avec les SMS reçus. Chaque ligne est un SMS représenté lui même par un tableau avec les clés suivantes :
    - ``at`` La date de réception du SMS au format ``Y-m-d H:i:s``.
    - ``text`` Le corps du SMS.
    - ``origin`` Le numéro de l'émetteur du SMS, au format international (ex : +33612345678).


La méthode test.
"""""""""""""""""
.. code-block::

    public function test (): bool

Cette méthode est appelée au moment de la création d'un nouveau téléphone utilisant l'adaptateur pour vérifier que la configuration c'est passée comme prévue. La méthode devrait par exemple être utilisée pour vérifier que les identifiants API fournis sont valides.
La méthode doit retournée ``TRUE`` en cas de succès et ``FALSE`` en cas d'échec, auquel cas l'utilisateur se verra indiqué que la configuration fournie n'est pas valide.

Les méthodes de callback
""""""""""""""""""""""""
Ces méthodes sont appelées par RaspiSMS lors de la réception d'une requête HTTP de callback concernant cet adaptateur.

Mise à jour du status d'un SMS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block::

    public static function status_change_callback()

La méthode est appelée lors de la réception d'un appel HTTP indiquant la mise à jour du status d'un SMS.

La méthode doit retournée ``FALSE`` si une erreure survient, ou un tableau en cas de succès avec:
 - ``uid`` L'identifiant unique du SMS au sein de la plateforme implémentée.
 - ``status`` Le nouveau status du SMS, soit ``\models\Sended::STATUS_UNKNOWN`` pour un status inconnu, ``\models\Sended::STATUS_DELIVERED`` pour un SMS reçus par le destinataire, ou ``\models\Sended::STATUS_FAILED`` si l'envoi du SMS à échoué.



Réception d'un SMS
~~~~~~~~~~~~~~~~~~~
.. code-block::

    public static function reception_callback() : array

La méthode est appelée lors e la réception d'un appel HTTP indiquant la réception d'un SMS.

La méthode doit transformé les données transmises par la plateforme implémentée en un SMS dans un format adapté à RaspiSMS. Pour cela elle doit retourner un tableau avec :
 - ``error`` ``TRUE`` en cas d'erreur, sinon ``FALSE``.
 - ``error_message`` Un message d'erreur en cas d'erreur, sinon ``NULL``.
 - ``sms`` Un tableau représentant le SMS reçu
    - ``at`` Date de réception du SMS au format ``Y-m-d H:i:s``
    - ``text`` Le corps du SMS
    - ``origin`` Le numéro de l'expéditeur au format international

