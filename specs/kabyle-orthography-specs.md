# Spécification de notation, Unicode et normalisation pour le kabyle

**Identifiant proposé :** `kabyle-orthography-spec`

**Version :** 0.4-draft *(proposition de révision non officielle — voir Annexe C)*

**Base :** révision de la version 0.3-draft (3 septembre 2026) publiée sur kabyle-specs.github.io

**Statut :** proposition de publication — validation linguistique et reproductibilité des mesures encore requises

**Langue du document :** français

**Script couvert par cette version :** alphabet latin berbère

**Code ISO 639-3 :** `kab`

> **Note sur cette révision.** Ce fichier n'est pas une publication officielle du projet Kabyle Specs. C'est une proposition de correction préparée à la demande d'un utilisateur, à soumettre au mainteneur (Athmane Mokraoui) via pull request ou issue avant toute adoption. Les changements par rapport à la v0.3 sont listés et justifiés en Annexe C ; rien n'a été ajouté qui ne soit soit repris tel quel de la v0.3, soit explicitement marqué `[candidate]`/`[disputed]`/`[NEEDS REVIEW]`.

## Résumé

Cette spécification définit un profil technique pour la représentation, l'échange et le nettoyage de textes kabyles écrits en alphabet latin. Elle distingue quatre opérations qui ne doivent pas être confondues : la notation orthographique, la normalisation Unicode, le nettoyage des corpus et l'identification de la langue.

Le profil de base recommande l'utilisation de l'Unicode en UTF-8, de la normalisation NFC et des caractères latins propres à la notation kabyle retenue. Il fournit également une table de contaminants fréquents, notamment les confusions entre `ɛ` et des caractères grecs ou cyrilliques visuellement proches. Ces confusions peuvent être corrigées automatiquement seulement lorsque le contexte établit avec une confiance suffisante qu'il s'agit d'une erreur d'encodage. Le texte original, les modifications et leur provenance doivent toujours être conservés.

Cette spécification ne prétend pas supprimer la variation dialectale, éditoriale ou liée aux choix d'auteur. Les points disputés sont explicitement marqués et ne doivent pas être arbitrés silencieusement.

> **Principe général :** une normalisation technique peut être automatique ; une correction orthographique ou linguistique doit être traçable et, lorsqu'elle est ambiguë, soumise à une révision humaine compétente en kabyle.

## 1. Périmètre et principes

### 1.1 Périmètre

Cette version couvre :

- les caractères de la notation latine kabyle retenue ;
- leurs points de code Unicode et leurs relations de casse ;
- la normalisation NFC et l'encodage UTF-8 ;
- les espaces, la ponctuation et les séparateurs dans un profil technique ;
- la détection des confusions interscripts et des graphies héritées ;
- la conservation de la provenance et des variantes dans les corpus NLP.

Cette version ne couvre pas encore :

- une orthographe complète en tifinagh ou en alphabet arabe ;
- la translittération réversible entre plusieurs écritures ;
- la césure typographique en fin de ligne ;
- la prononciation détaillée de tous les parlers kabyles ;
- la correction automatique générale des mots ou de la morphologie.

### 1.2 Terminologie

| Terme | Définition opérationnelle |
|---|---|
| **Graphème** | Unité écrite pertinente pour la notation, par exemple `ɛ`, `ɣ` ou `ṭ`. |
| **Caractère** | Unité Unicode. Un graphème peut être représenté par un caractère précomposé ou, dans certains cas, par une séquence canonique. |
| **Notation** | Convention écrite utilisée pour représenter le kabyle. Cette spécification traite principalement la notation latine usuelle. |
| **Normalisation Unicode** | Transformation technique telle que NFC ; elle ne constitue pas une correction linguistique. |
| **Contaminant** | Caractère ou séquence non conforme au profil attendu dans un segment kabyle, sans que sa présence soit nécessairement une erreur dans l'ensemble du document. |
| **Correction** | Modification linguistique ou orthographique proposée avec justification et provenance. |
| **Quarantaine** | État d'un segment qui ne doit pas être intégré à un corpus normalisé avant examen. |
| **Texte mixte** | Segment contenant du kabyle et une ou plusieurs autres langues, scripts ou types de données. |

### 1.3 Principes normatifs

1. **Conserver la source.** Toute transformation doit être réversible ou accompagnée de la chaîne originale.
2. **Ne pas confondre caractère et langue.** Un caractère étranger peut être valide dans un nom propre, une citation, une URL ou une métadonnée.
3. **Ne pas confondre Unicode et orthographe.** NFC ne décide pas si un mot est correctement écrit.
4. **Déclarer la convention.** Une ressource doit indiquer la convention orthographique et le profil de normalisation utilisés.
5. **Préserver la variation documentée.** Une variante attestée ne doit pas être remplacée silencieusement par une autre forme.
6. **Marquer l'incertitude.** Une correction ambiguë doit être signalée plutôt qu'appliquée automatiquement.

