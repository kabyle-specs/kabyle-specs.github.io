# Spécification du Tokenizer Morphologique pour le Kabyle (Taqbaylit)

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; structuration algorithmique et synthèse bibliographique.

**Date** : 29 juillet 2026

**Version** : 0.3-draft

**Cible** : Développeurs NLP/TAL, ingénieurs en tokenization, chercheurs en linguistique berbère.

---

## Résumé

Le kabyle (Taqbaylit, ISO 639-3 `kab`) possède une morphologie verbale complexe où coexistent **affixes d'accord sujet** (obligatoires, variables), **clitiques pronominaux objets** (optionnels, invariants à l'aspect), et **préfixes dérivationnels** (causatif, passif, réfléchi, réciproque). Cette spécification propose une architecture de tokenization morphologique en couches, capable de segmenter une forme verbale comme `ad-t-iyi-d-ini-ḍ` en unités distinctes avec étiquetage fonctionnel. Elle s'appuie sur le paradigme clitique documenté par Bedar, Quellec & Voeltzel (2021), la distinction affixe/clitique de Fahloune (2020), et les règles morphophonologiques du conjugueur algorithmique kabyle (Bouamara 2026 / Mokraoui 2026).

**Mots-clés** : kabyle, taqbaylit, tokenization, morphologie, clitiques, affixes, BPE, NLP.

---

## 1. Introduction et périmètre

### 1.1 Pourquoi un tokenizer morphologique ?

Les tokenizers statistiques actuels (BPE, SentencePiece, WordPiece) segmentent le kabyle en sous-mots fréquents sans compréhension morphologique. Cela produit des segmentations arbitraires :
- `tkecmeḍ` → `tke` + `cme` + `ḍ` (BPE naïf)
- `y-fka-as-tt-id` → `yf` + `kaas` + `ttid` (coupures non morphémiques)

Un tokenizer morphologique à base de règles garantit :
- La **reproductibilité** de la segmentation.
- La **réduction du vocabulaire** (le radical `kcem` est réutilisé pour toutes ses formes conjuguées).
- L'**alignement sémantique** en traduction automatique (morphème à morphème plutôt que sous-mot à sous-mot).

### 1.2 Périmètre

Cette spec couvre :
- La **morphologie verbale** (formes de base et dérivées).
- Les **clitiques pronominaux** datifs, accusatifs et directionnels.
- Les **préfixes aspectuo-modaux** (`ad`, `ur`, `wa`).
- Les **préfixes dérivationnels** (`s-`, `ttwa-`, `ttu-`, `mm-`, `nn-`, `my-`).

