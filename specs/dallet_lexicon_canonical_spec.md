# Spécification du lexique canonique Dallet–Kabyle Specs

**Statut :** brouillon de travail

**Version :** 0.2.0

**Date :** 2026-09-11

**Langue du document :** français

**Format de sortie recommandé :** JSONL, CSV/TSV et SQLite

**Statut de validation linguistique :** à compléter

## 0. Changelog depuis la v0.1

Cette version intègre les résultats d'un audit empirique de `dictionary.json`
(13 975 articles, `dataModelVersion: 8`, dernière modification source
2023-12-03) réalisé avec le script `dallet_audit.py`. Les changements
principaux :

1. **Modèle de racine corrigé** — `root` n'est pas un champ d'article mais
   une position structurelle (section 6, 13).
2. **Fréquences réelles des types éditoriaux** intégrées à la section 4.2,
   avec les taux de chevauchement mesurés.
3. **Table des valeurs de `nature`** remplacée par l'inventaire exhaustif
   observé (25 valeurs, section 8.3) et une étape de normalisation
   explicite.
4. **Statut `missing-target`** requalifié de cas limite à cas fréquent
   (26 % des renvois), avec une règle explicite pour les cibles à sens
   multiples (section 11).
5. **Inventaire de caractères** étendu avec les caractères fréquents
   observés mais absents de la v0.1 (section 12.2), à croiser avec
   `nonUnicodeCharacterSet` déclaré par la source.

Les chiffres cités dans ce document proviennent d'un run unique sur le
fichier source à la date ci-dessus ; ils doivent être régénérés à chaque
mise à jour de `dictionary.json` (voir section 18).

## 1. Objet

Cette spécification définit un modèle de données pour transformer le dictionnaire kabyle-français numérisé de Jean-Marie Dallet en une ressource lexicale canonique, traçable et interopérable avec les spécifications du projet **Kabyle Specs**.

La ressource doit séparer les **entrées lexicales**, les **renvois**, les **variantes**, les **références étymologiques**, les **informations éditoriales** et les **sous-articles**. Elle doit conserver les données originales et distinguer clairement les informations attestées dans Dallet des analyses produites par des règles automatiques.

Cette spécification ne prétend pas définir à elle seule une norme unique pour tous les parlers kabyles. Le Dallet décrit principalement le parler des At Mangellat. Les variantes dialectales, historiques, éditoriales et orthographiques doivent donc rester visibles.

## 2. Sources et provenance

La source primaire d'import est le fichier JSON structuré du projet DigitizedDallet : `dictionary.json` [1]. Le dépôt source décrit ce fichier comme un dictionnaire kabyle-français numérisé et structuré, tout en signalant que le modèle de données peut encore évoluer [2].

Chaque donnée dérivée doit conserver une provenance permettant de répondre aux questions suivantes :

1. Quelle est la forme originale dans le JSON ?
2. Quel article et quel identifiant sont concernés ?
3. Quelle règle a produit une normalisation ou une annotation ?
4. L'information est-elle explicitement attestée, inférée ou à vérifier ?
5. Quelle version du fichier source a été utilisée ?

### 2.1 Métadonnées minimales de provenance

| Champ | Description |
|---|---|
| `source_name` | Nom de la ressource, par exemple `Digitized Dallet`. |
| `source_url` | URL du fichier JSON ou du dépôt. |
| `source_article_id` | Identifiant UUID de l'article source. |
| `source_data_model_version` | Version déclarée dans `readme.dataModelVersion` (valeur observée : `8`). |
| `source_last_modification_date` | Valeur déclarée dans `readme.lastModificationDate`. |
| `source_collected_at` | Date et heure de collecte en UTC. |
| `source_commit` | Commit Git utilisé, si disponible. |
| `source_field` | Champ JSON à l'origine de l'information. |
| `evidence_type` | Type de preuve : champ explicite, position structurelle, renvoi, règle ou inférence. |

## 3. Principes directeurs

### 3.1 Conservation de la source

Les données originales ne doivent jamais être écrasées. L'export doit conserver une copie brute de `dictionary.json` et produire les données normalisées dans des fichiers séparés.

### 3.2 Séparation des niveaux d'analyse

Une forme observée, un lemme, une racine et une catégorie grammaticale sont des objets distincts. Ils ne doivent pas être fusionnés dans un seul champ.

### 3.3 Traçabilité des décisions

Toute annotation doit indiquer son origine. Une catégorie fournie par le champ `nature` ne doit pas être confondue avec une catégorie déduite de la présence de `conjugations`. Une racine dérivée de la position dans l'arbre ne doit pas être confondue avec une racine explicitement déclarée, puisqu'aucune des deux formes n'existe littéralement comme champ `root` sur l'article (voir section 13).

### 3.4 Prudence linguistique

Les relations morphologiques calculées automatiquement doivent être marquées comme candidates tant qu'elles n'ont pas été validées par une source linguistique ou par un locuteur compétent.

