# Partie 2 : Machine à états finis

## Mise en contexte
<!-- ws: {type: prose} -->

Ton fantôme poursuit **tout le temps**, même à l'autre bout de la carte : impossible de le semer
une seconde. Tu vas lui donner trois humeurs.

| `patrol` | `follow` | `scared` |
| --- | --- | --- |
| ![Fantôme orange](img/etat-patrol.png) | ![Fantôme rouge](img/etat-follow.png) | ![Fantôme bleu](img/etat-scared.png) |
| Tu es loin : il erre au hasard. | Tu es proche : il utilise ton arbre de la partie 1. | Tu as mangé une super pac-gomme : il fuit. |

**La couleur du fantôme te dit dans quel mode il est.** C'est comme ça que tu vérifieras ton code
à chaque étape.

> 🛟 **Coincé ?** Les termes propres à cette partie sont dans le glossaire juste en dessous.

<details><summary>Glossaire de la partie 2</summary>

| Terme | Signification |
| --- | --- |
| `state` | La variable où tu ranges le mode : `'patrol'`, `'follow'` ou `'scared'`. Le jeu la lit après chaque appel de `ghost` et colore le fantôme |
| `'patrol'` | Le fantôme erre au hasard (orange) |
| `'follow'` | Le fantôme utilise l'arbre de décision de la partie 1 (rouge) |
| `'scared'` | Le fantôme fuit (bleu) |
| super pac-gomme | Grande pac-gomme blanche dans les 4 coins, effraie le fantôme 8 secondes |
| `game.scaredTimer` | Temps restant de la super pac-gomme (supérieur à 0 = peur active) |
| `totalDistance` | Tu le calcules : le nombre de cases entre le fantôme et Pac-Man |
| `me.direction` | La direction que le fantôme suit en ce moment, `nil` s'il est arrêté. Sert au bonus **Tenir sa direction** |
| `compteur` | Une variable à toi qui compte les cases depuis le dernier virage. Sert au bonus **Tenir sa direction** |

</details>

## Étape 0 : L'état du fantôme
<!-- ws: {type: exercise, id: a2-etat-patrol} -->

Une **machine à états finis** est dans un seul mode à la fois, et elle en change quand un événement arrive.

![Les trois modes du fantôme, et ce qui le fait passer de l'un à l'autre.](img/machine-etats.png)

Ce diagramme est le plan de toute la partie. Le mode est une variable, `state`, que tu crées **en haut de ton fichier, en dehors de `ghost`**. Les **flèches** sont les règles qui changent `state` ; le reste de `ghost` décide comment bouger **une fois dans un mode**.

### 🥸 Mise en application

**Ton objectif :** créer la variable `state`, et vérifier que le jeu la lit.

```lua
-- tout en haut du fichier, avant function ghost()
state = 'patrol'
```

C'est son humeur de **départ** : tant que tu n'as pas écrit de flèche, il gardera celle-là toute la partie.

Pour vérifier, remplace `'patrol'` par `'follow'` et relance : le fantôme devient **rouge**. Remets `'patrol'`.

![Il poursuit comme à la fin de la partie 1, et il est orange, désormais cette couleur veut dire quelque chose.](img/a2-e1-orange.gif)

## Étape 1 : Prendre peur et fuir
<!-- ws: {type: exercise, id: a2-peur-fuite, validation: quiz} -->

Les quatre grosses pac-gommes blanches, dans les coins, sont des **super pac-gommes**. Quand tu en manges une, `game.scaredTimer` passe à 8 et redescend seconde après seconde. Au-dessus de 0, le fantôme a peur.

Ce que tu vas ajouter dans `ghost` :
- Passer en mode `scared`
- Sortir du mode `scared`
- Comment se déplacer quand le fantôme est en mode `scared`

<!-- ws:toolbox -->
### Boîte à outils

