---
name: gardefou
description: À invoquer systématiquement, sans répondre d'abord, dès que l'utilisatrice valide ou demande d'envoyer/publier — « ok publie », « valide le batch », « envoie », « c'est bon pour moi, tu peux envoyer », « publie tout sauf le troisième » — même comme tout premier message et même si aucun batch n'est visible dans la conversation : le skill retrouve les brouillons dans Drive. À invoquer aussi après creation pour contrôler un batch. C'est la couche de sécurité, seule autorisée à écrire dans planifie/ : contrôle chaque brouillon (bonne marque ? engagement respecté ? image ?), restitue les posts sûrs et les posts signalés en conversation, puis pousse dans Buffer et atteste l'envoi en créant le fichier planifie/. Publier ou envoyer = gardefou ; constater ce qui est déjà parti = sync.
---

# Gardefou — les contrôles en dur, puis la publication

Tu es le seul à faire progresser un post vers l'état « programmé » : le contrôle, l'envoi à Buffer, la création du fichier `planifie/` qui l'atteste (conventions §8). Tu ne modifies **aucun** fichier existant — le stockage est additif (conventions §1) : ton verdict vit en mémoire et en conversation, et ne se fige que dans le fichier `planifie/`, au moment de l'envoi. Lis d'abord `${CLAUDE_PLUGIN_ROOT}/references/conventions.md` et `${CLAUDE_PLUGIN_ROOT}/references/erreurs.md`.

## Phase 1 — Contrôler le batch, sans rien écrire

Travaille sur la **dernière version** de chaque post de `posts/` (le plus haut `__v<N>` fait foi — conventions §3). Si `search_files` renvoie deux fichiers de même titre dans un dossier (homonymes, corruption antérieure), ne choisis pas silencieusement : message « homonymes détectés » d'`erreurs.md`, et arrête-toi sur ce post.

Pour **chaque** brouillon du batch, exécute les trois contrôles en dur :

1. **Risque de mauvaise marque / mauvais canal** — l'en-tête du post (marque, canal) correspond-il au dossier où il vit, au Roster et au mapping de `channels` ? Le texte contient-il des indices d'une autre marque (nom, produit, tutoiement d'une marque qui vouvoie) ? C'est le risque le plus grave de tout le système : au moindre doute → `flagged`.
2. **Contradiction d'un engagement** — relis le post **contre chaque engagement** du contexte de sa marque, comme une seconde passe dédiée. Une contradiction possible — même indirecte — → `flagged`, avec l'engagement en cause cité dans la raison.
3. **Post avec visuel** — `image_source` renseigné dans l'en-tête → `flagged` (un visuel se valide toujours à l'œil ; le champ est présent même quand l'hébergement n'a pas eu lieu — conventions §5).

**Le verdict (`safe` / `flagged` + raison) reste en mémoire et se restitue en conversation — il ne s'écrit nulle part à ce stade.** Il est recalculé à chaque passage : un post modifié n'hérite jamais de son ancien contrôle. **Aucun post ne se publie sans check.**

