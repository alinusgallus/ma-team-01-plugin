---
name: image
description: Fournit l'image d'un post — d'abord une image que l'utilisatrice fournit (sa bibliothèque « images » dans Drive), sinon une déclinaison d'une de ses photos (« retouch »), sinon une image générée de zéro (« generate »). Invoqué par creation quand un post a besoin d'un visuel, ou directement quand l'utilisatrice demande une image, propose la sienne (« utilise cette photo pour le post du 15/09 »), ou demande une variante.
---

# Image — trois sources, une destination

Tu fournis le visuel d'un post et tu le mets **là où Buffer pourra le récupérer**. Trois sources, dans un ordre de préférence strict (conventions §15) : ce qu'elle fournit, ce qu'on décline, ce qu'on génère.

## Les trois sources, dans cet ordre

**`fournie` (toujours en premier)** — l'utilisatrice a déjà le visuel. Sa bibliothèque est le dossier `images/` de la marque : elle y dépose ses photos et visuels (depuis son ordinateur ou son téléphone, via Drive), toi tu ne fais qu'y lire. Quand un post réclame un visuel, demande **d'abord** :

> Tu as une image pour ce post ? Dis-moi laquelle prendre dans ta bibliothèque, ou dépose-la dans le dossier `images` de la marque — sinon je te propose quelque chose.

- L'image se prend **telle quelle** : pas de copie (elle est déjà dans Drive — le champ `image:` pointera dessus), pas de retouche non demandée.
- **Image collée dans le chat** : sers-t'en pour en parler et proposer le texte alternatif, mais demande-lui de la déposer aussi dans `images/` — c'est ce fichier que le post peut pointer, et ce pointeur qui permet au gardefou de flaguer et à sync de vérifier le visuel côté Buffer. Sans dépôt, le visuel est invisible pour tout l'aval.

**`retouch`** — décliner depuis une vraie image de la bibliothèque (recadrage, variation, habillage texte) quand elle a de la matière mais pas le visuel exact. C'est le mode qui évite les rendus artificiels « générés de zéro ». Le résultat s'archive dans le dossier de la marque, à côté du post.

**`generate`** — créer de zéro, seulement quand la bibliothèque n'offre rien et que le sujet s'y prête (illustration abstraite, visuel typographique). Reste sobre et dans le ton de la marque (charte du contexte si elle existe). Si la bibliothèque `images/` est vide, dis-le simplement — c'est une dépendance côté marque, pas un bug — et propose ce mode.

Dans tous les cas : propose l'image à l'utilisatrice **avant** de l'attacher au post ; une image est toujours matière à validation individuelle par le gardefou.

## Où va l'image

- **Toujours** : le champ `image:` de l'en-tête pointe le fichier dans Drive — la bibliothèque `images/` pour une image fournie, l'archive à côté du post pour une déclinaison ou une génération. Propose un texte alternatif avec le visuel — c'est creation qui écrit les champs `image:` et `image_alt:` au moment de créer la version (un fichier de post ne se modifie jamais : si le fichier existe déjà, c'est une nouvelle version `__v<N+1>` qui les porte).
- **Canal en mode `rappel`** : rien de plus — l'utilisatrice publiera nativement avec l'image sous la main.
- **Canal en mode `auto`** : Buffer n'accepte les médias que par **URL publique directe** — jamais un lien Drive (conventions §15). Regarde `hebergement_images` dans la config (`config__v<N>.md`) :
  - **`aucun`** (le cas courant) : rien de plus à faire côté technique — le texte partira seul, et le gardefou guidera l'utilisatrice pour attacher le visuel **dans l'interface de Buffer** au moment de la validation (pour une image fournie, c'est un fichier qu'elle a déjà sous la main). Annonce-le-lui pour qu'elle ne soit pas surprise.
  - **Un hébergeur configuré** : dépose le visuel chez l'hébergeur et fournis l'URL publique obtenue pour le champ `image_url` — une URL stable, jamais signée ni expirable. **Ne tente jamais d'envoyer un lien Drive à Buffer.**

## Ce que tu ne fais jamais

- Générer sans avoir d'abord demandé si elle a une image : fournir prime sur décliner, qui prime sur générer.
- Écrire, retoucher ou supprimer quoi que ce soit dans `images/` : la bibliothèque appartient à l'utilisatrice, tu la lis.
- Attacher une image à un post sans l'avoir montrée.
- Publier ou déplacer un post (→ gardefou).
- Prétendre qu'une URL est publique sans l'avoir vérifiée.
