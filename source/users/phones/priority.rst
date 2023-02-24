.. _users_phones_priority:

========================================================================================
Utiliser en priorité certains téléphones.
========================================================================================

Activer/désactiver le support des téléphones prioritaires
============================================================================
Le support des téléphones prioritaires peut être activé/désactivé. Pour cela, rendez-vous dans la partie "Réglages" de RaspiSMS, modifiez la valeur dans le champ "Support des téléphones prioritaires" et cliquez sur "Mettre à jour les données".

Si vous désactivez le support des téléphones prioritaires, tous les téléphones seront traités comme ayant la même priorité, ``0``.

Définir les téléphones prioritaires
===================================================
Lors de la création d'un téléphone vous pouvez définir sa priorité dans la partie "Priorité d'utilisation du téléphone".

Quand vous créez un SMS sans définir le téléphone a utilisé, un téléphone sera choisi au hasard parmi les téléphones disponibles avec la plus haute priorité.

Concrétement, lors de l'envoi du SMS RapiSMS choisira le téléphone à utiliser en effectuant les étapes suivantes :
#. Retrait des téléphones non disponibles.
#. Retrait des téléphones ayant déjà atteint leur limite d'envoi (:ref:`voir la documentation sur la limite d'envoi de SMS par téléphone <users_phones_limits>`).
#. Retrait des téléphones avec une priorité inférieure à la priorité maximal.
#. Choix d'un téléphone au hasard parmis les téléphones restants.

La priorité minimum d'un téléphone est de ``0``, c'est aussi la priorité par défaut. 

Quand utiliser des téléphones prioritaires ?
============================================
Pour la pluspart des utilisateurs la notion de priorité sur les téléphones n'est pas utile, et vous pouvez laisser tous vos téléphones avec la priorité par défaut.

Les téléphones prioritaires sont principalement utiles si vous mélangez plusieurs fournisseurs de SMS, et que vous souhaitez utiliser en priorité certains fournisseurs (par example parcequ'il sont moins chers), tout en conservant la possibilité de vous rabattre sur un autre fournisseur si le fournisseur prioritéraire n'est plus disponible ou que vous avez épuisé vos limites d'envoi sur ce fournisseur.
Dans ce cas, vous pouvez définir un score plus élevé pour vos téléphones prioritaires (par exemple ``1``) et créer votre téléphone de secours avec une priorité de ``0``.
