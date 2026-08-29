# Kabyle negation specification

**Version** : 0.2.0-draft
**Date** : 2026-08-29
**Statut** : DRAFT — amorce corrigée. Le noyau structural de la négation discontinue est établi, mais la particule postverbale `ara` est désormais traitée comme optionnelle et homographe, sur la base des données de Mettouchi (2001, 2021). Les règles fines restent des questions ouvertes marquées `needs-native-review`.
**Dépendances** :
- `kabyle-lemmatization-spec.md` (v1.3.2-rev ou ultérieure)
- `kabyle-conjugations-specs.md`
- `kabyle-ud-specification.md` (v0.7 ou ultérieure)
- `kabyle-morphological-tokenizer-spec.md`

---

## Changelog

**v0.2.0** (correction majeure — 2026-08-29)
- **Correction critique** : `ara` n'est plus présenté comme obligatoire dans la négation discontinue. Elle est désormais traitée comme un renforcement postverbal optionnel (présent dans ~52 % des négations en corpus, absent dans ~48 % selon Mettouchi 2021).
- **Homographe** : introduction du statut d'homographe pour `ara` (négation vs. particule modale aoriste / subordination relative).
- **Distribution contextuelle** : ajout d'une table des contextes d'absence et d'obligation de `ara` (§3.1bis).
- **Négations non verbales** : ajout de `mačči` et `ulac` dans l'inventaire des items négatifs (§5.1), avec renvoi documenté.
- **Mise à jour UD** : ajout des deux fonctions de `ara` dans la représentation proposée (§6).
- **Nouvelle référence** : Mettouchi (2001, 2021), Encyclopédie berbère (2015).
- **Nouveau code d'erreur** : E013 `HOMOGRAPHE_ARA_MISCLASSIFIED`.

**v0.1.0** (amorce)
- Établissement du noyau structural : négation discontinue `ur … ara`.
- Règles de lemmatisation des particules négatives.
- Interaction avec l'apophonie verbale (§4).
- Représentation UD proposée (§6).
- Portes de validation et codes d'erreur (§7).
- Cohérence inter-spécifications vérifiée : E012 réservé à la négation, aligné avec `kabyle-lemmatization-spec.md` §7bis.

---

## 1. Objectif

Cette spécification formalise la négation kabyle pour les besoins suivants :

1. la lemmatisation des particules négatives (renvoi : `kabyle-lemmatization-spec.md` §4.8) ;
2. la modélisation de l'apophonie négative (renvoi : `kabyle-lemmatization-spec.md` §5.2, étape 3) ;
3. l'annotation Universal Dependencies (trait de polarité, relation syntaxique de négation) ;
4. la génération morphologique de formes négatives ;
5. l'analyse et l'annotation des formes négatives en corpus.

Ce document ne duplique pas les règles déjà posées dans `kabyle-lemmatization-spec.md` ; il les référence et les développe. En cas de désaccord apparent entre les deux documents, `kabyle-lemmatization-spec.md` fait foi pour tout ce qui concerne la forme de citation et le pipeline général de lemmatisation, ce document faisant foi pour le détail de la négation.

---

## 2. Fait établi : négation verbale discontinue

La négation verbale kabyle est **discontinue** : elle est formée d'un élément préverbal obligatoire et d'un élément postverbal qui encadrent le verbe. L'élément postverbal est **optionnel** dans de nombreux contextes ; seul l'élément préverbal est le négateur proprement dit (Mettouchi 2021 : §4).

**Structure générale** :

```text
ur + verbe au prétérit négatif (+ ara optionnel)
```

| Particule | Position | Obligation | Lemme | Statut |
|-----------|----------|------------|-------|--------|
| `ur` | préverbal | obligatoire | `ur` | verified |
| `ara` | postverbal | optionnel (conditionnée, voir §3.1bis) | `ara` | verified (homographe) |

