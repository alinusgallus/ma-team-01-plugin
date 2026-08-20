# Conventions — la structure que tous les skills respectent

*Les invariants partagés. Un skill ne les redéfinit jamais localement : il lit ce fichier.*

## 1. Principe directeur — le stockage est additif

Le connecteur Google Drive sait **créer et lire**. Il ne sait ni modifier un fichier existant, ni le déplacer, ni le renommer, ni le supprimer — et aucune option ne le débloque. Toute évolution — une correction de post, un ajout au calendrier, un changement d'état — s'exprime donc par la **création d'un nouveau fichier**. Rien n'est jamais réécrit.

Deux mécanismes en découlent, et ils suffisent à tout :

- **Le versionnage** (§3) pour ce qui évolue en place : un post retravaillé, le calendrier, le contexte.
- **La présence dans un dossier** (§6) pour ce qui progresse dans un cycle : envoyé, paru.

Deux comportements du connecteur à connaître :

1. Créer un fichier portant un titre déjà présent dans le même dossier ne remplace rien : Drive crée un **homonyme** avec un identifiant différent. Dans un système dont la clé est le nom, c'est une corruption silencieuse — d'où la règle d'idempotence (§7).
2. Copier un fichier vers un autre dossier **laisse l'original en place**. Il n'existe aucun moyen de finir un déplacement.

## 2. Arborescence Drive (source de vérité unique)

```
<racine « Ma Team »>/                       ← créée par setup
  config__v1.md
  roster__v1.md
  <marque>/                                 ← créé par onboarding
    contexte__v1.md
    channels__v1.md
    sujets__v1.md                           ← le calendrier, en markdown
    exemples/                               ← le corpus, alimenté par l'utilisatrice
    posts/                                  ← tout ce qui a été écrit, versionné
      2026-09/                              ← dossier mensuel : AAAA-MM
    planifie/
      2026-09/
    publie/
      2026-09/
    annule/                                ← posts retirés de Buffer après envoi
      2026-09/
```

- La racine s'appelle « Ma Team » par défaut (l'utilisatrice peut choisir un autre nom au setup). Tous les skills la localisent via le Roster/config — jamais par recherche de nom dans Drive. Si la racine n'existe pas : « On dirait que le système n'est pas encore installé — lance d'abord le setup. »
- **Le dossier mensuel est indexé sur la date de parution voulue**, pas sur la date de création. Un post écrit le 8 août pour le 10 septembre va dans `2026-09/`. Les quatre dossiers `posts/`, `planifie/`, `publie/`, `annule/` répliquent la même structure mensuelle.
- **Création paresseuse** : un dossier mensuel n'est créé qu'au moment d'y déposer le premier fichier — après vérification d'existence (§7).
- `posts/` contient **tout ce qui a été écrit**, y compris ce qui est déjà paru — le nom dit la vérité à l'utilisatrice.
- Le calendrier `sujets` est un fichier **markdown versionné**, pas un Google Sheet : le connecteur sait créer un Sheet, pas y écrire une ligne. Le markdown reste lisible par l'utilisatrice dans Drive.

## 3. Règle de version — universelle

Tout enregistrement susceptible d'évoluer porte un suffixe :

```
<nom>__v<N>.md
```

`N` est un entier commençant à 1, sans zéro de tête. **Le fichier de rang le plus élevé fait foi.** Les précédents sont conservés : ils constituent l'historique.

Pour résoudre la version courante : `search_files` avec le `parentId` du dossier, filtrer les titres commençant par `<nom>__v`, extraire `N`, retenir le maximum. **Ne jamais se fier à `modifiedTime`** : une lecture, une conversion ou une intervention manuelle peuvent le déplacer. Le numéro dans le titre est la seule autorité.

**Règle de réécriture** : une nouvelle version est un fichier **complet**, jamais un delta. On lit la version courante, on applique la modification en mémoire, on écrit l'ensemble dans `__v<N+1>`.

Cette règle s'applique à : `config`, `roster`, `contexte`, `channels`, `sujets`, et les posts de `posts/`. Elle ne s'applique **pas** aux fichiers de `planifie/` et `publie/`, uniques par post (§5).

## 4. Nommage des posts

```
AAAA-MM-JJ_canal_slug__v<N>.md
```

`AAAA-MM-JJ` est la **date de parution voulue**. Exemple : `2026-09-10_linkedin_lancement-produit-x__v2.md`.

