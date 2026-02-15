---
title:  "OpenID Connect (OIDC)"
date:   2026-02-11
categories: théorie SSO
toc: true
code_bloc_prefix: true
---

# Description
OpenID Connect (OIDC) est un protocole permettant l'identification d'un utilisateur pour se connecter à un service. Cette identification passe par un tiers de confiance. Ce protocole est une base moderne du SSO (Single Sign-On)
OpenID Connect se base sur OAuth 2.0, qui gère l'autorisation, en ajoutant une couche d'identification.
On distingue alors plusieurs acteurs et jetons lors des échanges réseau, qui sont détaillés plus bas.

# Vocabulaire
## Acteurs
**L'utilisateur ou Resource Owner (RO)** : La personne à laquelle appartiennent les ressources (utilisateur).

**Client ou Relying Party (RP)** : L'application qui a besoin des ressources.

**Resource Server (RS)** : Le serveur qui héberge les ressources.

**Authorization Server (AS) ou OpenID Provider (OP)** : Le serveur qui génère les différents jetons et authentifie le Resource Owner.

> Le RS et l'AS peuvent être confondu dans un même produit (par exemple Keycloak ou Auth0), mais ont des rôles bien distincts.

> Une ressource est une information appartenant à l'utilisateur.

## Jetons
**Jeton d'accès** : Jeton permettant l'accès aux ressources, éventuellement sous forme d'un JWT.

**Jeton d'identification** : Jeton permettant au client de prouver à l'AS que le RO a été authentifié contenant les revendications d'authentification sous forme d'un JWT.

**Jeton de rafraîchissement** : Jeton permettant le renouvellement de l'access token, éventuellement sous forme d'un JWT.

**Code d'autorisation** : Code prouvant l'authentification du RO auprès de l'AS et pouvant être échangé par le client contre des jetons.


# Fonctionnement
En résumé, l'OIDC va intervenir quand un utilisateur va vouloir s'identifier sur une application (client).
L'utilisateur devra alors s'authentifier et donner son consentement pour transmettre ses ressources (dont le RS a connaissance) à l'application en question. Une fois l'autorisation accordée, l'application peut accéder à ces ressources et les utiliser pour créer ou retrouver une session locale.

# Cinématique

![cinématique.png](/assets/img/Cinématique OIDC.drawio.png)

1. Le RO requête le client
2. Le client redirige le RO vers l'AS en indiquant notamment une URL de redirection
3. L'AS authentifie le RO et lui demande son consentement
4. L'AS redirige le RO à L'URL de redirection, avec le code d'autorisation
5. Le client s'authentifie à l'AS, en backchannel, et fournit le code d'autorisation pour récupérer les jetons d'identification, d'accès et éventuellement de rafraîchissement
6. L'AS transmet les jetons au client
7. Le client vérifie notamment la signature du JWT via les clés publiques de l'AS puis demande les ressources au RS en présentant le jeton d'accès
8. Le RS retourne les ressources du RO
9. Le client donne accès au RO, avec éventuellement des droits calculés en fonction des valeurs des revendications