## 2. Convention orthographique de référence

La présente spécification s'appuie principalement sur la notation usuelle décrite par les recommandations de l'INALCO et par le manuel de notation usuelle du tamazight publié à Béjaïa [1] [2]. Cette base concerne la représentation écrite ; elle ne constitue pas une transcription phonétique exhaustive des parlers kabyles.

Les valeurs phonologiques indiquées ci-dessous sont donc des indications générales. Elles ne doivent pas être utilisées seules pour générer une prononciation, une conjugaison ou une analyse dialectale.

## 3. Inventaire des lettres

### 3.1 Lettres de base

L'inventaire retenu contient **23 lettres latines de base**. Les lettres `O`, `P` et `V` ne font pas partie de cet inventaire de base, mais peuvent apparaître dans des emprunts, des noms propres ou des segments étrangers selon le profil appliqué.

| Majuscule | Minuscule | Nom usuel indicatif (INALCO/Béjaïa) | Nom alternatif rapporté² | Remarque phonologique générale |
|---|---|---|---|---|
| `A` | `a` | a | — | voyelle /a/ |
| `B` | `b` | ba | `yab` `[stated]` | consonne /b/ |
| `C` | `c` | sha | `yash` `[stated]` | valeur notée /ʃ/ dans cette notation de référence¹ |
| `D` | `d` | da | `yad` `[stated]` | /d/ |
| `E` | `e` | e | — | schwa, selon la convention de notation |
| `F` | `f` | fa | `yaf` `[stated]` | /f/ |
| `G` | `g` | ga | `yag` `[stated]` | /g/ |
| `H` | `h` | ha | `yah` `[stated]` | /h/ |
| `I` | `i` | i | — | /i/ |
| `J` | `j` | ja | `yaj` `[stated]` | /ʒ/ |
| `K` | `k` | ka | `yak` `[stated]` | /k/ |
| `L` | `l` | la | `yal` `[stated]` | /l/ |
| `M` | `m` | ma | `yam` `[stated]` | /m/ |
| `N` | `n` | na | `yan` `[stated]` | /n/ |
| `Q` | `q` | qa | `yaq` `[stated]` | /q/ |
| `R` | `r` | ra | `yar` `[stated]` | /r/ ; la réalisation varie selon le contexte |
| `S` | `s` | sa | `yas` `[stated]` | /s/ |
| `T` | `t` | ta | `yat` `[stated]` | /t/ |
| `U` | `u` | u | — | /u/ |
| `W` | `w` | wa | `yaw` `[stated]` | semi-voyelle ou consonne selon le contexte |
| `X` | `x` | xa | `yax` `[stated]` | fricative dont la réalisation varie selon la convention descriptive |
| `Y` | `y` | ya | `yay` `[stated]` | semi-voyelle /j/ |
| `Z` | `z` | za | `yaz` `[stated]` | /z/ |

¹ *Distinction notation/réalisation (cf. §1.2) : la notation associe systématiquement `c` à /ʃ/, indépendamment de variations de réalisation phonétique propres à certains parlers ou emprunts, qui restent hors périmètre de cette spécification.*

² **Statut à clarifier.** Colonne ajoutée sur indication d'un relecteur se déclarant locuteur natif. Le motif `ya+consonne` est la convention de nommage documentée pour les caractères **tifinagh** (ex. *yaz* pour ⵣ, à l'origine du symbole identitaire amazigh), non pour l'alphabet latin que couvre cette spécification (§1.1 exclut le tifinagh de son périmètre). Deux lectures restent possibles et n'ont pas été départagées : (a) il s'agit d'un usage oral réel de nommage des lettres *latines*, distinct de la convention tifinagh mais non publié ; (b) il s'agit des noms des caractères *tifinagh* correspondants, auquel cas leur place est dans une future spécification tifinagh et non dans ce tableau. `[NEEDS REVIEW]` — à trancher avant toute version stable ; ne pas traiter cette colonne comme équivalente en autorité à la colonne INALCO/Béjaïa sourcée.

### 3.2 Caractères latins particuliers

L'inventaire contient **10 caractères latins particuliers**, soit **33 lettres au total** avec les 23 lettres de base. Les capitales sont incluses afin de permettre la casse, les noms propres et le début des phrases.