Le triplet `date_canal_slug` est la **clé d'identité du post**. Il est invariant à travers `posts/`, `planifie/` et `publie/` : c'est lui, et lui seul, qui permet de rapprocher les trois dossiers.

**Si la date de parution change, la clé change**, donc le dossier mensuel aussi. Traiter ce cas comme la création d'un nouveau post (nouvelle clé, `__v1`), et signaler à l'utilisatrice que l'ancien reste en place.

## 5. Format des fichiers

### `posts/<mois>/<clé>__v<N>.md` — écrit par creation

```
---
marque: camille
canal: linkedin
date: 2026-09-10            ← date de parution voulue
theme: lancement-produit-x
sujet: retour d'expérience sur le lancement
image: aucune | <chemin Drive du visuel archivé>
image_alt: <texte alternatif, si image — écrit par creation>
image_url: <URL publique, seulement si un hébergement est configuré (§15) — sinon absent>
exemples_utilises: <noms des posts du corpus mobilisés, ou « aucun — écrit sans exemples »>
version: 2
---

<le texte du post, prêt à publier>
```

Pas de champ `check` : le verdict du gardefou ne se stocke pas dans un brouillon (§6).

### `planifie/<mois>/<clé>.md` — écrit par gardefou, **après** confirmation de Buffer

Fichier unique, sans suffixe de version. Sa seule présence atteste que le post est parti.

```
---
marque: camille
canal: linkedin
date: 2026-09-10
theme: lancement-produit-x
version_source: 2                    ← quelle version de posts/ a été envoyée
image: aucune | <chemin Drive du visuel>
image_alt: <texte alternatif, si image>
image_url: <URL envoyée à Buffer, si hébergement — sinon absent>
check: safe | flagged
check_raison: <en clair, si flagged et validé quand même>
buffer_id: <identifiant renvoyé par Buffer>
envoye_le: 2026-08-08T14:32:00Z
---

<le texte exact envoyé à Buffer>
```

Le texte est recopié intégralement, et les champs image avec lui : ce fichier est la **preuve de ce qui est parti**, indépendamment des versions ultérieures dans `posts/`. Si le visuel doit être attaché dans Buffer (§15), le fichier dit quel visuel était prévu — c'est sync qui vérifiera qu'il y est.

### `publie/<mois>/<clé>.md` — écrit par sync, après confirmation de Buffer

Mêmes champs, plus :

```
publie_le: 2026-09-10T09:15:00Z
```

### `annule/<mois>/<clé>.md` — écrit par sync (ou sur constat de l'utilisatrice)

Fichier unique, sans suffixe de version. Sa présence atteste qu'un post **envoyé à Buffer n'y est plus** : supprimé dans Buffer, ou introuvable à l'interrogation par `buffer_id`. Il prime sur `planifie/` dans la déduction d'état (§6).

```
---
marque: camille
canal: linkedin
date: 2026-09-10
theme: lancement-produit-x
buffer_id: <l'identifiant qui n'est plus reconnu>
envoye_le: 2026-08-08T14:32:00Z      ← repris du fichier planifie/
annule_le: 2026-08-09T10:05:00Z
constate_par: sync | utilisatrice
raison: <en clair, si connue — supprimé dans Buffer, rejeté par le canal…>
---
```

Le post reste un **brouillon** dans `posts/` (rien n'est supprimé). Pour le remettre en file, on change sa date — donc sa clé (§4) — et on repart d'un `__v1` : recréer sous la même clé fabriquerait un homonyme (§7).

### Enregistrements versionnés (`config`, `roster`, `contexte`, `channels`, `sujets`)

Format libre en markdown, avec un en-tête minimal :

```
---
version: 3
remplace: roster__v2.md
---
```

## 6. L'état ne se stocke pas — il se déduit

Il n'existe nulle part de champ « statut ». L'état d'un post se calcule à partir de sa présence dans les dossiers, en interrogeant sa clé `date_canal_slug` :

| Condition | État |
|---|---|
| présent dans `publie/<mois>/` | **publié** |
| sinon, présent dans `annule/<mois>/` | **annulé** |
| sinon, présent dans `planifie/<mois>/` | **programmé** |
| sinon | **brouillon** |

L'ordre est strict : le dossier le plus avancé gagne. `publie/` prime sur tout ; `annule/` prime sur `planifie/` (un post retiré de Buffer n'est plus « programmé »).

