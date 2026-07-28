# Présentation du package rsonar (FR)

Ce répertoire contient une présentation Quarto en français, simple et orientée usage, pour expliquer les fonctionnalités principales de `rsonar`.

## Contenu

- `index.qmd` : slides RevealJS
- `_quarto.yml` : configuration du projet Quarto
- `images/` : visuels

## Générer la présentation

```bash
quarto render index.qmd
```

Le rendu est généré dans `docs/`.

## Ouvrir localement

```bash
quarto preview index.qmd
```

## Points couverts

- Pourquoi utiliser `rsonar`
- Analyse complète avec `sonar_analyse()`
- Score qualité en % avec `quality_score()`
- Rapport HTML avec `sonar_report()`
- Quality Gate avec `quality_gate()`
- **Auto-fix avec `sonar_fix()`** : 16 catégories de corrections automatiques (formatting, spacing, TRUE/FALSE, NULL, commas, parens, cleanup, simplify, pipes, return, assignment, comments, magrittr, library, namespace, dead_code)
- **`sonar_autofix()`** : automatisation complète du workflow GitLab/GitHub (analyse + commit + push + Merge Request) en une seule fonction, avec auto-détection de la plateforme CI
- **Pipeline CI complet** : description du workflow développeur avec les jobs `rsonar-check` (automatique) et `rsonar-autofix` (manuel)
- Exports CI (`export_junit()`, `export_sarif()`, `export_sonar_json()`)
- Comparaison et tendances (`sonar_diff()`, `sonar_trend()`)
