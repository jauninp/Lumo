# Format todo.txt Lumo

Ce fichier documente les conventions todo.txt utilisees par Lumo.
Il sert de reference produit et pourra alimenter une aide integree plus tard.

## Principes

- Une todo reste une ligne texte lisible.
- Le format reste proche de todo.txt.
- Les informations structurees utilisent des tokens courts.
- Une todo peut etre liee a un projet, a plusieurs projets, ou a aucun projet.
- Les dates sont au format ISO `YYYY-MM-DD`.
- Les metadonnees libres utilisent `key:value`.

## Structure standard

```txt
(A) 2026-05-27 Relancer Pierre +ISO20022 @mail followup:2026-05-30 waiting:Pierre id:5RKH02
```

Ordre recommande :

```txt
[x DATE_FIN] [(A)] [DATE_CREATION] texte +Projet @contexte due:DATE followup:DATE waiting:QUI blocked:RAISON status:STATUT id:ID
```

L'ordre des tokens reste souple, sauf pour les elements todo.txt standards :

- priorite `(A)` avant la date de creation
- date de creation au debut de la tache, apres la priorite si elle existe
- tache terminee avec `x DATE_FIN DATE_CREATION`

## Priorite

```txt
(A) Tache critique
(B) Tache importante
(C) Tache normale mais visible
```

Conventions :

- `(A)` = priorite haute
- `(B)` = important
- `(C)` et suivants = priorite plus faible
- pas de priorite = action normale

## Dates todo.txt

### Date de creation

```txt
2026-05-27 Appeler Jean
(A) 2026-05-27 Relancer Pierre
```

Regles :

- utiliser une date ISO au debut de la tache
- ne pas utiliser `created:YYYY-MM-DD`
- avec priorite, la date vient apres `(A)`

### Tache terminee

```txt
x 2026-05-28 2026-05-27 Envoyer rapport +Audit
```

Regles :

- `x` indique que la tache est terminee
- la premiere date est la date de completion
- la deuxieme date est la date de creation

## Projet

```txt
+ISO20022
+Audit
+IncidentVPN
```

Conventions :

- un projet commence par `+`
- une todo peut avoir plusieurs projets
- une todo peut aussi rester solitaire, sans projet
- si un projet inconnu apparait, Lumo peut creer le projet

Exemple :

```txt
Preparer point transverse +ISO20022 +Audit
```

## Contexte

```txt
@mail
@tel
@bureau
@vendor
```

Conventions :

- un contexte commence par `@`
- le contexte indique le canal, le lieu ou le mode d'action
- une todo peut avoir plusieurs contextes

## Recherche dans la vue Todo

La recherche de la vue Todo accepte une syntaxe proche d'Obsidian.

Exemples :

```txt
status:waiting
-status:blocked
is:alert
is:overdue
-is:done
+ISO20022 @bureau
"texte exact" -vendor
```

Regles :

- un mot simple cherche dans toute la ligne todo.txt
- les termes entre guillemets cherchent une phrase exacte
- `-terme` exclut les todos qui correspondent au terme
- `+Projet` filtre un projet
- `@contexte` filtre un contexte
- `project:Projet` filtre aussi un projet
- `context:contexte` filtre aussi un contexte
- `status:waiting` filtre le statut calcule
- `waiting:Pierre` filtre la valeur `waiting`
- `blocked:raison` filtre la valeur `blocked`
- `due:2026-05-30`, `followup:2026-05-30`, `id:ABC123` filtrent les metadonnees
- `next:ABC123` permet de pointer vers une autre todo par son `id:`
- `priority:A` filtre la priorite
- `text:mot` cherche seulement dans le texte non structure

Filtres `is:` disponibles :

```txt
is:open
is:done
is:alert
is:overdue
is:waiting
is:blocked
is:solo
is:priority
```

- `is:alert` = `due:` ou `followup:` aujourd'hui ou en retard.

## Kanban

La vue Kanban lit et modifie directement les lignes todo.txt.
La configuration de base est dans le JSON `config.kanban`.

Exemple :

```json
{
  "field": "status",
  "columns": [
    { "id": "none", "title": "Sans statut", "value": "" },
    { "id": "next", "title": "Next", "value": "next" },
    { "id": "waiting", "title": "Waiting", "value": "waiting" },
    { "id": "blocked", "title": "Blocked", "value": "blocked" },
    { "id": "done", "title": "Done", "value": "done" }
  ]
}
```

Champs Kanban supportes au depart :

```txt
status
project
context
priority
```

Regles :

- `field: "status"` lit/ecrit `status:...`.
- `value: "done"` marque la todo terminee.
- `value: ""` represente l'absence de valeur pour le champ.
- le drag-drop garde todo.txt comme source de verite.

## Liens entre todos

```txt
next:ABC123
```

Signification :

- pointe vers la todo principale via son `id:`
- utile pour indiquer une action secondaire, une suite ou une dependance
- le bouton `>` apparait sur la todo principale et affiche ses taches secondaires recursives
- seules les taches ouvertes participent a cette relation dans la vue Todo
- le clic sur le token `next:ABC123` filtre la vue Todo sur `id:ABC123`
- la relation reste dans la ligne todo.txt, sans donnee cachee dans le JSON

Exemple :

```txt
Attendre retour validation +Audit waiting:Chef followup:2026-06-05 next:ABC123 id:WAIT01
Preparer etape suivante +Audit id:ABC123
```

## Echeances et relances

### due

```txt
due:2026-05-30
```

Signification :

- echeance dure de la tache
- si la date est depassee, la todo est en retard
- a utiliser quand il y a une vraie deadline

