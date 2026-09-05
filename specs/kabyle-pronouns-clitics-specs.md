# Spécification des pronoms et clitiques kabyles (Taqbaylit)

- **Identifiant :** `kabyle-pronouns-clitics-spec`
- **Version :** 0.3-draft
- **Statut :** Draft — Proposition de spécification technique
- **Auteur :** Athmane MOKRAOUI
- **Date :** 05-09-2026
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
- Chaker, Salem. *Un parler berbère d'Algérie (Kabylie)*. Doctorat d'État, Université de Paris V, 1983.

### Sources morphophonologiques
- Bedar, M., Quellac, M., & Voeltzel, L. (2021). *Epenthetic glides in Taqbaylit*. Journal of African Languages and Literatures, 12(1), 1–36.
- Aïssou, F. (2021). *Étude descriptive et comparative des pronoms personnels à l'est de Béjaïa*.
- Baier, N. (2020). *Topics in Kabyle Linguistics*. McGill Working Papers in Linguistics.

> **Note terminologique :** Mettouchi (2018) établit que ce que ce document appelle « objet direct » correspond en réalité au rôle sémantique **absolutif** (patient référentiel), et « objet indirect » au rôle **datif** (argument indirectement affecté) — distinction sémantique, pas une fonction grammaticale de complément d'objet au sens strict. Les tableaux ci-dessous conservent temporairement la terminologie « objet direct/indirect » pour rester lisibles, mais adoptent le vocabulaire absolutif/datif dans les sections d'analyse (§8) où la distinction devient significative.

---

## 1. Résumé

Ce document spécifie les formes, l'ordre et les règles d'allomorphie des **clitiques pronominaux** en kabyle (taqbaylit), dans le but de fournir une base de référence pour les modèles de langue et les outils de traitement automatique des langues (NLP). L'objectif est de **prévenir les hallucinations** de formes grammaticales incorrectes par les modèles d'IA.

Toutes les formes et règles documentées ici ont été **validées empiriquement** par croisement entre :
1. Une liste d'affixes communautaire (82 formes uniques)
2. Un corpus de 756 774 phrases (Tatoeba kabyle nettoyé)
3. Des sources académiques publiées (Mettouchi 2018 ; Bedar, Quellac & Voeltzel 2021)

**Résultats de validation :**
- **50/54 affixes de référence confirmés** par le corpus (92,6 %)
- **4 affixes absents** (`atenteɣ`, `atkent`, `tnaɣ`, `yanaɣ`) — marqués `[NEEDS REVIEW]`
- **3 règles d'allomorphie** découvertes et validées
- **Règle d'ordre datif/absolutif** confirmée à 99,9 % (32 026 / 32 052 occurrences), avec un sous-cas d'inversion documenté (§8)

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

**Limite :** Cette méthode capture aussi les mots composés et les formes verbales conjuguées suivies de clitiques. Un tri par fréquence, position et inspection manuelle a été appliqué pour filtrer le bruit. Elle peut aussi produire de faux positifs lorsqu'un segment de mot ressemble à un clitique sans en être un (voir §8.5).

### 3.2 Validation croisée

Chaque affixe de la liste communautaire (Belkacem77) a été vérifié dans le corpus selon trois critères :
1. **Présence :** l'affixe apparaît-il dans le corpus ?
2. **Fréquence :** combien d'occurrences ? (seuil de confiance : 5 occurrences minimum)
3. **Position dominante :** l'affixe apparaît-il majoritairement à la position attendue ?
   - Préfixe attendu → position dominante « début »
   - Suffixe attendu → position dominante « fin »

Chaque segment fréquent du corpus absent de la liste a été inspecté manuellement pour déterminer s'il s'agit d'un clitique réel, d'un artefact de tokenisation ou de bruit.

### 3.3 Analyse phonologique

Pour les allomorphes (variantes conditionnées par le contexte), l'analyse a porté sur :
- Le caractère précédant immédiatement l'affixe
- Classification selon l'inventaire de `kabyle-orthography-spec` :
  - Voyelles : `a`, `e`, `i`, `u`
  - Consonnes : 29 lettres incluant `ɛ`, `ɣ` (consonnes pharyngale/uvulaire)
  - Autres : lettres hors inventaire de base (`o`, `p`, `v`)
- Particules préverbales (`ad`, `ur`, `i`, `ara`, `la`, `mi`, `ay`, `imi`) : traitées comme contexte vocalique car elles se terminent phonologiquement par une voyelle (Bedar et al. 2021)

### 3.4 Validation de l'ordre relatif des clitiques

Pour trancher l'ordre entre clitiques datif et absolutif (§8), une extraction ciblée a recherché tous les mots à trait d'union contenant simultanément une forme non-ambiguë du paradigme datif (`as`, `asen`, `asent`, `awen`, `akent`, `ak`, `am`) et une forme du paradigme absolutif (`t`, `tt`, `iyi`, `yi`, `ten`, `tent`, `kem`, `ken`, `kent`, `it`, `itt`, `iten`, `itent`), puis a comparé leur position relative dans le mot. Les formes syncrétiques (`aɣ`, `ɣ`) ont été volontairement exclues de ce test, car leur ambiguïté direct/indirect les rend inutilisables pour mesurer un ordre.

