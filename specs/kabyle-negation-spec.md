# Kabyle negation specification

**Version** : 0.1.0
**Date** : 2026-08-16
**Statut** : DRAFT — amorce. La quasi-totalité des règles fines sont des questions ouvertes marquées `needs-native-review`. Seul le fait grammatical large de la négation discontinue est présenté comme établi, sous réserve de vérification finale.
**Dépendances** :
- `kabyle-lemmatization-spec.md` (v1.3.2-rev ou ultérieure)
- `kabyle-conjugations-specs.md`
- `kabyle-ud-specification.md`
- `kabyle-morphological-tokenizer-spec.md`

---

## Changelog

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

La négation verbale kabyle est **discontinue** : elle est formée d'un élément préverbal et d'un élément postverbal qui encadrent le verbe. C'est un trait bien documenté de la description du berbère (cf. Chaker 1983, 1995, déjà cités dans `kabyle-lemmatization-spec.md` §9).

**Structure générale** :

```text
ur + verbe au prétérit négatif + ara
```

| Particule | Position | Lemme | status |
|-----------|----------|-------|--------|
| `ur` | préverbal | `ur` | verified |
| `ara` | postverbal | `ara` | verified |

Ce noyau structural — la discontinuité elle-même, et l'identité des deux particules `ur`/`ara` — est repris tel quel de `kabyle-lemmatization-spec.md` §4.8.A, où il est déjà marqué `verified` sur la base d'une description grammaticale large et convergente. Tout ce qui suit dans ce document (variantes, contraintes fines, exceptions) reste, en revanche, à documenter.

---

## 3. Règles de lemmatisation des particules négatives

### 3.1 Règle générale

Chaque particule négative est son propre lemme ; aucune n'est fusionnée avec le lemme verbal qu'elle encadre.

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

```json
{
  "token": "ara",
  "lemma": "ara",
  "root": null,
  "pos": "PART",
  "morph": { "polarity": "Neg", "part_type": "PostverbalNegative" },
  "dialect": null,
  "status": "verified",
  "confidence": 0.95,
  "source": "rule",
  "error": null
}
```

### 3.2 Non-fusion avec le verbe

Le lemme verbal en contexte négatif reste le lemme positif correspondant (cf. §4 ci-dessous). La négation est représentée par :
- les particules elles-mêmes, chacune avec son propre lemme ;
- des traits morphologiques (`Polarity=Neg`) sur le verbe ;
- une relation syntaxique de négation dans la couche dépendances (§6.2).

**Principe** : `ur` + forme verbale négative + `ara` → un seul verbe lemmatisé (lemme positif) + deux particules lemmatisées séparément. Aucune règle ne doit produire un lemme verbal distinct pour la forme négative, sauf cas lexicalisé documenté (§5.4).

---

## 4. Interaction avec l'apophonie verbale

La forme négative du prétérit peut présenter une apophonie spécifique par rapport à la forme positive.

**Exemple hérité de `kabyle-lemmatization-spec.md` §5.2, étape 3** (statut inchangé : `candidate`, non re-vérifié) :

| Forme négative | Lemme | Alternance | status |
|-----------------|-------|------------|--------|
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

Ce document ne couvre, avec un statut `verified`, que le noyau discontinu `ur … ara`. Une description complète de la négation kabyle devrait aussi couvrir :
- la négation d'attribution/présentation (équivalent de « ce n'est pas ») ;
- la négation d'existence (équivalent de « il n'y a pas ») ;
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

```text
[À COMPLÉTER — choisir la relation exacte en cohérence avec kabyle-ud-specification.md et documenter chaque cas (particule préverbale, particule postverbale, éventuels items de §5.1)]
```

### 6.3 Lemme verbal en contexte négatif

Principe (cf. §3.2) : le verbe en contexte négatif conserve son lemme positif.

```text
forme négative → lemme verbal positif + traits négatifs (Polarity=Neg)
```