Puis annonce : « X posts sûrs, Y à regarder individuellement — je te montre tout ça ? » (→ c'est la **revue** qui construit la vue consolidée ; toi, tu ne présentes pas, tu contrôles).

## Phase 2 — Publier sur validation

La validation est un **geste de chat** de l'utilisatrice (« ok pour les sûrs de Camille », « publie tout sauf le 3e »). Jamais un bouton, jamais une déduction : une phrase explicite de sa part.

- **Les posts sûrs se valident en bloc**, marque par marque. **Les posts flagged se traitent un par un** — jamais dans un lot, même si elle dit « publie tout » : confirme chacun.
- **Un post à visuel se revoit avec son image affichée** (par son `image_url`, dans le chat) : texte et image ensemble, par post — c'est le contrôle visuel, la raison d'être de l'hébergement à la création (conventions §15). Sur un canal `auto`, cette validation est le dernier regard humain. Sans `image_url` (hébergement `aucun`), restitue le nom du fichier Drive et l'`image_alt`.
- **Contrôle de dérive** (conventions §15), avant tout envoi, pour chaque post dont le constat `visuels/` porte un `drive_md5` : relis `get_file_metadata` du `drive_file_id` — somme différente → message « photo modifiée dans Drive depuis l'hébergement » d'`erreurs.md`. Constat `canva` : relis le design — `updated_at` postérieur à `canva_updated_at` → variante « design modifié dans Canva ». **Ce post ne part pas sans sa réponse** : « je réhéberge la nouvelle » (→ image : nouvelle version, nouveau constat) ou « garde l'ancienne ». Fichier introuvable dans Drive (ou design supprimé) → l'asset hébergé reste valable : dis-le simplement, sans bloquer.
- Avant l'envoi : vérifie qu'aucun canal ne dépasse **25 posts le même jour** (sinon → message « plafond Buffer »). Vérifie aussi que la clé du post n'existe pas déjà dans `planifie/<mois>/` : si le fichier existe, l'envoi a déjà eu lieu — dis-le au lieu de renvoyer (idempotence, conventions §7).
- **Créneau Buffer** (conventions §11) : vérifie que le canal a bien un créneau le jour de la date voulue. Aucun créneau ce jour-là → n'envoie pas ce post : message « jour sans créneau » d'`erreurs.md`, et propose le créneau le plus proche. Tu ne calcules jamais d'heure toi-même.
- **Clé déjà annulée** (conventions §6) : si la clé du post existe dans `annule/<mois>/`, c'est un post retiré de Buffer — ne le renvoie pas sous la même clé (ce serait un homonyme). Pour le remettre en file, il lui faut une nouvelle date, donc une nouvelle clé (conventions §4).
- Pour chaque post validé, dans cet ordre — **jamais l'inverse** :
  1. **Pousse-le à Buffer** sur son canal, avec sa **date** (colonne Date de l'en-tête). L'**heure** appartient à Buffer (conventions §11) — tu n'en calcules jamais. **Post avec visuel** (conventions §15) : si l'en-tête porte une `image_url`, envoie l'image **dans le même push** que le texte — `assets: [{ image: { url: <image_url>, metadata: { altText: <image_alt> } } }]` dans le `createPost`. Le contrôle « URL accessible » se fait **par la réponse de Buffer** : Buffer sonde l'URL à la création (dimensions résolues immédiatement) — une URL morte est un refus immédiat, à traiter avec le message « URL d'image inaccessible » d'`erreurs.md` (aucun fichier créé). Tu ne pré-vérifies pas l'URL toi-même. Sans `image_url` (hébergement `aucun`), le texte part seul.
  2. **Buffer a accepté** → crée `planifie/<mois>/<clé>.md` (format en conventions §5) : l'en-tête complet — `version_source` (la version de `posts/` envoyée), les champs `image` / `image_source` / `image_alt` / `image_url` recopiés du post, `check`, `check_raison` si flagged validé quand même, `buffer_id`, `envoye_le` — et le **texte exact envoyé**, recopié intégralement. Crée le dossier mensuel s'il n'existe pas. Le fichier atteste un fait accompli, il ne le prescrit pas.
  3. **Buffer a refusé** → aucun fichier. Le post reste un brouillon, l'état déduit reste exact, rien à compenser.
- **Post avec visuel parti sans `image_url`** (hébergement `aucun`) : une fois le fichier `planifie/` créé, guide l'utilisatrice — « Le texte est programmé dans Buffer ; le visuel est prêt dans Drive (<lien>). Ouvre le post dans Buffer et attache-le maintenant, pendant qu'on y est — dans le composer de Buffer tu peux le prendre directement depuis Google Drive. » (Sur ordinateur ; la première fois, Buffer demandera d'autoriser son intégration Google Drive — une fois.) Ne stocke pas sa confirmation : le fichier `planifie/` ne se modifie jamais, et c'est **sync** qui vérifiera côté Buffer que le visuel est bien en place.
- **Échec partiel d'un batch** : crée les fichiers `planifie/` **uniquement** pour les posts que Buffer a acceptés ; dis précisément ce qui est parti et ce qui reste, avec le message d'`erreurs.md` adapté (canal non connecté, Buffer injoignable…).
- L'utilisatrice demande une correction (« réécris celui-ci, moins de jargon ») : le post repart en **creation** (nouvelle version `__v<N+1>`), puis repasse **tes contrôles** — un post modifié n'hérite jamais de son ancien check.

## À la fin

Résume : combien envoyés à Buffer (par marque, par canal), combien en attente, combien écartés. Rappelle que **sync** confirmera les parutions effectives (création dans `publie/`).

## Ce que tu ne fais jamais

- Publier un post sans validation explicite de l'utilisatrice, ou un post `flagged` sans confirmation individuelle.
- Créer un fichier `planifie/` avant la confirmation de Buffer : l'ordre est Buffer d'abord, fichier ensuite, sans exception.
- Modifier, déplacer ou supprimer un fichier existant — tu ne fais que créer, et uniquement dans `planifie/` (le texte des posts appartient à creation, le Plan à planning, `publie/` à sync).
- Montrer une erreur technique brute.
