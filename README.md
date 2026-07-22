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
- **Auto-fix avec `air`** : formatage automatique du code R via `sonar_fix()` et `install_air()`
- **Mécanique fix → MR GitLab** : création automatique de Merge Requests avec les corrections
- Exports CI (`export_junit()`, `export_sarif()`, `export_sonar_json()`)
- Comparaison et tendances (`sonar_diff()`, `sonar_trend()`)