> **Note critique** : Contrairement à une description simplifiée, `ara` n'est **pas** un constituant obligatoire de la négation. Son absence ne supprime pas la polarité négative de la clause. La négation est portée par `ur` + le stem verbal négatif. `ara` fonctionne comme un renforcement postverbal (postneg) en cours de grammaticalisation (Mettouchi 2001, 2021).

---

## 3. Règles de lemmatisation des particules négatives

### 3.1 Règle générale — `ur`

La particule préverbale `ur` est son propre lemme ; elle n'est jamais fusionnée avec le lemme verbal.

```json
{
  "token": "ur",
  "lemma": "ur",
  "root": null,
  "pos": "PART",
  "morph": { "polarity": "Neg", "part_type": "PreverbalNegative" },
  "dialect": null,
  "status": "verified",
  "confidence": 0.95,
  "source": "rule",
  "error": null
}
```

### 3.1bis Règle générale — `ara` (homographe)

Le token `ara` est un **homographe** : deux lemmes fonctionnels distincts partagent la même forme orthographique. Le pipeline de lemmatisation doit désambiguïser par le contexte syntaxique (présence/absence de `ur` dans la clause, mode du verbe, type de subordination).

#### Fonction A — Renforcement négatif postverbal

```json
{
  "token": "ara",
  "lemma": "ara",
  "root": null,
  "pos": "PART",
  "morph": { "polarity": "Neg", "part_type": "PostverbalNegative" },
  "condition": "co-occurrence avec ur dans la même clause",
  "dialect": null,
  "status": "verified",
  "confidence": 0.90,
  "source": "rule",
  "error": null
}
```

#### Fonction B — Particule modale aoriste (non négative)

```json
{
  "token": "ara",
  "lemma": "ara",
  "root": null,
  "pos": "PART",
  "morph": { "mood": "Irr", "part_type": "AoristModal" },
  "condition": "absence de ur dans la clause ; typiquement en subordination relative restrictive ou en complétive deontique avec aoriste",
  "dialect": null,
  "status": "candidate",
  "confidence": 0.70,
  "source": "rule",
  "error": null
}
```

