# CLAUDE.md : règles SEO pour le codage du site AD Evenements

Claude lit ce fichier à chaque session. Les règles ci-dessous sont **obligatoires** pour tout code ajouté ou modifié.
Le contexte technique (architecture, style, décisions) est dans `CONTEXT.md` : le lire avant de coder.

## 0. Faits de l'entreprise (source unique, à recopier à l'identique partout)

| Donnée | Valeur |
|---|---|
| Nom | AD Evenements (sans accent, toujours écrit ainsi) |
| Domaine final | https://www.adevenements.fr |
| Téléphone | 06 77 49 38 59 / `+33677493859` |
| Ville | Arras (62000), Pas-de-Calais |
| Adresse postale | 16 rue des Capucins, 62000 Arras |
| Zone desservie | Arras, Lille, Hauts-de-France |
| Fiche Google | https://maps.google.com/?cid=3248654613300674967 |
| Mariages.net | https://www.mariages.net/musique-mariage/ad-evenements--e317676 |

L'adresse doit être écrite exactement comme sur la fiche Google Business (nom, adresse, téléphone identiques).
Si une donnée manque (SIRET, horaires, réseaux sociaux), **demander**, ne pas deviner, et laisser un `<!-- TODO: ... -->`.

## 1. Structure des URL (à ne pas casser)

Pages de service, en reprenant les chemins de l'ancien site pour garder leur référencement :

| Page | URL |
|---|---|
| Accueil | `/` |
| DJ mariage (cœur de métier) | `/dj-mariage-hauts-de-france` |
| DJ événement privé : anniversaire, baptême, communion, mairie… (cœur de métier) | `/dj-particulier-hauts-de-france` |
| DJ événement d'entreprise : séminaire, gala, lancement de produit… (cœur de métier) | `/dj-evenement-professionnel-hauts-de-france` |
| Blind test et quiz interactifs (une page, plusieurs formules selon le nombre de manches) | `/animation-blind-test-quiz-hauts-de-france` |
| Location de photobooth (offre propre) | `/location-photobooth-hauts-de-france` |
| Location de videobooth 360 (offre propre) | `/location-videobooth-360-hauts-de-france` |
| Réalisations : photos, références et avis clients | `/realisations` |
| Mentions légales | `/mentions-legales` |
| Confidentialité | `/politique-de-confidentialite` |

- Chaque page vit dans un dossier : `dj-mariage-hauts-de-france/index.html` (URL sans `.html`).
- URL en minuscules, mots séparés par des tirets, sans accent, sans date.
- **Ne jamais renommer ou supprimer une URL existante** sans une redirection 301 notée dans la section 9.
- Les liens internes sont **relatifs à la racine** (`/dj-mariage-hauts-de-france`), jamais vers `https://adevenements.fr/...` une fois la page créée.

## 2. Gabarit `<head>` obligatoire pour chaque page

```html
<html lang="fr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>[Mot-clé principal] à [Ville] | AD Evenements</title>
<meta name="description" content="[Phrase de 140 à 155 caractères : service, lieu, bénéfice, appel à l'action]">
<link rel="canonical" href="https://www.adevenements.fr/[chemin]">
<meta property="og:type" content="website">
<meta property="og:locale" content="fr_FR">
<meta property="og:site_name" content="AD Evenements">
<meta property="og:title" content="[même idée que le title]">
<meta property="og:description" content="[même idée que la description]">
<meta property="og:url" content="https://www.adevenements.fr/[chemin]">
<meta property="og:image" content="https://www.adevenements.fr/images/og/[page].jpg">
<meta name="twitter:card" content="summary_large_image">
<link rel="icon" href="/favicon.svg" type="image/svg+xml">
<link rel="icon" href="/favicon.ico" sizes="32x32">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">
<!-- JSON-LD : voir section 4 -->
</head>
```

