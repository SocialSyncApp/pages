# Relay â€” Cahier des charges

**Application desktop de gestion des rÃ©seaux sociaux**

| Champ | Valeur |
|---|---|
| **Version** | 1.0 â€” Initial |
| **Date** | Mai 2026 |
| **Plateforme cible** | Windows 10/11 (Flutter Desktop) |
| **Cible utilisateur** | PME, agences, crÃ©ateurs â€” marchÃ© francophone |

---

## Table des matiÃ¨res

1. [PrÃ©sentation gÃ©nÃ©rale du projet](#1-prÃ©sentation-gÃ©nÃ©rale-du-projet)
2. [PÃ©rimÃ¨tre fonctionnel](#2-pÃ©rimÃ¨tre-fonctionnel)
3. [Module Intelligence Artificielle (Premium)](#3-module-intelligence-artificielle-premium)
4. [ModÃ¨le de monÃ©tisation](#4-modÃ¨le-de-monÃ©tisation)
5. [Architecture technique](#5-architecture-technique)
6. [IntÃ©grations API des rÃ©seaux sociaux](#6-intÃ©grations-api-des-rÃ©seaux-sociaux)
7. [SÃ©curitÃ© et confidentialitÃ© des donnÃ©es](#7-sÃ©curitÃ©-et-confidentialitÃ©-des-donnÃ©es)
8. [ExpÃ©rience utilisateur (UX)](#8-expÃ©rience-utilisateur-ux)
9. [Performances et contraintes techniques](#9-performances-et-contraintes-techniques)
10. [Plan de dÃ©veloppement](#10-plan-de-dÃ©veloppement)
11. [Risques et plan de mitigation](#11-risques-et-plan-de-mitigation)
12. [Glossaire](#12-glossaire)

---

## 1. PrÃ©sentation gÃ©nÃ©rale du projet

### 1.1 Contexte et problÃ©matique

Les professionnels du marketing digital â€” agences, PME, crÃ©ateurs de contenu indÃ©pendants â€” gÃ¨rent quotidiennement plusieurs comptes sur de multiples plateformes sociales. Cette gestion fragmentÃ©e entraÃ®ne une perte de temps considÃ©rable, des incohÃ©rences dans la ligne Ã©ditoriale, et un risque Ã©levÃ© d'oublis ou de doublons.

Les outils existants (Hootsuite, Buffer, Sprout Social) sont majoritairement des solutions web avec un modÃ¨le par abonnement mensuel obligatoire, ce qui reprÃ©sente une charge financiÃ¨re rÃ©currente Ã©levÃ©e pour les structures modestes ou les indÃ©pendants. De plus, ces outils stockent les donnÃ©es et tokens d'authentification sur des serveurs tiers, ce qui soulÃ¨ve des prÃ©occupations lÃ©gitimes en matiÃ¨re de confidentialitÃ©.

### 1.2 Objectifs du projet

Relay est une application desktop native Windows dÃ©veloppÃ©e en Flutter, conÃ§ue pour centraliser la crÃ©ation, la planification et la publication de contenus sur plusieurs rÃ©seaux sociaux depuis un environnement local sÃ©curisÃ©.

Les objectifs principaux sont :

- Offrir une interface unifiÃ©e de crÃ©ation et de planification de posts multi-plateformes.
- Stocker les donnÃ©es et tokens d'authentification localement sur le poste de l'utilisateur.
- Proposer un modÃ¨le Ã©conomique flexible combinant version gratuite, licence Ã  achat unique et abonnement premium.
- IntÃ©grer des fonctionnalitÃ©s d'intelligence artificielle pour les utilisateurs premium.
- Concevoir une architecture extensible permettant l'ajout de nouvelles plateformes et une portabilitÃ© future vers macOS et Linux.

### 1.3 Positionnement et diffÃ©renciateurs

| CritÃ¨re | DÃ©tail |
|---|---|
| **Axe** | Positionnement Relay |
| **ModÃ¨le Ã©conomique** | Achat unique disponible â€” pas d'abonnement obligatoire pour les fonctions core |
| **Stockage des donnÃ©es** | 100 % local â€” tokens et posts stockÃ©s sur le poste utilisateur |
| **Plateforme** | Application native Windows via Flutter â€” expÃ©rience fluide hors navigateur |
| **Cible gÃ©ographique** | MarchÃ© francophone : Afrique, France, Belgique, Canada francophone |
| **IA intÃ©grÃ©e** | Module IA premium pour gÃ©nÃ©ration de contenu et suggestions Ã©ditoriales |
| **ExtensibilitÃ©** | Architecture modulaire pensÃ©e pour l'ajout de plateformes et le portage cross-platform |

---

## 2. PÃ©rimÃ¨tre fonctionnel

### 2.1 Plateformes sociales supportÃ©es (v1)

La version 1.0 de Relay intÃ©grera les quatre plateformes suivantes, sÃ©lectionnÃ©es pour leur prÃ©pondÃ©rance dans les stratÃ©gies de marketing digital :

| Plateforme | DÃ©tail d'intÃ©gration |
|---|---|
| **Meta (Facebook / Instagram)** | Publication de posts textuels, images et carrousels via le Graph API v18+. Gestion de pages et comptes professionnels. Support des Reels Instagram en v1.1. |
| **LinkedIn** | Publication sur profils personnels et pages entreprises via l'API LinkedIn v2. AdaptÃ© aux contenus B2B et professionnels. |
| **X (Twitter)** | Publication de tweets via l'API X v2. Support des threads en v1.1. Authentification OAuth 2.0. |
| **TikTok** | Publication de vidÃ©os courtes via le Content Posting API TikTok. Authentification via le TikTok Login Kit. |

> **Note importante :** L'accÃ¨s aux APIs des rÃ©seaux sociaux est soumis Ã  des conditions d'utilisation et potentiellement Ã  des frais selon les plateformes (notamment X/Twitter qui facture l'accÃ¨s API Basic). Ces coÃ»ts devront Ãªtre anticipÃ©s dans le budget de dÃ©veloppement et potentiellement rÃ©percutÃ©s dans les conditions d'utilisation de Relay.

### 2.2 Module Ã‰diteur de post

#### 2.2.1 FonctionnalitÃ©s de composition

- Zone de texte principale avec compteur de caractÃ¨res adaptatif par plateforme (280 pour X, 2 200 pour LinkedIn, etc.)
- SÃ©lecteur de mÃ©dias : import d'images (JPEG, PNG, GIF, WebP) et vidÃ©os (MP4, MOV) depuis le systÃ¨me de fichiers local
- PrÃ©visualisation en temps rÃ©el du rendu pour chaque plateforme ciblÃ©e
- SÃ©lection multi-plateformes : publier le mÃªme contenu sur plusieurs rÃ©seaux en une opÃ©ration
- BibliothÃ¨que de hashtags : suggestions basÃ©es sur l'historique et le contenu (basic) ou par IA (premium)
- Gestion des emojis : panneau de sÃ©lection intÃ©grÃ©
- Sauvegarde automatique en brouillon

#### 2.2.2 Gestion des mÃ©dias

- Compression automatique des images selon les spÃ©cifications de chaque plateforme
- AperÃ§u du ratio avant publication (1:1, 16:9, 4:5, 9:16)
- Cache local des mÃ©dias rÃ©cemment utilisÃ©s
- Vignettes et galerie de mÃ©dias rÃ©utilisables

### 2.3 Module Calendrier et planification

- Vue calendrier mensuelle et hebdomadaire avec aperÃ§u des posts planifiÃ©s par couleur selon la plateforme
- Planification d'une publication Ã  une date et heure prÃ©cises
- File d'envoi avec statuts : Brouillon, PlanifiÃ©, En cours d'envoi, PubliÃ©, Ã‰chec
- Retry automatique en cas d'Ã©chec d'envoi (3 tentatives espacÃ©es de 15 minutes)
- Suggestions d'horaires optimaux basÃ©es sur les donnÃ©es de performance passÃ©es (feature premium via IA)
- Duplication de posts pour republication
- Recherche et filtrage des posts planifiÃ©s par plateforme, statut, mots-clÃ©s

### 2.4 Module Gestion des comptes

- Ajout et suppression de comptes par plateforme via flux OAuth 2.0
- Connexion simultanÃ©e Ã  plusieurs comptes par plateforme (ex : 3 pages Facebook diffÃ©rentes)
- Statut de connexion en temps rÃ©el : ConnectÃ©, Token expirÃ©, Erreur de permission
- Renouvellement automatique des tokens d'accÃ¨s lorsque possible
- Attribution de libellÃ©s et couleurs aux comptes pour identification rapide

### 2.5 Module Analytics

- Tableau de bord global : portÃ©e, impressions, engagements, clics agrÃ©gÃ©s par pÃ©riode
- Graphiques de performance par plateforme et par compte
- Classement des posts les plus performants
- Export des rapports en PDF et CSV
- RafraÃ®chissement manuel des statistiques (les APIs n'offrent pas toutes un temps rÃ©el)

---

## 3. Module Intelligence Artificielle (Premium)

### 3.1 PÃ©rimÃ¨tre du module IA

Le module IA est exclusivement disponible dans le plan Premium. Il s'appuie sur un LLM via API externe (Claude API d'Anthropic ou OpenAI GPT-4o en fonction des conditions tarifaires). Les appels API sont routÃ©s via un serveur backend Relay afin de ne pas exposer les clÃ©s API cÃ´tÃ© client.

### 3.2 FonctionnalitÃ©s IA

| FonctionnalitÃ© | DisponibilitÃ© | PrioritÃ© |
|---|---|---|
| GÃ©nÃ©ration de post | Premium | P1 â€” v1.0 |
| Suggestions de hashtags | Premium | P1 â€” v1.0 |
| Reformulation de ton | Premium | P1 â€” v1.0 |
| Suggestions d'horaires optimaux | Premium | P2 â€” v1.1 |
| Analyse de sentiment du contenu | Premium | P2 â€” v1.1 |
| GÃ©nÃ©ration de description d'image | Premium | P2 â€” v1.1 |
| Traduction automatique du post | Premium | P3 â€” v2.0 |

### 3.3 DÃ©tail des fonctionnalitÃ©s IA prioritaires

#### 3.3.1 GÃ©nÃ©ration de post

L'utilisateur fournit un sujet, un ton (professionnel, dÃ©contractÃ©, inspirant, humoristique) et les plateformes cibles. Le module gÃ©nÃ¨re une ou plusieurs propositions de post adaptÃ©es aux contraintes de chaque plateforme (longueur, style, hashtags). L'utilisateur peut rÃ©gÃ©nÃ©rer, modifier ou accepter le contenu proposÃ©.

#### 3.3.2 Suggestions de hashtags

Ã€ partir du texte rÃ©digÃ© dans l'Ã©diteur, le module analyse le contenu et propose une liste de hashtags pertinents classÃ©s par niveau de popularitÃ© (niche, moyen, large). L'utilisateur sÃ©lectionne les hashtags Ã  insÃ©rer d'un clic.

#### 3.3.3 Reformulation de ton

Permet de transformer un texte existant selon un ton cible sÃ©lectionnÃ© par l'utilisateur. Utile pour adapter un contenu initialement formel Ã  une audience plus dÃ©tendue (ou inversement), ou pour dÃ©cliner un mÃªme message en plusieurs versions stylistiques.

---

## 4. ModÃ¨le de monÃ©tisation

### 4.1 Structure des plans

| FonctionnalitÃ© | Gratuit | Pro (licence) | Premium (abo.) |
|---|---|---|---|
| **Comptes connectÃ©s** | 2 maximum | IllimitÃ© | IllimitÃ© |
| **Posts / mois** | 10 posts | IllimitÃ© | IllimitÃ© |
| **Planification** | Oui | Oui | Oui |
| **AperÃ§u multi-plateforme** | Oui | Oui | Oui |
| **BibliothÃ¨que de mÃ©dias** | 50 Mo | 5 Go | 5 Go |
| **Analytics basiques** | Non | Oui | Oui |
| **Export rapports** | Non | PDF/CSV | PDF/CSV |
| **Module IA complet** | Non | Non | Oui |
| **Suggestions d'horaires IA** | Non | Non | Oui |
| **Support prioritaire** | Non | Email | Email + Chat |
| **Mises Ã  jour** | LimitÃ©es | Majeures incluses 2 ans | Toujours Ã  jour |

### 4.2 Tarification indicative

| Plan | Tarification |
|---|---|
| **Gratuit** | 0 â‚¬ â€” tÃ©lÃ©chargement libre, aucune carte bancaire requise |
| **Pro â€” licence** | Achat unique entre 29 â‚¬ et 49 â‚¬ (Ã  affiner selon Ã©tude de marchÃ©). Licence perpÃ©tuelle. 2 ans de mises Ã  jour majeures incluses. |
| **Premium â€” abonnement** | Entre 9 â‚¬ et 15 â‚¬ / mois, ou 79 â‚¬ Ã  99 â‚¬ / an (2 mois offerts). Inclut le plan Pro + module IA + analytics avancÃ©s. |
| **Licences Ã©quipe** | Option multi-siÃ¨ges envisagÃ©e en v2 : tarif dÃ©gressif Ã  partir de 5 utilisateurs. |

> **StratÃ©gie d'acquisition :** La version gratuite sert de levier d'acquisition principal. L'objectif est un taux de conversion Gratuit â†’ Pro de 8 Ã  12 %, et Gratuit/Pro â†’ Premium de 15 Ã  20 % sur 6 mois.

---

## 5. Architecture technique

### 5.1 Stack technologique

| Composant | Choix technique |
|---|---|
| **Framework UI** | Flutter 3.x â€” Desktop Windows (win32). Extensible vers macOS et Linux sans rÃ©Ã©criture majeure. |
| **Langage** | Dart 3.x â€” typage fort, null safety, performances natives. |
| **Base de donnÃ©es locale** | SQLite via le package Drift (ancien Moor). ORM typÃ©, migrations versionnÃ©es, requÃªtes rÃ©actives. |
| **Stockage sÃ©curisÃ©** | flutter_secure_storage â€” stockage chiffrÃ© des tokens OAuth via Windows Credential Manager (DPAPI). |
| **Cache mÃ©dias** | SystÃ¨me de fichiers local dans le rÃ©pertoire AppData de l'utilisateur. Gestion de la taille maximale configurable. |
| **HTTP / APIs** | Package Dio pour les appels HTTP avec intercepteurs (refresh token, logs, retry). |
| **Gestion d'Ã©tat** | Riverpod 2.x â€” architecture Provider/Notifier, code gÃ©nÃ©rÃ© par riverpod_generator. |
| **Injection de dÃ©pendances** | get_it â€” service locator pour les repositories et services. |
| **Routage** | go_router â€” navigation dÃ©clarative, deep links, guards d'authentification. |
| **Tests** | flutter_test (unitaires), mockito (mocks), integration_test (tests E2E). |
| **Backend IA (cloud)** | Serveur Laravel 11 (PHP 8.3) hÃ©bergÃ© sur un PaaS (Railway, Render ou Fly.io). API RESTful sÃ©curisÃ©e avec Sanctum. Proxy sÃ©curisÃ© vers Claude API / OpenAI. Jobs asynchrones via Laravel Queues (Redis ou database driver). |
| **Gestion des licences** | SystÃ¨me de clÃ©s de licence via backend. Validation en ligne Ã  l'activation, mode hors-ligne jusqu'Ã  30 jours. |

### 5.2 Architecture en couches

#### Couche 1 â€” PrÃ©sentation (UI)

Contient tous les widgets Flutter, les Ã©crans, les composants rÃ©utilisables et les thÃ¨mes. Ne contient aucune logique mÃ©tier directe. Communique exclusivement avec la couche logique via des Providers Riverpod.

- **Ã‰crans principaux :** Ã‰diteur, Calendrier, Analytics, Comptes, ParamÃ¨tres
- **Composants partagÃ©s :** PostCard, PlatformBadge, MediaPicker, AIAssistPanel
- **ThÃ¨me :** support du mode clair et sombre, palette cohÃ©rente

#### Couche 2 â€” Logique mÃ©tier (Domain)

Contient les use cases, les entitÃ©s mÃ©tier et les interfaces de repositories. Cette couche est indÃ©pendante de Flutter et peut Ãªtre testÃ©e en Dart pur.

- **Use cases :** CreatePost, SchedulePost, PublishPost, RefreshToken, GenerateAIContent
- **EntitÃ©s :** Post, SocialAccount, MediaAsset, ScheduledJob, LicenseInfo
- **Interfaces :** IPostRepository, IAccountRepository, IAIService, ILicenseService

#### Couche 3 â€” DonnÃ©es (Infrastructure)

ImplÃ©mentations concrÃ¨tes des repositories. Contient les Data Sources (SQLite local, APIs distantes), les mappers entre modÃ¨les de donnÃ©es et entitÃ©s, et les services systÃ¨me.

- **LocalPostDataSource :** CRUD SQLite via Drift
- **RemotePostDataSource :** appels aux APIs Meta, LinkedIn, X, TikTok
- **SecureTokenStorage :** flutter_secure_storage
- **AIRemoteService :** appels au backend IA Relay

### 5.3 SchÃ©ma de base de donnÃ©es locale

| Table | Colonnes principales |
|---|---|
| **posts** | id, content, platforms (JSON), status, scheduled_at, published_at, created_at, updated_at |
| **social_accounts** | id, platform, account_name, account_id, display_color, created_at |
| **tokens** | StockÃ© hors SQLite via flutter_secure_storage â€” clÃ© : `token_{platform}_{account_id}` |
| **media_assets** | id, local_path, file_type, file_size, thumbnail_path, created_at |
| **post_media** | post_id, media_id â€” table de jonction N:N |
| **analytics_snapshots** | id, account_id, platform, date, reach, impressions, engagements, clicks |
| **scheduled_jobs** | id, post_id, scheduled_at, attempts, last_attempt_at, status, error_message |
| **app_settings** | key, value â€” table de configuration gÃ©nÃ©rale |

---

## 6. IntÃ©grations API des rÃ©seaux sociaux

### 6.1 Flux d'authentification OAuth 2.0

Toutes les plateformes utilisent OAuth 2.0. Le flux est le suivant pour chaque connexion de compte :

1. L'utilisateur clique sur "Connecter un compte" et sÃ©lectionne la plateforme.
2. L'application ouvre le navigateur systÃ¨me (ou une WebView embarquÃ©e) vers l'URL d'autorisation de la plateforme.
3. L'utilisateur s'authentifie sur la plateforme et accorde les permissions.
4. La plateforme redirige vers un scheme URI personnalisÃ© (ex : `Relay://oauth/callback`).
5. L'application intercepte le callback, extrait le code d'autorisation et Ã©change contre un access token + refresh token.
6. Les tokens sont stockÃ©s chiffrÃ©s via flutter_secure_storage.
7. L'account record est crÃ©Ã© en base SQLite locale.

### 6.2 Permissions requises par plateforme

| Plateforme | Scopes OAuth requis |
|---|---|
| **Meta (Facebook)** | pages_manage_posts, pages_read_engagement, instagram_content_publish, instagram_basic |
| **Meta (Instagram)** | instagram_content_publish, instagram_basic, instagram_manage_insights |
| **LinkedIn** | w_member_social, r_basicprofile, rw_organization_admin (pour pages entreprises) |
| **X (Twitter)** | tweet.write, tweet.read, users.read, offline.access |
| **TikTok** | video.publish, video.list, user.info.basic |

### 6.3 Gestion des limites de taux (Rate Limiting)

- Chaque appel API est encapsulÃ© dans un intercepteur Dio qui inspecte les headers de rate limit retournÃ©s.
- En cas de dÃ©passement (HTTP 429), un mÃ©canisme de backoff exponentiel est appliquÃ© : 1s, 2s, 4s, 8s, max 3 tentatives.
- Les jobs planifiÃ©s s'espacent automatiquement pour respecter les quotas journaliers de chaque plateforme.
- Un tableau de bord de santÃ© API est visible dans les paramÃ¨tres avancÃ©s.

---

## 7. SÃ©curitÃ© et confidentialitÃ© des donnÃ©es

### 7.1 Principes de sÃ©curitÃ©

Relay est conÃ§u selon une approche privacy-by-design. L'ensemble des donnÃ©es utilisateur (posts, brouillons, comptes, mÃ©dias) rÃ©side exclusivement sur le poste local de l'utilisateur.

| Ã‰lÃ©ment | Mesure de sÃ©curitÃ© |
|---|---|
| **Tokens OAuth** | ChiffrÃ©s via Windows DPAPI via flutter_secure_storage. Jamais stockÃ©s en clair ni en SQLite. |
| **DonnÃ©es SQLite** | Base de donnÃ©es dans le rÃ©pertoire `AppData\Roaming\Relay`. Chiffrement optionnel SQLCipher en v1.1. |
| **Communication rÃ©seau** | HTTPS exclusivement (TLS 1.2+). Certificate pinning pour les endpoints du backend IA Relay. |
| **ClÃ©s API tierces** | Aucune clÃ© API tierce n'est stockÃ©e dans l'application. Routage via le backend Relay pour le module IA. |
| **Logs** | Logs d'erreur anonymisÃ©s. Aucun contenu de post ou token n'est loggÃ©. Option de dÃ©sactivation totale des logs. |
| **Mises Ã  jour** | Signature des packages de mise Ã  jour. VÃ©rification de l'intÃ©gritÃ© avant installation. |

### 7.2 ConformitÃ© RGPD

- Les donnÃ©es personnelles restent locales â€” pas de collecte cÃ´tÃ© serveur Relay sauf pour la validation de licence.
- Politique de confidentialitÃ© claire dÃ©crivant les donnÃ©es collectÃ©es lors de l'activation de la licence.
- Option d'export et de suppression des donnÃ©es locales depuis les paramÃ¨tres.
- Aucun tracker analytique dans l'application sans consentement explicite.

---

## 8. ExpÃ©rience utilisateur (UX)

### 8.1 Principes de design

- Interface Ã©purÃ©e, minimaliste, focalisÃ©e sur la productivitÃ© â€” pas de sur-chargement visuel.
- Navigation principale par sidebar gauche avec accÃ¨s direct aux 5 modules.
- Mode sombre et mode clair avec dÃ©tection automatique du thÃ¨me systÃ¨me Windows.
- Raccourcis clavier pour les actions frÃ©quentes (Ctrl+N nouveau post, Ctrl+S sauvegarder brouillon, etc.).
- Toasts et notifications discrÃ¨tes pour les confirmations d'actions et alertes.

### 8.2 Ã‰crans principaux

| Ã‰cran | Description |
|---|---|
| **Tableau de bord** | Vue synthÃ©tique : posts planifiÃ©s aujourd'hui, alertes de comptes, derniÃ¨res statistiques. |
| **Ã‰diteur de post** | Zone de composition principale. Panneau de prÃ©visualisation multi-plateforme Ã  droite. Panneau IA rÃ©tractable. |
| **Calendrier** | Vue mensuelle/hebdomadaire. Drag & drop pour repositionner les posts planifiÃ©s. |
| **File d'envoi** | Liste des posts planifiÃ©s et leur statut. Filtres et actions en masse. |
| **Analytics** | Graphiques par plateforme. SÃ©lecteur de pÃ©riode. Export. |
| **Comptes** | Grille des comptes connectÃ©s. Statut OAuth. Boutons connecter/dÃ©connecter. |
| **ParamÃ¨tres** | Langue, thÃ¨me, notifications, licences, gestion des donnÃ©es, logs. |

### 8.3 Onboarding et premier lancement

- Ã‰cran de bienvenue avec prÃ©sentation des 3 plans.
- Connexion d'au moins un compte social (guide pas Ã  pas).
- CrÃ©ation d'un premier post de dÃ©monstration.
- Visite guidÃ©e optionnelle des modules principaux (tooltip tour).

---

## 9. Performances et contraintes techniques

### 9.1 Configuration minimale requise

| CritÃ¨re | SpÃ©cification |
|---|---|
| **SystÃ¨me d'exploitation** | Windows 10 64-bit (version 1903 ou ultÃ©rieure) / Windows 11 |
| **Processeur** | Intel Core i3 ou AMD Ryzen 3 â€” 2 GHz minimum |
| **RAM** | 4 Go minimum, 8 Go recommandÃ©s |
| **Espace disque** | 200 Mo pour l'installation + espace pour le cache mÃ©dias (configurable) |
| **RÃ©solution Ã©cran** | 1280 Ã— 720 minimum, optimisÃ© pour 1920 Ã— 1080 |
| **Connexion internet** | Requise pour la publication et la synchronisation des analytics. Mode hors-ligne partiel pour la rÃ©daction. |

### 9.2 Objectifs de performance

| Action | Objectif |
|---|---|
| **DÃ©marrage de l'application** | < 3 secondes sur configuration recommandÃ©e |
| **Ouverture de l'Ã©diteur de post** | < 500 ms |
| **Chargement du calendrier (1 mois)** | < 1 seconde |
| **Envoi d'un post simple (texte)** | < 5 secondes par plateforme |
| **GÃ©nÃ©ration IA d'un post** | < 8 secondes (dÃ©pendant de la latence rÃ©seau et du LLM) |
| **Chargement des analytics (30 jours)** | < 3 secondes (donnÃ©es en cache local) |
| **Consommation mÃ©moire au repos** | < 150 Mo RAM |

---

## 10. Plan de dÃ©veloppement

### 10.1 DÃ©coupage en phases

| Phase | Contenu |
|---|---|
| **Phase 0 â€” Setup** (2 semaines) | Mise en place du projet Flutter, architecture de base, CI/CD (GitHub Actions), design system, maquettes Figma. |
| **Phase 1 â€” Core** (8 semaines) | Ã‰diteur de post, gestion des comptes (OAuth), stockage local SQLite, publication manuelle sur les 4 plateformes, gestion des brouillons. |
| **Phase 2 â€” Planification** (4 semaines) | Scheduler, calendrier, file d'envoi, retry automatique, notifications Windows. |
| **Phase 3 â€” Analytics** (3 semaines) | RÃ©cupÃ©ration des stats par plateforme, tableau de bord, graphiques, export PDF/CSV. |
| **Phase 4 â€” IA & Premium** (4 semaines) | Backend IA Laravel 11, intÃ©gration Claude API, gÃ©nÃ©ration de posts, suggestions hashtags, reformulation de ton. DÃ©ploiement sur PaaS, configuration des queues et du cache Redis. |
| **Phase 5 â€” MonÃ©tisation** (3 semaines) | SystÃ¨me de licences, gestion des plans (Free/Pro/Premium), page d'activation, paywall UX. |
| **Phase 6 â€” QA & Lancement** (2 semaines) | Tests E2E, beta fermÃ©e, corrections, packaging MSIX, publication sur le site et stores. |

> **DurÃ©e totale estimÃ©e :** 26 semaines (environ 6 mois) pour une Ã©quipe d'un dÃ©veloppeur principal. Ce dÃ©lai peut Ãªtre rÃ©duit Ã  4 mois avec une Ã©quipe de 2 dÃ©veloppeurs Flutter.

### 10.2 PrioritÃ©s de dÃ©veloppement (MoSCoW)

| FonctionnalitÃ© | CatÃ©gorie | PrioritÃ© |
|---|---|---|
| Ã‰diteur de post multi-plateforme | Must Have | P0 |
| Authentification OAuth 4 plateformes | Must Have | P0 |
| Publication immÃ©diate | Must Have | P0 |
| Sauvegarde brouillons locale | Must Have | P0 |
| Planification de posts | Must Have | P1 |
| Calendrier visuel | Should Have | P1 |
| Analytics basiques | Should Have | P1 |
| Module IA (gÃ©nÃ©ration, hashtags) | Should Have | P1 |
| Export rapports PDF/CSV | Could Have | P2 |
| Drag & drop dans le calendrier | Could Have | P2 |
| Threads X (Twitter) | Could Have | P2 |
| Mode hors-ligne avancÃ© | Won't Have v1 | P3 |
| Application mobile (iOS/Android) | Won't Have v1 | P3 |
| Collaboration multi-utilisateurs | Won't Have v1 | P3 |

---

## 11. Risques et plan de mitigation

| Risque | Niveau | Mitigation |
|---|---|---|
| Restriction ou modification des APIs rÃ©seaux sociaux | **Ã‰levÃ©** | Architecture modulaire avec couche d'abstraction par plateforme. Veille active sur les changelogs API. Communiquer les limitations aux utilisateurs. |
| CoÃ»t d'accÃ¨s Ã  l'API X (Twitter) Ã©levÃ© | **Ã‰levÃ©** | Passer l'intÃ©gration X en fonctionnalitÃ© Pro/Premium uniquement pour absorber le coÃ»t. |
| Concurrence des outils Ã©tablis (Buffer, Hootsuite) | **Moyen** | Se diffÃ©rencier sur le modÃ¨le achat unique, la confidentialitÃ© locale et la cible francophone. |
| Latence ou coÃ»t du LLM pour le module IA | **Moyen** | Cache des rÃ©ponses IA courantes. Quota mensuel par utilisateur Premium. Choix du LLM le moins cher Ã  qualitÃ© Ã©gale. |
| Fraude sur les licences | **Moyen** | Validation en ligne Ã  l'activation. Mode offline limitÃ© Ã  30 jours. Hardware fingerprinting lÃ©ger. |
| ComplexitÃ© de la gestion OAuth sur Windows | **Faible** | flutter_secure_storage mature et bien testÃ©. Tests E2E sur Windows rÃ©els. |

---

## 12. Glossaire

| Terme | DÃ©finition |
|---|---|
| **API** | Application Programming Interface â€” interface permettant Ã  deux logiciels de communiquer. |
| **DPAPI** | Data Protection API â€” mÃ©canisme Windows de chiffrement des donnÃ©es liÃ©es Ã  un compte utilisateur. |
| **Flutter** | Framework open-source de Google pour crÃ©er des applications natives multiplateformes depuis une seule base de code Dart. |
| **Laravel** | Framework PHP open-source (v11) orientÃ© API REST, incluant Eloquent ORM, Sanctum pour l'authentification, et Laravel Queues pour le traitement asynchrone. |
| **LLM** | Large Language Model â€” modÃ¨le de langage de grande taille utilisÃ© pour les fonctionnalitÃ©s d'intelligence artificielle. |
| **MoSCoW** | MÃ©thode de priorisation : Must Have, Should Have, Could Have, Won't Have. |
| **MSIX** | Format de package de dÃ©ploiement Windows moderne, supportÃ© par le Microsoft Store. |
| **OAuth 2.0** | Protocole d'autorisation standard permettant Ã  une application d'accÃ©der Ã  des ressources en ligne au nom d'un utilisateur, sans manipuler son mot de passe. |
| **ORM** | Object-Relational Mapping â€” technique permettant de manipuler une base de donnÃ©es via des objets dans le code. |
| **RGPD** | RÃ¨glement GÃ©nÃ©ral sur la Protection des DonnÃ©es â€” rÃ©glementation europÃ©enne sur la vie privÃ©e. |
| **Riverpod** | BibliothÃ¨que de gestion d'Ã©tat pour Flutter, basÃ©e sur les concepts de Provider. |
| **SQLite** | Moteur de base de donnÃ©es relationnelle intÃ©grÃ©, stockant les donnÃ©es dans un fichier local. |
| **Token OAuth** | Jeton d'accÃ¨s dÃ©livrÃ© par une plateforme aprÃ¨s authentification, permettant d'agir au nom de l'utilisateur. |

---

*Confidentiel â€” v2.0 | Mai 2026*

*â€” Fin du document â€”*