| Majuscule | Code Unicode | Minuscule | Code Unicode | Désignation Unicode abrégée | Remarque |
|---|---:|---|---:|---|---|
| `Č` | U+010C | `č` | U+010D | C avec caron | affriquée postalvéolaire selon la convention descriptive |
| `Ḍ` | U+1E0C | `ḍ` | U+1E0D | D avec point souscrit | réalisations [dˤ] ou [ðˤ] attestées selon le contexte et le parler |
| `Ɛ` | U+0190 | `ɛ` | U+025B | E latin ouvert | consonne pharyngale traditionnellement associée à l'ayn, généralement /ʕ/ ; ce n'est pas la voyelle française /ɛ/ |
| `Ǧ` | U+01E6 | `ǧ` | U+01E7 | G avec caron | affriquée /d͡ʒ/ selon la convention descriptive |
| `Ɣ` | U+0194 | `ɣ` | U+0263 | Gamma latin | fricative uvulaire, avec variation phonétique possible |
| `Ḥ` | U+1E24 | `ḥ` | U+1E25 | H avec point souscrit | pharyngale, généralement /ħ/ |
| `Ṛ` | U+1E5A | `ṛ` | U+1E5B | R avec point souscrit | statut et distribution à déclarer selon la convention choisie |
| `Ṣ` | U+1E62 | `ṣ` | U+1E63 | S avec point souscrit | emphatique ; distribution variable selon les conventions |
| `Ṭ` | U+1E6C | `ṭ` | U+1E6D | T avec point souscrit | emphatique |
| `Ẓ` | U+1E92 | `ẓ` | U+1E93 | Z avec point souscrit | emphatique |

### 3.3 Points encore disputés

Les questions suivantes ne doivent pas être résolues implicitement par un outil :

| Question | Statut dans cette version | Politique recommandée |
|---|---|---|
| Écriture systématique de `ṛ` et `ṣ` | `[disputed]` | déclarer la convention du corpus ; conserver la graphie source dans les autres cas |
| Position alphabétique de `ɛ` | `[disputed]` | déclarer l'ordre de collation choisi ; si une spécification de collation distincte existe dans ce projet, s'y référer explicitement plutôt que d'en fixer un ici² |
| Statut de `v` dans les emprunts | `[disputed]` | autoriser uniquement si le profil ou la source le justifie |
| Choix entre guillemets français `« »` et guillemets droits ASCII `"…"` | `[disputed]`³ | déclarer la convention du document ; ne pas présenter l'une des deux formes comme la forme canonique unique sans le dire explicitement (voir §6.2) |
| Réalisation de plusieurs consonnes emphatiques | variable | ne pas dériver automatiquement une prononciation de la seule lettre |
| Variantes dialectales et régionales | attestées | les annoter au niveau du corpus ou du document |

² *Ajout v0.4 : ce projet nomme une spécification de collation dans son organisation générale ; son existence et son contenu actuels n'ont pas été vérifiés pour cette révision — à confirmer avant de publier le renvoi comme un lien actif.*
³ *Ajout v0.4 — voir Annexe C, point 1.*

### 3.4 Lettres d'emprunts et segments étrangers

`O`, `P`, `V` et d'autres caractères absents de l'inventaire de base peuvent être conservés dans les cas suivants :

- emprunt lexical attesté dans la source étudiée ;
- nom propre ;
- titre ou citation ;
- segment étranger ;
- identifiant ou métadonnée.

Leur présence ne suffit donc pas à déclarer un document non kabyle. Un profil strict peut les signaler dans les **mots supposés kabyles**, mais il ne doit pas les supprimer dans le texte source.

## 4. Unicode et représentation canonique

### 4.1 Encodage

Les fichiers destinés à l'échange ou au stockage doivent être encodés en **UTF-8**. L'absence de BOM peut être exigée par un format de fichier particulier, mais elle ne constitue pas une propriété linguistique.

### 4.2 Normalisation NFC

Le profil `unicode-canonical` exige la normalisation Unicode **NFC** avant indexation ou comparaison. Cette opération met en cohérence les séquences canoniquement équivalentes ; elle ne remplace pas la correction orthographique.

Les systèmes peuvent préférer les formes précomposées pour l'indexation. Ils doivent toutefois conserver la chaîne d'origine lorsqu'une séquence combinée est rencontrée.

### 4.3 Casse

Les opérations de casse doivent utiliser les tables Unicode officielles et être testées explicitement pour les paires suivantes :

| Majuscule | Minuscule |
|---|---|
| `Č` | `č` |
| `Ḍ` | `ḍ` |
| `Ɛ` | `ɛ` |
| `Ǧ` | `ǧ` |
| `Ɣ` | `ɣ` |
| `Ḥ` | `ḥ` |
| `Ṛ` | `ṛ` |
| `Ṣ` | `ṣ` |
| `Ṭ` | `ṭ` |
| `Ẓ` | `ẓ` |

Une implémentation doit tester séparément `lowercase`, `uppercase`, `casefold` et NFC. Une comparaison insensible à la casse ne doit pas être confondue avec une comparaison orthographique.

### 4.4 Inventaire de validation

Une validation par blocs Unicode est insuffisante. Les validateurs doivent utiliser une liste explicite de caractères et de catégories autorisés par profil :

