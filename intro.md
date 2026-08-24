# IA du fantôme de Pac-Man

**Bienvenue !** Tu vas programmer l'intelligence du fantôme de Pac-Man.

Cet atelier se passe **entièrement dans le navigateur**. Le jeu est affiché à côté de ces
instructions : tu écris ton code dans le panneau **Code**, et tu le testes dans le panneau
**Jeu**.

Il se déroule en deux parties :

- **Atelier 1 - Arbre de décision.** Récupérer dans `buildInfos` les informations essentielles à
  la programmation du fantôme, puis écrire dans `chooseDirection` les règles qu'il doit suivre, à
  l'aide d'un arbre de décision.
- **Atelier 2 - Machine à états finis.** Programmer le fantôme avec 3 modes (`patrol`, `follow`,
  `scared`) qui changent selon la situation.

## Aide

**Clavier :** clique l'**éditeur** pour coder (flèches = curseur). Clique le **jeu** pour jouer
(flèches = Pac-Man). Le contour **jaune** indique le panneau actif.

Le code est sauvegardé automatiquement dans ton navigateur. Les erreurs s'affichent sous
l'éditeur.

## Glossaire

| Terme | Signification |
| --- | --- |
| `buildInfos` | Fonction où tu construis la table infos (les réponses pour le fantôme) |
| `chooseDirection` | Fonction où tu écris les règles « si... alors... » pour choisir une direction |
| `ghost.gridX / gridY` | Position du fantôme en cases sur la carte |
| `pacman.gridX / gridY` | Position de Pac-Man en cases |
| `map.isWall(x, y)` | true si la case (x, y) est un mur |
| `if / then / end` | `if condition then return 'left' end` — si la condition est vraie, choisis cette direction |
| `and` / `or` / `not` | Opérateurs logiques (remplacent `&&`, `\|\|`, `!`) |
| `'left' / 'right' / 'up' / 'down'` | Les 4 directions possibles (toujours entre apostrophes) |
| `canGoLeft` | Tu le calcules : `not map.isWall(ghost.gridX - 1, ghost.gridY)` |
| `distanceX` | Tu le calcules : `pacman.gridX - ghost.gridX` |
| `totalDistance` | Tu le calcules : `math.abs(distanceX) + math.abs(distanceY)` — nombre de cases entre fantôme et Pac-Man |
| `return nil` | « Je ne bouge pas » — à mettre à la fin si aucune règle ne s'applique |
