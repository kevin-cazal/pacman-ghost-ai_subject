# Atelier 2 - Machine à états finis

**Bienvenue dans l'Atelier 2 !** Tu vas programmer le fantôme avec 3 modes (`patrol`, `follow`, `scared`) qui changent selon la situation.

**Prérequis :** termine l'**Atelier 1** d'abord (avec le fantôme qui poursuit Pac-Man).

**Objectif :** le fantôme a 3 modes - patrouille (`patrol`), poursuite (`follow`), peur (`scared`). Tu programmes d'abord la patrouille et la poursuite, puis la super pac-gomme. Les changements de mode vont dans `updateState`, le mouvement dans `chooseDirection`.

**Super pac-gomme :** grosses pac-gommes blanches aux 4 coins - elles effraient le fantôme 8 secondes (il devient bleu).

**Méthodologie :** lis l'étape, modifie le code, clique le panneau **Jeu**, puis **Démarrer**.

### Glossaire de l'atelier 2

| Terme | Signification |
| --- | --- |
| `updateState` | Fonction qui choisit le mode du fantôme : `patrol`, `follow` ou `scared` |
| `'patrol'` | Mode patrouille - Pac-Man est loin, le fantôme erre au hasard sans le chercher (orange) |
| `'follow'` | Mode poursuite - Pac-Man est proche, le fantôme utilise l'arbre de décision de l'Atelier 1 (rouge) |
| `'scared'` | Mode peur - Pac-Man a mangé une super pac-gomme, le fantôme fuit (bleu) |
| `super pac-gomme` | Grande pac-gomme blanche dans les 4 coins de la carte - effraie le fantôme 8 secondes |
| `game.scaredTimer` | Temps restant de la super pac-gomme (> 0 = peur active) |
| `state` | Mode actuel du fantôme - tu le recopies dans `buildInfos` : `state = ghost.state` |
| `currentDirection` | Dernière direction du fantôme - tu le recopies : `currentDirection = ghost.direction` |
| `patrolLockTimer` | Temps restant (secondes) avant un nouveau choix aléatoire - tu le recopies : `patrolLockTimer = ghost.patrolLockTimer` |
| `totalDistance` | Tu le calcules : `math.abs(distanceX) + math.abs(distanceY)` — nombre de cases entre fantôme et Pac-Man |

## Étape 1 - L'état et le mode patrol
<!-- ws: {type: exercise, id: a2-etat-patrol} -->

<!-- ws:resume -->
- `state = ghost.state` dans `buildInfos`
- `updateState` renvoie `'patrol'` pour l'instant
- ne pas toucher `chooseDirection`
<!-- /ws:resume -->

### Concept

Une **machine à états finis**, c'est un système qui :

1. Est toujours dans **un état** parmi une liste finie - ici : `patrol`, `follow`, `scared`
2. **Change d'état** quand un événement arrive - ex. Pac-Man s'approche, ou mange une super pac-gomme
3. **Se comporte différemment** selon l'état actuel

Dans ton code, deux fonctions travaillent ensemble :

- `updateState` : décide **quel est le prochain mode** à adopter
- `chooseDirection` : choisit **comment bouger** selon le mode (via `infos.state`)

Pour que `chooseDirection` puisse lire le mode, tu dois d'abord l'ajouter dans `infos` via `buildInfos`. Pour cette étape, le fantôme reste en mode `'patrol'` par défaut - c'est le mode quand Pac-Man est loin.

### Observe le jeu

Le fantôme se comporte encore comme à la fin de l'Atelier 1 - il poursuit Pac-Man. C'est normal : tu n'as pas encore codé le mouvement par mode. À l'étape 2, tu ajouteras la patrouille.

### À toi de jouer

Dans `buildInfos`, ajoute :

```lua
state = ghost.state,
```

Dans `updateState`, laisse :

```lua
return 'patrol'
```

Ne modifie pas `chooseDirection`.

### Vérifie

Le fantôme poursuit encore Pac-Man (Atelier 1). Pas d'erreur sous l'éditeur.

### Réflexion

Pourquoi recopier `ghost.state` dans `infos` ? À quoi servira `infos.state` dans `chooseDirection` ?

## Étape 2 - Patrouiller
<!-- ws: {type: exercise, id: a2-patrouiller} -->

<!-- ws:resume -->
- `currentDirection` et `patrolLockTimer` dans `buildInfos`
- bloc `if infos.state == 'patrol' then` au début de `chooseDirection`
- garder la direction tant que le timer tourne, sinon tirer au hasard parmi les directions libres
<!-- /ws:resume -->

