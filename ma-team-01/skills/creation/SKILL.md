---
name: creation
description: Rédige les brouillons de posts d'une marque — soit un batch depuis le calendrier (« génère les posts de septembre pour Camille »), soit une idée à l'unité (« j'ai une idée de post pour … »). Écrit par analogie avec le corpus d'exemples de la marque et dépose les fichiers versionnés dans posts/. À utiliser pour toute demande de rédaction ou de réécriture de post. « Je veux prévoir / planifier un post pour plus tard » sans demande de rédaction immédiate = planning, pas creation. Ne publie ni n'envoie jamais rien : « publie », « valide le batch », « envoie » = gardefou.
---

# Creation — du sujet au brouillon, par analogie

Tu assembles chaque brouillon à partir de trois matières : un **sujet** (fourni par l'utilisatrice, jamais inventé), le **contexte** de la marque, et son **corpus**. Tu déposes tes fichiers dans `posts/`, et uniquement là — chaque écriture est une **création de fichier**, jamais une modification (conventions §1). Lis d'abord `references/conventions.md` et `references/vocabulaire.md`.

## Les deux entrées, un seul circuit

**A. Le batch** — « Génère les posts de septembre pour Camille » : lis la **dernière version** du fichier `sujets` de la marque (le plus haut `__v<N>` fait foi) et retiens les lignes qui correspondent au périmètre (période et/ou marque et/ou thème). Une ligne = un brouillon, sur le canal indiqué. Calendrier vide pour ce périmètre → message « calendrier vide » d'`erreurs.md`, rien à générer.

**B. L'idée à l'unité** — « J'ai une idée de post pour Camille : … » : le sujet vient de la conversation. Il ne passe pas par le calendrier — il reste éphémère.

Dans les deux cas **la marque doit être nommée**. Si elle ne l'est pas, demande — c'est la première protection contre le pire risque : écrire pour la mauvaise marque.

## Pour chaque sujet

1. **Charge** le contexte de la marque (cadre + engagements) — la dernière version de `contexte__v<N>.md`, ou le document qu'elle tient elle-même si le contexte y renvoie — et son dossier `exemples/`.
2. **Détecte le corpus — automatiquement, sans que ce soit un mode** (seuils en conventions §13) :
   - **Corpus utile (≥ 5 exemples)** → écris **par analogie** avec les exemples les plus proches du sujet : leur structure, leur rythme, leur registre. Note lesquels tu as utilisés.
   - **Corpus maigre (1–4 exemples)** → appuie-toi sur ce qui existe, mais préviens : « le corpus est encore maigre (N posts) — la voix s'affinera à mesure qu'il grandit ».
   - **Corpus vide (0)** → écris depuis le seul contexte, et **dis-le franchement** (message « corpus absent » d'`erreurs.md`) : le post risque d'être passe-partout. Propose, une fois par conversation, d'écrire ensemble quelques posts fondateurs (voir onboarding). Ne bloque jamais pour autant.
3. **Rédige** le post, prêt à publier, dans la langue et le registre de la marque. Le sujet du Plan est une phrase d'angle — jamais le texte à recopier.
4. **Image nécessaire ?** (Instagram toujours ; LinkedIn si le sujet le réclame ou si les Notes le précisent) → appelle le skill **image**, puis renseigne dans l'en-tête `image` (le chemin Drive du visuel) et `image_alt` — le texte alternatif est **à toi** : une phrase qui décrit le visuel, dans la langue et le ton de la marque (conventions §15). Si un hébergement est configuré et que le skill image a produit une URL publique, renseigne aussi `image_url`.
5. **Dépose** le fichier dans `posts/<mois>/` de la marque, où `<mois>` est le mois de **parution voulue** (`AAAA-MM`) : nommage `AAAA-MM-JJ_canal_slug__v1.md`, format des conventions (§4–5), avec `exemples_utilises` et `version` renseignés. Crée le dossier mensuel s'il n'existe pas encore (création paresseuse), et vérifie toujours par `search_files` que le titre n'existe pas déjà (conventions §7). Pas de champ `check` : le verdict du gardefou ne se stocke pas dans un brouillon.
6. **Toute réécriture est une nouvelle version.** Pour une idée à l'unité (entrée B) comme après un retour du gardefou (« plus court », « moins de jargon ») : relis la version courante, réécris, et dépose `__v<N+1>` — le fichier précédent reste en place, la version la plus haute fait foi. Itère jusqu'à ce que l'utilisatrice soit satisfaite.

## Deux cas à signaler

- **Le post retravaillé était déjà programmé** (sa clé existe dans `planifie/`) : dis-le — « la version programmée n'est plus la dernière » (message d'`erreurs.md`). La suite côté Buffer appartient au gardefou.
- **La date de parution change** : la clé change, donc le dossier mensuel aussi. Crée un **nouveau post** (`__v1`) sous la nouvelle clé et signale que l'ancien reste en place (conventions §4).

## À la fin d'un batch

Résume : combien de brouillons, pour quelles marques et quels canaux, lesquels sont écrits « sans exemples », lesquels attendent une image. Puis annonce la suite :

> Les brouillons sont posés. Je passe le gardefou dessus, puis je te montre le batch d'un coup ?

## Ce que tu ne fais jamais

- Inventer un sujet, ou générer quoi que ce soit depuis un calendrier vide.
- Modifier, déplacer ou supprimer un fichier existant : une réécriture est une nouvelle version, rien d'autre.
- Écrire ailleurs que dans `posts/` : `planifie/` et `publie/` appartiennent au gardefou et à sync ; publier = gardefou.
- Mélanger les voix : un batch multi-marques charge le contexte et le corpus **de chaque marque**, jamais un pot commun.
