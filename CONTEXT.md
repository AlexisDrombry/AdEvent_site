# AD Evenements — contexte technique du site

Refonte de la page d'accueil de www.adevenements.fr. Agence DJ et événementielle basée à Arras, active à Lille et dans les Hauts-de-France.
Dernière mise à jour : 18 novembre 2026.

## 1. Architecture

- **Site statique sur une seule page.** Tout est dans `index.html` (HTML, CSS et JS en ligne, environ 670 lignes). Il n'y a ni build, ni framework, ni `package.json`.
- **Animations :** GSAP 3.12.5 et ScrollTrigger 3.12.5, hébergés dans `js/` (pas de CDN).
- **Défilement :** `scroll-behavior:smooth` en CSS natif. Lenis a été retiré.
- **Polices :**
  - Cormorant Garamond 300 (Google Fonts), qui remplace TT Ramillas, police payante.
  - Satoshi 500/700 (Fontshare).
  - Les deux sont chargées depuis un CDN externe.
- **Formulaire de devis :** envoi par e-mail via FormSubmit (`https://formsubmit.co/ajax/contact@adevenements.fr`), sans serveur à nous.
- **Aperçu local :** `/usr/bin/python3 -m http.server 8765` à la racine, puis http://localhost:8765. Déclaré dans `.claude/launch.json` (« AD Evenements Site »). Utiliser `/usr/bin/python3` : le `python3` du PATH est un vieux Python 3.6 que macOS tue au démarrage (code 137). Les chemins d'images doivent rester relatifs à la racine. Sans serveur, GSAP et les images ne se chargent pas dans un `file://`.

### Arborescence

```
adevenements-site/
├── index.html          page d'accueil complète
├── CONTEXT.md          ce fichier
├── js/
│   ├── gsap.min.js
│   └── ScrollTrigger.min.js
├── images/             10 JPEG, environ 4 Mo au total
```

### Images (`images/`)

| Fichier | Taille | Usage | Source |
|---|---|---|---|
| hero-soiree-guirlandes.jpg | 2400×1600, 696 Ko | Hero plein écran | Unsplash (photo-1623788452350-4c8596ff40bb) |
| agence-dj-mariage-arras.jpg | 1600×1066, 452 Ko | L'agence, grande image | Photos du client |
| agence-piste-de-danse.jpg | 2000×1333, 384 Ko | L'agence, petite image | Unsplash (photo-1758810409996-583f3bad7d51) |
| dj-mariage-hauts-de-france.jpg | 1400×1000 | Carte Mariage | Photos du client |
| dj-soiree-entreprise-carlton.jpg | 1400×1050 | Carte Entreprise | Photos du client |
| dj-soiree-privee.jpg | 1400×935 | Carte Particulier | Photos du client |
| location-photobooth-arras.jpg | 1400×1050 | Carte Photobooth | Photos du client |
| options-artifices.jpg | 1400×933 | Carte Options | Photos du client |
| ambiance-eclairage-soiree.jpg | 2400×1600 | Section Ambiance | Photos du client |
| contact-salle-reception.jpg | 1800×1200 | Fond du contact (opacité 0,2) | Photos du client |

- Les originaux du client sont dans `~/Desktop/AD EVENEMENTS/WEB/MEDIAS` et n'ont pas été modifiés.
- Les exports ont été faits avec `sips`, en JPEG qualité 78.
- Variantes mobiles servies via `srcset` : `*-1200.jpg` (hero, ambiance, contact) et `*-900.jpg` (autres), JPEG qualité 72. À régénérer si une image change.

## 2. Structure de la page (dans l'ordre)

1. **Nav fixe.**
   - Logo AD en SVG en ligne (`fill:currentColor`, couleur unie, sans carré ni texte).
   - Liens Prestations / Photobooth / Options / L'agence / Contact, téléphone, bouton « Devis gratuit » vers `#devis`.
   - Le bouton de la nav est en contour sur le hero, puis plein onyx dès `.is-solid` (règles `.nav.is-solid .btn`). Le CTA du hero est, lui, plein parchment (`btn--fill`).
   - Burger et menu plein écran sous 960 px.
   - Trois états :
     - transparente sur le hero ;
     - `.is-solid` (parchment) après le hero ;
     - `.is-dark` (roast) sur la section contact.
