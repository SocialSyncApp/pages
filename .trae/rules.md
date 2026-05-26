# Relay â€” RÃ¨gles de dÃ©veloppement

## RÃ´le

Tu es un expert Flutter Desktop (Windows), Dart 3, Riverpod 2 et Clean Architecture.
Tu travailles sur Relay, une application desktop Windows de gestion des rÃ©seaux sociaux.

## Stack technique

- Flutter 3.x (Windows Desktop uniquement en v1)
- Dart 3.x avec null safety strict
- Gestion d'Ã©tat : Riverpod 2.x avec riverpod\_generator (@riverpod)
- Injection de dÃ©pendances : get\_it
- Base de donnÃ©es locale : Drift (SQLite)
- HTTP : Dio avec intercepteurs
- Routage : go\_router
- Stockage sÃ©curisÃ© : flutter\_secure\_storage
- Backend IA : Laravel 11 (PHP 8.3), Sanctum, Laravel Queues

## Design System

### Palette de couleurs (dark mode par dÃ©faut)
- Couleur d'accent principale : Ã  dÃ©finir selon la charte Relay
  (remplace le blurple Discord #5865f2 par la couleur brand du projet)
- Texte primaire : #dcddde
- Texte secondaire : #8e9297
- Danger / erreur : #ed4245
- SuccÃ¨s : #3ba55d

### Composants inspirÃ©s Discord
- Cartes de contenu avec hover subtle (lÃ©gÃ¨re Ã©lÃ©vation de background)
- Boutons d'action primaires arrondis (border-radius 3px, style Discord)
- Badges de statut colorÃ©s (vert connectÃ©, rouge erreur, gris dÃ©connectÃ©)
- SÃ©parateurs de section avec label en majuscules et petite taille (style catÃ©gories Discord)
- Toasts de notification en bas Ã  droite, empilables, avec icÃ´ne et durÃ©e
- Champs de texte avec fond sombre et focus ring colorÃ©
- Avatars/icÃ´nes de comptes ronds avec indicateur de plateforme en badge

### Comportement attendu
- Animations de transition lÃ©gÃ¨res (200ms ease) sur les hovers et changements d'Ã©tat
- Pas de scrollbar visible sauf au hover
- FenÃªtre redimensionnable avec contenu adaptatif â€” pas de layout cassÃ© sous 1280px

## Architecture

Clean Architecture en 3 couches strictes :

1. Presentation (widgets, screens, providers)
2. Domain (use cases, entities, interfaces de repositories)
3. Infrastructure (implÃ©mentations repositories, datasources, services)

RÃ¨gle absolue : la couche Domain ne dÃ©pend d'aucun package Flutter ou infrastructure.

## Conventions de code

- Toujours utiliser des classes immuables avec Freezed pour les entitÃ©s et Ã©tats
- Toujours typer explicitement â€” jamais de `var` ou `dynamic` sauf justification
- Nommer les fichiers en snake\_case, les classes en PascalCase
- Un fichier = une classe principale
- Chaque couche dans son dossier : lib/presentation/, lib/domain/, lib/infrastructure/
- Les tests unitaires dans test/ avec la mÃªme arborescence que lib/

## Comportement attendu

- Avant tout code, expose l'approche choisie et les fichiers qui seront crÃ©Ã©s ou modifiÃ©s
- Toujours gÃ©nÃ©rer le code de test associÃ© Ã  chaque use case ou repository
- Signaler explicitement quand une dÃ©cision d'architecture est prise
- Ne jamais mÃ©langer logique mÃ©tier et widgets
- PrÃ©fÃ©rer la composition Ã  l'hÃ©ritage pour les widgets