- lettres de base et caractères particuliers de la section 3 ;
- lettres d'emprunts, si le profil les autorise ;
- chiffres (voir §4.5) ;
- espaces ;
- ponctuation ;
- caractères de format autorisés par le format de données ;
- caractères propres aux métadonnées lorsque celles-ci sont séparées du texte.

Les caractères de contrôle non requis, les caractères non attribués et les caractères invisibles non documentés doivent être signalés.

### 4.5 Chiffres *(nouvelle section, v0.4 ; simplifiée après relecture)*

Cette version antérieure (0.3) mentionnait les « chiffres » comme catégorie de validation acceptée (§4.4) sans préciser le jeu de chiffres canonique. Cette section comble ce manque.

Le profil de base retient les chiffres arabes (occidentaux) `0123456789` (U+0030–U+0039) comme forme canonique unique. `[stated, non sourcé]` — selon un relecteur, l'usage kabyle latin n'emploie jamais d'autre jeu de chiffres ; cette spécification suit ce point à titre d'indication d'usage, sans disposer d'un audit de corpus publié pour le confirmer. Une version antérieure de cette section proposait un contrôle de confusion avec les chiffres arabes-indiens (U+0660–U+0669) et arabes-indiens étendus (U+06F0–U+06F9) ; ce contrôle est retiré ici faute de toute occurrence rapportée, mais pourrait être réintroduit si un audit de corpus futur en montrait l'utilité (voir §13, point 10).

## 5. Contaminants et confusions interscripts

### 5.1 Principales confusions

La table suivante décrit des confusions fréquentes. La colonne « action » indique une politique par défaut, et non une conversion inconditionnelle.

| Caractère rencontré | Code Unicode | Cible possible | Contexte typique | Action par défaut |
|---|---:|---|---|---|
| `ε` | U+03B5 | `ɛ` | grec ou copier-coller | signaler ; corriger si le segment est confirmé kabyle |
| `Σ` | U+03A3 | `Ɛ` | confusion de capitale | signaler ; corriger si le contexte est confirmé |
| `γ` | U+03B3 | `ɣ` | grec ou copier-coller | signaler ; corriger si le contexte est confirmé |
| `Γ` | U+0393 | `Ɣ` | confusion de capitale | signaler ; corriger si le contexte est confirmé |
| `Ԑ` | U+0510 | `Ɛ` | cyrillique | signaler ; corriger si le contexte est confirmé |
| `ԑ` | U+0511 | `ɛ` | cyrillique | signaler ; corriger si le contexte est confirmé |
| `ı` | U+0131 | `i` | turc | signaler ; corriger si la langue et le mot sont confirmés |
| `İ` | U+0130 | `I` | turc | signaler ; corriger si la langue et le mot sont confirmés |
| `ğ` | U+011F | `ǧ` ou `ɣ` | turc, ancienne saisie, autre convention | ne jamais choisir la cible sans contexte |

Une occurrence dans un nom propre, une citation ou un segment étranger doit être conservée et annotée plutôt que corrigée.

### 5.2 Graphies héritées et digraphes

Les séquences `ch`, `dj`, `gh`, `th`, `dh`, `sh`, `zh`, `rh` et `3` peuvent correspondre à des habitudes de saisie, à des conventions historiques, à de l'Arabizi ou à une autre langue. Elles ne doivent pas être converties automatiquement dans un texte général.

Un outil peut proposer une conversion à haute confiance uniquement si un lexique, un paradigme morphologique ou une annotation humaine confirme la correspondance. Sinon, il retourne un signalement avec position et hypothèses possibles.

### 5.3 Caractères étrangers

Les caractères `ç`, `ñ`, `ø`, `œ`, `ß`, `þ`, les lettres grecques et les lettres cyrilliques ne doivent pas être supprimés globalement. Ils sont non conformes à un **segment kabyle strict** lorsqu'ils ne sont pas justifiés, mais peuvent être valides dans un segment étranger ou une métadonnée.

## 6. Ponctuation, espaces et tirets

### 6.1 Convention d'espacement

La rédaction kabyle latine visée par cette spécification suit une convention de ponctuation de type anglais. Cette convention est choisie précisément pour éviter les règles typographiques françaises relatives aux espaces insécables et aux espaces fines insécables.

Dans le profil `kabyle-standard`, l'espace ordinaire U+0020 est l'espace canonique. Il ne doit y avoir aucun espace avant ou après une parenthèse ouvrante ou fermante, ni avant les signes `.`, `,`, `;`, `:`, `?` et `!`. Un espace U+0020 est placé après ces signes lorsqu'un autre mot suit. Les espaces multiples sont réduits à une seule unité hors des blocs préformatés.

Les caractères U+00A0 et U+202F ne sont pas utilisés comme espaces de ponctuation dans ce profil. Ils doivent être signalés ou convertis en U+0020 dans une copie normalisée, sans modifier le texte source.

### 6.2 Ponctuation *(révisé, v0.4)*