2. **Hero.**
   - Contenu : H1 « DJ et prestations événementielles à Arras, Lille et dans les Hauts-de-France », sous-titre, CTA plein.
   - Wordmark décoratif « Evenements » (`aria-hidden`, sans accent).
   - Intro : ouverture en clip-path, dézoom de l'image, lettres qui montent. Elle démarre quand les polices et l'image sont prêtes, ou au plus tard après 1,5 s.
3. **#agence.** Manifeste dont les mots s'allument au défilement (scrub), puis 2 images en parallaxe et un texte.
4. **#prestations.** Défilement horizontal épinglé sur desktop : 5 cartes Mariage, Entreprise, Particulier, Photobooth (`#photobooth`), Options. Sur mobile, colonne simple.
5. **Ambiance.** Section pleine largeur de 100 svh, sans défilement épinglé. Le cadre s'ouvre une seule fois (`clip-path` de 14 % à 0 %, `once: true`) quand la section atteint 85 % de l'écran. Le texte « Le son, la lumière, l'émotion. » est en bas du cadre, parchment sur fond sombre. Sur mobile, le titre fait `clamp(40px,8vw,115px)`.
6. **Marquee #options.** Bandeau sous l'Ambiance, en défilement CSS infini. Version réduite : padding 14 px, taille 18–26 px, petit losange en contour.
7. **Pourquoi nous choisir.** 5 engagements, fond parchment.
8. **Ils nous ont fait confiance.** Grille de 12 références, fond linen pour la distinguer de la section précédente.
9. **#contact.** Fond roast.
   - À gauche : coordonnées.
   - À droite : formulaire `#devis` avec nom, téléphone, e-mail, type, date, lieu, chips de prestations et message.
   - Le pied de page est intégré à cette section.

## 3. Style guide appliqué (Amrit Palace)

- **Couleurs :** parchment `#d8cbb8`, linen `#bfb4a3`, stone `#b6ab9c`, walnut `#978e81`, espresso `#615b53`, onyx `#2c2c2c`, roast `#292622`. Le saffron `#d49653` est le seul accent : focus, barre de progression, erreurs.
- **Typographie :** serif 300 en majuscules pour les titres, Satoshi 500 pour le texte.
- **Formes :** rayon 0 sur les cartes, 3 px sur les boutons, filets de 1 px.
- **Grade photo :** les images du hero, de l'ambiance et du contact passent en monochrome chaud (`grayscale + sepia`). Les autres reçoivent un léger sépia.

## 4. Décisions prises

- **Pas de loader, pas de Lenis.** L'utilisateur l'a demandé : le site s'affiche plus vite et il y a moins de JS.
- **GSAP auto-hébergé** plutôt qu'un CDN, pour ne pas dépendre d'un tiers.
- **Repli sans animation.**
  - Si `prefers-reduced-motion` est actif, ou si GSAP ou ScrollTrigger ne chargent pas, la page reste lisible.
  - Le formulaire et le menu fonctionnent aussi sans GSAP : le gestionnaire du formulaire est placé avant le bloc GSAP.
- **Hovers en propriétés CSS `scale` et `translate`, jamais `transform`.** GSAP possède `transform` ; mélanger les deux décalait les titres de « Pourquoi nous choisir ».
- **Formulaire fusionné dans « Parlons de votre événement ».** L'utilisateur a refusé une section devis séparée, qui faisait doublon.
- **FormSubmit** plutôt qu'un backend :
  - envoi AJAX en JSON, avec un champ anti-spam caché `_honey` (un robot reçoit un faux succès) ;
  - date minimale = aujourd'hui, `reportValidity()` avant l'envoi ;
  - bouton désactivé pendant l'envoi, message de statut lu par les lecteurs d'écran (`aria-live`) ;
  - en cas d'échec, le message renvoie vers le téléphone.
