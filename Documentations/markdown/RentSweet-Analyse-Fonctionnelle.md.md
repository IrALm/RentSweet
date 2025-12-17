# Application de Gestion Immobilière : **RentSweet**

## Plan d'Action

---

## 1. Objectif du projet

L'objectif du projet est de développer une application web de gestion immobilière de particulier à particulier permettant :

* La gestion complète des biens, propriétaires et locataires.
* La génération, la signature et le suivi des contrats de location.
* La gestion des quittances de loyer.
* La réalisation et le suivi des états des lieux (texte + photos).
* Une messagerie interne minimaliste pour la communication propriétaire ↔ locataire.
* Un workflow simple et sécurisé pour les documents et échanges.

La gestion des paiements n'est pas incluse afin d'assurer la faisabilité du projet en trois mois.

---

## 2. Modules principaux et fonctionnalités

| Module          | Fonctionnalités principales                                                                  |
| --------------- | -------------------------------------------------------------------------------------------- |
| Utilisateurs    | Gestion des rôles (propriétaire / locataire), authentification, permissions                  |
| Biens           | Création et modification des biens, liens propriétaires/locataires                           |
| Contrats        | Création, génération PDF, signature électronique simple, versionnage, workflow de validation |
| États des lieux | Checklist pièce par pièce, ajout de photos, génération PDF, double validation                |
| Quittances      | Génération automatique de quittances PDF, lien avec contrats et locataires                   |
| Messagerie      | Conversations par bien, envoi/réception de messages, horodatage, statut « lu / non lu »      |
| Documents       | Stockage centralisé (PDF, images), horodatage, accès contrôlé                                |
| Notifications   | Alertes pour nouveaux messages ou documents à valider                                        |

---

## 3. Flux général

* Création d’un bien → assignation propriétaire/locataire.
* Création d’un contrat → génération PDF → signature → archivage.
* Réalisation d’un état des lieux → validations → PDF.
* Messagerie interne associée à un bien / contrat / état des lieux.
* Génération automatique des quittances.

---

## 4. Couverture du projet et impact

### Couverture métier

* Contrats, états des lieux, quittances, communication → gestion locative complète.

### Couverture technique

* Modules métiers, workflow documentaire, génération PDF, messagerie, rôles.

### Impact pour la soutenance

* Démonstration d'une architecture solide et modulaire.
* Mise en avant de règles métier complexes et d'un flux documentaire clair.

**Points forts :** projet crédible, modulable, concret pour un M1, réalisable en 3 mois.
**Limites :** pas de paiement, pas de messagerie temps réel, fonctionnalités minimalistes.

---

# Aspects Techniques : Technologies

## Bases de données

### Besoin : gestion des POI (Points d'Intérêt)

Les POI représentent des lieux utiles proches des biens immobiliers, tels que :

* Services publics : poste, commissariat, mairie, écoles.
* Transports : bus, métro, gare.
* Commerces : supermarchés, boulangeries, pharmacies, banques, restaurants.

Ces informations permettent des recherches enrichies :

* « Biens proches d’une école (< 1 km) ».
* « Biens à moins de 5 minutes d’une station de métro ».
* « Zones avec forte présence de commerces ».

### Double base de données : PostgreSQL + MongoDB

#### 1. PostgreSQL (+ PostGIS)

Pour :

* Données structurées.
* Relations métiers (propriétaires, locataires, biens, contrats, messages...).
* Intégrité forte.
* Géolocalisation précise via PostGIS.

#### 2. MongoDB

Pour :

* Données externes (POI) massives et flexibles.
* Structure variable.
* Intégration facile depuis OpenStreetMap.
* Requêtes géospatiales rapides.

### Répartition des données

#### PostgreSQL

* Utilisateurs
* Biens immobiliers (avec PostGIS)
* Contrats
* Quittances
* États des lieux
* Messages internes
* Documents
* Sessions/Tokens (optionnel)

#### MongoDB

* POI (écoles, commerces, transports...)
* Données externes flexibles
* Caches géographiques (optionnel)

---

## Architecture globale

### Vue d’ensemble

```
Front-end (Angular)
        │
     API REST
        │
Back-end Core (Spring Boot)
 - Couche métier : biens , utilisateurs , contrats ,etc.
 - Couche messagerie : notifications , discussions bailleurs-locataire
 - Couche recherche : en fonction des POI
        │        │
PostgreSQL     MongoDB
(PostGIS)       (POI)
```

### Architecture logique

#### A. Back-end Core

Couches :

* **User Management** : rôles, authentification, profils.
* **Property Management** : CRUD, géolocalisation PostGIS, photos.
* **Contrats & Documents** : création, signature interne, PDF.
* **Messagerie interne** : messages, statut, conversations.
* **Recherche avancée** : distance, types de POI, mix PostgreSQL + MongoDB.

#### B. Base PostgreSQL

Assure l'intégrité, les relations et la géolocalisation précise.

#### C. Base MongoDB

Gère les POI et données externes, optimisant la recherche.

### Architecture multi-couches

* Séparation claire logique métier-repository-modèles-controller.
* Couche pour chaque fonctionalité.
* Code testable et maintenable.

---

# Technologies utilisées

## Back-end : Java 21 — Spring Boot

### Avantages

* Architecture multi-couche.
* Intégration PostgreSQL, PostGIS, MongoDB.
* Sécurité avancée (Spring Security).
* Scalabilité.

### Outils

* Spring Data JPA
* Spring Data MongoDB
* Hibernate Spatial
* Spring Security
* JWT (optionnel)
* WebSocket/STOMP (optionnel pour messagerie temps réel)
* Maven

## Front-end : Angular

### Objectifs front-end

* Gestion des biens et utilisateurs.
* Messagerie.
* Contrats et documents PDF.
* États des lieux.
* Recherche avancée (POI + cartes).
* Interface responsive.

### Outils

* Angular Router
* Reactive Forms
* RxJS
* Tailwind CSS / Angular Material
* Leaflet.js / Angular-Leaflet
* ngx-extended-pdf-viewer

## CI/CD : GitHub Actions

* Build et tests automatiques.
* Déploiement continu.
* Intégration simple dans GitHub.
* Support Docker.

## Tests et Qualité

* Backend : JUnit 5, Mockito, Checkstyle.
* Frontend : Karma, Jasmine/Jest, ESLint, Prettier.
* Optionnel : SonarQube.

## Monitoring et documentation

* Spring Actuator
* Logback
* Swagger / OpenAPI
* UML (architecture, couches)

---

Fin du document.
