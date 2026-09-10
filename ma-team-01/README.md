# ma-team-01

Un plugin Claude qui rédige, contrôle et publie les posts sociaux d'un portefeuille de marques — piloté entièrement **en français, dans le chat**. Google Drive est la source de vérité unique ; Buffer publie en aval. Aucune application à héberger, rien à maintenir.

Conçu pour une opératrice non-technique : pas de fichier à éditer à la main, pas de message d'erreur technique, pas de geste quotidien obligatoire. Elle parle, relit, valide — le système fait le reste.

## Ce que ça fait

- **Rédige par analogie avec vos meilleurs posts** (le « corpus »), marque par marque — pas à partir d'une description abstraite de votre style. La qualité vient des exemples que vous fournissez, et s'améliore à chaque post validé.
- **Retient vos corrections durables** : « celui-ci, plus court » retouche un post ; « toujours trop long » — avec votre accord explicite — devient un retour appliqué à tous les posts suivants de la marque. Vous ne répétez pas deux fois la même remarque.
- **Gère plusieurs marques en isolation totale** : chaque marque a son dossier, sa voix, ses canaux. Un garde-fou traite le pire risque — publier sur le mauvais compte — comme une erreur de classe désastre.
- **Travaille par batch, à la demande** : « Génère les posts de septembre pour Camille » produit tous les brouillons d'un coup ; vous validez en bloc les posts sûrs et un par un les posts signalés.
- **Publie via Buffer** à la date prévue par votre calendrier ; l'heure vient des créneaux Buffer de chaque canal.

## Prérequis

| Prérequis | Détail |
|---|---|
| **Claude** avec support des plugins | Claude Cowork (ou Claude Code) — l'interface de chat où tout se passe. |
| **Google Drive** connecté | Activez le connecteur Google Drive intégré de Claude (réglages → connecteurs), une fois. Drive héberge toute la donnée : contextes, calendriers, corpus, posts. |
| **Buffer** connecté à vos réseaux | Le plugin déclare le serveur MCP officiel de Buffer (`mcp.buffer.com`) : à l'installation, approuvez-le et connectez votre compte Buffer, une fois. Chaque canal (LinkedIn, Instagram…) doit être connecté à Buffer, avec ses créneaux de publication réglés. |
| **Comptes Instagram Business/Créateur** *(si Instagram en mode auto)* | Buffer ne peut auto-publier sur Instagram que sur ces types de comptes. |
| **Le connecteur Cloudinary** *(recommandé — pour les visuels)* | Gratuit, autorisé une fois (réglages → connecteurs). C'est lui qui met vos photos en ligne pour que Buffer les attache automatiquement aux posts. Sans lui, rien ne casse : vous attachez les visuels vous-même dans Buffer au moment de la validation. |
| **L'app Google Drive sur le téléphone** | C'est le geste photo : galerie → Partager → Drive → dossier `images` de la marque. Trois gestes, dans une app que vous avez déjà. |
| **Le connecteur Canva** *(optionnel)* | Si vous dessinez vos visuels dans Canva : autorisé une fois, le système va chercher vos designs par leur titre — « mets mon design *Rejoignez le mouvement* sur le post du 15 » — sans aucun geste hors du chat. |
| **Vos meilleurs posts** | ~15–30 posts validés par marque, à déposer dans son dossier `exemples`. **C'est le levier de qualité n°1** — sans eux, les posts seront corrects mais passe-partout (et le système vous le dira franchement). |

Aucun de ces prérequis n'est un geste répété : les autorisations et connexions se font une fois.

## Installation

Depuis le marketplace GitHub :

```
/plugin marketplace add alinusgallus/ma-team-01
/plugin install ma-team-01@ma-team-01
```

*(Dépôt privé : votre environnement doit avoir accès au dépôt GitHub.)*

## Mise en route

