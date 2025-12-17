# 📘 RentSweet — Modèle Métier Back-End

## Entités métier (PostgreSQL & MongoDB)

---

## 1. Utilisateurs

### Utilisateur

| Attribut | Type |
|----------|------|
| id | Long |
| email | String |
| motDePasse | String |
| nom | String |
| prenom | String |
| telephone | String |
| role | RoleUtilisateur |
| actif | Boolean |
| dateCreation | LocalDateTime |

### RoleUtilisateur (enum)
- PROPRIETAIRE
- LOCATAIRE

---

## 2. Biens immobiliers

### BienImmobilier

| Attribut | Type |
|----------|------|
| id | Long |
| titre | String |
| description | String |
| typeBien | TypeBien |
| surface | Double |
| adresse | String |
| localisation | Point (PostGIS) |
| proprietaire | Utilisateur |
| locataire | Utilisateur (nullable) |
| actif | Boolean |
| dateCreation | LocalDateTime |

### TypeBien (enum)
- APPARTEMENT
- MAISON
- STUDIO
- AUTRE

---

### PhotoBien

| Attribut | Type |
|----------|------|
| id | Long |
| bienImmobilier | BienImmobilier |
| urlFichier | String |
| dateAjout | LocalDateTime |

---

## 3. Contrats de location

### ContratLocation

| Attribut | Type |
|----------|------|
| id | Long |
| bienImmobilier | BienImmobilier |
| proprietaire | Utilisateur |
| locataire | Utilisateur |
| dateDebut | LocalDate |
| dateFin | LocalDate |
| montantLoyer | BigDecimal |
| montantCharges | BigDecimal |
| statut | StatutContrat |
| version | Integer |
| dateCreation | LocalDateTime |

### StatutContrat (enum)
- BROUILLON
- EN_ATTENTE_SIGNATURE
- SIGNE
- ARCHIVE

---

### SignatureContrat

| Attribut | Type |
|----------|------|
| id | Long |
| contratLocation | ContratLocation |
| signataire | Utilisateur |
| dateSignature | LocalDateTime |
| adresseIP | String |

---

## 4. Quittances de loyer

### QuittanceLoyer

| Attribut | Type |
|----------|------|
| id | Long |
| contratLocation | ContratLocation |
| locataire | Utilisateur |
| mois | Integer |
| annee | Integer |
| montantTotal | BigDecimal |
| documentPDF | Document |
| dateGeneration | LocalDateTime |

---

## 5. États des lieux

### EtatDesLieux

| Attribut | Type |
|----------|------|
| id | Long |
| bienImmobilier | BienImmobilier |
| contratLocation | ContratLocation |
| typeEtat | TypeEtatDesLieux |
| dateRealisation | LocalDate |
| valideParProprietaire | Boolean |
| valideParLocataire | Boolean |
| dateValidation | LocalDateTime |

### TypeEtatDesLieux (enum)
- ENTREE
- SORTIE

---

### PieceEtatDesLieux

| Attribut | Type |
|----------|------|
| id | Long |
| etatDesLieux | EtatDesLieux |
| nomPiece | String |

---

### ElementEtatDesLieux

| Attribut | Type |
|----------|------|
| id | Long |
| pieceEtatDesLieux | PieceEtatDesLieux |
| description | String |
| etat | String |
| commentaire | String |

---

### PhotoEtatDesLieux

| Attribut | Type |
|----------|------|
| id | Long |
| etatDesLieux | EtatDesLieux |
| urlFichier | String |
| typePhoto | TypePhotoEtat |

### TypePhotoEtat (enum)
- AVANT
- APRES

---

## 6. Messagerie interne

### Conversation

| Attribut | Type |
|----------|------|
| id | Long |
| bienImmobilier | BienImmobilier |
| contratLocation | ContratLocation (nullable) |
| dateCreation | LocalDateTime |

---

### Message

| Attribut | Type |
|----------|------|
| id | Long |
| conversation | Conversation |
| auteur | Utilisateur |
| contenu | String |
| dateEnvoi | LocalDateTime |
| lu | Boolean |

---

## 7. Documents

### Document

| Attribut | Type |
|----------|------|
| id | Long |
| typeDocument | TypeDocument |
| urlFichier | String |
| dateCreation | LocalDateTime |
| proprietaire | Utilisateur |
| accesRestreint | Boolean |

### TypeDocument (enum)
- CONTRAT
- QUITTANCE
- ETAT_DES_LIEUX
- AUTRE

---

## 8. Notifications

### Notification

| Attribut | Type |
|----------|------|
| id | Long |
| utilisateur | Utilisateur |
| typeNotification | TypeNotification |
| contenu | String |
| lu | Boolean |
| dateCreation | LocalDateTime |

### TypeNotification (enum)
- MESSAGE
- DOCUMENT
- VALIDATION

---

## 9. Points d'intérêt (MongoDB)

### PointInteret

| Attribut | Type |
|----------|------|
| id | String |
| nom | String |
| type | TypePointInteret |
| localisation | GeoJSON |
| metadonnees | Map<String, Object> |

### TypePointInteret (enum)
- ECOLE
- TRANSPORT
- COMMERCE
- SERVICE_PUBLIC

---

## 📊 Résumé des Entités

| Catégorie | Entités | Base de données |
|-----------|---------|-----------------|
| **Utilisateurs** | Utilisateur | PostgreSQL |
| **Biens** | BienImmobilier, PhotoBien | PostgreSQL + PostGIS |
| **Contrats** | ContratLocation, SignatureContrat | PostgreSQL |
| **Finances** | QuittanceLoyer | PostgreSQL |
| **États des lieux** | EtatDesLieux, PieceEtatDesLieux, ElementEtatDesLieux, PhotoEtatDesLieux | PostgreSQL |
| **Communication** | Conversation, Message | PostgreSQL |
| **Documents** | Document | PostgreSQL + MinIO |
| **Notifications** | Notification | PostgreSQL |
| **Géolocalisation** | PointInteret | MongoDB |

---

## 🔗 Relations principales

### Utilisateur
- **1 → N** BienImmobilier (en tant que propriétaire)
- **1 → N** ContratLocation (en tant que propriétaire ou locataire)
- **1 → N** Message (en tant qu'auteur)
- **1 → N** Notification (destinataire)
- **1 → N** Document (propriétaire)

### BienImmobilier
- **N → 1** Utilisateur (propriétaire)
- **1 → 1** Utilisateur (locataire, nullable)
- **1 → N** PhotoBien
- **1 → N** ContratLocation
- **1 → N** EtatDesLieux
- **1 → N** Conversation

### ContratLocation
- **N → 1** BienImmobilier
- **N → 1** Utilisateur (propriétaire)
- **N → 1** Utilisateur (locataire)
- **1 → N** SignatureContrat
- **1 → N** QuittanceLoyer
- **1 → N** EtatDesLieux

### EtatDesLieux
- **N → 1** BienImmobilier
- **N → 1** ContratLocation
- **1 → N** PieceEtatDesLieux
- **1 → N** PhotoEtatDesLieux

### Conversation
- **N → 1** BienImmobilier
- **N → 1** ContratLocation (nullable)
- **1 → N** Message