Le profil de texte courant accepte au minimum les signes suivants et applique la convention d'espacement ci-dessus :

| Fonction | Caractères recommandés |
|---|---|
| Phrase | `.`, `?`, `!` |
| Coordination | `,`, `;`, `:` |
| Citation | voir ci-dessous — convention à déclarer, `[disputed]` |
| Parenthèses | `(`, `)` |
| Trait d'union ou séparateur | `-`, selon la convention morphographique déclarée |

**Guillemets — point disputé, non arbitré dans cette version.** La v0.3 de ce document désignait les guillemets droits ASCII `"..."` comme la forme canonique et traitait `« »` comme une simple option éditoriale. Cette révision retire cette hiérarchisation : les deux conventions sont attestées dans l'usage kabyle écrit selon la source et le registre (presse, édition, saisie technique), et le principe normatif n°4 de ce document (« Déclarer la convention ») s'applique ici comme pour `ṛ`/`ṣ` ou l'ordre de collation. Un profil ou un document doit déclarer explicitement lequel des deux il utilise ; aucun outil ne doit convertir l'un vers l'autre sans que cette déclaration soit faite. `[NEEDS REVIEW]` — la question de savoir si un registre par défaut existe réellement (par ex. `« »` en presse vs `"…"` en profil technique/échange de données) devrait être tranchée par relecture native plutôt que par ce document.

Les espaces insécables associées à l'usage français des guillemets `« »` restent hors du profil canonique de ce document quel que soit le choix retenu pour le signe lui-même (voir §6.1).

### 6.3 Apostrophe

L'apostrophe n'est pas un graphème de l'inventaire kabyle latin retenu. Dans un segment supposé kabyle, elle doit être signalée pour révision. Elle ne doit cependant pas être supprimée dans une citation, un nom propre, un segment étranger ou une métadonnée.

### 6.4 Tiret et clitiques

Le tiret est un séparateur morphographique ou typographique ; ce n'est pas une lettre. Son emploi dépend de la convention morphosyntaxique et éditoriale. Un validateur ne doit pas déduire automatiquement qu'un tiret est obligatoire dans toute construction.

Les constructions comportant des clitiques, des particules directionnelles ou des préverbes doivent être traitées par des règles morphologiques documentées. Les règles de cette spécification ne remplacent pas une grammaire ou un tokeniseur kabyle.

## 7. Profils de traitement

Une ressource doit déclarer le profil appliqué.

| Profil | Objectif | Transformations autorisées | Sortie attendue |
|---|---|---|---|
| `source-preserving` | conservation documentaire | NFC facultative sur une copie ; aucune correction destructive | original intact + annotations |
| `unicode-canonical` | échange et indexation | UTF-8, NFC, espaces techniques selon configuration | texte technique + journal de transformations |
| `kabyle-standard` | texte kabyle révisé | corrections validées et transformations à haute confiance | texte révisé + provenance |
| `strict-training` | corpus prêt pour entraînement | validation stricte, quarantaine des ambiguïtés, revue humaine | données propres + rapport qualité |
| `mixed-language` | corpus multilingue | segmentation et annotation des langues | segments conservés avec étiquettes |

Un seul texte peut donc avoir plusieurs représentations : l'original, la forme Unicode canonique, la forme révisée et la forme destinée à l'entraînement.

## 8. Pipeline recommandé

### Étape 1 — Préservation

Conserver le fichier original, son empreinte, sa date d'acquisition, sa source, son encodage déclaré et ses métadonnées.

### Étape 2 — Segmentation

Séparer, lorsque cela est possible, le texte courant, les citations, les noms propres, les URL, les identifiants et les métadonnées. La validation linguistique ne doit pas s'appliquer indistinctement à toutes ces zones.

### Étape 3 — Normalisation Unicode

Convertir une copie en UTF-8 et NFC. Enregistrer les changements de représentation, sans encore modifier les lettres selon des hypothèses linguistiques.

### Étape 4 — Détection

Détecter les caractères, séquences et espaces suspects. Pour chaque occurrence, enregistrer :

- la position dans le segment ;
- le point de code et le nom Unicode ;
- la chaîne environnante ;
- les remplacements possibles ;
- la confiance ;
- la règle ayant produit le signalement.

### Étape 5 — Correction à haute confiance

Appliquer uniquement les corrections pour lesquelles le segment est identifié comme kabyle et où la cible est déterminée par une règle explicite. Une correction automatique doit être réversible.

### Étape 6 — Révision

Mettre en quarantaine les cas ambigus. Une validation humaine doit être capable d'accepter, de modifier ou de rejeter la proposition automatique.

### Étape 7 — Contrôle final

Calculer les métriques par segment et par type de données. Publier les taux de correction, de quarantaine et de texte mixte avec le corpus.

## 9. Contrôles qualité pour les corpus NLP

### 9.1 Contrôles minimaux

