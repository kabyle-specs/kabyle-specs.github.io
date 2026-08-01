# Rapport d'analyse du corpus Tatoeba Kabyle
## MaskLID + GlotLID + DistilBERT + Hunspell — Pipeline de filtrage v3

**Auteur** : boffire / kabyle-specs  
**Date** : 2026-08-01  
**Corpus** : [Tatoeba.org](https://tatoeba.org) — `sentences.csv` (lang == kab)  
**Licence** : MIT (code) / CC BY 2.0 FR (données Tatoeba)

---

## Résumé exécutif

Ce rapport présente une analyse complète du corpus Tatoeba Kabyle (**789 712 phrases**) à l'aide d'un pipeline multi-couches combinant identification de langue (GlotLID), classification berbère (DistilBERT), dictionnaire morphologique (Hunspell kabyle) et détection de code-switching (MaskLID). L'objectif est de quantifier la contamination orthographique, le taux de code-switching réel et la qualité globale du corpus pour l'entraînement de LLM.

| Métrique | Valeur |
|----------|--------|
| **Phrases analysées** | 789 712 |
| **Phrases monolingues kab** | 787 797 (**99,76 %**) |
| **Phrases avec code-switching** | 1 915 (**0,24 %**) |
| **Phrases contaminées** (orthographe) | 28 887 (**3,66 %**) |
| **Phrases "pures"** (export final) | **756 774** (**95,83 %**) |
| **Couverture dictionnaire Hunspell** | **85,1 %** (3,17M / 3,73M tokens) |
| **Mismatch kab→tachélit** (DistilBERT) | 67 (**0,67 %** des 10K analysées) |

---

## 1. Architecture du pipeline

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Tatoeba CSV    │────▶│  Normalisation  │────▶│   GlotLID v3    │
│  (lang == kab)  │     │  orthographique │     │  (sentence-lvl) │
└─────────────────┘     └─────────────────┘     └────────┬────────┘
                                                         │
                              ┌──────────────────────────┼──────────┐
                              │                          │          │
                              ▼                          ▼          ▼
                    ┌─────────────────┐        ┌─────────────────┐  │
                    │  Hunspell kab   │        │  DistilBERT     │  │
                    │  (Imseɣti…)     │        │  (ber/kab/shi)  │  │
                    │  + prefix strip │        │  [limit: 10K]   │  │
                    └────────┬────────┘        └────────┬────────┘  │
                             │                          │           │
                             └────────────┬─────────────┘           │
                                          ▼                         │
                               ┌─────────────────┐                  │
                               │    MaskLID      │◀─────────────────┘
                               │  (token-level)  │
                               └────────┬────────┘
                                        ▼
                               ┌─────────────────┐
                               │   Filtrage      │
                               │  pure / CS /    │
                               │  contamination  │
                               └─────────────────┘
```

### Composants

| Composant | Rôle | Source |
|-----------|------|--------|
| **GlotLID v3** | Identification de langue sentence-level et word-level | [cis-lmu/glotlid](https://huggingface.co/cis-lmu/glotlid) |
| **DistilBERT** | Classification binaire kabyle / tachélit | [boffire/distilbert-kabyle-tachelhit-classifier-v2](https://huggingface.co/boffire/distilbert-kabyle-tachelhit-classifier-v2) |
| **Hunspell kab** | Dictionnaire morphologique (~30 000 lemmes + règles `.aff`) | Belkacem77 / [Imseɣti n tira n teqbaylit](https://addons.mozilla.org/firefox/addon/imseti_n_tira_n_teqbaylit/) |
| **MaskLID** | Détection de code-switching token-level | Implémentation maison |

---

## 2. Méthodologie

### 2.1 Normalisation orthographique

Spécification : [kabyle-specs.github.io](https://kabyle-specs.github.io/specs/kabyle-orthography-specs.md)

- **Faux amis** : correction automatique des caractères grecs/cyrilliques (ε→ɛ, γ→ɣ, Γ→Ɣ, Σ→Ɛ, etc.)
- **Caractères interdits** : apostrophes typographiques, ç, ñ, ø, æ, œ, ß, þ
- **Diacritiques français** : normalisation (á→a, é→e, etc.)

### 2.2 Dictionnaire Hunspell kabyle

Le dictionnaire **Imseɣti n tira n teqbaylit** (v1.0, M. Belkacem, licence MIT) est téléchargé depuis [addons.mozilla.org](https://addons.mozilla.org/firefox/addon/imseti_n_tira_n_teqbaylit/). Il contient :
- ~30 000 lemmes de base
- Règles morphologiques `.aff` (conjugaisons, pluriels, dérivations)

**Mécanismes de couverture** :
1. Lookup direct dans le dictionnaire (`lookup()` de spylls)
2. *Prefix stripping* pour formes verbales préfixées (`ireqq` → `i` + `reqq`)
3. Vocabulaire d'appoint pour faux positifs identifiés empiriquement (`Gedha`, `Asiggez`, etc.)

### 2.3 Allowlist de langues plausibles

Toute prédiction GlotLID hors de `{kab, fra, ara, eng, ber, shi}` est traitée comme du **bruit du modèle** et retombe sur la langue de la phrase. Cela élimine les faux positifs en turc, sarde, kinyarwanda, ktunaxa, etc.

### 2.4 DistilBERT — validation berbère

Limité aux **10 000 premières phrases** pour des raisons de temps de calcul (~1h15 sur CPU Colab). Le reste du corpus (779 712 phrases) est traité par GlotLID + Hunspell uniquement.

Seuils :
- Phrases < 4 mots : HIGH_CONF ≥ 0,95, LOW_CONF ≥ 0,85
- Phrases ≥ 4 mots : HIGH_CONF ≥ 0,90, LOW_CONF ≥ 0,70

---

## 3. Résultats sur 789 712 phrases

### 3.1 Vue d'ensemble

| Métrique | Valeur |
|----------|--------|
| **Phrases analysées** | 789 712 |
| **Phrases monolingues kab** | 787 797 (99,76 %) |
| **Phrases avec code-switching** | 1 915 (0,24 %) |
| **Phrases contaminées** (orthographe) | 28 887 (3,66 %) |
| **Phrases "pures"** (export final) | **756 774** (95,83 %) |
| **Mismatch kab→tach** | 67 (0,67 % des 10K analysées) |

### 3.2 Contamination orthographique

| Type | Occurrences | Phrases touchées |
|------|-------------|------------------|
| Faux ami (ε→ɛ, γ→ɣ, etc.) | 31 805 | ~28 500 |
| Non canonique | 10 092 | ~400 |
| Interdit | 617 | ~50 |

**Taux global** : **3,658 %**

> La contamination est dominée par le **epsilon grec** (U+03B5) utilisé à la place du epsilon latin kabyle (U+025B). Ce phénomène est bien documenté dans le rapport CV26 Kabyle ([butterflyoffire.codeberg.page/cv26/](https://butterflyoffire.codeberg.page/cv26/)) : 11 258 clips contaminés sur 609 940 validés (2,15 %).

### 3.3 Code-switching

| Métrique | v1 (bugué) | v2 (1K phrases) | v3 (789K phrases) |
|----------|-----------|-----------------|-------------------|
| CS détecté | **56,06 %** | 1,00 % | **0,24 %** |
| Phrases pures | 43,94 % | 99,00 % | **99,76 %** |

La chute drastique du CS (56 % → 0,24 %) s'explique par trois mécanismes :

1. **L'allowlist** qui filtre le bruit GlotLID (turc, sarde, kinyarwanda...)
2. **Le dictionnaire Hunspell** qui reconnaît les mots kabyles authentiques (`yekka`, `Yedda`, `umur`, `unafag`, `Senndet`, `Gedha`, `Asiggez`)
3. **Le prefix stripping** qui attrape les formes verbales (`ireqq` → `reqq`, `waman` → `aman`)

**Vrais emprunts détectés** (sous-ensemble représentatif sur 3 259 tokens) :

| Token | Langue | Contexte |
|-------|--------|----------|
| London | eng | Toponyme — *Rziɣ ɣer London.* |
| baseball | eng | Emprunt sportif |
| Tom | shi | Nom propre (ambigu kab/shi) |
| Google | eng | Nom propre technique |
| Philadelphia | eng | Toponyme |
| firewall | eng | Terme informatique |
| 3D | eng | Terme technique — *Asiggez 3D.* |
| voice | eng | *Common voice* |
| office | eng | *Open office* |
| internet | eng | *tuqqna n internet* |
| almend | eng | *tjeǧǧigin almend* (amandes) |
| Said | eng | Nom propre — *Muḥend Said Amlikec* |

### 3.4 Validation berbère (DistilBERT)

| Status | Count | % des 10K analysées |
|--------|-------|---------------------|
| HIGH_CONF | 9 679 | 96,79 % |
| LOW_CONF | 101 | 1,01 % |
| REJECT | 208 | 2,08 % |
| NON_BERBER | 12 | 0,12 % |
| **Mismatch kab→tach** | **67** | **0,67 %** |

Les 67 mismatches sont des phrases que GlotLID classe kabyle mais DistilBERT classe tachélit avec haute confiance. Exemples :

| Phrase | GlotLID | DistilBERT |
|--------|---------|------------|
| *Triḍ lkas n ccrab?* | kab (1,000) | tach (0,978) |
| *Ur ttru!* | kab (1,000) | tach (0,994) |
| *Uḥwaǧeɣ Ṭum tura.* | kab (1,000) | tach (0,996) |

### 3.5 Couverture du dictionnaire Hunspell

| Échantillon | Tokens totaux | Tokens reconnus | Couverture |
|-------------|--------------|-----------------|------------|
| 1 000 phrases | 4 349 | 3 797 | 87,3 % |
| 10 000 phrases | 53 948 | 46 474 | 86,1 % |
| **789 712 phrases** | **3 730 724** | **3 173 402** | **85,1 %** |

> **Note technique** : Une version intermédiaire (v final) affichait 0,1 % de couverture due à un bug d'API (`spell()` au lieu de `lookup()` dans la bibliothèque `spylls`). Ce bug a été corrigé en v3.

---

## 4. Visualisations

Le pipeline génère automatiquement un dashboard de 6 graphiques :

1. **Code-switching vs Monolingue** (pie chart)
2. **Validation berbère DistilBERT** (bar chart par status)
3. **Contaminants orthographiques** (horizontal bar chart)
4. **Distribution confiance GlotLID** (histogramme avec seuil 0,95)
5. **Couverture dictionnaire** (pie chart Hunspell vs LID)
6. **Langues secondaires détectées** (bar chart top emprunts)

![Dashboard](tatoeba_kabyle_dashboard.png)

---

## 5. Exports produits

| Fichier | Format | Contenu | Taille estimée |
|---------|--------|---------|----------------|
| `tatoeba_kab_masklid.tsv` | TSV | Toutes les phrases avec métadonnées | ~800K lignes |
| `tatoeba_kab_masklid.jsonl` | JSONL | Même contenu + tokens détaillés | ~800K objets |
| `tatoeba_kab_cs.conll` | CoNLL-U | Phrases CS uniquement (IOB2) | ~2K phrases |
| `tatoeba_kab_pure.tsv` | TSV | **Corpus filtré final** (pur) | **~757K phrases** |
| `tatoeba_kab_report.json` | JSON | Rapport statistique complet | 1 fichier |

### Critères du corpus "pur" (`tatoeba_kab_pure.tsv`)

```python
pure = df[
    (~df['is_cs']) &                    # Pas de code-switching
    (df['num_contaminants'] == 0) &     # Pas de contamination orthographique
    (~df['is_mismatch']) &              # Pas de mismatch kab→tach
    (df['berber_status'].isin(['HIGH_CONF', 'SKIPPED'])) &  # Validation berbère OK
    (df['glot_conf'] >= 0.95)          # Confiance GlotLID ≥ 0.95
]
```

**Résultat** : **756 774 phrases** (95,83 % du corpus initial).

---

## 6. Limitations et travaux futurs

### Limitations connues

1. **DistilBERT limité aux 10 000 premières phrases** : le reste du corpus (779 712 phrases) n'a pas de validation berbère explicite. Le taux de mismatch extrapolé serait de ~5 220 phrases sur l'ensemble du corpus (0,67 % × 779 712), mais cela reste une estimation.

2. **Noms propres ambigus** : les noms courts (`Tom`, `Samir`) sans caractères kabyle spécifiques sont parfois classés `shi` par GlotLID. C'est un faux positif acceptable pour le filtrage CS.

3. **Vocabulaire d'appoint statique** : les nouveaux néologismes ou noms propres non présents dans le dictionnaire Hunspell nécessitent une mise à jour manuelle de `KABYLE_EXTRA_WORDS`.

4. **GlotLID sentence-level sur phrases courtes** : une phrase de 2-3 tokens contenant un nom propre anglais peut faire basculer la prédiction sentence-level. Le fallback sur `kab` (v2.1+) corrige partiellement ce problème.

### Travaux futurs

- Intégrer un **G2P kabyle** pour la validation phonologique des mots inconnus
- Entraîner un **classifieur CS dédié** (fine-tuning d'un modèle berbère multilingue)
- Publier le corpus pur sur HuggingFace (`boffire/tatoeba-kabyle-pure`)
- Étendre le dictionnaire Hunspell avec les formes verbales des 6 198 verbes du dataset `kabyle-verbs` (344K formes conjuguées)

---

## 7. Crédits et licences

### Code
- **Auteur** : boffire (Athmane MOKRAOUI / ButterflyOfFire)
- **Licence** : MIT
- **Repository** : [kabyle-specs.github.io](https://kabyle-specs.github.io)

### Données
- **Tatoeba** : CC BY 2.0 FR — [tatoeba.org](https://tatoeba.org)
- **Hunspell kabyle** : MIT — M. Belkacem, [addons.mozilla.org](https://addons.mozilla.org/firefox/addon/imseti_n_tira_n_teqbaylit/)

### Modèles
- **GlotLID v3** : [cis-lmu/glotlid](https://huggingface.co/cis-lmu/glotlid)
- **DistilBERT kab/shi** : [boffire/distilbert-kabyle-tachelhit-classifier-v2](https://huggingface.co/boffire/distilbert-kabyle-tachelhit-classifier-v2)

---

## 8. Changelog

| Version | Date | Changements |
|---------|------|-------------|
| v1 | 2026-07-31 | Pipeline initial — bug arithmétique DistilBERT, CS 56 % (faux positifs massifs) |
| v2 | 2026-08-01 | Ajout Hunspell + allowlist — CS 1 %, corrigé bug analyzed_count |
| v2.1 | 2026-08-01 | Prefix stripping + sentence fallback — CS 0,3 % (1K phrases) |
| **v3** | **2026-08-01** | **Correction API spylls (`lookup()`), vocabulaire enrichi, 789K phrases, CS 0,24 %** |

---

*Ce rapport a été généré automatiquement par le pipeline MaskLID + GlotLID + DistilBERT + Hunspell kabyle v3.*
*Temps d'exécution total : ~1h15.*