### 3.5 Variété kabyle

Le parler décrit, les variantes régionales, le registre et le statut historique doivent être conservés lorsqu'ils sont connus. Une variante ne doit pas être supprimée au profit d'une forme considérée comme standard sans justification documentée.

## 4. Typologie des entrées

La ressource doit distinguer le **type éditorial de l'entrée** du **statut grammatical**.

### 4.1 Types éditoriaux

| Valeur de `entry_type` | Définition | Fréquence observée (approx., 13 975 articles) |
|---|---|---:|
| `lexical_entry` | Article autonome représentant une entrée lexicale ou grammaticale. | ~12 511 (`meanings` présent) |
| `redirect` | Article renvoyant à un autre article via `redirectToId`. | 1 433 |
| `variant` | Article ou forme présentant une variante explicite, sans définition autonome. | ~2 (quasi nul, voir 4.2) |
| `cross_reference` | Article principalement constitué d'un renvoi vers une autre entrée, sans définition autonome. | ~1 (quasi nul, voir 4.2) |
| `etymological_note` | Article ou information centrée sur l'étymologie, sans définition autonome. | rare — `etymologicalReferences` coexiste avec `meanings` dans 3 693 cas sur 3 698 |
| `editorial_information` | Information bibliographique, éditoriale ou descriptive. | 559 (`info`), 45 (`mark`) |
| `subarticle` | Article comprenant des sous-articles ou appartenant à une structure imbriquée. | 83 |
| `compound` | Entrée explicitement modélisée comme composée. | 92 |
| `unclassified` | Type impossible à déterminer automatiquement. | à mesurer par implémentation |

Les fréquences ci-dessus confirment que **`etymological_note` et `cross_reference` purs seront rares** : dans l'immense majorité des cas, `etymologicalReferences` et `see` coexistent avec un contenu lexical défini (`meanings`), et l'entrée doit donc rester `lexical_entry` avec ces champs conservés comme métadonnées annexes plutôt que comme type dominant.

### 4.2 Priorité de classification éditoriale

Lorsque plusieurs champs sont présents, appliquer l'ordre suivant :

1. `redirectToId` → `redirect` ;
2. `subArticles` → `subarticle` ;
3. `compound` → `compound` ;
4. `alternativeForms` sans autre contenu lexical principal (`meanings` absent) → `variant` ;
5. `see` sans définition autonome (`meanings` absent) → `cross_reference` ;
6. `etymologicalReferences` sans information lexicale suffisante (`meanings` absent) → `etymological_note` ;
7. `info` ou `mark` sans catégorie lexicale suffisante → `editorial_information` ;
8. sinon → `lexical_entry`.

**Validation empirique de cette priorité (v0.2) :** les combinaisons qui casseraient un typage unique n'apparaissent pas dans le corpus observé — `redirectToId + subArticles` : 0 cas ; `redirectToId + alternativeForms` : 0 cas ; `subArticles + compound` : 0 cas. La hiérarchie à un seul niveau `entry_type` par entrée est donc suffisante pour ces trois champs structurants. En revanche, `alternativeForms + meanings` (875 cas) et `see + meanings` (462 cas) sont fréquents : dans ces cas, `meanings` l'emporte par construction de la règle (étapes 4 et 5 exigent son absence), ce qui est le comportement voulu, mais l'implémentation doit **conserver `alternativeForms` et `see` comme relations associées** (section 10) même quand l'entrée est typée `lexical_entry`, pour ne pas perdre l'information.

Cette priorité reste provisoire pour le champ `unclassified` et devra être validée sur un échantillon annoté manuellement (voir section 16, échantillonnage stratifié désormais généré automatiquement).

## 5. Modèle conceptuel

Le modèle minimal comporte les objets suivants :

```text
Entry
 ├── LexemeForm
 ├── RootMembership
 ├── GrammaticalAnnotation
 ├── Sense
 ├── VariantRelation
 ├── RedirectRelation
 ├── MorphologicalEvidence
 └── Provenance
```

Une entrée peut comporter plusieurs sens, plusieurs formes alternatives et plusieurs preuves grammaticales. Une racine peut regrouper plusieurs entrées, mais ce regroupement doit être décrit comme une **famille Dallet** tant que la relation morphologique n'est pas validée. Le corpus observé compte 3 749 familles de racines réparties en 2 506 familles courtes (1 à 3 articles), 1 028 familles moyennes (4 à 10 articles) et 215 familles larges (plus de 10 articles).

## 6. Schéma JSONL recommandé

Chaque ligne de `dallet_lexicon_canonical.jsonl` représente une entrée.

