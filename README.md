# The Dive Logbook

Application web de carnet de plongée numérique, développée en fullstack Java/Angular dans le cadre d'un projet portfolio.

> ⚠️ Le code source de ce projet est privé. Ce repository présente l'architecture, les choix techniques et les fonctionnalités de l'application.

---

## Objectif du projet

The Dive Logbook permet aux plongeurs de consigner, consulter et gérer leurs plongées en ligne. L'application cible à terme les clubs de plongée.

**URL de démonstration :** `http://51.210.42.119` *(HTTPS en cours de configuration)*

---

## 🛠️ Stack technique

### Backend
| Technologie | Version |
|-------------|---------|
| Java | 21 |
| Spring Boot | 3.3 |
| Spring Security | JWT stateless |
| PostgreSQL | 16.3 |
| Flyway | Migrations versionnées |
| Lombok | Réduction boilerplate |
| Springdoc OpenAPI | Documentation Swagger |

### Frontend
| Technologie | Version |
|-------------|---------|
| Angular | 19 |
| Tailwind CSS | v3 |
| TypeScript | 5.x |

### Infrastructure
| Composant | Détail |
|-----------|--------|
| Reverse proxy | Nginx |
| Orchestration | Docker Compose |
| Hébergement | VPS OVH (Ubuntu 24.04) |
| Recette | Raspberry Pi 4 |

---

## Architecture

```
┌─────────────────────────────────────────┐
│                  Nginx                  │
│         (reverse proxy / routing)       │
└────────────┬────────────────────────────┘
             │
    ┌────────┴────────┐
    │                 │
┌───▼───┐       ┌─────▼──────┐
│Angular│       │ Spring Boot│
│  SPA  │       │  REST API  │
└───────┘       └─────┬──────┘
                      │
               ┌──────▼──────┐
               │ PostgreSQL  │
               │   16.3      │
               └─────────────┘
```

- **Frontend Angular** servi statiquement par Nginx
- **Backend Spring Boot** exposé en API REST sur le port 9000
- **Nginx** route les requêtes selon leur méthode HTTP (GET → Angular SPA, POST/PUT/DELETE → backend)
- **JWT** stocké côté client, vérifié à chaque requête par Spring Security
- **Flyway** gère les migrations de base de données de façon versionnée

---

## Fonctionnalités implémentées

### Authentification
- Inscription avec validation email (RegEx)
- Activation de compte par email (Gmail SMTP)
- Connexion JWT / déconnexion
- Réinitialisation de mot de passe par email
- Modification de mot de passe

### Gestion des plongées
- Création, modification, suppression de plongées
- Upload multiple de photos (stockage serveur)
- Prévisualisation des photos avant soumission
- Lightbox d'agrandissement des photos au clic
- Gestion des paliers de décompression
- Filtrage par type de plongée (enseignement, exploration, nuit, épave, nitrox, trimix)
- Validation des données (date future bloquée, champs obligatoires, numéro de rue limité)
- Modal de confirmation avant suppression

### Profil utilisateur
- Affichage du profil avec photo
- Modification du mot de passe
- Suppression de compte avec nettoyage des fichiers

### UX / Interface
- Design responsive mobile-first (Tailwind CSS)
- Burger menu mobile avec sidebar
- Indicateurs visuels des champs obligatoires
- Messages d'erreur contextuels

---

## Sécurité

- Authentification stateless par **JWT**
- Mots de passe hashés avec **BCrypt**
- **Spring Security** sur tous les endpoints protégés
- **CORS** configuré pour les origines autorisées
- Secrets externalisés en **variables d'environnement** (jamais dans le code)
- Validation des données côté back ET front
- Validation email par RegEx côté back et front

---

## Déploiement

L'application est déployée via **Docker Compose** avec 3 containers :

```yaml
services:
  db:        # PostgreSQL 16.3
  backend:   # Spring Boot (multi-stage Maven build)
  frontend:  # Angular + Nginx (multi-stage Node build)
```

### Git Flow
```
feat/* ou fix/*
       ↓ PR
    develop  ← recette (Raspberry Pi)
       ↓ PR
      main   ← production (VPS OVH)
```

---

## Roadmap V2

- [ ] Page modification de profil
- [ ] Validation complète Problem Details RFC 7807
- [ ] Certifications FFESSM / PADI
- [ ] Carte interactive des sites de plongée (Leaflet)
- [ ] Réseau social entre plongeurs
- [ ] Validation de plongées par QR Code (sessions moniteur)
- [ ] Abonnements clubs / moniteurs (Stripe)
- [ ] Reconnaissance d'espèces marines (iNaturalist API)
- [ ] OAuth2 Google
- [ ] PWA mobile
- [ ] CI/CD GitHub Actions
- [ ] RGPD complet

---

## Profil développeur

Ce projet est développé par **Grégoire Dubois**, développeur Java backend avec une double expertise :
- ~2 ans de développement Java/Spring Boot
- ~10 ans de gestion de sinistres en assurance IARD (MRH, MRI, MRP, flottes auto)

---

## 📬 Contact

- **GitHub :** [Gregoire-Dubois](https://github.com/Gregoire-Dubois)
- **Email :** gregoire.dominiquedubois@gmail.com