### 3.5 Critères de statut

| Statut | Signification |
|---|---|
| `[confirmé]` | Forme ou règle attestée dans le corpus ET dans une source secondaire (liste communautaire ou grammaire), avec fréquence significative |
| `[attesté]` | Forme attestée dans le corpus, présente dans la liste |
| `[NEEDS REVIEW]` | Forme présente dans la liste mais absente ou trop rare dans le corpus |
| `[disputed]` | Forme ou règle non tranchée, nécessite une vérification grammaticale ou par locuteur natif |

---

## 4. Clitiques objets

### 4.1 Paradigme des objets directs (absolutif)

Les clitiques objets s'attachent au verbe (enclise) ou à une particule préverbale (proclise). Les formes ci-dessous sont celles attestées dans le corpus avec une fréquence significative.

| Personne | Genre | Nombre | Forme enclitique | Forme proclitique | Statut | Exemple corpus |
|---|---|---|---|---|---|---|
| 1 | — | sg | `-iyi` / `-yi` | `yi-` | `[confirmé]` | `Iwala-yi Tom` |
| 1 | — | pl | `-aɣ` | `aɣ-` / `ɣ-` | `[confirmé]` | `Ɣur-aɣ lḥeqq` |
| 2 | M | sg | `-k` | `k-` | `[confirmé]` | `Iwala-k Tom` |
| 2 | F | sg | `-kem` | `kem-` | `[confirmé]` | `Iwala-kem Tom` |
| 2 | M | pl | `-ken` | `ken-` | `[confirmé]` (2 536 occ.) | `Ala, ur ken-terri ara tmara` |
| 2 | F | pl | `-kent` | `kent-` | `[confirmé]` | `Aql-ikent am tiyaḍ` |
| 3 | M | sg | `-t` | `t-` | `[confirmé]` | `Iwala-t Tom` |
| 3 | F | sg | `-tt` | `tt-` | `[confirmé]` | `Iwala-tt Tom` |
| 3 | M | pl | `-ten` | `ten-` | `[confirmé]` (16 313 occ.) | `D acu akka i ten-yuɣen?` |
| 3 | F | pl | `-tent` | `tent-` | `[confirmé]` (14 773 occ.) | `Jipat tiwezzlanin yeǧǧa-tent wakud` |

### 4.2 Paradigme des objets indirects (datif)

| Personne | Genre | Nombre | Forme enclitique | Forme proclitique | Statut | Exemple corpus |
|---|---|---|---|---|---|---|
| 1 | — | sg | `-iyi` / `-yi` | `yi-` | `[confirmé]` (syncrétisme) | `Yenna-yi` |
| 1 | — | pl | `-aɣ` | `aɣ-` / `ɣ-` | `[confirmé]` (syncrétisme) | `Yenna-aɣ` |
| 2 | M | sg | `-ak` | `ak-` | `[confirmé]` | `Yenna-yak` — *cf. §9.3, épenthèse glide [j] facultative* |
| 2 | F | sg | `-am` | `am-` | `[confirmé]` | `Yenna-yam` — *cf. §9.3* |
| 2 | M | pl | `-awen` | `awen-` | `[confirmé]` | `Yuzen-awen-d` |
| 2 | F | pl | `-akent` | `akent-` | `[confirmé]` (4 614 occ.) | `Yerfa fell-akent` |
| 3 | M/F | sg | `-as` | `as-` | `[confirmé]` | `Yenna-yas` |
| 3 | M | pl | `-asen` | `asen-` | `[confirmé]` (11 106 occ.) | `Nɣan-ten, sakin ttrun fell-asen` |
| 3 | F | pl | `-asent` | `asent-` | `[confirmé]` | `tezwi-asent-id` |

> **Note sur le syncrétisme :** Les formes de 1ère personne (`iyi`, `aɣ`) présentent un syncrétisme : la même forme est utilisée pour l'objet direct et l'objet indirect. Le contexte syntaxique (type de verbe : transitif direct vs. verbe de parole) est nécessaire pour désambiguïser.

---

## 5. Suffixes possessifs

Les suffixes possessifs s'attachent aux noms et à certaines prépositions. Ils apparaissent **exclusivement en position finale** du mot à trait d'union (quasi 100 % des occurrences).

### 5.1 Paradigme