```json
{
  "entry_id": "2fd8de45-9234-49a8-af88-1cf3b97d1b2b",
  "entry_type": "lexical_entry",
  "form": {
    "surface_original": "mceḍ",
    "surface_nfc": "mceḍ",
    "dallet_names": ["mceḍ"]
  },
  "lemma": {
    "value": "mceḍ",
    "status": "source-attested",
    "confidence": 1.0,
    "method": "source-form"
  },
  "root": {
    "root_id": "M-C-Ḍ",
    "display": "M C Ḍ",
    "status": "source-attested",
    "evidence": "structural-position",
    "position": {
      "letter": "M",
      "root_group": "MCḌ"
    }
  },
  "grammatical": {
    "pos": ["VERB"],
    "gender": null,
    "number": null,
    "nominal_state": null,
    "dialect": "At Mangellat"
  },
  "morphology": {
    "prefix": null,
    "conjugations": null,
    "verbal_nouns": null,
    "plural_forms": null,
    "feminine_forms": null,
    "feminine_plural_forms": null,
    "annexation": null
  },
  "senses": [
    {
      "translations_fr": ["Peigner", "Se peigner"],
      "examples": []
    }
  ],
  "relations": [],
  "validation": {
    "status": "source-attested",
    "confidence": 1.0,
    "native_review": "not_assessed",
    "needs_review": false
  },
  "provenance": {
    "source_name": "Digitized Dallet",
    "source_article_id": "2fd8de45-9234-49a8-af88-1cf3b97d1b2b",
    "source_field": ["name", "meanings"],
    "source_url": "https://github.com/sferhah/DigitizedDallet",
    "collected_at": "2026-09-11T00:00:00Z"
  }
}
```

**Changement v0.2 :** `root.evidence` utilise désormais `structural-position` (dérivée de `letters[].roots[].articles[]`) au lieu de `explicit-root-field`, qui n'existe pas dans le fichier source. Le champ `root.position` expose explicitement la lettre et le groupe de racine d'origine pour permettre une re-dérivation vérifiable.

## 7. Champs obligatoires

| Champ | Obligatoire | Contraintes |
|---|---:|---|
| `entry_id` | Oui | Identifiant stable et unique (`id` source, UUID, 100 % uniques sur les 13 975 articles observés). |
| `entry_type` | Oui | Valeur de la typologie définie en section 4. |
| `form.surface_original` | Oui | Forme telle qu'affichée dans la source (`name`, 100 % des articles). |
| `form.surface_nfc` | Oui | Forme Unicode NFC, sans remplacement silencieux. |
| `root.root_id` | Oui | Toujours dérivable par position structurelle (voir 6, 13) — n'est donc plus optionnel comme en v0.1. |
| `lemma.value` | Non | Peut rester nul ou candidat. |
| `grammatical.pos` | Oui | Peut contenir `UNKNOWN` si aucune preuve n'est disponible. |
| `senses` | Non | Liste des sens et exemples conservés (absent sur ~10,5 % des articles, notamment les renvois purs). |
| `validation.status` | Oui | Statut défini en section 9. |
| `provenance` | Oui | Provenance minimale obligatoire. |

## 8. Annotation grammaticale

### 8.1 Sources de preuve

Les preuves doivent être enregistrées dans une structure indépendante :

```json
{
  "category": "VERB",
  "status": "structural-strong",
  "source_field": "conjugations",
  "detail": "...",
  "confidence": 0.98
}
```

### 8.2 Hiérarchie provisoire des preuves

| Niveau | Source | Statut par défaut | Fréquence observée | Confiance indicative |
|---|---|---|---:|---:|
| 1 | `nature` explicite reconnue | `explicit-nature` | 807 articles (5,8 %) | 1,00 |
| 2 | `conjugations` | `structural-strong` | 6 038 articles (43,2 %) | 0,98 |
| 2 | `verbalNouns` | `structural-strong` | 5 446 articles (39,0 %) | 0,95 |
| 2 | `annexation` | `structural-strong` | 3 296 articles (23,6 %) | 0,90 |
| 3 | `pluralForms` | `structural-weak` | 3 519 articles (25,2 %) | 0,72 |
| 3 | `feminineForms` | `structural-weak` | 624 articles (4,5 %) | 0,62 |
| 3 | `gender` | `structural-weak` | 462 articles (3,3 %) | 0,60 |
| 4 | traduction française seule | `candidate` | — | Non calibrée |

Les valeurs de confiance restent provisoires et non calibrées : elles ne sont pas dérivées d'un taux d'accord mesuré entre sources concurrentes (ex. `nature` explicite vs `conjugations` sur les mêmes articles) et **ne doivent pas être publiées telles quelles comme probabilités**. Une calibration empirique (comparer les cas où plusieurs preuves coexistent pour le même article) reste un travail à faire avant la v1.0 ; en attendant, une implémentation prudente peut se limiter à trois niveaux ordinaux (fort / moyen / faible) plutôt qu'à ces décimales.

### 8.3 Catégories principales — table de normalisation de `nature`

Le champ `nature` est du **texte libre en français avec abréviations non standardisées**. L'inventaire exhaustif observé sur le corpus (25 valeurs distinctes pour 807 articles) est le suivant, avec la normalisation recommandée avant mapping vers UD :

