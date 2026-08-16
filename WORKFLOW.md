# Workflow du Cockpit

Le Cockpit contient tout, mais ne montre le matin que ce qui merite ton
attention. La vue **Projets** explique sur quoi tu travailles ; la vue
**Taches** conserve toutes les actions sans devenir ta liste du jour. La regle
centrale est :

> La base stocke. La revue decide. Le Focus agit.

## Routine simple

### 1. Capturer sans classer

Un titre suffit. Chaque ligne de **Capture** devient une tache a clarifier et
revient dans la **Revue du matin**.

```text
Relancer Xavier sur KPI
Regarder le fichier Excel ATM
Idee amelioration bancomat
```

Les raccourcis restent facultatifs :

| Saisie | Effet |
| --- | --- |
| `focus` | Place directement dans le Focus du jour |
| `+[Nom du projet]` | Lie la tache au projet |
| `#tag` | Ajoute un tag |
| `@Daniel` ou `waiting:Daniel` | Indique la reponse attendue |
| `review:2026-08-20` | Programme la prochaine decision |
| `deadline:2026-08-31` | Definit une vraie date limite |
| `rappel:2026-08-20T09:00` | Cree une alerte exceptionnelle |

Les anciens raccourcis `next`, `revoir:` et `due:` restent compris lors de la
capture et des imports.

### 2. Faire la Revue du matin (5 a 10 minutes)

La revue affiche d'abord :

- les deadlines depassees, du jour et des trois prochains jours ;
- les dates de revue arrivees ;
- les captures et incoherences a clarifier ;
- les journaux passes encore a traiter ;
- les projets en cours sans prochaine action ;
- la prochaine action des projets en cours.

Le backlog d'un projet ne remonte pas ligne par ligne. Seule sa prochaine
action designee revient normalement. Une autre tache liee remonte quand meme si
elle porte sa propre deadline, une date de revue arrivee ou une incoherence.
Les taches autonomes sans urgence restent dans **Plus tard**, replie par defaut
et exclu du compteur rouge.

Pour chaque ligne, prendre une seule decision :

- `+` : mettre dans le Focus du jour ;
- `+j` : choisir quand revoir l'element ;
- `Wait` : noter qui doit repondre et quand verifier ;
- `OK` : terminer ;
- ouvrir le titre : ajouter du contexte ou modifier les proprietes.

Le bouton **Final Version** applique la methode de Mark Forster :

1. le premier element de la liste devient le repere marque ;
2. parcourir les elements suivants dans l'ordre ;
3. demander pour chacun : « Est-ce que je veux le faire avant le repere ? » ;
4. si oui, le marquer et l'utiliser comme nouveau repere ;
5. a la fin, travailler la chaine marquee dans l'ordre inverse.

La chaine devient le Focus du jour. Elle ne cree aucune liste independante et
n'ajoute aucune priorite permanente.

### 3. Choisir puis travailler dans Focus

Le **Focus** contient seulement les elements dont `focusDate` vaut la date du
jour. La selection d'hier disparait donc automatiquement sans modifier les
taches elles-memes.

Choisir en general 3 a 5 elements. **Final Version** aide a les comparer sans
leur attribuer une priorite permanente. Commencer ensuite par le premier
element du Focus et ne rouvrir la grande liste que pour capturer ou verifier un
contexte.

Un element en attente peut etre dans le Focus pour effectuer une relance tout
en restant `waiting`. Apres l'action, le terminer, le retirer du Focus ou lui
donner une nouvelle date de revue.

### 4. Fermer la journee (2 minutes, facultatif)

- noter les faits utiles dans le journal ou l'avancement du projet ;
- terminer les actions faites ;
- donner une date de revue aux attentes creees dans la journee ;
- verifier que chaque projet encore en cours possede une prochaine action.

Il n'est pas necessaire de preparer parfaitement demain : la revue du matin
reconstruit la selection.

## Les proprietes utiles

### Etats

| Type | Etats de travail |
| --- | --- |
| Tache | A faire, En attente, Terminee |
| Projet | En cours, En attente, Plus tard, Termine |
| Journal | A traiter, Traite |

