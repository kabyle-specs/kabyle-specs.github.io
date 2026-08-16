# Kabyle Lemmatization Specification

**Version** : 1.3.2  
**Auteur** : Athmane Mokraoui. Spec rédigée et révisée par relecture critique croisée ; corrections v1.3.1 (locuteur natif) et v1.3.2 (auditabilité et cohérence formelle) intégrées.  
**Date** : 2026-08-16  
**Statut** : Spécification en cours de validation — **non normative** tant qu'aucun jeu de test annoté n'a produit de métriques (cf. §10bis) et que la checklist §11 n'est pas close.  
**Dépendances** :
- `kabyle-orthography-specs.md`
- `kabyle-morphological-tokenizer-spec.md`
- `kabyle-conjugations-specs.md`
- `kabyle-ud-specification.md`
- `kabyle-hunspell-dictionary-spec.md`
- `kabyle-negation-spec.md`

---

## Changelog

### v1.3.2 (Auditabilité, cohérence formelle et compatibilité)
- **§5.3** — Clarification du contexte de l'état d'annexion féminin : distinction formelle entre l'épenthèse de `e` (`taC… → teC…`, ex: `temɣart`, `temdint`) et la perte vocalique simple (`taC… → tC…`, ex: `tmurt`).
- **§7** — Ajout d'un **Journal de validation** pour tracer les preuves des items passés à `verified` en v1.3.1.
- **§7** — Le gloss clitique de `yefka-yas-t` est rendu prudent (`ACC.3SG` au lieu de `ACC.3SG.F`) en attente de validation morphosyntaxique fine.
- **§7** — Les intervalles de `confidence` sont explicitement marquées comme *provisoires* jusqu'à calibration par jeu de test (§10bis).
- **§7bis** — Réintroduction de `E012` sous le nom `RULE_SINGLE_SOURCE` (réintroduit v1.3.2), puis **correction en E013** dans la version auditée pour éviter le conflit avec `kabyle-negation-spec.md` qui réserve E012.
- **§4.7** — Note méthodologique interdisant la promotion vers `verified` des formes issues uniquement de Wiktionary/Glosbe.
- **§11** — Mise à jour de la checklist avec les nouveaux points de vigilance de la v1.3.2.

### Corrections d'audit intégrées (v1.3.2-rev)
- **§7bis.1** — Suppression de la table de compatibilité erronée ; clarification : les codes E001–E011 sont stables depuis la v1.3.0. Aucune renumérotation n'a eu lieu entre v1.3.0 et v1.3.1.
- **§7bis** — `E012` déplacé en **E013** (`RULE_SINGLE_SOURCE`) pour éviter le conflit avec `kabyle-negation-spec.md` qui définit E012 = `NEGATION_ITEM_NOT_INVENTORY`.
- **§7.1** — Les items du Journal de validation (`temdint`, `iẓuran`, `sbeɣ`) sont alignés sur le statut `candidate` en attendant la liaison formelle des preuves (attestation lexicographique ou corpus).
- **§5.3** — `temɣart` promu au statut `verified` par cohérence avec `temdint` (même mécanisme d'épenthèse `ta- → te-` validé par locuteur natif).
- **§5.3.C** — Nuance ajoutée sur `iẓur` : conservé par prudence méthodologique, non attesté comme doublet dialectal spécifique à ce stade.

### v1.3.1 (corrections post-relecture locuteur natif)
- **§5.3** — `tmdint` corrigé en `temdint` dans l'état d'annexion. La forme `temdint` est alignée sur le mécanisme de perte de voyelle avec `e` d'appui attesté pour les féminins en `ta-`.
- **§5.3.B / §5.3.C** — Le pluriel mixte `aẓar` → `iẓuran` est confirmé et passe au statut `verified`. Le pluriel interne `iẓur` est conservé comme variante dialectale possible.
- **§5.4** — L'exemple `sebɣeɣ` → `sebɣer` / `bɣer` (« j'ai fait entrer ») était erroné. Le verbe correct est `sbeɣ` (« peindre »), préterite 1sg `sebɣeɣ`.