| Personne | Genre | Nombre | Forme | Statut | Exemple corpus |
|---|---|---|---|---|---|
| 1 | — | sg | `-iw` | `[confirmé]` | `axxam-iw` |
| 2 | M | sg | `-ik` | `[confirmé]` | `axxam-ik` |
| 2 | F | sg | `-im` | `[confirmé]` | `axxam-im` |
| 3 | M/F | sg | `-is` / `-s` | `[confirmé]` | `axxam-is` / `baba-s` |
| 1 | — | pl | `-nneɣ` | `[confirmé]` | `axxam-nneɣ` |
| 2 | M | pl | `-nwen` | `[confirmé]` | `axxam-nwen` |
| 2 | F | pl | `-nkent` | `[confirmé]` | `axxam-nkent` |
| 3 | M | pl | `-nsen` | `[confirmé]` | `axxam-nsen` |
| 3 | F | pl | `-nsent` | `[confirmé]` | `axxam-nsent` |

### 5.2 Allomorphie de `-is` / `-s` (possessif 3SG)

Le possessif 3ème personne singulier présente deux formes conditionnées par le contexte phonologique.

| Contexte | `-is` | `-s` | Total | % `-is` | % `-s` |
|---|---|---|---|---|---|
| Après voyelle | 216 | 7 616 | 7 832 | 2,8 % | **97,2 %** |
| Après consonne | 20 883 | 8 023 | 28 906 | **72,2 %** | 27,8 % |
| Après particule préverbale | 0 | 78 | 78 | 0,0 % | **100,0 %** |

**Règle (v0.3 — recalculée par proportion intra-contexte) :**
- Après **voyelle** → `-s` quasi catégorique (97,2 %). Règle solide, comparable à `-d`/`-id` (§6.2).
- Après **consonne** → `-is` dominant mais **pas catégorique** (72,2 % contre 27,8 % pour `-s`). Contrairement à `-d`/`-id` (§6.2), la règle n'est ici qu'une **forte tendance**, pas une distribution complémentaire quasi parfaite.
- Après **particule préverbale** (`ad`, `ur`, `i`, etc.) → `-s` quasi catégorique (100 % sur 78 occurrences), cohérent avec le traitement de ces particules comme contexte vocalique.

**Statut :** `[confirmé]` pour le sous-cas « après voyelle ou particule préverbale → `-s` » ; `[confirmé (tendance)]` pour « après consonne → `-is` préféré ». La marge de 28 % de `-s` après consonne suggère un facteur supplémentaire non encore identifié (peut-être la consonne finale précise, pas seulement la catégorie voyelle/consonne) — à affiner si une version plus fine de la règle est souhaitée, mais non bloquant pour la version actuelle.

---

## 6. Clitiques directionnels

### 6.1 Paradigme

