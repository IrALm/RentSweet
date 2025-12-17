# 🏗️ RentSweet — Architecture Globale

## 📐 Vue d'Ensemble de l'Architecture

RentSweet est une plateforme de gestion locative complète permettant aux propriétaires et locataires de gérer leurs biens, contrats, documents et communications.

**Stack Technologique :**
- **Back-End :** Spring Boot
- **Front-End :** Angular

---

## 🎨 Architecture Globale

```mermaid
graph TB
    subgraph "Couche Présentation"
        FE[🌐 Front-end Angular<br/>Interface Utilisateur<br/>Responsive Design]
    end
    
    subgraph "Couche API"
        API[🚪 API REST Spring Boot<br/>Endpoints RESTful<br/>Validation & Sécurité]
    end
    
    subgraph "Couche Services - Back-end Core"
        direction TB
        
        UM[👥 Gestion Utilisateurs<br/>Authentification<br/>Autorisation<br/>Profils]
        
        PM[🏠 Gestion Biens<br/>CRUD Biens Immobiliers<br/>Photos<br/>Géolocalisation PostGIS]
        
        CM[📄 Contrats & Documents<br/>Création Contrats<br/>Signatures Électroniques<br/>Génération PDF]
        
        ED[📋 États des Lieux<br/>Création EDL<br/>Validation Bipartite<br/>Photos Avant/Après]
        
        MSG[💬 Messagerie Interne<br/>Conversations<br/>Messages Temps Réel<br/>Notifications]
        
        RECH[🔍 Recherche Avancée<br/>Filtres Multi-critères<br/>Recherche Géographique<br/>Points d'Intérêt]
        
        NOTIF[🔔 Notifications<br/>Alertes Système<br/>Emails<br/>Push Notifications]
        
        SEC[🔐 Sécurité & Auth<br/>JWT Tokens<br/>Spring Security<br/>RBAC]
    end
    
    subgraph "Couche Données"
        DB1[(🗄️ PostgreSQL + PostGIS<br/>Données Relationnelles<br/>Géolocalisation<br/>Transactions ACID)]
                
        FS[(📦 Stockage Fichiers - MinIO<br/>Photos Biens<br/>Documents PDF<br/>Photos États des Lieux)]
    end
    
    FE -->|HTTPS / JSON| API
    
    API --> UM
    API --> PM
    API --> CM
    API --> ED
    API --> MSG
    API --> RECH
    API --> NOTIF
    
    API --> SEC
    SEC -.authentifie.-> UM
    SEC -.autorise.-> PM
    SEC -.autorise.-> CM
    SEC -.autorise.-> ED
    SEC -.autorise.-> MSG
    
    UM --> DB1
    PM --> DB1
    CM --> DB1
    ED --> DB1
    MSG --> DB1
    NOTIF --> DB1
    
    RECH --> DB1
    
    CM --> FS
    ED --> FS
    PM --> FS
    
    style FE fill:#e1f5ff
    style API fill:#e8f5e9
    style UM fill:#fff3e0
    style PM fill:#f3e5f5
    style CM fill:#ffebee
    style ED fill:#e0f2f1
    style MSG fill:#fff9c4
    style RECH fill:#f1f8e9
    style NOTIF fill:#fce4ec
    style SEC fill:#ffebee
    style DB1 fill:#e8f5e9
    style FS fill:#fff3e0
```

---

## 🔄 Architecture Détaillée par Couche

### 1. Couche Présentation

```mermaid
graph LR
    subgraph "Front-end Angular"
        direction TB
        
        COMP[🧩 Composants<br/>Dashboard<br/>Biens<br/>Contrats<br/>Messages]
        
        SERV[⚙️ Services Angular<br/>HTTP Client<br/>State Management<br/>Routing]
        
        AUTH_FE[🔐 Auth Guard<br/>Route Protection<br/>Token Management]
    end
    
    API_BACKEND[🚪 API REST]
    
    COMP --> SERV
    SERV --> AUTH_FE
    AUTH_FE --> API_BACKEND
    
    style COMP fill:#e1f5ff
    style SERV fill:#f3e5f5
    style AUTH_FE fill:#ffebee
    style API_BACKEND fill:#e8f5e9
```

---

### 2. Couche Services Back-End