- **Choix d'images :** l'utilisateur préfère choisir dans une planche numérotée que je prépare (hero : n° 19 ; petite image de L'agence : H).
- **SEO :**
  - title « DJ et prestations événementielles à Arras et Lille | AD Evenements » ;
  - meta description alignée sur le H1 ;
  - noms de fichiers d'images descriptifs et `alt` renseignés.

## 5. Problèmes connus

- **CSS en double :** `.cta__inner` et `.cta__contacts` sont définis deux fois (bloc d'origine, puis bloc « contact + quote form »). Le second l'emporte. `.cta__actions` et `.cta .link` ne servent plus.
- **Bouton de la nav :** `setNav()` bascule `btn--dark` / `btn--light` sur le bouton, ce qui est inutile depuis `btn--fill`, qui gère seul ses couleurs. C'est sans effet visible.
- **Liens des cartes vers l'ancien site :** ils pointent vers les pages de l'ancien site (`https://adevenements.fr/dj-mariage-hauts-de-france`, etc.). Ils casseront si ces URL changent pendant la refonte.
- **Lien « Options » :** il mène au marquee (`#options`), pas à une vraie section.
- **Images lourdes :** `width`/`height` et `srcset` sont en place, mais pas de WebP (hero à 696 Ko).
- **Polices externes :** Google Fonts et Fontshare sont chargées depuis des CDN tiers. La police officielle TT Ramillas est remplacée par Cormorant Garamond.
- **E-mail visible dans la page :** l'adresse contact@ apparaît en clair dans le HTML (action du formulaire, `mailto:`), ce qui l'expose au spam.
- **Git local incomplet :** `images/` et `CONTEXT.md` ne sont pas indexés et aucun commit n'existe. Le dépôt GitHub `Adevent_site` a été poussé à la main par l'utilisateur.
- **Vérification mobile :** le changement d'image de L'agence n'a été vérifié qu'en desktop (1440×900).
- **Animation Ambiance non vérifiée :** l'utilisateur a signalé qu'elle ne fonctionnait pas. Cause non trouvée, car le rendu n'a pas pu être testé sans serveur localhost. À confirmer sur téléphone.
- **Responsive tablette et téléphone :** aucun audit fait. La demande « 100 % responsive » attend une liste des sections qui cassent, avec captures, et l'accord sur la suppression des animations liées au défilement sur mobile.
- **Classes de nav :** `setNav()` bascule toujours `btn--dark` / `btn--light` sur le bouton, sans effet visible (voir plus haut).

## 6. Tâches restantes

**Côté utilisateur**

- [ ] Activer FormSubmit : faire un premier envoi réel, puis cliquer le lien de confirmation reçu sur contact@adevenements.fr.
- [ ] Pousser sur GitHub `index.html`, `images/` (dont `agence-piste-de-danse.jpg`) et `CONTEXT.md`, et retirer `agence-champagne.jpg` s'il est déjà en ligne.
- [ ] Mentionner FormSubmit dans les mentions légales et la politique de confidentialité (RGPD).

**Côté technique (suggestions)**

- [x] Créer les pages légales (19/11/2026). Reste à remplir les `[à compléter]` (SIRET, hébergeur, etc.).
- [ ] Optimiser les images : WebP, `srcset`, `width`/`height`, préchargement de l'image du hero.
- [ ] Nettoyer le CSS en double et le basculement inutile de classes dans `setNav()`.
- [x] Construire les pages de service et remplacer les liens vers l'ancien site (19/11/2026). Reste : prix, avis par service, vraies salles (TODO dans chaque page).
- [x] Ajouter les données structurées `LocalBusiness`, les balises Open Graph et un favicon (19/11/2026).
- [ ] Revérifier la page sur mobile (375×812) après les derniers changements.
- [ ] Tester l'Ambiance, le marquee et les boutons sur localhost (1440, 768 et 375 px). Lancer le serveur à la main : `python3 -m http.server 8765`.
- [ ] Audit responsive tablette (768 px) et téléphone (375 px) : attendre la liste des points qui cassent, puis corriger seulement ceux-là.

## 7. Journal des modifications

**19 nov. 2026, espaces dans les titres en lignes de l'accueil (non commité)**

- Audit contenu (`/seo-content`) : les 7 `<h2 data-lines>` de l'accueil avaient leurs `<span class="line">` collés, d'où un texte lu par Google et les lecteurs d'écran du type « Nos prestationsDJ », « Pourqui ? ». Ajout d'une espace entre les spans : rendu et animation GSAP inchangés (`.line` en `display:block`). Garder cette espace dans tout nouveau titre en lignes.
- Fil d'Ariane du bas collé à la bordure du pied de page (signalé par l'utilisateur) : `.crumbs` passe de `padding:20px var(--gut) 0` à `28px var(--gut) 20px` sur les 9 pages internes.
- Section « Pour compléter votre événement » des 6 pages de service refaite (demande utilisateur) : titre au-dessus, 3 cartes avec photo (`.related__img`, image `alt=""` car le titre de la carte suffit), en ligne sur ordinateur, photo à gauche du texte sous 960 px. Règle : **la carte DJ ne cite aucun événement** (« DJ », lien vers `/#prestations`). Blind test et quiz proposé partout sauf sur sa propre page. Cartes : mariage, particulier, entreprise → blind test, photobooth, videobooth ; blind test → DJ, photobooth, videobooth ; photobooth → DJ, videobooth, blind test ; videobooth → DJ, photobooth, blind test.

**19 nov. 2026, hero des pages internes aligné sur l'accueil (non commité)**

- Retour utilisateur : hero trop grand et fil d'Ariane en haut à gauche non voulu. Consigne : reproduire l'accueil.
- Le hero des pages internes reprend exactement celui de l'accueil : même hauteur (100 svh), mêmes classes et tailles (`.hero__tag` serif 500 de 28 à 50 px, `.hero__sub` 15 px, ligne de preuves, boutons à droite), mot décoratif à la taille de celui de l'accueil, même pied « Découvrir », même grade photo et même dégradé. Mêmes règles mobiles. Mot décoratif = nom de la page (Mariage, Privé, Entreprise, Blind test, Photobooth, Videobooth, Réalisations, à la demande de l'utilisateur) : même taille que « AD Evenements » sur l'accueil, réduite automatiquement si le mot est trop long (`.wordmark--page`, variables `--n` / `--nm`). Vérifié sans débordement à 1440, 768 et 375 px sur les 7 pages.
- Même intro que l'accueil (ouverture de la photo, dézoom, lettres du wordmark, fondus, nav) et même effet au scroll (photo en parallaxe, « AD » et « Evenements » qui s'écartent, contenu qui s'efface).
- Nav comme l'accueil : transparente sur le hero, parchment après, roast sur le bloc contact final. Bouton « Devis gratuit » de la nav en contour (`btn--light`) sur le hero et sur le bloc sombre, plein onyx sur la barre parchment.
- Fil d'Ariane retiré du haut. Il reste visible, discret, en bas de page juste au-dessus du pied de page, pour que le JSON-LD `BreadcrumbList` corresponde à du contenu visible (`CLAUDE.md` §4).
- Les pages internes chargent Cormorant Garamond 300 et 500, comme l'accueil.

**19 nov. 2026, pages internes immersives et animées (non commité)**

- Demande : appliquer aux pages refaites la consigne de départ (site premium, très immersif, GSAP + ScrollTrigger, hero plein écran avec intro marquante, sans effet gadget).
- **Hero plein écran** sur les 6 services et Réalisations : photo pleine largeur (gradée sépia, dégradé sombre), fil d'Ariane en haut, H1 + texte + boutons en bas, grand mot décoratif (`.wordmark`, `aria-hidden` : Mariage, Privé, Entreprise, Blind test, Photobooth, Videobooth, Réalisations), ligne « Découvrir » animée. Pages légales : hero court sans photo.
- **Intro** (même grammaire que l'accueil) : ouverture de la photo en `clip-path` depuis le centre, dézoom, lettres du mot décoratif puis mots du H1 qui montent, textes et nav en fondu. Démarre dès que polices et image sont prêtes, au plus tard après 1 s.
- **Au scroll** : parallaxe de la photo du hero et glissement du mot décoratif, contenu du hero qui s'efface ; barre de progression saffron ; titres H2 révélés mot par mot ; paragraphes en fondu montant ; listes (inclus, avis, logos, FAQ, services liés, notes) en cascade ; filets des « inclus » qui se tracent ; photos de la galerie révélées par `clip-path` + dézoom, colonne du milieu décalée en parallaxe (≥ 961 px) ; bloc contact final qui s'ouvre d'un panneau encadré au plein écran, avec photo de fond en parallaxe.
- **Profondeur et micro-interactions** : titres de section collants (`position:sticky`, ≥ 961 px) pendant que le contenu défile ; boutons magnétiques (souris) ; cartes « services liés » et avis qui se soulèvent au survol ; ouverture fluide des FAQ (`::details-content`, navigateurs récents).
- **Nav** : fixe et transparente sur le hero, roast pleine dès que le hero est passé. Sans JS : nav roast pleine.
- **Repli** : sans JS, si GSAP ne charge pas ou avec « réduire les animations », tout le contenu est visible et rien ne bouge (le code GSAP ne s'exécute pas). Le découpage en mots se fait en JS : le texte reste entier dans le HTML.
- GSAP 3.12.5 et ScrollTrigger auto-hébergés (`/js/`), chargés en fin de `<body>`, en cache d'une page à l'autre.
- Limite : les photos du hero sont servies au mieux en 1 400 px de large (originaux plus petits) ; un peu douces sur grand écran. Exporter des versions de 2 400 px depuis MEDIAS si besoin.

**19 nov. 2026, page Réalisations et retrait de la section Avis (non commité)**

- Nouvelle page `/realisations` (`realisations/index.html`) : galerie de 10 photos du client avec légende (mariage à Arras, Carlton, rooftop à Paris, dîner de gala, anniversaire 40 ans, quiz, photobooth, videobooth), les 11 logos de références (+ Green Way Festival cité dans le texte), notes Google / Mariages.net, badge Wedding Awards 2026 et les 7 avis réels. JSON-LD `BreadcrumbList`. Image OG `images/og/realisations.jpg`. Ajoutée au sitemap, à la 404, au pied de page de toutes les pages et au tableau des URL de `CLAUDE.md`. Page de références, pas de service : la règle des 600 mots ne s'applique pas (561 mots).
- Nouvelles images WebP : `ambiance-eclairage-soiree`, `contact-salle-reception`, `agence-son-lumiere`, `agence-entreprise` (+ `-900`), `agence-soiree-privee`.
- Le lien « Réalisations » du menu pointe maintenant vers `/realisations`. Sur l'accueil, le bloc « Ils nous ont fait confiance » garde ses logos et gagne un bouton « Voir nos réalisations ».
- Accueil : section Avis (`#avis`) supprimée, avec son CSS (`.reviews`, `.review`, `.score`, `.award`) et son JS (défilement continu, animation GSAP). Les numéros de section sont décalés : (04) Références à (07) Contact. Les avis restent sur `/realisations` et sur les pages de service ; la ligne « 5,0/5 » du hero est conservée.

**19 nov. 2026, menu déroulant sur toutes les pages (non commité)**

- Ordre du menu (demande de l'utilisateur) : Prestations DJ (sous-menu des 3 pages DJ) · Photobooth · Videobooth 360 · Blind test & quiz · Réalisations (`/#realisations`, la section des logos de références de l'accueil) · Contact. « Avis » et « Animation & location » ne sont plus dans le menu.
- Ordinateur : « Prestations DJ » a un sous-menu (3 pages, avec une ligne de description). Entre 961 et 1240 px, le téléphone de la nav est masqué et les liens sont resserrés. Le libellé reste un lien vers sa section de l'accueil ; le chevron à côté est un bouton (`.dd__btn`, `aria-expanded`) qui ouvre le sous-menu au clavier et au toucher. Ouverture au survol sur les écrans à souris ; fermeture par Échap, clic à l'extérieur ou quand le focus sort. Sans JS : ouverture au survol et au focus. La page en cours est marquée `aria-current="page"` (saffron).
- Mobile (menu burger) : un accordéon `<details>` par groupe (natif, sans JS), avec un lien « Vue d'ensemble » vers la section de l'accueil. Sur une page de service, son groupe est ouvert d'office.
- Code identique sur l'accueil et les 8 pages internes : CSS « nav dropdowns » et bloc JS « Nav dropdowns ». Toute modification du menu est à reporter dans les 9 fichiers.

**19 nov. 2026, pages de service et pages légales (non commité)**

- 8 nouvelles pages, chacune dans son dossier (`<chemin>/index.html`) : les 6 services de `CLAUDE.md` §1, plus `/mentions-legales` et `/politique-de-confidentialite` (noindex).
- Gabarit commun, sans GSAP : nav roast fixe (`position:sticky`) avec menu burger, fil d'Ariane, hero 2 colonnes (texte + photo `fetchpriority="high"`), « Ce que nous prenons en charge », 2 sections de texte, « Comment ça se passe », avis réels (ou logos de références s'il n'y en a pas), FAQ en `<details>`, 2 services liés, bloc contact roast, pied de page avec tous les services et les pages légales. CSS en ligne dans chaque page (pas de feuille partagée) : une modification du gabarit doit être reportée dans les 8 fichiers.
- JSON-LD : `Service` (provider `@id` `#entreprise`) + `BreadcrumbList` sur les pages de service ; `BreadcrumbList` seul sur les pages légales.
- Images servies en WebP (`images/*.webp` et `*-900.webp`, cwebp q 70–78, toutes < 150 Ko). Images OG 1200×630 dans `images/og/` (dj-mariage, dj-particulier, dj-entreprise, blind-test-quiz, photobooth, videobooth-360).
- Textes : uniquement des faits déjà présents sur le site. Les manques sont des `<!-- TODO -->` en tête de `<main>` (prix, vraies salles, avis par service, détails des formules blind test, photobooth et videobooth) et des `[à compléter]` surlignés dans les pages légales (forme juridique, SIRET, TVA, directeur de publication, hébergeur, durée de conservation).
- Accueil : liens des cartes vers l'ancien domaine passés en chemins relatifs ; pied de page avec les 6 services. `404.html` : liens vers les 6 services (l'ancien lien « Options » est retiré). `sitemap.xml` : accueil + 6 services.
- À la mise en ligne : l'hébergeur doit servir `/dj-mariage-hauts-de-france` sans rediriger vers la version avec `/` final (le serveur Python local le fait), pour rester identique aux canonical.

**19 nov. 2026, nouvelle stratégie de la page d'accueil (non commité)**

- Nouvel ordre : Hero → Prestations DJ (`#prestations`) → En complément de nos prestations DJ (`#animations`, une seule section `.extras` : texte d'intro, 3 panneaux photo `.offer` dans `.offers#location`, dans l'ordre voulu par l'utilisateur : photobooth (04), videobooth 360 (05), blind test et quiz (06)) → Pour qui ? (`#pour-qui`, 11 publics en 3 groupes : particuliers ; entreprises et pros de l'événementiel ; associations, écoles, collectivités et établissements, chacun avec un lien vers l'offre adaptée) → Avis (`#avis`) → Références → Engagements → Comment ça se passe (`#etapes`) → Contact.
- Les sections **sont désormais dans cet ordre-là** : la section 2 « Structure de la page » plus haut décrit l'ancienne version.
- 6 offres : 3 DJ (mariage, événement privé, entreprise) en grandes cartes, puis blind test et quiz, photobooth, videobooth 360 en cartes plus petites (`.card--sm`). Grille statique (`.services__grid`) : le défilement horizontal épinglé et sa barre de progression sont supprimés.
- **Arrivée de Prestations DJ** : la photo de fond de `.djstage` s'ouvre d'un panneau encadré (`clip-path: inset(14px round 8px)`, cadre parchment) au plein écran pendant que la section monte (GSAP scrub, de « haut de section en bas d'écran » à « haut de section en haut d'écran »). Le hero défile normalement (il ne reste pas fixe, à la demande de l'utilisateur).
- **Vidéos de fond de Prestations DJ** : une vidéo par carte, superposées dans `.djstage__bg`, licence Pexels gratuite (sans mention obligatoire) :
  - Mariage : `videos/danse-guirlandes-mariage.mp4` (Pexels 32009248, Orhan Pergel), boucle aller-retour 13,2 s, 1,7 Mo ;
  - Événement privé : `videos/soiree-privee-cierges.mp4` (Pexels 6396219, Pavel Danilyuk), recadrée en 16/9, boucle avec fondu 12 s, 2,6 Mo ;
  - Entreprise : `videos/soiree-entreprise-toast.mp4` (Pexels 6396220, Pavel Danilyuk : toast d'équipe au champagne), boucle avec fondu 11,8 s, 1,5 Mo. Remplace la salle de gala (Pexels 30801413), jugée inadaptée.
  Toutes en 1280×720, H.264, sans son, avec une image d'attente `images/<nom>-poster.jpg`, `preload="none"`. Au défilement, la vidéo n+1 apparaît en fondu à mesure que la carte n+1 monte vers le milieu de l'écran (et inversement en remontant). Seules les vidéos visibles jouent ; une vidéo entièrement recouverte se met en pause. « Réduire les animations » : images d'attente seulement, toujours en fondu. Les fichiers `dj-*.jpg` ne servent plus.
- **Prestations DJ en « scène » (`.djstage`)** : photo de fond fixe (`position:sticky`), titre fixe à gauche, 3 cartes translucides (`.djcard`, flou d'arrière-plan) qui défilent à droite. La carte qui passe au milieu de l'écran s'allume et amène sa photo en fond (fondu + léger dézoom). Géré par un `IntersectionObserver`, sans GSAP. Sans JS : première photo et cartes pleinement lisibles. Mobile : même principe, titre non fixe, cartes pleine largeur. Chaque carte contient une photo (`.djcard__img`, 16/10) et 3 points inclus (`.djcard__list`) : mariage = `agence-dj-mariage-arras`, privé = `agence-soiree-privee`, entreprise = `agence-entreprise`.
- **En complément (`.extras`) en panneaux dépliables** : les cartes statiques (`.card`, `.services__grid`) sont remplacées par 3 grands panneaux photo côte à côte (`.offer`, 78 svh de haut). Le panneau ouvert (`.is-active`, le photobooth au départ) prend 2,6 fois plus de place, sa photo s'éclaircit et dézoome, puis ses détails apparaissent en cascade : texte, 3 étiquettes reprises des textes existants, « Découvrir → ». Les panneaux fermés n'affichent que le numéro, le type et le titre, avec un « + ». Ouverture au survol sur grand écran (délai de 110 ms pour éviter les ouvertures en passant), au clic ou au toucher, et au clavier (focus sur le lien). La taille du titre suit la largeur du panneau (`cqi`). À l'arrivée, les panneaux montent l'un après l'autre (GSAP, `clip-path`) pendant que leurs photos se posent. Mobile et tablette (≤ 960 px) : panneaux empilés, le panneau ouvert fait 74 svh et les autres 156 px ; on ouvre en touchant, et le panneau ouvert est ramené à l'écran. Sans JS : tous les panneaux sont ouverts et lisibles. « Réduire les animations » : aucune transition.
- Sections **Ambiance** et **marquee des options** supprimées (HTML, CSS, JS).
- Bloc **« Du premier échange à la dernière danse »** de L'agence supprimé (liste fixe + 6 photos, CSS et JS). Puis le manifeste aussi : la section **L'agence** (`#agence`) n'existe plus, ni son lien dans le menu, ni l'effet des mots qui s'allument. Les images `agence-*.jpg` ne servent plus, sauf les 3 réutilisées dans les cartes DJ. Les images `ambiance-eclairage-soiree*.jpg` ne servent plus.
- Hero : H1 « Agence DJ et animations à Arras, Lille et dans les Hauts-de-France », ligne de preuves (5,0/5, Wedding Awards), boutons « Devis gratuit » et « Réserver un appel ».
- Title « AD Evenements | DJ et animation à Arras et Lille », description et schema mis à jour.
- Nouvelles images (MEDIAS/CORPORATE/Quizz:Team Building/IMG_1942, MEDIAS/SECTION BACKGROUND/WEB/Videobooth/IMG_4262) : `animation-blind-test-quiz-interactif(-900).jpg`, `location-videobooth-360-hauts-de-france(-900).jpg`.
- **Calendly** : 3 boutons « Réserver un appel » (`data-calendly`, hero, étapes, contact) ouvrent https://calendly.com/contact-adevenements/reunion-de-suivi dans un nouvel onglet.
- Liens des cartes blind test et videobooth : chemins relatifs vers des pages pas encore créées (404 en local).

**19 nov. 2026, mise en conformité SEO avec `CLAUDE.md` (non commité)**

- `<head>` : title raccourci à 52 caractères (« DJ et événementiel à Arras et Lille | AD Evenements »), canonical `https://www.adevenements.fr/`, Open Graph et Twitter, liens favicon.
- JSON-LD `EntertainmentBusiness` (`@id` `#entreprise`) : adresse 16 rue des Capucins 62000 Arras, téléphone, zone desservie, `sameAs` Google et Mariages.net. Pas de `priceRange` ni d'horaires (non fournis).
- Adresse ajoutée dans le bloc contact et dans le pied de page ; liens Mentions légales et Confidentialité dans le pied de page.
- `width`/`height` réels sur toutes les `<img>` (le CSS garde la main sur l'affichage).
- Nouveaux fichiers : `robots.txt`, `sitemap.xml` (accueil seul), `404.html` (noindex), `favicon.svg` / `favicon.ico` / `apple-touch-icon.png` / `icon-512.png` (logo AD parchment sur roast), `images/og/accueil.jpg` (1200×630, recadrage du hero).

**Nov. 2026, session de refonte (non commité, `index.html` seul modifié)**

- **Marquee des options :** réduit (padding 14 px, texte 18–26 px, losange en contour) et déplacé sous la section Ambiance, avant « Pourquoi nous choisir ». L'ancre `#options` est conservée.
- **Boutons « Devis gratuit » :** le CTA du hero est plein (`btn--fill`). Le bouton de la nav est en contour sur le hero, puis plein onyx dès `.is-solid` (règles `.nav.is-solid .btn`).
- **Section Ambiance :** l'animation liée au défilement a été remplacée par une révélation unique du cadre (`once: true`). Le cadre `position:sticky` de 120 svh est devenu une section de 100 svh. Le `clip-path` et le `scale(1.25)` de l'image ont été retirés. Le texte est pleine largeur.
- **Nom de marque :** « AD Événements » est devenu « AD Evenements » partout (titre, `aria-label` du logo, alt de l'image, objet du formulaire, pied de page, `CONTEXT.md`). Le wordmark décoratif du hero est « Evenements » sans accent. « Événements professionnels » est inchangé.
- **Aperçu local :** `.claude/launch.json` a été créé pour déclarer « AD Evenements Site ». Il lance `python3 -m http.server 8765`.

## Avis clients (#avis)

- Extraits de vrais avis relevés le 18/11/2026 : Google Maps (5,0 · 20 avis) et Mariages.net (5,0 · 24 avis, fiche e317676). Mettre à jour notes, nombres et extraits à la main.
- Badge officiel : `images/wedding-awards-2026.jpg`, copié depuis `MEDIAS/MARIAGE/badge-weddingawards_fr_FR.jpg`.

## Références (logos)

- Défilement de logos dans `images/logos/` (11 fichiers, relevés le 18/11/2026) : Wikimedia Commons (Red Bull, Bulgari, Club Med, Coca-Cola, Porsche, Carlton) et sites officiels (Versailles, Le Fairway, ValJoly, One Events Live, On Stage Audiovisuel).
- Teinte monochrome en CSS ; `--s` sur chaque `<li>` règle la taille visuelle ; `.logo--white` = logo blanc, reste teinté au survol.
- Green Way Festival : pas de logo trouvé (site en maintenance), à ajouter.

## L'agence (#agence)

- Colonne de texte fixe à gauche + 6 photos plein cadre à droite ; la ligne de la liste correspondant à la photo visible s'allume (≥ 961 px). Tablette/mobile : carrousel de photos avec légendes.
- Nouvelles photos exportées depuis MEDIAS : `agence-mariage` (MARIAGE/Mariage.jpg), `agence-soiree-privee` (PARTICULIER/Anniversaire 40 ans.jpeg, 736 px de large seulement), `agence-entreprise` (CORPORATE/HACHETTE PARIS/IMG_6266), `agence-son-lumiere` (CORPORATE/Corporate Carlton/IMG_2333).
