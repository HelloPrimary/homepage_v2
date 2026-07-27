# Homepage Primary — Contexte projet

*Maquette de projection de la homepage patient Primary (médecine générale premium). Fichier unique `index.html`, pas de build, pas de framework. Sert de base à toute reprise de conversation sur ce projet.*
*Dernière mise à jour : 27 juillet 2026.*

---

## 1. Nature du projet

- **Fichier de travail unique** : `index.html` (HTML + CSS + JS inline, un seul fichier).
- **Statut** : maquette de projection pour validation CEO (bandeau wireframe visible en haut de page : *« Maquette de projection · Homepage Patient · Juillet 2026 · Pour validation CEO »*).
- **Pas de framework, pas de build step.** Ouvrir directement dans un navigateur ou servir avec `python3 -m http.server 8765` depuis ce dossier.
- **Repo canonique** : ce dossier `primary-homepage/` est bien la maquette statique en cours de travail. Un autre repo (`PRIMARY DAY LP`, app Next.js) existe pour un projet différent (l'événement Primary Care Day) — ne pas confondre les deux. **Attention au terminal** : si une commande `cd` ou `pwd` répond depuis `PRIMARY DAY LP`, c'est le mauvais dossier — toujours vérifier/forcer `cd /Users/clemencechantry/Desktop/primary-homepage` avant toute commande git.
- **Déployé sur GitHub Pages** : le dossier est maintenant un vrai dépôt git, connecté à `https://github.com/HelloPrimary/homepage_v2.git` (remote `origin`, branche `main`, repo **Public**). Site en ligne : **https://helloprimary.github.io/homepage_v2/**.
- **Workflow de mise à jour** : après chaque modification locale, `git add`/`git commit` peuvent être faits par Claude, mais **le `git push` doit être fait par l'utilisatrice elle-même dans son propre Terminal** (authentification par token GitHub — Claude ne doit jamais manipuler ni voir de token/mot de passe). Voir §7 pour le détail du flow d'auth.

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

2. **`.hero`** — Immersif, 92vh/min-height 620px, photo pleine largeur (actuellement `hero-focus-medecin.png`, cadrage `background-position: 58% 22%` — plusieurs variantes testées et laissées dans le dossier pour comparaison rapide : `hero_medecin_2.png`, `hero_medecin_2 1.png`, `hero_medecin_3.png`), léger voile (pas de gros dégradé sombre), texte bas-gauche centré verticalement dans le hero. Titre manifeste *« La médecine, proactive. »* (interligne resserré 0.96) + sous-titre 2 lignes *« Un médecin qui vous accompagne toute l'année. Et veille déjà sur la suite. »* (~32-34px, sans `<br>` forcé — retour naturel). CTA unique.

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
     - Étape 3 (Décider) : retour à l'humain (photo `etape3-echange.jpg` — **renommée**, voir §4), **une seule** carte "Mon plan d'action" (structure jumelle du bilan étape 2, sans redondance des catégories déjà vues).
     - Étape 4 (Accompagner) : **le téléphone devient le héros** — d'abord un mockup produit dessiné en CSS, puis remplacé par la vraie photo `photo-etape4.png` (**renommée**, voir §4) (téléphone posé sur un bureau, lifestyle) + 2 cartes flottantes (objectif atteint / rappels+RDV).
     - Étape 5 (Répondre) : scène de vie (`etape5famille.jpg`, mère + enfants) — **pas de grande carte UI**, bulles de conversation façon iMessage posées directement sur la photo, rapprochées, sans label « AMIA », signature *« Échange revu par le Dr Pavesi »* intégrée au dernier message.
   - Toutes les scènes utilisent le même gabarit de stage (fond teinté ou photo, mêmes coins arrondis, même ombre) pour une cohérence visuelle sur les 5 étapes.

5. **`.bloc-medecins`** — Ex-section témoignages patients, **entièrement remplacée** par un **mur éditorial de reels médecins** (plusieurs itérations successives, cf. §5 de l'historique). Répond à la question implicite du visiteur à ce stade : *« Qui sont les médecins qui vont m'accompagner ? »* — objectif final reformulé par le client : ne pas présenter l'équipe, mais faire ressentir *une manière particulière d'exercer la médecine* (« j'aimerais avoir un médecin qui pense comme ça », pas « ils ont une belle équipe »).
   - Titre actuel : *« Une autre idée de la médecine générale »* (`.medecins-title`, **`white-space:nowrap` remis** pour forcer une seule ligne — `.medecins-head` en `max-width:none` pour laisser la place nécessaire). Titres alternatifs envisagés, plus émotionnels : *« Ils exercent la médecine autrement »* / *« La médecine, telle qu'on aimerait qu'elle soit »*.
   - **Assets réels utilisés (mis à jour)** : `scene1.jpg` → `scene6.jpg` — **découpées par Claude** (script PIL/Pillow) à partir de la seule image de référence fournie par le client, `inspi témoignages.png` (montage 6 vignettes déjà avec sous-titre + timecode + icône son incrustés). Les anciennes `reel#1.png` → `reel#4.png` (4 photos, plans trop similaires entre eux) ont été **abandonnées** au profit de ces 6 scènes qui montrent enfin la variété de plans voulue (champ/contrechamp, marche dans le couloir du cabinet, gros plan, moment spontané/réfléchi).
   - **Layout** : `.reels-rail` = rail horizontal scrollable (`scroll-snap-type:x`, drag-to-scroll en JS, `overflow-x:auto`) contenant désormais **6** `.reel-card` (ratio de base 9:16). **Rythme volontairement irrégulier** via `nth-child` (1 à 6) : tailles différentes (`clamp` de largeur variable par carte) + ratios différents (certaines cartes en 3:4, 4:5 ou 1:1 au lieu de 9:16) + alignement vertical alterné (`align-self:flex-start` avec `margin-bottom` vs `align-items:flex-end` du conteneur) → crée un « skyline » façon mur de contenu premium (Apple/Airbnb Experiences), pas une grille/galerie uniforme. La carte 3 (`scene3.jpg`, champ-contrechamp) a un `object-position` ajusté (`68% 38%`) pour garder le médecin dans le cadre malgré le crop 9:16.
   - **Quand les vraies vidéos arriveront** : remplacer chaque `<img class="reel-video" src="...">` par `<video class="reel-video" muted loop autoplay playsinline src="...">` — le CSS `.reel-video` (`object-fit:cover`) s'applique déjà correctement à une vidéo (commentaire laissé dans le HTML à cet effet).

6. **`.bloc-cta`** — CTA final, fond bordeaux, titre avec mot rotatif (*« bonnes » ↔ « vraies »*).

**Sections supprimées pendant le travail** : ancien bloc « Phone scroll » (« L'expérience patient / La continuité de soin que vous attendiez ») et son JS de zoom sur téléphone — retiré à la demande du client, jugé redondant avec la timeline scrollytelling.

---

## 4. Arbitrages techniques notables

- **⚠️ Éviter les noms de fichiers avec accents/espaces — cause de bug réel en prod.** `étape3.jpg` et `photo étape 4.png` cassaient (404) une fois déployés sur GitHub Pages alors qu'ils fonctionnaient en local (probable mismatch d'encodage Unicode NFD/NFC entre macOS et un serveur Linux case-sensitive). **Renommés en `etape3-echange.jpg` et `photo-etape4.png`** (ASCII pur, sans accent ni espace) — fix appliqué et vérifié. **Règle à suivre pour tout nouvel asset : toujours nommer en `minuscules-avec-tirets.ext`, jamais d'accent ni d'espace.** Des fichiers legacy avec accents/espaces subsistent encore dans le dossier (`photo médecin.png`, `inspi témoignages.png`, `page dépistage.png`, etc.) mais ne sont plus référencés dans `index.html` — inoffensifs tant qu'ils ne sont pas utilisés en `src`.
- **`etape3OLD.jpg`** existe encore (ancienne version) mais n'est plus référencée.
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
10. Portraits médecins : remplacement des 4 `reel#X.png` (plans trop similaires) par 6 `sceneX.jpg` découpées depuis l'image de référence unique du client — variété de plans enfin obtenue.
11. Mise en ligne du projet : création du repo GitHub `HelloPrimary/homepage_v2`, activation de GitHub Pages, diagnostic et fix d'un bug de police cassée (fonts/ manquant → texte en tofu), renommage des fichiers à noms accentués/espacés cassant sur GitHub Pages, remplacement de la photo hero (`hero-focus-medecin.png`), établissement du workflow commit-par-Claude / push-par-l'utilisatrice.

---

## 6. Déploiement & partage (GitHub Pages)

- **Repo** : `github.com/HelloPrimary/homepage_v2` (organisation **HelloPrimary**, avec SSO activé). Repo **Public** (nécessaire pour que le lien GitHub Pages soit ouvrable par n'importe qui, y compris hors de l'org — un repo Internal/Private ne serait visible qu'aux membres connectés à GitHub).
- **URL publique** : `https://helloprimary.github.io/homepage_v2/`
- **Config locale** : dossier connecté en `git remote origin` sur l'URL ci-dessus, branche `main`. `.gitignore` créé pour exclure `.DS_Store`, `.claude/` (config de session Claude Code) et `originaux/` (dossier de fichiers sources bruts, pas utile au site).
- **⚠️ Bug police cassée (tofu/glyphes carrés) — résolu.** Le dossier `fonts/` (contenant `Brockmann-Regular.otf` et `Brockmann-SemiBold.otf`) n'avait **jamais été uploadé** lors du premier envoi manuel via l'interface web GitHub (drag & drop des fichiers un par un, sans le sous-dossier `fonts/`). Résultat : `@font-face` chargeait un fichier absent, et pour une raison spécifique au comportement de `font-display:swap` / fallback, le texte s'affichait entièrement en glyphes de substitution (carrés avec point d'interrogation) plutôt que de retomber proprement sur la police système. **Fix en 2 temps** : (1) désactivation temporaire de `'Brockmann'` dans la variable `--f` (fallback système immédiat, texte lisible pendant le diagnostic) le temps de committer/pousser `fonts/` ; (2) une fois `fonts/` confirmé présent sur GitHub, `'Brockmann'` remise dans `--f`. **Si ce bug réapparaît** : vérifier en premier que `fonts/*.otf` est bien présent dans le repo distant.
- **Workflow d'édition/publication établi** :
  1. Claude modifie `index.html` (et les assets) localement, teste en preview.
  2. Claude fait `git add` + `git commit` localement (jamais de `git push` par Claude).
  3. L'utilisatrice pousse elle-même (`git push origin main`) depuis son propre Terminal, avec son propre token GitHub — **Claude ne doit jamais voir, manipuler ou générer un token/mot de passe**, même si l'utilisatrice le propose spontanément (si un token est collé dans le chat par erreur, le signaler et recommander de le révoquer immédiatement sur github.com/settings/tokens).
  4. Après un push, on peut vérifier la synchro sans toucher aux credentials : `git fetch origin` (lecture seule, pas d'auth nécessaire sur un repo public) puis comparer `git log --oneline -3` en local vs `git log origin/main --oneline -3`.
- **Repères utiles pour l'auth git (déjà rencontrés)** :
  - Un push échoue en **403 immédiat sans même demander username/password** → signe qu'une **ancienne credential est en cache dans le Trousseau macOS** et est réutilisée automatiquement. Fix : `printf "protocol=https\nhost=github.com\n" | git credential-osxkeychain erase` (vide juste le cache, force git à redemander).
  - Token **fine-grained** (`github_pat_...`) : doit être créé avec *Resource owner* = **HelloPrimary**, *Repository access* → *Only select repositories* → `homepage_v2`, permission **Contents: Read and write**. Sans ça → 403 même avec un token par ailleurs valide.
  - Org avec **SSO activé** : penser à cliquer *« Enable SSO »* à côté du token sur `github.com/settings/tokens` et autoriser pour HelloPrimary, sinon le token est bloqué même bien scopé.
  - Vérifier aussi que le compte a bien un rôle **Write** (pas juste lecture, que le repo soit Public ne suffit pas) sur `homepage_v2` via *Settings → Collaborators and teams*.
- **Comment partager rapidement sans passer par tout ce setup** (alternative retenue mais pas utilisée finalement) : Netlify Drop (`app.netlify.com/drop`, glisser le dossier, lien instantané) — utile si on veut un lien jetable sans toucher au repo GitHub.

---

## 7. Pour reprendre une conversation sur ce projet

Dans le premier message d'une nouvelle conversation, il suffit de dire :

> « Je travaille sur `/Users/clemencechantry/Desktop/primary-homepage/index.html` (voir `CONTEXTE.md` dans le même dossier pour l'historique). Je veux [ta demande]. »

Claude peut relire directement `index.html` pour voir l'état exact du code, et ce fichier pour comprendre le *pourquoi* des choix déjà faits (éviter de proposer à nouveau des directions déjà essayées et rejetées — stepper classique, cartes de témoignages, gros hero SaaS, etc.).