**Pourquoi `annule/` existe.** Buffer peut supprimer ou refuser un post *après* son envoi — l'utilisatrice efface une entrée dans Buffer, un canal rejette la parution. La seule présence de `planifie/` dirait alors « programmé » à tort : le fichier atteste l'envoi, pas la persistance en file. Deux garde-fous complémentaires corrigent ça :
- **sync réconcilie** : pour chaque post programmé, il interroge Buffer par `buffer_id` ; si Buffer ne le connaît plus et que le post n'est pas paru, sync crée `annule/<mois>/<clé>.md`. L'état déduit redevient exact.
- **revue vérifie à la lecture** : avant d'afficher un post comme « programmé », revue interroge Buffer par `buffer_id` et signale tout écart (Drive dit programmé, Buffer ne le connaît plus → « à réconcilier, lance sync »). Un « programmé » non réconcilié n'est jamais présenté comme certain.

**Le verdict du gardefou (`safe` / `flagged`) ne se stocke pas non plus** pour un brouillon. Il est recalculé à chaque passage : un post modifié n'hérite jamais de son ancien contrôle. Le verdict n'est figé qu'au moment de l'envoi, dans le fichier `planifie/`, où il devient une trace historique.

**Pour connaître l'état réel d'un post programmé** (toujours en file ? publié ? supprimé côté Buffer ?), interroger Buffer avec le `buffer_id`. Drive dit ce qui a été envoyé ; Buffer dit ce qu'il en advient.

## 7. Règles d'écriture — impératives

- **Idempotence avant toute création.** Le connecteur crée des homonymes sans avertir. Avant chaque `create_file`, vérifier par `search_files` que le titre n'existe pas déjà dans le dossier cible. Le fichier existe déjà → l'opération a déjà eu lieu, ne pas recréer, continuer. Cette règle vaut pour les dossiers comme pour les fichiers.
- **Ordre des opérations à l'envoi : Buffer d'abord, fichier ensuite.** Le fichier `planifie/` atteste d'un fait accompli, il ne le prescrit pas. Si Buffer refuse, aucun fichier n'est créé et le post reste un brouillon — l'état déduit reste exact sans compensation. Même ordre pour sync : confirmer auprès de Buffer, puis créer dans `publie/`.
- **Jamais de copie pour faire progresser un post.** Utiliser `create_file`, pas `copy_file` : la copie ne permet pas d'écrire l'en-tête enrichi (`buffer_id`, `check`, `envoye_le`). `copy_file` reste utilisable pour dupliquer un exemple ou archiver, jamais pour le cycle de vie.
- **Aucune suppression, aucun ménage automatique.** Le système ne supprime rien et ne le propose pas comme une action qu'il exécuterait. Si l'utilisatrice demande du ménage : lui lister les fichiers de `posts/` dont la clé existe aussi dans `publie/` — ceux-là sont sans risque — et lui expliquer qu'elle les met à la corbeille elle-même dans Drive.
- **Homonymes déjà présents.** Si `search_files` renvoie deux fichiers de même titre dans un dossier (corruption antérieure), ne pas choisir silencieusement : signaler à l'utilisatrice et s'arrêter sur ce post.

## 8. Le contrat « un seul écrivain »

Chaque emplacement n'a qu'un écrivain — et le geste est toujours **créer**, jamais modifier :

| Élément | Écrit par | Lu par |
|---|---|---|
| `config`, `roster` | setup, onboarding | tous |
| `contexte`, `channels` | onboarding — **ou l'utilisatrice à la main** (voir ci-dessous) | creation, gardefou |
| `sujets` | planning | creation |
| `exemples/` (corpus) | l'utilisatrice ; onboarding quand elle colle un post dans le chat | creation |
| `posts/` | creation | tous |
| `planifie/` | gardefou | revue, sync |
| `annule/` | sync | revue |
| `publie/` | sync | revue |

La **revue** est une vue en lecture seule : elle déduit l'état (§6), elle n'écrit rien.

Le scope lecture du connecteur couvrant tout le Drive, **le contexte d'une marque peut être un document que l'utilisatrice rédige et modifie elle-même** — le système se contente de le lire. Option recommandée : elle contourne la limite d'écriture et rend la définition de la marque à sa propriétaire. Onboarding doit accepter les deux cas : un `contexte__v<N>.md` qu'il a créé, ou un document désigné par l'utilisatrice.

