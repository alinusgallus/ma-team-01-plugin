# Vocabulaire — le langage commun des huit skills

*Tous les skills emploient exactement ces mots, en français, face à l'utilisatrice.*

- **L'utilisatrice** — la cliente et l'opératrice du système. Seule juge de la qualité. Non-technique : tout se passe dans le chat, jamais dans un fichier à éditer à la main. On s'adresse à elle par « tu ».
- **Marque** — un compte qu'elle gère (le sien, ou celui d'une marque cliente — dans ces documents, « Camille » est l'exemple fictif). Isolation totale entre marques : la voix de l'une ne déborde jamais chez l'autre. *Ne jamais dire « client » pour une marque.*
- **Roster** — la liste des marques gérées. Vit à la racine Drive ; jamais re-déduite à la volée.
- **Canal** — une destination de publication d'une marque (ex. le LinkedIn de Camille). Une marque a un ou plusieurs canaux. *Ne pas dire « compte ».*
- **Intégration** — Buffer, le puits de publication en aval. Drive reste la source de vérité ; Buffer ne stocke rien qui fasse foi.
- **Plan / fichier `sujets`** — le calendrier éditorial d'une marque : un fichier markdown versionné (`sujets__v<N>.md`), une ligne par futur post (`Date | Canal | Thème | Sujet | Notes`). La **Date est la date de parution voulue**. Rempli par le skill planning, jamais à la main ; chaque modification est une nouvelle version complète.
- **Batch** — l'unité de production et de relecture : un filtre sur les lignes du Plan par période, marque et/ou thème. Déclenché à la demande — il n'existe aucun traitement quotidien.
- **Corpus** — le dossier `exemples/` d'une marque : ses ~15–30 meilleurs posts, validés. **C'est le corpus qui EST la voix** ; le contexte ne fait que poser le cadre.
- **Contexte** — le fichier `contexte__v<N>.md` d'une marque (ou un document qu'elle tient elle-même et que le système lit) : ton, ligne éditoriale, engagements. Un cadre et des contraintes — pas un modèle de style.
- **Engagements** — les propositions non négociables d'une marque, vérifiées en dur par le gardefou. Une contradiction est l'erreur la plus grave que le système puisse commettre.
- **Clé d'un post** — le triplet `date_canal_slug`, invariant à travers `posts/`, `planifie/` et `publie/`. C'est elle, et elle seule, qui relie les trois dossiers.
- **Version** — le suffixe `__v<N>` des enregistrements qui évoluent (posts, calendrier, contexte…). La plus haute fait foi ; les précédentes sont l'historique. Rien n'est jamais réécrit : toute modification est une nouvelle version complète.
- **États d'un post** — jamais stockés, toujours déduits de la présence de sa clé dans les dossiers (conventions §6) : dans `publie/` = **publié** ; sinon dans `planifie/` = **programmé** (envoyé à Buffer) ; sinon = **brouillon**. *Toujours dire « brouillon », pas « draft ».*
- **Check-state (flag)** — le verdict du gardefou par post : `safe` ou `flagged` + raison. Recalculé à chaque passage et restitué en conversation ; il ne se fige que dans le fichier `planifie/` au moment de l'envoi, comme trace historique.
- **Mode de publication** — par canal : `auto` (Buffer publie seul — il faut une image à URL publique) ou `rappel` (Buffer notifie l'utilisatrice, elle publie à la main).
