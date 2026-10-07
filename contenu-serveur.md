# Contenu technique — Présentation « Mon serveur & mon usage de l'IA »

> Contenu prêt à intégrer dans la présentation HTML de Kevin Leca — candidature poste testeur expérimenté (IA).

---

## Mon environnement technique

### Serveur

| Caractéristique | Valeur |
|-----------------|--------|
| OS | Ubuntu 26.04 LTS (VM isolée) |
| CPU | AMD Ryzen 5 5600X — 4 cœurs alloués |
| RAM | 7,1 Go |
| Stockage | 20 Go |
| Exposition réseau | Aucune exposition directe à Internet |
| Accès | Tailscale (mesh VPN) — 0 port ouvert publiquement |

- VM dédiée, entièrement administrée par moi-même (mise à jour, supervision, sauvegardes).
- Stack applicative déployée en Docker Compose : `/opt/server/docker-compose.yml` (7 services, réseau dédié `server_default`).

### Stack Docker

| Service | Rôle | Technologies |
|---------|------|-------------|
| Nginx | Reverse proxy, termination TLS | nginx:latest |
| node-app | Application portail | Express, Node.js 20 |
| PostgreSQL | Base de données | postgres:17 |
| Redis | Cache, sessions | redis:alpine |
| pgAdmin | Administration BDD | dpage/pgadmin4 |
| Portainer | Management Docker | portainer-ce |
| Netdata | Monitoring temps réel | netdata |

- 7 conteneurs orchestrés par Docker Compose, réseau isolé `server_default`.
- Code applicatif monté en lecture seule (`:ro`) dans Nginx → immutabilité de la config servie.
- Volumes persistants pour PostgreSQL et les sauvegardes.

### Architecture réseau

- **Tailscale mesh VPN** : serveur + devices familiaux sur un même réseau privé chiffré (WireGuard).
- **Nginx reverse proxy** : point d'entrée unique, HTTPS, routage vers les services internes (`/opt/server/nginx/default.conf`).
- **Isolation Docker** : chaque service dans son conteneur, communications limitées au réseau Compose dédié.
- **Sauvegardes PostgreSQL planifiées** : dumps automatisés sur volume persistant.

### Sécurité