Elle ne couvre pas encore :
- La morphologie nominale complète (préfixes `a-`, `ta-`, `i-`, `ti-`, état libre vs état d'annexion).
- Le sandhi inter-mots (assimilations enchaînées).
- Les emprunts non intégrés.

---

## 2. Sources primaires

### 2.1 Documents internes (Mokraoui 2026)
- **Conjugueur algorithmique** : paradigmes des 64 types morphologiques, règles du schwa (Règle 14), collision dentale `tett-` (Règle 15.2), supplétisme (`efk`, `ini`, `ecč`).
- **G2P** : inventaire des 34 graphemes, règles de spirantisation/blocage, schwa épenthétique.
- **Expression du temps** : particules `D`, `u`, `ɣiṛ`, `swaswa` (traitées ici comme tokens indépendants de niveau phrastique).

### 2.2 Bedar, Quellec & Voeltzel (2021) — Paradigme clitique
**Amazigh Bedar, Lucie Quellec & Laurence Voeltzel**, *Epenthetic glides in Taqbaylit*, Journal of African Languages and Literatures 2/2021, pp. 1-29.

Source primaire pour le **paradigme complet des clitiques pronominaux** (datifs, accusatifs, directionnels) et pour la phonologie des frontières morphémiques (glides épenthétiques [j] et [w]).

### 2.3 Fahloune (2020) — Statut affixe vs clitique
**Khokha Fahloune**, *On the status of subject and object markers in Kabyle: New evidence*, McGill Working Papers in Linguistics 26.1, 2020, pp. 1-17.

Source primaire pour la **distinction entre affixes d'accord sujet** (obligatoires, sensibles à l'aspect pour les verbes d'état) et **clitiques objets** (optionnels, invariants à l'aspect, pouvant s'attacher à des têtes fonctionnelles autres que le verbe).

### 2.4 Ouhalla (2005) — Placement des clitiques
**Jamal Ouhalla**, cité dans Fahloune (2020). Les clitiques berbères obéissent à la **loi de la seconde position** : un clitique ne peut pas être le premier mot de la proposition. Il peut s'attacher au verbe (V-CL) ou à une catégorie fonctionnelle (F-CL V), notamment `ad`, `ur`, les complémentiseurs.

### 2.5 Référence de conjugaison en ligne
**amyag.com** — *Taseftit n wemyag di teqbaylit* (Conjugaison du verbe kabyle). Données de référence pour la validation des formes de surface (verbes `kcem`, `ger`, `lmed`, etc.).

---

## 3. Typologie morphémique : affixes vs clitiques

Cette distinction est fondamentale pour l'architecture du tokenizer.

| Propriété | Affixes d'accord sujet | Clitiques pronominaux objets |
|-----------|------------------------|------------------------------|
| **Obligation** | Obligatoires | Optionnels |
| **Position** | Fixe (préfixe ou suffixe au verbe) | Variable (post-verbe ou attaché à F) |
| **Variation aspectuelle** | Oui (verbes d'état au parfait) | Non (invariants) |
| **Attachement** | Verbe uniquement | Verbe, T, Neg, C, ou autre clitique |
| **Stacking** | Un seul par verbe | Jusqu'à 3+ en chaîne |

**Conséquence algorithmique** : les affixes sujet sont intégrés au **stem verbal** dans la première passe, tandis que les clitiques objets sont segmentés dans une **passe distincte** autour du noyau verbal.

---

## 4. Inventaire morphémique

### 4.1 Affixes d'accord sujet (intégrés au stem)

#### Préfixes personnels (affixes)
| Personne | Forme | Condition | Exemple |
|----------|-------|-----------|---------|
| 1sg | ∅ (zéro) | — | `kecmeɣ` |
| 2sg | `t-` | — | `tkecmeḍ` |
| 3sg masc | `y-` | Devant voyelle | `yura` |
| 3sg masc | `i-` | Devant consonne | `ikcem` |
| 3sg fém | `t-` | — | `tekcem` |
| 1pl | `n-` | — | `nekcem` |
| 2pl | `t-` | — | `tekcemem` |
| 3pl masc | ∅ | — | `kecmen` |
| 3pl fém | ∅ | — | `kecment` |

#### Suffixes personnels (affixes)
| Personne | Forme | Nature | Exemple |
|----------|-------|--------|---------|
| 1sg | `-eɣ` | Vocalique | `kecmeɣ` |
| 2sg | `-eḍ` / `-ḍ` | Vocalique | `tkecmeḍ` |
| 3sg masc | `-∅` | Zéro | `yekcem` |
| 3sg fém | `-∅` | Zéro | `tekcem` |
| 1pl | `-∅` | Zéro | `nekcem` |
| 2pl masc | `-em` | Vocalique | `tekcemem` |
| 2pl fém | `-emt` | Vocalique | `tekcememt` |
| 3pl masc | `-en` | Vocalique | `kecmen` |
| 3pl fém | `-ent` | Vocalique | `kecment` |

**Note** : Les suffixes `-eɣ` et `-eḍ` sont structurellement vocaliques et déclenchent l'effacement du schwa radical selon la position de celui-ci (Règle 14 du conjugueur).

### 4.2 Clitiques pronominaux objets (segmentés séparément)

D'après Bedar et al. (2021), les clitiques s'attachent au verbe ou à une tête fonctionnelle par l'intermédiaire d'un **tiret `-`** dans la segmentation. **En orthographe kabyle standard, les clitiques sont toujours séparés par un tiret `-`.**

#### Clitiques datifs (indirects, DAT)
| Personne | SG | PL |
|----------|-----|-----|
| 1 | `-iyi` | `-aɣ` |
| 2 M | `-ak` | `-awen` |
| 2 F | `-am` | `-acemt` |
| 3 M | `-as` | `-asen` |
| 3 F | `-as` | `-asent` |

#### Clitiques accusatifs (directs, ACC)
| Personne | SG | PL |
|----------|-----|-----|
| 1 | `-iyi` | `-aɣ` |
| 2 M | `-ak` | `-akwen` |
| 2 F | `-akem` | `-akwent` |
| 3 M | `-t` | `-ten` |
| 3 F | `-tt` | `-tent` |

**Note sur le 3e personne** : En kabyle standard, le clitique ACC 3sg masc est `-t` et le 3sg fém est `-tt` (distinction par tension). Certaines variétés utilisent `-ṭ` (affrication) pour le fém. Le tokenizer doit supporter les deux formes.

#### Clitiques directionnels (DIR)
| Direction | Forme | Sens |
|-----------|-------|------|
| Ventive | `-d` | Vers le locuteur |
| Itive | `-n` | Vers l'auditeur / loin du locuteur |

### 4.3 Préfixes aspectuo-modaux et dérivationnels

| Catégorie | Forme | Fonction | Type |
|-----------|-------|----------|------|
| Aoriste | `ad` | Marqueur d'aoriste | Particule |
| Négation | `ur` | Négation prétérit/aoriste | Particule |
| Négation aoriste | `ad ur` / `ur ad` | Négation aoriste | Particules combinées |
| Transitif | `s-` / `se-` / `sse-` | Causatif/factitif | Préfixe dérivationnel |
| Passif | `ttwa-` / `ttu-` / `mm-` / `mma-` | Passif | Préfixe dérivationnel |
| Réfléchi | `nn-` / `nne-` | Réfléchi | Préfixe dérivationnel |
| Réciproque | `my-` / `mm-` / `mi-` | Réciproque | Préfixe dérivationnel |
| Intensif | `tt-` | Aoriste intensif | Préfixe thématique |

---

## 5. Architecture du tokenizer : pipeline en couches

Le tokenizer fonctionne en **6 passes ordonnées**, de la périphérie vers le noyau.

```
Entrée : Forme verbale brute (ex. "ad-t-iyi-d-ini-ḍ")

Passe 1 : Segmentation phrastique (particules modales)
  → [ad] [t-iyi-d-ini-ḍ]

Passe 2 : Segmentation des clitiques pronominaux (de droite à gauche)
  → [ad] [t] [iyi] [d] [ini-ḍ]

Passe 3 : Identification du préfixe personnel
  → [ad] [t-] [iyi] [d] [ini-ḍ]  (t- = préfixe 2sg)

Passe 4 : Extraction du radical + suffixes personnels (lookup conjugueur)
  → [ad] [t-] [iyi] [d] [ini] [-ḍ]

Passe 5 : Détection des préfixes dérivationnels (si présents)
  → (non applicable ici)

Passe 6 : Post-traitement phonologique (schwas épenthétiques, glides)
  → Validation de la syllabation
```

### 5.1 Ordre canonique des morphèmes

Pour une forme verbale complète, l'ordre de surface est :

```
[Particule modale] - [Clitique OBJ] ... - [Préfixe dérivationnel] + [Préfixe personnel] + [Radical] + [Suffixe personnel]
```

**Règle de stacking clitique** (d'après Fahloune 2020 / Ouhalla 2005) :
```
DAT - ACC - DIR
```
Exemple : `y-fka-as-tt-id` (il a donné [à lui] [le] [vers ici]).

---

## 6. Règles de segmentation détaillées

### 6.1 Règle 1 : Segmentation des particules modales

Les particules `ad`, `ur`, `wa` (et leurs combinaisons `ad ur`, `ur ad`, `wa ad`) sont segmentées comme tokens indépendants en début de proposition.

| Forme | Segmentation |
|-------|--------------|
| `ad tkecmeḍ` | `[ad]` `[tkecmeḍ]` |
| `ur ikecm` | `[ur]` `[ikecm]` |
| `ad ur tkecmeḍ` | `[ad]` `[ur]` `[tkecmeḍ]` |

### 6.2 Règle 2 : Segmentation des clitiques pronominaux (algorithme glouton)

Les clitiques sont identifiés par **matching glouton de droite à gauche** sur la chaîne restante après retrait des particules. L'algorithme tente d'abord les clitiques les plus longs. **Le séparateur est le tiret `-`.**

**Pseudo-code** :
```python
def segment_clitics(token):
    clitics = []
    remaining = token
    while remaining.endswith(known_clitic):
        clitic = longest_matching_clitic(remaining)
        clitics.prepend(clitic)  # On remonte depuis la fin
        remaining = remaining[:-len(clitic)]
    return remaining, clitics
```

**Exemples** :
| Forme | Segmentation clitique | Reste (stem) |
|-------|----------------------|--------------|
| `yfka-yas` | `-yas` (DAT 3sg) | `yfka` |
| `yfka-yas-tt` | `-tt` (ACC 3sg fém) + `-yas` (DAT 3sg) | `yfka` |
| `yfka-yas-tt-id` | `-id` (DIR) + `-tt` (ACC) + `-yas` (DAT) | `yfka` |
| `ad-t-iyi-d-iniḍ` | `-d` (DIR) + `-iyi` (ACC/DAT 1sg) | `ad-t-iniḍ` |

**Attention** : Le clitique `-d` (directionnel) est homographe avec la lettre `d` du radical ou la particule `ad` tronquée. La disambiguation repose sur le contexte : `-d` en fin de chaîne clitique = directionnel ; `d-` en début de radical = lettre du radical.

### 6.3 Règle 3 : Identification du préfixe personnel

Après retrait des clitiques, le stem est analysé de gauche à droite.

| Préfixe détecté | Condition | Exemple |
|-----------------|-----------|---------|
| `t-` | Début de stem, non suivi de `t-` (sinon voir Règle 5) | `t-iniḍ` → `t-` + `iniḍ` |
| `y-` | Début de stem, suivant une voyelle | `y-ura` |
| `i-` | Début de stem, suivant une consonne | `i-kcem` |
| `n-` | Début de stem | `n-ekcem` |
| `∅` | Absence de préfixe consonantique | `kecmeɣ` |

**Ambiguïté majeure : `n-`** — 1pl vs négation intensif.
- Résolution : si la particule `ur` est présente dans la proposition et que le stem commence par `n-` + forme intensif (`tt-`), alors `n-` = négation intensif.
- Sinon, `n-` = 1pl.

### 6.4 Règle 4 : Extraction du radical et suffixes personnels (lookup conjugueur)

Le stem restant (après préfixe) est cherché dans le dictionnaire de conjugaisons (6 198 lemmes, 344K formes).

**Format d'entrée du dictionnaire** :
```yaml
lemme: "ini"
racine: "√n"
groupe: "G4"
stype: "G4-6"
themes:
  pret_aff: "ini"
  pret_neg: "ini"  # (supplétif qqaṛ pour intensif)
  aor_simple: "ini"
  aor_intensif: "ttini"  # (supplétif qqaṛ)
```

**Matching** : Le stem `ini` correspond au radical de l'aoriste simple. Le suffixe `-ḍ` est identifié comme 2sg.

**Règle du schwa épenthétique** (Règle 14) : Lors du lookup, le tokenizer doit reconnaître que `telmed` = `t-` + `e` (épenthétique) + `lmed`. Le schwa d'appui n'est pas un morphème lexical ; il est inséré par phonotactique. De même, `kecmeɣ` contient un schwa épenthétique entre `k` et `c` (cluster `kcm` non prononçable), bien que le radical soit `kcem`.

### 6.5 Règle 5 : Gestion de la géminée `tt-` et collision `tett-`

C'est la règle la plus critique pour la désambiguïsation.

| Contexte | Analyse | Exemple |
|----------|---------|---------|
| `tt-` en début de lemme (G1) | Géminée interne du radical | `ttcuddu` (aoriste intensif de `cudd`) |
| `tt-` après préfixe personnel `t-` | Collision : `t-` + `tt-` → `tett-` avec schwa | `tettekcameḍ` (2sg aoriste intensif de `ttekcam`) |
| `tt-` après `n-`, `y-`, `i-` | Préfixe intensif standard | `nettcuddu`, `yettcuddu` |
| `tt-` après `ur` | Négation intensif : `n-` + `tt-` | `ur nettcuddu` |

**Algorithme de désambiguïsation `tt-`** :
1. Si le lemme commence par `tt-` (G1) → `tt` = consonne initiale du radical.
2. Si le précédent morphème est `t-` (préfixe 2sg/3sgfém) → insérer `e` : `t-e-tt...` → surface `tett...`.
3. Sinon → `tt-` = préfixe intensif.

### 6.6 Règle 6 : Glides épenthétiques aux frontières

D'après Bedar et al. (2021), un glide [j] ou [w] peut apparaître à la jonction entre une racine vocalique et un clitique vocalique.

| Contexte | Glide | Exemple |
|----------|-------|---------|
| Racine en voyelle + clitique en `i-` | [j] | `ruḥ-iyi` → surface `ruḥji` (?) |
| Racine en voyelle + clitique en `a-` | [w] | *(à valider)* |

**Recommandation** : Le tokenizer doit d'abord segmenter les morphèmes, puis appliquer un module phonologique optionnel pour réinsérer les glides épenthétiques dans la forme de surface si nécessaire pour la TTS.

---

## 7. Format de sortie

Chaque token morphologique est représenté par un objet JSON :

```json
{
  "form": "ad-t-iyi-d-ini-ḍ",
  "tokens": [
    {"morpheme": "ad", "type": "particle", "subtype": "aorist", "position": "preverbal"},
    {"morpheme": "t-", "type": "affix", "subtype": "subject_agreement", "person": "2", "number": "sg", "gender": null},
    {"morpheme": "-iyi", "type": "clitic", "subtype": "dat_acc", "person": "1", "number": "sg"},
    {"morpheme": "-d", "type": "clitic", "subtype": "directional", "direction": "ventive"},
    {"morpheme": "ini", "type": "root", "lemma": "ini", "aspect": "aor_simple"},
    {"morpheme": "-ḍ", "type": "affix", "subtype": "subject_agreement", "person": "2", "number": "sg"}
  ],
  "schwa_epenthetic": [],
  "confidence": 1.0
}
```

Pour un modèle BPE hybride, la sortie peut être linéarisée :
```
ad | t- | -iyi | -d | ini | -ḍ
```
où `|` est le séparateur de sous-tokens.

---

## 8. Jeu de test obligatoire

Les formes ci-dessous sont validées contre les paradigmes amyag.com (kcem, ger, lmed) et la littérature académique (Fahloune 2020, Bouamara 2026).

| ID | Forme | Segmentation attendue | Source / Règle testée |
|----|-------|----------------------|----------------------|
| T01 | `kecmeɣ` | `∅` + `kcem` + `-eɣ` | amyag (aoriste 1sg) — Préfixe zéro 1sg, suffixe vocalique |
| T02 | `tkecmeḍ` | `t-` + `kcem` + `-eḍ` | amyag (aoriste 2sg) — Préfixe 2sg, schwa conservé (C2-C3) |
| T03 | `tegreḍ` | `t-` + `ger` + `-eḍ` | amyag (aoriste 2sg) — Schwa C1-C2 effacé, e d'appui inséré (Règle 14) |
| T04 | `telmed` | `t-` + `e` + `lmed` | amyag (prétérit 3sg fém) — Schwa épenthétique post-préfixe |
| T05 | `yura` | `y-` + `ura` | Bouamara G4-6 — Règle 3 (y- devant voyelle) |
| T06 | `ikcem` | `i-` + `kcem` | amyag (prétérit 3sg masc) — Règle 3 (i- devant consonne) |
| T07 | `tettekcameḍ` | `t-` + `e` + `ttekcam` + `-eḍ` | amyag (aoriste intensif 2sg) — Collision dentale Règle 15.2 |
| T08 | `ad-t-ini-ḍ` | `ad` + `t-` + `ini` + `-ḍ` | Bouamara G4-6 — Aoriste simple avec préfixe |
| T09 | `y-fka-yas-tt-id` | `y-` + `fka` + `-yas` + `-tt` + `-id` | Fahloune (2020, ex. 21) — Ordre DAT-ACC-DIR |
| T10 | `ur-yas-tt y-fka` | `ur` + `-yas` + `-tt` + `y-` + `fka` | Fahloune (2020, ex. 17c) — Clitiques attachés à Neg (F-CL V) |
| T11 | `sselmed` | `sse-` + `lmed` | Bouamara SG2-1 — Préfixe dérivationnel causatif |
| T12 | `ttwabder` | `ttwa-` + `bder` | Bouamara TG2-1 — Préfixe passif |
| T13 | `yettaru` | `y-` + `ttaru` | Bouamara G4-6 — Intensif (pas de collision avec y-) |
| T14 | `ittakk` | `i-` + `ttakk` | Bouamara supplétisme — `efk` → `ttakk` |
| T15 | `D lweḥda u neṣṣ` | `D` + `lweḥda` + `u` + `neṣṣ` | Mokraoui (2026) — Tokenization phrastique (heure) |

---

## 9. Limites connues et feuille de route

| ID | Limite | Statut |
|----|--------|--------|
| L1 | **Morphologie nominale** : préfixes `a-`, `ta-`, `i-`, `ti-`, état libre vs état d'annexion (`w-`, `y-`, `t-`) | Non modélisé — nécessite une spec dédiée |
| L2 | **Sandhi inter-mots** : `/n/ + /w/ → [bb]`, `/d/ + /t/ → [ts]` | Non modélisé (voir G2P spec §8.2) |
| L3 | **Clitiques attachés à F** : `ad-tt`, `ur-as` (formes proclitiques sur particules) | Partiellement couvert, nécessite parseur syntaxique |
| L4 | **Ambiguïté `-d`** : directionnel vs lettre `d` du radical | Résolu par position (fin de chaîne clitique) |
| L5 | **Paradigme ACC 3e personne** : variations dialectales (`-t` vs `-tt` vs `-ṭ`) | Paramètre dialectal requis |
| L6 | **Verbes d'état** : paradigme sujet spécifique au parfait (`-it` pluriel) | Couvert par lookup, mais pas par règle générale |
| L7 | **Emprunts** : formes non conjuguées par le paradigme natif | Nécessite lexique d'exceptions |
| L8 | **Schwa épenthétique phonotactique** : `kecmeɣ` (cluster `kcm` impossible) vs `bdeɣ` (cluster `bdr` possible) | Nécessite validateur phonotactique post-segmentation |

---

## 10. Implémentation recommandée

### 10.1 Architecture logicielle

```python
class KabyleMorphologicalTokenizer:
    def __init__(self, conjugation_dict, clitic_dict):
        self.conjugator = conjugation_dict      # 344K formes
        self.clitics = clitic_dict              # DAT, ACC, DIR
        self.particles = {"ad", "ur", "wa", "ad ur", "ur ad"}

    def tokenize(self, text):
        # Passe 1 : tokenization phrastique (Stanza)
        words = self.word_tokenize(text)

        # Passe 2 : analyse morphologique mot par mot
        for word in words:
            yield self.analyze_word(word)

    def analyze_word(self, word):
        # 1. Particules modales
        stem, particles = self.strip_particles(word)

        # 2. Clitiques (glouton droite-gauche)
        stem, clitics = self.segment_clitics(stem)

        # 3. Préfixe personnel
        prefix, stem = self.identify_subject_prefix(stem)

        # 4. Lookup radical + suffixe
        root, suffix = self.match_conjugation(stem)

        # 5. Post-traitement schwa
        schwas = self.identify_epenthetic_schwa(prefix, root)

        return MorphologicalToken(...)
```

### 10.2 Intégration avec BPE

Pour un tokenizer hybride :
1. **Entraînement** : appliquer le tokenizer morphologique sur le corpus d'entraînement pour générer un corpus pré-segmenté.
2. **BPE** : entraîner SentencePiece/BPE sur ce corpus pré-segmenté.
3. **Inférence** : segmenter morphologiquement en amont, puis appliquer le BPE sur chaque morphème individuellement.

**Résultat attendu** : le token `kecmeɣ` est segmenté en `kcem` + `-eɣ` avant BPE, ce qui empêche le modèle de créer un sous-mot `kec` arbitraire.

---

## 11. Conclusion

Cette spécification propose la première architecture de tokenization morphologique formalisée pour le kabyle. Elle distingue rigoureusement **affixes d'accord sujet** (intégrés au stem, variables selon l'aspect) et **clitiques pronominaux objets** (segmentés séparément, invariants, stackables en chaîne DAT-ACC-DIR). Elle s'appuie sur le paradigme clitique documenté par Bedar et al. (2021), la distinction affixe/clitique de Fahloune (2020), et les règles morphophonologiques du conjugueur algorithmique.

La principale avancée par rapport à un BPE standard est la **segmentation explicite des clitiques** et la **désambiguïsation des préfixes** (`t-` personnel vs `tt-` intensif, `n-` 1pl vs négation). Cette spécification constitue la brique manquante entre votre conjugueur (344K formes) et vos tokenizers statistiques existants.

---

## Références

1. **Bedar, Amazigh ; Quellec, Lucie ; Voeltzel, Laurence**, *Epenthetic glides in Taqbaylit*, Journal of African Languages and Literatures 2/2021, pp. 1-29.
2. **Fahloune, Khokha**, *On the status of subject and object markers in Kabyle: New evidence*, McGill Working Papers in Linguistics 26.1, 2020, pp. 1-17.
3. **Ouhalla, Jamal** (2005), cité dans Fahloune (2020). *Clitic placement in Berber*.
4. **Bouamara, K.** (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle*. HAL.
5. **Mokraoui, Athmane (boffire)** — Documents internes : Conjugueur, G2P, Expression du temps (2026).
6. **Mokraoui, Athmane (boffire)**, *Kabyle Stanza Tokenizer*, HuggingFace : `boffire/kabyle-stanza-tokenizer`.
7. **amyag.com** — *Taseftit n wemyag di teqbaylit*.

---

*Document rédigé dans le cadre du développement des ressources NLP pour la langue kabyle. Les zones nécessitant une validation native supplémentaire sont signalées comme telles.*
