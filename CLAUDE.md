# Mes Recettes

App web statique (une page) — carnet de recettes de cuisine personnel, installable en PWA sur iPhone/mobile.

## Structure

- `index.html` — tout l'appli : CSS (thème clair/sombre via variables CSS), et un unique script JS.
  - `FAMILIES` — catégories de recettes (apéritif, entrée, plat, pain, sauce, dessert) avec leurs couleurs.
  - `RECIPES` — tableau JS codé en dur, une entrée par recette : `id`, `title`, `family`, `subtitle`, `time`, `tags`, `intro`, `sections` (chacune avec `heading` + `ingredients`/`steps`/`notes`). Entouré des marqueurs `/* RECIPES:START */` … `/* RECIPES:END */` — ne pas les retirer, ils servent de point d'ancrage pour la réécriture automatique du fichier (voir "Édition en direct" ci-dessous).
  - Favoris stockés dans `localStorage` (clé `FAV_KEY`), pas de backend.
  - Recherche + filtre par famille + filtre favoris, tout côté client.
- `sw.js` — service worker : stratégie réseau-prioritaire avec fallback cache (`recettes-cache-v1`), pour permettre un usage hors-ligne basique.

## Pour ajouter une recette

Depuis l'appli, en mode édition (voir ci-dessous), ou manuellement en ajoutant une entrée dans le tableau `RECIPES` (index.html) en suivant la forme des entrées existantes.

## Édition en direct (depuis l'appli)

- Un bouton d'engrenage dans l'en-tête ouvre les paramètres d'édition : coller un jeton d'accès personnel GitHub (fine-grained, droits "Contents: Read and write" sur ce seul repo) active le mode édition **sur cet appareil**.
- Le jeton est stocké uniquement dans le `localStorage` de l'appareil qui l'a saisi — jamais dans le code source. Sans jeton, l'appli reste en lecture seule (c'est le cas pour toute personne à qui l'appli est partagée).
- En mode édition : bouton "Modifier" sur chaque recette, bouton "+" dans l'en-tête pour en créer une nouvelle, bouton "Supprimer" dans le formulaire d'édition.
- À la publication, l'appli appelle l'API GitHub Contents pour réécrire le bloc `RECIPES` directement dans `index.html` sur la branche `main`, et commite le changement (pas de relecture/PR, publication immédiate). GitHub Pages republie automatiquement ; les autres appareils récupèrent la mise à jour au chargement suivant (service worker réseau-prioritaire).
- Repo/branche/chemin ciblés par ce mécanisme : constantes `REPO_OWNER`, `REPO_NAME`, `REPO_BRANCH`, `REPO_FILE_PATH` en haut du script principal.

## Notes

- Pas de build, pas de dépendances — tout est en HTML/CSS/JS vanilla dans un seul fichier.
- Toutes les données personnelles (favoris, notes libres, liste de courses, jeton d'édition) vivent en `localStorage` : liées à un seul navigateur/appareil tant qu'il n'y a pas d'export/sync.
