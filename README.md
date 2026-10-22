# TP 1 — MVC vs MVVM : Todo-list avec filtre

**Parcours Dev · pas à pas · page 13**

Ce TP compare deux façons d'organiser une Todo-list : MVC et MVVM. Les deux versions
permettent d'ajouter, de cocher et de supprimer des tâches, de filtrer la liste et
d'afficher le nombre de tâches restantes.

## Lancer l'exemple MVVM

Prérequis : Node.js 20 ou plus récent.

Depuis la racine du dépôt, lancez les tests du modèle et du ViewModel :

```bash
node --test seance1-presentation/tp1-Steve_JIOKENG_WOTEU/TodoModel.test.js seance1-presentation/tp1-Steve_JIOKENG_WOTEU/TodoViewModel.test.js
```

Pour essayer l'interface, démarrez le serveur local depuis la racine :

```bash
npm run serve
```

Puis ouvrez `seance1-presentation/tp1-Steve_JIOKENG_WOTEU/index.html` avec le serveur local.

## Organisation de l'exemple MVVM

- `TodoModel.js` contient les tâches et les règles métier : ajout, bascule d'état,
  suppression, filtrage et calcul du nombre de tâches restantes.
- `TodoViewModel.js` porte l'état de présentation (`filtre`, `visibles`, `restantes`
  et `erreur`) et expose les actions destinées à la vue.
- `observable.js` fournit le mécanisme d'abonnement aux changements.
- `index.html` affiche l'état du ViewModel et lui transmet les actions de l'utilisateur.
- `TodoModel.test.js` et `TodoViewModel.test.js` vérifient les comportements sans
  navigateur.

## Bilan : où se trouve la logique du filtre ?

Dans les deux architectures, le calcul de la liste filtrée reste une règle du Model,
dans `filtrer()`.

- **MVC :** le Controller mémorise le filtre actif et coordonne le Model et la View.
  La View affiche le résultat que le Controller lui transmet.
- **MVVM :** le ViewModel porte l'état observable `filtre` et expose `visibles`.
  Quand le filtre ou les tâches changent, il recalcule la liste ; la vue s'abonne à
  cet état et n'implémente pas elle-même la logique du filtre.

## Qu'a-t-on testé sans navigateur ?

- **MVC :** le Model uniquement, notamment `filtrer()` et `restantes()`. Le Controller
  et la View dépendent du DOM et ne sont pas couverts par ces tests.
- **MVVM :** le Model et le ViewModel : ajout, rejet d'un titre vide, changement de
  filtre, message d'erreur et compteur. Les tests couvrent ainsi aussi l'état de
  présentation, sans lancer de navigateur.

## Fichiers concernés par l'ajout du filtre en MVC

Le filtre demande des changements dans quatre fichiers de code :

- `index.html` pour ajouter les commandes de filtre ;
- `TodoModel.js` pour calculer les tâches filtrées ;
- `TodoView.js` pour afficher le filtre et la liste résultante ;
- `TodoController.js` pour mémoriser le filtre courant et coordonner le rafraîchissement.

`TodoModel.test.js` est également mis à jour pour tester le filtrage : cela fait donc
cinq fichiers modifiés au total, dont quatre fichiers de code.

## Application au projet QueenBee

Pour QueenBee et l'affichage de mesures en temps réel, **MVVM est un choix adapté** :
les valeurs observables peuvent mettre l'écran à jour lorsqu'une mesure arrive, sans
rafraîchissement manuel. La logique qui prépare l'affichage reste dans le ViewModel
et peut être testée sans navigateur.