| Identifiant | Contrôle | Résultat |
|---|---|---|
| `Q-UTF8` | Encodage UTF-8 valide | succès ou erreur technique |
| `Q-NFC` | Texte en NFC | succès ou liste des positions |
| `Q-INVENTORY` | Caractères compatibles avec le profil | accepté, signalé ou mis en quarantaine |
| `Q-CONTAMINANT` | Confusions interscripts détectées | liste des occurrences et hypothèses |
| `Q-DIGITS` | Chiffre hors de `0-9` détecté *(nouveau, v0.4, voir §4.5)* | signalement ; catégorie retenue à titre préventif, aucune occurrence rapportée à ce jour |
| `Q-MIXED` | Segments étrangers ou mixtes | annotation, non suppression |
| `Q-SPACE` | Espaces non conformes au profil | rapport et transformation réversible |
| `Q-PUNCT` | Ponctuation incohérente, y compris convention de guillemets non déclarée | signalement éditorial |
| `Q-PROVENANCE` | Source et transformations conservées | obligatoire pour les corpus publiés |
| `Q-LANGID` | Indice de langue auxiliaire | score et statut, jamais décision unique |

### 9.2 Identification de langue

Un modèle d'identification de langue peut aider à repérer les segments probablement kabyles, mais son score ne constitue pas une preuve orthographique. Les textes courts, les noms propres, les phrases mixtes et les dialectes peuvent produire des scores peu fiables.

Les résultats doivent être classés au minimum comme suit : `kabyle-probable`, `mixte`, `incertain` ou `non-kabyle-probable`. Un seuil tel que `0,95` ne doit pas être présenté comme un standard établi sans protocole expérimental, jeu de test et intervalle d'incertitude publiés.

### 9.3 Métriques

Toute mesure doit indiquer son dénominateur et son unité. Les rapports doivent distinguer le nombre de documents, phrases, clips, tokens, caractères et occurrences.

| Métrique | Définition recommandée |
|---|---|
| Taux de contaminants | occurrences contaminantes / occurrences examinées |
| Taux de segments contaminés | segments contenant au moins un contaminant / segments examinés |
| Taux de corrections automatiques | corrections appliquées / corrections proposées |
| Taux de quarantaine | segments mis en quarantaine / segments examinés |
| Taux de textes mixtes | segments annotés mixtes / segments examinés |
| Taux de conformité NFC | segments NFC / segments examinés |

Un taux cible doit être accompagné du corpus, de sa version, de sa date d'extraction, du script de comptage et de la méthode de déduplication.

## 10. Exigences pour les polices et les logiciels

Une police destinée au kabyle doit couvrir les caractères de la section 3 et leurs capitales. Elle doit notamment rendre distincts :

- `d` et `ḍ` ;
- `h` et `ḥ` ;
- `r` et `ṛ` ;
- `s` et `ṣ` ;
- `t` et `ṭ` ;
- `z` et `ẓ` ;
- `c` et `č` ;
- `g` et `ǧ` ;
- `ɛ` et `Ɛ` ;
- `ɣ` et `Ɣ`.

Les tests de rendu doivent être réalisés à plusieurs tailles et sur plusieurs systèmes. Un critère numérique unique, tel que « lisible à 11 px », ne suffit pas à garantir l'accessibilité : la lisibilité dépend également de la police, du moteur de rendu, de l'écran et du contraste.

Les logiciels doivent tester les formes de casse, l'affichage des points souscrits, l'indexation NFC et la copie-coller entre systèmes.

## 11. API indicative

L'API suivante distingue normalisation technique et audit linguistique :

```python
from dataclasses import dataclass

@dataclass
class Finding:
    start: int
    end: int
    value: str
    codepoints: list[str]
    category: str
    suggestions: list[str]
    confidence: float | None
    action: str

@dataclass
class NormalizedText:
    original: str
    unicode_text: str
    findings: list[Finding]
    changes: list[dict]
    profile: str


def normalize_unicode(text: str) -> str:
    """Retourne une copie UTF-8 logique normalisée en NFC."""
    ...


def audit_kabyle(text: str, profile: str = "unicode-canonical") -> list[Finding]:
    """Détecte les caractères et séquences suspects sans correction destructive."""
    ...


def propose_corrections(text: str, findings: list[Finding]) -> list[dict]:
    """Retourne des propositions réversibles avec justification et confiance."""
    ...


def is_canonical_unicode(text: str) -> bool:
    """Vérifie uniquement les propriétés Unicode du profil, notamment NFC."""
    ...
```

La fonction `is_canonical_unicode` ne doit pas être utilisée pour décider si un texte est grammaticalement ou orthographiquement correct en kabyle.

## 12. Jeux de tests minimaux

Une implémentation conforme doit tester au minimum :

