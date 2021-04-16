==============================================
Téléphone Android
==============================================
.. warning::
    L'utilisation d'un téléphone Android comme gateway entre RaspiSMS et votre opérateur n'est disponible qu'en mode SaaS.

Il est possible d'utiliser `l'application RaspiSMS Relay <https://play.google.com/store/apps/details?id=fr.raspisms.raspismsrelay>`_ pour tranformer votre téléphone Android en une passerelle (gateway) qui sera utilisé par RaspiSMS pour envoyer et recevoir des messages.

Cette méthode permet d'utiliser un forfait téléphonique classique avec RaspiSMS afin d'envoyer et de recevoir des messages, sans avoir à installer et maintenir une version locale de RaspiSMS et sans gérer des équipements compliqués comme des modem GSM USB, etc.

C'est une solution idéale pour la mise en place simple et rapide d'un système dédié à l'envoi de petits volumes de messages, de messages transactionnels (non commerciaux) ou le prototypage.

Fonctionnalités supportées
--------------------------
=============================== =========
 Fonctionnalité                 Support
=============================== =========
Envoi de SMS                    Oui
Réception de SMS                Oui
Envoi de MMS                    Oui
Réception de MMS                Oui
Suivi de l'envoi                Oui
SMS Flash                       Non
Alerte sur réception d'un appel Oui
Alerte sur fin d'un appel       Non
=============================== =========


1. Configurer un téléphone Android comme relais RaspiSMS
---------------------------------------------------------
Pour savoir comment configurer un téléphone Android pour pouvoir l'utiliser au sein de RaspiSMS pour envoyer et recevoir des messages, reportez-vous au guide :ref:`Commencer à utiliser RaspiSMS avec un téléphone Android.<starting_with_raspisms_relay_guide>`.

