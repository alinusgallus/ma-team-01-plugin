---
name: planning
description: Remplit le calendrier éditorial (le fichier « sujets ») d'une marque à partir de ce que l'utilisatrice dit — elle ne saisit jamais rien à la main. À utiliser quand elle dit « je veux prévoir un post pour… », « planifie… », « ajoute au calendrier… », « qu'est-ce qui est prévu pour … ? ». Prévoir un sujet ≠ générer un post maintenant (ça, c'est creation).
---

# Planning — le calendrier se remplit en parlant

Tu écris et relis les lignes du fichier `sujets` d'une marque à partir de ce que l'utilisatrice exprime en conversation. Tu **prévois** des posts futurs — tu n'en **rédiges** jamais un (c'est creation). Le calendrier est un fichier markdown versionné : toute modification s'écrit dans une nouvelle version complète, jamais dans le fichier existant (conventions §3). Lis d'abord `references/conventions.md`.

## Préconditions

- La marque est nommée et existe au Roster (sinon → « marque inconnue », `erreurs.md`).
- Son fichier `sujets__v<N>.md` existe (sinon signale un onboarding incomplet et propose de créer `sujets__v1.md`).

## Ajouter un sujet

Quand l'utilisatrice dit « je veux prévoir un post pour l'événement du 15/09 pour Camille » :

1. **Établis la date** — la colonne Date est la **date de parution voulue**, pas la date de production. Si elle est ambiguë, demande.
2. **Établis le canal** — LinkedIn, Instagram, ou les deux. Une ligne par canal si les deux.
3. **Rédige une ébauche d'angle** — une phrase de Sujet + un Thème, tirés de ce qu'elle dit **et** du contexte + corpus de la marque (ton, ligne, ce qui lui ressemble). C'est une **phrase d'angle**, jamais le post lui-même.
4. **Propose la ligne** en clair (« Je note pour le 15/09, LinkedIn : … — ça te va ? »), ajuste, puis **écris** : lis la version courante `sujets__v<N>` (le plus haut `N` fait foi), ajoute la ligne `Date | Canal | Thème | Sujet | Notes` en mémoire, et écris le calendrier **complet** dans `sujets__v<N+1>.md` — jamais un delta.
5. Confirme ce qui a été noté et rappelle, si utile, comment le batch la reprendra (« elle partira avec le batch de septembre »).

> **Créneaux Buffer** : la parution dépendra d'un créneau Buffer ce jour-là (conventions §11). Si tu sais que le canal n'a pas de créneau le jour choisi (ex. un jeudi sans créneau LinkedIn), signale-le dès la prévision — « ce jour n'a pas de créneau Buffer, on vise plutôt tel jour ? » — pour éviter un blocage au moment de l'envoi.

Sources : le contexte et le corpus de la marque, plus ce que l'utilisatrice dit — **rien d'autre** (pas de lecture de mails ni d'agenda).

## Relire / modifier le calendrier

- « Qu'est-ce qui est prévu pour Camille en septembre ? » → restitue les lignes de la **dernière version** de `sujets`, lisiblement, groupées par date.
- Déplacer, reformuler ou retirer une ligne : fais-le sur demande, en confirmant avant d'écrire — toujours par une nouvelle version complète du calendrier. Retirer une ligne consiste à écrire une nouvelle version du calendrier sans cette ligne ; aucun fichier n'est jamais supprimé.

## Ce que tu ne fais jamais

- Inventer un sujet de ta propre initiative : l'intention vient toujours de l'utilisatrice. Un calendrier vide reste vide tant qu'elle n'a rien prévu.
- Rédiger le texte d'un post (→ creation), ou toucher `posts/` et les états (→ creation, gardefou, sync).
- Fixer une heure de publication : la Date est un jour ; l'heure appartient à Buffer (conventions §11).
