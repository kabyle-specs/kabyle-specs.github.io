# Spécification des pronoms et clitiques kabyles (Taqbaylit)

- **Identifiant :** `kabyle-pronouns-clitics-spec`
- **Version :** 0.2-draft
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

> **Note terminologique :** Mettouchi (2018) établit que ce que ce document appelle « objet direct » correspond en réalité au rôle sémantique **absolutif** (patient référentiel), et « objet indirect » au rôle **datif** (argument indirectement affecté) — distinction sémantique, pas une fonction grammaticale de complément d'objet au sens strict. Les tableaux ci-dessous conservent temporairement la terminologie « objet direct/indirect » pour rester lisibles, mais adoptent le vocabulaire absolutif/datif dans les sections d'analyse (§8) où la distinction devient significative.

### Spécifications connexes
- `kabyle-orthography-spec` (v0.4-draft) — alphabet et normalisation Unicode
- `kabyle-collation-spec` — ordre alphabétique (à vérifier)

---

## 1. Résumé

Ce document spécifie les formes, l'ordre et les règles d'allomorphie des **clitiques pronominaux** en kabyle (taqbaylit), dans le but de fournir une base de référence pour les modèles de langue et les outils de traitement automatique des langues (NLP). L'objectif est de **prévenir les hallucinations** de formes grammaticales incorrectes par les modèles d'IA.

Toutes les formes et règles documentées ici ont été **validées empiriquement** par croisement entre :
1. Une liste d'affixes communautaire (82 formes uniques)
2. Un corpus de 756 774 phrases (Tatoeba kabyle nettoyé)
3. Une source académique publiée (Mettouchi 2018) pour la règle d'ordre des clitiques

**Résultats de validation :**
- **70/82 affixes confirmés** par le corpus (85,4 %)
- **3 règles d'allomorphie** découvertes et validées
- **Règle d'ordre datif/absolutif** confirmée à 99,9 % (32 026 / 32 054 occurrences), avec un sous-cas d'inversion documenté (§8)

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

**Limite :** Cette méthode capture aussi les mots composés et les formes verbales conjuguées suivies de clitiques. Un tri par fréquence, position et inspection manuelle a été appliqué pour filtrer le bruit. Elle peut aussi produire de faux positifs lorsqu'un segment de mot ressemble à un clitique sans en être un (voir §8.6).

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

### 3.4 Validation de l'ordre relatif des clitiques