### Concept

Quand Pac-Man est **loin**, le fantôme est en mode `'patrol'` : il **erre** sans le chercher, en choisissant des directions **au hasard**.

Pour éviter un mouvement saccadé (changement à chaque case), le fantôme **garde sa direction pendant un moment** grâce à `patrolLockTimer` :

```lua
currentDirection = ghost.direction,
patrolLockTimer = ghost.patrolLockTimer,
```

`ghost.direction` vaut `'left'`, `'right'`, `'up'`, `'down'` ou `nil` (immobile).
`patrolLockTimer` compte les secondes restantes avant de pouvoir choisir une nouvelle direction au hasard.

**Règles de patrouille :**

1. Si `patrolLockTimer > 0` et `currentDirection` est libre : continue dans cette direction
2. Sinon (timer fini ou mur devant) : choisis une direction **au hasard** parmi les directions valides

**Astuce : tirer un élément aléatoire dans une liste** :

Tu choisis un dessert au hasard dans un menu. Tu commences avec une **liste vide** `{}`, tu n'y mets que les desserts **encore disponibles** `if ... then table.insert(desserts, ...) end`, puis tu tires un **numéro au hasard** pour en sélectionner un (index **1**, **2**, **3**... selon la taille de la liste — en Lua, les tables commencent à **1**).

```lua
local desserts = {}
if glaceDispo then table.insert(desserts, 'glace') end
if gateauDispo then table.insert(desserts, 'gâteau') end
if fruitDispo then table.insert(desserts, 'fruit') end
if crepeDispo then table.insert(desserts, 'crêpe') end
local index = math.random(1, #desserts)
return desserts[index]
```

Imaginons qu'il n'y ait plus de gâteaux : la liste vaut `{'glace', 'fruit', 'crêpe'}` — **3** desserts, donc des index **1**, **2** ou **3**.

**Comment l'index aléatoire est calculé :**

1. `math.random(1, n)` tire un entier aléatoire entre **1** et **n** inclus (ex. `2` pour une liste de 3 desserts)
2. `desserts[2]` renvoie le deuxième élément : `'fruit'`

Autre tirage possible : `math.random(1, 3)` donne **1**, puis `'glace'`.
Si la liste est vide (`#desserts == 0`), ne fais pas ce calcul — il n'y a rien à choisir.

Adapte cette idée aux directions du fantôme.

### Observe le jeu

Sans patrouille, le fantôme **poursuit toujours** Pac-Man (Atelier 1) même quand il est loin. Après cette étape, il doit errer de façon imprévisible quand tu es loin.

### À toi de jouer

Dans `buildInfos`, ajoute :

```lua
currentDirection = ghost.direction,
patrolLockTimer = ghost.patrolLockTimer,
```

Dans `chooseDirection`, ajoute **au début** (avant tes règles de poursuite) un bloc `if infos.state == 'patrol' then` :

1. Si `patrolLockTimer > 0`, continue `currentDirection` tant que la case devant est libre (même principe que quatre `if` avec `canGoLeft`, `canGoRight`, etc.)
2. Sinon, choisis une direction aléatoire parmi celles possibles

### Vérifie

Loin de Pac-Man, le fantôme erre (orange) au lieu de poursuivre.

### Réflexion

Pourquoi utilise-t-on un timer pour garder la direction au lieu de choisir au hasard à chaque carrefour ?

## Étape 3 - Mode follow
<!-- ws: {type: exercise, id: a2-mode-follow} -->

<!-- ws:resume -->
- une propriété de distance en cases dans `buildInfos`
- `updateState` : `'follow'` si Pac-Man est à 8 cases ou moins, sinon `'patrol'`
- envelopper l'arbre de l'Atelier 1 dans un test sur `infos.state`
<!-- /ws:resume -->

### Concept

Quand Pac-Man est **proche**, le fantôme passe en `'follow'` et utilise ton arbre de décision de l'Atelier 1. Loin, il reste en `'patrol'`.

Mesure la distance en **cases** (horizontal + vertical, pas en diagonale).

La poursuite Atelier 1 ne doit s'appliquer **que** en mode `'follow'`.

### Observe le jeu

Sans le mode `follow`, le fantôme patrouille même quand tu t'approches.

### À toi de jouer

