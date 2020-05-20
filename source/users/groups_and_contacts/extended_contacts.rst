=================================
Les contacts enrichis
=================================


À quoi ça sert ?
=================================
Les contacts enrichis permettent d'associer des données à un contact allant au delà du nom et du numéro de téléphone.

Ces données peuvent ensuite être utilisées pour générer des listes de diffusions basées sur ces données, ou encore personnaliser des SMS standards en insérant ces données au sein du message.

Comment ça fonctionne ?
========================
Lors de la création du contact vous renseigner les données supplémentaires dans la partie **"Données du contact"**. Pour chaque donnée deux champs sont disponibles, le premier permet de renseigner le nom de la donnée, qui sera utilisé pour y accéder, le second contient la valeure de la données.

Format des données
==================
Les données sont toutes stockées sous forme de chaînes de caractères.

Il n'y a pas de format particulier à respecter pour les données, mais il est conseillé de choisir des noms de données simples, totalement en minuscule. En effet, les noms de données sont sensibles à la casse, c'est à dire que le nom ``Nom`` est différent de ``nom`` ou de ``NOM``. Pour plus de simplicité lors de l'utilisation future des données et afin d'éviter de commettre des erreures d'inattention, l'adoption d'un format uniforme est donc conseillé.

De la même façon, afin de tirer pleinement parti de la puissance des données enrichies vous devriez essayer de standardiser au maximum les données enrichies des contacts. Concrétement, cela signifie que vous devriez tenter d'utiliser toujours les mêmes noms et le même format pour des données similaires.

  **Exemple:**

  Si vous stocker la date de naissance de vos contacts, évitez d'avoir une date avec le nom ``date de naissance`` au format ``jour/mois/année`` et une autre au format ``jour mois année`` avec le nom ``birthdate``.

  À la place, choisissez un seul et unique nom et un seul et unique format, par exemple ``birthdate`` et ``jour/mois/année``.


Utiliser les données des contacts enrichis
==========================================
Pour en savoir plus sur l'utilisation des données des contacts enrichis, reportez-vous aux pages ci-dessous.

.. toctree::
    :maxdepth: 1

    
