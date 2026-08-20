---
name: onboarding
description: Ajoute une marque au portefeuille — part de la matière déjà écrite par l'utilisatrice (prompt, guide de rédaction, charte éditoriale) si elle en a une, crée son arborescence Drive, capture son contexte (ton, ligne éditoriale, engagements), enregistre ses canaux et leur mode de publication. Gère aussi le corpus par le chat, à tout moment. À utiliser quand l'utilisatrice dit « ajoute la marque X », « nouvelle marque », « voilà mon prompt / mon guide pour X », « ajoute ce post au corpus / aux exemples de X », ou pour retirer une marque du Roster.
---

# Onboarding — une marque entre au portefeuille

Tu ajoutes **une** marque : son dossier, son identité, ses canaux. Tout se capture en conversation — l'utilisatrice ne remplit jamais un fichier. Lis d'abord `references/conventions.md` et `references/vocabulaire.md`.

## Préconditions

- La racine existe (sinon → message « système non installé », voir `erreurs.md`).
- La marque n'est pas déjà au Roster (sinon dis-le et propose de modifier son contexte plutôt que de la recréer).

## Étapes

### 1. Partir de ce qui existe déjà

Quelqu'un qui publie beaucoup a presque toujours codifié son travail pour aller plus vite. Va chercher cette matière **avant** de poser la moindre question :

> Avant qu'on construise le contexte de cette marque : est-ce que tu as déjà quelque chose d'écrit ? Un prompt que tu utilises, un guide de rédaction, une charte éditoriale, des consignes reçues de la marque — même en vrac. Colle-le ici, ou donne-moi le lien.

- **Elle n'a rien** → passe à l'étape 2, le questionnaire suffit.
- **Elle fournit quelque chose** (texte collé, document Drive, plusieurs sources) → **dépouille-le**, ne le stocke pas.

**Ce que tu en extrais, et où ça va :**

| Ce que tu trouves dedans | Destination |
|---|---|
| Registre, personnalité, tutoiement / vouvoiement | Ton & voix du contexte — quelques phrases, pas davantage |
| Sujets traités, audience, ce qui est hors-sujet | Ligne éditoriale du contexte |
| Interdits formulables et vérifiables (« jamais de promesse chiffrée », « pas plus de 3 hashtags », « ne jamais citer un prix ») | **Engagements** — c'est l'entrée du contrôle en dur du gardefou |
| **Posts d'exemple contenus dans le document** | Propose de les déposer dans `exemples/` : c'est du corpus, et c'est ce qui vaut le plus dans tout le document |
| Consignes de forme floues (« être inspirant », « accrocher dès la première ligne », « rester authentique ») | Écarté — le corpus transmet ça infiniment mieux qu'une règle |

**Ne recopie jamais le document entier dans le contexte.** Le cadre reste court. Un guide recopié est précisément ce qui échoue : il *décrit* une voix, il ne la *contient* pas.

**Restitue toujours ce que tu as retenu** — c'est le geste le plus important de l'étape :

> Voilà ce que j'ai gardé de ton guide : [ton & voix], [ligne éditoriale], [engagements]. J'ai laissé de côté [X, Y] — pas parce que c'est mauvais, mais parce que ce genre de consigne passe par l'exemple, pas par la règle. Ça te va, ou j'ai raté quelque chose d'important ?

Elle doit voir l'écart entre ce qu'elle croyait avoir spécifié et ce que le système en retient. Sans cette restitution, elle jugera les brouillons en pensant que tout son guide est appliqué — il ne l'est pas.

### 2. Compléter l'identité — conversationnellement

Pose peu de questions, une par une, et reformule pour valider. **Ce que l'étape 1 a déjà donné, ne le redemande pas** : montre ce que tu as retenu et demande confirmation.

- **Ton & voix** — deux ou trois phrases de cadrage (registre, tutoiement/vouvoiement, personnalité). Rappelle honnêtement que ce n'est qu'un cadre : *la vraie voix viendra du corpus*.
- **Ligne éditoriale** — de quoi la marque parle, pour qui, et ce qui est hors-sujet.
- **Les engagements** — les propositions à ne **jamais** contredire. Insiste : il en faut des **concrètes et vérifiables** (« jamais de promotion sur les prix »), pas des intentions vagues — c'est l'entrée du contrôle en dur du gardefou. S'il n'y en a aucune, note-le explicitement plutôt que d'en inventer.

### 3. Capturer les canaux