Exemple :

```txt
(A) Envoyer rapport +Audit due:2026-05-30
```

### followup

```txt
followup:2026-05-30
```

Signification :

- date de relance
- date a laquelle la tache doit ressortir
- utile pour les attentes, les mails envoyes, les retours fournisseur

Exemple :

```txt
Relancer Pierre pour document +ISO20022 @mail waiting:Pierre followup:2026-05-30
```

Difference :

- `due:` = la tache doit etre terminee pour cette date
- `followup:` = il faut y repenser ou relancer a cette date

## Statuts

### Action normale

```txt
Verifier mapping +ISO20022
```

Signification :

- pas de `status:`
- pas de `waiting:`
- pas de `blocked:`
- action ouverte normale

### status:waiting

```txt
status:waiting
```

Signification :

- action en attente
- forme generique acceptee
- a utiliser seulement si aucune personne/source n'est encore precisee
- preferer `waiting:NomOuSource` des que possible

Exemple :

```txt
Attendre validation +Audit status:waiting followup:2026-05-31
```

### waiting

```txt
waiting:Pierre
waiting:Chef
waiting:Vendor
waiting:SupportIT
```

Signification :

- attente d'une personne, equipe, fournisseur ou systeme
- forme recommandee pour les attentes
- implique un etat waiting dans l'UI
- il n'est pas necessaire d'ajouter aussi `status:waiting`

Exemple recommande :

```txt
Recevoir retour XML +ISO20022 waiting:Daniel followup:2026-05-30
```

### status:blocked

```txt
status:blocked
```

Signification :

- action bloquee
- forme generique acceptee
- a utiliser seulement si aucune raison/source n'est encore precisee
- preferer `blocked:RaisonOuSource` des que possible

Exemple :

```txt
Continuer recette +Migration status:blocked followup:2026-06-01
```

### blocked

```txt
blocked:VPN
blocked:Acces
blocked:DecisionChef
blocked:Vendor
```

Signification :

- blocage par une personne, equipe, fournisseur, acces, decision ou systeme
- forme recommandee pour les blocages
- implique un etat blocked dans l'UI
- il n'est pas necessaire d'ajouter aussi `status:blocked`

Exemple recommande :

```txt
Tester recette +Migration blocked:AccesVPN followup:2026-05-29
```

## Valeurs status possibles

Valeurs standardisees :

```txt
status:next
status:waiting
status:blocked
status:someday
status:cancelled
```

Sens :

- `status:next` = prochaine action concrete
- `status:waiting` = en attente generique, preferer `waiting:...`
- `status:blocked` = bloque generique, preferer `blocked:...`
- `status:someday` = a garder, pas actif maintenant
- `status:cancelled` = abandonne sans le marquer comme termine

Valeurs a eviter pour l'instant :

- `status:todo`, car l'absence de statut suffit
- `status:done`, car todo.txt utilise `x`
- `status:open`, car l'absence de `x` suffit

## Identifiant

```txt
id:5RKH02
```

Conventions :

- identifiant court genere par Lumo
- reste une metadonnee `key:value`
- permet de garder une todo stable quand le texte change
- permet de detecter les doublons

## Autres metadonnees libres

Lumo accepte les tokens `key:value`.

Possibilites utiles :

```txt
ticket:INC12345
ref:CHG0042
url:https://example.local/ticket/123
owner:Pierre
area:payments
```

Regles :

- garder les cles courtes
- eviter les espaces dans la valeur
- preferer `CamelCase`, tirets ou underscores si necessaire

## Liens

```txt
https://example.local/ticket/123
http://intranet.local/page
```

Conventions :

- les liens `http://` et `https://` sont cliquables dans l'UI
- cliquer sur un lien ne doit pas appliquer de filtre todo
- une URL peut aussi etre stockee comme metadonnee, par exemple `url:https://...`

## Todo solitaire

```txt
Appeler service RH @tel followup:2026-05-30
```

Signification :

- todo sans `+Projet`
- utile pour les actions administratives, rapides ou non rattachees
- doit rester visible dans les vues Inbox/Aujourd'hui

## Todo multi-projets

```txt
Preparer point de coordination +ISO20022 +Audit @bureau due:2026-06-02
```

Signification :

- la todo apparait dans chaque projet lie
- utile pour les sujets transverses

## Exemples complets

### Relance fournisseur

```txt
(A) 2026-05-27 Relancer fournisseur pour exemple XML +ISO20022 @mail waiting:Vendor followup:2026-05-30 id:5RKH02
```

### Blocage acces

```txt
2026-05-27 Tester recette +Migration @bureau blocked:AccesVPN followup:2026-05-29 id:5RKH03
```

### Echeance dure

```txt
(A) 2026-05-27 Envoyer rapport final +Audit due:2026-05-31 id:5RKH04
```

### Todo terminee

```txt
x 2026-05-28 2026-05-27 Envoyer compte-rendu +ISO20022 @mail id:5RKH05
```

### Action solitaire

```txt
2026-05-27 Appeler RH @tel followup:2026-05-30 id:5RKH06
```

## Interpretation UI cible

- `due:` depasse = todo en retard
- `followup:` aujourd'hui ou depasse = relance active
- `waiting:` = attente nommee, forme recommandee
- `blocked:` = blocage nomme, forme recommandee
- `status:waiting` sans `waiting:` = attente non nommee / generique
- `status:blocked` sans `blocked:` = blocage non nomme / generique
- todo sans projet = solitaire / inbox
- todo avec plusieurs projets = visible dans chaque projet
- todo terminee `x` = cachee des vues ouvertes par defaut
