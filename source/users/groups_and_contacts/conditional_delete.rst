.. _contacts_conditional_delete:

=========================================
Supprimer une liste dynamique de contacts
=========================================

À quoi ça sert ?
=================================
La suppression d'une liste dynamique de contacts vous permet de facilement supprimer de nombreux contacts en fonction :ref:`des données enrichis des contacts<extended_contacts>`.

Comment ça fonctionne ?
========================
Lors de la suppression d'une liste dynamique de contacts, au lieu de sélectionner une liste fixe de contact à supprimer, vous écrivez une règle qui sera évaluée pour chaque contact avec les données de celui-ci. Si la règle est évaluée comme ``VRAIE``, alors le contact est supprimé, sinon il est ignoré.

.. note::
    Avant de valider la suppression d'une liste dynamique de contact, vous pouvez prévisualiser la liste des contacts qui seront supprimés en cliquant sur le bouton "Prévisualiser les contacts qui seront supprimés".


Comment écrire une règle pour supprimer des contacts ?
======================================================

Le système de règles pour supprimer des contacts est le même que pour la création de groupes conditionnels. Pour plus d'information sur l'écriture de ces règles, repportez vous à :ref:`la documention sur les groupes conditionnels.<conditionnals_groups>`
