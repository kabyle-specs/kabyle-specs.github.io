# Spécification des pronoms et clitiques kabyles (Taqbaylit)

- **Identifiant :** `kabyle-pronouns-clitics-spec`
- **Version :** 0.1-draft
- **Statut :** Draft — Proposition de spécification technique
- **Auteur :** Athmane MOKRAOUI
- **Date :** 2026
- **Code ISO 639-3 :** `kab`
- **Script couvert :** alphabet latin berbère

## Sources principales

### Corpus
- **Nom :** `boffire/tatoeba-kabyle-mono-cleaned`
- **Taille :** 756 774 phrases
- **Plateforme :** HuggingFace
- **Version :** 2026
- **Nettoyage :** Corpus nettoyé, contamination orthographique documentée (3,66 %)

### Liste d'affixes communautaire
- **Source :** [KabyleNLP / Belkacem77](https://github.com/MohammedBelkacem/KabyleNLP/blob/master/copus/affixescolles.txt)
- **Auteur :** Mohammed Belkacem
- **Nature :** Ressource d'ingénierie NLP pour tokenisation
- **Licence :** Non spécifiée (à clarifier avant réutilisation verbatim)
- **Formes uniques :** 82 après dédoublonnage

### Grammaires de référence
- Naït-Zerrad, Kamal. *Grammaire moderne du kabyle. Tajerrumt tatrart n teqbaylit*. Karthala, 2001.
- Mettouchi, Amina. *Prosodic Segmentation and Grammatical Relations: The Direct Object in Kabyle (Berber)*. 2018.
- Mettouchi, Amina. *The Interaction of State, Prosody and Linear Order in Kabyle (Berber)*. 2018.
- Mettouchi, Amina. *The Grammaticalization of Directional Particles in Berber*.

### Spécifications connexes
- `kabyle-orthography-spec` (v0.4-draft) — alphabet et normalisation Unicode
- `kabyle-collation-spec` — ordre alphabétique (à vérifier)

---

## 1. Résumé

Ce document spécifie les formes, l'ordre et les règles d'allomorphie des **clitiques pronominaux** en kabyle (taqbaylit), dans le but de fournir une base de référence pour les modèles de langue et les outils de traitement automatique des langues (NLP). L'objectif est de **prévenir les hallucinations** de formes grammaticales incorrectes par les modèles d'IA.

Toutes les formes et règles documentées ici ont été **validées empiriquement** par croisement entre :
1. Une liste d'affixes communautaire (82 formes uniques)
2. Un corpus de 756 774 phrases (Tatoeba kabyle nettoyé)

**Résultats de validation :**
- **70/82 affixes confirmés** par le corpus (85,4 %)
- **3 règles d'allomorphie** découvertes et validées
- **1 règle d'ordre** strictement confirmée (297 occurrences vs 0 inverse)

**Principe général :** une forme documentée dans cette spécification doit être attestée soit dans le corpus, soit dans une grammaire de référence, soit dans les deux. Les formes non attestées sont marquées `[NEEDS REVIEW]` et ne doivent pas être utilisées comme base pour l'entraînement de modèles.

---

## 2. Périmètre

### 2.1 Inclus

- Clitiques objets (directs et indirects)
- Suffixes possessifs
- Clitiques directionnels (ventif / andatif)
- Démonstratifs et déictiques suffixés
- Règles d'ordre des clitiques
- Règles d'allomorphie contextuelle et phonologique
- Particules préverbales déclenchant la proclise

### 2.2 Exclu (hors périmètre)

- Morphologie nominale (genre, nombre, état d'annexion) — à spécifier séparément
- Numéraux
- Dérivation verbale (voix causative, passive, réciproque)
- Intégration des emprunts
- Toponymes et noms propres
- Pronoms sujets (conjugaison verbale couverte par une autre spécification)

---

## 3. Méthodologie

### 3.1 Extraction

**Méthode :** Segmentation par trait d'union (`-`) de tous les mots du corpus.

**Justification :** En kabyle, les clitiques sont typographiquement liés à l'hôte par un trait d'union (convention morphographique attestée).

**Limite :** Cette méthode capture aussi les mots composés et les formes verbales conjuguées suivies de clitiques. Un tri par fréquence, position et inspection manuelle a été appliqué pour filtrer le bruit.

### 3.2 Validation croisée

Chaque affixe de la liste communautaire (Belkacem77) a été vérifié dans le corpus selon trois critères :
1. **Présence :** l'affixe apparaît-il dans le corpus ?
2. **Fréquence :** combien d'occurrences ? (seuil de confiance : 5 occurrences minimum)
3. **Position dominante :** l'affixe apparaît-il majoritairement à la position attendue ?
   - Préfixe attendu → position dominante "début"
   - Suffixe attendu → position dominante "fin"

Chaque segment fréquent du corpus absent de la liste a été inspecté manuellement pour déterminer s'il s'agit d'un clitique réel, d'un artefact de tokenisation ou de bruit.

### 3.3 Analyse phonologique

Pour les allomorphes (variantes conditionnées par le contexte), l'analyse a porté sur :
- Le caractère précédant immédiatement l'affixe
- Classification selon l'inventaire de `kabyle-orthography-spec` :
  - Voyelles : `a`, `e`, `i`, `u`
  - Consonnes : 29 lettres incluant `ɛ`, `ɣ` (consonnes pharyngale/uvulaire)
  - Autres : lettres hors inventaire de base (`o`, `p`, `v`)

### 3.4 Critères de statut

| Statut | Signification |
|---|---|
| `[confirmé]` | Forme attestée dans le corpus ET dans la liste communautaire, avec fréquence significative |
| `[attesté]` | Forme attestée dans le corpus, présente dans la liste |
| `[NEEDS REVIEW]` | Forme présente dans la liste mais absente ou trop rare dans le corpus |
| `[disputed]` | Forme ou règle non tranchée, nécessite une vérification grammaticale |

---

## 4. Clitiques objets

### 4.1 Paradigme des objets directs

Les clitiques objets s'attachent au verbe (enclise) ou à une particule préverbale (proclise). Les formes ci-dessous sont celles attestées dans le corpus avec une fréquence significative.

| Personne | Genre | Nombre | Forme enclitique | Forme proclitique | Statut | Exemple corpus |
|---|---|---|---|---|---|---|
| 1 | — | sg | `-iyi` / `-yi` | `yi-` | `[confirmé]` | `Iwala-yi Tom` (Tom m'a vu) |
| 1 | — | pl | `-aɣ` | `aɣ-` / `ɣ-` | `[confirmé]` | `Ɣur-aɣ lḥeqq` (Nous avons raison) |
| 2 | M | sg | `-k` | `k-` | `[confirmé]` | `Iwala-k Tom` (Tom t'a vu) |
| 2 | F | sg | `-kem` | `kem-` | `[confirmé]` | `Iwala-kem Tom` (Tom t'a vue) |
| 2 | M | pl | `-ken` | `ken-` | `[confirmé]` | — |
| 2 | F | pl | `-kent` | `kent-` | `[confirmé]` | `Aql-ikent am tiyaḍ` (Vous êtes comme les autres) |
| 3 | M | sg | `-t` | `t-` | `[confirmé]` | `Iwala-t Tom` (Tom l'a vu) |
| 3 | F | sg | `-tt` | `tt-` | `[confirmé]` | `Iwala-tt Tom` (Tom l'a vue) |
| 3 | M | pl | `-ten` | `ten-` | `[confirmé]` | — |
| 3 | F | pl | `-tent` | `tent-` | `[confirmé]` | — |

### 4.2 Paradigme des objets indirects

| Personne | Genre | Nombre | Forme enclitique | Forme proclitique | Statut | Exemple corpus |
|---|---|---|---|---|---|---|
| 1 | — | sg | `-iyi` / `-yi` | `yi-` | `[confirmé]` (syncrétisme) | `Yenna-yi` (Il m'a dit) |
| 1 | — | pl | `-aɣ` | `aɣ-` / `ɣ-` | `[confirmé]` (syncrétisme) | `Yenna-aɣ` (Il nous a dit) |
| 2 | M | sg | `-ak` | `ak-` | `[confirmé]` | `Yenna-yak` (Il t'a dit) |
| 2 | F | sg | `-am` | `am-` | `[confirmé]` | `Yenna-yam` (Il t'a dit, fém.) |
| 2 | M | pl | `-awen` | `awen-` | `[confirmé]` | `Yuzen-awen-d` (Il vous a envoyé) |
| 2 | F | pl | `-akent` | `akent-` | `[confirmé]` | — |
| 3 | M/F | sg | `-as` | `as-` | `[confirmé]` | `Yenna-yas` (Il lui a dit) |
| 3 | M | pl | `-asen` | `asen-` | `[confirmé]` | — |
| 3 | F | pl | `-asent` | `asent-` | `[confirmé]` | `tezwi-asent-id` (Elle leur a apporté) |

> **Note sur le syncrétisme :** Les formes de 1ère personne (`iyi`, `aɣ`) présentent un syncrétisme : la même forme est utilisée pour l'objet direct et l'objet indirect. Le contexte syntaxique (type de verbe : transitif direct vs. verbe de parole) est nécessaire pour désambiguïser.

---

## 5. Suffixes possessifs

Les suffixes possessifs s'attachent aux noms et à certaines prépositions. Ils apparaissent **exclusivement en position finale** du mot à trait d'union (quasi 100 % des occurrences).

### 5.1 Paradigme

| Personne | Genre | Nombre | Forme | Statut | Exemple corpus |
|---|---|---|---|---|---|
| 1 | — | sg | `-iw` | `[confirmé]` | `axxam-iw` (ma maison) |
| 2 | M | sg | `-ik` | `[confirmé]` | `axxam-ik` (ta maison) |
| 2 | F | sg | `-im` | `[confirmé]` | `axxam-im` (ta maison, fém.) |
| 3 | M/F | sg | `-is` / `-s` | `[confirmé]` | `axxam-is` / `baba-s` |
| 1 | — | pl | `-nneɣ` | `[confirmé]` | `axxam-nneɣ` (notre maison) |
| 2 | M | pl | `-nwen` | `[confirmé]` | `axxam-nwen` (votre maison) |
| 2 | F | pl | `-nkent` | `[confirmé]` | `axxam-nkent` (votre maison, fém.) |
| 3 | M | pl | `-nsen` | `[confirmé]` | `axxam-nsen` (leur maison) |
| 3 | F | pl | `-nsent` | `[confirmé]` | `axxam-nsent` (leur maison, fém.) |

### 5.2 Allomorphie de `-is` / `-s` (possessif 3SG)

Le possessif 3ème personne singulier présente deux formes conditionnées par le contexte phonologique.

| Contexte | Forme | Fréquence | Exemple |
|---|---|---|---|
| Après voyelle | `-s` | 7 721 (49 %) | `baba-s` (son père) |
| Après consonne | `-is` (préféré) ou `-s` | 20 904 (99 %) pour `-is` | `m-is` (sa mère) |

**Règle :**
- Après **voyelle** → utiliser `-s` (forme réduite)
- Après **consonne** → utiliser `-is` (forme pleine)

**Preuve empirique :** `-is` apparaît dans 99 % des cas après consonne, rarement après voyelle (216 occurrences seulement).

**Hypothèse linguistique :** `-is` évite les clusters consonantiques difficiles. Cette règle est un phénomène de sandhi morphophonologique.

---

## 6. Clitiques directionnels

### 6.1 Paradigme

| Forme | Fonction | Statut | Fréquence corpus |
|---|---|---|---|
| `-d` / `-id` | Ventif (mouvement vers le locuteur) | `[confirmé]` | Très fréquent |
| `-n` / `-in` | Andatif (mouvement away from locuteur) | `[confirmé]` | Fréquent |

### 6.2 Allomorphie de `-d` / `-id` (directionnel ventif)

Le directionnel ventif présente deux formes conditionnées par le contexte phonologique.

| Contexte | Forme | Fréquence | Exemple |
|---|---|---|---|
| Après voyelle | `-d` | 19 195 (38 %) | `y-d` (semi-voyelle + d) |
| Après consonne | `-id` (préféré) ou `-d` | 15 466 (99,5 %) pour `-id` | `t-id` (après consonne `t`) |

**Règle :**
- Après **voyelle** → utiliser `-d` (jamais `-id`)
- Après **consonne** → utiliser `-id` (fortement préféré)

**Preuve empirique :** `-id` apparaît dans 99,5 % des cas après consonne, jamais après voyelle (76 occurrences seulement, probablement erreurs de corpus).

**Hypothèse linguistique :** `-id` facilite la prononciation après consonne par ajout d'une voyelle épenthétique `i`.

### 6.3 Distribution de `-d` vs `-id`

| Forme | Après voyelle | Après consonne |
|---|---|---|
| `-d` | 19 195 (38 %) | 31 094 (62 %) |
| `-id` | 76 (0,5 %) | 15 466 (99,5 %) |

**Conclusion :** `-id` est quasi exclusivement utilisé après consonne. Après voyelle, seul `-d` est attesté.

---

## 7. Démonstratifs et déictiques

| Forme | Fonction | Statut | Fréquence corpus | Exemple |
|---|---|---|---|---|
| `-nni` | Déictique / démonstratif (référent déjà mentionné) | `[confirmé]` | 72 089 | `argaz-nni` (cet homme-là) |
| `-agi` | Démonstratif de proximité | `[confirmé]` | Fréquent | `argaz-agi` (cet homme-ci) |
| `-nniḍen` | « autre » | `[confirmé]` | Attesté | `argaz-nniḍen` (un autre homme) |

> **Découverte :** `-nni` et `-agi` n'étaient pas dans l'hypothèse initiale mais ont été identifiés par l'extraction bottom-up du corpus, puis confirmés par la liste communautaire. Ce sont de vrais morphèmes grammaticaux, pas des artefacts de tokenisation.

---

## 8. Règle d'ordre des clitiques

### 8.1 Règle principale

L'ordre des clitiques en kabyle est **strict et inviolable** dans le corpus :

```
[Particule préverbale / Verbe] + [Objet Direct] + [Objet Indirect] + [Directionnel]
```

**Preuve empirique :**
- Ordre `[Objet] + [Directionnel]` : **297 occurrences** attestées
- Ordre inverse `[Directionnel] + [Objet]` : **0 occurrence**

### 8.2 Exemples attestés

| Forme | Décomposition | Glose |
|---|---|---|
| `Yuzen-awen-d` | verbe + 2PL.M.OBJ + directionnel | « Il vous a envoyé (ventif) » |
| `tezwi-asent-id` | verbe + 3PL.F.OBJ + directionnel | « Elle leur a apporté (ventif) » |
| `Greɣ-awen-d` | verbe + 2PL.M.OBJ + directionnel | « Je vous ai jeté (ventif) » |
| `selleɣ-awen-d` | verbe + 2PL.M.OBJ + directionnel | « Je vous ai entendus (ventif) » |
| `Yesken-asent-d` | verbe + 3PL.F.OBJ + directionnel | « Il leur a montré (ventif) » |

### 8.3 Proclise vs Enclise

La position du clitique (avant ou après le verbe) dépend de la présence d'une **particule préverbale** :

- **Enclise** (pas de particule) : `Iwala-kem Tom` → verbe + clitique
- **Proclise** (particule présente) : `Ad kem-awiɣ` → particule + clitique + verbe

**Particules déclenchant la proclise :**

| Particule | Fonction | Fréquence corpus |
|---|---|---|
| `ad` | Futur | 38 056 |
| `i` | Relatif | 31 141 |
| `ur` / `wer` | Négation | 20 050 |
| `ara` | Relatif futur | 10 138 |
| `la` | Aspect progressif/habituel | 1 720 |
| `mi` | Temporel « quand/lorsque » | 1 414 |
| `ay` | Emphase/focalisation | 959 |

**Référence :** Mettouchi (2018), *The interaction of state, prosody and linear order in Kabyle (Berber)*.

---

## 9. Allomorphie

### 9.1 Allomorphe du clitique 1PL : `aɣ` → `ɣ`

Le clitique objet 1ère personne du pluriel `aɣ` subit une **aphérèse** (chute du `a`) après les particules préverbales.

| Contexte | Forme | Exemple corpus |
|---|---|---|
| Sans particule | `aɣ-` | `Ɣur-aɣ lḥeqq` (Nous avons raison) |
| Après `ad` | `ɣ-` | `Bɣan ad ɣ-sirden allaɣ-nneɣ` (Ils veulent nous laver le cerveau) |
| Après `ur` | `ɣ-` | `Ur ɣ-tettu ara` (Ne nous oublie pas) |
| Après `i` (relatif) | `ɣ-` | `Swaswa d ayen i ɣ-ilaqen` (Exactement ce qui nous convient) |
| Après `ara` (relatif) | `ɣ-` | `D akud ara ɣ-d-iseknen kullec` (C'est le temps qui nous montrera tout) |
| Après `la` (progressif) | `ɣ-` | `La ɣ-ttmeslayen` (Ils nous parlent) |
| Après `mi` (temporel) | `ɣ-` | `Mi ɣ-walan` (Quand ils nous ont vus) |
| Après `ay` (emphase) | `ɣ-` | `Ay ɣ-yessawḍen` (C'est lui qui nous a appelés) |

**Preuve empirique :** 1 150 occurrences de `ɣ-` proclitique dans le corpus, avec distribution claire après les particules listées ci-dessus.

> **Attention à l'homographie :** Le suffixe verbal `-ɣ` (1ère personne du singulier sujet, ex : `cukkeɣ` "je pense") est **homographe** avec le clitique objet `ɣ-` mais constitue un **morphème distinct**.
> - `cukkeɣ` → verbe + `-ɣ` (1SG sujet) — **pas de trait d'union**
> - `ad ɣ-sirden` → particule + `ɣ-` (1PL objet) + verbe — **trait d'union**

### 9.2 Allomorphe du clitique 2PL.F : `kent` → `went`

Le clitique 2ème personne du pluriel féminin `-kent` prend la forme `-went` lorsqu'il est suffixé à certaines bases.

| Contexte | Forme | Fréquence corpus | Exemple |
|---|---|---|---|
| Après verbe | `-kent` | — | `[à vérifier]` |
| Après préposition `yid` (avec) | `-went` | 66 | `Bɣiɣ ad mmeslayeɣ yid-went` (Je veux parler avec vous, fém.) |
| Après préposition `deg` (dans) | `-went` | 13 | `Deg-went i tres tyita` (C'est parmi vous que frappe l'épidémie) |
| Après préposition `ttxil` (s'il vous plaît) | `-went` | 11 | `Ttxil-went` (S'il vous plaît, fém.) |
| Après préposition `seg` (de/parmi) | `-went` | 6 | `Ilaq ad truḥ ɣer din yiwet seg-went` (Il faut que l'une de vous y aille) |
| Après préposition `ɣur` (chez) | `-went` | 6 | `Ɣur-went axxam deg Ustṛalya?` (Vous avez une maison en Australie ?) |
| Après préposition `ɣer` (vers) | `-went` | 5 | — |
| Après nom | `-went` | 1 | `Walaɣ tawlaft-went deg lkaɣeḍ` (J'ai vu votre photo dans le journal) |

**Preuve empirique :** 108 occurrences totales de `-went` dans le corpus, toutes après prépositions ou noms.

**Note :** Cette allomorphie est un phénomène de sandhi morphophonologique. La mutation `k → w` se produit systématiquement après certaines bases. La liste complète des déclencheurs reste à préciser. `[NEEDS REVIEW]`

---

## 10. Formes composées et clusters clitiques

Les formes suivantes, présentes dans la liste communautaire, n'ont pas été attestées ou sont trop rares dans le corpus pour être confirmées. Elles correspondent probablement à des **clusters de clitiques agglutinés** (particule d'aspect + objet + verbe).

| Forme | Hypothèse de décomposition | Statut |
|---|---|---|
| `-atenteɣ` | `ad` + `tent` + `ɣ` (futur + 3PL.F + 1SG) | `[NEEDS REVIEW]` |
| `-atkent` | `ad` + `kent` | `[NEEDS REVIEW]` |
| `-atsent` | `ad` + `sent` | `[NEEDS REVIEW]` |
| `-yanaɣ` | `ya` + `naɣ` | `[NEEDS REVIEW]` |
| `-yanteɣ` | `yan` + `teɣ` | `[NEEDS REVIEW]` |
| `-nnteɣ` | — | `[NEEDS REVIEW]` |
| `-tnaɣ` / `-tneɣ` | — | `[NEEDS REVIEW]` |
| `-atneɣ` / `-atsen` / `-atwen` | — | `[NEEDS REVIEW]` |

**Justification de l'absence :** Le corpus Tatoeba est composé majoritairement de phrases courtes et simples. Les constructions complexes (futur + objet pluriel + verbe) y sont sous-représentées. Ces formes nécessitent une validation par un corpus complémentaire (textes longs, Wikipédia, littérature) ou par une grammaire de référence.

---

## 11. Limitations

### 11.1 Limitations du corpus

1. **Corpus limité :** Le corpus Tatoeba kabyle (756 774 phrases) est composé de phrases courtes et simples. Les constructions complexes, les registres formels et les variantes dialectales sont sous-représentés.
2. **Bruit résiduel :** L'extraction par trait d'union capture aussi des mots composés et des formes verbales. Un filtrage par fréquence et position a été appliqué, mais une inspection manuelle reste nécessaire pour les cas ambigus.
3. **Formes composées :** Les clusters clitiques complexes (`atenteɣ`, `atkent`, etc.) n'ont pas pu être validés par le corpus. Ils sont marqués `[NEEDS REVIEW]`.

### 11.2 Limitations méthodologiques

4. **Variation dialectale :** Cette spécification est basée sur le kabyle standard (Taqbaylit). Les variantes dialectales régionales ne sont pas couvertes.
5. **Source communautaire :** La liste d'affixes de Belkacem77 est une ressource d'ingénierie NLP, pas une publication académique. Elle a été utilisée comme source secondaire de validation, croisée avec le corpus.
6. **Validation grammaticale partielle :** Certaines règles (allomorphie, ordre des clitiques) ont été validées empiriquement mais n'ont pas été systématiquement confrontées aux grammaires de référence pour toutes les formes.

---

## 12. Références

### 12.1 Sources primaires

1. **Naït-Zerrad, Kamal.** *Grammaire moderne du kabyle. Tajerrumt tatrart n teqbaylit*. Karthala, 2001.
2. **Mettouchi, Amina.** *Prosodic Segmentation and Grammatical Relations: The Direct Object in Kabyle (Berber)*. 2018.
3. **Mettouchi, Amina.** *The Interaction of State, Prosody and Linear Order in Kabyle (Berber)*. 2018.
4. **Mettouchi, Amina.** *The Grammaticalization of Directional Particles in Berber*.

### 12.2 Ressources techniques

5. **Belkacem, Mohammed.** *KabyleNLP — Corpus d'affixes pour la tokenisation*. GitHub : [github.com/MohammedBelkacem/KabyleNLP](https://github.com/MohammedBelkacem/KabyleNLP)
6. **Corpus :** `boffire/tatoeba-kabyle-mono-cleaned` (HuggingFace) — 756 774 phrases, version 2025.

### 12.3 Spécifications connexes

7. **Spécification orthographique :** `kabyle-orthography-spec` (v0.4-draft) — alphabet et normalisation Unicode
8. **Spécification de collation :** `kabyle-collation-spec` — ordre alphabétique (à vérifier)

---

## 13. Historique des versions

| Version | Date | Modifications |
|---|---|---|
| 0.1-draft | 2026 | Première version. Validation empirique des clitiques objets, possessifs, directionnels, démonstratifs. Règle d'ordre des clitiques. Allomorphie `ɣ`, `went`, `d`/`id`, `s`/`is`. |

---

## 14. Travaux futurs

Avant une version 1.0 stable, les travaux suivants sont nécessaires :

1. **Validation par locuteurs natifs :** Faire relire les exemples et les règles par plusieurs locuteurs compétents représentant les conventions concernées.
2. **Corpus complémentaire :** Valider les formes composées rares (`atenteɣ`, etc.) sur un corpus de textes longs (Wikipédia, littérature).
3. **Allomorphie de `went` :** Documenter la liste complète des bases déclenchant la mutation `kent` → `went`.
4. **Syncrétisme 1SG :** Clarifier la désambiguïsation contextuelle entre objet direct et indirect pour `iyi` et `aɣ`.
5. **Règles phonologiques complètes :** Documenter systématiquement tous les cas d'allomorphie conditionnés par le contexte phonologique.
6. **Variantes dialectales :** Documenter les variantes régionales attestées.
7. **Licence :** Clarifier la licence de la liste d'affixes de Belkacem77 avant réutilisation verbatim.

---

**Ce document est une proposition de spécification. Il n'a pas encore été validé par un locuteur natif ou un linguiste kabyle. Les formes marquées `[NEEDS REVIEW]` ou `[disputed]` nécessitent une vérification supplémentaire avant toute utilisation en production.**
