# Pac-Man — IA du fantôme

Programmez l'intelligence du fantôme de Pac-Man, en Lua.

Contenu d'atelier au format **convention 2.0**, prêt à être importé dans une instance CTFd par la plateforme [CTFd_coding_platform](https://github.com/kevin-cazal/CTFd_coding_platform).

## Ce que contient ce dépôt

| Fichier | Rôle |
|---|---|
| `subject.yaml` | la fiche d'identité du sujet : nom, ordre de lecture, runtime, défauts |
| `intro.md` | document de l'atelier (page d'entrée) |
| `atelier1.md` | document de l'atelier |
| `atelier2.md` | document de l'atelier |
| `img/` | les images, référencées en chemin relatif |

Le texte est du Markdown normal, lisible tel quel sur GitHub. Tout ce dont la plateforme a besoin est porté par des commentaires HTML `<!-- ws: ... -->`, invisibles à la lecture. La structure de l'atelier est lue dans les titres : il n'y a pas de sommaire à tenir à jour.

## Comment cet atelier est réglé

- **Mode** : `instructor_led`
- **Validation** : `checkpoint`
- **Points par étape** : `25` par défaut
- **Runtime** : Pac-Man (`pacman` épinglé en `00ffe3f`) — [pacman-ghost-ai_runtime](https://github.com/kevin-cazal/pacman-ghost-ai_runtime)

## Mettre cet atelier en ligne

```sh
git clone https://github.com/kevin-cazal/CTFd_coding_platform
cd CTFd_coding_platform
docker compose up -d                      # CTFd sur :8080
python3 tools/sync_subject.py <ce-depot> \
    --url http://localhost:8080 --admin-user admin --admin-pass ...
```

Relancer la commande met le contenu à jour : les étapes sont réconciliées par leur `id`, les réussites des participants sont conservées.

## Écrire ou modifier du contenu

La convention complète est dans [`docs/CONTENT_CONVENTION.md`](https://github.com/kevin-cazal/CTFd_coding_platform/blob/main/docs/CONTENT_CONVENTION.md), la version courte dans [`docs/CONTRIBUER.md`](https://github.com/kevin-cazal/CTFd_coding_platform/blob/main/docs/CONTRIBUER.md).

Deux règles de fond : le contenu s'écrit **en français**, en phrases simples, pour des lycéens de 15 à 18 ans — et **on ne donne jamais la réponse**, ni dans le texte ni dans les indices.