> **Source** : La fonction modale de `ara` est attestée dans la littérature berbériste comparative (Encyclopédie berbère 2015 : « particules modales de l'aoriste » ; Mettouchi 2001 : grammaticalisation de `ara` dans la subordination relative). Son statut reste `candidate` dans le kabyle central en l'absence d'attestation directe dans le corpus Tatoeba ou le CorpAfroAs.

### 3.2 Non-fusion avec le verbe

Le lemme verbal en contexte négatif reste le lemme positif correspondant (cf. §4 ci-dessous). La négation est représentée par :
- les particules elles-mêmes, chacune avec son propre lemme ;
- des traits morphologiques (`Polarity=Neg`) sur le verbe ;
- une relation syntaxique de négation dans la couche dépendances (§6.2).

**Principe** : `ur` + forme verbale négative (+ `ara`) → un seul verbe lemmatisé (lemme positif) + une ou deux particules lemmatisées séparément. Aucune règle ne doit produire un lemme verbal distinct pour la forme négative, sauf cas lexicalisé documenté (§5.4).

> L'absence de `ara` ne change pas le statut négatif de la clause. Le lemme verbal reste le lemme positif même en contexte `ur` seul.

---

## 4. Interaction avec l'apophonie verbale

La forme négative du prétérit peut présenter une apophonie spécifique par rapport à la forme positive.

**Exemple hérité de `kabyle-lemmatization-spec.md` §5.2, étape 3** (statut inchangé : `candidate`, non re-vérifié) :

| Forme négative | Lemme | Alternance | Statut |
|----------------|-------|------------|--------|
| `ur ufigeɣ ara` | `afeg` | a → u (prétérit) + a → i (négatif) | candidate |
| `ur tufigeḍ ara` | `afeg` | — | candidate |

**⚠️** Ces exemples doivent être validés avant tout usage normatif. Aucune règle générative de l'apophonie négative ne doit être publiée comme `verified` sans double vérification (source académique + attestation de corpus, ou confirmation par locuteur natif).

### 4.1 Apophonie négative par type morphologique — à documenter

Pour chaque type verbal défini dans `kabyle-conjugations-specs.md` (types G1–G4), il reste à documenter :
- la forme positive ;
- la forme négative correspondante ;
- l'alternance vocalique observée ;
- les exceptions au patron général ;
- les verbes supplétifs au négatif, le cas échéant.

```text
[À COMPLÉTER — paradigme des 6 000 verbes (kabyle-lemmatization-spec.md §3.1) + validation par locuteur natif]
```

Cette table, une fois établie, est la ressource attendue pour lever la limitation notée dans `kabyle-lemmatization-spec.md` §5.2 : « si les formes négatives ne sont pas présentes dans le paradigme, l'apophonie négative doit être inférée par règle ou annotée manuellement ».

### 4.2 Interaction avec les clitiques

À documenter :
- position des clitiques objet/indirect par rapport à `ur` et `ara` ;
- ordre relatif clitique + négation ;
- effets phonologiques éventuels de cette interaction ;
- conséquences, s'il y en a, sur la lemmatisation.

```text
[À COMPLÉTER — locuteur natif + grammaires de référence (Chaker 1983, Mammeri 1989)]
```

### 4.3 Spirantisation

Certaines descriptions berbères associent la négation à des phénomènes consonantiques (spirantisation). Pour le kabyle spécifiquement, il reste à vérifier :
- si la négation déclenche une spirantisation ;
- sur quels segments ;
- dans quels contextes morphosyntaxiques ;
- si ce phénomène, s'il existe, est pertinent pour la lemmatisation (c'est-à-dire s'il peut faire varier la forme du lemme lui-même, ou seulement une forme de surface).

```text
[À COMPLÉTER — validation par locuteur natif + source académique primaire]
```

---

## 5. Questions ouvertes

### 5.1 Inventaire des items négatifs au-delà de `ur … ara`

Ce document ne couvre, avec un statut `verified`, que le noyau discontinu `ur (+ ara)`. Une description complète de la négation kabyle devrait aussi couvrir :

#### 5.1.1 Négations non verbales (documentées, hors périmètre détaillé)

| Item | Fonction | Origine | Statut |
|------|----------|---------|--------|
| `mačči` | Négation attributive / équative / cleft négatif | Emprunt arabe (ma-…-š) | documented |
| `ulac` | Négation existentielle / locative / possessive | `ur` + `illi` + `ša` | documented |

> **Sources** : Mettouchi 2021 §4 ; Chaker 1983. Ces items sont répertoriés ici pour cohérence inter-spécifications mais leur traitement détaillé relève d'une spécification de prédication non verbale séparée.

#### 5.1.2 Autres items à documenter

- la négation d'attribution/présentation (équivalent de « ce n'est pas ») → voir `mačči` ci-dessus ;
- la négation d'existence (équivalent de « il n'y a pas ») → voir `ulac` ci-dessus ;
- les quantifieurs négatifs (« rien », « personne », etc.) ;
- la négation nominale ;
- la négation à l'impératif, si elle diffère structurellement de la négation à l'indicatif ;
- l'interaction de la négation avec les particules aspectuelles (`ad`, `la`, etc., déjà mentionnées dans `kabyle-lemmatization-spec.md` §4.8 comme hors périmètre détaillé).

```text
[À COMPLÉTER — locuteur natif]
```

### 5.2 Variation dialectale

À documenter :
- variantes attestées de `ur` selon les parlers ;
- variantes attestées de `ara` selon les parlers ;
- différences régionales dans l'usage ou l'omission des items négatifs ;
- emprunts éventuels à l'arabe dans le champ de la négation.

```text
[À COMPLÉTER — locuteur natif + corpus régionaux (UMMTO, UBouira, HCA), cf. kabyle-lemmatization-spec.md §3.2]
```

---

## 6. Représentation UD proposée