| Test | Entrée | Résultat attendu |
|---|---|---|
| NFC | forme précomposée et séquence combinée équivalente | même représentation NFC |
| Casse | toutes les paires de la section 4.3 | conversions Unicode cohérentes |
| Faux ami grec | `ε`, `γ`, `Σ`, `Γ` dans un segment kabyle confirmé | signalement ou correction traçable |
| Faux ami cyrillique | `Ԑ`, `ԑ` dans un segment kabyle confirmé | signalement ou correction traçable |
| Turc | `ğ`, `ı`, `İ` dans un nom propre | conservation ou signalement, pas de conversion aveugle |
| Chiffres | tout caractère numéral hors `0-9` dans un segment kabyle confirmé | signalement (§4.5) — catégorie préventive, aucune occurrence encore rapportée |
| Citation | segment français contenant `ç` | conservation |
| Métadonnée | URL contenant des caractères non kabyles | exclusion de la validation linguistique |
| Texte mixte | phrase kabyle avec citation étrangère | segmentation et conservation |
| Apostrophe | apostrophe dans un segment kabyle | signalement ; conservation de la source |
| Espacement | NBSP dans un profil technique | transformation réversible si activée |

## 13. Limitations et travaux futurs

Les travaux suivants restent nécessaires avant une version 1.0 normative :

1. établir un inventaire documenté des variantes orthographiques régionales et éditoriales ;
2. publier un ordre de collation avec exemples et tests ;
3. documenter séparément les règles morphographiques des clitiques ;
4. définir les profils tifinagh et arabe ;
5. construire un jeu de données annoté pour les contaminants et les textes mixtes ;
6. reproduire les statistiques Common Voice et Tatoeba avec scripts et versions archivés ;
7. faire relire les exemples kabyles par plusieurs locuteurs compétents représentant les conventions concernées ;
8. publier la licence et les conditions de réutilisation des tables et scripts associés ;
9. *(ajout v0.4)* trancher, ou documenter par relecture native, la question du registre par défaut pour les guillemets (§6.2) ;
10. *(ajout v0.4, révisé)* si un besoin apparaît, documenter avec un audit de corpus versionné toute confusion de jeu de chiffres avant de réintroduire un contrôle dédié (§4.5) ; en l'état, `0-9` est retenu comme seul jeu attesté.

## 14. Références

### 14.1 Sources citées dans le texte

[1]: https://hal.science/hal-05530877/ "Bouamara et al., Ilugan n tira n tmaziɣt — Règles de la notation usuelle du tamazight kabyle"

[2]: https://www.centrederechercheberbere.fr/tl_files/doc-pdf/notation.pdf "Salem Chaker, Propositions pour la notation usuelle à base latine du berbère"

### 14.2 Lectures complémentaires *(non citées explicitement dans le corps du texte — reclassées en v0.4, voir Annexe C, point 2)*

[3]: https://www.unicode.org/standard/standard.html "Unicode Consortium, The Unicode Standard"

[4]: https://www.unicode.org/reports/tr15/ "Unicode Standard Annex #15, Unicode Normalization Forms"

[5]: https://www.unicode.org/reports/tr44/ "Unicode Standard Annex #44, Unicode Character Database"

[6]: https://www.karthala.com/ "Kamal Naït-Zerrad, Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit"

[7]: https://hal.science/search/index/?q=Vers%20une%20normalisation%20du%20kabyle%20alphabet "F. Adjed, Vers une normalisation du kabyle : alphabet — recherche bibliographique HAL"

[8]: https://docs.weblate.org/ "Weblate documentation — checks and translation quality"

[9]: https://huggingface.co/datasets/boffire/common-voice-scripted-speech-kab-26 "Athmane Mokraoui, Common Voice Scripted Speech Kabyle 26.0 — dataset reference"

[10]: https://butterflyoffire.codeberg.page/cv26/ "Athmane Mokraoui, CV26 Kabyle Contamination Report"

[11]: https://huggingface.co/datasets/boffire/tatoeba-en-kab "Athmane Mokraoui, Tatoeba English–Kabyle Parallel Corpus — dataset reference"

[12]: https://downloads.tatoeba.org/exports/sentences.tar.bz2 "Tatoeba Project, sentences export"

## Annexe A — Changelog de la version 0.3

La version 0.3 corrige le décompte de l'inventaire, distingue les 23 lettres de base des 10 caractères particuliers, corrige la description de `ɛ`, retire les conversions destructives sans contexte, sépare les profils de traitement, remplace la whitelist par blocs par une validation explicite et introduit la conservation obligatoire de la provenance.

Elle ne tranche pas les questions disputées relatives à `ṛ`, `ṣ`, `v`, à l'ordre alphabétique ou aux variantes dialectales. Ces sujets doivent être traités dans une version ultérieure ou dans des profils explicitement nommés.

## Annexe B — Statut de validation

