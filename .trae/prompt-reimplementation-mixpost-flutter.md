# Prompt : RÃ©implÃ©mentation d'une application de gestion de rÃ©seaux sociaux (style Mixpost) â€” Flutter

## Contexte gÃ©nÃ©ral

Tu dois implÃ©menter une **application Flutter de gestion de rÃ©seaux sociaux** (Social Media Management Tool). L'application s'appelle **Social Sync** et est utilisÃ©e par des Ã©quipes marketing pour planifier, publier et analyser leur contenu sur plusieurs plateformes sociales.

L'interface doit Ãªtre **propre, professionnelle et Ã©purÃ©e** :
- Fond principal : `Color(0xFFF8F9FA)`
- Sidebar sombre : `Color(0xFF1E1B4B)`
- Accents violets/indigo : `Color(0xFF6366F1)` / `Color(0xFF4F46E5)`
- Texte principal : `Color(0xFF111827)`
- Texte secondaire : `Color(0xFF6B7280)`

Utilise des `BorderRadius.circular(12)` Ã  `BorderRadius.circular(16)` pour les cartes, des ombres lÃ©gÃ¨res (`BoxShadow`), et la police **DM Sans** ou **Outfit** (via `google_fonts`).

---

## Structure gÃ©nÃ©rale de l'application

### Layout principal (`ScaffoldWithSidebar`)

L'app tourne sur **desktop/tablette** (cible principale : Flutter Web ou tablette large). Utilise un layout `Row` permanent :

- **Sidebar gauche fixe** (largeur : `180px`) :
  - Logo + nom de l'app en haut (icÃ´ne carrÃ©e avec dÃ©gradÃ© violetâ†’bleu, `BorderRadius.circular(10)`)
  - Bouton "**NOUVEAU POST**" prominent (`ElevatedButton`, violet foncÃ©, pleine largeur, icÃ´ne `Icons.add_circle_outline`)
  - Section **Content** avec label gris et items : `Posts`, `Calendar`, `Media Library`, `Templates`
  - Section **Configuration** avec items : `Social Accounts`, `Posting Schedule`, `Webhooks`
  - En bas : `ListTile` avec `CircleAvatar` + nom utilisateur + workspace
  - Item actif : fond `Color(0xFF6366F1).withOpacity(0.1)` + texte violet + barre verticale violette Ã  gauche

- **Zone de contenu principale** (`Expanded`) : affiche l'Ã©cran courant

### Navigation
Utilise `GoRouter` pour le routing avec les routes suivantes :
- `/` â†’ redirige vers `/posts`
- `/posts`
- `/posts/create`
- `/posts/:id/edit`
- `/calendar`
- `/accounts`
- `/analytics`

---

## Palette de couleurs (ThemeData)

DÃ©finis un `AppColors` statique :

```dart
class AppColors {
  static const primary       = Color(0xFF4F46E5);
  static const primaryDark   = Color(0xFF1E1B4B);
  static const background    = Color(0xFFF8F9FA);
  static const surface       = Color(0xFFFFFFFF);
  static const border        = Color(0xFFE5E7EB);
  static const textPrimary   = Color(0xFF111827);
  static const textMuted     = Color(0xFF6B7280);
  static const success       = Color(0xFF10B981);
  static const warning       = Color(0xFFF59E0B);
  static const danger        = Color(0xFFEF4444);
}
```

---

## Ã‰cran 1 â€” Social Accounts (`/accounts`)

### En-tÃªte de page
- `Text` "Social Accounts" (style : `fontSize: 24, fontWeight: bold`)
- `Text` sous-titre gris : "Connect a social account you'd like to manage."

### Grille de comptes
- `GridView` avec `crossAxisCount: 4`, `mainAxisSpacing: 16`, `crossAxisSpacing: 16`

**Carte "Ajouter un compte"** (1Ã¨re cellule) :
- `DashedBorder` (package `dashed_border` ou dessinÃ© avec `CustomPainter`)
- `Icon(Icons.add_circle_outline)` centrÃ© + `Text("Ajouter un compte")`
- Fond blanc, `InkWell` avec hover

**Cartes de comptes existants** :
- `Card` avec `elevation: 1`, `BorderRadius.circular(16)`
- `PopupMenuButton` (icÃ´ne `Icons.more_vert`) en haut Ã  droite via `Stack`
- Avatar circulaire (`CircleAvatar`, rayon 30) avec **badge de plateforme** en bas Ã  droite :
  - Badge = petit cercle colorÃ© (~20px) avec icÃ´ne SVG de la plateforme
  - ImplÃ©mentÃ© en `Stack` + `Positioned`