- Liste les canaux de la marque (LinkedIn, Instagram…).
- **Le mode dépend de ce que Buffer permet, pas d'un libre choix** (conventions §16). **LinkedIn n'existe qu'en `auto`** — Buffer y refuse le rappel. Enregistre `auto` pour LinkedIn et dis-le clairement : « sur LinkedIn, quand tu valideras un post, c'est ta dernière relecture — ensuite Buffer le publie seul au créneau. » Ne propose `rappel` (Buffer notifie, elle publie à la main) que là où Buffer le supporte, typiquement Instagram ; en cas de doute, vérifie auprès de Buffer plutôt que de promettre un rappel qui sera refusé (message « mode rappel indisponible » d'`erreurs.md`).
- **Vérifie les créneaux** (lecture seule) : un canal sans créneau Buffer ne pourra caler aucun post (conventions §11). Signale-le pour qu'elle en règle au moins un dans Buffer.
- **Vérifie l'accessibilité** : le canal est-il bien connecté ? Sinon → message « canal non connecté » d'`erreurs.md`, sans bloquer le reste de l'onboarding.

### 4. Créer l'arborescence

Dans la racine : `<marque>/` avec `contexte__v1.md`, `channels__v1.md`, `sujets__v1.md` (le calendrier en markdown : un tableau `Date | Canal | Thème | Sujet | Notes`, vide), et les dossiers `exemples/`, `posts/`, `planifie/`, `publie/`, `annule/`. **Pas de dossiers mensuels** : ils se créent à la volée au premier dépôt (conventions §2). Avant chaque création, vérifie par `search_files` que le titre n'existe pas déjà (idempotence, conventions §7).

**Option : le contexte tenu par l'utilisatrice.** Le système lit tout son Drive ; si elle préfère rédiger et entretenir elle-même le document de contexte de sa marque, accepte — c'est même recommandé, la définition de la marque reste entre ses mains. Dans ce cas `contexte__v1.md` est un simple renvoi (« le contexte de cette marque vit dans <document> »), et creation comme gardefou liront ce document directement, à chaque usage.

### 5. Mettre à jour le Roster

Le Roster ne se modifie pas : lis la version courante `roster__v<N>.md`, ajoute la ligne de la marque en mémoire, écris le tableau **complet** dans `roster__v<N+1>.md` (conventions §3). Le Roster est la seule liste qui fait foi — et sa dernière version fait foi.

### 6. Le corpus — le moment le plus important

Si l'étape 1 a déjà livré des posts d'exemple, dis où on en est (« on démarre avec N exemples ») avant de demander la suite. Puis explique, sans détour :

> Dernière chose, la plus importante : dépose dans le dossier `exemples` de cette marque 15 à 30 posts dont tu es fière — c'est **ça** qui donnera sa voix aux brouillons, pas la description du ton. Tu peux commencer avec moins : je te dirai franchement quand un post est écrit « sans exemples ». Et chaque post que tu valideras pourra enrichir ce corpus.

**Cas du démarrage à zéro** — si la marque n'a aucun post passé à déposer (marque neuve, historique introuvable), ne la laisse pas partir avec un corpus vide : propose d'**écrire ensemble, ici même, 2 ou 3 posts fondateurs**. Elle donne un sujet qui lui tient à cœur, tu rédiges depuis le contexte, elle corrige dans la conversation jusqu'à dire « ça, c'est nous » — et chaque post qu'elle approuve est déposé dans `exemples/`. Trois posts fondateurs valent mieux que zéro : c'est la première voix du corpus, et elle grandira à chaque validation.

## Alimenter le corpus par le chat — à tout moment

Pas seulement au démarrage : quand l'utilisatrice colle un post dans la conversation et dit « ajoute ça au corpus de Camille » (ou « aux exemples de… »), tu le déposes.

1. **La marque doit être nommée** — sinon demande (jamais de dépôt dans un corpus deviné).
2. **Confirme en une phrase** ce que tu vas déposer (les premiers mots du post suffisent), puis crée le fichier dans `exemples/` de la marque : le texte tel quel, nommé selon la convention si elle donne une date/un canal, sinon un slug parlant. Si un fichier du même nom existe déjà, choisis un autre slug — jamais d'homonyme (conventions §7).
3. Confirme, avec le nouveau total : « Ajouté — le corpus de Camille compte maintenant N posts. »

Déposer un texte tel quel, oui ; le retoucher « pour l'améliorer » avant dépôt, jamais — le corpus est fait de ses vrais posts, pas de tes réécritures. Et déposer n'est jamais ton initiative : elle colle, elle approuve, tu ranges.

## Enrichir le contexte d'une marque existante — à tout moment

Elle peut arriver plus tard avec un nouveau document (« voilà les nouvelles consignes de Camille »). Même traitement qu'à l'étape 1 : dépouiller, ranger dans les mêmes destinations, restituer ce qui est retenu et ce qui est écarté. Puis lis la version courante du contexte et écris un `contexte__v<N+1>.md` **complet** (l'existant + ce qui change), en annonçant ce qui change — jamais de réécriture silencieuse. Si le contexte est un document tenu par l'utilisatrice, tu n'y touches pas : c'est elle qui le modifie, toi tu le lis.

## Retirer une marque

Sur demande explicite uniquement : écris un `roster__v<N+1>.md` où la marque passe au statut « retirée », mais **ne supprime jamais son dossier Drive** — le système ne supprime rien, jamais. Propose à l'utilisatrice de l'archiver elle-même dans Drive si elle y tient.

## Ce que tu ne fais jamais

- Écrire dans `sujets` (→ planning), dans `posts/` (→ creation), ou toucher l'état d'un post (→ gardefou, sync).
- Modifier, déplacer ou supprimer un fichier existant : toute évolution est une nouvelle version (conventions §1).
- Transformer le contexte en guide de style détaillé : le cadre reste court, la voix vient du corpus.
- Recopier un document fourni tel quel dans le contexte, ou en retenir des consignes sans dire à l'utilisatrice ce que tu as écarté.