| Domaine | Statut |
|---|---|
| Points de code Unicode | vérification technique requise dans les tests d'implémentation |
| Inventaire 23 + 10 = 33 | corrigé dans cette version |
| Notation usuelle de référence | fondée sur [1] et [2] |
| Valeur phonologique de `ɛ` | corrigée ; revue spécialisée recommandée |
| Table des contaminants | à valider sur des corpus versionnés |
| Statistiques de contamination | à reproduire avec protocole publié |
| Règles de cliticisation | hors périmètre normatif détaillé de cette version |
| Validation native des exemples | requise avant déclaration de version stable |
| Convention de guillemets *(v0.4)* | non tranchée ; relecture native requise (§6.2) |
| Jeu de chiffres *(v0.4)* | `0-9` retenu comme seul jeu attesté, sur indication non sourcée d'un relecteur ; aucune confusion rapportée (§4.5) |

> Cette spécification est une base technique publiable comme **proposition**. Elle ne doit pas être présentée comme une norme définitive tant que les points marqués `[disputed]`, les statistiques et la validation des exemples n'ont pas été documentés.

## Annexe C — Changelog de la révision 0.4 (non officielle)

Cette révision a été préparée en réponse à cinq points relevés lors d'une relecture de la v0.3-draft. Chaque changement est listé avec sa justification ; aucun fait linguistique nouveau n'a été introduit.

1. **§6.2 Guillemets.** La v0.3 déclarait les guillemets droits ASCII `"..."` comme forme canonique et reléguait `« »` à un usage éditorial secondaire. Cette hiérarchisation n'est pas cohérente avec l'usage attesté de `« »` dans la presse kabyle et contredit le principe normatif n°4 du document lui-même (« déclarer la convention », déjà appliqué à `ṛ`/`ṣ` et à l'ordre de collation). Le point est retiré de la liste des formes canoniques fixées et déplacé dans la table des points disputés (§3.3), avec statut `[NEEDS REVIEW]` en attente de relecture native.
2. **§14 Références.** Dans la v0.3, seules les références [1] et [2] étaient effectivement appelées dans le corps du texte ; [3] à [12] figuraient dans la liste sans marqueur d'appel, ce qui contredit le principe n°1 (« conserver la source ») et le contrôle `Q-PROVENANCE`. Cette révision sépare la liste en « sources citées » et « lectures complémentaires » plutôt que d'inventer des appels de citation non vérifiables pour les rattacher à des affirmations précises.
3. **§4.5 et §9.1 (`Q-DIGITS`) Chiffres.** La v0.3 mentionne les « chiffres » comme catégorie de validation (§4.4) sans jamais préciser le jeu canonique — une omission par rapport à la v0.2 antérieure, qui traitait ce point. Une première version de cette révision proposait aussi un contrôle de confusion avec les chiffres arabes-indiens, marqué `[candidate]` faute de mesure. Après relecture, ce contrôle a été retiré : `0-9` est retenu comme seul jeu attesté en kabyle latin, sur la base d'une indication non sourcée reçue en relecture plutôt que d'un audit de corpus publié — statut noté comme tel plutôt que présenté comme confirmé.
4. **§3.1, note sur `c`.** La formulation « généralement /ʃ/ » de la v0.3 mélangeait notation et réalisation phonétique, deux notions que le document distingue lui-même en §1.2. La note reformulée précise que c'est la valeur *notée* qui est fixe, la variation concernant la réalisation phonétique hors périmètre du document.
5. **§3.3, ordre de collation.** Ajout d'un renvoi conditionnel vers une éventuelle spécification de collation distincte au sein du même projet, explicitement marqué comme non vérifié à ce stade plutôt que présenté comme un lien confirmé.

6. **§3.1, colonne « Nom alternatif rapporté ».** Ajout après relecture par un contributeur se déclarant locuteur natif, proposant le motif `ya+consonne` (`yab`, `yash`, `yad`...) comme nommage des lettres latines. Ce motif correspond à la convention documentée de nommage des caractères **tifinagh**, script hors périmètre de cette spécification (§1.1). Ajouté par prudence en colonne séparée et marqué `[stated]`/`[NEEDS REVIEW]` plutôt que substitué au nommage INALCO/Béjaïa déjà sourcé, faute d'avoir départagé s'il s'agit d'un usage oral réel pour le latin ou d'un glissement de script.

Comme pour la v0.3, cette révision ne tranche aucun point disputé restant. Elle ne doit pas être fusionnée dans le dépôt sans revue par le mainteneur du projet et, pour les points 1 et 4, par un locuteur natif compétent.

---

**Auteur et mainteneur proposés (document de base, v0.3) :** Athmane Mokraoui.
**Révision non officielle (v0.4-draft) :** préparée par un tiers à des fins de relecture ; à ne pas attribuer au mainteneur avant validation.
**Licence proposée :** à compléter explicitement avec une licence libre (inchangé depuis la v0.3).

<!-- Fin du document -->
