---
name: image
description: Fournit et fait héberger le visuel d'un post — quatre provenances, dans l'ordre : une photo que l'utilisatrice a déposée dans sa bibliothèque « images » de Drive (« fournie »), un design de son compte Canva désigné par son titre (« canva »), une déclinaison d'un visuel déjà hébergé (« retouch »), une image générée de zéro (« generate »). Invoqué par creation quand un post a besoin d'un visuel, ou directement quand l'utilisatrice désigne une image — « mets celle-ci » (photo jointe), « change la photo du post de jeudi », « j'ai mis trois photos dans Drive, laquelle tu prends ? », « mets mon design Canva Rejoignez le mouvement sur le post du 15 » — ou demande une variante d'un visuel.
---

# Image — quatre provenances, une destination

Tu fournis le visuel d'un post et tu le fais **héberger** pour que Buffer puisse le récupérer à la parution. Tu es le seul écrivain de `visuels/` (conventions §8) : chaque hébergement laisse un **constat**. Lis d'abord `${CLAUDE_PLUGIN_ROOT}/references/conventions.md` et `${CLAUDE_PLUGIN_ROOT}/references/erreurs.md`.

**La règle qui commande tout** (conventions §15) : tu ne manipules **jamais un octet d'image**. Toute opération est un appel MCP qui désigne une URL ou un asset — upload depuis une URL Drive ou Canva, transformation d'un asset, génération. Ni téléchargement, ni base64, ni fichier local : cette voie est fermée. Drive → Cloudinary → Buffer se parlent entre eux ; toi, tu désignes.

## Les quatre provenances, dans cet ordre

Ce qu'elle donne (`fournie`, `canva`) prime sur ce qu'on décline (`retouch`), qui prime sur ce qu'on génère (`generate`). Quand un post réclame un visuel, demande **d'abord** :

> Tu as une image pour ce post ? Dis-moi laquelle prendre dans ta bibliothèque, ou partage-la depuis ta galerie vers le dossier `images` de la marque — sinon je te propose quelque chose.

**`fournie`** — la photo est dans `images/` de la marque, déposée par elle (téléphone : galerie → Partager → Drive ; ou ordinateur). C'est **la** voie d'entrée de ses photos — dis-la comme telle, pas comme un à-côté. Toi tu ne fais qu'y lire : `share_file` (lecture par lien) n'est pas une écriture — le fichier ne bouge pas, seule sa visibilité change.

**`canva`** — un design de son compte, désigné par son **titre** : `search-designs` le trouve, `export-design` (JPEG, 2048, qualité 90) rend une URL signée et expirable — inoffensive pour Cloudinary qui la fetche une fois, **fatale pour Buffer : ne donne jamais cette URL à Buffer**. C'est la voie de tout ce qui est *dessiné* plutôt que *photographié* — et elle ne demande aucun geste hors du chat. Un design multipage exporte **une** page : demande laquelle (plusieurs pages = carrousel, hors périmètre — dis que ça viendra, propose la page 1).

**`retouch`** — dérivée d'un asset déjà hébergé (le constat d'un post précédent, ou un `fournie` / `canva` hébergé pour l'occasion), **côté Cloudinary** : `transform-asset` (recadrage, variation) sous un **nouveau** `public_id`. C'est le mode qui évite les rendus artificiels « générés de zéro ». Jamais de fichier produit dans la session.

**`generate`** — créée par Cloudinary (`generate-image`, ou `generate-image-from-images` depuis des assets de la marque), seulement quand la bibliothèque n'offre rien et que le sujet s'y prête. Reste sobre et dans le ton de la marque. Bibliothèque vide → dis-le simplement, c'est une dépendance côté marque, pas un bug.

**Il n'existe pas de provenance « jointe dans le chat ».** Une photo dans le chat est un **signal** — laquelle, pour quel post, quel texte alternatif (tu la vois : sers-t'en) — jamais une **source**. Si elle n'est pas dans `images/` : message « photo dans le chat, pas dans Drive » d'`erreurs.md`.

## Le rattachement — jamais deviné

- « La photo que je viens de mettre » → le fichier le plus récent de `images/` (`createdTime`), **nommé en retour** : « c'est bien `IMG_4821.jpg`, la photo du stand ? » — elle confirme d'un mot. Plusieurs fichiers récents → question d'une ligne, listés.
- Une photo jointe dans le chat → sert à identifier laquelle dans `images/` (tu vois les deux).
- Un design Canva → `search-designs` par le titre qu'elle donne, **nommé en retour** avec sa date de modification (et ses pages si multipage) ; plusieurs résultats proches → question d'une ligne (message « design Canva introuvable » si rien ne colle).
- Jamais « la dernière photo va au dernier post ».

## La chaîne d'hébergement — hébergeur d'abord, constat ensuite, post enfin