- `Text` nom du compte (bold, centrÃ©)
- `Text` "Added: X minutes ago" (style muted)

### Plateformes Ã  supporter

| Plateforme | Couleur badge |
|---|---|
| Facebook | `Color(0xFF1877F2)` |
| Instagram | dÃ©gradÃ© `Color(0xFFE1306C)` â†’ `Color(0xFF833AB4)` |
| Threads | `Color(0xFF000000)` |
| X (Twitter) | `Color(0xFF000000)` |
| LinkedIn | `Color(0xFF0A66C2)` |
| YouTube | `Color(0xFFFF0000)` |
| TikTok | `Color(0xFF000000)` |
| Pinterest | `Color(0xFFE60023)` |
| Mastodon | `Color(0xFF6364FF)` |
| Bluesky | `Color(0xFF0085FF)` |

CrÃ©e un widget rÃ©utilisable `PlatformBadge(platform: SocialPlatform)` qui retourne le bon badge colorÃ©.

---

## Ã‰cran 2 â€” Analytics (`/analytics`)

### En-tÃªte
- `Text` "Analytics" (h1)
- `TabBar` avec tabs : `Overview`, `Engagement`, `Video`, `Content`, `Hashtags`, `Insights`
  - Tab actif : soulignÃ© violet, texte violet
  - Tabs inactifs : texte gris
- SÃ©lecteur de pÃ©riode en haut Ã  droite : `OutlinedButton` avec `Icon(Icons.calendar_today)` + texte "This month" + date range + `Icon(Icons.expand_more)`

### MÃ©triques (grille 3 colonnes)
Utilise un `GridView` 3 colonnes ou une `Wrap` de `MetricCard` :

Chaque `MetricCard` contient :
- `Text` label (style muted, petit)
- `Text` valeur (bold, `fontSize: 32`, couleur `AppColors.primary`)
- Badge de variation : `Row` avec icÃ´ne flÃ¨che + texte
  - Positif : `Icons.trending_up`, couleur `AppColors.success`
  - NÃ©gatif : `Icons.trending_down`, couleur `AppColors.danger`

MÃ©triques : `Total followers`, `Media views`, `Post engagements`, `Page views`, `New follows`, `Unfollows`

### Graphiques (2 colonnes cÃ´te Ã  cÃ´te)
Utilise le package **`fl_chart`** :

1. **Follower growth** (gauche) :
   - `BarChart` combinÃ© : barres vertes (nouveaux followers) + barres rouges (unfollows)
   - Superposer une `LineChart` pour la courbe bleue de tendance (utilise un `Stack`)
   - Double axe Y (gauche : valeurs absolues, droit : delta)
   - Axe X : labels de dates

2. **Page views** (droite) :
   - `LineChart` courbe bleue seule
   - Axe Y : 0 â†’ 1400, Axe X : dates du mois

Chaque graphique est dans un `Card` avec titre en haut et lÃ©gende "You can drag the chart to zoom." en bas.

---

## Ã‰cran 3 â€” Calendar vue Mois (`/calendar` avec `view=month`)

### Barre de navigation
`Row` contenant :
- `OutlinedButton` "TODAY"
- `IconButton(Icons.chevron_left)` et `IconButton(Icons.chevron_right)`
- `Text` mois/annÃ©e bold (ex: "January 2026")
- `Spacer()`
- Dropdown de vue : `DropdownButton` avec options `MONTH` / `WEEK`
- `SearchBar` "Search by keyword"
- `ElevatedButton` "FILTERS" (violet, `Icons.filter_alt`)

### Grille calendrier mensuel
- `Table` ou `GridView` 7 colonnes
- En-tÃªtes : `Mon Tue Wed Thu Fri Sat Sun` (texte gris, centrÃ©)
- Cellule jour :
  - NumÃ©ro en haut Ã  gauche (`Text`)
  - Aujourd'hui : numÃ©ro dans `CircleAvatar` violet
  - Liste scrollable de `PostChip` dans la cellule

**Widget `PostChip`** :
- `Container` avec `BoxDecoration(color: Colors.white, borderRadius: 8, boxShadow: ...)`
- Barre colorÃ©e verticale Ã  gauche (`Container` de 4px de large, couleur selon label)
- `Text` tronquÃ© du post (`overflow: TextOverflow.ellipsis`, `maxLines: 2`)
- `Row` d'icÃ´nes de plateformes (~14px chacune)

---

## Ã‰cran 4 â€” Calendar vue Semaine (`/calendar` avec `view=week`)

### MÃªme barre de navigation que la vue mois
- Titre format : "Jan 26 - Feb 1"

