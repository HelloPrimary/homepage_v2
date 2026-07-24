# Homepage Primary — Contexte projet

*Maquette de projection de la homepage patient Primary (médecine générale premium). Fichier unique `index.html`, pas de build, pas de framework. Sert de base à toute reprise de conversation sur ce projet.*
*Dernière mise à jour : 23 juillet 2026.*

---

## 1. Nature du projet

- **Fichier de travail unique** : `index.html` (HTML + CSS + JS inline, un seul fichier).
- **Statut** : maquette de projection pour validation CEO (bandeau wireframe visible en haut de page : *« Maquette de projection · Homepage Patient · Juillet 2026 · Pour validation CEO »*).
- **Pas de framework, pas de build step.** Ouvrir directement dans un navigateur ou servir avec `python3 -m http.server 8765` depuis ce dossier.
- **Repo canonique** : ce dossier `primary-homepage/` est bien la maquette statique en cours de travail. Un autre repo (`PRIMARY DAY LP`, app Next.js) existe pour un projet différent (l'événement Primary Care Day) — ne pas confondre les deux.

---

## 2. Direction artistique globale

**Ce qu'on fuit activement** (retour constant du client) : tout ce qui ressemble à une **landing page SaaS/startup** — navbar blanche flottante, hero classique H1/paragraphe/CTA, cartes de bénéfices identiques en grille, stepper avec ronds + ligne de progression, captures d'écran d'app présentées comme des mockups produit.

**Ce qu'on vise** : une **marque de médecine premium et éditoriale**, avec des références explicites récurrentes du client : **Apple, Oura, Stripe (Press), Linear, Kinfolk, Monocle, Lucis, Headspace, MasterClass, Airbnb Experiences**.

Principes transverses qui reviennent à chaque itération :
- Beaucoup d'espace blanc / silence visuel plutôt que de la densité d'info.
- Une idée par écran, pas une accumulation de fonctionnalités.
- Les visuels produit ne doivent **jamais ressembler à une capture d'écran d'app** : on montre des *indices* (un badge, une jauge, une notification, un chiffre) jamais une interface complète.
- Typographie : **Brockmann** (Regular/SemiBold), chargée en `@font-face` locale (`fonts/Brockmann-*.otf`).
- Couleurs de marque : `--bordeaux:#3a1c33`, `--gold:#EFC712`, `--ink:#101010`, `--muted:#5e6266`.
- Quand on hésite entre ajouter ou retirer un élément → **retirer**.

---

## 3. Structure actuelle de la page (ordre des sections)

1. **`nav`** — Navigation fixe, transparente sur le hero, devient glassmorphism (fond translucide + flou) une fois qu'on scrolle *au-delà* de la hauteur du hero (seuil calculé en JS sur `hero.offsetHeight`, pas un scroll fixe). Logo/liens blancs sur le hero, foncés une fois scrollée. CTA unique dominant **« Commencer mon suivi »** (gold) ; « Se connecter » est un lien texte secondaire discret.

2. **`.hero`** — Immersif, 92vh/min-height 620px, photo pleine largeur (`5232771 (2).jpg`), léger voile (pas de gros dégradé sombre), texte bas-gauche centré verticalement dans le hero. Titre manifeste *« La médecine, proactive. »* (interligne resserré 0.96) + sous-titre 2 lignes *« Un médecin qui vous accompagne toute l'année. Et veille déjà sur la suite. »* (~32-34px, sans `<br>` forcé — retour naturel). CTA unique.

3. **`.bloc-vision`** — Fusion **manifeste + bénéfices concrets** en une seule section éditoriale (ex-deux sections séparées, fusionnées sur demande explicite pour éviter l'effet « composants juxtaposés »).
   - Gauche : eyebrow *« Notre vision »* + manifeste (*« La médecine proactive n'est pas une nouvelle médecine... »*), colonne sticky sur desktop.
   - Droite : photo lifestyle (`image manifeste.png`, femme sur canapé/tablette) avec **2 widgets flottants façon Apple Health/widget météo** (jamais une interface complète) :
     - Carte **Score** : picto horloge + « SCORE » + « 71/100 » + jauge fine bordeaux + « Bonne santé globale ».
     - Carte **Dépistage recommandé** : même format d'en-tête (picto calendrier réduit + label ambre/doré) + « Col de l'utérus · sept. 2026 » sur une seule ligne (`white-space:nowrap`).
   - Sous la photo : liste de **4 engagements** (pas des fonctionnalités) avec coche fine + titre + une ligne, séparateurs très discrets (1px).
   - Fond blanc cassé `#FAF8F5`.

4. **`.bloc-programme`** (« On prend soin de [mot rotatif] ») — **Timeline scrollytelling**, entièrement repensée plusieurs fois :
   - **Navigation** : plus un stepper (ronds + ligne + progression) mais une **navigation éditoriale façon table des matières** — `.stepper-rail` en `justify-content:space-between` sur toute la largeur (1080px, alignée avec le contenu), chiffre = repère discret (petit, muted), mot = info principale, état actif = soulignement fin + graisse, **aucun rond, aucune ligne de connexion**.
   - **Important (bug corrigé)** : le rail est **sorti** du bloc `.stepper-sticky` (qui centre verticalement les panneaux/photos pendant le scroll pin) pour garder un espacement fixe et prévisible sous la phrase, indépendant de la hauteur de viewport.
   - **5 étapes** : Préparer · Comprendre · Décider · Accompagner · Répondre — chacune a un sous-titre reformulé (« Votre médecin vous connaît vraiment », etc., sans point final).
   - **Pilotage par le scroll** (pas par clic) : le scroll dans la section fait progresser l'étape active automatiquement (ligne de progression continue sur le rail, réagit dès le 1er pixel). Le clic sur un chapitre reste un raccourci (scrollTo fluide vers la portion correspondante), pas le mode de nav par défaut.
   - **Composition par étape** — pas 5 écrans identiques, du rythme volontaire :
     - Étape 1 (Préparer) : photo + **une seule** carte discrète, fiche patient façon Apple Health (photo de profil ronde + 3 lignes pictos outline : antécédents/traitements/comptes-rendus).
     - Étape 2 (Comprendre/bilan) : **le bilan devient le héros** — composition produit sur fond à halo violet très diffus (jamais un rectangle net), score en grand, interprétation, évolution discrète, 5 domaines en jauges fines sans chiffres, priorités en puces.
     - Étape 3 (Décider) : retour à l'humain (photo `étape3.jpg`), **une seule** carte "Mon plan d'action" (structure jumelle du bilan étape 2, sans redondance des catégories déjà vues).
     - Étape 4 (Accompagner) : **le téléphone devient le héros** — d'abord un mockup produit dessiné en CSS, puis remplacé par la vraie photo `photo étape 4.png` (téléphone posé sur un bureau, lifestyle) + 2 cartes flottantes (objectif atteint / rappels+RDV).
     - Étape 5 (Répondre) : scène de vie (`etape5famille.jpg`, mère + enfants) — **pas de grande carte UI**, bulles de conversation façon iMessage posées directement sur la photo, rapprochées, sans label « AMIA », signature *« Échange revu par le Dr Pavesi »* intégrée au dernier message.
   - Toutes les scènes utilisent le même gabarit de stage (fond teinté ou photo, mêmes coins arrondis, même ombre) pour une cohérence visuelle sur les 5 étapes.

5. **`.bloc-medecins`** — Ex-section témoignages patients, **entièrement remplacée** par un **mur éditorial de reels médecins** (plusieurs itérations successives, cf. §5 de l'historique). Répond à la question implicite du visiteur à ce stade : *« Qui sont les médecins qui vont m'accompagner ? »* — objectif final reformulé par le client : ne pas présenter l'équipe, mais faire ressentir *une manière particulière d'exercer la médecine* (« j'aimerais avoir un médecin qui pense comme ça », pas « ils ont une belle équipe »).
   - Titre actuel : *« Une autre idée de la médecine générale »* (`.medecins-title`, pas de `white-space:nowrap` — le titre doit pouvoir passer sur 2 lignes). Titres alternatifs envisagés, plus émotionnels : *« Ils exercent la médecine autrement »* / *« La médecine, telle qu'on aimerait qu'elle soit »* — à trancher, plusieurs pistes possibles.
   - **Assets réels utilisés** : `reel#1.png` → `reel#4.png` (4 vraies photos façon capture de reel, avec sous-titre + timecode + icône son déjà incrustés dans l'image elle-même — pas un overlay CSS séparé, le rendu vient directement des PNG). Fournies par le client avec une image de référence `inspi témoignages.png` (montage 6 vignettes) qui sert de direction artistique cible (plans variés : champ/contrechamp, marche en couloir, gros plan, spontané).
   - **Layout** : `.reels-rail` = rail horizontal scrollable (`scroll-snap-type:x`, drag-to-scroll en JS, `overflow-x:auto`) contenant 4 `.reel-card` (ratio de base 9:16). **Rythme volontairement irrégulier** via `nth-child` : tailles différentes (`clamp` de largeur variable par carte) + ratios différents (certaines cartes en 3:4 ou 4:5 au lieu de 9:16) + alignement vertical alterné (`align-self:flex-start` avec `margin-bottom` vs `align-items:flex-end` du conteneur) → crée un « skyline » façon mur de contenu premium (Apple/Airbnb Experiences), pas une grille/galerie uniforme.
   - **Limite assumée à date** : les 4 photos disponibles sont toutes des plans « visage face caméra, assis ou debout, cabinet » — la variété de plans souhaitée par le client (marche dans le couloir, champ-contrechamp, gros plan candid) demandera des photos/vidéos sources supplémentaires correspondant à ces types de plan ; la structure HTML (ajouter un `.reel-card` de plus dans `#reelsTrack`) est prête à en accueillir sans limite de nombre.
   - **Quand les vraies vidéos arriveront** : remplacer chaque `<img class="reel-video" src="...">` par `<video class="reel-video" muted loop autoplay playsinline src="...">` — le CSS `.reel-video` (`object-fit:cover`) s'applique déjà correctement à une vidéo (commentaire laissé dans le HTML à cet effet).

6. **`.bloc-cta`** — CTA final, fond bordeaux, titre avec mot rotatif (*« bonnes » ↔ « vraies »*).

**Sections supprimées pendant le travail** : ancien bloc « Phone scroll » (« L'expérience patient / La continuité de soin que vous attendiez ») et son JS de zoom sur téléphone — retiré à la demande du client, jugé redondant avec la timeline scrollytelling.

---

## 4. Arbitrages techniques notables

- **Nommage fichiers avec accents/espaces** : plusieurs assets ont des noms avec accents ou espaces (`étape3.jpg`, `photo médecin.png`, `photo étape 4.png`) — à respecter tel quel dans les `src`.
- **`etape3OLD.jpg`** existe encore (ancienne version) mais n'est plus référencée — `étape3.jpg` (avec accent) est la version actuelle utilisée à l'étape 3 du parcours.
- **CSS en cascade avec duplications** : le fichier contient plusieurs déclarations dupliquées pour une même classe (ex. `.bloc-programme` padding défini 3 fois à des endroits différents) — c'est un résidu d'itérations successives. La règle qui s'applique réellement est **la dernière dans le fichier** (spécificité égale = ordre de cascade). Toujours vérifier avec `grep -n` avant de modifier un padding/margin, et penser à mettre à jour **toutes** les occurrences dupliquées pour éviter la confusion, même si une seule "gagne".
- **Outil de preview** : la capture d'écran (`preview_screenshot`) ne fonctionne de façon fiable qu'à la **taille native** du panneau (`preset: desktop`, ~691-980px de large selon contexte) — les largeurs custom (1280px, 1440px...) cassent le rendu de la capture (fond blanc) bien que le DOM/CSS soit correct. Toujours vérifier via `getComputedStyle`/`getBoundingClientRect` en JS en complément ou à la place du screenshot quand on teste une largeur non-native.
- **Auto-cycle du stepper** : historiquement un `setInterval` faisait défiler les étapes automatiquement toutes les 5s — supprimé quand la navigation est passée en pilotage par le scroll (l'histoire n'avance que quand le visiteur scrolle).
- **Fond « halo diffus »** : pour les étapes produit de la timeline (bilan, téléphone), le fond n'est plus un rectangle violet plat mais un halo radial très désaturé et flouté (`radial-gradient` + `blur(55px)`, opacité ~20%) pour donner une ambiance sans jamais lire « il y a un fond violet ».

---

## 5. Historique des grandes itérations (dans l'ordre)

1. Bloc manifeste : passage d'un format carte/overlay sombre à un split texte-gauche/photo-pleine-largeur.
2. Timeline : passage d'un stepper classique (ronds + ligne) → cartes flottantes sur photo → refonte complète en scrollytelling avec navigation éditoriale (table des matières).
3. Hero : refonte complète d'un hero SaaS classique vers un hero immersif éditorial (nav intégrée, texte bas-gauche puis centré verticalement, H1/sous-titre resserrés).
4. Fusion bénéfices + manifeste en une seule section (`.bloc-vision`).
5. Ajout puis raffinement des 2 widgets flottants sur la photo du bloc vision (score + dépistage), uniformisés au même format d'en-tête.
6. Refonte de la navigation timeline en 2 temps : d'abord stepper → chapitres numérotés avec soulignement, puis correction du bug de centrage vertical (rail sorti du bloc sticky).
7. Section témoignages patients → portraits médecins statiques (photo + nom + rôle) → portraits vidéo façon Reels/TikTok (sous-titres intégrés, autoplay simulé, layout asymétrique).
8. Suppression du bloc "Phone scroll" jugé redondant.
9. Multiples ajustements fins de texte (sous-titre hero réécrit ~5 fois), d'espacement (phrase rotative recentrée entre vision et timeline, puis aérée davantage) et de recadrage photo (visages coupés → corrigés via `transform-origin`+scale).

---

## 6. Pour reprendre une conversation sur ce projet

Dans le premier message d'une nouvelle conversation, il suffit de dire :

> « Je travaille sur `/Users/clemencechantry/Desktop/primary-homepage/index.html` (voir `CONTEXTE.md` dans le même dossier pour l'historique). Je veux [ta demande]. »

Claude peut relire directement `index.html` pour voir l'état exact du code, et ce fichier pour comprendre le *pourquoi* des choix déjà faits (éviter de proposer à nouveau des directions déjà essayées et rejetées — stepper classique, cartes de témoignages, gros hero SaaS, etc.).
