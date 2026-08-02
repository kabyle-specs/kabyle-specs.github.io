# Architecture du dictionnaire Hunspell kabyle (Taqbaylit)

**Auteurs** : Athmane Mokraoui (boffire), mainteneur des ressources NLP kabyles ; M. Belkacem, auteur du dictionnaire source *Imseɣti n tira n teqbaylit* (v1.0, MIT).

**Date** : 2 août 2026

**Cible** : Grand public, linguistes, développeurs NLP/TAL, mainteneurs de correcteurs orthographiques.

**Statut** : Draft — Phase 1 terminée. Dataset nettoyé publié sur [HuggingFace](https://huggingface.co/datasets/boffire/hunspell-kab).

---

## Résumé

Le kabyle (Taqbaylit, code ISO 639-1 `kab`) dispose d'un correcteur orthographique Hunspell développé par M. Belkacem sous licence MIT (v1.0). Ce document expose la structure métadonnées du dictionnaire (`kab.dic`), l'architecture des règles d'affixation (`kab.aff`), inventorie son système de balises morphologiques (`po:`, `st:`, `is:`), documente les anomalies détectées lors de l'analyse automatique, et propose une feuille de route pour la modularisation du fichier `.aff` et la génération automatisée à partir de données structurées.

Une version nettoyée du dictionnaire (21 823 entrées, 6 fichiers) est disponible sur HuggingFace : [`boffire/hunspell-kab`](https://huggingface.co/datasets/boffire/hunspell-kab).

**Mots-clés** : kabyle, taqbaylit, hunspell, correcteur orthographique, dictionnaire, affixation, métadonnées morphologiques, NLP.

---

## 1. Introduction

Le dictionnaire Hunspell kabyle `imseti_n_tira_n_teqbaylit` (litt. « correcteur d'écriture kabyle ») est le seul correcteur orthographique libre et complet disponible pour la langue kabyle. Distribué sous forme de module Firefox (`.xpi`) et utilisé dans LibreOffice, il repose sur un fichier `.dic` de ~25 500 entrées brutes et un fichier `.aff` définissant ~32 classes d'affixation.

Ce document vise à :
1. **Décrire** la structure métadonnées du dictionnaire pour les linguistes et développeurs.
2. **Documenter** le système de balises (`po:`, `st:`, `is:`) et son mapping vers les features Universal Dependencies.
3. **Inventorier** les anomalies structurelles (doublons, faux-amis, compteur erroné).
4. **Proposer un pipeline de nettoyage et de génération** aligné sur les standards des dictionnaires catalan (Softcatalà) et hongrois.

---

## 2. Sources primaires

### 2.1 Dictionnaire source

**M. Belkacem**, *Imseɣti n tira n teqbaylit* (v1.0), module Firefox Hunspell, distribué via [addons.mozilla.org](https://addons.mozilla.org), **~2020**. Licence MIT.

* Fichiers : `kab.dic` (liste de mots, ~842 Ko), `kab.aff` (règles d'affixation, ~19 Ko).
* Métadonnées : système à trois niveaux (`po:`, `st:`, `is:`).
* Architecture : flags mono-caractères (a–z, A–Z), affixation inline sans blocs réutilisables.

### 2.2 Données complémentaires

**Athmane Mokraoui (boffire)**, *kabyle-verbs*, dataset HuggingFace, **2026**.
* 6 198 verbes lemmatisés, ~344 000 formes conjuguées en JSON.
* Utilisé comme référence de couverture pour valider le dictionnaire Hunspell.

**Softcatalà**, *Catalan Dictionary Tools*, [github.com/Softcatala/catalan-dict-tools](https://github.com/Softcatala/catalan-dict-tools).
* Architecture modulaire avec blocs réutilisables (`_C`, `_D`, `_E`, `_V`…).
* Pipeline de génération `build-hunspell.sh` (Perl).
* Référence architecturale pour la modularisation du `.aff` kabyle.

**Németh László**, *Hunspell*, [github.com/hunspell/hunspell](https://github.com/hunspell/hunspell).
* Documentation des directives `AF` (compression d'affixes), `FULLSTRIP`, `FLAG long`.
* Référence technique pour l'optimisation du `.aff` kabyle.

---

## 3. Structure du dictionnaire

### 3.1 Organisation des fichiers

```
imseti_n_tira_n_teqbaylit-1.0.xpi
├── dictionaries/
│   ├── kab.aff   (18 602 octets) — règles d'affixation
│   └── kab.dic   (841 983 octets) — liste de mots
├── manifest.json
└── README_dict_kab.txt
```

### 3.2 Statistiques du fichier kab.dic (v1.0 brut)

| Métrique | Valeur |
|----------|--------|
| Entrées déclarées (en-tête) | 30 000 |
| **Entrées réelles** | **25 551** |
| Lignes commentées (licence MIT) | 1 315 |
| **Écart d'en-tête** | **4 449 (ANOMALIE)** |
| Entrées avec flags (`/`) | 22 227 |
| Entrées sans flags | 4 639 |
| Entrées multi-flags | 14 538 |
| **Doublons word/flag** | **3 728** |
| Entrées avec faux-ami (`˚`) | 36 |
| Mots ≤ 2 caractères | 199 |

---

## 4. Système de métadonnées

Le `.dic` utilise un système à trois niveaux attaché après le mot et ses flags :

```
mot/flags po:catégorie st:lemme is:trait
```

### 4.1 Niveau 1 : `po:` — catégorie lexicale

Le fichier source brut contient **31 balises `po:` distinctes**, dont **28 confirmées** et **3 typos** corrigés automatiquement lors du nettoyage. Le tableau ci-dessous présente les comptages dans le source brut (avant déduplication), car 392 entrées homographes (même `mot/flags`, `po:` différent) partagent des formes identiques.

| Balise | Kabyle | Français | Occurrences (brut) | Statut |
|--------|--------|----------|-------------------|--------|
| `po:isem` | isem amagnu | nom commun | 6 004 | confirmé |
| `po:amyag` | amyag | verbe | 3 402 | confirmé |
| `po:arbib` | arbib | adjectif | 482 | confirmé |
| `po:tazelɣa` | tazelɣa | particule | 302 | confirmé |
| `po:isem_n_umdan` | isem amaẓlay | nom propre (personne) | 265 | confirmé |
| `po:isem_n_tigawt` | isem n tigawt | nom d'outil/instrument | 263 | confirmé |
| `po:amernu` | amernu | adverbe | 89 | confirmé |
| `po:tanzeɣt` | tanzeɣt | préposition | 58 | confirmé |
| `po:aferdis_n_ubhat` | aferdis n uβat | interjection | 56 | confirmé |
| `po:ameskan` | ameskan | démonstratif | 38 | confirmé |
| `po:isem_amḍan` | isem amḍan | numéral | 18 | confirmé |
| `po:amqim` | amqim | quantifieur | 15 | confirmé |
| `po:isem_n_tmurt` | isem n tmurt | nom de pays | 13 | confirmé |
| `po:isem_uzzig` | isem uzzig | nom propre | 11 | confirmé |
| `po:isem_asinan` | isem asinan | nom de plante | 8 | confirmé |
| `po:awṣil` | awṣil | adjectif qualificatif | 8 | confirmé |
| `po:isem_n_umkan` | isem n umkan | nom de lieu | 5 | confirmé |
| `po:tazelɣa_n_wurmir` | tazelɣa n wurmir | particule d'aoriste | 3 | confirmé |
| `po:tazelɣa_n_tilawt` | tazelɣa n tilawt | particule de manière | 1 | confirmé |
| `po:tazelɣa_n_tibawt` | tazelɣa n tibawt | particule de négation | 1 | confirmé |
| `po:tazelɣa_n_usiwel` | tazelɣa n usiwel | particule d'appel | 1 | confirmé |
| `po:tanzeɣt_n_wallal` | tanzeɣt n wallal | préposition de moyen | 1 | confirmé |
| `po:tanzeɣt_n_tnila` | tanzeɣt n tnila | préposition de direction | 1 | confirmé |
| `po:amassaɣ` | amassaɣ | conjonction | 1 | confirmé |
| `po:tili` | tili | verbe d'existence | 1 | confirmé |
| `po:tissi` | tissi | *(à confirmer)* | 1 | ? |
| `po:awṣil_n_yisem` | awṣil n yisem | adjectif nominal | 1 | confirmé |
| `po:isem_n_ssaɛa` | isem n ssaɛa | nom de temps | 1 | confirmé |
| `po:tazelɣ` | — | typo pour `tazelɣa` | 1 | corrigé |
| `po:isemn_tigawt` | — | typo pour `isem_n_tigawt` | 1 | corrigé |
| `po:tanzeɣṭ` | — | typo pour `tanzeɣt` | 1 | corrigé |

**Note** : `po:tazelɣa` désigne les particules en général ; les sous-catégories (`n_wurmir`, `n_tilawt`, `n_tibawt`, `n_usiwel`) ont été découvertes lors de l'analyse automatique.

**Dans le `.dic` nettoyé** : 20 balises `po:` distinctes restent après déduplication des homographes (Hunspell ne conserve qu'une entrée par `mot/flags`).

### 4.2 Niveau 2 : `st:` — référence au lemme

| Balise | Occurrences | Fonction |
|--------|-------------|----------|
| `st:xxx` | 5 365 distincts | Renvoie au lemme source pour les formes dérivées |

**Exemple** :
```
imɣuzen/y st:amɣuz is:asget   # pluriel de amɣuz
```

### 4.3 Niveau 3 : `is:` — traits morphologiques

#### 4.3.1 Genre

| Balise | Kabyle | Français | Occurrences |
|--------|--------|----------|-------------|
| `NT` | unti | féminin | 2 446 |
| `ML` | amalay | masculin | 13 |

#### 4.3.2 Nombre

| Balise | Kabyle | Français | Occurrences |
|--------|--------|----------|-------------|
| `SF` | asuf | singulier | 1 253 |
| `SG` | asget | pluriel | 1 206 |

#### 4.3.3 Combinaisons genre + nombre

| Combinaison | Exemple | Forme | Signification |
|-------------|---------|-------|---------------|
| `NT + SF` | `tabacaṛt` | ta-…-t | féminin singulier |
| `NT + SG` | `tibacaṛin` | ti-…-in | féminin pluriel |
| `ML + SF` | `amaziɣ` | a-…-∅ | masculin singulier (dérivé de féminin) |
| `ML + SG` | `imaziɣen` | i-…-en | masculin pluriel (dérivé de féminin) |

#### 4.3.4 Temps / aspect / mode (verbes)

| Balise | Français | Occurrences |
|--------|----------|-------------|
| `urmir_ussid` | aoriste intensif | 3 005 |
| `izri` | passé | 1 423 |
| `izri_ibaw` | passé à la négation | 808 |

#### 4.3.5 Types verbaux

| Balise | Français | Occurrences |
|--------|----------|-------------|
| `aswaɣ` | verbe transitif | 157 |
| `amyaɣ` | verbe réciproque | 82 |
| `attwaɣ` | verbe passif | 79 |

#### 4.3.6 Nombre (autonome)

| Balise | Français | Occurrences |
|--------|----------|-------------|
| `asget` | pluriel (général) | 3 055 |
| `asuf` | singulier (général) | 1 |

#### 4.3.7 Formes dérivées / variantes (à confirmer)

| Balise | Occurrences | Statut |
|--------|-------------|--------|
| `flippedAorist` | 1 623 | aoriste avec apophonie vocalique (a→u, e→a) |
| `flippedUrmirUssid` | 515 | aoriste intensif avec apophonie |
| `flipped` | 286 | apophonie générique |
| `flippedAoristNZ` | 195 | aoriste apophonique (variante NZ — *à confirmer*) |
| `flippedFromParticiple` | 61 | apophonie depuis le participe |
| `flippedNZ` | 22 | apophonie (variante NZ — *à confirmer*) |
| `substitutedFromAorist` | 380 | substitution consonantique depuis l'aoriste (*à confirmer*) |
| `substituted` | 209 | substitution consonantique générique (*à confirmer*) |
| `iaVerb` | 132 | verbe avec pattern C-i-C-a (*à confirmer*) |
| `possibleIaVerb` | 15 | possible pattern i-a (*à confirmer*) |
| `asgetOnly` | 5 | pluriel seulement (*à confirmer*) |
| `flippedAoristAlt1` | 22 | apophonie aoriste variante 1 |
| `flippedAoristAlt2` | 22 | apophonie aoriste variante 2 |
| `flippedUrmirUssidAlt1` | 2 | aoriste intensif variante 1 |
| `flippedUrmirUssidAlt2` | 2 | aoriste intensif variante 2 |
| `flippedUrmirUssidNZ` | 2 | aoriste intensif (variante NZ) |

#### 4.3.8 Anomalies à normaliser

| Original | Correction | Occurrences | Nature |
|----------|------------|-------------|--------|
| `nt` | → `NT` | 65 | minuscule → majuscule |
| `ml` | → `ML` | 12 | minuscule → majuscule |
| `sf` | → `SF` | 8 | minuscule → majuscule |
| `urmir_issid` | → `urmir_ussid` | 1 | faute de frappe |
| `tazelɣ` | → `tazelɣa` | 1 | typo (ɣ final manquant) |
| `isemn_tigawt` | → `isem_n_tigawt` | 1 | typo (m collé, _n_ manquant) |
| `tanzeɣṭ` | → `tanzeɣt` | 1 | typo (ṭ final au lieu de t) |

#### 4.3.9 Inconnues (1 occurrence chacune — à vérifier)

| Balise | Hypothèse |
|--------|-----------|
| `amɣaɣ` | **faute de frappe probable** → `amyaɣ` (verbe réciproque) |
| `flippedAoristAlt1` | forme alternative d'apophonie |
| `flippedAoristAlt2` | forme alternative d'apophonie |
| `flippedUrmirUssidAlt1` | forme alternative |
| `flippedUrmirUssidAlt2` | forme alternative |
| `flippedUrmirUssidNZ` | variante NZ |

---

## 5. Système de flags (kab.aff)

Le `.aff` définit 32 flags mono-caractères (a–z, A–Z) répartis en classes `PFX` (préfixes) et `SFX` (suffixes).

### 5.1 Tableau des flags

| Flag | Occurrences dans `.dic` | Classe `.aff` | Fonction |
|------|--------------------------|---------------|----------|
| `A` | 11 218 | `PFX A` | 1ʳᵉ personne pluriel (`n-`, `ne-`) |
| `m` | 10 969 | `PFX m` | 3ᵉ pers. m.sg + préfixe de participe (`i-`, `ye-`, `y-`) |
| `u` | 10 969 | `PFX u` | 3ᵉ pers. f.sg / préfixe `t-` |
| `M` | 8 738 | `SFX M` | 3ᵉ pers. m.sg (`-n`, `-en`) |
| `U` | 8 719 | `SFX U` | 3ᵉ pers. f.pl (`-nt`, `-ent`) |
| `C` | 8 716 | `SFX C` | 2ᵉ pers. m.sg (`-m`, `-em`) |
| `D` | 8 696 | `SFX D` | 2ᵉ pers. f.pl (`-mt`, `-emt`) |
| `a` | 8 620 | `SFX a` | 1ʳᵉ pers. f.sg (`-ɣ`, `-eɣ`) |
| `c` | 8 504 | `SFX c` | 2ᵉ pers. f.sg (`-ḍ`, `-eḍ`) |
| `x` | 7 940 | `SFX x` | Participe (`-n`, `-en`) |
| `G` | 6 787 | `SFX G` | Impératif m.pl forme 1 (`-t`, `-et`) |
| `H` | 6 769 | `SFX H` | Impératif m.pl forme 2 (`-m`, `-em`) |
| `F` | 6 766 | `SFX F` | Impératif f.pl (`-mt`, `-emt`) |
| `e` | 4 501 | `PFX e` | Préfixe féminin (`ta-`, `te-`, `ti-`, `t-`) |
| `w` | 2 665 | `PFX w` | Dérivation d'état masculin (`we-`, `u-`) |
| `V` | 2 490 | — | `NEEDAFFIX` — requiert un autre flag |
| `y` | 1 480 | `PFX y` | Préfixe `i-` mou pour noms |
| `N` | 1 287 | `SFX N` | Pluriel féminin (`-in`) |
| `Y` | 540 | `PFX Y` | Préfixe `y-` fort |
| `n` | 488 | `SFX n` | Variante pluriel féminin (`-in`) |
| `W` | 485 | `PFX W` | Préfixe `w-` |
| `d` | 108 | `SFX d` | Suffixe seul (2ᵉ f.sg) |
| `f` | 108 | `SFX f` | Suffixe seul (3ᵉ f.pl) |
| `z` | 107 | `SFX z` | Suffixe seul (participe) |
| `g` | 107 | `SFX g` | Suffixe seul (pluriel) |
| `t` | 106 | `SFX t` | Féminin singulier (`-t`) |
| `T` | 88 | `SFX T` | Variante pluriel féminin |
| `S` | 61 | — | `NOSUGGEST` — ne pas suggérer |
| `s` | 1 | — | |
| `X` | — | — | `CIRCUMFIX` |
| `Z` | — | — | `FORBIDDENWORD` |
| `J` | — | — | `COMPOUNDFLAG` |
| `O` | — | — | `ONLYINCOMPOUND` |
| `j` | — | — | `COMPOUNDPERMITFLAG` |

### 5.2 Problème architectural

La classe de caractères `[bcčdḍfgǧhḥjklmnpqrṛsṣtṭvwxyzẓɣɛ]` est répétée **plus de 50 fois** dans le `.aff`. Contrairement au catalan (Softcatalà), qui utilise des blocs réutilisables (`_C`, `_D`, `_E`, `_V`…), le `.aff` kabyle est entièrement inline. Cela le rend difficile à maintenir et à valider.

---

## 6. Anomalies et qualité des données

### 6.1 Bugs critiques

| Anomalie | Occurrences | Impact | Correction |
|----------|-------------|--------|------------|
| **Compteur d'en-tête erroné** | 30 000 déclaré, 25 551 réel | Hunspell ignore les entrées excédentaires | Fixer à 21 823 après nettoyage |
| **Doublons word/flag** | 3 728 | Gonflement artificiel du dictionnaire | Dédupliquer, garder première occurrence |
| **Faux-amis (`˚`) dans `.dic`** | 36 | Caractère non kabyle (`ring above` au lieu de `ʷ`) | Remplacer `˚` → `ʷ` |
| **Licence MIT en commentaires `.dic`** | 1 315 lignes | Pollution du fichier source | Déplacer vers README uniquement |
| **Mots courts dupliqués** | `k/`, `t/`, `aɣ/` ×3–4 | Redondance | Dédupliquer |
| **Tags PO mal orthographiés** | 3 (`tazelɣ`, `isemn_tigawt`, `tanzeɣṭ`) | Tags non reconnus par un validateur strict | Normaliser (§4.3.8) |

### 6.2 Problèmes structurels

| Anomalie | Occurrences | Impact | Correction |
|----------|-------------|--------|------------|
| Entrées avec métadonnées mais sans flags | 3 372 | Hunspell ne peut pas les fléchir | Ajouter flags appropriés ou marquer invariant |
| Incohérences de casse IS (`nt` vs `NT`) | 65 + 12 + 8 | Tags non reconnus par un validateur strict | Normaliser en majuscules |
| `.aff` inline non modulaire | 50+ répétitions | Maintenance error-prone | Générer depuis un fichier modèle |

---

## 7. Principes de conception pour l'amélioration

1. **Préserver les métadonnées existantes.** Le système `po:` / `st:` / `is:` est bien conçu ; il doit être aligné avec l'UD, pas remplacé.
2. **Nettoyer avant de générer.** Corriger les doublons, faux-amis et compteur avant toute refactorisation du `.aff`.
3. **`.aff` généré, `.dic` curaté.** Le `.aff` doit être produit par un script à partir d'un fichier modèle. Le `.dic` conserve ses métadonnées curatées.
4. **`ICONV` uniquement pour la normalisation.** La correction des faux-amis se fait en pré-traitement ; `ICONV` gère NFD→NFC et la canonicalisation des lookalikes.
5. **`REP` pour les suggestions.** Les erreurs clavier et ASR sont gérées via `REP`, pas `ICONV`.
6. **Alignement UD.** Les balises `is:` doivent mapper vers les features Universal Dependencies du pipeline kabyle.

---

## 8. Pipeline de nettoyage (Phase 1)

```
┌─────────────┐     ┌─────────────────┐     ┌─────────────┐
│ kab.dic v1.0│────▶│ clean-kab-dic.py│────▶│ kab.dic      │
│ (Belkacem)  │     │                  │     │ (nettoyé)    │
└─────────────┘     │ • corrige en-tête│     └─────────────┘
                    │ • déduplique     │
                    │ • répare faux-amis│
                    │ • supprime commentaires licence│
                    │ • normalise tags │
                    │ • valide st:     │
                    │ • rapporte stats │
                    └─────────────────┘
```

### 8.1 Résultats du nettoyage (v1.0 → clean)

| Métrique | Avant | Après |
|----------|-------|-------|
| Entrées | 25 551 | **21 823** |
| Doublons | 3 728 | **0** |
| Faux-amis (`˚`) | 36 | **0** |
| Lignes commentées | 1 315 | **0** |
| En-tête | 30 000 (erroné) | **21 823 (correct)** |
| Tags IS normalisés (cas) | 86 | **0 restant** |
| Tags PO normalisés (typos) | 3 | **0 restant** |
| Références `st:` manquantes | — | **0** |

---

## 9. Architecture cible du `.aff` (Phase 2)

### 9.1 Directives globales

```aff
SET UTF-8
FLAG long          # ← NOUVEAU : noms de flags lisibles
FULLSTRIP          # ← NOUVEAU : permet de supprimer le mot entier (apherèse de e-)
CIRCUMFIX X

WORDCHARS ɛƐɣƔčǧḥṛṣṭẓʷ˚°

BREAK 2
BREAK -
BREAK ‑
```

### 9.2 ICONV — normalisation d'entrée uniquement

```aff
ICONV 14
ICONV γ ɣ
ICONV Γ Ɣ
ICONV ε ɛ
ICONV Σ Ɛ
ICONV ° ʷ
ICONV ˚ ʷ
# NFD → NFC pour diacritiques combinants
ICONV ̣d ḍ
ICONV ̣t ṭ
ICONV ̣s ṣ
ICONV ̣r ṛ
ICONV ̣z ẓ
ICONV ̣h ḥ
ICONV ̌c č
ICONV ̌g ǧ
```

### 9.3 Blocs d'affixes réutilisables (inspirés du catalan)

#### Suffixes personne/nombre/genre (`_PERS`)

```aff
SFX _PERS Y 10
SFX _PERS 0 ɣ/EX   [aiu]      is:Person=1|Gender=Fem
SFX _PERS 0 eɣ/EX  [^aiu]     is:Person=1|Gender=Fem
SFX _PERS 0 ḍ/PX   [aiu]      is:Person=2|Gender=Fem
SFX _PERS 0 eḍ/PX  [^aiu]     is:Person=2|Gender=Fem
SFX _PERS 0 m/PX   [aiu]      is:Person=2|Gender=Masc|Number=Sing
SFX _PERS 0 em/PX  [^aiu]     is:Person=2|Gender=Masc|Number=Sing
SFX _PERS 0 mt/PX  [aiu]      is:Person=2|Gender=Neut|Number=Sing
SFX _PERS 0 emt/PX [^aiu]     is:Person=2|Gender=Neut|Number=Sing
SFX _PERS 0 n/EX   [aiu]      is:Person=3|Gender=Masc|Number=Sing
SFX _PERS 0 en/EX  [^aiu]     is:Person=3|Gender=Masc|Number=Sing
SFX _PERS 0 nt/EX  [aiu]      is:Person=3|Gender=Neut|Number=Sing
SFX _PERS 0 ent/EX [^aiu]     is:Person=3|Gender=Neut|Number=Sing
```

#### Clitiques objet (`_CLITIC`)

```aff
SFX _CLITIC Y 8
SFX _CLITIC 0 as .   is:Clitic=3sg.masc
SFX _CLITIC 0 am .   is:Clitic=1sg
SFX _CLITIC 0 ak .   is:Clitic=2sg
SFX _CLITIC 0 an .   is:Clitic=1pl
SFX _CLITIC 0 aɣ .   is:Clitic=1sg.irr
SFX _CLITIC 0 t  .   is:Clitic=3sg.fem
SFX _CLITIC 0 ten .  is:Clitic=3pl.masc
SFX _CLITIC 0 tent . is:Clitic=3pl.fem
```

#### Participe (`_PART`)

```aff
SFX _PART Y 2
SFX _PART 0 n/pX   [aiu]      is:VerbForm=Part
SFX _PART 0 en/pX  [^aiu]     is:VerbForm=Part
```

### 9.4 Classes de modèles verbaux

```aff
SFX V_RG Y 6
SFX V_RG 0 ɣ/EX   [aiu]      is:Person=1|Gender=Fem
SFX V_RG 0 eɣ/EX  [^aiu]     is:Person=1|Gender=Fem
SFX V_RG 0 ḍ/PX   [aiu]      is:Person=2|Gender=Fem
SFX V_RG 0 eḍ/PX  [^aiu]     is:Person=2|Gender=Fem
SFX V_RG 0 n/EX   [aiu]      is:Person=3|Gender=Masc|Number=Sing
SFX V_RG 0 en/EX  [^aiu]     is:Person=3|Gender=Masc|Number=Sing
# … délègue à _PERS, _PART, _CLITIC via flags de continuation
```

### 9.5 Moteur de suggestions (REP)

Expansion de 8 à 40+ entrées :

```aff
REP 40
# Faux-amis ASR
REP ɣ γ
REP Ɣ Γ
REP ɛ ε
REP Ɛ Σ
REP ʷ °
REP ʷ ˚

# Fallbacks clavier
REP g ǧ
REP c č
REP z ẓ
REP t ṭ
REP d ḍ
REP r ṛ
REP s ṣ
REP h ḥ

# Confusions phonétiques
REP k ç
REP a ɛ
REP u w
REP i y

# Confusions vocaliques
REP a e
REP e a
REP i u
REP u i

# Prothèse e- manquante
REP n ne
REP t te
REP ḍ eḍ
REP ɣ eɣ
```

---

## 10. Pipeline de génération (Phase 3)

```
┌─────────────────┐     ┌──────────────────────┐     ┌─────────────────┐
│ kabyle-root.tsv │────▶│  build-kabyle-dict.py │────▶│   kab.dic       │
│ (curaté)        │     │                      │     │   kab.aff       │
└─────────────────┘     │  • lit dic nettoyé    │     │   kabyle_words  │
                        │  • applique modèles   │     │   validation.md │
┌─────────────────┐     │  • étend affixes      │     └─────────────────┘
│ exclusions.txt  │────▶│  • exécute tests      │
└─────────────────┘     └──────────────────────┘
```

### 10.1 Format source

```tsv
lemme	pos	modèle	flags	features_ud	notes
ɣer	VERB	V_RG	AacCDDeeGHMmUux	VerbForm=Inf	
efk	VERB	V_WEAK_E	AacCDDeeGHMmUux	VerbForm=Inf	e initial
yeččur	VERB	V_GEM	AacCDDeeGHMmUux	VerbForm=Inf	pattern yeCC
aɣrum	NOUN	N_FREE	yNw	Gender=Masc|Number=Sing|State=Free	
```

### 10.2 Outils à développer

| Outil | Priorité | Statut | Description |
|-------|----------|--------|-------------|
| `clean-kab-dic.py` | **P0** | Terminé | Nettoyage du `.dic` Belkacem |
| `validate-dic.py` | **P0** | Terminé | Validation des références `st:` et du vocabulaire `is:` |
| `build-kabyle-dict.py` | **P1** | En attente | Générateur `.dic` + `.aff` + wordlist |
| `aff-linter.py` | **P1** | En attente | Vérification de cohérence du `.aff` |
| `hf-publish.py` | **P2** | Terminé | Publication automatique sur HuggingFace |

---

## 11. Feuille de route

### Phase 1 : Nettoyage (TERMINÉ)
- [x] Exécuter `clean-kab-dic.py` sur Belkacem v1.0
- [x] Corriger 3 728 doublons
- [x] Corriger 36 faux-amis
- [x] Corriger l'en-tête (30 000 → 21 823)
- [x] Supprimer les commentaires de licence du `.dic`
- [x] Valider les références `st:`
- [x] Normaliser les tags IS (`nt→NT`, `ml→ML`, `sf→SF`, `urmir_issid→urmir_ussid`)
- [x] Normaliser les tags PO (`tazelɣ→tazelɣa`, `isemn_tigawt→isem_n_tigawt`, `tanzeɣṭ→tanzeɣt`)
- [x] Livrables : `kab.dic` nettoyé + `cleaning-report.md` + `tag-vocabulary.md`
- [x] Publier sur HuggingFace : [`boffire/hunspell-kab`](https://huggingface.co/datasets/boffire/hunspell-kab)

### Phase 2 : Documentation (1–2 semaines)
- [ ] Confirmer les balises `is:` restantes (`flippedAorist`, `substituted`, `iaVerb`, `NZ`)
- [ ] Corriger `amɣaɣ` → `amyaɣ` (1 occurrence, typo évidente)
- [ ] Mapper les balises `is:` confirmées vers les features UD
- [ ] Mapper les balises `po:` vers les UPOS UD
- [ ] Rédiger le vocabulaire contrôlé `kabyle-metadata-vocab.md`

### Phase 3 : Modularisation (2–3 semaines)
- [ ] Ajouter `FLAG long` au `.aff`
- [ ] Extraire les blocs `_PERS`, `_PART`, `_CLITIC`, `_STATE`
- [ ] Créer les modèles verbaux `V_RG`, `V_WEAK_E`, `V_GEM`
- [ ] Ajouter `FULLSTRIP`
- [ ] Étendre `REP` à 40+ entrées
- [ ] Livrable : `kab.aff` v2.0

### Phase 4 : Automatisation (2 semaines)
- [ ] Écrire `build-kabyle-dict.py`
- [ ] Créer `kabyle-root.tsv` à partir du `.dic` nettoyé
- [ ] Mettre en place CI/CD (GitHub Actions)
- [ ] Publier sur HuggingFace (`boffire/hunspell-kab`) — **dataset publié en v1.0**

---

## 12. Questions ouvertes

Les balises et traits suivants nécessitent une confirmation par un linguiste ou un locuteur natif avant de pouvoir être intégrés dans le vocabulaire contrôlé stable :

| Question | Balises concernées | Hypothèse actuelle |
|----------|-------------------|-------------------|
| Que décrit exactement le processus « flipped » ? | `flippedAorist` (1 623), `flipped` (286), `flippedFromParticiple` (61) | Apophonie vocalique (a→u, e→a) |
| Que signifie « substituted » ? | `substitutedFromAorist` (380), `substituted` (209) | Substitution consonantique (causatif, passif, racine faible) |
| Que signifie le suffixe `NZ` ? | `flippedAoristNZ` (195), `flippedNZ` (22) | Inconnu — possible variante dialectale ou classe interne de Belkacem |
| Quel est le pattern du verbe `i-a` ? | `iaVerb` (132), `possibleIaVerb` (15) | Verbes de forme C-i-C-a |
| `amɣaɣ` est-il une faute de frappe ? | `amɣaɣ` (1) | **Probable typo** → `amyaɣ` (verbe réciproque) |
| Le kabyle autorise-t-il l'empilement de clitiques objet (ex. `-as-t`) ? | Nécessite `twofold suffix stripping` si oui | À confirmer — reporter en v3.0 si trop complexe pour Hunspell natif |
| Le nom à l'état annexé (`wəɣrum`) doit-il être dérivé de l'état libre (`aɣrum`) via `_STATE`, ou listé séparément ? | Impact sur la taille du `.dic` | Recommandation : lister séparément (trop irrégulier pour règles générales) |

---

## 13. Références

1. **M. Belkacem**, *Imseɣti n tira n teqbaylit* (v1.0), module Hunspell Firefox, [addons.mozilla.org](https://addons.mozilla.org), ~2020. Licence MIT.

2. **Athmane Mokraoui (boffire)**, *kabyle-verbs*, dataset HuggingFace, [huggingface.co/datasets/boffire/kabyle-verbs](https://huggingface.co/datasets/boffire/kabyle-verbs), 2026.

3. **Softcatalà**, *Catalan Dictionary Tools*, [github.com/Softcatala/catalan-dict-tools](https://github.com/Softcatala/catalan-dict-tools). Référence architecturale modulaire.

4. **Németh László**, *Hunspell*, [github.com/hunspell/hunspell/blob/master/docs/hunspell.5.adoc](https://github.com/hunspell/hunspell/blob/master/docs/hunspell.5.adoc). Référence technique.

5. **Athmane Mokraoui (boffire)**, *CV26 Kabyle Contamination Report*, [https://butterflyoffire.codeberg.page/cv26/kabyle_corpus_contamination_report_v2.html](https://butterflyoffire.codeberg.page/cv26/kabyle_corpus_contamination_report_v2.html), 2026. Analyse des faux-amis de caractères.

6. **Kabyle Specs**, [kabyle-specs.github.io](https://kabyle-specs.github.io/). Spécifications interdépendantes : [Keyboard], [Tokenizer], [UD], [G2P], [Color Terms].

7. **Athmane Mokraoui (boffire)**, *hunspell-kab*, dataset HuggingFace, [huggingface.co/datasets/boffire/hunspell-kab](https://huggingface.co/datasets/boffire/hunspell-kab), 2026. Dictionnaire nettoyé (21 823 entrées) + documentation.

---

*Document rédigé dans le cadre du développement des ressources NLP pour la langue kabyle. Les balises marquées « à confirmer » sont signalées comme telles et n'engagent que leurs auteurs.*
