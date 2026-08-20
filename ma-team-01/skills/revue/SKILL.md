---
name: revue
description: Présente un batch d'un coup, en lecture seule — un tableau consolidé des posts depuis Drive, groupés par marque, avec l'état de chacun (brouillon, programmé, publié) déduit de la présence de sa clé dans les dossiers. À utiliser quand l'utilisatrice veut voir un batch (« montre-moi le batch », « où en est septembre ? », « qu'est-ce qui attend ma validation ? »). Vue seulement : si elle valide ou dit de publier/envoyer, c'est gardefou.
---

# Revue — une vue, rien qu'une vue

Tu rends **visible** l'état d'un batch tel qu'il est dans Drive. Tu n'écris rien, tu ne décides rien : tu lis les trois dossiers et tu **déduis** l'état de chaque post selon la règle des conventions (§6). Lis d'abord `references/conventions.md`.

## Construire la vue

Pour le périmètre demandé (période / marque / thème) :

1. Lis `posts/<mois>/` — en ne retenant que la **dernière version** de chaque clé —, `planifie/<mois>/` et `publie/<mois>/`.
2. Déduis l'état par clé `date_canal_slug` : présente dans `publie/` → **publié** ; sinon dans `planifie/` → **programmé** ; sinon → **brouillon**. Le dossier le plus avancé gagne.
3. Pour un post programmé, compare `version_source` (en-tête du fichier `planifie/`) à la version courante dans `posts/` : si elle est plus ancienne, signale « la version programmée n'est plus la dernière » (message d'`erreurs.md`).

## Rendu

Construis un Artifact consolidé, **groupé par marque** — jamais de mélange de voix dans une même section :

1. **Les brouillons d'abord.** Date, canal, sujet, le texte (dernière version), la mention « écrit sans exemples » le cas échéant. Si le gardefou vient de contrôler ces posts dans la conversation, reprends ses verdicts : les sûrs ensemble (présentés pour une validation en bloc), puis les signalés un par un, chacun avec sa raison en clair (« pourrait contredire l'engagement : … », « contient une image », « doute sur la marque ») et l'image affichée s'il y en a une. Sinon, montre-les comme « pas encore contrôlés » — ne les présente jamais comme sûrs, et n'invente pas de verdict : il n'est stocké nulle part pour un brouillon.
2. **Les programmés.** Date d'envoi (`envoye_le`), verdict figé dans le fichier `planifie/`, et l'écart de version s'il existe.
3. **Les publiés.** L'historique du périmètre, en bref.
4. En tête : le résumé chiffré — « 14 posts : 3 publiés, 5 programmés, 6 brouillons dont 2 à regarder ».

## La validation reste un geste de chat

Termine toujours par la marche à suivre :

> Pour valider : dis-moi par exemple « ok pour les 11 sûrs de Camille », puis on regarde les 3 signalés un par un. Pour corriger un post : dis-le-moi (« le 2e, plus court ») et je le fais réécrire puis recontrôler.

Le tableau est en **lecture seule** : aucune modification ne passe par lui. Toute correction passe par le chat → creation (nouvelle version) → re-contrôle du gardefou.

## Ce que tu ne fais jamais

- Écrire quoi que ce soit dans Drive : tu ne fais que lire et déduire.
- Inventer un verdict de gardefou : tu reprends celui de la conversation ou celui figé dans `planifie/`, sinon « pas encore contrôlé ».
- Déclencher une publication : tu transmets le geste de l'utilisatrice au gardefou, c'est tout.
