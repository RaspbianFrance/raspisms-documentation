.. _users_phones_limits:

========================================================================================
Limiter le nombre de SMS par téléphone.
========================================================================================

Activer/désactiver le support des limites d'envoi par téléphone
============================================================================
Le support des limites d'envoi de SMS par téléphone peut être activé/désactivé. Pour cela, rendez-vous dans la partie "Réglages" de RaspiSMS, modifiez la valeur dans le champ "Support des limites d'envoi par téléphones" et cliquez sur "Mettre à jour les données".

Si vous désactivez le support des limites d'envoi par téléphone, toutes les limites déjà enregistrées seront ignorées.

Limiter le nombre de SMS qu'un téléphone peut envoyer.
============================================================================
Lors de la création d'un téléphone dans la partie "Limites des volumes d'envoi du téléphone" vous pouvez définir une ou plusieures limites d'envoi sur différentes périodes de temps.
Ce réglage est particulièrement utile si vous souhaitez vous assurer de ne pas dépasser certains volumes d'envoi, par exemple pour limiter les risques de sûr-facturation.

Lors de l'envoi d'un SMS avec ce téléphone, si celui-ci a dépassé sa ou ses limites d'envoi pour la période donnée, RaspiSMS ne transmettra pas la demande d'envoi au téléphone et le SMS sera directement marqué en échec.
Si le téléphone a utiliser pour l'envoi d'un SMS n'a pas été défini et qu'il doit être choisi au hasard, les téléphones dont les limites d'envoi sont déjà atteintes seront automatiquement ignorés.

Si aucune limite n'est définie, RaspiSMS ne bloquera jamais l'envoi sur le téléphone.
Si plusieures limites sont définies, la première limite atteinte bloquera le téléphone.

.. danger::
    RaspiSMS utilise l'historique des SMS envoyés pour déterminer si un téléphone a atteint ou non ses limites. Si vous supprimez des SMS envoyés, le calcul des limites sera donc faussé.

Les différentes périodes applicables.
============================================================================
Les limites d'envoi sont appliquées sur des périodes de temps (un jour, une semaine, etc.). Ainsi vous pouvez jouer avec ces périodes pour régler très finement le comportement du téléphone.

Ces différentes limites peuvent êtres exprimées sous formes glissantes, ou sous forme fixe, pour mieux comprendre, voici les dates à partir desquelles le nombre de SMS envoyés sera compté pour ces différentes périodes.
En considérant que nous essayons d'envoyez un SMS le Jeudi 4 Août 2022 à 14h30 (2022-08-04 14:30:00).

=============================== =========
Période                         Date
=============================== =========
Par jour                        2022-08-04 00:00:00
24 heures glissantes            2022-08-03 14:30:00
Cette semaine                   2022-08-01 00:00:00
7 jours glissants               2022-07-28 14:30:00
Ces deux dernières semaines     2022-07-25 00:00:00
14 jours glissants              2022-07-21 14:30:00
Ce mois                         2022-08-01 00:00:00
1 mois glissant                 2022-07-04 14:30:00
28 jours glissants              2022-07-07 14:30:00
30 jours glissants              2022-07-05 14:30:00
31 jours glissants              2022-07-04 14:30:00
=============================== =========