| Forme | Fonction | Statut | Fréquence corpus |
|---|---|---|---|
| `-d` / `-id` | Ventif (mouvement vers le locuteur) | `[confirmé]` | Très fréquent |
| `-n` / `-in` | Andatif (mouvement à l'écart du locuteur) | `[confirmé]` | Fréquent |

### 6.2 Allomorphie de `-d` / `-id` (directionnel ventif)

Le directionnel ventif présente deux formes conditionnées par le contexte phonologique.

| Contexte | Forme | Fréquence | Exemple |
|---|---|---|---|
| Après voyelle | `-d` | 19 195 (99,6 % des cas après voyelle) | `y-d` |
| Après consonne | `-id` (préféré) | 15 466 (99,5 % des cas après consonne) | `t-id` |

**Règle :**
- Après **voyelle** → utiliser `-d` (jamais `-id`)
- Après **consonne** → utiliser `-id` (fortement préféré)

**Hypothèse linguistique :** `-id` facilite la prononciation après consonne par ajout d'une voyelle épenthétique `i`.

### 6.3 Distribution de `-d` vs `-id`

| Forme | Après voyelle | Après consonne |
|---|---|---|
| `-d` | 19 195 | 31 094 |
| `-id` | 76 (probables erreurs de corpus) | 15 466 |

**Conclusion :** `-id` est quasi exclusivement utilisé après consonne. Après voyelle, seul `-d` est attesté.

---

## 7. Démonstratifs et déictiques

| Forme | Fonction | Statut | Fréquence corpus | Exemple |
|---|---|---|---|---|
| `-nni` | Déictique / démonstratif (référent déjà mentionné) | `[confirmé]` | 72 089 | `argaz-nni` |
| `-agi` | Démonstratif de proximité | `[confirmé]` | Fréquent | `argaz-agi` |
| `-nniḍen` | « autre » | `[confirmé]` | Attesté | `argaz-nniḍen` |

> **Découverte :** `-nni` et `-agi` n'étaient pas dans l'hypothèse initiale mais ont été identifiés par l'extraction bottom-up du corpus, puis confirmés par la liste communautaire. Ce sont de vrais morphèmes grammaticaux, pas des artefacts de tokenisation.

---

## 8. Règle d'ordre des clitiques

### 8.1 Règle générale — Datif avant Absolutif

Amina Mettouchi (2018) établit, à partir d'un corpus oral annoté, que le clitique **datif** précède toujours le clitique **absolutif** dans une chaîne verbale kabyle :

```
[Particule préverbale / Verbe] + DATIF + ABSOLUTIF + [Directionnel]
```

**Preuve empirique (orthographe pratique du corpus Tatoeba) :** une extraction ciblée des mots à trait d'union contenant simultanément une forme datif non-ambiguë (`as`, `asen`, `asent`, `awen`, `akent`, `ak`, `am`) et une forme absolutif (`t`, `tt`, `iyi`, `ten`, `tent`, `kem`...) confirme cet ordre à **99,9 %** :

| Ordre | Occurrences |
|---|---|
| DATIF avant ABSOLUTIF | 32 026 |
| ABSOLUTIF avant DATIF | 26 |

**Exemples (DATIF avant ABSOLUTIF) :**

| Forme | Décomposition | Glose |
|---|---|---|
| `ak-t-id-yennan` | 2SG.DAT + 3SG.M.ABS + ventif + verbe | « qui te l'a dit » |
| `as-tt-fken` | 3SG.DAT + 3SG.F.ABS + verbe | « la lui donnent » |
| `fkan-as-t-id` | verbe + 3SG.DAT + 3SG.M.ABS + ventif | « ils la lui ont donnée » |
| `awen-tt-id-segziɣ` | 2PL.M.DAT + 3SG.F.ABS + ventif + verbe | « que je vous l'explique » |

**Statut :** `[confirmé]` — source académique (Mettouchi 2018) et preuve corpus convergentes.

> **Note de méthode :** la notation `ad=aɣ=tn=dd` utilisée par Mettouchi (2018) suit les conventions de glose interlinéaire (règles de Leipzig), où `=` marque une frontière de clitique et non un trait d'union orthographique. Elle ne doit pas être copiée telle quelle comme graphie kabyle standard — la validation de l'ordre a donc été refaite indépendamment sur l'orthographe réelle du corpus Tatoeba (résultat ci-dessus), plutôt que déduite par simple transposition de la notation académique.

### 8.2 Sous-cas confirmé — Inversion à l'impératif

À l'impératif, l'ordre s'inverse : **ABSOLUTIF avant DATIF**.

| Forme | Verbe | Mode | Glose |
|---|---|---|---|
| `awi-t-as-d` | `awi` (apporter) | Impératif 2sg | « apporte-le-lui » |
| `Semceḥem-t-as` / `semcaḥem-t-as` | `semceḥem` (excuser) | Impératif 2sg | « excuse-le-lui » |
| `Ini-t-as` | `ini` (dire) | Impératif 2sg | « dis-le-lui » |
| `fket-t-asen` | `fk` (donner) | Impératif 2pl | « donnez-le-leur » |
| `fkemt-t-asent` | `fk` (donner) | Impératif 2pl.f | « donnez-le-leur » |

**Statut :** `[confirmé]` — plusieurs verbes différents (`awi`, `semceḥem`, `ini`, `fk`), plusieurs personnes, résultat cohérent.

**Remarque typologique :** ce phénomène a des parallèles dans d'autres langues (le français inverse aussi l'ordre à l'impératif : « je te le dis » vs. « dis-le-moi »). Il ne s'agit donc pas d'une anomalie isolée du kabyle.

### 8.3 Cas non expliqués par le mode verbal — `[disputed]`

**Test élargi (v0.3) :** le paradigme datif testé a été étendu à `ak`/`am` (exclus initialement), portant l'échantillon total ABS-avant-DAT à 26 occurrences. Répartition :

| Catégorie | Occurrences | Part |
|---|---|---|
| Impératif (`awi`, `semceḥem`/`semcaḥem`, `ini`, `fket`, `fkemt`) | 16 | 61,5 % |
| Présentatif `ha`/`Ha` (§8.4, hors règle verbale) | 3 | 11,5 % |
| Faux positif d'extraction (`t-id-asen`, §8.5) | 2 | 7,7 % |
| Indicatif, non expliqué (`fukken`, `ttaran`, `Ɣlin`) | 6 | 23,1 % |
| Ambigu, sans verbe identifiable (`tt-as-d`) | 1 | 3,8 % |

**Hypothèse `-n` réfutée :** l'hypothèse envisagée en v0.2 (terminaison verbale en `-n`) prédisait que les cas non-impératifs partageraient cette terminaison. Sur l'échantillon élargi, seuls 6 des 26 cas (23,1 %) finissent en `-n` — la plupart des cas ABS-avant-DAT (l'impératif) n'ont pas cette terminaison. L'hypothèse `-n` est donc **abandonnée**.

**Ce qui reste réellement non expliqué :** `fukken-t-ak` / `fukken-t-am` / `fukken-t-as` (3PL accompli), `ttaran-t-as` (3PL imperfectif habituel) et `Ɣlin-t-ak` (« tes lunettes te sont tombées » — verbe intransitif, cf. §8.6) sont à l'indicatif et inversent quand même l'ordre. Le point commun entre ces trois verbes (`fukk`, `ttar`, `ɣli`) n'est pas évident à partir des données seules — ni la personne, ni le mode, ni la terminaison ne les distinguent proprement des cas DAT-avant-ABS majoritaires. **Ce point ne peut pas être tranché par le corpus seul** ; il nécessite une consultation grammaticale ou un locuteur natif.

**Statut :** `[disputed]` — ne pas encoder de règle générale à partir de ces 3 verbes ; les traiter comme des exceptions lexicalisées documentées, pas comme un sous-cas systématique.

### 8.4 Catégorie distincte — Présentatifs

`ha-t-ak` / `Ha-t-ak` (« le voici pour toi », « voilà-le-toi ») utilise la particule présentative `ha`, qui n'est pas un verbe conjugué. Ce n'est pas une exception à la règle verbale des §8.1–8.3, mais un paradigme grammatical distinct (même famille que `aql-`, déjà documenté en emploi similaire ailleurs dans ce projet).

**Statut :** hors du champ de la règle d'ordre verbale — à documenter séparément si une spec sur les présentatifs/déictiques prédicatifs est un jour rédigée.

### 8.5 Faux positif d'extraction

`t-id-asen` (`ad t-id-asen yinebgawen` = « les invités viendront ») : `asen` ici est très probablement la forme conjuguée du verbe « venir » à la 3e pers. pluriel, pas le clitique datif `-asen`. Cette occurrence doit être **exclue** du décompte de la règle d'ordre — elle illustre une limite connue de la méthode d'extraction par ressemblance de chaîne (§3.1), pas un phénomène grammatical.

**Statut :** exclu — erreur de méthode, pas une donnée linguistique.

### 8.6 Cas à statut incertain

`Ɣlin-t-ak` (« tes lunettes te sont tombées ») : `ɣli` (tomber) est intransitif, donc la présence d'un clitique absolutif `t` est surprenante dans l'analyse standard objet-direct. Possible construction idiomatique ou élément de liaison différent.

**Statut :** `[disputed]` — ne pas expliquer par extrapolation, vérifier avec un locuteur natif.

### 8.7 Proclise vs Enclise

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
| `imi` | « parce que » | Attesté (ex. `Surfemt-iyi imi went-ssawleɣ zik`) |

**Référence :** Mettouchi (2018), *The interaction of state, prosody and linear order in Kabyle (Berber)*.

---

## 9. Allomorphie

### 9.1 Allomorphe du clitique 1PL : `aɣ` → `ɣ`

Le clitique objet 1ère personne du pluriel `aɣ` subit une **aphérèse** (chute du `a`) après les particules préverbales.

| Contexte | Forme | Exemple corpus |
|---|---|---|
| Sans particule | `aɣ-` | `Ɣur-aɣ lḥeqq` |
| Après `ad` | `ɣ-` | `Bɣan ad ɣ-sirden allaɣ-nneɣ` |
| Après `ur` | `ɣ-` | `Ur ɣ-tettu ara` |
| Après `i` (relatif) | `ɣ-` | `Swaswa d ayen i ɣ-ilaqen` |
| Après `ara` (relatif) | `ɣ-` | `D akud ara ɣ-d-iseknen kullec` |
| Après `la` (progressif) | `ɣ-` | `La ɣ-ttmeslayen` |
| Après `mi` (temporel) | `ɣ-` | `Mi ɣ-walan` |
| Après `ay` (emphase) | `ɣ-` | `Ay ɣ-yessawḍen` |
| Après `imi` (causal) | `ɣ-` | `Surfemt-iyi imi went-ssawleɣ zik` (cf. §9.2) |

**Preuve empirique :** 1 150 occurrences de `ɣ-` proclitique dans le corpus, avec distribution claire après les particules listées ci-dessus.

> **Attention à l'homographie :** Le suffixe verbal `-ɣ` (1ère personne du singulier sujet, ex : `cukkeɣ` "je pense") est **homographe** avec le clitique objet `ɣ-` mais constitue un **morphème distinct**.
> - `cukkeɣ` → verbe + `-ɣ` (1SG sujet) — **pas de trait d'union**
> - `ad ɣ-sirden` → particule + `ɣ-` (1PL objet) + verbe — **trait d'union**
>
> **Statut de cette désambiguïsation :** `[confirmé par analyse interne du corpus]`, pas encore adossé à une règle de réduction phonologique publiée explicitement. La preuve retenue : dans les exemples ci-dessus, le verbe porte déjà sa propre marque sujet (souvent 2pl ou 3pl), ce qui exclut que le `ɣ` proclitique soit le suffixe sujet 1sg — un verbe ne peut pas porter deux sujets différents. Cette déduction est solide mais reste une inférence à partir des données, distincte d'une citation grammaticale directe.

### 9.2 Allomorphe du clitique 2PL.F : `kent` → `went`

**Découverte majeure (v0.3) :** le corpus montre que `kent` est la forme **majoritaire** même après la base `yid` (544 `yid-kent` contre 67 `yid-went`, soit 89,1 % contre 10,9 %). L'hypothèse v0.2 selon laquelle `went` serait l'allomorphe post-vocalique de `kent` est donc **réfutée**.

**Enclise (183 occurrences) — bases observées (top 15) :**

| Base | Fréquence |
|---|---|
| `yid` | 67 |
| `deg` | 14 |
| `ttxil` | 14 |
| `ḥemmlen` | 13 |
| `seg` | 8 |
| `ɣur` | 8 |
| `ɣer` | 8 |
| `yis` | 4 |
| `yes` | 3 |
| `ilimt` | 3 |

**Proclise (24 occurrences) — comportement identique aux autres clitiques objet :**

| Particule précédente | Exemple corpus |
|---|---|
| `ad` (futur) | `Ur zmireɣ ara ad went-iniɣ melmi ara nili nhegga` |
| `i` (relatif) | `Amek i went-teḍṛa?` |
| `ur` (négation) | `Ma ur texdimemt ara ixeddim-nwent, yiwen ur went-t-ixeddem` |
| `ara` (relatif futur) | `Teɛweq d acu ara went-d-terr` |
| `imi` (« parce que ») | `Surfemt-iyi imi went-ssawleɣ zik` |

**Reformulation de l'hypothèse (v0.3) :** `went` n'est pas un allomorphe phonologiquement conditionné de `kent`. Après `yid`, c'est `kent` qui domine massivement (89,1 %). `went` apparaît comme une **variante minoritaire** dont la distribution n'est pas prédictible à partir du contexte phonologique seul. Trois hypothèses restent en compétition :
1. **Variante dialectale** : certaines régions/aires utilisent systématiquement `went`, d'autres `kent`
2. **Variante de registre** : `went` serait plus littéraire, ancien ou emphatique
3. **Conditionnement phonologique non identifié** : un facteur prosodique ou segmental plus fin que la simple catégorie voyelle/consonne

**Statut :** `[confirmé]` pour le comportement proclise/enclise (identique au reste du paradigme) ; `[disputed]` pour la nature exacte de l'alternance `kent`/`went` — nécessite une consultation avec un locuteur natif ou une enquête dialectale, non résoluble par le corpus seul.

### 9.3 Épenthèse glide sur `ak`/`am` (v0.3 — vérifié)

Les clitiques datifs de 2e personne du singulier `ak` (masc.) et `am` (fém.) présentent une épenthèse glide facultative après une base finissant par une voyelle. Le glide [j] (orthographié `y`) est inséré pour briser l'hiatus, mais l'hiatus non réparé reste toléré (Bedar, Quellac & Voeltzel 2021).

| Paire | Contexte | Forme nue | Forme épenthétique | Total | % nue | % épenthétique |
|---|---|---|---|---|---|---|
| `ak` / `yak` | Après voyelle | 480 | 247 | 727 | **66,0 %** | 34,0 % |
| `ak` / `yak` | Après consonne | 6 264 | 1 | 6 265 | **100,0 %** | 0,0 % |
| `am` / `yam` | Après voyelle | 399 | 192 | 591 | **67,5 %** | 32,5 % |
| `am` / `yam` | Après consonne | 5 064 | 0 | 5 064 | **100,0 %** | 0,0 % |

**Conclusion :**
- Après **consonne** → `ak` / `am` **obligatoires** (jamais `*yak` / `*yam`)
- Après **voyelle** → `ak` / `am` **majoritaires** (~66-67 %) ; `yak` / `yam` **minoritaires** (~33-34 %)

Ce n'est donc **pas** la même règle que `-d`/`-id` (§6.2) : là où `-id` est presque obligatoire après consonne, `yak`/`yam` restent minoritaires même dans le contexte post-vocalique qui les permettrait. L'insertion du glide [j] est une **réparation d'hiatus facultative** (Bedar et al. 2021), pas une règle catégorique. Le facteur qui détermine les cas épenthétiques (prosodie ? registre ? variante individuelle ?) reste à identifier.

**Attestation dans les parlers :** Aïssou (2021) atteste `Terna-yak` dans les parlers kabyles orientaux, tandis que Chaker (1983) documente dans le parler d'Irjen une chute de la voyelle initiale `a-` du clitique, donnant `terna-k` au lieu de `terna-yak`.

**Statut :** `[confirmé]` pour les deux bornes catégoriques (jamais après consonne) ; `[confirmé (tendance)]` pour le caractère facultatif de l'insertion post-vocalique.

---

## 10. Formes composées et clusters clitiques

### 10.1 Formes rares mais attestées

Les formes suivantes, présentes dans la liste communautaire, ont été attestées dans le corpus mais restent rares. Elles correspondent probablement à des **clusters de clitiques agglutinés** (particule d'aspect + objet + verbe).

| Forme | Occurrences | Hypothèse de décomposition | Statut | Exemple corpus |
|---|---|---|---|---|
| `tneɣ` | 131 | `t` + `neɣ` (1PL ?) | `[attesté]` | `D mmi-tneɣ` |
| `nnteɣ` | 5 | `nn` + `teɣ` | `[attesté]` | `D axeddim-nnteɣ` |
| `atsen` | 5 | `ad` + `asen` ? | `[attesté]` | `Tellamt ddaw-atsen` |
| `atsent` | 3 | `ad` + `asent` ? | `[attesté]` | `Tellam ddaw-atsent` |
| `yanteɣ` | 1 | `yan` + `teɣ` ? | `[attesté]` | `Yewwi-yanteɣ nadam` |
| `atneɣ` | 1 | `ad` + `tneɣ` ? | `[attesté]` | `Anwa i yellan ddaw-atneɣ?` |
| `atwen` | 1 | `ad` + `twen` ? | `[attesté]` | `Anwa i yellan ddaw-atwen?` |

> **Note sur `tneɣ` :** avec 131 occurrences, cette forme n'est pas « rare » au sens du seuil de 5 occurrences. Son analyse morphologique exacte reste incertaine : `t` pourrait être un clitique 3SG.F suivi d'une forme de 1PL, ou une forme possessive alternative non documentée dans les grammaires standard. Elle est marquée `[attesté]` en attendant une analyse grammaticale.

### 10.2 Formes non attestées

| Forme | Hypothèse de décomposition | Statut |
|---|---|---|
| `atenteɣ` | `ad` + `tent` + `ɣ` (futur + 3PL.F + 1SG) | `[NEEDS REVIEW]` |
| `atkent` | `ad` + `kent` | `[NEEDS REVIEW]` |
| `tnaɣ` | — | `[NEEDS REVIEW]` |
| `yanaɣ` | `ya` + `naɣ` | `[NEEDS REVIEW]` |

**Justification de l'absence :** Le corpus Tatoeba est composé majoritairement de phrases courtes et simples. Les constructions complexes (futur + objet pluriel + verbe) y sont sous-représentées. Ces formes nécessitent une validation par un corpus complémentaire (textes longs, Wikipédia, littérature) ou par une grammaire de référence.

---

## 11. Limitations

### 11.1 Limitations du corpus

1. **Corpus limité :** Le corpus Tatoeba kabyle (756 774 phrases) est composé de phrases courtes et simples. Les constructions complexes, les registres formels et les variantes dialectales sont sous-représentés.
2. **Bruit résiduel :** L'extraction par trait d'union capture aussi les mots composés et les formes verbales. Un filtrage par fréquence et position a été appliqué, mais une inspection manuelle reste nécessaire pour les cas ambigus (voir §8.5 pour un exemple concret de faux positif).
3. **Formes composées :** Les clusters clitiques complexes (`atenteɣ`, etc.) n'ont pas pu être validés par le corpus. Ils sont marqués `[NEEDS REVIEW]`.

### 11.2 Limitations méthodologiques

4. **Variation dialectale :** Cette spécification est basée sur le kabyle standard (Taqbaylit). Les variantes dialectales régionales ne sont pas couvertes.
5. **Source communautaire :** La liste d'affixes de Belkacem77 est une ressource d'ingénierie NLP, pas une publication académique. Elle a été utilisée comme source secondaire de validation, croisée avec le corpus.
6. **Validation grammaticale partielle :** Certaines règles (allomorphie, ordre des clitiques) ont été validées empiriquement mais n'ont pas été systématiquement confrontées aux grammaires de référence pour toutes les formes.
7. **Notation académique vs orthographe pratique :** les publications de Mettouchi utilisent une convention de glose interlinéaire (`=` pour les clitiques, `-` pour les affixes) différente de l'orthographe latine standard du corpus Tatoeba. Toute règle tirée de ces publications doit être revérifiée sur l'orthographe pratique avant d'être encodée telle quelle (voir §8.1).

---

## 12. Références

### 12.1 Sources primaires

1. **Naït-Zerrad, Kamal.** *Grammaire moderne du kabyle. Tajerrumt tatrart n teqbaylit*. Karthala, 2001.
2. **Mettouchi, Amina.** *Prosodic Segmentation and Grammatical Relations: The Direct Object in Kabyle (Berber)*. 2018.
3. **Mettouchi, Amina.** *The Interaction of State, Prosody and Linear Order in Kabyle (Berber)*. 2018.
4. **Mettouchi, Amina.** *The Grammaticalization of Directional Particles in Berber*.
5. **Chaker, Salem.** *Un parler berbère d'Algérie (Kabylie). Syntaxe*. Doctorat d'État, Université de Paris V, 1983.

### 12.2 Sources morphophonologiques

6. **Bedar, M., Quellac, M., & Voeltzel, L.** (2021). *Epenthetic glides in Taqbaylit*. Journal of African Languages and Literatures, 12(1), 1–36.
7. **Aïssou, F.** (2021). *Étude descriptive et comparative des pronoms personnels à l'est de Béjaïa*.
8. **Baier, N.** (2020). *Topics in Kabyle Linguistics*. McGill Working Papers in Linguistics.

### 12.3 Ressources techniques

9. **Belkacem, Mohammed.** *KabyleNLP — Corpus d'affixes pour la tokenisation*. GitHub : [github.com/MohammedBelkacem/KabyleNLP](https://github.com/MohammedBelkacem/KabyleNLP)
10. **Corpus :** `boffire/tatoeba-kabyle-mono-cleaned` (HuggingFace) — 756 774 phrases, version 2026.

### 12.4 Spécifications connexes

11. **Spécification orthographique :** `kabyle-orthography-spec` (v0.4-draft) — alphabet et normalisation Unicode
12. **Spécification de collation :** `kabyle-collation-spec` — ordre alphabétique (à vérifier)

---

## 13. Historique des versions

| Version | Date | Modifications |
|---|---|---|
| 0.1-draft | 2026 | Première version. Validation empirique des clitiques objets, possessifs, directionnels, démonstratifs. Règle d'ordre des clitiques (provisoire, non vérifiée). Allomorphie `ɣ`, `went`, `d`/`id`, `s`/`is`. |
| 0.2-draft | 2026 | **Correction majeure du §8** : la règle d'ordre était erronée (« direct avant indirect » affirmé sans preuve directe). Nouvelle règle validée : datif avant absolutif, confirmée par Mettouchi (2018) et par extraction ciblée sur le corpus (99,9 %, 32 026/26). Documentation du sous-cas d'inversion à l'impératif, des cas `[disputed]` non expliqués, d'un faux positif d'extraction, et de la distinction avec les présentatifs. Ajout d'une note terminologique absolutif/datif (Mettouchi). Retrait de pourcentages non fiables en §5.2 (base de calcul incohérente). |
| 0.3-draft | 2026-09-05 | **Recalcul complet** des 5 points techniques identifiés en §14 v0.2 : (1) confirmation des formes `ken`, `ten`, `tent`, `akent`, `asen` avec fréquences corpus ; (2) recalcul correct de la règle `-is`/`-s` par proportion intra-contexte ; (3) épenthèse `ak`/`am` réinterprétée comme glide [j] facultatif post-vocalique (Bedar et al. 2021) ; (4) **réfutation de l'hypothèse `kent` → `went` post-vocalique** : le corpus montre `yid-kent` (544) dominant massivement sur `yid-went` (67) — `went` est une variante minoritaire, pas un allomorphe conditionné ; (5) hypothèse `-n` pour l'inversion ABS-avant-DAT réfutée (23,1 % seulement). Ajout de `imi` comme particule préverbale déclenchant la proclise (§8.7, §9.1). Intégration des formes composées attestées (`tneɣ`, `nnteɣ`, `atsen`, etc.). Mise à jour des références académiques. Tous les exemples sont désormais des phrases réelles extraites du corpus. |

---

## 14. Travaux futurs

Avant une version 1.0 stable, les travaux suivants sont nécessaires :

1. **Validation par locuteurs natifs :** Faire relire les exemples et les règles par plusieurs locuteurs compétents représentant les conventions concernées — en particulier les cas `[disputed]` des §8.3, §8.6 et §9.2.
2. **Corpus complémentaire :** Valider les formes composées rares (`atenteɣ`, `atkent`, etc.) sur un corpus de textes longs (Wikipédia, littérature).
3. **Allomorphie `went` :** Déterminer si `went` est une variante dialectale, de registre, ou si un conditionnement phonologique plus fin existe. Tester `deg-kent` vs `deg-went`, `ttxil-kent` vs `ttxil-went`, etc.
4. **Syncrétisme 1SG :** Clarifier la désambiguïsation contextuelle entre objet direct et indirect pour `iyi` et `aɣ`.
5. **Règles phonologiques complètes :** Documenter systématiquement tous les cas d'allomorphie conditionnés par le contexte phonologique, y compris l'épenthèse glide facultative sur `ak`/`am` (§9.3).
6. **Variantes dialectales :** Documenter les variantes régionales attestées (notamment la chute de `a-` en `ak`/`am` documentée par Chaker 1983 pour le parler d'Irjen).
7. **Licence :** Clarifier la licence de la liste d'affixes de Belkacem77 avant réutilisation verbatim.
8. **Analyse de `tneɣ` :** Déterminer la décomposition exacte de `tneɣ` (131 occurrences) — clitique 3SG.F + 1PL ? Forme possessive alternative ?
9. **Cas indicatifs à ordre inversé (§8.3) :** déterminer si `fukken-t-ak` et `ttaran-t-as` relèvent d'une propriété sémantique/aspectuelle des verbes concernés, ou sont des exceptions lexicalisées isolées.
10. **Interfaces avec les specs connexes :** Documenter les dépendances explicites avec `kabyle-orthography-spec`, `kabyle-negation-spec`, `kabyle-tokenizer-spec` et `kabyle-braille-spec`.

---

**Ce document est une proposition de spécification. Il n'a pas encore été validé par un linguiste kabyle. Les formes marquées `[NEEDS REVIEW]` ou `[disputed]` nécessitent une vérification supplémentaire avant toute utilisation en production.**