1. **Installer le système** — dites : « installe le système ». Le skill *setup* crée le dossier racine dans votre Drive (« Ma Team » par défaut), la configuration globale et le Roster des marques. Une seule fois.
2. **Ajouter une marque** — dites : « ajoute la marque Camille ». Le skill *onboarding* vous pose quelques questions (ton, ligne éditoriale, engagements à ne jamais contredire), enregistre les canaux et leur mode de publication, et crée l'arborescence de la marque.
3. **Déposer le corpus** — glissez vos 15–30 meilleurs posts dans le dossier `exemples` de la marque, ou collez-les simplement dans le chat (« ajoute ça au corpus de Camille »). Vous pouvez commencer avec moins — et même à zéro : le système propose alors d'écrire ensemble 2 ou 3 posts fondateurs.
4. **Prévoir des sujets** — dites : « je veux prévoir un post pour l'événement du 15/09 ». Le skill *planning* remplit le calendrier ; vous ne saisissez jamais rien dans le tableau.
5. **Produire et publier** — dites : « génère les posts de septembre pour Camille », relisez le batch, validez. Les posts partent dans Buffer à leur date.

## Les huit skills

| Skill | Rôle |
|---|---|
| **setup** | Installation globale, une seule fois : racine Drive, config, Roster. |
| **onboarding** | Ajoute une marque : arborescence, contexte, canaux et mode de publication. |
| **planning** | Remplit le calendrier éditorial (`sujets`) à partir de ce que vous dites. |
| **creation** | Rédige les brouillons — par batch depuis le calendrier, ou une idée à l'unité — par analogie avec le corpus. |
| **image** | Fournit le visuel et le met en ligne pour Buffer : d'abord vos propres photos (bibliothèque `images`) ou vos designs Canva, sinon une déclinaison, sinon généré de zéro. |
| **gardefou** | Contrôle chaque post (bonne marque ? engagement respecté ? image ?), puis publie sur votre validation. Seul écrivain de l'état des posts. |
| **revue** | Présente un batch d'un coup, en lecture seule : posts sûrs groupés, posts signalés un par un. |
| **sync** | Confirme ce qui est réellement paru et construit l'historique (`publie/`, `annule/`). |

## Ce que Drive contient

```
Ma Team/
  config__v1.md, roster__v1.md  ← global
  <marque>/
    contexte__v1.md             ← ton, ligne éditoriale, engagements
    channels__v1.md             ← canaux → mode auto / rappel
    sujets__v1.md               ← calendrier : Date | Canal | Thème | Sujet | Notes
    retours__v1.md              ← vos corrections durables (« toujours plus court »…)
    exemples/                   ← votre corpus (le levier de qualité)
    images/                     ← vos photos (la bibliothèque — vous seule y déposez)
    visuels/                    ← les traces d'hébergement des visuels (le système y écrit)
    posts/  planifie/  publie/  annule/   ← le cycle de vie, par mois (2026-09/…)
```

Rien n'est jamais réécrit ni supprimé : toute évolution est une **nouvelle version** (`__v2`, `__v3`…), et l'état d'un post se lit à sa présence dans les dossiers.

Drive fait foi ; Buffer n'est qu'un canal de sortie. Vous n'ouvrez ces dossiers presque jamais — sauf `exemples`, pour y déposer vos meilleurs posts, et `images`, pour y déposer vos photos.

## À savoir, honnêtement

- **La qualité dépend du corpus.** Donnez des exemples dont vous êtes fière et le système écrit dans votre voix ; donnez seulement une description de ton et vous obtiendrez un imitateur plausible. Le système vous prévient quand il écrit « sans exemples ».
- **Relire un batch reste du temps.** Le système supprime la rédaction, pas la relecture — un gros batch fait des dizaines de posts. C'est vous qui choisissez quand.
- **Les sujets restent votre rôle.** Le système n'invente jamais un sujet : un calendrier vide ne génère rien. (Mais vous le remplissez en parlant.)
- **Une photo pour un post = un partage vers Drive.** Depuis votre téléphone : galerie → Partager → Drive → dossier `images` de la marque, puis dites-le dans le chat — le système la met en ligne, vous montre le post avec son image, et Buffer reçoit texte et visuel ensemble. À savoir : dès qu'une photo est utilisée pour un post, elle devient visible sur internet, même si le post ne part jamais ; et si vous retouchez dans Drive une photo déjà utilisée, le système vous préviendra avant l'envoi. Sans le connecteur Cloudinary, le visuel s'attache dans Buffer au moment de la validation, guidé — comme avant.
- **Mode « rappel » Instagram = gestes manuels** sur le téléphone, avec en contrepartie le recadrage et le tagging natifs.
- **Limite Buffer** : 25 posts par jour et par canal — le système vérifie avant d'envoyer.