> 🧰 **Outil #1 : `==` demande « est-ce exactement ça ? »**
> Un seul `=` stocke une valeur ; deux `==` posent une question.
> ▶️ **Essaye dans la Console :**
> ```lua
> motDePasse = 'secr3t'
> if motDePasse == 'secr3t' then
>   print('Accès autorisé')
> else
>   print('Mot de passe incorrect')
> end
> ```
> 📘 [Les opérateurs relationnels](https://www.lua.org/manual/5.3/manual.html#3.4.4)
<!-- /ws:toolbox -->

### 🥸 Mise en application

**Ton objectif :** un fantôme qui devient bleu quand tu manges une grosse pac-gomme blanche, qui s'écarte au lieu de te courir après, et que tu peux attraper sans mourir.

Poses-toi les bonnes questions pour t'aider à coder cette étape :
- Dans quelle condition le fantôme passe en mode `scared` ?
- Dans quelle condition il en sort ?
- Dans la partie 1, tu as un fantôme qui essaye de se rapprocher de Pac-Man, comment faire pour dire au fantôme de s'en éloigner ?
- Dans la partie 1, tu as vu que l'ordre dans lequel ton code est exécuté a son importance, que doit faire le fantôme en priorité ?

Tu dois obtenir ceci :

![Le fantôme orange remonte vers Pac-Man, comme à l'étape précédente. Pac-Man mange la super pac-gomme du coin : le fantôme vire au bleu et s'écarte.](img/a2-e2-peur-fuite.gif)

<!-- ws: {type: hint} -->
<details><summary>Si tu es bloqué</summary>

- **Coupe l'étape en deux, teste au milieu.**
- Tu peux utiliser `print(game.scaredTimer)` ou `print(state)` au début de `ghost` et regarder le panneau **Console** pour t'aider à comprendre ce que fait ton code.
- Essaye de bien te rappeler ce que signifie :
  - `distanceX > 0` et `distanceX < 0` : voir l'étape 3 de la partie 1
  - `distanceY > 0` et `distanceY < 0` : voir l'étape 5 de la partie 1

</details>

Le fantôme ne bouge plus quand il a peur ? Il est peut-être acculé : toutes les directions libres le rapprochent de toi, donc aucune de tes règles ne s'applique :

![Pac-Man bouge, le fantôme bleu non : ses deux seules cases libres le rapprocheraient de toi.](img/a2-e5-accule.gif)

<!-- ws: {type: quiz, id: a2-peur-fuite-1, kind: single} -->
> Dans quelle condition le fantôme passe en mode `scared` ?
- A. Quand `game.scaredTimer > 0`
- B. Quand `game.scaredTimer == 0`
- C. Quand `game.scaredTimer < 0`
- D. Quand Pac-Man s'approche de lui

<!-- ws: {type: quiz, id: a2-peur-fuite-2, kind: single} -->
> Lorsque le fantôme a peur et Pac-Man est à droite, où doit aller le fantôme ?
- A. En bas
- B. À droite
- C. À gauche
- D. En haut

<!-- ws: {type: quiz, id: a2-peur-fuite-3, kind: single} -->
> Dans quel ordre doit s'exécuter ton code pour que ton fantôme ait le bon comportement ?
- A. Vérifier si le fantôme a peur. S'il a peur : fuir, sinon : se rapprocher.
- B. Se rapprocher. Vérifier si le fantôme a peur, s'il a peur : fuir.
- C. L'ordre d'exécution du code n'a pas d'importance

## Étape 2 : Errer au hasard
<!-- ws: {type: exercise, id: a2-patrouiller} -->

En `'patrol'`, le fantôme ne te cherche pas. À chaque case, il regarde les directions libres et en tire une au hasard.

Ça donne un fantôme qui **ne va nulle part en particulier** : il tourne autour de son point de départ au lieu de traverser la carte.

<!-- ws:toolbox -->
### Boîte à outils

> 🧰 **Outil #1 : construire une liste petit à petit**
> `{}` crée une liste vide, `table.insert` y ajoute un élément à la fin.
> ▶️ **Essaye dans la Console :**
> ```lua
> glaceDispo = true
> gateauDispo = false
> cookieDispo = true
> desserts = {}
> if glaceDispo then table.insert(desserts, 'glace') end
> if gateauDispo then table.insert(desserts, 'gâteau') end
> if cookieDispo then table.insert(desserts, 'cookie') end
> ```
> Tape ensuite `desserts[1]` dans la **Console** : tu lis `glace`.
> 📘 [table.insert](https://www.lua.org/manual/5.3/manual.html#pdf-table.insert)

> 🧰 **Outil #2 : tirer au hasard dans une liste**
> `#liste` donne le nombre d'éléments, `math.random(1, n)` tire un entier entre 1 et n inclus.
> À taper juste après l'outil #1 : il se sert du `desserts` que tu viens de construire.
> ▶️ **Essaye dans la Console :**
> ```lua
> index = math.random(1, #desserts)
> return desserts[index]
> ```
> **En Lua, les listes commencent à 1**, pas à 0.
> 📘 [math.random](https://www.lua.org/manual/5.3/manual.html#pdf-math.random) · [l'opérateur #](https://www.lua.org/manual/5.3/manual.html#3.4.7)
<!-- /ws:toolbox -->

### 🥸 Mise en application

**Ton objectif :** un fantôme qui se déplace de manière aléatoire.

Un bloc `if state == 'patrol' then`, entre le bloc `scared` que tu viens d'écrire et tes règles de poursuite. Dedans :

1. construis la liste des directions libres, en te servant des `canGo...` que tu as depuis la partie 1
   - Ta liste est une variable, tu peux l'appeler `possibleDirections`
2. tires-en une au hasard, et renvoie-la avec `return`

Tu dois obtenir ceci : il erre sans traverser un seul mur, et il ne te poursuit plus du tout, même collé à toi. C'est voulu et c'est temporaire : la poursuite revient à l'étape 3, en mieux.

![Le fantôme orange tire une direction au hasard à chaque case : il tourne autour de son point de départ sans jamais s'éloigner vraiment.](img/a2-e2-hasard.gif)

<!-- ws: {type: hint} -->
<details><summary>Si tu es bloqué</summary>

- **Ta liste se construit-elle ?** `print(#possibleDirections)` juste avant le tirage, et regarde le panneau **Console**, sous l'éditeur. Si le nombre reste à 0, le problème est dans la construction de ta liste, pas dans le tirage. (Et tu risques d'avoir une erreur qui s'affiche.)
- **Il te poursuit encore, comme avant ?** Alors ton bloc `patrol` n'est jamais atteint : vérifie que `state = 'patrol'` est bien en haut de ton fichier (étape 0), et que ton bloc s'écrit `state == 'patrol'`, avec deux `=`.
- **Il traverse les murs ?** Tu as mis les quatre directions dans la liste sans les filtrer. Mets uniquement les directions libres dans ta liste.

</details>

## Étape 3 : Poursuivre seulement quand tu es proche
<!-- ws: {type: exercise, id: a2-mode-follow} -->

Tu tiens les trois comportements, et les deux branches de `'scared'`. Il te manque les deux qui relient `'patrol'` et `'follow'` : proche fait passer en `'follow'`, loin ramène en `'patrol'`.

« Proche » se mesure en **cases** : l'écart horizontal **plus** l'écart vertical. Le seuil est de **5 cases**.

<!-- ws:toolbox -->
### Boîte à outils

> 🧰 **Outil #1 : `math.abs` retire le signe**
> ▶️ **Essaye dans la Console :**
> ```lua
> print(math.abs(5))    -- 5
> print(math.abs(-5))   -- 5
> ```
> Que Pac-Man soit trois cases à gauche ou trois cases à droite, il est à trois cases.
> *(Si tu as fait l'étape 7 de la partie 1, tu le connais déjà.)*
> 📘 [math.abs](https://www.lua.org/manual/5.3/manual.html#pdf-math.abs)

> 🧰 **Outil #2 : `<=` « plus petit ou égal »**
> `a < b` est faux quand `a` vaut exactement `b`. `a <= b` est vrai dans ce cas.
> ▶️ **Essaye dans la Console :**
> ```lua
> age = 15
> if age <= 17 then
>   return 'tarif jeune'
> end
> ```
> 📘 [Les opérateurs de comparaison](https://www.lua.org/manual/5.3/manual.html#3.4.4)

<!-- /ws:toolbox -->

### 🥸 Mise en application

**Ton objectif :** un fantôme qui patrouille quand tu t'éloignes, et te repère quand tu reviens.

Tout se joue dans tes branches. Tes règles de déplacement ne changent pas : ton arbre de la partie 1 est déjà ce que fait le fantôme quand il n'est ni `scared` ni `patrol`.

1. une variable `totalDistance` qui vaut la distance en cases
2. les deux branches qui manquent : celle qui part de `'patrol'` quand `totalDistance` descend à 5 ou moins, et celle du retour, qui part de `'follow'` quand tu t'éloignes

Tu dois obtenir ceci : **orange** de loin, **rouge** à 5 cases ou moins, orange à nouveau quand tu t'éloignes. En rouge, le jeu le fait aussi accélérer un peu : ça ne vient pas de ton code.

![Pac-Man approche : le fantôme erre en orange, puis vire au rouge dès qu'il est à moins de cinq cases de Pac-Man.](img/a2-e4-bascule.gif)

![Le même instant, figé : à cinq cases ou moins, le fantôme est rouge et fonce droit sur Pac-Man.](img/a2-e4-rouge.png)

<!-- ws: {type: hint} -->
<details><summary>Si tu es bloqué</summary>

Orange en permanence ? `print(totalDistance)` juste après l'avoir calculée, et regarde le panneau **Console** : en te rapprochant, le nombre doit descendre vers 0 en restant **positif**. S'il devient négatif, il te manque la valeur absolue.

Rouge en permanence ? Il te manque la flèche du retour : il entre en `'follow'` et rien ne l'en fait ressortir.

Plus jamais bleu quand il est rouge ? Ta flèche vers `'scared'` part sûrement de `'patrol'` seulement. Sur le diagramme elle part **des deux** modes : elle ne teste pas `state`, seulement `game.scaredTimer`.

Rouge mais immobile ? Ton arbre de la partie 1 est resté **dans** le bloc `patrol`, au lieu d'être à côté.

</details>

## Étape 4 : Le fantôme complet
<!-- ws: {type: exercise, id: a2-fantome-complet} -->

Tes trois modes existent séparément. Reste à voir s'ils s'enchaînent proprement.

### 🥸 Mise en application

**Ton objectif :** retrouver les six comportements ci-dessous.

| Ce que tu fais | Ce que tu dois voir |
| --- | --- |
| Tu restes à l'autre bout de la carte | **orange**, il erre au hasard sans te chercher |
| Tu approches à 5 cases ou moins | **rouge**, il accélère et fonce sur toi |
| Tu manges une grosse pac-gomme blanche | **bleu**, il s'écarte pendant 8 s |
| Tu le touches en bleu | tu l'attrapes : il repart à l'autre bout de la carte |
| Tu le touches en orange ou rouge | tu meurs, ça redémarre après 3 s |
| À tout moment | il ne traverse aucun mur |

Et si tu veux la gagner en entier, 211 pac-gommes et un score de **2 270** :

![Partie gagnée. Le fantôme est rouge : il chassait au moment de la dernière pac-gomme.](img/a2-e6-victoire.png)

<!-- ws: {type: hint} -->
<details><summary>Si tu es bloqué</summary>

Un mode ne se déclenche jamais ? Reviens à l'étape qui l'a introduit et refais sa **Mise en application.**

</details>

## Bonus : Tenir sa direction
<!-- ws: {type: exercise, id: a2-tenir-direction, optional: true} -->

En mode `patrol`, ton fantôme erre, mais il ne va nulle part : à chaque case il retire aléatoirement une direction, et une fois sur quatre c'est un demi-tour.

Il lui faut garder sa direction quelques cases au lieu de changer à chacune. Le jeu appelle `ghost` **à chaque case**, donc compter les appels, c'est compter les cases.

Deux outils : `me.direction`, la direction que le fantôme suit en ce moment, et une variable à toi, `compteur`, qui augmente de 1 à chaque appel.

La règle à poser **avant** ton tirage au hasard : si `compteur < 5` **et** que la case devant est libre, continue dans `me.direction`. Sinon, remets `compteur` à 0 et tire.

<!-- ws:toolbox -->
### Boîte à outils

> 🧰 **Outil #1 : une variable qui survit d'un appel à l'autre**
> Écris-la **en dehors** de la fonction, tout en haut de ton fichier : la ligne qui la met à `0` ne passe alors qu'une seule fois, et la variable garde sa valeur d'un appel au suivant. Mets cette même ligne **dans** la fonction et elle repasse à chaque appel : la variable revient à `0` à chaque case.
> ▶️ **Essaye dans la Console :**
> ```lua
> nombreDeVisites = 0
> function visite()
>   nombreDeVisites = nombreDeVisites + 1
> end
> ```
> Appelle `visite()` trois fois dans la **Console**, puis tape `nombreDeVisites` : tu lis `3`.
> 📘 [Les variables](https://www.lua.org/manual/5.3/manual.html#3.2)
<!-- /ws:toolbox -->

### 🥸 Mise en application

**Ton objectif :** un fantôme qui tient sa direction cinq cases d'affilée, et qui traverse donc vraiment la carte.

1. En haut du fichier, à côté de `state` : `compteur = 0`
2. Dans ton bloc `patrol`, la règle ci-dessus **avant** le tirage au hasard, et `compteur = compteur + 1` juste avant elle

`me.direction` vaut `'left'`, `'right'`, `'up'`, `'down'`, ou `nil` au premier appel et quand il est arrêté.

Tu dois obtenir ceci :

![Le fantôme orange tient sa direction plusieurs cases, puis tire la suivante au hasard : il traverse la carte au lieu de tourner en rond.](img/a2-e2-patrouille.gif)

*C'est réussi si :* il file en ligne droite sur plusieurs cases avant de tourner.

## Bonus : Ton fantôme à toi
<!-- ws: {type: exercise, id: a2-ton-fantome, optional: true, requires: a2-patrouiller} -->

*Tu es arrivé au bout, les trois modes s'enchaînent. Ce qui suit n'est plus un exercice : c'est ta récompense, et c'est la partie que personne ne fait pareil.*

Ton fantôme marche, mais ses réglages sont ceux qu'on t'a donnés. Son caractère tient dans ces trois lignes du tableau : deux nombres et un ordre :

| Ce que tu changes | Ce que ça donne |
| --- | --- |
| le seuil des 5 cases | un fantôme myope, ou un qui te repère de l'autre bout de la carte |
| l'ordre des règles dans `follow` | poursuivre ou essayer de te couper la route |
| le nombre auquel tu compares `compteur`, si tu as fait le bonus **Tenir sa direction** | `5` le laisse tenir cinq cases ; `2` le fait tourner deux fois plus souvent |

Un seul de ces nombres suffit à changer son caractère. Même trajet de Pac-Man, deux fantômes opposés :

![Seuil à 2 : Pac-Man lui passe devant à trois cases et il ne bouge pas d'un pixel.](img/a2-e7-myope.gif)

![Seuil à 30, même trajet : il vire au rouge et le prend en chasse à travers toute la carte.](img/a2-e7-longue-vue.gif)

### 🥸 Mise en application

**Ton objectif :** un fantôme qui te ressemble, et une partie jouée contre lui.

1. Écris son caractère en trois phrases, **en français**. Par exemple : *« Il ne me voit que de très près. Mais dès qu'il me voit, il coupe au plus court. Et il ne lâche plus. »*
2. Traduis chaque phrase en un réglage, et donne-lui un nom.
3. Lance, joue trente secondes : ta description se vérifie-t-elle à l'écran ?

<!-- ws: {type: hint} -->
<details><summary>Si tu es bloqué</summary>

Tu ne vois pas quoi changer ? Prends le seuil, et uniquement lui. Mets-le à `2`, joue trente secondes. Mets-le à `30`, rejoue.

Ton fantôme ne ressemble pas à ta description ? Change **un** réglage à la fois et relance entre chaque. À deux changements d'un coup, on ne sait plus lequel a fait quoi.

</details>

## Défis bonus - Partie 2
<!-- ws: {type: exercise, id: a2-bonus, optional: true} -->

Trois variantes de ta machine à états.

### Défi #1 : L'opportuniste

Dans ta flèche de sortie de `'scared'`, ne le laisse pas avoir peur jusqu'au bout : dès que `game.scaredTimer` descend sous 2, remets-le en chasse sans attendre la fin des 8 secondes.

*C'est réussi si :* il repasse au rouge **avant** que la super pac-gomme soit épuisée.

### Défi #2 : Le fantôme rancunier

En `scared`, ne fuis que si tu es proche : au-delà de 10 cases, il n'a plus peur de toi et repart en patrouille, même bleu.

*C'est réussi si :* colle-toi à lui, il s'écarte ; éloigne-toi de dix cases, il se remet à errer au hasard sans t'éviter.

### Défi #3 : Le fantôme imprévisible

Combine les deux modes : en `patrol`, fais-le foncer sur toi une fois de temps en temps, au hasard, même si tu es loin.

*C'est réussi si :* il t'arrive de le voir charger depuis l'autre bout de la carte.

## Fin de la partie 2

Ton fantôme sait patrouiller, poursuivre et prendre peur. Trois humeurs, trois couleurs, et c'est toi qui as écrit les règles de chacune.

Tu as utilisé deux façons de programmer un comportement : un arbre de décision, puis une **machine à états**. Ce qui sépare les deux tient dans un seul mot, `state` : ton fantôme se souvient de ce qu'il est en train de faire, et c'est ce souvenir qui décide de la suite.