- **HTTPS** : termination TLS sur Nginx (certificats Let's Encrypt / gestion des certificats).
- **Firewall** configuré sur la VM — aucun service exposé directement à Internet.
- **Isolation réseau Docker** : réseau `server_default`, pas de publication de ports inutiles.
- **Authentification** obligatoire sur les services d'administration (pgAdmin, Portainer).
- **Principe du moindre privilège** : code applicatif monté en lecture seule, fichiers de config centralisés dans `/opt/server/`.

---

## Mon workflow d'agents IA

### Orchestrateur : Jarvis

Workflow en 5 étapes, appliqué systématiquement à chaque demande :

1. **Réception** — Jarvis reçoit la demande utilisateur en langage naturel.
2. **Analyse & planification** — découpage en sous-tâches, choix des agents mobilisés.
3. **Délégation** — chaque sous-tâche confiée à l'agent spécialisé concerné (Jarvis ne code pas, il orchestre).
4. **Vérification** — contrôle systématique des livrables (tests exécutés, résultats chiffrés).
5. **Livraison** — livrable final consolidé + documentation.

### Agents spécialisés

| Agent | Rôle | Compétences | Outils |
|-------|------|-------------|--------|
| Pixel 🎨 | Frontend | React, Vue, TypeScript, Tailwind | Éditeur, navigateur |
| Rotor 🔧 | Backend | Node.js, Python, APIs, BDD | Terminal, Docker |
| Temper ⚡ | QA & Tests | Playwright, Jest, Cypress | Test runners |
| Bic 📝 | Documentation | Doxygen, Markdown, README | Éditeur |
| Turbo 🚀 | DevOps | Docker, CI/CD, GitHub Actions | Terminal, Git |
| Ronnie 🏋️ | Sport & Performance | Programmation, nutrition | Sheets |

### Points forts de l'approche

- **Expertise dédiée par agent** → chaque livrable produit par un agent spécialisé dans son domaine (qualité métier).
- **Mémoire persistante entre sessions** → continuité du contexte : conventions de nommage, arborescences, formats imposés conservés d'une session à l'autre.
- **Délégation inter-agents** → les tâches complexes (feature + tests + doc + CI/CD) sont traitées en chaîne sans perte d'information.
- **Vérification systématique** → chaque livrable est validé par l'exécution réelle des tests (taux de passage mesuré, pas déclaratif).
- **Documentation automatique** → README, CHANGELOG et plans de tests générés à chaque livraison → traçabilité complète.

---

## Réalisations concrètes

### Projet 1 : Liste de cadeaux (web)

- Refactoring complet par les agents (front, tests, doc) — HTML/CSS/JS vanilla, 0 dépendance npm.
- **22 tests Playwright automatisés** couvrant 5 familles : tri (4), filtres (5), dépendances (6), responsive (3), accessibilité (4) — **100 % passants**.
- **CI/CD GitHub Actions** : exécution des tests à chaque push + déploiement automatique.
- Déploiement en production sur **GitHub Pages**.
- Livrables annexes : plan de tests documenté, README, CHANGELOG.

### Projet 2 : Portail personnel

- Application **Express (Node.js 20) + PostgreSQL 17 + Redis** déployée en Docker Compose.
- **Reverse proxy Nginx avec HTTPS** (termination TLS, point d'entrée unique).
- **Monitoring temps réel Netdata** (métriques CPU, RAM, réseau, conteneurs).
- **Administration via Portainer** (gestion des conteneurs) et **pgAdmin** (gestion BDD).
- Code source monté en lecture seule dans Nginx, réseau Docker isolé.

### Projet 3 : Analyse de documents techniques (manuel technique)

- Analyse du **manuel manuel technique : 2 252 lignes** de spécifications.
- **874 tests de validation générés automatiquement** à partir des spécifications.
- Couverture structurée : **8 fonctions × 6 modules** (organisation matricielle module × fonction).
- Identifiants de test normalisés : `FCT_[description]` / `ERR_[code]` (discriminant en cas de doublon).
- Arborescence d'environnement de test standardisée : `Tables/`, `Tables/`, `Fichier_Config/`, `Librairies/`, `Jeux/` (chemins relatifs).
- Démarche d'erreurs maîtrisée : injection de variantes de tables → détection → restauration.
- **Format de sortie compatible avec l'outil de test interne** → tests directement exécutables dans la chaîne existante.

### Projet 4 : Intégration Google Workspace

- **4 API connectées** : Drive, Calendar, Sheets, Gmail.
- Automatisation de la gestion de fichiers (organisation Drive : arborescence `v2.0/Tests/MODULE_X/Fonction_Y/{PARAM,IN,OUT,REF}/`).
- Planification automatisée d'événements (agenda, convention de titres validée).
- Sheets pilotés par API (suivi sport et nutrition).

---

## Pertinence pour le poste de testeur

| Besoin du poste | Apport concret |
|-----------------|----------------|
| Productivité accrue | L'IA comme accélérateur : ce qui prenait des jours (874 tests) se génère en une session |
| Génération de tests | Tests générés automatiquement à partir de spécifications (projet technique : 2 252 lignes → 874 tests) |
| Organisation des campagnes | Structure matricielle module × fonction, identifiants normalisés (`FCT_`/`ERR_`) |
| Documentation & traçabilité | Plans de tests, README, CHANGELOG générés systématiquement à chaque livraison |
| Reproductibilité | Approche modulaire : environnement standardisé, chemins relatifs, formats compatibles outil interne |
| Fiabilité | Vérification systématique par exécution réelle : 22 tests Playwright, 100 % passants, CI/CD en garantie |
