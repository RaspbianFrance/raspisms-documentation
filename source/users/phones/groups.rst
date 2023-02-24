.. _users_phones_groups:

========================================================================================
Créer des groupes de téléphones
========================================================================================

Les groupes de téléphones c'est quoi ?
======================================
Les groupes de téléphones sont un moyen de regrouper plusieurs téléphones au sein d'un même groupe de façon à pouvoir envoyer des SMS via les téléphones de ce groupe, sans avoir à spécifier le téléphone précis qui sera employé.

Ainsi, lors de l'envoi d'un SMS via un groupe de téléphone, c'est RaspiSMS se chargera de répartir de façon aléatoire les SMS entre les différents téléphones du groupe, en tenant compte si nécessaire :ref:`des priorités <users_phones_priority>` et :ref:`des limites <users_phones_limits>` des différents téléphones du groupe.

Cette fonction est donc particulièrement utile si vous devez gérez plusieures flottes de téléphones, par exemple pour différents services, et que vous cherchez à répartir les envois de SMS entre les différents téléphones de cette flotte.

Créer un groupe de téléphone
===================================================
Pour créer un groupe de téléphone, il vous suffit de vous rendre dans la partie "Téléphones", "Groupes de téléphones", et de cliquer sur "Ajouter un groupe téléphones".

Vous n'aurez plus qu'à définir le nom du groupe et à sélectionner les téléphones que vous souhaitez inclure dans le groupe. Une fois le groupe enregistré vous pourrez le choisir dans la partie "Numéro à employer" lors de la création d'un SMS.