| Valeur brute observée | Occurrences | Forme normalisée | Catégorie UD indicative |
|---|---:|---|---|
| `adj.` | 581 | adjectif | `ADJ` |
| `vb. de qual.` / `vb de qual.` / `Vb. de qual.` / `verb. de qual.` / `vestige de conj. de verbe de qual.` / `vestige de {{conjug.}} de {{vb.}} de {{qual.}}` | 190+3+1+1+1+1 | verbe qualificatif | `VERB` (trait `Quality=Yes` à définir) |
| `interj.` / `interjection.` / `interjection` / `interjection ` | 8+1+1+1 | interjection | `INTJ` |
| `prép.` | 3 | préposition | `ADP` |
| `n. vb.` / `nom vb.` | 2+1 | nom verbal | `NOUN` avec trait morphologique spécifique à définir |
| `subst.` | 2 | substantif | `NOUN` |
| `adj. et subst.` / `n. subst. et adj.` / `adj. et n. subst.` / `adj. et substantif` / `n. substantif et adj.` / `subst. et adj.` | 1×6 | nom_or_adjectif (double étiquette) | `X` ou annotation ambiguë hors UD final — nécessite un trait multi-POS, pas une valeur unique |
| `interj / adv.` | 1 | interjection_or_adverbe | double étiquette, idem ci-dessus |
| `adv.` | 1 | adverbe | `ADV` |
| `conjonct. de coord. alternative.` | 1 | conjonction de coordination | `CCONJ` |
| `n. pr. masc.` | 1 | nom propre masculin | `PROPN` |

**Règle de normalisation (v0.2) :** appliquer `strip()` puis `lower()` avant comparaison, puis faire correspondre via une table d'équivalence explicite (et non une correspondance approximative), car les variantes observées sont dues à des différences de casse et de ponctuation, pas à des catégories réellement distinctes. Les six variantes "double étiquette" (`X et Y`) ne doivent pas être réduites à une seule catégorie UD : elles indiquent une ambiguïté catégorielle réelle dans la source et doivent être conservées comme telle (`pos: ["ADJ", "NOUN"]`, `validation.status: "needs-native-review"`).

La conversion vers UD ne doit pas supprimer l'annotation originale ni les preuves utilisées.

## 9. Statuts de validation

| Statut | Définition |
|---|---|
| `source-attested` | Information explicitement présente dans le JSON source. |
| `explicit-nature` | Catégorie fournie par le champ `nature`. |
| `structural-strong` | Catégorie déduite d'un champ morphologique fortement informatif. |
| `structural-weak` | Catégorie seulement suggérée par des indices morphologiques ambigus. |
| `candidate` | Analyse plausible non confirmée. |
| `verified` | Analyse confirmée par une source linguistique ou une validation compétente. |
| `needs-native-review` | Analyse nécessitant une vérification par un locuteur compétent (inclut désormais les cas de double étiquette de `nature`, voir 8.3, et les renvois à cible multi-sens, voir 11). |
| `disputed` | Analyse faisant l'objet d'une divergence documentée. |
| `not-classified` | Aucune analyse disponible. |
| `not-applicable` | Élément éditorial ou renvoi ne pouvant pas recevoir cette annotation. |

## 10. Relations entre entrées

Les relations doivent être exportées dans `dallet_lexical_relations.jsonl`.

```json
{
  "relation_id": "...",
  "relation_type": "redirect",
  "source_entry_id": "...",
  "target_entry_id": "...",
  "status": "source-attested",
  "provenance": {
    "source_field": "redirectToId"
  }
}
```

Les relations minimales sont :

| Relation | Champ source typique | Description | Fréquence observée |
|---|---|---|---:|
| `redirect` | `redirectToId` | Renvoi d'une entrée vers une entrée cible. | 1 433 |
| `alternative-form` | `alternativeForms` | Forme alternative explicitement associée. | 877 articles concernés |
| `cross-reference` | `see` | Renvoi éditorial ou lexical. | 463 articles concernés |
| `etymological-reference` | `etymologicalReferences` | Référence étymologique. | 3 698 articles concernés |
| `contains-subarticle` | `subArticles` | Relation entre article et sous-article. | 83 articles concernés |
| `listed-in-article` | `inArticles` | Relation inverse ou appartenance éditoriale. | 31 articles concernés |
| `same-root-family` | position structurelle (`letters[].roots[].articles[]`) | Appartenance à une même famille Dallet. | 3 749 familles |
| `standardized-form` | `standardizedForm` | Relation entre forme source et forme indiquée comme standardisée. | 10 articles concernés |

**Changement v0.2 :** la relation `same-root-family` est désormais explicitement rattachée à la position structurelle plutôt qu'à un champ `root`, cohérent avec la correction de la section 13.

Une relation ne doit pas être interprétée comme une relation morphologique certaine sauf si son type et sa preuve l'établissent explicitement.

## 11. Résolution des renvois