Règles :
- **Title** : 50 à 60 caractères, mot-clé principal au début, unique sur tout le site.
- **Description** : 140 à 155 caractères, unique, avec la ville et « devis gratuit ».
- **Canonical** : URL absolue avec `https://www.`, sans paramètre, identique à l'URL de la page.
- **Image OG** : 1200×630, JPEG de moins de 300 Ko, dans `images/og/`.
- Pages légales : ajouter `<meta name="robots" content="noindex, follow">`.

## 3. Contenu et balises de texte

- **Un seul `<h1>`** par page, qui contient le mot-clé principal et la ville.
- Hiérarchie `h2` puis `h3` sans saut de niveau. Les titres décoratifs (wordmark, marquee) restent hors titres, avec `aria-hidden="true"`.
- **Minimum 600 mots utiles par page de service**, rédigés pour le visiteur :
  - ce qui est inclus (matériel, durée, déroulé) ;
  - pour qui, et dans quels lieux (citer de vraies salles et villes de la région quand l'utilisateur les fournit) ;
  - fourchette de prix ou « à partir de », si l'utilisateur l'accepte ;
  - 2 ou 3 avis réels liés au service ;
  - une FAQ de 4 à 6 vraies questions (en HTML visible ; **pas** de schema FAQPage, Google l'a retiré en mai 2026) ;
  - un appel à l'action vers `/#devis` ou le téléphone.
- Mots-clés : un mot-clé principal par page (par exemple « DJ mariage Arras ») et ses variantes naturelles (Lille, Pas-de-Calais, Nord, Hauts-de-France). **Pas de bourrage** : la ville apparaît 3 à 6 fois par page, pas plus.
- **Ne jamais inventer** d'avis, de chiffres, de clients, de prix ou de récompenses. Tout fait doit venir de l'utilisateur ou de `CONTEXT.md`.
- Pas de contenu dupliqué entre les pages : chaque page de service a son propre texte.
- Les textes importants sont dans le HTML au chargement, pas injectés en JavaScript.

## 4. Données structurées (JSON-LD)

- **Accueil** : un bloc `LocalBusiness` (type `EntertainmentBusiness`) avec `name`, `url`, `telephone`, `image`, `logo`, `address` (`PostalAddress` : 16 rue des Capucins, 62000 Arras, FR), `areaServed` (Arras, Lille, Hauts-de-France), `sameAs` (fiche Google, Mariages.net, réseaux), `priceRange`. Ajouter `geo` et `openingHours` seulement si l'utilisateur les fournit.
- **Pages de service** : un bloc `Service` avec `serviceType`, `areaServed` et `provider` qui renvoie à l'entreprise par `"@id": "https://www.adevenements.fr/#entreprise"`.
- **Toutes les pages sauf l'accueil** : un `BreadcrumbList`.
- **Interdits** :
  - `aggregateRating` ou `review` recopiés depuis Google ou Mariages.net (contraire aux règles de Google) ;
  - `FAQPage` et `HowTo` (sans effet aujourd'hui) ;
  - toute valeur absente de la page visible.
- Valider chaque bloc avec https://validator.schema.org et le test des résultats enrichis de Google avant de valider la tâche.

## 5. Images

- Nom de fichier descriptif en minuscules avec tirets : `dj-mariage-chateau-arras.jpg`.
- `alt` qui décrit l'image en français (avec le lieu quand c'est naturel). Une image décorative a `alt=""`.
- **Toujours `width` et `height`** (dimensions réelles du fichier) pour éviter le décalage de mise en page.
- Format : **WebP** (avec un repli JPEG via `<picture>` si besoin), qualité 75 à 80.
- Toujours un `srcset` et un `sizes` : 900 px et 1600 px pour les images de contenu, 1200 px et 2400 px pour les images pleine largeur.
- Image principale du haut de page : `fetchpriority="high"`, **jamais** `loading="lazy"`. Toutes les autres : `loading="lazy"`.
- Poids maximal : 250 Ko pour une image pleine largeur, 150 Ko pour les autres.

## 6. Performance (Core Web Vitals)

Objectifs mesurés sur mobile : **LCP < 2,5 s, INP < 200 ms, CLS < 0,1**.
- Pas de nouvelle bibliothèque JS sans accord de l'utilisateur. GSAP reste auto-hébergé dans `js/`, chargé en fin de `<body>`.
- Polices : `display=swap`, au maximum 2 familles. Précharger la police du H1 si elle bloque l'affichage.
- Pas d'animation qui cache le contenu tant que le JS n'a pas chargé : le texte doit rester lisible sans JavaScript et avec `prefers-reduced-motion`.
- CSS critique en ligne dans le `<head>`, comme sur l'accueil actuel.

## 7. Maillage interne et navigation

- La nav et le pied de page lient chaque page de service et les pages légales avec de vrais `<a href>`, pas des boutons JS.
- Texte de lien descriptif (« DJ pour votre mariage », pas « cliquez ici ») ; « Découvrir → » est accepté si la carte contient un titre de service.
- Chaque page de service renvoie vers 2 autres services proches et vers le formulaire de devis.
- Pas de lien cassé : vérifier chaque `href` avant de valider la tâche.
- Liens externes : `target="_blank" rel="noopener"`.

## 8. Fichiers à la racine

À créer et à tenir à jour à chaque nouvelle page :
- `robots.txt` : tout autoriser, plus la ligne `Sitemap: https://www.adevenements.fr/sitemap.xml`.
- `sitemap.xml` : toutes les pages indexables, avec `<lastmod>` mis à jour à chaque modification. Sans les pages en `noindex`.
- `favicon.svg`, `favicon.ico`, `apple-touch-icon.png` (180×180), tirés du logo AD.
- Une page `404.html` utile, avec liens vers l'accueil et les services.

## 9. Redirections à configurer à la mise en ligne

Lister ici chaque ancienne URL qui change, avec sa nouvelle cible (301).

| Ancienne URL | Nouvelle URL |
|---|---|
| `/services-evenementiels-dj-hauts-de-france` | `/` (cible à confirmer avec l'utilisateur ; l'offre « Options » est supprimée) |

Forcer aussi `http://` vers `https://` et `adevenements.fr` vers `www.adevenements.fr` (ou l'inverse, mais une seule version, identique aux canonical).

## 10. Obligations légales (France)

- Pages `/mentions-legales` et `/politique-de-confidentialite` liées dans le pied de page de **chaque** page.
- La politique cite FormSubmit (envoi du formulaire), Google Fonts et Fontshare (polices chargées depuis des serveurs tiers).
- Pas de traceur (Analytics, pixel) sans bandeau de consentement.

## 11. Checklist avant de dire qu'une tâche est finie

Pour chaque page créée ou modifiée, vérifier sur http://localhost:8765 (voir `CONTEXT.md` pour lancer le serveur) :

- [ ] title et description uniques, avec la bonne longueur ;
- [ ] canonical correcte, balises OG complètes, image OG présente ;
- [ ] un seul H1, hiérarchie des titres correcte ;
- [ ] JSON-LD valide et conforme au contenu visible ;
- [ ] toutes les images ont `alt`, `width`, `height`, `srcset` ; seule l'image du haut de page n'est pas lazy ;
- [ ] aucun lien cassé, aucun lien interne vers l'ancien domaine ;
- [ ] page ajoutée au `sitemap.xml` et liée dans la nav ou le pied de page ;
- [ ] rendu vérifié à 375, 768 et 1440 px, texte lisible sans JavaScript ;
- [ ] aucune erreur dans la console ;
- [ ] `CONTEXT.md` mis à jour (journal des modifications).

Signaler à l'utilisateur toute case non cochée, avec la raison.

## 12. Après la mise en ligne (rappel pour l'utilisateur)

- Déclarer le site et envoyer `sitemap.xml` dans Google Search Console et Bing Webmaster Tools.
- Mettre l'URL du nouveau site sur la fiche Google Business, avec le même nom et le même téléphone.
- Mesurer les vraies performances avec `/seo google` et relancer `/seo audit https://www.adevenements.fr`.