### Grille hebdomadaire
- Layout : `Row` avec colonne horaire fixe + `Expanded` pour les 7 jours
- **Colonne heures** (largeur ~60px) : labels "2 pm", "3 pm"... espacÃ©s uniformÃ©ment
- **7 colonnes de jours** :
  - En-tÃªte : nom du jour (gris, petit) + numÃ©ro (bold) dans un `CircleAvatar` si aujourd'hui
  - Corps : `Stack` positionnÃ© selon l'heure (`top: heure * pixelsParHeure`)

**Widget `WeekPostCard`** positionnÃ© dans le Stack :
- Barre colorÃ©e Ã  gauche (4px)
- `Text` tronquÃ© du post
- `Row` d'icÃ´nes plateformes (14px)
- `Row` avec `Icon(Icons.access_time, size: 12)` + heure
- Point de statut colorÃ© (orange = needs approval, cyan = scheduled) en bas Ã  droite

---

## Ã‰cran 5 â€” Posts (`/posts`)

### En-tÃªte
- `Text` "Posts" (h1)
- `Spacer()`
- `SearchBar` "Search by keyword"
- `ElevatedButton` "FILTRES" (violet, `Icons.filter_alt`)

### Tabs de filtre
`TabBar` avec : `All`, `Drafts`, `Needs approval`, `Scheduled`, `Published`, `Trash`
- Tab actif : soulignÃ© violet

### Tableau des posts
Utilise un `DataTable` ou une `ListView` de `PostRow` :

**Colonnes** : Checkbox | Status | Content | Media | Labels | Accounts | Actions

**Widget `PostRow`** :
- **Checkbox** : `Checkbox`
- **Status** : widget `StatusBadge(status)` â€” `Row` avec `Container` rond (8px) colorÃ© + `Text`
  - Draft â†’ gris
  - Needs approval â†’ orange + date/heure dessous
  - Scheduled â†’ cyan + date/heure dessous
  - Published â†’ vert
- **Content** : `Text` tronquÃ© (`maxLines: 3`)
- **Media** : `ClipRRect(borderRadius: 8)` autour d'un `Image.network` ou `Image.asset` (60Ã—60)
- **Labels** : `Wrap` de `LabelChip(label)` â€” `Container` arrondi avec fond colorÃ© + texte
  - Tips & Tricks â†’ jaune
  - Customer Story â†’ violet
  - Announcement â†’ cyan
  - Event â†’ rose
  - Blog Post â†’ bleu
  - Product Launch â†’ rose foncÃ©
- **Accounts** : `SizedBox` avec avatars empilÃ©s (`Stack` + `Positioned` avec offset x) + badge `+N` si >3
- **Actions** : `IconButton(Icons.edit_outlined)` + `PopupMenuButton(Icons.more_vert)`

---

## Ã‰cran 6 â€” Create/Edit Post (`/posts/create`)

### Layout
`Row` en deux colonnes :
- **Colonne gauche** (`flex: 65`) : Ã©diteur
- **Colonne droite** (`flex: 35`) : preview + activitÃ©
- SÃ©parÃ©s par un `VerticalDivider`

---

### Colonne gauche â€” Ã‰diteur

#### En-tÃªte
- `Text` "Your post" (bold)
- `Spacer()`
- `Row` de badges statut :
  - `_StatusDot(color: Colors.grey, label: "Draft")`
  - `_StatusDot(color: AppColors.success, label: "Saved")`

#### SÃ©lecteur de comptes
- `SingleChildScrollView(scrollDirection: Axis.horizontal)`
- `Row` d'`AccountAvatarButton(account, isSelected)` :
  - `CircleAvatar` avec bordure colorÃ©e selon plateforme (2px)
  - `PlatformBadge` en bas Ã  droite via `Stack`
  - Si non sÃ©lectionnÃ© : `opacity: 0.4`
  - `onTap` : toggle sÃ©lection

#### Zone d'onglets de variantes
- `TabBar` avec onglet "Original" actif + bouton `+` (`IconButton`) pour ajouter des variantes

#### Zone de texte
- `TextField` multiligne, sans dÃ©coration de bordure visible (`InputDecoration(border: InputBorder.none)`)
- `TextStyle` qui colore les hashtags en violet (via `RichText` ou package `flutter_hashtag`)
- Miniature de media attachÃ© : `Stack` avec `Image` + bouton `Ã—` (`IconButton(Icons.close)`)