Les entrées possédant `redirectToId` doivent être conservées dans la base, mais leur contenu lexical peut être enrichi par résolution vers l'article cible.

**Changement v0.2 — fréquence réelle des cibles manquantes :** sur les 1 433 renvois observés, 374 (26,1 %) pointent vers un identifiant absent du fichier source. `missing-target` n'est donc **pas un cas limite mais un résultat fréquent et attendu**, à ne pas traiter comme une anomalie de pipeline : le rapport d'audit doit le signaler de façon visible (ex. en tête de rapport) plutôt que comme une ligne parmi d'autres. Aucun cycle ni auto-référence n'a été observé sur ce corpus (0 cas chacun), ce qui reste à vérifier à chaque régénération.

**Changement v0.2 — cibles à sens multiples :** 81 renvois (5,7 % des renvois totaux, soit environ 7,6 % des 1 059 renvois résolus) pointent vers un article cible comportant plusieurs `meanings`. La résolution ne doit **pas hériter automatiquement d'un sens** dans ce cas — il n'existe dans le fichier source aucune indication de quel sens de la cible est visé par le renvoi. La règle est donc :

- si l'article cible a un seul `meanings` → hériter directement (`resolution_status: "resolved"`) ;
- si l'article cible a plusieurs `meanings` → `resolution_status: "resolved-ambiguous-senses"`, laisser `senses` vide côté entrée source, et marquer `validation.status: "needs-native-review"`.

La résolution doit produire les champs suivants :

```json
{
  "redirect": {
    "target_id": "...",
    "target_exists": true,
    "resolution_status": "resolved",
    "target_form": "...",
    "target_root": "...",
    "target_pos": ["VERB"]
  }
}
```

Les valeurs possibles de `resolution_status` sont :

- `resolved` : l'identifiant cible existe, a un sens unique, et l'article cible a été trouvé ;
- `resolved-ambiguous-senses` : l'identifiant cible existe mais comporte plusieurs sens (nouveau en v0.2, voir ci-dessus) ;
- `missing-target` : l'identifiant cible n'existe pas dans le fichier source (26,1 % des cas observés — fréquent, pas exceptionnel) ;
- `self-reference` : l'entrée pointe vers elle-même ;
- `cycle-detected` : une chaîne de renvois forme un cycle ;
- `not-applicable` : l'entrée ne comporte pas de renvoi.

La résolution ne doit pas supprimer l'entrée source ni remplacer sa forme par celle de la cible.

## 12. Normalisation orthographique

La normalisation doit respecter les spécifications orthographiques kabyles retenues par le projet Kabyle Specs [3].

### 12.1 Conservation des formes

Chaque entrée doit pouvoir conserver :

```text
surface_original
surface_nfc
surface_normalized
surface_candidate_standard
```

`surface_original` est la valeur source. `surface_nfc` est une normalisation Unicode réversible au niveau des caractères (0 forme non-NFC détectée sur 28 309 formes observées — la source est déjà normalisée NFC). `surface_normalized` applique uniquement les règles explicitement adoptées. `surface_candidate_standard` est réservée à une proposition qui n'est pas encore validée.

### 12.2 Caractères

Les caractères kabyles distinctifs listés en v0.1 doivent être conservés, notamment :

```text
ɛ Ɛ ɣ Ɣ č Č ǧ Ǧ ḍ Ḍ ḥ Ḥ ṛ Ṛ ṣ Ṣ ṭ Ṭ ẓ Ẓ
```

**Changement v0.2 — inventaire étendu :** l'audit sur 28 309 formes de surface a révélé des caractères fréquents absents de cette liste v0.1 et qui doivent être ajoutés à l'inventaire autorisé (ou explicitement classés comme transcription historique à convertir) :

| Caractère | Occurrences | Nom Unicode |
|---|---:|---|
| `ţ` | 1 079 | LATIN SMALL LETTER T WITH CEDILLA |
| `ʷ` | 772 | MODIFIER LETTER SMALL W (labio-vélarisation) |
| `ḱ` | 681 | LATIN SMALL LETTER K WITH ACUTE |
| `ġ` | 308 | LATIN SMALL LETTER G WITH DOT ABOVE |
| `ḋ` | 249 | LATIN SMALL LETTER D WITH DOT ABOVE |
| `ḃ` | 226 | LATIN SMALL LETTER B WITH DOT ABOVE |
| `ṫ` | 182 | LATIN SMALL LETTER T WITH DOT ABOVE |
| `ɉ` | 60 | LATIN SMALL LETTER J WITH STROKE |
| `ḷ` | 26 | LATIN SMALL LETTER L WITH DOT BELOW |
| `ḇ` | 21 | LATIN SMALL LETTER B WITH LINE BELOW |
| `ž` | 16 | LATIN SMALL LETTER Z WITH CARON |
| `ṯ` | 13 | LATIN SMALL LETTER T WITH LINE BELOW |
| `ḵ` | 13 | LATIN SMALL LETTER K WITH LINE BELOW |

