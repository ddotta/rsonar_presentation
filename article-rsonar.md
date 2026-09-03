# rsonar : la qualité de code façon SonarQube, directement dans R

Quand on développe des packages ou des projets R en équipe, une question revient vite : comment s'assurer que le code reste propre, testé et maintenable, sans avoir à déployer une usine à gaz de type SonarQube ? C'est exactement le problème que résout **[rsonar](https://github.com/ddotta/rsonar)**, un package R développé par Damien Dotta.

## Le principe : un SonarQube pensé pour R

`rsonar` part d'un constat simple : l'écosystème R dispose déjà d'excellents outils d'analyse de qualité (`lintr`, `styler`, `covr`, `goodpractice`), mais chacun produit son propre rapport, dans son propre format, sans vue d'ensemble. `rsonar` vient les orchestrer et centraliser leurs résultats dans un rapport unique, interactif, avec une estimation de la dette technique — un peu comme le ferait SonarQube pour d'autres langages.

| Outil | Rôle |
|---|---|
| `lintr` | Analyse statique — bugs, style, complexité |
| `styler` | Formatage du code (style tidyverse) |
| `covr` | Couverture de tests |
| `goodpractice` | Bonnes pratiques de packaging R |

Le package reprend même le vocabulaire de SonarQube pour rester familier aux équipes qui en ont l'habitude : les *issues* de `lintr` deviennent les "issues", la couverture `covr` alimente le "Coverage", et un véritable **Quality Gate** (`quality_gate()`) permet de fixer des seuils bloquants (couverture minimale, zéro erreur de lint, etc.).

## Un usage pensé pour tous les contextes

Ce qui rend `rsonar` particulièrement pratique, c'est qu'il s'utilise aussi bien en local que dans une CI :

- **En local, pour un feedback instantané** : la fonction `quality_score(".")` affiche directement un pourcentage de qualité et une note (A à E) dans la console de l'IDE, sans configuration de CI ni de forge.
- **En profondeur, pour une analyse complète** : `sonar_analyse()` lance l'ensemble des outils sur un package et retourne un objet exploitable par toutes les autres fonctions.

Parmi les fonctionnalités clés :

- `sonar_report()` génère un **rapport HTML interactif**.
- `debt_index()` calcule un **indice de dette technique** noté de A à E, à la manière du "Maintainability Rating" de SonarQube.
- `sonar_hotspots()` classe les fichiers **à corriger en priorité**, en fonction de leur dette technique.
- `sonar_diff()` compare deux analyses pour détecter des **régressions de qualité** entre deux versions.
- `sonar_trend()` permet de suivre l'**évolution de la qualité dans le temps**.
- `export_sonar_json()`, `export_sarif()` et `export_junit()` exportent les résultats dans des formats standards, respectivement compatibles avec l'import générique d'issues SonarQube, le **Code Scanning GitHub** (SARIF), et les artefacts de tests **GitLab CI** (JUnit XML).

## La correction automatique avec air

Autre atout notable : l'intégration avec **[air](https://github.com/posit-dev/air)**, le formateur de code R rapide développé par Posit. Avec `sonar_fix()`, `rsonar` peut :

1. installer `air` via `install_air()` ;
2. formater automatiquement l'ensemble des fichiers R du projet ;
3. et, en une seule option, ouvrir directement une Merge Request ou une Pull Request avec les corrections.

```r
library(rsonar)

# Voir ce qui serait modifié, sans rien changer
fix <- sonar_fix(".", dry_run = TRUE)

# Corriger automatiquement le style du code
fix <- sonar_fix(".")

# Corriger ET ouvrir une Merge Request
fix <- sonar_fix(".", create_mr = TRUE)
```

## Le point fort : la création automatique de MR/PR

C'est sans doute la fonctionnalité la plus intéressante pour un usage en équipe : intégré dans une pipeline CI, `rsonar` peut ouvrir tout seul une Merge Request (GitLab) ou une Pull Request (GitHub) contenant les corrections de style. Le job `rsonar-fix` est conçu comme une **tâche manuelle optionnelle** de la pipeline :

1. installation du formateur `air` ;
2. exécution de `sonar_fix()` sur l'ensemble des fichiers R ;
3. ouverture automatique d'une MR/PR avec les changements.

Résultat : plus besoin de reformater son code à la main avant de commiter, ni de faire des allers-retours en revue de code sur de simples questions de style. Un développeur déclenche le job, et une MR toute prête arrive avec le diff de mise en forme — il ne reste plus qu'à la relire et la fusionner.

Deux dépôts GitLab illustrent concrètement ce fonctionnement :

- **[rproject_for_rsonar](https://gitlab.com/ddotta/rproject_for_rsonar)** : un projet R d'exemple, qui sert de terrain de test pour exécuter `rsonar` "en conditions réelles" et déclencher la génération automatique de MR.
- **[ci-templates-r](https://gitlab.com/ddotta/ci-templates-r)** : un dépôt de templates de pipelines CI réutilisables, dans lequel est notamment défini le job `rsonar-fix` qui orchestre l'installation d'`air`, le passage de `sonar_fix()` et la création de la Merge Request.

En combinant les deux, une équipe peut brancher `rsonar` sur n'importe quel projet R en quelques lignes de configuration GitLab CI, et bénéficier immédiatement d'un contrôle qualité centralisé... jusqu'à la correction automatisée du style de code, sans jamais quitter sa forge.

---

Pour aller plus loin, la documentation complète du package est disponible sur [ddotta.github.io/rsonar](https://ddotta.github.io/rsonar/), avec notamment un article dédié à l'[intégration CI/CD](https://ddotta.github.io/rsonar/articles/ci-integration.html).