`archived` sert a ranger un element termine hors du travail courant. Le Focus
n'est pas un etat.

### Date de revue

`reviewDate` repond a :

> Quand dois-je revoir cet element et decider quoi en faire ?

Elle sert aux relances, aux sujets temporairement silencieux et aux projets en
attente. Un element `waiting` doit avoir une personne/source et une date de
revue.

### Deadline

`deadline` repond a :

> Quand cet element doit-il reellement etre termine ?

Une deadline n'est jamais un simple rappel. Elle est coloree selon qu'elle est
future, proche, aujourd'hui ou depassee.

### Rappel imperatif

Le rappel avec date et heure est reserve aux oublis ayant une consequence
importante. Il affiche la bande **A ne pas oublier** et peut declencher une
notification. **Fait** acquitte le rappel sans terminer l'element.

## Projets et prochaines actions

Regle simple : si un resultat demande plusieurs actions, le creer comme
projet. Le projet nomme le resultat a obtenir ; ses taches decrivent les etapes.

Une tache reste toujours une page `task`, liee ou non a un projet. Elle ne doit
pas etre cachee dans le Markdown du projet.

Un projet **En cours** designe normalement une tache liee avec `nextActionId`.
Dans la revue, c'est cette action concrete qui est proposee. Quand elle est
terminee, le projet revient pour choisir la suivante. Toutes ses autres taches
restent visibles dans **Taches** et sous la croix du projet.

Un projet **En attente** indique de qui vient la reponse et porte une
`reviewDate`. Un projet **Plus tard** porte aussi une `reviewDate`. Ils restent
silencieux jusqu'a cette date et n'ont pas besoin d'une action active.

**Planifier la suite** depuis un projet permet :

- de fixer sa prochaine date de revue ;
- de le placer en attente avec une personne ;
- de creer et lier sa prochaine action.

La vue **Projets** est la vue portefeuille. Elle regroupe En cours, En attente
et Plus tard, montre le dernier avancement, la prochaine action, les dates et
la charge en nombre de taches. C'est la vue a ouvrir quand on te demande :
« Sur quoi travailles-tu ? »

Pour une seance avec ton chef, **Point avec mon chef** cree un journal organise
d'abord par projets en cours et en attente, puis par avancements, realisations,
taches hors projet et deadlines. Il reste editable avant la seance.

Le Markdown conserve le contexte, les decisions et l'avancement :

```markdown
# Info


# Contexte


# Avancement

## 15.08.2026 09:30

- Message envoye a Jean.


# Questions ouvertes


# Decisions
```

## Journal et Divers

La date d'un journal vient uniquement de son titre :

```text
2026-08-15 - Point avec Daniel
```

Un journal passe reste dans la revue tant qu'il est **A traiter**. Le passer a
**Traite** signifie que les taches et informations utiles ont ete extraites.
Il n'est pas archive automatiquement.

**Divers** garde les notes qui ne sont ni une tache, ni un projet, ni un
journal. Une note active peut recevoir une date de revue si elle doit revenir.

## Recherche

La recherche est globale des qu'un critere est saisi :

```text
rapport #bancomat -#chef
@Daniel status:waiting
project:"Release ATM" focus:true
deadline:next7d
(#ATM OR #bancomat) updated:last7d
```

Un espace signifie ET, `OR` accepte l'un des criteres et `-` exclut. Les
colonnes, leur largeur, leur ordre et le tri sont memorises par vue.

## Filet de securite

La vue **Controle** signale notamment :

- une attente sans personne ou sans date de revue ;
- un projet en cours sans prochaine action ;
- un projet Plus tard sans date de revue ;
- une tache ouverte rattachee a un projet termine ou introuvable ;
- une prochaine action absente, terminee ou liee au mauvais projet ;
- un journal sans date valide dans son titre.

Faire un export JSON regulier. L'export contient les pages, le Markdown, les
proprietes, les favoris et les preferences de colonnes. Les anciens champs
`next`, `dueDate`, `revoir` et les anciens statuts sont migres a l'import.