### v1.3.0 (audit croisé — corrections d'anomalies internes + garde-fous)
- Retrait de la citation « ACL Anthology 2007 » non vérifiable.
- `tmdint → tamdtint` : correction de l'erreur de transcription.
- `tmurt → tamurt` reclassé de « invariant » à « perte de voyelle ».
- Chiffres du corpus Tatoeba rendus cohérents (renvoi au rapport d'audit).
- Exemple `yefka → efka` retiré de la ligne « Aoriste » (c'était un prétérit).
- Ajout de la dépendance `kabyle-hunspell-dictionary-spec.md`.
- Garde-fous : champ `status`, porte Hunspell, codes d'erreur, abandon de `lemme[tag]`, §4.7/§4.8 rédigés.

---

## 1. Résumé

Cette spécification définit les règles de **lemmatisation** pour le kabyle (Taqbaylit) dans un pipeline NLP. Elle établit la **forme de citation** (lemme canonique) pour chaque catégorie grammaticale, ainsi que les règles algorithmiques pour remonter d'une forme fléchie à son lemme.

La spécification s'appuie sur trois piliers :
1. La linguistique descriptive (Basset, Mammeri, Chaker, Naït-Zerrad, Dallet, Berkai)
2. Un paradigme de conjugaison de 6 000 verbes (~300 000 formes fléchies)
3. Le corpus Tatoeba kabyle (taille exacte à reprendre du rapport d'audit)

---

## 2. Définitions

| Terme | Définition |
|---|---|
| **Lemme** | Forme canonique sous laquelle une unité lexicale est répertoriée dans un dictionnaire de référence. |
| **Forme fléchie** | Forme morphologiquement marquée (personne, nombre, genre, état, aspect, négation…). |
| **État libre** | Forme non marquée du nom, utilisée en position autonome. |
| **État d'annexion** | Forme marquée du nom, utilisée en position de dépendance. |
| **Aoriste** | Thème aspectuel non accompli, forme de base de la conjugaison. |
| **Prétérit** | Thème aspectuel accompli (accompli/parfait). |
| **Intensif** | Thème aspectuel imperfectif, dérivé de l'aoriste par préfixation de `tt-` ou `tte-`. |
| **Apophonie** | Alternance vocalique interne conditionnée par l'aspect ou la négation. |
| **Pluriel externe** | Pluriel formé par suffixation (-en, -in) et changement a- → i-. |
| **Pluriel interne** | Pluriel brisé (broken plural) par changement vocalique interne sans suffixe. |
| **Pluriel mixte** | Pluriel combinant changement vocalique interne et suffixation. |

---

## 3. Ressources de validation

### 3.1 Paradigme verbal (6 000 verbes / ~300 000 formes)

Cette ressource contient les formes fléchies de 6 000 verbes selon les 64 types morphologiques G1–G4.

**Rôle** : validation mécanique de la forme de citation, détection des supplétifs, formalisation de l'apophonie négative, validation des dérivés.

**Limitation** : couvre exclusivement le domaine verbal.

### 3.2 Corpus Tatoeba kabyle

**Statut** : `needs-verification`. Le nombre exact de phrases et de paires bilingues doit être repris du rapport d'audit du corpus Tatoeba (dépôt `kabyle-specs`).

**Rôle** : validation contextuelle des lemmes, détection des variantes dialectales, construction des tables de cooccurrence, filtrage des formes négatives.

**Limitation** : orthographe latine INALCO uniquement ; pas de tifinagh.

### 3.3 Provenance et licence

⚠️ **À DOCUMENTER** : Le paradigme de 6 000 verbes est décrit comme « fourni par la communauté kabyle » sans référence citable, sans licence explicite, et sans méthode de constitution documentée. Tant que ces points ne sont pas clarifiés, les validations mécaniques doivent être comprises comme auto-cohérence interne au paradigme.

### 3.4 Porte de validation lexicale (Hunspell)

**Règle** : tout lemme produit **DEVRAIT** être vérifié par lookup dans `kab.dic` avant d'être considéré `verified`. Un lemme absent n'est pas nécessairement faux, mais son statut doit refléter cette absence de confirmation externe. Voir `E008` en §7bis.

---

## 4. Formes de citation par catégorie grammaticale

### 4.1 Verbes

**Règle** : La forme de citation est l'**impératif à la 2ème personne du singulier** (aoriste simple, sans indice de personne).

> « En berbère c'est la deuxième personne de l'impératif (aoriste simple), à cause de sa simplicité : c'est en effet la forme la plus simple du verbe, dépourvue même de la marque de l'indice de personne qui est indissociable du verbe en berbère. » — Berkai (2013), citant Basset (1952)

| Lemme | Impératif 2sg | Aoriste 1sg | Prétérit 3sg | Divergence |
|---|---|---|---|---|
| `kcem` | `kcem` | `ad kcemɣ` | `ikcem` | Aucune |
| `efka` | `efka` | `ad efkeɣ` | `yefka` | Aucune |
| `aru` | `aru` | `ad aruɣ` | `yaru` | Aucune |
| `ečč` | `ečč` | `ad eččeɣ` | `yečča` | Aucune |
| `afeg` | `afeg` | `ad afgeɣ` | `yufeg` | Aucune |
| `ini` | `ini` | `ad iniɣ` | `yenna` | **Supplétion** |

**Convention pour verbes sans impératif attesté** : prétérit 3sg (Berkai 2013).

### 4.2 Noms communs

**Règle** : La forme de citation est le **masculin singulier à l'état libre**.

> « Pour le nom, c'est la forme non marquée qui est adoptée généralement comme entrée. En berbère c'est le masculin singulier. » — Berkai (2013)

| Lemme | État libre | État d'annexion | Féminin | Pluriel externe |
|---|---|---|---|---|
| `argaz` | `argaz` (homme) | `wergaz` | `targazt` | `irgazen` |
| `aqcic` | `aqcic` (garçon) | `uqcic` | `taqcict` | `iqcicen` |
| `axxam` | `axxam` (maison) | `wexxam` | `taxxamt` | `ixxamen` |
| `amɣar` | `amɣar` (vieillard) | `umɣar` | `tamɣart` | `imɣaren` |
| `adrar` | `adrar` (montagne) | `udrar` | — | `idurar` |

**Justification typologique** : cohérent avec le traitement UD de l'état construit en hébreu (`Definite=Cons`) et en arabe. Le kabyle gagnerait à s'aligner sur ce précédent.

### 4.3 Noms propres

Les noms propres ne sont pas lemmatisés. Ils constituent leur propre lemme.

### 4.4 Adjectifs

**Règle** : Lemmatisé sous sa **forme masculine singulière à l'état libre**, en l'absence de divergence sémantique.

Exemple : `amellal` (blanc) → lemme `amellal`. Exception : `tamellalt` = œuf/testicule a son propre lemme.

### 4.5 Pronoms

Les pronoms personnels indépendants sont lemmatisés sous leur forme de base (`nekk`, `kečč`, `netta`/`nettat`). Les pronoms affixes sont gérés par le tokenizer.

### 4.6 Adverbes, prépositions, conjonctions

Les mots invariables sont leur propre lemme.

### 4.7 Numéraux

**Règle** : Cardinaux lemmatisés sous la forme masculine singulière à l'état libre, avec `NumType=Card`. Ordinaux : `NumType=Ord`.

**A. Cardinaux natifs 1–10** — `status: candidate`  
*(Sources : Wiktionary + Glosbe. ⚠️ Note v1.3.2 : Ces sources non académiques ne peuvent en aucun cas promouvoir une forme au statut `verified`. Une validation contre Naït-Zerrad/Dallet ou par locuteur natif est requise.)*

| Valeur | Masculin | Féminin | status |
|---|---|---|---|
| 1 | `yiwen` | `yiwet` | candidate |
| 2 | `sin` | `snat` | candidate |
| 3 | `kraḍ` | `kraḍt` | candidate |
| 4 | `kuẓ` | `kuẓt` | candidate |
| 5 | `semmus` | `semmust` | candidate |
| 6 | `sḍis` | `sḍist` | candidate |
| 7 | `sa` | `sat` | candidate |
| 8 | `tam` | `tamt` | candidate |
| 9 | `tẓa` | `tẓat` | candidate |
| 10 | `mraw` | `mrawt` | candidate |

**B. Cardinaux > 10** — `needs-native-review`. `[À COMPLÉTER]`

**C. Emprunts arabes** — `needs-native-review`. Table des variantes berbérisées vs empruntées à établir.

**D. Ordinaux** — `needs-native-review`. `[À COMPLÉTER]`

### 4.8 Particules de négation

**Règle** : Particules invariables, chacune son propre lemme. Aucune fusion avec le verbe.

**A. Négation verbale discontinue** — `status: verified` : `ur` (préverbal) … `ara` (postverbal).

**B. Autres items négatifs** — `needs-native-review`. `[À COMPLÉTER]`

---

## 5. Règles algorithmiques de lemmatisation

### 5.1 Pipeline

Le lemmatizer prend en entrée un token et produit un lemme + traits morphologiques normalisés.

### 5.2 Lemmatisation des verbes

**Étape 1** : classe morphologique (G1–G4).

**Étape 2** : suppression des affixes.

| Aspect | Indices à supprimer | Exemple | status |
|---|---|---|---|
| Aoriste | Préfixes `y-`, `t-`, `n-`, `a-`… | `[À COMPLÉTER]` | needs-native-review |
| Prétérit | Préfixes + suffixes personne | `yefka` → `efka` | verified |
| Impératif | Suffixes `-(a)θ`, `-m`, `-mθ`, `-n`, `-nt` | `fk-aθ` → `efka` (par table) | candidate |
| Participe | Préfixes `i-`/`t-` + suffixes `-n`/`-t` | `ifkan` → `efka` (par table) | candidate |

**Étape 3** : normalisation de l'apophonie.

| Type | Aoriste | Prétérit | Négatif | Règle |
|---|---|---|---|---|
| Faible | `krez` | `yekrez` | `ur krez…` | Identique |
| a → u | `afeg` | `yufeg` | `ur ufig…` | Remonter `u` → `a` |
| a → Ø | `awi` | `wwi` | — | Remonter `ww-` → `awi` |
| i → a | `mlil` | `mlal` | — | Remonter `a` → `i` |
| e → u | `mmet` | `mmut` | — | Remonter `u` → `e` |
| Ø → a | `yezz` | `yezza` | — | Lemme `ezz` |
| i-i → u-a | `inṭih` | `unṭah` | — | Remonter `u-a` → `i-i` |
| a-u → u-a | `argu` | `urga` | — | Remonter `u-a` → `a-u` |

**Étape 4** : verbes supplétifs (`yenna` → `ini`). *Note : `yura` n'est pas supplétif de `ɣer` ; c'est le prétérit de `aru`.*

### 5.3 Lemmatisation des noms

**Étape 2 : remontée à l'état libre**

| Règle | État d'annexion | État libre | Condition |
|---|---|---|---|
| Vowel alternation | `uCC-` | `aCC-` | Masc., initiale `a-` + 2 cons. |
| Vowel alternation | `weC-` | `aCC-` | Masc., initiale `a-` + 2 cons. (allographe) |
| Semi-vowel | `waCV-` | `aCV-` | Masc., initiale `a-` + voyelle |
| Semi-vowel | `wuC-` | `uC-` | Masc., initiale `u-` |
| Semi-vowel | `yeC-` | `iCC-` | Masc., initiale `i-` + 2 cons. |
| Semi-vowel | `yiCV-` | `iCV-` | Masc., initiale `i-` + voyelle |
| Perte de voyelle + épenthèse | `teC…` | `taC…` | Fém., initiale `ta-` (ex: `temɣart`, `temdint`) |
| Perte de voyelle simple | `tC…` | `taC…` | Fém., initiale `ta-` + cons. (ex: `tmurt`) — *needs-native-review* |
| Invariant | `tiCt` | `tiCt` | Fém., initiale `ti-` + cons. + `t` |

**Exemples corrigés (v1.3.1 / v1.3.2) :**

| État d'annexion | État libre | Contexte | status |
|---|---|---|---|
| `wergaz` | `argaz` | `a-CC-` → `weC-` | verified |
| `temɣart` | `tamɣart` | `taC…` → `teC…` (épenthèse) | **verified** *(v1.3.2-rev : aligné sur `temdint`)* |
| `temdint` | `tamdint` | `taC…` → `teC…` (épenthèse) | verified (v1.3.1) |
| `tmurt` | `tamurt` | `taC…` → `tC…` (perte simple) | needs-native-review |

**Étape 4 : gestion du pluriel**

- **Pluriel externe** : `argaz` → `irgazen`
- **Pluriel interne** : `adrar` → `idurar` (verified), `aẓar` → `iẓur` (conservé par prudence méthodologique ; non attesté comme doublet dialectal spécifique à ce stade)
- **Pluriel mixte** : `aẓar` → `iẓuran` (verified v1.3.1), `afus` → `ifassen`

### 5.4 Lemmatisation des dérivés

| Forme | Lemme superficiel | Lemme profond | status |
|---|---|---|---|
| `sebɣeɣ` (j'ai peint) | `sbeɣ` | `sbeɣ` | verified (v1.3.1) |
| `anefli` (agriculteur) | `anefli` | `nefla` | needs-native-review |
| `asefka` (don) | `asefka` | `efka` | candidate |

---

## 6. Gestion des variantes dialectales

**Convention** : lemme standard = variété centrale (Djurdjura / Grande Kabylie). Variantes dans le champ JSON `dialect` (abandon de la notation `lemme[tag]`).

| Variante | Lemme standard | dialect | Source | status |
|---|---|---|---|---|
| `di` / `deg` | `deg` | `maritime` | Corpus Tatoeba, Guerrab (2014) | candidate |
| `si` / `seg` | `seg` | `maritime` | Corpus Tatoeba, Guerrab (2014) | candidate |
| `anezzum` / `agezzum` | `agezzum` | `oriental` | Naït-Zerrad (2001) | needs-native-review |

---

## 7. Format de sortie et Journal de validation

Chaque token produit une structure JSON. Les intervalles de `confidence` (`verified` [0.9,1.0], `candidate` [0.5,0.9), `needs-native-review` → `null`) sont **provisoires** et devront être calibrées par les métriques du §10bis.

### 7.1 Journal de validation (Validation Log)

Pour garantir l'auditabilité, les formes promues au statut `verified` doivent être appuyées par une preuve (attestation corpus, dictionnaire, ou validateur natif). Les items ci-dessous suivent le mécanisme validé par locuteur natif (épenthèse `ta- → te-` pour les féminins) mais restent en attente de **liaison formelle** à une source académique ou à une entrée du paradigme.

| Item | Statut spécifié | Version | Preuve / Source attendue | Statut effectif (audit) |
|---|---|---|---|---|
| `temdint` (annexé de `tamdint`) | verified | v1.3.1 | Attestation Dallet/Naït-Zerrad ou corpus | **candidate** *(en attente de preuve liée)* |
| `temɣart` (annexé de `tamɣart`) | verified | v1.3.2-rev | Même mécanisme que `temdint` | **candidate** *(en attente de preuve liée)* |
| `aẓar` → `iẓuran` (pluriel mixte) | verified | v1.3.1 | Attestation lexicographique | **candidate** *(en attente de preuve liée)* |
| `sebɣeɣ` (prét. 1sg de `sbeɣ`) | verified | v1.3.1 | Paradigme verbal ou dictionnaire | **candidate** *(en attente de preuve liée)* |

> **Note méthodologique** : le statut `verified` dans le corps de la spec (§5.3, §5.3.C, §5.4) reflète la validation du **mécanisme** par locuteur natif (épenthèse, pluriel mixte, lemme verbal). Le Journal de validation distingue le mécanisme de l'**attestation item par item**. Tant que la preuve n'est pas liée, le statut effectif reste `candidate` pour tout usage normatif externe.

### 7.2 Exemples JSON

**Verbe avec clitiques** (Gloss prudent v1.3.2) :

```json
{
  "token": "yefka-yas-t",
  "lemma": "efka",
  "root": "efka",
  "pos": "VERB",
  "morph": {
    "aspect": "preterite",
    "person": "3",
    "number": "sg",
    "gender": "m",
    "clitics": ["DAT.3SG", "ACC.3SG"]
  },
  "dialect": null,
  "status": "verified",
  "confidence": 0.95,
  "source": "rule+paradigm"
}
```

*(Note v1.3.2 : `ACC.3SG` est utilisé au lieu de `ACC.3SG.F` tant que le genre exact du clitique `-t` dans ce contexte spécifique n'est pas formellement validé).*

**Nom à état d'annexion** :

```json
{
  "token": "wergaz",
  "lemma": "argaz",
  "root": null,
  "pos": "NOUN",
  "morph": {
    "gender": "m",
    "number": "sg",
    "state": "annexion"
  },
  "dialect": null,
  "status": "verified",
  "confidence": 0.98,
  "source": "rule"
}
```

**Verbe `sbeɣ`** (v1.3.1, statut audit : candidate) :

```json
{
  "token": "sebɣeɣ",
  "lemma": "sbeɣ",
  "root": "sbeɣ",
  "pos": "VERB",
  "morph": {
    "aspect": "preterite",
    "person": "1",
    "number": "sg"
  },
  "dialect": null,
  "status": "candidate",
  "confidence": 0.75,
  "source": "lexical_table",
  "error": null
}
```

**Nom annexion corrigé (v1.3.1, statut audit : candidate)** :

```json
{
  "token": "temdint",
  "lemma": "tamdint",
  "root": null,
  "pos": "NOUN",
  "morph": {
    "gender": "f",
    "number": "sg",
    "state": "annexion"
  },
  "dialect": null,
  "status": "candidate",
  "confidence": 0.75,
  "source": "rule",
  "error": null
}
```

**Sur le champ `status`** : `verified` | `candidate` | `needs-native-review`.  
**Sur le champ `confidence`** : provisoire — calibration attendue du §10bis.

---

## 7bis. Codes d'erreur

| Code | Nom | Déclencheur |
|---|---|---|
| E001 | `NON_NORMALIZED_INPUT` | Token non normalisé orthographiquement |
| E002 | `MISSING_OR_AMBIGUOUS_POS` | POS manquant ou ambigu |
| E003 | `STATE_FORM_UNRESOLVED` | État libre/annexion non résolu |
| E004 | `BROKEN_PLURAL_NOT_IN_TABLE` | Pluriel brisé absent de la table |
| E005 | `SUPPLETIVE_NOT_IN_TABLE` | Supplétif absent de la table fermée |
| E006 | `DERIVATION_UNVERIFIED` | Dérivation profonde non vérifiée |
| E007 | `NEGATIVE_APOPHONY_UNVERIFIED` | Apophonie négative non vérifiable |
| E008 | `HUNSPELL_VALIDATION_FAILED` | Lemme absent de `kab.dic` |
| E009 | `AMBIGUOUS_LEMMA` | Plusieurs lemmes candidats à POS identique |
| E010 | `DIALECT_VARIANT_UNTAGGED` | Variante dialectale détectée, champ non renseigné |
| E011 | `BORROWING_NOT_INTEGRATED` | Emprunt non intégré morphologiquement |
| E012 | *Réservé* | Réservé par `kabyle-negation-spec.md` (`NEGATION_ITEM_NOT_IN_INVENTORY`) — **ne pas utiliser dans le présent document** |
| E013 | `RULE_SINGLE_SOURCE` | Règle manque de source ou de validation (ajout v1.3.2-rev) |

> **Note de compatibilité (v1.3.2-rev)** : Les codes E001–E011 sont stables depuis la v1.3.0. Aucune renumérotation n'a eu lieu entre v1.3.0 et v1.3.1. Le code E012 est réservé par la spécification de négation ; le présent document utilise E013 pour les règles à source unique non académique.

---

## 8. Limitations et travaux futurs

1. Pluriels brisés des noms simples (nécessitent table lexicale).
2. Apophonie négative (à inférer par type G1–G4 si absent du paradigme).
3. Noms invariables en état (liste non exhaustive).
4. Néologismes et emprunts.
5. Négation syntaxique (voir `kabyle-negation-spec.md`).
6. Tifinagh (non couvert).
7. Corpus d'entraînement annoté (manquant, spec rule-based).
8. Profondeur de lemmatisation (superficiel par défaut).

---

## 9. Références

### Sources primaires

| Référence | Année | Titre | Contribution |
|---|---|---|---|
| Basset, A. | 1952 | *La langue berbère* (Handbook of African Languages, 1) | Forme de citation verbale |
| Mammeri, M. | 1976/1989 | *Tajerrumt n tmaziɣt* | Morphologie du nom |
| Dallet, J.-M. | 1982 | *Dictionnaire kabyle-français* | Monument lexicographique |
| Naït-Zerrad, K. | 1994 | *Manuel de conjugaison kabyle* | Paradigmes |
| Naït-Zerrad, K. | 1995 | *Grammaire du berbère contemporain (kabyle), I — Morphologie* | Morphologie nominale et verbale |
| Naït-Zerrad, K. | 2001 | *Grammaire moderne du kabyle* | Classification dialectale |

### Sources secondaires

| Référence | Année | Titre |
|---|---|---|
| Chaker, S. | 1973 | *Le système dérivationnel verbal berbère (dialecte kabyle)* |
| Chaker, S. | 1983 | *Un parler berbère d'Algérie (Kabylie) : syntaxe* |
| Chaker, S. | 1995 | *Linguistique berbère : études de syntaxe et de diachronie* |
| Bader, Y.F. | 1984 | *Kabyle Berber Phonology and Morphology* |
| Kessai, F. | 2019 | *Élaboration d'un dictionnaire électronique de berbère* |

### Articles

| Référence | Année | Titre |
|---|---|---|
| Berkai, M. | 2013 | « Quelques problèmes macrostructurels en lexicographie berbère » |
| Guerrab, S. | 2014 | *Analyse dialectométrique des parlers berbères de Kabylie* |
| Bendjaballah, S. | 1999/2000/2001 | Articles sur l'apophonie en kabyle |
| Guerssel, M. & Lowenstamm, J. | 1993/1996 | Articles sur l'apophonie en berbère |
| Achab, K. | 2001/2003/2012 | Articles sur l'état construit en berbère |
| Fahloune, K. | 2020 | « On the status of subject and object markers in Kabyle » |
| Galand, L. | 1969/2002 | *Études de linguistique berbère* |
| Souag, L. | 2013/2020 | Articles sur l'emprunt en berbère |

### Ressources corpus et outils

| Ressource | Type | Contribution |
|---|---|---|
| Tatoeba kabyle | Corpus bilingue | Validation contextuelle, variantes |
| KabTatoebaCorpus | Export nettoyé | Ressource NLP |
| Imsidag en-kab-parallel | Alignement bilingue | Validation par traduction |
| Hugging Face kabyle | Corpus régionaux | Cartographie dialectale |
| Paradigme verbal kabyle | 6 000 verbes / ~300k formes | Validation mécanique |
| Wikipedia — Kabyle grammar | Référence en ligne | Pluriels, état, apophonie |

### Sources NLP et typologie

| Référence | Année | Titre | Contribution |
|---|---|---|---|
| Nivre, J. et al. | 2020 | *Universal Dependencies v2* | Cadre méthodologique UD |
| Taji, D., Habash, N., Zeman, D. | 2017 | *Universal Dependencies for Arabic* | État construit arabe |
| CAMeL Tools / MADAMIRA | — | Outils NLP arabe | Séparation `lex` / `root` |
| Ammari, N. & Zenkouar, L. | 2020 | *APMorph* | FST pronominal amazighe |
| Nejme, F.-Z. et al. | 2016 | *AmAMorph* | Analyseur morphologique amazighe (NooJ) |
| Karlsson, F. et al. | 1995 | *Constraint Grammar* | Désambiguïsation contextuelle |

---

## 10. Questions ouvertes

1. Exhaustivité des supplétifs (vérification sur corpus Tatoeba)
2. Pluriels brisés (extraction systématique depuis corpus + dictionnaires)
3. Emprunts arabes : un ou deux lemmes ?
4. Profondeur de lemmatisation : surface vs racine
5. Formes négatives dans le paradigme (incluses ? générables ?)
6. Noms propres et toponymie

---

## 10bis. Méthodologie d'évaluation quantitative

### 10bis.1 Jeu de test annoté
Constituer un échantillon de 1 000 à 3 000 tokens tiré aléatoirement du corpus Tatoeba, stratifié par POS, annoté manuellement en lemmes par locuteur natif avec double annotation (kappa de Cohen).

### 10bis.2 Métriques
- Exactitude de lemmatisation (lemma accuracy)
- Exactitude par catégorie grammaticale
- Taux d'erreur par règle (apophonie négative, pluriels brisés, emprunts)

### 10bis.3 Calibration du champ `confidence`
Les taux d'erreur mesurés par catégorie de règle sont la source de calibration attendue pour `confidence`.

---

## 11. Checklist de validation par locuteur natif — v1.3.2-rev

### Statut des corrections récentes :
- [x] **v1.3.1** — `tamdint` → `temdint` : voyelle d'appui `e` confirmée (mécanisme global).
- [x] **v1.3.1** — `aẓar` → `iẓuran` : pluriel mixte confirmé (mécanisme global).
- [x] **v1.3.1** — `sbeɣ` (« peindre ») : lemme et sens confirmés.
- [x] **v1.3.2-rev** — `temɣart` aligné sur `temdint` (même mécanisme d'épenthèse).
- [x] **v1.3.2-rev** — E013 remplace E012 pour éviter le conflit avec `kabyle-negation-spec.md`.
- [x] **v1.3.2-rev** — Journal de validation : distinction entre mécanisme validé (`verified`) et attestation item par item (`candidate`).

### Points restants en attente :
- [ ] §4.1 — Prétérit 3sg de `ɣer` (« lire/appeler »)
- [ ] §4.1 — Suffixes d'impératif par personne/genre
- [ ] §4.7 — Orthographe exacte des 10 cardinaux natifs (validation académique requise, Wiktionary insuffisant)
- [ ] §4.7 — Cardinaux > 10, ordinaux, emprunts arabes
- [ ] §4.8 — Inventaire complet des items négatifs au-delà de `ur`/`ara`
- [ ] §5.2 — Exemple d'aoriste réel
- [ ] §5.2 — Paires d'apophonie verbale (`mlil→mlal`, `mmet→mmut`, etc.)
- [ ] §5.3 — `tmurt → tamurt` : conditionnement exact de la perte de voyelle simple vs épenthèse (`temdint`)
- [ ] §5.3 — `taddart`, `tuccent` : invariabilité confirmée ?
- [ ] §5.3 — `yimi` : état libre `imi` ou forme possessive ?
- [ ] §5.3 — `amic → imcac` : forme INALCO exacte
- [ ] §5.3.C — `iẓur` : attestation dialectale ou suppression si non confirmé ?
- [ ] §5.4 — `anefli → nefla` : existence et sens
- [ ] §6 — `anezzum / agezzum` : doublet dialectal confirmé ?
- [ ] §7.1 — Fournir les liens/attestations exactes pour le Journal de validation (`temdint`, `iẓuran`, `sbeɣ`).
- [ ] §7.2 — Valider le gloss clitique de `-t` dans `yefka-yas-t`.
- [ ] §3.2 — Chiffres exacts du corpus Tatoeba (reprendre du rapport d'audit)
- [ ] §3.2 — Nombre exact de paires `Imsidag en-kab-parallel`
- [ ] §10 — Toutes les questions ouvertes restent en attente

---

*Cette spécification est conçue pour être améliorée par la communauté. Toute contribution validée par corpus ou par source académique peut être intégrée dans une version ultérieure.*
