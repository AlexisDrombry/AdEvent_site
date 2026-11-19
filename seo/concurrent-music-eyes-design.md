# Analyse concurrent : Music Eyes Design

Relevé fait le 19 novembre 2026 à partir de musiceyesdesign.com (accueil, `/tarifs`, `/lieux-de-reception`). Tout ce qui concerne ce concurrent vient de son site public, à revérifier avant toute réutilisation.

## 1. Recommandation : pas de page « AD Evenements vs Music Eyes Design »

Une page comparative nommant ce concurrent est déconseillée ici :

- **Pas de recherche à capter.** Les requêtes « [marque] vs [marque] » ou « alternative à [marque] » existent pour des logiciels, presque jamais pour un DJ local. Les clients cherchent « DJ mariage Lille » ou « DJ mariage château », pas le nom d'un concurrent.
- **Risque juridique.** En France, la publicité comparative (Code de la consommation, art. L122-1 et suivants) doit comparer des services équivalents de façon objective et vérifiable. Une erreur ou un ton dénigrant expose à une action du concurrent.
- **Données impossibles à vérifier.** AD Evenements n'a pas de prix publics (TODO dans `CLAUDE.md` §3). Le concurrent n'affiche qu'un « à partir de ». Le tableau comparatif serait donc creux.
- **Effet d'image.** Sur un marché de recommandation (mariage), une page qui cite un confrère fait plus petit qu'elle ne rapporte.

Le gabarit de schéma proposé par l'outil (`Product` + `aggregateRating`) est en plus interdit par `CLAUDE.md` §4. Aucun fichier JSON-LD n'est donc livré.

**À faire à la place :** prendre les requêtes sur lesquelles ce concurrent se positionne, avec les propres pages d'AD Evenements (section 4).

## 2. Ce que fait le concurrent

| Point | Music Eyes Design | AD Evenements |
|---|---|---|
| Base | Dunkerque | Arras |
| Zone annoncée | Hauts-de-France (Nord, Pas-de-Calais, Somme), Paris, international | Arras, Lille, Hauts-de-France |
| Positionnement | « French luxury DJ », mariage en château et domaine | Agence DJ et animations, mariage + privé + entreprise |
| Title de l'accueil | « DJ mariage château & domaine \| Hauts-de-France & Paris » | « AD Evenements \| DJ et animation à Arras et Lille » |
| Prix publiés | Oui : page `/tarifs`, mariage « à partir de 2 490 € » (son, lumière, micro sans fil, fumée) | Non (TODO) |
| Page des lieux | Oui : `/lieux-de-reception`, environ 60 salles classées par secteur (Flandre, Côte d'Opale, Artois, Métropole lilloise…) | Non (TODO « vraies salles » dans chaque page) |
| Options | Une page par option (étincelles froides, fumée lourde, piste LED, laser, saxophoniste, vinyles…) : environ 18 URL | Photobooth, videobooth 360, blind test et quiz |
| Avis | « Plus de 200 avis », page `/avis` | Avis réels sur `/realisations` et pages de service, 5,0/5 |
| Équipe | 5 personnes présentées par prénom | Non présentée |
| Ancienneté | « Depuis 2014 » | Non indiquée sur le site |
| FAQ | Page `/faq` dédiée | FAQ sur chaque page de service |
| Langue | Français + anglais | Français |

### Ce que AD Evenements fait mieux ou à part

- **Offre entreprise et particulier** avec des pages dédiées (le concurrent est centré mariage).
- **Blind test et quiz interactifs** et **videobooth 360** : le concurrent n'a pas ces offres.
- **Ancrage Arras / Artois** : le concurrent couvre l'Artois avec seulement 4 salles et vit sur le littoral et la Flandre.

## 3. Écarts de contenu à combler (par ordre de priorité)

1. **Prix « à partir de ».** Le concurrent se classe sur « tarif DJ mariage » avec un prix dans le title. Ajouter un « à partir de » sur `/dj-mariage-hauts-de-france` (et les autres services) ferait gagner ces requêtes et lever le TODO existant. **Montant à fournir par vous.**
2. **Salles où vous êtes intervenu.** Sa page des lieux capte « DJ mariage [nom du château] » et « DJ mariage [ville] ». Une section « Lieux où nous avons animé » sur la page mariage (ou une page `/lieux-de-reception` si la liste dépasse 15 salles), classée par secteur (Arrageois, Artois, Métropole lilloise, Douaisis…), ciblerait le même type de requêtes là où le concurrent est faible. **Liste à fournir par vous** : uniquement des salles où vous avez vraiment joué.
3. **Mot-clé « château / domaine ».** Ajouter cette variante naturelle dans le texte de la page mariage (« mariage en château ou en domaine autour d'Arras »), sans toucher au title actuel.
4. **Présentation de l'équipe et ancienneté.** Signal de confiance (E-E-A-T) que le concurrent a et pas vous : une courte section « Qui sommes-nous » sur l'accueil ou `/realisations`. **Faits à fournir** (prénoms, année de création).
5. **Options de scène.** Si vous proposez des étincelles froides, de la fumée lourde ou une piste LED, une ligne dans la page mariage suffit au début ; des pages dédiées seulement si l'offre existe vraiment et a du contenu propre.

## 4. Mots-clés à viser (vos pages, pas une page comparative)

| Requête | Page cible | Concurrent présent ? |
|---|---|---|
| DJ mariage Arras | `/dj-mariage-hauts-de-france` | Non (base Dunkerque) |
| DJ mariage château Pas-de-Calais / Artois | `/dj-mariage-hauts-de-france` | Oui, faible sur l'Artois |
| tarif DJ mariage Hauts-de-France | `/dj-mariage-hauts-de-france` (avec prix) | Oui, page dédiée |
| DJ mariage Lille | `/dj-mariage-hauts-de-france` | Oui (2 salles lilloises seulement) |
| DJ soirée entreprise Lille | `/dj-evenement-professionnel-hauts-de-france` | Oui, peu développé |
| blind test animation entreprise Lille | `/animation-blind-test-quiz-hauts-de-france` | Non |
| location videobooth 360 Arras / Lille | `/location-videobooth-360-hauts-de-france` | Non |
| DJ mariage [nom de salle] | section lieux (à créer) | Oui, sur le littoral |

Volumes de recherche non mesurés (pas d'accès DataForSEO ou Search Console dans cette session).

## 5. Ce dont j'ai besoin pour passer à l'action

- un prix « à partir de » par service (ou votre refus) ;
- la liste des salles où vous avez animé, avec la ville ;
- année de création et prénoms de l'équipe, si vous voulez une section « Qui sommes-nous » ;
- les options de scène réellement proposées (étincelles, fumée lourde, piste LED…).
