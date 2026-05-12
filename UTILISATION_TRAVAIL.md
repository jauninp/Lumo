# TodoTxtJs - utilisation au travail

## Statuts

Les statuts sont des metadata todo.txt. Tu les ajoutes directement dans la tache :

```text
(A) Appeler le client +Projet @bureau due:2026-05-12 status:next
```

- `status:next` : prochaine action claire, faisable maintenant.
- `status:waiting` : en attente de quelqu'un ou de quelque chose.
- `status:blocked` : impossible d'avancer tant qu'un blocage n'est pas leve.

Pour une tache en attente, ajoute souvent une date de relance :

```text
Relancer Sophie pour le devis +Client @bureau status:waiting wait:2026-05-15
```

## Dates utiles

- `due:YYYY-MM-DD` : echeance.
- `start:YYYY-MM-DD` : ne pas traiter avant cette date.
- `wait:YYYY-MM-DD` : date de relance.
- `today:YYYY-MM-DD` : tache choisie pour le plan du jour.
- `note:...` : contexte encode dans la ligne todo.txt.

## Routine ZTD simple

1. Collecter : ajoute tout dans la zone de saisie, sans trop reflechir.
2. Clarifier : ajoute `+Projet`, `@contexte`, `status:` et une date si necessaire.
3. Planifier : utilise le bouton `Aujourd'hui` pour choisir peu de taches pour la journee.
4. Faire : commence par `status:next`.
5. Relancer : utilise `Demain` sur une tache en attente ; cela ajoute `status:waiting wait:YYYY-MM-DD`.

## Filtres utiles

Les filtres se cumulent. Tu peux par exemple cliquer `+Client`, `@bureau` et `status:next`, puis ajouter `is:week` dans la zone de recherche.

- `is:today` : taches avec `due:`, `start:` ou `wait:` aujourd'hui.
- `is:followup` : relances dues ou depassees.
- `is:overdue` : taches ouvertes en retard.
- `is:week` : echeances dans les 7 jours.
- `is:available` : taches ouvertes qui ne sont pas bloquees par `start:` ou `wait:` dans le futur.
- `status:next` : prochaines actions.
- `status:waiting` : taches en attente.
- `status:blocked` : taches bloquees.
- `has:due` : taches avec echeance.
- `no:project` : taches sans projet.

## Sauvegarde conseillee

L'application sauvegarde dans le navigateur et garde un historique local recent. Pour eviter toute perte en environnement verrouille :

1. Clique regulierement sur `Backup` dans l'application.
2. Garde les fichiers `todo-backup-*.txt` dans un dossier sauvegarde par ton entreprise.