### 6.1 Trait de polarité

Trait de polarité négative recommandé, conforme aux guidelines Universal Dependencies :

```text
Polarity=Neg
```

À appliquer aux tokens négatifs pertinents (verbe et, le cas échéant, particules elles-mêmes), en cohérence avec `kabyle-ud-specification.md`.

### 6.2 Relation de négation

La relation UD standard pour la négation (`advmod:neg` dans les guidelines UD généralistes, ou son équivalent choisi par `kabyle-ud-specification.md`) doit être utilisée de façon cohérente entre les deux documents.

### 6.3 Lemme verbal en contexte négatif

Principe (cf. §3.2) : le verbe en contexte négatif conserve son lemme positif.

```text
forme négative → lemme verbal positif + traits négatifs (Polarity=Neg)
```

La négation ne doit jamais créer un lemme verbal distinct, sauf cas lexicalisé explicitement documenté et sourcé (aucun cas de ce type n'est actuellement recensé dans ce document).

### 6.4 Exemples UD commentés

#### Exemple 1 — Négation standard avec `ara`

```text
ur t-zwiʤ ara
NEG SBJ3.SG.F-marry:PFVNEG POSTNEG
"Elle n'était pas mariée"
```

- `ur` → `PART` + `Polarity=Neg`
- `t-zwiʤ` (prétérit négatif) → lemme `zwiʤ`, `Polarity=Neg` sur le verbe
- `ara` → `PART` + `Polarity=Neg` (fonction A, renforcement négatif)

#### Exemple 2 — Négation standard sans `ara`

```text
ur=dd zwiʤ-ɣ
NEG=PROX marry:PFVNEG-SBJ.1SG
"Je jure de ne pas me marier"
```

- `ur` → `PART` + `Polarity=Neg`
- `zwiʤ-ɣ` → lemme `zwiʤ`, `Polarity=Neg`
- Pas de `ara` : la polarité négative est pleinement assurée par `ur` + stem négatif.

#### Exemple 3 — `ara` en fonction modale aoriste (homographe, non négatif)

```text
[À compléter — attestation corpus requise]
```

> Hypothèse : dans une subordination relative restrictive positive avec aoriste, `ara` pourrait apparaître comme marqueur d'irréel sans `ur`. Dans ce cas : `ara` → `PART` + `Mood=Irr`, **sans** `Polarity=Neg`.

---

## 7. Portes de validation

Toute règle de négation introduite dans ce document doit suivre les mêmes portes que `kabyle-lemmatization-spec.md` :

1. Normalisation orthographique du token (`kabyle-orthography-specs.md`).
2. Règle sourcée (référence académique) ou attestée par table validée (paradigme des 6 000 verbes, corpus Tatoeba).
3. Validation Hunspell si la règle produit un lemme (`kabyle-lemmatization-spec.md` §3.4).
4. Validation par locuteur natif pour toute forme candidate avant passage au statut `verified`.

Codes d'erreur pertinents (repris de `kabyle-lemmatization-spec.md` §7bis, avec deux codes additionnels spécifiques à ce document) :

| Code | Nom | Déclencheur | Document |
|------|-----|-------------|----------|
| E007 | `NEGATIVE_APOPHONY_UNVERIFIED` | Apophonie négative non vérifiable (§4) | `kabyle-lemmatization-spec.md` §7bis |
| E009 | `AMBIGUOUS_LEMMA` | Plusieurs lemmes candidats pour un item négatif | `kabyle-lemmatization-spec.md` §7bis |
| E012 | `NEGATION_ITEM_NOT_IN_INVENTORY` | Item négatif rencontré en corpus mais absent de l'inventaire de §5.1 | **présent document** |
| E013 | `HOMOGRAPHE_ARA_MISCLASSIFIED` | Le token `ara` est tagué comme négatif dans un contexte sans `ur`, ou comme modal dans un contexte avec `ur` | **présent document** |

> **Note de cohérence inter-spécifications** : Le code E012 est réservé à la spécification de négation. Il n'est pas utilisé par `kabyle-lemmatization-spec.md`, qui laisse E012 vacant (cf. `kabyle-lemmatization-spec.md` §7bis : « E012 — Réservé par kabyle-negation-spec.md »). Le code E013 est nouveau en v0.2.0. Aucun conflit de numérotation n'existe entre les documents.

---

## 8. Évaluation

La négation doit être évaluée séparément, dans le cadre du jeu de test défini par `kabyle-lemmatization-spec.md` §10bis, avec des métriques dédiées :

- exactitude de détection de `ur` ;
- exactitude de détection de `ara` (taux de faux positifs sur la fonction négative) ;
- exactitude du lemme verbal produit en contexte négatif (doit rester le lemme positif) ;
- taux d'erreur de l'apophonie négative, par type morphologique (une fois §4.1 rédigé) ;
- taux de confusion entre négation verbale discontinue et les autres items négatifs de §5.1 ;
- taux de formes négatives non résolues (renvoyant à `E012`) ;
- **taux de désambiguïsation correcte de l'homographe `ara`** (renvoyant à `E013`).

---

## 9. Références

### Sources primaires directement citées dans ce document

- Mettouchi, A. (2001). « La grammaticalisation de ara en kabyle, négation et subordination relative », dans *Travaux du CerLiCO* n°14, Col G. et Roulland D. (eds), P.U. Rennes, pp. 215-235.
- Mettouchi, A. (2021). *Negation in Kabyle (Berber)*. JaLaLit (Journal of African Languages and Literatures) n° 2, pp. 30-79. https://doi.org/10.6092/jalalit.v2i2.8059
- Encyclopédie berbère (2015). « Participe (formes régionales) », document 43, en ligne sur https://journals.openedition.org/encyclopedieberbere/3160

### Sources reprises de `kabyle-lemmatization-spec.md` §9

- Chaker, S. (1983). *Un parler berbère d'Algérie (Kabylie) : syntaxe*, thèse d'État, Université de Provence.
- Chaker, S. (1995). *Linguistique berbère : études de syntaxe et de diachronie*.
- Bendjaballah, S. (1999/2000/2001). Articles sur l'apophonie en kabyle.
- Guerssel, M. & Lowenstamm, J. (1993/1996). Articles sur l'apophonie en berbère.
- Naït-Zerrad, K. (1995). *Grammaire du berbère contemporain (kabyle), I — Morphologie*.
- Mammeri, M. (1976/1989). *Tajerrumt n tmaziɣt*.
- Dallet, J.-M. (1982). *Dictionnaire kabyle-français* (parler des At Mangellat).

**⚠️** Les références de `kabyle-lemmatization-spec.md` sont reprises par renvoi, où leur pertinence générale est déjà établie. Leur pertinence *spécifique* à chaque règle de négation détaillée dans ce document (pages exactes, passages traitant explicitement de la négation) reste à vérifier et à citer précisément avant publication normative.

```text
[À COMPLÉTER — références exactes, pages, liens d'accès, et vérification de la disponibilité de chaque source]
```

---

## 10. Statut final

Ce document est une **amorce corrigée** qui fixe le cadre, l'interface avec `kabyle-lemmatization-spec.md`, et les questions ouvertes. La correction majeure de la v0.2.0 (optionnalité de `ara`, statut d'homographe) est fondée sur des données de corpus publiées (Mettouchi 2021) et doit être intégrée dans tout pipeline de lemmatisation ou d'annotation UD. Toute règle détaillée (apophonie par type morphologique, inventaire complet des items négatifs, variation dialectale) devra être validée par double vérification — source académique et corpus, ou confirmation par locuteur natif — avant intégration dans une version stable.

---

*Cette spécification est conçue pour être améliorée par la communauté, en cohérence avec `kabyle-lemmatization-spec.md`. Toute contribution validée par corpus ou par source académique peut être intégrée dans une version ultérieure.*