```mermaid
graph TB
    subgraph "Services Métiers"
        direction LR
        
        subgraph "Gestion Utilisateurs"
            U1[Inscription]
            U2[Authentification]
            U3[Gestion Profils]
        end
        
        subgraph "Gestion Biens"
            B1[CRUD Biens]
            B2[Upload Photos]
            B3[Géolocalisation]
        end
        
        subgraph "Contrats & Documents"
            C1[Création Contrats]
            C2[Signatures]
            C3[Génération Quittances]
        end
        
        subgraph "États des Lieux"
            E1[Création EDL]
            E2[Validation]
            E3[Photos EDL]
        end
        
        subgraph "Messagerie"
            M1[Conversations]
            M2[Messages]
            M3[Notifications Temps Réel]
        end
        
        subgraph "Recherche"
            R1[Filtres Biens]
            R2[Recherche Géo]
            R3[Points d'Intérêt]
        end
    end
    
    style U1 fill:#fff3e0
    style U2 fill:#fff3e0
    style U3 fill:#fff3e0
    style B1 fill:#f3e5f5
    style B2 fill:#f3e5f5
    style B3 fill:#f3e5f5
    style C1 fill:#ffebee
    style C2 fill:#ffebee
    style C3 fill:#ffebee
    style E1 fill:#e0f2f1
    style E2 fill:#e0f2f1
    style E3 fill:#e0f2f1
    style M2 fill:#fff9c4
    style M3 fill:#fff9c4
    style R1 fill:#f1f8e9
    style R2 fill:#f1f8e9
    style R3 fill:#f1f8e9
```

---

### 3. Couche Données

```mermaid
graph TB
    subgraph "PostgreSQL + PostGIS"
        direction TB
        
        T1[(👥 Utilisateurs)]
        T2[(🏠 Biens Immobiliers)]
        T3[(📄 Contrats)]
        T4[(💰 Quittances)]
        T5[(📋 États des Lieux)]
        T6[(💬 Messages)]
        T7[(📎 Documents)]
        T8[(🔔 Notifications)]
    end
    
    subgraph "MinIO Object Storage"
        F1[📷 Photos Biens]
        F2[📄 Documents PDF]
        F3[📸 Photos EDL]
    end
    
    style T1 fill:#fff3e0
    style T2 fill:#f3e5f5
    style T3 fill:#ffebee
    style T4 fill:#e0f2f1
    style T5 fill:#e0f2f1
    style T6 fill:#fff9c4
    style T7 fill:#e8f5e9
    style T8 fill:#fce4ec
    style F1 fill:#fff3e0
    style F2 fill:#fff3e0
    style F3 fill:#fff3e0
```

---

## 🔐 Flux d'Authentification

```mermaid
sequenceDiagram
    actor User as Utilisateur
    participant FE as Front-end Angular
    participant API as API REST
    participant Auth as Service Auth
    participant DB as PostgreSQL
    
    User->>FE: Saisir email/password
    FE->>API: POST /api/auth/login
    API->>Auth: Authentifier
    Auth->>DB: Vérifier credentials
    
    alt Credentials valides
        DB-->>Auth: ✅ Utilisateur trouvé
        Auth->>Auth: Générer JWT Token
        Auth-->>API: Token + User Info
        API-->>FE: 200 OK + JWT
        FE->>FE: Stocker token (localStorage)
        FE->>User: ✅ Connexion réussie
    else Credentials invalides
        DB-->>Auth: ❌ Utilisateur non trouvé
        Auth-->>API: Erreur authentification
        API-->>FE: 401 Unauthorized
        FE->>User: ❌ Email/Password incorrect
    end
```

---

## 🏠 Flux de Création de Bien Immobilier

```mermaid
sequenceDiagram
    actor Proprio as Propriétaire
    participant FE as Front-end
    participant API as API REST
    participant BienSvc as Service Biens
    participant DB as PostgreSQL
    participant MinIO as MinIO Storage
    
    Proprio->>FE: Créer nouveau bien
    FE->>FE: Formulaire de création
    Proprio->>FE: Remplir informations + photos
    
    FE->>API: POST /api/biens<br/>(JWT Token)
    API->>API: Valider JWT
    API->>BienSvc: Créer bien
    
    BienSvc->>DB: INSERT BienImmobilier
    DB-->>BienSvc: ✅ Bien créé (ID)
    
    loop Pour chaque photo
        BienSvc->>MinIO: Upload photo
        MinIO-->>BienSvc: ✅ URL fichier
        BienSvc->>DB: INSERT PhotoBien
    end
    
    BienSvc-->>API: ✅ Bien créé avec photos
    API-->>FE: 201 Created + Bien
    FE->>Proprio: ✅ Bien publié
```

---

## 📄 Flux de Signature de Contrat