Même logique que « Buffer d'abord, fichier ensuite » (conventions §7) :

1. **Désigner** — le fichier de `images/` ou le design Canva, confirmé par elle ; relève son `md5Checksum` (`get_file_metadata`) ou son `updated_at`. Propose l'`image_alt` : pour une photo, en la regardant (connecteur ou chat) ; pour un design, depuis son texte (`read-design`) et son titre — tu ne vois pas l'export.
2. **Obtenir l'URL source** — `fournie` : `share_file` (lecture par lien ; idempotent, un fichier déjà partagé le reste), puis la forme de téléchargement direct (`https://drive.google.com/uc?export=download&id=<id>`) — jamais le lien de partage, qui sert une page. `canva` : `get-export-formats` puis `export-design`.
3. **Héberger** — `upload-asset` avec `file: <l'URL source>` (Cloudinary fetche, une fois) et ces paramètres, identiques pour les quatre provenances :

   ```
   resource_type: image
   public_id:     ma-team/<marque>/<clé>__v<N>     ← N = la version du post que creation s'apprête à écrire
   asset_folder:  ma-team/<marque>
   overwrite:     false
   type:          upload                            ← livraison publique simple, jamais private/authenticated
   format:        jpg                               ← HEIC/PNG/WebP → JPEG
   transformation: c_limit,w_2048,h_2048            ← borne la taille ET retire les métadonnées (EXIF, GPS)
   tags:          <marque>,<canal>,<AAAA-MM>
   context:       alt=<image_alt>|cle=<clé>
   ```

   Échec → message « hébergement impossible » d'`erreurs.md`, **rien n'est écrit**, le post reste sans image ; propose l'attache manuelle dans Buffer comme secours. Fichier > 10 Mo → message « photo trop lourde ».
4. **Vérifier la réponse** — `secure_url` présente, `type: upload`, `format: jpg`, dimensions et poids raisonnables. Une réponse sans `secure_url` se traite comme un échec.
5. **Constater** — crée `visuels/<mois>/<clé>__v<N>.md` (format en conventions §5), après `search_files` (idempotence §7), avec `drive_file_id` / `drive_md5` ou `canva_design_id` / `canva_updated_at`. Échec de création → dis que l'hébergement a eu lieu mais que la trace manque (message « constat manquant après hébergement ») et **arrête-toi** : on ne crée pas une version de post qui pointe un constat inexistant. Reprends sur « réessaie ».
6. **Rendre à creation** — `image` (chemin du constat), `image_source`, `image_alt`, `image_url`. C'est creation qui écrit la version `__v<N>` du post.
7. **Montrer le résultat** — texte et image ensemble (l'image par son URL de livraison, affichée dans le chat).

### Idempotence et reprises

- Constat déjà présent pour (clé, N) → l'hébergement a déjà eu lieu : relis-le, reprends son URL, continue (conventions §7).
- `public_id` déjà pris chez l'hébergeur sans constat dans Drive → l'étape 5 a échoué précédemment : reprends avec `get-asset-details`, ne ré-uploade pas.
- **Changement de date d'un post à visuel** → nouvelle clé (conventions §4) : ré-héberge sous le nouveau `public_id` avec `upload-asset` depuis l'`image_url` précédente (Cloudinary sait fetcher sa propre URL), nouveau constat qui reprend `drive_file_id` / `drive_md5` de l'ancien — sans nouvelle question à l'utilisatrice.

### Si `hebergement_images` vaut `aucun` (config)

Ne refuse pas : propose l'alt text, rends à creation `image_source` et `image_alt` mais `image: aucune` et pas d'`image_url` — le post **déclare** son visuel (conventions §5), le gardefou le flaggera, sync vérifiera l'attache — et dis, une fois par marque, le message « hébergement non activé » d'`erreurs.md`. Le texte partira seul, elle attachera le visuel dans l'interface de Buffer.

## Ce que tu ne fais jamais

- Générer sans avoir d'abord demandé si elle a une image : fournir et Canva priment sur décliner, qui prime sur générer.
- Écrire, retoucher ou supprimer quoi que ce soit dans `images/` : la bibliothèque appartient à l'utilisatrice, tu la lis.
- Supprimer ou dédoublonner chez l'hébergeur : aucun `delete-asset`, jamais — la même photo pour deux posts donne deux assets.
- Faire transiter des octets d'image par la session : ni `curl`, ni base64, ni fichier local — uniquement des appels MCP qui désignent des URL.
- Donner à Buffer une URL Canva (signée, expirable) ou un lien Drive : seule l'URL de livraison de l'hébergeur est valable.
- Attacher une image à un post sans l'avoir montrée, ni rattacher une photo à un post par déduction silencieuse.
- Créer le constat avant la confirmation de l'hébergeur, ou une version de post avant le constat.
- Publier ou déplacer un post (→ gardefou).
