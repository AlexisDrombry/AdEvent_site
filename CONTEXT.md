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

- [ ] Créer les pages légales (mentions légales, confidentialité), obligatoires en France. Les liens du pied de page existent déjà (`/mentions-legales`, `/politique-de-confidentialite`) et donnent une 404 en attendant.
- [ ] Optimiser les images : WebP, `srcset`, `width`/`height`, préchargement de l'image du hero.
- [ ] Nettoyer le CSS en double et le basculement inutile de classes dans `setNav()`.
- [ ] Construire les autres pages : Mariage, Entreprise, Particulier, Photobooth, Options. Remplacer ensuite les liens vers l'ancien site.
- [x] Ajouter les données structurées `LocalBusiness`, les balises Open Graph et un favicon (19/11/2026).
- [ ] Revérifier la page sur mobile (375×812) après les derniers changements.
- [ ] Tester l'Ambiance, le marquee et les boutons sur localhost (1440, 768 et 375 px). Lancer le serveur à la main : `python3 -m http.server 8765`.
- [ ] Audit responsive tablette (768 px) et téléphone (375 px) : attendre la liste des points qui cassent, puis corriger seulement ceux-là.

## 7. Journal des modifications

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
