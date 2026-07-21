# Mes Recettes

App web statique (une page) — carnet de recettes de cuisine personnel, installable en PWA sur iPhone/mobile.

## Structure

- `index.html` — tout l'appli : CSS (thème clair/sombre via variables CSS), et un unique script JS.
  - `FAMILIES` — catégories de recettes (apéritif, entrée, plat, pain, sauce, dessert) avec leurs couleurs.
  - `RECIPES` — tableau JS codé en dur, une entrée par recette : `id`, `title`, `family`, `subtitle`, `time`, `tags`, `intro`, `sections` (chacune avec `heading` + `ingredients`/`steps`/`notes`).
  - Favoris stockés dans `localStorage` (clé `FAV_KEY`), pas de backend.
  - Recherche + filtre par famille + filtre favoris, tout côté client.
- `sw.js` — service worker : stratégie réseau-prioritaire avec fallback cache (`recettes-cache-v1`), pour permettre un usage hors-ligne basique.

## Pour ajouter une recette

Ajouter une entrée dans le tableau `RECIPES` (index.html) en suivant la forme des entrées existantes.

## Notes

- Pas de build, pas de dépendances — tout est en HTML/CSS/JS vanilla dans un seul fichier.
- Toutes les données personnelles (favoris, futures notes libres, liste de courses) vivent en `localStorage` : liées à un seul navigateur/appareil tant qu'il n'y a pas d'export/sync.