La négation ne doit jamais créer un lemme verbal distinct, sauf cas lexicalisé explicitement documenté et sourcé (aucun cas de ce type n'est actuellement recensé dans ce document).

---

## 7. Portes de validation

Toute règle de négation introduite dans ce document doit suivre les mêmes portes que `kabyle-lemmatization-spec.md` :

1. Normalisation orthographique du token (`kabyle-orthography-specs.md`).
2. Règle sourcée (référence académique) ou attestée par table validée (paradigme des 6 000 verbes, corpus Tatoeba).
3. Validation Hunspell si la règle produit un lemme (`kabyle-lemmatization-spec.md` §3.4).
4. Validation par locuteur natif pour toute forme candidate avant passage au statut `verified`.

Codes d'erreur pertinents (repris de `kabyle-lemmatization-spec.md` §7bis, avec un code additionnel spécifique à ce document) :

| Code | Nom | Déclencheur | Document |
|------|-----|-------------|----------|
| E007 | `NEGATIVE_APOPHONY_UNVERIFIED` | Apophonie négative non vérifiable (§4) | `kabyle-lemmatization-spec.md` §7bis |
| E009 | `AMBIGUOUS_LEMMA` | Plusieurs lemmes candidats pour un item négatif | `kabyle-lemmatization-spec.md` §7bis |
| E012 | `NEGATION_ITEM_NOT_IN_INVENTORY` | Item négatif rencontré en corpus mais absent de l'inventaire de §5.1 | **présent document** |

> **Note de cohérence inter-spécifications** : Le code E012 est réservé à la spécification de négation. Il n'est pas utilisé par `kabyle-lemmatization-spec.md`, qui laisse E012 vacant (cf. `kabyle-lemmatization-spec.md` §7bis : « E012 — Réservé par kabyle-negation-spec.md »). Aucun conflit de numérotation n'existe entre les deux documents.

---

## 8. Évaluation

La négation doit être évaluée séparément, dans le cadre du jeu de test défini par `kabyle-lemmatization-spec.md` §10bis, avec des métriques dédiées :

- exactitude de détection de `ur` ;
- exactitude de détection de `ara` ;
- exactitude du lemme verbal produit en contexte négatif (doit rester le lemme positif) ;
- taux d'erreur de l'apophonie négative, par type morphologique (une fois §4.1 rédigé) ;
- taux de confusion entre négation verbale discontinue et les autres items négatifs de §5.1 ;
- taux de formes négatives non résolues (renvoyant à `E012`).

---

## 9. Références

Reprises de `kabyle-lemmatization-spec.md` §9, pour la partie directement pertinente à la négation et à l'apophonie :

- Chaker, S. (1983). *Un parler berbère d'Algérie (Kabylie) : syntaxe*, thèse d'État, Université de Provence.
- Chaker, S. (1995). *Linguistique berbère : études de syntaxe et de diachronie*.
- Bendjaballah, S. (1999/2000/2001). Articles sur l'apophonie en kabyle.
- Guerssel, M. & Lowenstamm, J. (1993/1996). Articles sur l'apophonie en berbère.
- Naït-Zerrad, K. (1995). *Grammaire du berbère contemporain (kabyle), I — Morphologie*.
- Mammeri, M. (1976/1989). *Tajerrumt n tmaziɣt*.
- Dallet, J.-M. (1982). *Dictionnaire kabyle-français* (parler des At Mangellat).

**⚠️** Ces références sont reprises par renvoi à `kabyle-lemmatization-spec.md` §9, où leur pertinence générale est déjà établie. Leur pertinence *spécifique* à chaque règle de négation détaillée dans ce document (pages exactes, passages traitant explicitement de la négation) reste à vérifier et à citer précisément avant publication normative.

```text
[À COMPLÉTER — références exactes, pages, liens d'accès, et vérification de la disponibilité de chaque source]
```

---

## 10. Statut final

Ce document est une **amorce** qui fixe le cadre, l'interface avec `kabyle-lemmatization-spec.md`, et les questions ouvertes. Il ne doit **pas** être considéré comme normatif. Toute règle détaillée (apophonie par type morphologique, inventaire complet des items négatifs, variation dialectale) devra être validée par double vérification — source académique et corpus, ou confirmation par locuteur natif — avant intégration dans une version stable.

---

*Cette spécification est conçue pour être améliorée par la communauté, en cohérence avec `kabyle-lemmatization-spec.md`. Toute contribution validée par corpus ou par source académique peut être intégrée dans une version ultérieure.*