La source déclare elle-même un `nonUnicodeCharacterSet` de 7 entrées (`c_WITH_DOT_BELOW`, `k_WITH_DOT_ABOVE`, `j_WITH_DOT_BELOW`, `z_WITH_CEDILLA_BELOW`, `z_WITH_CEDILLA_AND_DOT_BELOW`, `s_WITH_CEDILLA_AND_DOT_BELOW`, `d_WITH_LINE_AND_DOT_BELOW`), qui ne couvre qu'une partie des caractères effectivement observés (par exemple `ţ`, le plus fréquent avec 1 079 occurrences, n'y figure pas explicitement sous ce nom). Une table de correspondance complète entre caractères observés et `nonUnicodeCharacterSet` déclaré doit être produite avant de finaliser `normalization_profile` (section 18), afin de déterminer lesquels de ces caractères relèvent d'une convention de transcription Dallet historique à convertir, et lesquels sont des caractères kabyles distinctifs à ajouter à l'inventaire permanent.

### 12.3 Variantes

Les graphies issues de Dallet et les graphies normalisées par Kabyle Specs doivent être stockées dans des champs distincts. Une variante ne doit pas être supprimée parce qu'elle ne correspond pas à la convention d'un autre parler.

## 13. Racines et familles lexicales

**Changement v0.2 — correction majeure :** contrairement à ce que suggérait la v0.1, il n'existe **aucun champ `root` sur l'objet article** dans `dictionary.json`. La racine est entièrement portée par la position de l'article dans la structure `letters[i].roots[j].articles[k]` : `letters[i].name` donne la lettre, `roots[j].name` donne la racine affichée. Toute mention de « champ `root` de DigitizedDallet » dans les versions antérieures de cette spec doit être comprise comme une dérivation structurelle, pas comme une lecture directe de champ.

Cette position constitue une source d'attestation lexicographique dérivée. Elle ne constitue pas automatiquement une preuve de productivité morphologique.

La ressource doit donc utiliser les termes suivants :

- `root_attested` pour une racine dérivée de la position structurelle dans la source (remplace la notion de « racine explicitement fournie par un champ ») ;
- `dallet_root_family` pour le regroupement des entrées par cette racine (3 749 familles observées) ;
- `morphological_relation` pour une relation dérivationnelle explicitement documentée ;
- `inferred_root` pour une racine calculée automatiquement (par exemple par analyse comparative inter-familles, non couverte par cette version) ;
- `needs-native-review` pour une relation nécessitant une validation.

Une famille Dallet peut regrouper des formes apparentées, des variantes, des renvois et des formes lexicalisées. Les utilisateurs doivent pouvoir filtrer ces types séparément. La distribution observée (2 506 familles courtes de 1 à 3 articles, 1 028 familles moyennes de 4 à 10 articles, 215 familles larges de plus de 10 articles) doit servir de base à l'échantillonnage stratifié de la section 16.

## 14. Formats d'export

### 14.1 Fichier canonique JSONL

Le fichier recommandé est :

```text
dallet_lexicon_canonical.jsonl
```

Il contient une entrée JSON par ligne et conserve les structures imbriquées nécessaires à la provenance et aux preuves.

### 14.2 Export tabulaire des entrées

Le fichier recommandé est :

```text
dallet_lexicon_canonical.csv
```

Il doit contenir au minimum :

| Colonne | Description |
|---|---|
| `entry_id` | Identifiant source stable. |
| `entry_type` | Type éditorial. |
| `form_original` | Forme source. |
| `form_nfc` | Forme Unicode NFC. |
| `lemma` | Lemme retenu ou candidat. |
| `root_id` | Identifiant de racine (dérivé de la position, voir section 13). |
| `root_display` | Racine affichée. |
| `pos_primary` | Catégorie principale. |
| `pos_all` | Toutes les catégories proposées (peut contenir plusieurs valeurs pour les `nature` à double étiquette, voir 8.3). |
| `validation_status` | Statut global. |
| `confidence` | Confiance indicative. |
| `redirect_target_id` | Cible d'un renvoi éventuel. |
| `redirect_resolution_status` | Statut de résolution du renvoi (voir section 11). |
| `translations_fr` | Traductions françaises. |
| `source_article_id` | Identifiant de provenance. |

### 14.3 Export des relations

Le fichier recommandé est :

```text
dallet_lexical_relations.jsonl
```

Chaque relation doit comporter :

```text
relation_id
relation_type
source_entry_id
target_entry_id
status
confidence
source_field
```

### 14.4 Base SQLite

Une base SQLite peut être produite pour la recherche locale. Elle devrait contenir au minimum les tables suivantes, avec des clés explicites :

