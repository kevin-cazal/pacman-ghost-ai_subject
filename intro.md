# IA du fantôme de Pac-Man

Dans le Pac-Man original, les quatres fantômes n’ont pas tous le même caractère. Blinky fonce droit sur
toi. Pinky vise la case *devant* toi pour te couper la route. Clyde te suit, puis file dans son
coin dès qu’il s’approche trop. Inky, lui, se décide en regardant où est Blinky. 
Rien de tout ça n’est du hasard : quelqu’un l’a écrit, règle par règle.

[illustration photo des véritables fantôme du pacman original]

Aujourd'hui c'est toi qui va ré-écrire ces règles pour donner vie à ton premier PNJ. 

![Le fantôme poursuit Pac-Man : c’est ton code qui décide de sa direction, case par case.](img/jeu.png)

**Bientôt c'est TON code qui fera bouger ce fantôme**

Et à la fin, tu en auras fait un vrai chasseur.

**Deux parties de 2 h 30**, en **Lua**, un langage de programmation bien pratique pour développer des petit jeux vidéo. Rien à installer, tout se fait dans ton navigateur.

- **Partie 1 — Arbre de décision.** Des règles « si… alors… » afin d'avoir un fantôme qui poursuit.
- **Partie 2 — Machine à états finis.** Pour avoir un fantôme qui change d’humeur.

## Le fichier de départ

À recoller si ton code ne fonctionne pas et te paraît illisible.

```lua
function buildInfos(ghost, pacman, map)
  return {}
end

function chooseDirection(infos, map)
  return nil
end

function updateState(infos, game)
  return 'patrol'
end
```

## Ce que le jeu te donne

Ces termes viennent du jeu, pas de Lua.

| Terme | Signification |
| --- | --- |
| `buildInfos` | La fonction où tu construis la liste `infos` : les informations que tu prépares pour le fantôme |
| `chooseDirection` | La fonction où tu écris les règles « si… alors… » qui permettent au fantôme de choisir une direction |
| `updateState` | La fonction où tu écris les règles pour choisir l’humeur du fantôme (partie 2) |
| `ghost.X / Y` | Position du fantôme (X,Y), en cases |
| `pacman.X / Y` | Position de Pac-Man (X,Y), en cases |
| `map.isWall(x, y)` | `true` si la case `(x, y)` est un mur |
| `'left'` `'right'` `'up'` `'down'` | Les quatre directions que le jeu comprend. Toujours en anglais, toujours entre apostrophes |
