============================================
Télécharger les numéros invalides
============================================

Cette fonctionnalité vous permet de télécharger une liste des numéros de téléphone considérés comme invalides. Un numéro est défini comme invalide si les SMS envoyés à ce numéro atteignent un certain taux d'échecs ou d'inconnus, selon les critères que vous définissez.

Accès à la fonctionnalité
=========================

Vous pouvez accéder à cette option depuis l'interface "SMS Envoyés", via le bouton "Télécharger les numéros invalides".

Critères de sélection
=====================

L'interface vous permet de personnaliser les critères utilisés pour déterminer si un numéro est invalide. 

1. **Volume minimum de SMS envoyés** : Ce paramètre détermine le nombre minimum de messages qui doivent avoir été envoyés au numéro. Cette valeur permet d'éviter la sélection de numéros avec un taux d'échec basé sur une faible quantité de messages.

2. **Taux d'échecs** : Vous pouvez spécifier un pourcentage minimal de SMS échoués pour qu'un numéro soit considéré comme invalide. 

3. **Taux d'inconnus** : De la même manière, vous pouvez définir un pourcentage minimal de SMS marqués comme "inconnus".

**Note** : Vous pouvez fixer un des taux à 0 % pour désactiver son effet, se basant alors uniquement sur l'autre taux.

Fonctionnement
==============
Lorsque vous cliquez sur "Télécharger les numéros invalides", le système génère un fichier CSV contenant la liste des numéros qui répondent aux critères que vous avez sélectionnés. Ce fichier est ensuite téléchargé pour un usage ultérieur, par exemple pour nettoyer les listes de diffusion.

Exemple de fichier CSV obtenu
=============================

Voici un exemple de fichier CSV avec la liste des numéros de téléphones invalides :

.. literalinclude:: /_code_examples/invalid_numbers/invalid-numbers.csv
    :language: none