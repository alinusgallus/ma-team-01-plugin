# Erreurs — le catalogue des messages en clair

*Règle produit : si l'utilisatrice ne comprend pas un échec, l'échec est un bug de notre côté. Aucun message technique brut ne doit jamais l'atteindre.*

## Le gabarit

Chaque erreur suit trois temps, en français simple :

1. **Ce qui s'est passé** — une phrase, sans jargon.
2. **Ce que ça veut dire** — la conséquence concrète pour elle.
3. **Ce qu'elle peut faire** — l'action suivante, faisable dans le chat (« dis-moi "réessaie" quand c'est fait »).

## Messages types

**Canal non connecté**
> Ce post n'a pas pu partir : le canal Instagram de Camille n'est pas connecté à Buffer. Ouvre les réglages de Cowork, autorise Buffer pour ce compte, puis dis-moi « réessaie ». Les autres posts du batch sont partis normalement.

**Système non installé**
> On dirait que le système n'est pas encore installé — il n'y a pas de dossier racine dans ton Drive. Dis-moi « installe le système » et je m'en occupe.

**Marque inconnue**
> Je ne trouve pas de marque « X » dans ton Roster. Tes marques actuelles : Camille, … . Si c'est une nouvelle marque, dis-moi « ajoute la marque X » et je te pose quelques questions.

**Calendrier vide**
> Il n'y a aucun sujet prévu pour Camille en septembre — donc rien à générer. Pour en prévoir, dis-moi simplement : « je veux prévoir un post pour … ».

**Corpus absent (avertissement, pas une erreur)**
> Le dossier `exemples` de Camille est vide : j'ai écrit ces posts à partir du seul contexte. Ils risquent d'être corrects mais passe-partout. Dépose quelques-uns de ses meilleurs posts dans `exemples` et la voix reviendra — ou si elle n'a aucun post passé, on peut en écrire 2 ou 3 ensemble, maintenant, pour fonder son corpus.

**Corpus maigre (avertissement, pas une erreur)**
> Le corpus de Camille ne compte que N posts — je m'en suis inspiré, mais la voix s'affinera à mesure qu'il grandit. Chaque post que tu valides peut le rejoindre.

**Image manquante pour Instagram**
> Ce post Instagram n'a pas d'image, et Instagram n'accepte pas de post sans image. Dis-moi si je la génère, si je pars d'une photo de la marque, ou si tu préfères passer ce post en mode rappel.

**Visuel non attaché (rappel, pas une erreur)**
> Le post LinkedIn de Camille du 10/09 est programmé dans Buffer, mais son visuel n'y est pas encore attaché. L'image est prête dans Drive — ouvre le post dans Buffer, ajoute-la, et dis-moi « c'est fait ». S'il part comme ça, il paraîtra en texte seul.

**URL d'image inaccessible**
> Le visuel de ce post est hébergé à une adresse qui ne répond pas publiquement — Buffer ne pourrait pas le récupérer au moment de la parution. Le post n'est pas parti, rien n'est perdu. Dis-moi si je réessaie avec un autre hébergement, ou si tu préfères l'attacher toi-même dans Buffer.

**Plafond Buffer**
> Ce batch mettrait plus de 25 posts le même jour sur le LinkedIn de Camille — Buffer refuse au-delà. Je peux étaler sur les jours voisins : dis-moi oui, ou donne-moi tes dates.

**Post bloqué par le gardefou**
> J'ai mis ce post de côté : il pourrait contredire un engagement de Camille (« <l'engagement> »). Relis-le individuellement — dis-moi « ok publie », « réécris en tenant compte de ça », ou « laisse-le de côté ».

**Buffer injoignable**
> Buffer ne répond pas pour le moment. Rien n'est parti et rien n'est perdu : tes posts restent des brouillons, prêts à l'envoi. Réessaie dans quelques minutes en me disant « relance l'envoi ».

**Version programmée dépassée**
> Le post LinkedIn de Camille du 10/09 a été retravaillé après son envoi : la version en file dans Buffer n'est plus la dernière. Dis-moi ce que tu préfères — et selon ce que Buffer permet, je te dirai si je peux corriger la version en file ou s'il faudra le faire à la main.

**Homonymes détectés**
> Il y a deux fichiers du même nom pour ce post dans Drive — je ne peux pas deviner lequel fait foi, donc je laisse ce post de côté. Ouvre le dossier, mets l'un des deux à la corbeille, puis dis-moi « réessaie ».

## Interdits

- Ne jamais montrer un code d'erreur, une stack trace, un nom d'API ou un identifiant technique.
- Ne jamais dire qu'un envoi a réussi sans l'avoir vérifié.
- En cas d'échec partiel d'un batch : toujours dire **ce qui est passé** et **ce qui reste**, jamais un échec global vague.
