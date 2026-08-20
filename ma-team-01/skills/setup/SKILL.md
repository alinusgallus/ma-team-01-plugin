---
name: setup
description: Installation initiale du système, une seule fois — crée le dossier racine dans Google Drive, la configuration globale et le Roster des marques. À utiliser quand l'utilisatrice dit « installe le plugin Ma Team », « installe Ma Team », « installe le système », « mets en place le système », « démarre », « initialise », ou quand aucune racine n'existe encore dans son Drive.
---

# Setup — l'installation globale, une seule fois

Tu établis la couche **globale** du système : la racine Drive, la config, le Roster. Tu ne crées **aucun** dossier de marque — c'est le rôle d'onboarding (une marque à la fois). Lis d'abord `references/conventions.md` et `references/vocabulaire.md` du plugin. Réponds court : confirme, propose la suite, sans disserter (conventions §17).

## Préconditions

1. **Drive accessible ?** Si le MCP Google Drive n'est pas autorisé, ne tente rien : explique en une phrase qu'il faut autoriser Google Drive dans les réglages de Cowork, et attends qu'elle te dise « c'est fait ».
2. **Déjà installé ?** Cherche une racine existante (dossier contenant `config__v<N>.md` + `roster__v<N>.md`). Si elle existe : dis-le, résume son contenu (marques du Roster), et **arrête-toi** — le setup est strictement idempotent, il ne recrée ni n'écrase jamais rien.

## Étapes

1. **Demander le nom du dossier racine avant de rien créer** : « Comment veux-tu appeler le dossier qui contiendra tout ? Je propose "Ma Team" par défaut. » Attends sa réponse (elle peut garder le défaut). Puis **crée le dossier racine** sous ce nom — après avoir vérifié par `search_files` qu'un dossier de ce titre n'existe pas déjà (conventions §7).
2. **Vérifier Buffer** (lecture seule) : le MCP Buffer est-il autorisé ? Si non, signale simplement — sans bloquer — que l'autorisation restera à faire avant la première publication.
3. **Créer `config__v1.md`** : nom de la racine, date d'installation, langue (français), l'état de l'Intégration (Buffer autorisé ou non), et `hebergement_images: aucun` (conventions §15 — aucun hébergement public d'images au départ). Une évolution ultérieure de la config s'écrira dans `config__v2.md` — on ne réécrit jamais un fichier (conventions §3).
4. **Créer `roster__v1.md`** : le tableau des marques, vide pour l'instant (format en conventions §9).

## À la fin

Confirme en clair ce qui existe désormais, et propose la suite naturelle :

> C'est installé : ton dossier « Ma Team » est créé dans Drive, avec un Roster vide.
> Prochaine étape : ajouter ta première marque — dis-moi par exemple « ajoute la marque Camille ».

## Ce que tu ne fais jamais

- Créer un dossier de marque (→ onboarding).
- Piloter une connexion ou une autorisation OAuth : c'est l'utilisatrice qui le fait, une fois, dans l'interface de Cowork.
- Recréer ou écraser une racine existante — ni créer un fichier dont le titre existe déjà dans le dossier cible.
