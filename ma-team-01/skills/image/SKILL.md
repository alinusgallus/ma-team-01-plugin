---
name: image
description: Fournit l'image d'un post — soit générée de zéro (« generate »), soit déclinée depuis une vraie photo de la marque (« retouch »). Invoqué par creation quand un post a besoin d'un visuel, ou directement quand l'utilisatrice demande une image, une variante ou un visuel.
---

# Image — deux modes, une destination

Tu fournis le visuel d'un post et tu le mets **là où Buffer pourra le récupérer**. Deux modes, à choisir selon la matière disponible.

## Les deux modes

**`retouch` (à préférer quand c'est possible)** — décliner depuis une **vraie image de base** : une photo de la bibliothèque d'assets de la marque (recadrage, variation, habillage texte). C'est le mode qui évite les rendus artificiels « générés de zéro ».
- Précondition : une image de base réelle. Si la marque n'a pas de bibliothèque d'assets, dis-le — c'est une dépendance côté marque, pas un bug.

**`generate`** — créer de zéro quand aucune base réelle n'existe ou que le sujet s'y prête (illustration abstraite, visuel typographique). Reste sobre et dans le ton de la marque (charte du contexte si elle existe).

Dans les deux cas : propose l'image à l'utilisatrice **avant** de l'attacher au post ; une image est toujours matière à validation individuelle par le gardefou.

## Où va l'image

- **Toujours** : range l'image dans le dossier de la marque, à côté du post, et propose un texte alternatif avec le visuel — c'est creation qui écrit les champs `image:` et `image_alt:` de l'en-tête au moment de créer la version (un fichier de post ne se modifie jamais : si le fichier existe déjà, c'est une nouvelle version `__v<N+1>` qui les porte).
- **Canal en mode `rappel`** : rien de plus — l'utilisatrice publiera nativement avec l'image sous la main.
- **Canal en mode `auto`** : Buffer n'accepte les médias que par **URL publique directe** — jamais un lien Drive (conventions §15). Regarde `hebergement_images` dans la config (`config__v<N>.md`) :
  - **`aucun`** (le cas courant) : rien de plus à faire côté technique — le texte partira seul, et le gardefou guidera l'utilisatrice pour attacher le visuel **dans l'interface de Buffer** au moment de la validation. Annonce-le-lui pour qu'elle ne soit pas surprise.
  - **Un hébergeur configuré** : dépose le visuel chez l'hébergeur et fournis l'URL publique obtenue pour le champ `image_url` — une URL stable, jamais signée ni expirable. **Ne tente jamais d'envoyer un lien Drive à Buffer.**

## Ce que tu ne fais jamais

- Attacher une image à un post sans l'avoir montrée.
- Publier ou déplacer un post (→ gardefou).
- Prétendre qu'une URL est publique sans l'avoir vérifiée.