```mermaid
sequenceDiagram
    actor Proprio as Propriétaire
    actor Loc as Locataire
    participant API as API REST
    participant ContratSvc as Service Contrats
    participant DB as PostgreSQL
    participant MinIO as MinIO
    participant NotifSvc as Service Notifications
    
    Proprio->>API: POST /api/contrats<br/>Créer contrat
    API->>ContratSvc: Créer contrat (BROUILLON)
    ContratSvc->>DB: INSERT ContratLocation
    DB-->>ContratSvc: ✅ Contrat créé
    
    Proprio->>API: PUT /api/contrats/{id}/soumettre
    API->>ContratSvc: Changer statut → EN_ATTENTE_SIGNATURE
    ContratSvc->>DB: UPDATE statut
    ContratSvc->>NotifSvc: Notifier locataire
    NotifSvc->>Loc: 🔔 Contrat à signer
    
    Loc->>API: POST /api/contrats/{id}/signer
    API->>ContratSvc: Signer contrat (Locataire)
    ContratSvc->>DB: INSERT SignatureContrat
    
    Proprio->>API: POST /api/contrats/{id}/signer
    API->>ContratSvc: Signer contrat (Propriétaire)
    ContratSvc->>DB: INSERT SignatureContrat
    
    ContratSvc->>ContratSvc: Vérifier 2 signatures
    ContratSvc->>DB: UPDATE statut → SIGNE
    
    ContratSvc->>ContratSvc: Générer PDF contrat
    ContratSvc->>MinIO: Upload PDF
    MinIO-->>ContratSvc: ✅ URL PDF
    
    ContratSvc->>NotifSvc: Notifier les deux parties
    NotifSvc->>Proprio: 🔔 Contrat signé
    NotifSvc->>Loc: 🔔 Contrat signé
```

---

## 📋 Flux de Création d'État des Lieux

```mermaid
sequenceDiagram
    actor User as Utilisateur
    participant FE as Front-end
    participant API as API REST
    participant EDLSvc as Service États des Lieux
    participant DB as PostgreSQL
    participant MinIO as MinIO
    
    User->>FE: Créer état des lieux
    FE->>API: POST /api/etats-des-lieux
    API->>EDLSvc: Créer EDL
    EDLSvc->>DB: INSERT EtatDesLieux
    
    loop Pour chaque pièce
        User->>FE: Ajouter pièce + éléments
        FE->>API: POST /api/etats-des-lieux/{id}/pieces
        API->>EDLSvc: Ajouter pièce
        EDLSvc->>DB: INSERT PieceEtatDesLieux
        
        loop Pour chaque élément
            EDLSvc->>DB: INSERT ElementEtatDesLieux
        end
    end
    
    loop Pour chaque photo
        User->>FE: Prendre photo
        FE->>API: POST /api/etats-des-lieux/{id}/photos
        API->>EDLSvc: Upload photo
        EDLSvc->>MinIO: Stocker photo
        MinIO-->>EDLSvc: ✅ URL photo
        EDLSvc->>DB: INSERT PhotoEtatDesLieux
    end
    
    User->>FE: Valider EDL
    FE->>API: PUT /api/etats-des-lieux/{id}/valider
    API->>EDLSvc: Valider EDL
    EDLSvc->>DB: UPDATE valideParProprietaire/Locataire
    EDLSvc-->>API: ✅ EDL validé
    API-->>FE: 200 OK
    FE->>User: ✅ État des lieux finalisé
```

---

## 💬 Flux de Messagerie

```mermaid
sequenceDiagram
    actor User1 as Propriétaire
    actor User2 as Locataire
    participant FE as Front-end
    participant API as API REST
    participant MsgSvc as Service Messagerie
    participant DB as PostgreSQL
    participant NotifSvc as Service Notifications
    
    User1->>FE: Démarrer conversation
    FE->>API: POST /api/conversations
    API->>MsgSvc: Créer conversation
    MsgSvc->>DB: INSERT Conversation
    DB-->>MsgSvc: ✅ Conversation créée
    
    User1->>FE: Envoyer message
    FE->>API: POST /api/conversations/{id}/messages
    API->>MsgSvc: Créer message
    MsgSvc->>DB: INSERT Message
    
    MsgSvc->>NotifSvc: Notifier destinataire
    NotifSvc->>DB: INSERT Notification
    NotifSvc->>User2: 🔔 Nouveau message
    
    User2->>FE: Ouvrir conversation
    FE->>API: GET /api/conversations/{id}/messages
    API->>MsgSvc: Récupérer messages
    MsgSvc->>DB: SELECT Messages
    DB-->>MsgSvc: Liste messages
    MsgSvc->>DB: UPDATE Message (lu = true)
    MsgSvc-->>API: Messages
    API-->>FE: 200 OK + Messages
    FE->>User2: Afficher conversation
```

---

## 🔍 Flux de Recherche Avancée

