# Format Markdown / YAML Lumo

Ce fichier documente les conventions YAML/frontmatter utilisees par Lumo.

## Principes

- Le Markdown garde le contexte humain.
- Le frontmatter YAML garde les metadonnees lisibles par l'application.
- Le champ officiel pour placer un document dans une vue est `type`.
- Le champ officiel pour qualifier la nature d'un document est `category`.
- Ne pas utiliser `kind`.
- Les tags servent a retrouver, filtrer et regrouper les informations.

Types de vue :

```txt
project
journal
note
```

Regle d'affichage :

- seule la valeur `type` decide la vue principale
- `category` ne decide jamais la vue
- pendant l'edition, le Markdown est sauvegarde en live
- `type` et `category` sont relus depuis le YAML seulement quand on quitte l'editeur ou change de note
- la vue Notes affiche tous les documents, meme si `type` est invalide ou incomplet

Categories possibles :

```txt
incident
meeting
decision
question
documentation
suivi
demande
maintenance
worklog
```

## Projet

```yaml
---
title: Incident paiement PACS.002
type: project
category: incident
status: active
tags: [pacs, fournisseur]
personnes: [Daniel, SupportIT]
summary: Incident PACS.002 en cours.
---
```

Champs reconnus :

- `title` : titre lisible du projet, affiche dans les listes.
- `type` : vue principale. Pour un projet, utiliser `project`.
- `category` : categorie du projet.
- `status` : etat du projet.
- `tags` : tags libres.
- `personnes` : participants, contacts ou personnes concernees.
- `summary` : resume court affiche dans les listes.

Nom court todo.txt :

- Le nom court du projet reste stocke dans l'application.
- Il sert aux todos sous forme `+NomCourt`.
- Le champ YAML `title` sert seulement a l'affichage humain.

Activite projet :

- Lumo cherche les titres Markdown qui commencent par une date.
- La date la plus recente trouvee sert a afficher l'activite projet.
- Si aucune date n'est trouvee, Lumo utilise la date technique de modification.

Formats reconnus :

```md
## 2026-06-02
## 2026-06-02 - Point coordination
## 2026-06-02 10:25 - Incident ouvert
## 02.06.2026
## 02.06.2026 - Recette
## 02.06.2026 10:25 - Escalade support
```

Categories projet possibles :

```txt
projet
incident
suivi
documentation
demande
maintenance
```

Statuts projet possibles :

```txt
active
waiting
blocked
dormant
archived
```

Convention :

- `active` : projet en cours ou visible au quotidien.
- `waiting` : projet en attente d'une reponse ou d'un evenement externe.
- `blocked` : projet bloque par une decision, un acces, un incident ou une dependance.
- `dormant` : projet encore actif, mais sans mouvement recent. Il reste visible et modifiable.
- `archived` : projet termine ou retire du flux quotidien. Il est cache des vues actives.

Groupes de la vue Projet :

- `Importants` : tag `important`.
- `Bloques` : status blocked ou todo bloquante ouverte.
- `En attente` : status waiting ou todo en attente ouverte.
- `A venir` : tag `a-venir`.
- `Actifs` : projets avec actions ouvertes.
- `Sans action ouverte` : projets visibles sans todo ouverte.
- `Dormants` : status dormant ou activite Markdown trop ancienne.

## Journal

Convention de titre :

```txt
YYYY-MM-DD - sujet
```

Pour le classement dans la vue Journal, Lumo utilise d'abord la date au debut du titre.
Cela permet de creer aujourd'hui une note pour une seance future.

```yaml
---
type: journal
category: meeting
projects: [ISO20022, Audit]
tags: [decision, suivi]
personnes: [Daniel, Marie]
processed: false
---
```

Champs reconnus :

- `type` : vue principale. Pour le journal, utiliser `journal`.
- `category` : categorie de note.
- `projects` : projets lies.
- `tags` : tags libres.
- `personnes` : participants ou personnes concernees.
- `processed` : `true` si l'information a ete traitee, sinon `false`.

Categories journal possibles :

```txt
note
meeting
incident
worklog
decision
question
```

## processed

- `processed: false` = information encore a traiter, relire, extraire ou ranger.
- `processed: true` = information traitee.
- une note sans `processed` est consideree comme non traitee.

## Tags

Les tags sont libres.

Exemples :

```yaml
tags: [decision, TODO, vendor, a-relire]
```

Tags projet recommandes :

```txt
important
relance
decision
TODO
a-suivre
a-venir
a-archiver
fournisseur
prod
recette
audit
doc
```

Signification :

- `important` : projet a surveiller regulierement.
- `relance` : sujet avec suivis ou retours externes frequents.
- `decision` : decision prise ou attendue.
- `TODO` : document qui contient des actions a retrouver ou extraire.
- `a-suivre` : pas urgent, mais a garder visible.
- `a-venir` : projet discute, pas encore lance officiellement.
- `a-archiver` : projet presque termine, a ranger/exporter.
- `fournisseur` : dependance externe.
- `prod` : impact production.
- `recette` : validation ou tests.
- `audit` : conformite, controle, preuve.
- `doc` : documentation, runbook ou procedure.

Signaux visibles dans les listes projet :

- `important` affiche `!`
- `a-venir` affiche `a venir`
- `a-archiver` affiche `archive ?`

Usages possibles :

- retrouver les decisions
- marquer les notes a relire
- regrouper les incidents
- filtrer les notes par sujet transverse

## Bloc-notes temporaire

Le bloc-notes temporaire n'est pas une note journal.

Il sert a poser :

- idees rapides
- copier-coller en attente
- texte brut a transformer plus tard
- brouillons temporaires

Il est stocke dans le JSON exporte sous :

```txt
state.scratchpad.body
```