## 9. Roster (`roster__v<N>.md`)

| Marque | Dossier | Canaux | Statut |
|---|---|---|---|
| camille | `<racine>/camille/` | linkedin (auto), instagram (rappel) | active |

Toute mise à jour est une nouvelle version complète du tableau (§3).

## 10. Batch

Un batch = un filtre sur les lignes du fichier `sujets` : par **période** (« septembre »), **marque** (« tout Camille »), **thème** (« la série lancement »), ou une combinaison. Il n'existe aucun traitement quotidien ni planifié : l'utilisatrice déclenche.

## 11. Date, heure et créneaux de publication

On pousse à Buffer la **date** (colonne Date). L'**heure** est décidée par Buffer via ses **créneaux par canal** — les skills ne calculent jamais d'horaire, ils prennent le créneau de Buffer (dans le **fuseau réglé côté Buffer**, pas celui de l'utilisatrice).

Deux conséquences concrètes, vérifiées en conditions réelles :

- **Un jour sans créneau ne peut pas recevoir de post.** Si la date voulue tombe un jour où le canal n'a aucun créneau Buffer, l'envoi échoue tel quel. Ne jamais forcer une heure : signaler (message « jour sans créneau » d'`erreurs.md`) et proposer le **créneau le plus proche** existant, ou laisser l'utilisatrice ajouter un créneau dans Buffer. Le gardefou fait ce contrôle avant de pousser ; planning peut le signaler dès la prévision.
- **Plafond** : avant de pousser un batch, vérifier qu'aucun canal ne dépasse **25 posts sur un même jour** (message « plafond Buffer »).

## 12. Authentification

Les autorisations MCP (Drive, Buffer) sont faites une fois par l'utilisatrice dans l'interface de Cowork. **Aucun skill ne pilote une connexion.** Les skills vérifient seulement l'accessibilité et produisent un message clair si un canal n'est pas connecté (voir `erreurs.md`).

## 13. Le corpus (`exemples/`)

- **Un exemple = un fichier** dans `exemples/` de la marque : le texte publié tel quel (Google Doc ou markdown), idéalement nommé `AAAA-MM-JJ_canal_slug` comme les posts. Pas de méta obligatoire — le texte suffit.
- **Seuils partagés** (utilisés par creation) : **0 exemple** = corpus vide → écriture contexte seul + avertissement franc ; **1–4** = corpus maigre → analogie partielle + mention « corpus encore maigre » ; **≥ 5** = corpus utile ; **15–30** = la cible.
- **Qui alimente** : l'utilisatrice, en déposant ses meilleurs posts dans Drive **ou en les collant dans le chat** (« ajoute ça au corpus de Camille » → onboarding les range) ; onboarding, via les posts fondateurs co-écrits au démarrage à zéro ; sync, en proposant les posts publiés dont elle est fière. Dans tous les cas c'est **elle qui bénit** — « validé » ne veut pas dire « exemplaire ».
- Aucun skill ne supprime ni ne réécrit un exemple.

## 14. Langue

Tout ce que l'utilisatrice lit est en **français**, dans le vocabulaire de `vocabulaire.md`. Jamais de message technique brut : toute erreur passe par le gabarit d'`erreurs.md`.

## 15. Images — comment un visuel arrive dans Buffer

Deux faits techniques commandent tout le reste :

1. Buffer n'a **pas d'envoi de fichier** : un média ne se référence que par **URL publique directe** (HTTPS, sans authentification, pointant sur le fichier — pas une page de prévisualisation). **Un lien Drive n'est jamais une URL de média valable.**
2. Buffer ne copie pas le média à la création du post : il **va le chercher au moment de la parution** — des semaines plus tard pour un batch. Une URL morte entre-temps casse le post, alors que le fichier `planifie/` atteste un envoi réussi. À l'inverse, un média déposé **dans l'interface de Buffer** est stocké par Buffer : aucune fragilité à la parution.

La config globale porte la clé **`hebergement_images`** (dans `config__v<N>.md`) :

- **`aucun`** (défaut) — le visuel est archivé dans Drive à côté du post ; le **texte part seul** à Buffer, et l'utilisatrice attache le visuel **dans l'interface de Buffer**, au moment où elle valide le post — guidée par le gardefou. C'est le mode prévu tant que les posts à visuel restent minoritaires (LinkedIn).
- **Un hébergeur nommé** (prévu pour la phase Instagram) — le skill image dépose le visuel chez l'hébergeur et renseigne `image_url` : une URL publique et **stable** (jamais une URL signée ou expirable). Le gardefou vérifie qu'elle répond sans authentification, puis l'envoie **dans le même push** que le texte.

Dans tous les cas : `image` (archive Drive) et `image_alt` (texte alternatif, écrit par creation dans la langue de la marque) sont renseignés dans le post et recopiés dans `planifie/`. Et **sync** vérifie côté Buffer que les posts déclarant un visuel en portent bien un — y compris ceux encore en file, pour rattraper un oubli avant la date de parution.

## 16. Modes de publication par canal (selon ce que Buffer permet)

Chaque canal a un **mode**, mais le choix **dépend de ce que Buffer autorise pour ce type de canal** — ce n'est pas libre :

- **`auto`** — Buffer publie seul au créneau. C'est le **seul mode disponible pour LinkedIn** : Buffer y refuse la notification (« Notification scheduling is not supported for linkedin channels »). Conséquence à énoncer clairement à l'utilisatrice : sur un canal `auto`, **la validation au gardefou est le dernier regard humain** — ensuite le post part au créneau, sans relecture avant parution.
- **`rappel`** — Buffer notifie l'utilisatrice, qui publie à la main. Disponible **uniquement là où Buffer le supporte** (typiquement **Instagram** en compte personnel / certains canaux). Utile quand elle veut un geste manuel (recadrage, tag natif).

À l'onboarding, ne proposer `rappel` que sur un canal qui le supporte ; pour LinkedIn, enregistrer `auto` sans faire miroiter un rappel qui n'existe pas. En cas de doute, vérifier auprès de Buffer (lecture seule) — un `rappel` refusé se traite avec le message « mode rappel indisponible » d'`erreurs.md`.

## 17. Concision — répondre court

L'utilisatrice pilote ; elle n'a pas besoin d'un rapport ni d'un cours. Chaque réponse est **courte et actionnable** :

- **Confirme en une ou deux phrases** ce qui a été fait, puis propose la suite en une ligne. Pas de préambule, pas de récapitulatif du modèle de stockage, pas de justification de l'architecture.
- **N'explique un mécanisme que si elle le demande**, ou si un choix a une conséquence qu'elle doit connaître pour décider (ex. « sur LinkedIn, valider = publier »). Dans ce cas, une phrase suffit.
- **Une question à la fois**, formulée simplement. Quand une recommandation claire suffit, ne déroule pas toutes les options.
- Les tableaux et récaps chiffrés servent une **revue** de batch ; ailleurs, ils alourdissent. Va à l'essentiel.

Exception : la **restitution** en onboarding (ce qui a été retenu / écarté d'un guide) reste explicite — mais tenue, en puces courtes, pas en dissertation.

## 18. Économie d'appels — lire par lot

La lenteur vient des allers-retours, pas du volume. Trois règles, valables pour tous les skills :

- **Une seule liste par dossier et par batch.** Pour résoudre les versions (§3) et vérifier l'idempotence (§7) sur plusieurs fichiers d'un même dossier, ne fais pas une recherche par titre : liste le dossier une fois (`search_files` avec le `parentId`, sans filtre de titre) et déduis tout de cette liste — versions courantes, existences, homonymes. Une recherche par fichier sur un batch de vingt posts, c'est vingt allers-retours pour ce qu'une liste donne en un.
- **Buffer s'interroge par canal, pas par post.** Pour vérifier des posts programmés (revue, sync), une requête de **liste** par canal couvre tout le lot : rapprocher les `buffer_id` des fichiers `planifie/` de cette liste. Ne redescendre à l'interrogation individuelle par `buffer_id` que pour les posts absents de la liste (candidats à l'annulation) — pour confirmer avant de conclure.
- **Les lectures indépendantes se font en parallèle** : le corpus d'une marque, les quatre dossiers d'état d'un mois, les contextes de plusieurs marques d'un batch — rien n'impose de les lire l'un après l'autre.

Aucune de ces règles ne change ce qui est écrit ni quand : elles ne touchent que le nombre d'appels pour lire la même vérité.