Pour trancher l'ordre entre clitiques datif et absolutif (§8), une extraction ciblée a recherché tous les mots à trait d'union contenant simultanément une forme non-ambiguë du paradigme datif (`as`, `asen`, `asent`, `awen`, `akent`) et une forme du paradigme absolutif (`t`, `tt`, `iyi`, `yi`, `ten`, `tent`, `kem`, `ken`, `kent`, `it`, `itt`, `iten`, `itent`), puis a comparé leur position relative dans le mot. Les formes syncrétiques (`aɣ`, `ɣ`) ont été volontairement exclues de ce test, car leur ambiguïté direct/indirect les rend inutilisables pour mesurer un ordre.

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
| 1 | — | sg | `-iyi` / `-yi` | `yi-` | `[confirmé]` | `Iwala-yi Tom` (Tom m'a vu) |
| 1 | — | pl | `-aɣ` | `aɣ-` / `ɣ-` | `[confirmé]` | `Ɣur-aɣ lḥeqq` (Nous avons raison) |
| 2 | M | sg | `-k` | `k-` | `[confirmé]` | `Iwala-k Tom` (Tom t'a vu) |
| 2 | F | sg | `-kem` | `kem-` | `[confirmé]` | `Iwala-kem Tom` (Tom t'a vue) |
| 2 | M | pl | `-ken` | `ken-` | `[confirmé]` (2 536 occ.) | `ken-terri` (« qu'il te réponde », `Ur ken-terri ara tmara`) |
| 2 | F | pl | `-kent` | `kent-` | `[confirmé]` | `Aql-ikent am tiyaḍ` (Vous êtes comme les autres) |
| 3 | M | sg | `-t` | `t-` | `[confirmé]` | `Iwala-t Tom` (Tom l'a vu) |
| 3 | F | sg | `-tt` | `tt-` | `[confirmé]` | `Iwala-tt Tom` (Tom l'a vue) |
| 3 | M | pl | `-ten` | `ten-` | `[confirmé]` (16 313 occ.) | `ten-yuɣen` (« qui les a affligés », `D acu akka i ten-yuɣen?`) |
| 3 | F | pl | `-tent` | `tent-` | `[confirmé]` (14 773 occ.) | `yeǧǧa-tent` (« l'a laissées », `Jipat tiwezzlanin yeǧǧa-tent wakud`) |

### 4.2 Paradigme des objets indirects (datif)

| Personne | Genre | Nombre | Forme enclitique | Forme proclitique | Statut | Exemple corpus |
|---|---|---|---|---|---|---|
| 1 | — | sg | `-iyi` / `-yi` | `yi-` | `[confirmé]` (syncrétisme) | `Yenna-yi` (Il m'a dit) |
| 1 | — | pl | `-aɣ` | `aɣ-` / `ɣ-` | `[confirmé]` (syncrétisme) | `Yenna-aɣ` (Il nous a dit) |
| 2 | M | sg | `-ak` | `ak-` | `[confirmé]` | `Yenna-yak` (Il t'a dit) — *cf. §9.3, épenthèse `y` à vérifier* |
| 2 | F | sg | `-am` | `am-` | `[confirmé]` | `Yenna-yam` (Il t'a dit, fém.) — *cf. §9.3* |
| 2 | M | pl | `-awen` | `awen-` | `[confirmé]` | `Yuzen-awen-d` (Il vous a envoyé) |
| 2 | F | pl | `-akent` | `akent-` | `[confirmé]` (4 614 occ.) | `fell-akent` (« sur/contre vous, fém. », `Yerfa fell-akent`) |
| 3 | M/F | sg | `-as` | `as-` | `[confirmé]` | `Yenna-yas` (Il lui a dit) |
| 3 | M | pl | `-asen` | `asen-` | `[confirmé]` (11 106 occ.) | `fell-asen` (« sur/contre eux », `Nɣan-ten, sakin ttrun fell-asen`) |
| 3 | F | pl | `-asent` | `asent-` | `[confirmé]` | `tezwi-asent-id` (Elle leur a apporté) |

> **Note sur le syncrétisme :** Les formes de 1ère personne (`iyi`, `aɣ`) présentent un syncrétisme : la même forme est utilisée pour l'objet direct et l'objet indirect. Le contexte syntaxique (type de verbe : transitif direct vs. verbe de parole) est nécessaire pour désambiguïser.

> **Point résolu (v0.3) :** les cinq formes précédemment marquées `[NEEDS REVIEW]` (`ken`, `ten`, `tent`, `akent`, `asen`) ont toutes été retrouvées en grand nombre dans le corpus (2 500 à 16 000+ occurrences chacune) et requalifiées en `[confirmé]` avec un exemple attesté.

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

| Contexte | `-is` | `-s` | Total | % `-is` | % `-s` |
|---|---|---|---|---|---|
| Après voyelle | 216 | 7 686 | 7 902 | 2,7 % | **97,3 %** |
| Après consonne | 20 883 | 8 031 | 28 914 | **72,2 %** | 27,8 % |

**Règle (v0.3 — recalculée correctement, proportion dans chaque contexte) :**
- Après **voyelle** → `-s` quasi catégorique (97,3 %). Règle solide, comparable à `-d`/`-id`.
- Après **consonne** → `-is` dominant mais **pas catégorique** (72,2 % contre 27,8 % pour `-s`). Contrairement à `-d`/`-id` (§6.2), la règle n'est ici qu'une **forte tendance**, pas une distribution complémentaire quasi parfaite.

**Statut :** `[confirmé]` pour le sous-cas « après voyelle → `-s` » ; `[confirmé (tendance, pas catégorique)]` pour « après consonne → `-is` préféré ». La marge de 28 % de `-s` après consonne suggère un facteur supplémentaire non encore identifié (peut-être la consonne finale précise, pas seulement la catégorie voyelle/consonne) — à affiner si une version plus fine de la règle est souhaitée, mais non bloquant pour la version actuelle.

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
| Après voyelle | `-d` | 19 195 (99,6 % des cas après voyelle) | `y-d` (semi-voyelle + d) |
| Après consonne | `-id` (préféré) | 15 466 (99,5 % des cas après consonne) | `t-id` (après consonne `t`) |

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
| `-nni` | Déictique / démonstratif (référent déjà mentionné) | `[confirmé]` | 72 089 | `argaz-nni` (cet homme-là) |
| `-agi` | Démonstratif de proximité | `[confirmé]` | Fréquent | `argaz-agi` (cet homme-ci) |
| `-nniḍen` | « autre » | `[confirmé]` | Attesté | `argaz-nniḍen` (un autre homme) |

> **Découverte :** `-nni` et `-agi` n'étaient pas dans l'hypothèse initiale mais ont été identifiés par l'extraction bottom-up du corpus, puis confirmés par la liste communautaire. Ce sont de vrais morphèmes grammaticaux, pas des artefacts de tokenisation.

---

## 8. Règle d'ordre des clitiques

### 8.1 Règle générale — Datif avant Absolutif

**Correction (v0.2) :** la version précédente de cette section affirmait l'ordre « objet direct + objet indirect », par erreur — sans preuve empirique directe sur des mots empilant les deux, et en contradiction avec la source académique de référence. Cette version corrige la règle.

Amina Mettouchi (2018) établit, à partir d'un corpus oral annoté, que le clitique **datif** précède toujours le clitique **absolutif** dans une chaîne verbale kabyle :

```
[Particule préverbale / Verbe] + DATIF + ABSOLUTIF + [Directionnel]
```

**Preuve empirique (orthographe pratique du corpus Tatoeba) :** une extraction ciblée des mots à trait d'union contenant simultanément une forme datif non-ambiguë (`as`, `asen`, `asent`, `awen`, `akent`) et une forme absolutif (`t`, `tt`, `iyi`, `ten`, `tent`, `kem`...) confirme cet ordre à **99,9 %** :

| Ordre | Occurrences |
|---|---|
| DATIF avant ABSOLUTIF | 32 026 |
| ABSOLUTIF avant DATIF | 28 |

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
| `fket-t-as(en/ent)` | `fk` (donner) | Impératif 2pl/2pl.f | « donnez-le-lui/leur » |
| `fkemt-t-as(en/ent)` | `fk` (donner) | Impératif 2pl.f | « donnez-le-lui/leur » |

**Statut :** `[confirmé]` — plusieurs verbes différents (`awi`, `semceḥem`, `ini`, `fk`), plusieurs personnes, résultat cohérent.

**Remarque typologique :** ce phénomène a des parallèles dans d'autres langues (le français inverse aussi l'ordre à l'impératif : « je te le dis » vs. « dis-le-moi »). Il ne s'agit donc pas d'une anomalie isolée du kabyle.

### 8.3 Cas non expliqués par le mode verbal — `[disputed]`

**Test élargi (v0.3) :** le paradigme datif testé a été étendu à `ak`/`am` (exclus initialement), portant l'échantillon total ABS-avant-DAT à 28 occurrences. Répartition :

| Catégorie | Occurrences | Part |
|---|---|---|
| Impératif (`awi`, `semceḥem`/`semcaḥem`, `ini`, `fket`, `fkemt`) | 16 | 57 % |
| Présentatif `ha`/`Ha` (§8.4, hors règle verbale) | 3 | 11 % |
| Faux positif d'extraction (`t-id-asen`, §8.5) | 2 | 7 % |
| Indicatif, non expliqué (`fukken`, `ttaran`, `Ɣlin`) | 6 | 21 % |
| Ambigu, sans verbe identifiable (`tt-as-d`) | 1 | 4 % |

**Hypothèse `-n` réfutée :** l'hypothèse envisagée en v0.2 (terminaison verbale en `-n`) prédisait que les cas non-impératifs partageraient cette terminaison. Sur l'échantillon élargi, seuls 6 des 28 cas (21,4 %) finissent en `-n` — la plupart des cas ABS-avant-DAT (l'impératif) n'ont pas cette terminaison. L'hypothèse `-n` est donc **abandonnée** : elle expliquait une coïncidence sur 2 exemples, pas un vrai facteur.

**Ce qui reste réellement non expliqué :** `fukken-t-ak/am/as` (3PL accompli), `ttaran-t-as` (3PL imperfectif habituel) et `Ɣlin-t-ak` (« tes lunettes te sont tombées » — verbe intransitif, cf. §8.6) sont à l'indicatif et inversent quand même l'ordre. Le point commun entre ces trois verbes (`fukk`, `ttar`, `ɣli`) n'est pas évident à partir des données seules — ni la personne, ni le mode, ni la terminaison ne les distinguent proprement des cas DAT-avant-ABS majoritaires. **Ce point ne peut pas être tranché par le corpus seul** ; il nécessite une consultation grammaticale ou un locuteur natif.

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
| `imi` | « parce que » | *(fréquence globale à mesurer — identifiée via §9.2)* |

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
>
> **Statut de cette désambiguïsation :** `[confirmé par analyse interne du corpus]`, pas encore adossé à une règle de réduction phonologique publiée explicitement. La preuve retenue : dans les exemples ci-dessus, le verbe porte déjà sa propre marque sujet (souvent 2pl ou 3pl), ce qui exclut que le `ɣ` proclitique soit le suffixe sujet 1sg — un verbe ne peut pas porter deux sujets différents. Cette déduction est solide mais reste une inférence à partir des données, distincte d'une citation grammaticale directe.

### 9.2 Allomorphe du clitique 2PL.F : `kent` → `went`

Le clitique 2ème personne du pluriel féminin `-kent` prend la forme `-went` lorsqu'il est suffixé à certaines bases.

**Extraction complète (v0.3) : 183 occurrences en enclise, 24 en proclise.**

**Enclise — bases déclenchantes observées (extrait) :**

| Base | Fréquence | Exemple |
|---|---|---|
| `yid` (avec) | 67 | `Bɣiɣ ad mmeslayeɣ yid-went` (Je veux parler avec vous, fém.) |
| `deg` (dans) | 14 | `Deg-went i tres tyita` (C'est parmi vous que frappe l'épidémie) |
| `ttxil` (s'il vous plaît) | 14 | `Ttxil-went` (S'il vous plaît, fém.) |
| `ḥemmlen` (ils/elles aiment) | 13 | — |
| `ɣur` (chez) | 8 | `Ɣur-went axxam deg Ustṛalya?` (Vous avez une maison en Australie ?) |
| `ɣer` (vers) | 8 | — |
| `seg` (de/parmi) | 8 | `Ilaq ad truḥ ɣer din yiwet seg-went` (Il faut que l'une de vous y aille) |
| *(~30 autres bases, chacune 1 à 3 occurrences)* | — | verbes très divers (`rran`, `ufiɣ`, `nudaɣ`, noms comme `baba`, `tawlaft`...) |

**Reformulation de l'hypothèse (v0.3) :** la diversité des bases (plus de 35 bases différentes, la plupart avec une seule occurrence) rend peu probable une règle de sandhi phonologique conditionnée par une liste fermée de déclencheurs, comme le suggérait la v0.2. Une hypothèse plus économique : `-went` et `-kent` pourraient être des **variantes libres** (dialectales ou de registre) du même clitique, plutôt que des allomorphes strictement conditionnés par le contexte phonologique. Cette hypothèse reste à confirmer par un locuteur natif — le corpus seul ne permet pas de trancher entre « conditionnement phonologique complexe non identifié » et « variation libre ».

**Proclise (nouveau, v0.3) — 24 occurrences, comportement cohérent avec les autres clitiques objets :**

| Particule précédente | Exemple |
|---|---|
| `ad` (futur) | `Ur zmireɣ ara ad went-iniɣ melmi ara nili nhegga` |
| `i` (relatif) | `Amek i went-teḍṛa?` |
| `ur` (négation) | `Ur went-n-ssawleɣ ara` |
| `ara` (relatif futur) | `Teɛweq d acu ara went-d-terr` |
| `imi` (« parce que ») | `Surfemt-iyi imi went-ssawleɣ zik` |

Ces particules (`ad`, `i`, `ur`, `ara`) correspondent exactement à celles déjà identifiées comme déclenchant la proclise pour les autres clitiques (§8.7) — `went` suit donc la même règle de proclise/enclise que le reste du paradigme objet. `imi` est une nouvelle particule à ajouter à la liste du §8.7, qui ne la mentionnait pas encore.

**Statut :** `[confirmé]` pour le comportement proclise/enclise (identique au reste du paradigme) ; `[disputed]` pour la nature de l'alternance `kent`/`went` elle-même (conditionnement phonologique non identifié vs. variation libre) — nécessite un locuteur natif, non résoluble par le corpus seul.

### 9.3 Épenthèse partielle sur `ak`/`am` (v0.3 — vérifié)

Les exemples `Yenna-yak` et `Yenna-yam` (§4.2) utilisent `yak`/`yam` après un verbe finissant par une voyelle. Une vérification systématique sur tout le corpus donne :

| Paire | Après voyelle | Après consonne |
|---|---|---|
| `ak` / `yak` | `ak` = 481 (66,1 %) / `yak` = 247 (33,9 %) — n=728 | `ak` = 6 265 (100,0 %) / `yak` = 1 — n=6 266 |
| `am` / `yam` | `am` = 399 (67,5 %) / `yam` = 192 (32,5 %) — n=591 | `am` = 5 066 (100,0 %) / `yam` = 0 — n=5 066 |

**Conclusion, différente de l'hypothèse initiale :** l'épenthèse `y-` n'est **jamais** obligatoire ici (contrairement à `-id` après consonne, quasi-catégorique). Elle est :
- **impossible après consonne** (0 à 1 occurrence sur plus de 11 000) ;
- **optionnelle après voyelle** (environ 1 cas sur 3 seulement prend le `y-`).

Ce n'est donc pas la même règle que `-d`/`-id` (§6.2) : là où `-id` est presque obligatoire après consonne, `yak`/`yam` restent minoritaires même dans le contexte qui les permettrait. Le facteur qui détermine ces cas minoritaires (identité du mot précédent ? registre ? variante individuelle ?) reste à identifier.

**Statut :** `[confirmé]` pour les deux bornes catégoriques (jamais après consonne) ; `[disputed]` pour le facteur qui explique la variation observée après voyelle.

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
2. **Bruit résiduel :** L'extraction par trait d'union capture aussi des mots composés et des formes verbales. Un filtrage par fréquence et position a été appliqué, mais une inspection manuelle reste nécessaire pour les cas ambigus (voir §8.5 pour un exemple concret de faux positif).
3. **Formes composées :** Les clusters clitiques complexes (`atenteɣ`, `atkent`, etc.) n'ont pas pu être validés par le corpus. Ils sont marqués `[NEEDS REVIEW]`.

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

### 12.2 Ressources techniques

5. **Belkacem, Mohammed.** *KabyleNLP — Corpus d'affixes pour la tokenisation*. GitHub : [github.com/MohammedBelkacem/KabyleNLP](https://github.com/MohammedBelkacem/KabyleNLP)
6. **Corpus :** `boffire/tatoeba-kabyle-mono-cleaned` (HuggingFace) — 756 774 phrases, version 2026.

### 12.3 Spécifications connexes

7. **Spécification orthographique :** `kabyle-orthography-spec` (v0.4-draft) — alphabet et normalisation Unicode
8. **Spécification de collation :** `kabyle-collation-spec` — ordre alphabétique (à vérifier)

---

## 13. Historique des versions

| Version | Date | Modifications |
|---|---|---|
| 0.1-draft | 2026 | Première version. Validation empirique des clitiques objets, possessifs, directionnels, démonstratifs. Règle d'ordre des clitiques (provisoire, non vérifiée). Allomorphie `ɣ`, `went`, `d`/`id`, `s`/`is`. |
| 0.2-draft | 2026 | **Correction majeure du §8** : la règle d'ordre était erronée (« direct avant indirect » affirmé sans preuve directe). Nouvelle règle validée : datif avant absolutif, confirmée par Mettouchi (2018) et par extraction ciblée sur le corpus (99,9 %, 32026/28). Documentation du sous-cas d'inversion à l'impératif, des cas `[disputed]` non expliqués, d'un faux positif d'extraction, et de la distinction avec les présentatifs. Ajout d'une note terminologique absolutif/datif (Mettouchi). Retrait de pourcentages non fiables en §5.2 (base de calcul incohérente). Signalement de plusieurs formes `[confirmé]` sans exemple corpus (§4), à requalifier avant publication. Harmonisation de la version du corpus (2026) entre les sections. |

---

## 14. Travaux futurs

Avant une version 1.0 stable, les travaux suivants sont nécessaires :

1. **Validation par locuteurs natifs :** Faire relire les exemples et les règles par plusieurs locuteurs compétents représentant les conventions concernées — en particulier les cas `[disputed]` des §8.3 et §8.6.
2. **Corpus complémentaire :** Valider les formes composées rares (`atenteɣ`, etc.) sur un corpus de textes longs (Wikipédia, littérature).
3. **Allomorphie de `went` :** Documenter la liste complète des bases déclenchant la mutation `kent` → `went`, y compris le cas de proclise repéré en §9.2.
4. **Syncrétisme 1SG :** Clarifier la désambiguïsation contextuelle entre objet direct et indirect pour `iyi` et `aɣ`.
5. **Règles phonologiques complètes :** Documenter systématiquement tous les cas d'allomorphie conditionnés par le contexte phonologique, y compris l'épenthèse possible sur `ak`/`am` (§9.3).
6. **Variantes dialectales :** Documenter les variantes régionales attestées.
7. **Licence :** Clarifier la licence de la liste d'affixes de Belkacem77 avant réutilisation verbatim.
8. **Formes `[NEEDS REVIEW]` du §4 :** Rechercher des exemples corpus pour `-ken`, `-ten`, `-tent`, `-akent`, `-asen` avant de confirmer ou retirer leur statut `[confirmé]`.
9. **Recalcul de la règle `-is`/`-s` (§5.2) :** refaire le calcul de fréquence par contexte (proportion de chaque forme *dans* chaque contexte), sur le modèle de la règle `-d`/`-id`.
10. **Cas indicatifs à ordre inversé (§8.3) :** déterminer si `fukken-t-ak` et `ttaran-t-as` relèvent d'une règle liée à la terminaison `-n`, ou sont des cas isolés.

---

**Ce document est une proposition de spécification. Il n'a pas encore été validé par un locuteur natif ou un linguiste kabyle. Les formes marquées `[NEEDS REVIEW]` ou `[disputed]` nécessitent une vérification supplémentaire avant toute utilisation en production.**