```mermaid
sequenceDiagram
    actor User as Utilisateur
    participant FE as Front-end
    participant API as API REST
    participant RechSvc as Service Recherche
    participant PG as PostgreSQL + PostGIS
    participant Mongo as MongoDB
    
    User->>FE: Rechercher biens
    FE->>FE: Définir filtres<br/>(type, surface, prix, localisation)
    
    FE->>API: GET /api/biens/recherche?filters=...
    API->>RechSvc: Rechercher biens
    
    RechSvc->>PG: Query avec filtres<br/>+ Recherche géographique (PostGIS)
    PG-->>RechSvc: Liste biens correspondants
    
    alt Recherche avec points d'intérêt
        User->>FE: Filtrer par POI (écoles, transports)
        FE->>API: GET /api/biens/recherche?poi=ecole,transport
        API->>RechSvc: Recherche avec POI
        
        RechSvc->>Mongo: Query GeoJSON<br/>Points d'intérêt à proximité
        Mongo-->>RechSvc: POI proches
        
        RechSvc->>RechSvc: Filtrer biens par distance POI
    end
    
    RechSvc-->>API: Résultats de recherche
    API-->>FE: 200 OK + Biens
    FE->>User: Afficher résultats sur carte
```

---

## 🛠️ Stack Technologique

### Front-End
```
Angular 17+
TypeScript
Angular Material
Leaflet (Cartes interactives)
RxJS (Reactive Programming)
```

### Back-End
```
Spring Boot 3.x
Java 17+
Spring Security + JWT
Spring Data JPA
Spring Data MongoDB
```

### Bases de Données
```
PostgreSQL 15+ (Données relationnelles)
PostGIS (Extension géospatiale)
MongoDB 7+ (Points d'intérêt)
```

### Stockage Fichiers
```
MinIO (Object Storage compatible S3)
```

### Infrastructure
```
Docker
Docker Compose
Nginx (Reverse Proxy)
```

---

## 📊 Composants Principaux

| Composant | Technologie | Rôle |
|-----------|-------------|------|
| **Front-end** | Angular | Interface utilisateur responsive |
| **API REST** | Spring Boot | Endpoints RESTful, validation |
| **Authentification** | Spring Security + JWT | Sécurité, gestion sessions |
| **Base relationnelle** | PostgreSQL + PostGIS | Données métier + géolocalisation |
| **Stockage fichiers** | MinIO | Photos, documents PDF |
| **Notifications** | Spring Events | Alertes temps réel |

---

## 🔐 Sécurité

### Authentification & Autorisation
- **JWT Tokens** : Authentification stateless
- **Spring Security** : Protection des endpoints
- **RBAC** : Contrôle d'accès basé sur les rôles (PROPRIETAIRE, LOCATAIRE)
- **HTTPS** : Chiffrement des communications

### Protection des Données
- **Validation** : Bean Validation (JSR-303)
- **Sanitization** : Protection XSS
- **CORS** : Configuration stricte
- **Rate Limiting** : Protection contre abus

---

## 📈 Fonctionnalités Principales

### Pour les Propriétaires
- ✅ Gestion des biens immobiliers
- ✅ Création et signature de contrats
- ✅ Génération automatique de quittances
- ✅ États des lieux numériques
- ✅ Messagerie avec locataires
- ✅ Tableau de bord analytique

### Pour les Locataires
- ✅ Recherche de biens avec filtres avancés
- ✅ Visualisation sur carte interactive
- ✅ Signature électronique de contrats
- ✅ Téléchargement de quittances
- ✅ États des lieux collaboratifs
- ✅ Communication avec propriétaire

---

## 🎯 Avantages de l'Architecture

### Scalabilité
- Architecture modulaire (services indépendants)
- Base de données optimisée (PostgreSQL + MongoDB)
- Stockage distribué (MinIO)

### Performance
- Cache applicatif
- Requêtes optimisées (indexes, PostGIS)
- Lazy loading des données

### Maintenabilité
- Code structuré (Clean Architecture)
- Tests unitaires et d'intégration
- Documentation API (Swagger/OpenAPI)

### Sécurité
- Authentification robuste (JWT)
- Autorisation fine (RBAC)
- Validation des données
- Chiffrement HTTPS

---

## 📝 Conclusion

RentSweet offre une architecture **moderne, sécurisée et scalable** pour la gestion locative :

- ✅ **Séparation claire** des responsabilités (Angular / Spring Boot)
- ✅ **Multi-base de données** (PostgreSQL + MongoDB) pour performances optimales
- ✅ **Géolocalisation avancée** avec PostGIS
- ✅ **Stockage distribué** avec MinIO
- ✅ **Sécurité renforcée** avec Spring Security + JWT
- ✅ **Expérience utilisateur fluide** avec Angular Material

Cette architecture garantit une plateforme **performante, fiable et évolutive** pour tous les acteurs de la location immobilière.