```text
entries       (entry_id PK, entry_type, form_original, form_nfc, lemma, root_id FK, validation_status)
senses        (sense_id PK, entry_id FK, translations_fr, examples_json)
forms         (form_id PK, entry_id FK, surface_original, surface_nfc, form_kind)
roots         (root_id PK, letter, root_display, family_size)
relations     (relation_id PK, relation_type, source_entry_id FK, target_entry_id FK, status)
evidence      (evidence_id PK, entry_id FK, category, status, source_field, confidence)
provenance    (entry_id PK/FK, source_article_id, source_field_json, collected_at)
```

Cette table de clés reste indicative et devra être affinée à l'implémentation ; l'objectif de la v0.2 est de ne plus laisser le schéma SQLite sous-spécifié par rapport au JSONL.

Les champs textuels multivalués peuvent rester en JSON dans SQLite ou être normalisés dans des tables de liaison selon les besoins de l'application.

## 15. Contrôles qualité obligatoires

Chaque génération doit produire un rapport d'audit comprenant au moins :

1. le nombre total d'articles source (13 975 observés) ;
2. le nombre d'identifiants uniques (100 % uniques observés) ;
3. le nombre de formes vides (0 observé) ;
4. le nombre de racines présentes et absentes — à reformuler v0.2 : puisque la racine est structurelle (section 13), mesurer plutôt le nombre de familles de racines (3 749 observées) et leur distribution de taille ;
5. le nombre de renvois résolus et non résolus (1 059 résolus / 374 non résolus observés, **26 % — à signaler en alerte, pas en simple compteur**) ;
6. le nombre de cycles de renvoi (0 observé) ;
7. le nombre de doublons de formes (1 204 observés) ;
8. le nombre d'entrées par type éditorial ;
9. le nombre d'entrées par statut grammatical ;
10. le nombre d'entrées ambiguës (inclut désormais les `nature` à double étiquette et les renvois `resolved-ambiguous-senses`, voir 8.3 et 11) ;
11. les caractères non NFC (0 observé) ;
12. les caractères hors inventaire autorisé (3 729 occurrences observées avant extension de l'inventaire en 12.2 — à recalculer après mise à jour de la liste) ;
13. les champs inconnus du modèle JSON ;
14. les articles contenant `#NULL` (0 observé) ;
15. les champs `nature` non reconnus (25 valeurs distinctes observées, voir table 8.3).

Un pipeline ne doit pas échouer uniquement parce qu'une entrée est ambiguë. Il doit conserver l'entrée, produire un avertissement et attribuer le statut approprié.

## 16. Validation manuelle et native

La validation doit être effectuée sur des échantillons stratifiés. Les échantillons devraient couvrir :

| Strate | Critère | Taille observée |
|---|---|---:|
| Entrées lexicales | `entry_type == lexical_entry`. | ~12 511 |
| Renvois | `entry_type == redirect`. | 1 433 |
| Familles courtes | Racines avec 1 à 3 entrées. | 2 506 familles |
| Familles moyennes | Racines avec 4 à 10 entrées. | 1 028 familles |
| Familles larges | Racines avec plus de 10 entrées. | 215 familles |
| Catégories fortes | `explicit-nature` ou `structural-strong`. | à mesurer par intersection |
| Catégories faibles | `structural-weak`. | à mesurer par intersection |
| Ambiguïtés | `needs-native-review` (double étiquette `nature`, sens multiples). | 25 (nature) + 81 (renvois) |
| Variantes | Articles comportant `alternativeForms`. | 877 |

Un tirage aléatoire stratifié (seed fixée pour reproductibilité) peut être généré directement à partir de ces strates — voir `dallet_audit.py`, section G.

Chaque validation doit conserver :

```text
item_id
reviewer_id
review_date
decision
comment
source_consulted
```

Les formes générées ou normalisées par un système automatique ne doivent pas être marquées `verified` sans preuve externe ou validation compétente.

## 17. Interopérabilité avec Kabyle Specs

Le lexique canonique doit pouvoir alimenter les composants suivants :

| Composant | Données utilisées |
|---|---|
| Orthographe | `surface_original`, `surface_nfc`, anomalies Unicode (voir inventaire étendu, section 12.2). |
| Lemmatisation | `form`, `lemma`, `pos`, `root` (dérivé, section 13), états nominaux. |
| Tokenizer morphologique | formes, préfixes, clitiques et relations morphologiques. |
| Conjugaison | `conjugations`, lemmes verbaux et preuves. |
| État nominal | `annexation`, genre, nombre et lemme. |
| UD | `pos`, traits morphologiques, relations et statut de validation. |
| Hunspell | lemmes, variantes acceptées et métadonnées de flexion. |
| G2P | formes normalisées et informations phonologiques lorsqu'elles existent. |

L'export vers ces outils doit être dérivé du corpus canonique. Aucun outil cible ne doit devenir la seule copie de la donnée source.

## 18. Versionnement et reproductibilité

Chaque export doit être associé à :

```text
source_data_model_version   (observé : 8)
source_last_modification_date (observé : 2023-12-03T00:12:05.8010488+01:00)
source_commit
pipeline_version
spec_version                (0.2.0)
generated_at
normalization_profile       (à définir — voir 12.2)
```

Une modification de règle qui change une catégorie, un lemme, une racine ou un type éditorial doit entraîner une nouvelle version du pipeline ou de la spécification.

Les fichiers dérivés doivent pouvoir être régénérés à partir de la source brute et d'un script versionné (`dallet_audit.py` pour l'audit, à compléter par un script d'export séparé). Les corrections manuelles doivent être stockées dans un fichier d'annotations séparé plutôt que directement dans la copie brute.

## 19. Limites connues et questions ouvertes

Les points suivants restent à améliorer :

1. la définition opérationnelle de `lexical_entry` par rapport à `variant` — largement clarifiée par l'audit v0.2 (`variant` pur est un cas quasi inexistant, ~2 occurrences) ;
2. la résolution des renvois indirects et des cycles — aucun cycle observé sur ce run, à revérifier à chaque régénération ;
3. la distinction entre nom et adjectif lorsque seuls le genre et le pluriel sont disponibles ;
4. la conversion des noms verbaux vers UD ;
5. le traitement des formes d'annexion manquantes signalées par le projet source (`toDoTasks: ["create articles for annexed state"]`, déclaré par la source elle-même) ;
6. la calibration des scores de confiance (section 8.2) — reste à faire, non résolu par cet audit ;
7. l'identification du dialecte lorsque l'article ne le précise pas ;
8. la validation des racines regroupant des formes lexicalisées éloignées ;
9. l'alignement entre la transcription historique de Dallet et les conventions orthographiques contemporaines — partiellement informé par l'inventaire de caractères étendu (12.2), mais la classification transcription-historique vs caractère-distinctif reste à faire ;
10. les règles de licence et d'attribution à documenter pour chaque redistribution — non résolu, question juridique hors du champ de cet audit ;
11. *(nouveau v0.2)* la source déclare elle-même une tâche non terminée : `restore all article.dalletNames` — à surveiller, car `dalletNames` n'est présent que sur 87,5 % des articles et son incomplétude est reconnue par le projet source lui-même.

Ces points doivent rester visibles dans les rapports et ne doivent pas être résolus silencieusement par le pipeline.

## 20. Critères d'acceptation de la version 0.2

Une implémentation conforme à cette version doit :

- importer le JSON source sans le modifier ;
- produire un identifiant unique pour chaque article ;
- distinguer au minimum les renvois et les entrées lexicales ;
- dériver les racines par position structurelle, sans supposer un champ `root` explicite ;
- conserver les variantes et traductions, y compris quand elles coexistent avec `meanings` (voir 4.2) ;
- produire les preuves des annotations grammaticales ;
- distinguer `explicit-nature`, `structural-strong`, `structural-weak` et `not-classified` ;
- normaliser les valeurs de `nature` selon la table de la section 8.3 avant tout mapping UD ;
- exporter les relations de renvoi séparément, avec un statut `resolved-ambiguous-senses` distinct pour les cibles à sens multiples ;
- signaler les renvois cassés comme un résultat fréquent (~26 %), pas comme une anomalie ;
- produire un rapport d'audit incluant la distribution des familles de racines ;
- signaler les cas nécessitant une validation ;
- être reproductible à partir de la source et du code versionné.

## 21. Évolutions prévues

Les versions ultérieures pourront ajouter :

- un schéma JSON formel ;
- une ontologie des relations lexicales ;
- un export CoNLL-U pour les exemples ;
- un format Lexical Markup Framework ;
- une interface de validation collaborative ;
- des tests unitaires pour l'orthographe et la lemmatisation ;
- un alignement avec les tables de conjugaison ;
- des annotations dialectales et de registre ;
- un index plein texte français-kabyle ;
- un graphe interactif des racines et des variantes ;
- *(nouveau v0.2)* une calibration empirique des scores de confiance de la section 8.2, à partir des cas où plusieurs preuves grammaticales coexistent sur le même article ;
- *(nouveau v0.2)* une table de correspondance complète entre les caractères observés hors inventaire (section 12.2) et le `nonUnicodeCharacterSet` déclaré par la source.

## Références

[1]: https://raw.githubusercontent.com/sferhah/DigitizedDallet/master/DigitizedDallet/wwwroot/dictionary.json "Dictionnaire JSON structuré Digitized Dallet"

[2]: https://github.com/sferhah/DigitizedDallet "Projet DigitizedDallet et documentation du modèle de données"

[3]: https://kabyle-specs.github.io/ "Kabyle Language Specs — spécifications techniques pour le kabyle"

[4]: https://github.com/kabyle-specs/kabyle-specs.github.io/blob/main/specs/kabyle-lemmatization-spec.md "Kabyle Lemmatization Specification"

[5]: https://github.com/kabyle-specs/kabyle-specs.github.io/blob/main/specs/kabyle-orthography-specs.md "Kabyle Latin Orthography and Character Normalization"