#### Barre d'outils
`Row` en bas de l'Ã©diteur :
- `IconButton(Icons.emoji_emotions_outlined)` Emoji
- `IconButton(Icons.image_outlined)` Media
- `IconButton(Icons.tag)` Hashtag
- `IconButton(Icons.data_object)` Variable
- `IconButton(Icons.grid_view)` Mise en page
- `Spacer()`
- `Text` compteur de caractÃ¨res + `IconButton(Icons.add_circle_outline)`

---

### Colonne droite â€” Preview & Activity

#### Tabs
`TabBar` :
- **Preview** (icÃ´ne `Icons.visibility_outlined`, violet soulignÃ© si actif)
- **Activity** (icÃ´ne `Icons.chat_bubble_outline`)
- `IconButton(Icons.notifications_none)` en haut Ã  droite hors du TabBar

#### AperÃ§u du post par plateforme
`ListView` de `PlatformPreviewCard(platform, account, content, media)` :
- `Align(alignment: Alignment.topRight)` : icÃ´ne de la plateforme
- `Row` : `CircleAvatar` + `Column(nom, handle)`
- `Text` du post avec hashtags colorÃ©s
- `Image` du media
- `Row` de stats fictives : ðŸ’¬ `0` | ðŸ” `27` | â¤ï¸ `312` | `Icons.ios_share`

---

### Footer sticky (bas de l'Ã©cran, `BottomAppBar` ou `Container` fixe)
`Row` :
- `LabelChip("Tips & Tricks")` (jaune)
- `OutlinedButton.icon(Icons.label_outline, "LABELS")`
- `OutlinedButton.icon(Icons.calendar_today, "PICK TIME")`
- `ElevatedButton.icon(Icons.bolt, "POST NOW")` (violet foncÃ©)
- `ElevatedButton.icon(Icons.queue, "ADD TO QUEUE")` (orange, `Color(0xFFF97316)`)

---

## Widgets rÃ©utilisables Ã  crÃ©er

| Widget | Description |
|---|---|
| `AppSidebar` | Sidebar de navigation globale |
| `PlatformBadge({required SocialPlatform platform})` | Petit badge colorÃ© avec icÃ´ne plateforme |
| `StatusBadge({required PostStatus status, DateTime? scheduledAt})` | Point colorÃ© + label + date |
| `LabelChip({required String label})` | Badge colorÃ© selon le label |
| `AccountAvatar({required Account account, double size = 36, bool isSelected = true})` | Avatar avec badge plateforme |
| `PostCalendarChip({required Post post})` | Carte compacte pour le calendrier mois |
| `WeekPostCard({required Post post})` | Carte positionnÃ©e pour le calendrier semaine |
| `MetricCard({required String label, required String value, double? change})` | Carte de mÃ©trique analytics |

---

## Packages Flutter recommandÃ©s

```yaml
dependencies:
  flutter:
    sdk: flutter
  go_router: ^14.0.0          # Navigation
  google_fonts: ^6.0.0        # Typographie (DM Sans / Outfit)
  fl_chart: ^0.68.0           # Graphiques analytics
  provider: ^6.0.0            # State management (ou riverpod)
  intl: ^0.19.0               # Formatage des dates
  cached_network_image: ^3.3.0 # Images rÃ©seau avec cache
```

---

## Instructions d'implÃ©mentation

ImplÃ©mente les Ã©crans dans cet ordre :

1. `AppColors` + `ThemeData` + polices
2. `AppSidebar` + layout `ScaffoldWithSidebar`
3. Widgets rÃ©utilisables (`PlatformBadge`, `StatusBadge`, `LabelChip`, `AccountAvatar`)
4. Ã‰cran `SocialAccountsScreen`
5. Ã‰cran `PostsScreen`
6. Ã‰cran `CreatePostScreen`
7. Ã‰cran `CalendarScreen` (vue mois, puis vue semaine)
8. Ã‰cran `AnalyticsScreen`

---

## Comportements interactifs attendus

- **Sidebar** : item actif avec fond violet clair + texte violet + barre verticale gauche violette
- **Create Post** : tap sur un avatar de compte â†’ toggle `isSelected` (opacitÃ© 0.4 si inactif)
- **Calendar** : tap sur un `PostCalendarChip` â†’ navigation vers `/posts/:id/edit`
- **Posts** : tap sur un tab â†’ filtre la liste localement (`provider` ou `setState`)
- **Analytics** : les tabs autres que `Overview` affichent un `Center(child: Text("Coming soon"))`
- **Bouton "CREATE POST"** dans la sidebar â†’ `context.go('/posts/create')`
- **Dropdown vue calendrier** : switcher entre `Month` et `Week` met Ã  jour l'affichage dans le mÃªme Ã©cran