1. `buildInfos` — ajoute une propriété pour la distance en cases entre fantôme et Pac-Man
2. `updateState` — si Pac-Man est proche (<= 8 cases), `'follow'`, sinon `'patrol'`
3. `chooseDirection` — enveloppe tout l'arbre de poursuite Atelier 1 dans un test sur `infos.state`

### Vérifie

- Pac-Man loin : `patrol` (orange)
- Pac-Man proche : `follow` (rouge, poursuite)

### Réflexion

Pourquoi la poursuite ne doit-elle marcher qu'en mode `follow` et pas en `patrol` ?

## Étape 4 - La super pac-gomme et le mode scared
<!-- ws: {type: exercise, id: a2-super-pac-gomme} -->

<!-- ws:resume -->
- `updateState` teste les 3 modes dans l'ordre `scared`, puis `follow`, puis `patrol`
- `game.scaredTimer > 0` déclenche `scared`
- ne pas toucher `chooseDirection`
<!-- /ws:resume -->

### Concept

Quand Pac-Man mange une **super pac-gomme** (grosse pac-gomme blanche), le fantôme a peur 8 secondes : `game.scaredTimer > 0`, mode `'scared'`.

**Astuce (priorités)** : on teste d'abord l'urgence, puis le reste.

```lua
if urgence then return 'rouge' end
if proche then return 'orange' end
return 'vert'
```

Adapte à `scared` > `follow` > `patrol`.

### Observe le jeu

Après une super pac-gomme, le fantôme devient **bleu** mais patrouille ou poursuit encore — la fuite viendra à l'étape 5.

### Astuce

Les petites pac-gommes orange ne comptent pas — cherche les **grosses pac-gommes blanches** aux 4 coins.

### À toi de jouer

1. `updateState` — 3 modes avec priorité : `scared` avant `follow` avant `patrol`
2. Ne modifie pas `chooseDirection`

### Vérifie

1. Sans super pac-gomme : loin : `patrol`, proche : `follow`
2. Super pac-gomme : fantôme **bleu** (`scared`)
3. Fin du timer : retour `patrol` ou `follow`
4. Approche puis éloignement : `follow` puis `patrol`

### Réflexion

Pourquoi tester `scared` avant `follow` dans `updateState` ?

## Étape 5 - Fuir en mode scared
<!-- ws: {type: exercise, id: a2-fuir-scared} -->

<!-- ws:resume -->
- bloc `scared` dans `chooseDirection`, entre `patrol` et `follow`
- règles inverses de la poursuite, sans oublier les `canGo...`
<!-- /ws:resume -->

### Concept

En mode `'scared'`, le fantôme **fuit** Pac-Man — direction **opposée** à la poursuite.

### Observe le jeu

Après une super pac-gomme, le fantôme est bleu mais patrouille ou poursuit encore. Il doit maintenant **s'éloigner** de toi.

### À toi de jouer

Dans `chooseDirection`, ajoute un bloc `scared` **entre** `patrol` et `follow`. Écris les règles de fuite (inverse de la poursuite, vérifie `canGo...`).

### Vérifie

Après super pac-gomme, fantôme bleu s'éloigne sans traverser les murs.

### Réflexion

Fuite = directions inverses par rapport à la poursuite ?

## Étape 6 - Défi : fantôme complet
<!-- ws: {type: exercise, id: a2-fantome-complet} -->

<!-- ws:resume -->
- relire et tester les étapes 1 à 5 ensemble
- une partie complète avec les 3 modes visibles
<!-- /ws:resume -->

### Concept

Relis et teste ton code des étapes 1 à 5.

### Vérifie

Joue une partie complète et vérifie que ton programme se comporte correctement :

- **patrol** : fantôme orange qui erre au hasard quand tu es loin (direction gardée ~1,5 s)
- **follow** : poursuite active (rouge) quand tu t'approches
- **scared** : fantôme bleu qui fuit après une super pac-gomme
- Super pac-gomme : grosses pac-gommes blanches aux 4 coins
- En mode scared, tu peux croiser le fantôme sans mourir
- En mode patrol ou follow, toucher le fantôme = mort (redémarrage après 3 s)
- Pas de murs traversés
- Victoire = **toutes** les pac-gommes mangées

### Réflexion

Comment rendrais-tu la patrouille plus imprévisible sans ajouter un 4e mode ?

## Fin de l'atelier 2

Bravo, tu es arrivé au bout. Ton fantôme sait patrouiller, poursuivre et prendre peur.

Tu as utilisé deux façons de programmer un comportement : un arbre de décision, puis une machine
à états. Ces deux idées servent bien au-delà de Pac-Man.
