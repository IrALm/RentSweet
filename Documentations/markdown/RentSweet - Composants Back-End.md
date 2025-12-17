# RentSweet - Composants Back-End

## 1. User Management (Gestion des utilisateurs)
**Rôle / Fonctionnalités :**
- Gestion des rôles : propriétaire / locataire / admin
- Inscription / login / modification profil
- Authentification sécurisée
- Permissions d’accès aux modules / couches

**Dépendances :**
- spring-boot-starter-security
- jjwt (JWT pour API sécurisée)
- spring-boot-starter-data-jpa (stockage utilisateurs PostgreSQL)
- lombok (réduction boilerplate)

---

## 2. Property Management (Gestion des biens)
**Rôle / Fonctionnalités :**
- CRUD des biens immobiliers
- Géolocalisation des biens (latitude / longitude)
- Liens avec propriétaire(s) et locataire(s)
- Gestion des photos liées au bien

**Dépendances :**
- spring-boot-starter-data-jpa
- hibernate-spatial (PostGIS)
- spring-boot-starter-web (API REST)
- io.minio:minio (stockage des images)

---

## 3. Contracts & Documents (Contrats et documents)
**Rôle / Fonctionnalités :**
- Création et génération de contrats PDF
- Signature électronique simplifiée
- Stockage et archivage des documents
- Gestion des quittances et états des lieux

**Dépendances :**
- openpdf (PDF)
- spring-boot-starter-data-jpa
- io.minio:minio (stockage réel des fichiers)
- spring-boot-starter-web

---

## 4. Inventories (États des lieux)
**Rôle / Fonctionnalités :**
- Checklist pièce par pièce
- Ajout de photos avant/après
- Validation double
- Génération PDF pour archivage

**Dépendances :**
- spring-boot-starter-data-jpa
- openpdf
- io.minio:minio
- spring-boot-starter-web

---

## 5. Messaging (Messagerie interne)
**Rôle / Fonctionnalités :**
- Conversations entre propriétaires et locataires
- Statut “lu/non lu”
- Filtrage par bien ou contrat

**Dépendances :**
- spring-boot-starter-websocket (option temps réel)
- spring-boot-starter-data-jpa (stockage messages)
- spring-boot-starter-web (API REST)

---

## 6. Search & Filters (Recherche avancée)
**Rôle / Fonctionnalités :**
- Recherche par distance aux POI (écoles, commerces…)
- Filtrage par type de bien, prix, localisation
- Combinaison PostgreSQL (biens) + MongoDB (POI)

**Dépendances :**
- spring-boot-starter-data-jpa
- spring-boot-starter-data-mongodb
- hibernate-spatial
- spring-boot-starter-web

---

## 7. File & Media Management (Gestion des fichiers / images)
**Rôle / Fonctionnalités :**
- Upload et récupération de fichiers
- Stockage structuré par type d’entité
- Sécurisation via backend ou URLs signées

**Dépendances :**
- io.minio:minio
- spring-boot-starter-web
- spring-boot-starter-data-jpa

---

## 8. Security & Auth (Sécurité / Authentification)
**Rôle / Fonctionnalités :**
- Authentification / JWT
- Gestion des rôles et accès aux endpoints
- Sécurisation des fichiers et documents

**Dépendances :**
- spring-boot-starter-security
- jjwt
- spring-boot-starter-data-jpa

---

## 9. Utilities & Monitoring (Optionnel)
**Rôle / Fonctionnalités :**
- Logs, metrics, monitoring
- Tests unitaires et d’intégration
- Documentation API

**Dépendances :**
- spring-boot-starter-actuator
- spring-boot-starter-test + mockito-core
- springdoc-openapi-starter-webmvc-ui
- lombok

---

# Dépendances Spring Boot nécessaires pour le développement back-end
- spring-boot-starter-web
- spring-boot-starter-data-jpa
- spring-boot-starter-data-mongodb
- spring-boot-starter-security
- spring-boot-starter-websocket
- spring-boot-starter-validation
- spring-boot-starter-actuator
- lombok
- io.minio:minio
- jjwt
- openpdf
- hibernate-spatial
- spring-boot-starter-test
- mockito-core
- springdoc-openapi-starter-webmvc-ui
