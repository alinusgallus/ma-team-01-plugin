---
name: sync
description: Rapproche Drive de la réalité — interroge Buffer pour savoir ce qui est effectivement paru et l'atteste en créant les fichiers publie/. À utiliser quand l'utilisatrice demande « qu'est-ce qui est parti ? », « mets à jour l'historique », « où en sont les publications ? », ou après une période de publications. Ne publie ni ne valide jamais rien : « publie », « valide », « envoie » = gardefou.
---

# Sync — attester ce qui est paru, d'après la réalité

Tu confirmes les parutions **effectives** et tu avances l'historique : tu es le seul à créer dans `publie/` (conventions §8). Tu ne modifies, ne déplaces et ne supprimes rien — tu crées, c'est tout : Drive dit ce qui a été envoyé, Buffer dit ce qu'il en advient, et toi tu graves la confirmation. Lis d'abord `references/conventions.md`.

## Étapes

1. **Périmètre** : la ou les marques demandées, ou tout le Roster si l'utilisatrice dit « partout ».
2. **Liste les posts programmés non encore parus** : les clés présentes dans `planifie/<mois>/` mais absentes de `publie/<mois>/`.
3. **Interroge Buffer** (lecture seule), avec le `buffer_id` de chaque fichier `planifie/` : la publication est-elle confirmée sur son canal ?
4. **Confirmé paru** → crée `publie/<mois>/<clé>.md` (format en conventions §5) : les mêmes champs que le fichier `planifie/`, plus `publie_le`. Crée le dossier mensuel s'il n'existe pas, et vérifie d'abord que le fichier n'existe pas déjà (idempotence, conventions §7). L'historique de la marque se construit par ajout.
5. **Pas encore parti** (date future, file d'attente) → ne crée rien, sans bruit.
6. **Cas `rappel` / publication native** : Buffer ne sait pas toujours ce qui a été publié à la main depuis le téléphone. Pour ces posts, demande simplement : « Le post Instagram de Camille du 12/09, tu l'as publié ? » — sa confirmation vaut vérité, et tu crées le fichier `publie/` (avec la date qu'elle indique, ou la date prévue à défaut). Sans réponse, rien n'est créé : mieux vaut un historique incomplet qu'un historique inventé.
7. **Posts avec visuel** (conventions §15) : pour chaque fichier `planifie/` dont l'en-tête déclare une image, vérifie auprès de Buffer que le post porte bien un média — **y compris les posts encore en file**, pas seulement les parus.
   - **En file, sans média** → rappel doux, message « visuel non attaché » d'`erreurs.md` : le visuel est dans Drive, il suffit de l'attacher dans Buffer avant la date. C'est le moment de rattraper l'oubli.
   - **Paru, sans média** → anomalie : dis-le en clair (le post est paru en texte seul). Le fichier `publie/` se crée quand même — il atteste la parution, pas sa complétude.
8. **Anomalie** (un `buffer_id` que Buffer ne connaît plus, un envoi qui semble perdu) → signale-le en clair, sans rien créer.

## Le bonus corpus

Pour chaque post fraîchement confirmé publié, propose — sans insister :

> Il y en a dans ce lot dont tu es particulièrement fière ? Dis-le-moi et je les ajoute au dossier `exemples` de la marque — c'est ce qui affine sa voix pour la suite.

Ajoute au corpus **uniquement** les posts qu'elle désigne : « validé » ne veut pas dire « exemplaire » — le corpus reste son choix.

## À la fin

Résume par marque : combien confirmés publiés, combien encore en file, combien en attente de sa confirmation (mode rappel), et toute anomalie.

## Ce que tu ne fais jamais

- Déclarer publié un post sans confirmation de Buffer **ou** de l'utilisatrice.
- Créer dans `planifie/`, pousser quoi que ce soit à Buffer, ou valider (→ gardefou).
- Modifier, déplacer ou supprimer un fichier : l'historique se construit par ajout, jamais autrement.
- Harceler l'utilisatrice sur le mode rappel : une question simple, et on n'en parle plus jusqu'à la prochaine sync.
