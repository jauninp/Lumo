# Format Markdown / YAML Worklog

Ce fichier documente les conventions YAML/frontmatter utilisees par Worklog.

## Principes

- Le Markdown garde le contexte humain.
- Le frontmatter YAML garde les metadonnees lisibles par l'application.
- Le champ officiel pour classer un document est `type`.
- Ne pas utiliser `kind`.
- Les tags servent a retrouver, filtrer et regrouper les informations.

## Projet

```yaml
---
type: incident
status: active
tags: [pacs, fournisseur]
summary: Incident PACS.002 en cours.
---
```

Champs reconnus :

- `type` : categorie du projet.
- `status` : etat du projet.
- `tags` : tags libres.
- `summary` : resume court affiche dans les listes.

Types projet possibles :

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

## Journal

Convention de titre :

```txt
YYYY-MM-DD - sujet
```

Pour le classement dans la vue Journal, Worklog utilise d'abord la date au debut du titre.
Cela permet de creer aujourd'hui une note pour une seance future.

```yaml
---
type: meeting
projects: [ISO20022, Audit]
tags: [chef, decision]
processed: false
---
```

Champs reconnus :

- `type` : categorie de note.
- `projects` : projets lies.
- `tags` : tags libres.
- `processed` : `true` si l'information a ete traitee, sinon `false`.

Types journal possibles :

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
tags: [chef, decision, vendor, a-relire]
```

Tags projet recommandes :

```txt
important
chef
relance
decision
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
- `chef` : sujet souvent discute avec le chef.
- `relance` : sujet avec suivis ou retours externes frequents.
- `decision` : decision prise ou attendue.
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
- `chef` affiche `chef`
- `a-archiver` affiche `archive ?`

Usages possibles :

- retrouver les decisions
- retrouver les discussions avec le chef
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
