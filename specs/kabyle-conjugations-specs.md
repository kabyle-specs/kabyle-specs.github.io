# Spécifications du Conjugueur Automatique du Verbe Kabyle (Taqbaylit)

| | |
|---|---|
| **Auteur** | Athmane Mokraoui (boffire) |
| **Rôle** | Locuteur natif kabyle, mainteneur des ressources NLP kabyles. Structuration phonologique, recherche documentaire, vérification native et audit empirique. |
| **Statut** | Draft v2.0 — Révision empirique |
| **Date** | 11 août 2026 |
| **Version précédente** | v1.0 (draft initial, 5 août 2026) |
| **Cible** | Linguistes computationnels, développeurs TTS/ASR, ingénieurs phonétiques, chercheurs en linguistique berbère. |

---

## Historique des versions

| Version | Date | Modifications |
|---------|------|---------------|
| v1.0 | 2026-08-05 | Draft initial. Structuration des 64 types morphologiques (Bouamara). |
| v2.0 | 2026-08-11 | Révision empirique. Audit contre le corpus JSON amyag.com (6 198 verbes). Réfutation de la gradation `z`→`ẓ` et `ɣ`→`q`. Ajout de l'apophonie du prétérit, de la règle Glue-on-Front, correction de la typo `mellul`. |

---

## Citation

> Mokraoui, A. (2026). *Spécifications du Conjugueur Automatique du Verbe Kabyle (Taqbaylit)*, v2.0. Draft. Basé sur Bouamara (HAL, 2026) et le corpus amyag.com (Naït-Zerrad).

---

# Spécifications du Conjugueur Automatique du Verbe Kabyle (Taqbaylit) — v2.0

> **Références théoriques** : Kamel Bouamara, *Modélisation des types morphologiques et de la conjugaison du verbe kabyle* — Volumes 1 (Formes de base) & 2 (Formes dérivées). HAL 2026.

> **Vérité terrain (Ground Truth)** : Corpus JSON de 6 198 verbes extraits de *amyag.com* (Dictionnaire de Kamal Naït-Zerrad).

> **Objectif** : Fournir un blueprint algorithmique, exhaustif et sans ambiguïté, pour la génération automatique des paradigmes verbaux en kabyle.

> **Posture épistémologique** : *Principe de traçabilité empirique*. Ce document signale explicitement les écarts entre les tableaux théoriques (affectés par des artefacts d'OCR dans les prépublications HAL) et les formes attestées dans le corpus JSON de référence. Toute forme non vérifiée est signalée comme telle plutôt que générée par extrapolation.

---

## Changelog v2.0 — Mises à jour empiriques critiques

| # | Sujet | Ancienne version (théorique) | Nouvelle version (empirique v2.0) |
|---|-------|------------------------------|-----------------------------------|
| 1 | Gradation `z` → `ẓ` | Hypothèse théorique | **RÉFUTÉ** (0/490 verbes audités) |
| 2 | Gradation `ɣ` → `q` | Hypothèse théorique | **RÉFUTÉ** comme règle générale (9/436) |
| 3 | Formes dérivées | Règle 17 (schwa à deux niveaux) | **Glue-on-Front** + Épenthèse Causative |
| 4 | Prétérit affirmatif | Pas d'apophonie documentée | **Apophonie `a` → `i`** devant 1sg/2sg |
| 5 | `mellul` 1sg | `melluɣ` (chute géminée) | **Typo corrigée** → `melluleɣ` |
| 6 | Aoriste intensif négatif | Préfixe `n-` | `ur ... ara` (conjugaison) ; `n-` = participe |
| 7 | Supplétif `efk` intensif | `ttakk` | **`ttak`** (corrigé via amyag.com) |
| 8 | Épenthèse `t-` + `bna` | `tbna` (bug) | **`tebna`** (corrigé via amyag.com) |

---

## 1. Architecture Générale du Système Verbal

### 1.1 Principe Aspectuel
Le kabyle est fondamentalement **aspectuel**, non temporel. L'évaluation porte sur l'accomplissement de l'action au moment de l'énonciation.

### 1.2 Les Quatre Aspects et la Négation

| Aspect | Abréviation | Thème distinct ? | Négation |
|--------|-------------|------------------|----------|
| Prétérit affirmatif | Prét. Aff. | Oui | `ur` + [Prét. Nég.] + `ara` |
| Prétérit négatif | Prét. Nég. | Oui | (Voir ci-dessus) |
| Aoriste simple | Aor. Simple | Oui | `ur` + [Aor. Simple] + `ara` |
| Aoriste intensif | Aor. Intensif | Oui | `ur` + [Aor. Intensif] + `ara` |

> **Mise à jour empirique v2.0 (Aoriste Intensif Négatif)** :
> La théorie (Règle 4b) suggérait une formation par préfixation de `n-` (ex : `nettcuddu`). L'audit empirique prouve que les **formes conjuguées** utilisent la particule `ur ... ara` comme les autres aspects. Le préfixe `n-` s'applique **uniquement au participe négatif**, et non au paradigme verbal fini.

### 1.3 Forme de Base (Lemme) et Forme Canonique
- **Lemme** : Impératif à la 2e personne du singulier (ex : `ad t-aru-ḍ` → `aru`).
- **Forme Canonique d'Observation** : 3e personne du singulier féminin (préfixe `t-`, suffixe zéro) pour visualiser les alternances thématiques.

### 1.4 Architecture Algorithmique SOTA (Lexicon + Generative Fallback)

Pour garantir l'absence d'hallucination, le moteur suit un pipeline strict de résolution :

```
1. Suppletive Shield     → ini, efk, ečč (hardcodés, protégés)
2. Volume 2 Wrapper      → Détection ss-, ttwa-, nn-, my- (Glue-on-Front)
3. Lexicon Lookup        → 6 198 verbes amyag.com (vérité terrain)
4. Generative Fallback   → Type Registry G1–G4 (règles vérifiées)
5. UNRESOLVED            → Forme ambiguë → retourne null / "—"
```

---

## 2. Groupes Morphologiques de Base (G1–G4)

Le corpus de 1 774 verbes de base se répartit en **4 groupes** et **64 types morphologiques**.

| Groupe | Nb de thèmes | Définition | Occurrences | Part |
|--------|--------------|------------|-------------|------|
| **G1** | 1 | Un seul thème ; pas d'aoriste intensif (lemme en `tt-`). | 18 | 1,01 % |
| **G2** | 2 | Thème 1 (Prét/Aor) ; Thème 2 (Intensif). | 726 | 40,92 % |
| **G3** | 3 | Sous-groupes 3.1 (Prét.Aff=Aor) et 3.2 (Prét.Aff=Prét.Nég). | 841 | 47,41 % |
| **G4** | 4 | Un thème distinct par aspect. | 189 | 10,66 % |

### 2.1 Sous-groupes de G3
- **G3.1** : Prét. Aff. et Aor. Simple partagent le Thème 1 ; Prét. Nég. = Thème 2 ; Aor. Intensif = Thème 3.
- **G3.2** : Prét. Aff. et Prét. Nég. partagent le Thème 1 ; Aor. Simple = Thème 2 ; Aor. Intensif = Thème 3.

### 2.2 Typologie : 64 Types Morphologiques

| Groupe | Nb de types | Désignation |
|--------|-------------|-------------|
| G1 | 11 | G1-1 à G1-11 |
| G2 | 21 | G2-1 à G2-22 |
| G3.1 | 2 | G3.1-1, G3.1-2 |
| G3.2 | 20 | G3.2-1 à G3.2-20 (incl. 7a/7b, 11a/11b, 12a/12b) |
| G4 | 10 | G4-1 à G4-10 |
| **Total** | **64** | |

### 2.3 Types Dominants (à optimiser en priorité)

| # | Type | Schème | Exemple | Occurrences | Part |
|---|------|--------|---------|-------------|------|
| 1 | G3.1-1 | `C1C2eC3` | *lmed*, *bder* | 480 | 27,07 % |
| 2 | G2-1 | `C1eC2C2eC3` | *cerreg* | 356 | 20,00 % |
| 3 | G4-1 | `C1C2u` | *bḍu*, *cfu* | 106 | 5,95 % |

---

## 3. Affixes Personnels (Règles 2, 3, 18)

### 3.1 Invariance des Affixes (Règle 2)
Les affixes personnels sont **strictement invariants** quel que soient le verbe, le type morphologique ou l'aspect.

#### Préfixes personnels

| Personne | Préfixe | Condition / Remarque |
|----------|---------|---------------------|
| 1sg | — (zéro) | — |
| 2sg | `t-` | — |
| 3sg masc | `y-` / `i-` | Voir Règle 3 |
| 3sg fém | `t-` | — |
| 1pl | `n-` | — |
| 2pl | `t-` | — |
| 3pl masc | — (zéro) | — |
| 3pl fém | — (zéro) | — |

#### Suffixes personnels

| Personne | Suffixe | Nature |
|----------|---------|--------|
| 1sg | `-eɣ` | Vocalique |
| 2sg | `-eḍ` | Vocalique |
| 3sg masc | `-∅` | Zéro |
| 3sg fém | `-∅` | Zéro |
| 1pl | `-∅` | Zéro |
| 2pl masc | `-em` | Vocalique |
| 2pl fém | `-emt` | Vocalique |
| 3pl masc | `-en` | Vocalique |
| 3pl fém | `-ent` | Vocalique |

### 3.2 Règle de la 3e Personne du Singulier Masculin (Règle 3)
- `y-` devant une initiale **vocalique** : `y-ufeg`, `y-aru`.
- `i-` devant une initiale **consonantique** : `i-rwi`, `i-kteb`.

**Norme de standardisation** : cette règle est retenue comme norme orthographique et algorithmique.

### 3.3 Invariance du suffixe `-em` (Règle 18)
Le suffixe de la 2e personne du pluriel `-em` reste constant quel que soit l'aspect. Ex. `tsefrefdem` (Prét. Aff.) / `tesferfidem` (Aor. Intensif).

---

## 4. Règles Morphophonologiques Fondamentales

### 4.1 Apophonie du Prétérit Affirmatif (Règle Empirique v2.0)

Pour une vaste classe de verbes d'action (notamment G4), le thème du Prétérit Affirmatif se termine par `a`. Cette voyelle mute obligatoirement en **`i`** devant les suffixes **1sg (`-eɣ`)** et **2sg (`-eḍ`)**.

| Verbe | Thème PA | 1sg | 2sg | 3sg_m |
|-------|----------|-----|-----|-------|
| `bnu` | `bna` | `bniɣ` | `tebniḍ` | `ibna` |
| `aru` | `ura` | `uriɣ` | `turiḍ` | `yura` |
| `els` | `lsa` | `lsiɣ` | `telsiḍ` | `ilsa` |
| `cfu` | `cfa` | `cfiɣ` | `tecfiḍ` | `icfa` |

> **Contrainte** : Cette règle **ne s'applique pas** aux verbes d'état (ex : `ggami` → `ggumaɣ`, et non `*ggumiɣ`).

### 4.2 Comportement du Schwa `e` (Règle 14) et Épenthèse

Le schwa `e` est une voyelle d'appui **instable**. Son comportement dépend de la nature du suffixe et de la position du `e` dans le radical.

#### 4.2.1 Devant suffixe zéro (3sg masc, 3sg fém, 1pl)
Le `e` du radical **se maintient** à sa place.

**a. Sans préfixe (3sg masc)** : le radical s'articule seul.
*Ex.* : `ilmed`, `ikcem`, `iɣiwel`, `iddem`.

**b. Avec préfixe consonantique (`t-` 3sg fém, `n-` 1pl)** :
- Si `e` est entre C1 et C2 : le préfixe s'attache directement.
  *Ex.* : `t-ger` → `tger` ; `n-ger` → `nger`.
- Si `e` est entre C2 et C3, ou si le radical commence par une géminée : un **e d'appui** s'insère.
  *Ex.* : `t-e-lmed` → `telmed` ; `n-e-lmed` → `nelmed`.

#### 4.2.2 Devant suffixe vocalique (`-eɣ`, `-eḍ`, `-em`, `-emt`, `-en`, `-ent`)
La voyelle du suffixe prend en charge la dernière consonne et libère la fin du mot. Le `e` du radical **s'efface**.

**a. Sans préfixe** :
- `e` entre C1 et C2 : il s'efface ; les consonnes restantes s'articulent directement avec le suffixe.
  *Ex.* : `kcem` → `kecmeɣ`, `kecmen`.
- `e` entre C2 et C3 : il s'efface simplement **sans basculer**.
  *Ex.* : `lmed` → `lemdeɣ`, `lemden`.
- Géminée initiale : il s'efface simplement.
  *Ex.* : `ddem` → `ddmeɣ`, `ddmen`.

**b. Avec préfixe consonantique** : un **e d'appui** s'insère **toujours** entre le préfixe et le radical.
*Ex.* : `t-e-lmed-eḍ` → `tlemdeḍ` ; `t-e-ddem-eḍ` → `teddmeḍ`.

#### 4.2.3 Épenthèse de cluster (Bug Fix v8.1)
Un préfixe consonantique (`t-`, `n-`) attaché à un thème sans schwa mais possédant **2+ consonnes initiales adjacentes** requiert un `e` d'appui pour briser le cluster illégal.

| Avant (bug) | Après (corrigé) | Verbe |
|-------------|-----------------|-------|
| `*tbna` | `tebna` | `bnu` 3sg_f |
| `*tbniḍ` | `tebniḍ` | `bnu` 2sg |
| `*tlsa` | `telsa` | `els` 3sg_f |
| `*tbnam` | `tebnam` | `bnu` 2pl_m |

#### 4.2.4 Exception de Sonorité (`rfed`)
Documentée dans [S2] Règle 14.2.a : pour les verbes où C1 est une sonorante (`r`, `l`) et C2 une obstruante, le schwa médian chute **sans se relocaliser** devant les suffixes pluriels.

- `rfed` + `en` → `rfeden` (et non `*refden`)
- `rfed` + `eɣ` → `refdeɣ` (relocalisation normale au singulier)

#### 4.2.5 Chute Multi-Schwa (Intensif)
Les thèmes intensifs possédant deux schwas structurels voient le schwa **final** chuter devant un suffixe vocalique.

- `keččem` + `eḍ` → `tkeččmeḍ`
- `bedder` + `eɣ` → `beddreɣ`

### 4.3 Collision des Dentales (Règle 15.2)
Préfixe `t-`/`n-` + thème intensif en `tt-` → insertion d'un schwa d'appui.
- `t-` + `ttcuddu` → `tettcuddu`

### 4.4 Loi des Biliteres (Règle 16)

#### A. Structure `eC1C2` (ex : `els`, `ens`, `ers`)
- Aoriste intensif : préfixe `tt-` + voyelle `u` → `ttlusu`.
- Devant suffixe vocalique : le `e` initial disparaît (`lseɣ`).
- Devant préfixe consonantique : le `e` reste en place (`telsedḍ`).

#### B. Structure `C1eC2` (ex : `ger`, `gen`, `ẓer`)
- Aoriste intensif : gémination de C1 sur C2 → `ggar`, `ggan`.
- Devant suffixe vocalique : effacement du schwa médian (`greɣ`).

### 4.5 Gradation Consonantique (MISE À JOUR EMPIRIQUE MAJEURE v2.0)

Les anciennes hypothèses de durcissement systématique des fricatives lors de la gémination (Aoriste Intensif) sont **réfutées** par l'audit du corpus JSON de 6 198 verbes.

| Consonne | Hypothèse théorique | Vérité empirique (Audit JSON) | Exemple |
|----------|---------------------|-------------------------------|---------|
| `c` | `c` → `čč` | **CONFIRMÉ** | `kcem` → `keččem` |
| `ḍ` | `ḍ` → `ṭṭ` | **CONFIRMÉ** | `bḍu` → `beṭṭu` |
| `z` | `z` → `ẓ` | **RÉFUTÉ** (0/490 verbes) | `brez` → `berrez` |
| `ɣ` | `ɣ` → `q` | **RÉFUTÉ** (9/436 verbes) | `ɣer` → `ɣɣar` |

> **Directive d'implémentation** : Les moteurs NLP ne doivent **jamais** appliquer `z`→`ẓ` ou `ɣ`→`q` comme règles morphologiques génératives. Seuls `c`→`čč` et `ḍ`→`ṭṭ` sont confirmés.

---

## 5. Formes Dérivées (Volume 2)

### 5.1 Classification Générale
Corpus de 1 112 verbes dérivés, 103 types morphologiques.

| Forme | Préfixe | Fonction | Occurrences | Nb types |
|-------|---------|----------|-------------|----------|
| Transitif-Factitif | `s-` / `ss-` | Faire faire / rendre transitif | 611 | 43 |
| Passif | `ttwa-`, `ttu-`, `mm-` | Subir l'action | 273 | 20 |
| Réfléchi | `nn-` | Action sur soi-même | 63 | 10 |
| Réciproque | `my-` / `mm-` | Action partagée (pluriel exclusif) | 125 | 20 |

### 5.2 La Règle Empirique du « Glue-on-Front » (v2.0)

L'audit de 2 530 verbes dérivés prouve qu'il n'est **pas nécessaire** de créer un moteur à double schwa (comme le suggérait la Règle 17 théorique). Les préfixes dérivationnels se situent **en dehors** du domaine phonologique du verbe de base.

**Algorithme :**
1. Le verbe de base se conjugue normalement (avec toutes ses règles internes).
2. Le préfixe est collé au début de la forme fléchie.

| Préfixe | Base | Forme fléchie | Résultat |
|---------|------|---------------|----------|
| `ttwa-` | `bedreɣ` | `ttwa-` + `bedreɣ` | `ttwabedreɣ` |
| `ss-` | `lemdeɣ` | `ss-` + `lemdeɣ` | `sslemdeɣ` |

### 5.3 Heuristique d'Épenthèse Causative

Lors du collage de `ss-`, `s-`, `nn-`, `my-`, si la première voyelle du verbe de base fléchi est à l'index ≥ 2 (créant un cluster de consonnes initial illégal), un `e` épenthétique est inséré après le préfixe.

| Préfixe + Base | Résultat | Explication |
|----------------|----------|-------------|
| `ss-` + `lmed` (3sg_m) | `isselmed` | Épenthèse ajoutée (`ss-e-lmed`) |
| `ss-` + `lemdeɣ` (1sg) | `sslemdeɣ` | Pas d'épenthèse (voyelle précoce) |

### 5.4 Formes Complexes
Combinaisons de deux (voire trois) préfixes. Seul le **réciproque du transitif** (`m-` + `s-`) est représentatif dans le corpus.

*Exemples lexicalisés* : `msebɣu` (s'entendre), `mseddu` (vivre ensemble en couple), `msufaɣ` (se séparer à l'amiable), `msawad` (en venir aux mains).

---

## 6. Verbes d'État (Imyagen n tyara)

### 6.1 Définition et Comportement
Les verbes d'état ont une conjugaison particulière au prétérit (affirmatif et négatif). Aux deux aoristes, ils se conjuguent comme les verbes ordinaires.

**Particularité au prétérit** :
- Absence des préfixes personnels en début de forme.
- Conjugaison reposant sur **5 suffixes uniquement** :

| Personne | Suffixe |
|----------|---------|
| 1sg | `-eɣ` |
| 2sg | `-eḍ` |
| 3sg masc | `-∅` |
| 3sg fém | `-et` |
| Pluriel (toutes) | `-it` (syncrétique) |

### 6.2 Correction de la Typo de Bouamara (v2.0)

Les tableaux théoriques indiquent que la 1sg de `mellul` est `melluɣ` (chute de la géminée finale devant `-eɣ`), tout en gardant la géminée à la 2sg (`melluleḍ`).

**Vérité empirique** : L'audit JSON de tous les verbes d'état se terminant par une géminée prouve que la consonne finale **ne chute jamais** :

| Verbe | 1sg (amyag.com) | 2sg |
|-------|-----------------|-----|
| `intill` | `ntelleɣ` | `ntelleḍ` |
| `bbuzṭeṭṭ` | `bezṭeṭṭeɣ` | `bezṭeṭṭeḍ` |

> **Verdict** : `melluɣ` est une **erreur de transcription** dans les sources. La concaténation régulière (`melluleɣ`) est la norme absolue.

### 6.3 Répartition
- Verbes d'état communs : 6 types, 42 occurrences.
- Formes passives en `m-` : 5 types, 20 occurrences.
- Total : 11 types, 62 occurrences (~3,5 % du corpus de base).

### 6.4 Types les plus représentatifs
- **G3.2-7a** (`iC1C2uC3`, ex. `ifsus`, être léger) : 23 occurrences.
- **TG1-1** (`C1eC2C3uC4`, ex. `mectuḥ`, être petit) : 15 occurrences.

---

## 7. Irrégularités et Supplétisme (Règle 4b)

### 7.1 Définition
Un verbe est irrégulier lorsqu'il possède l'un de ses thèmes complètement différent des autres, provenant d'une **autre racine** (phénomène de supplétisme).

### 7.2 Le « Suppletive Shield » (v2.0)
Trois verbes utilisent des racines entièrement différentes pour l'Aoriste Intensif. Ils doivent être **hardcodés et protégés** pour éviter que les règles génératives ou les erreurs d'OCR ne corrompent leurs thèmes.

| Verbe | Racine de base | Thème Intensif | Note |
|-------|----------------|----------------|------|
| `ini` (dire) | `√nn` (Prét) | `qqaṛ` | — |
| `efk` (donner) | `√fk` (Prét) | **`ttak`** | Corrigé v2.0 (et non `ttakk`) |
| `ečč` (manger) | `√čč` (Prét) | `ttett` | Racine `√tt` |

**Principe typologique** : ces trois verbes figurent parmi les plus fréquemment employés en taqbaylit, conformément au principe universel selon lequel les verbes les plus usités sont les plus résistants à la régularisation.

---

## 8. Participes (Règle 13)

Le verbe kabyle dispose d'au moins **quatre formes participiales** (une par aspect affirmatif), invariables selon les personnes.

### 8.1 Formation
- **Aspects affirmatifs** : ajout du suffixe **`-n`** à la 3e personne du masculin singulier.
- **Aspects négatifs** : ajout du préfixe **`n-`** à cette même forme.

### 8.2 Exemples

| Aspect | Forme | Participe |
|--------|-------|-----------|
| Prét. Aff. | `yura` | `yuran` |
| Aor. Simple | `ad yaru` | `yarun` |
| Aor. Intensif | `yettaru` | `yettarun` |
| Prét. Nég. | `ur yuri` | `ur nurin` |
| Aor. Intensif Nég. | `ur yettaru` | `ur nettarun` |

---

## 9. Impératifs

### 9.1 Impératif Simple
- 2sg : identique au **lemme** (forme de base).
- 2pl masc. : lemme + `-em`.
- 2pl fém. : lemme + `-emt`.

### 9.2 Impératif Intensif
- 2sg : thème de l'aoriste intensif (sans préfixe personnel).
- 2pl masc. : thème de l'aoriste intensif + `-em`.
- 2pl fém. : thème de l'aoriste intensif + `-emt`.

---

## 10. Spécifications Algorithmiques

### 10.1 Structure de Données Requise

```yaml
lexicon_entry:
  lemme: "bnu"
  themes:
    pret_aff: "bna"       # Déclenche l'apophonie (bniɣ)
    pret_neg: "bni"
    aor_simple: "bnu"
    aor_intensif: "bennu"  # Schwa en position 1 (auto-porteur)
  is_state: false
  is_derived: false
  confidence: "verified_lexicon"
```

### 10.2 Pipeline de Génération

```
Entrée : Lemme + Aspect + Personne

1. Suppletive Shield : Si lemme ∈ {ini, efk, ečč} → thèmes hardcodés.
2. Derived Wrapper   : Si préfixe ∈ {ss-, ttwa-, nn-, my-} →
     a. Isoler le verbe de base.
     b. Conjuguer le verbe de base (récursivité).
     c. Appliquer l'Heuristique d'Épenthèse Causative.
     d. Coller le préfixe (Glue-on-Front).
3. Lexicon Lookup    : Si lemme ∈ JSON (6198 verbes) → thèmes empiriques.
4. Generative Fallback : Matcher le lemme contre le Type Registry (G1 à G4).
5. Schwa & Phonology Engine :
     a. Apophonie (a → i) si Prét. Aff. + 1sg/2sg.
     b. Règles de relocation/épenthèse du schwa.
     c. Gradation (c→čč, ḍ→ṭṭ UNIQUEMENT).
6. Sortie : Forme conjuguée (ou "UNRESOLVED" si ambiguïté OOV).
```

### 10.3 Niveaux de Confiance (Confidence Grading)

Tout système implémentant ces spécifications doit taguer ses sorties avec l'un des 4 niveaux suivants :

| Niveau | Définition | Exemple |
|--------|------------|---------|
| `verified_lexicon` | Vérité terrain empirique (Corpus amyag.com). Écrase les règles génératives. | `kcem` → `ikeccem` |
| `verified` | Règles génératives adossées à de la prose propre, ou les 3 supplétifs. | `bnu` → `bniɣ` |
| `regular-rule` | Règles génératives adossées aux tableaux théoriques (risque d'OCR). | `cudd` → `ttcuddu` |
| `unresolved` | Formes OOV ambiguës. Le moteur **doit** retourner `null` ou `—`. | `ger` (sans lexique) |

### 10.4 Gestion des Caractères Kabyles
Le conjugueur doit utiliser l'alphabet latin standardisé pour le kabyle :
- **Consonnes** : `ɛ`, `ɣ`, `č`, `ḍ`, `ǧ`, `ḥ`, `ṛ`, `ṣ`, `ṭ`, `ẓ`.
- **Voyelles** : `a`, `i`, `u`, et le schwa épenthétique `e`.
- **Important** : `ɛ` (pharyngale voisée /ʕ/) est une **consonne**, pas une voyelle.
- **Rejet des faux amis grecs** : `ε` → `ɛ`, `γ`/`Γ` → `ɣ`/`Ɣ`, `Σ` → `Ɛ`.

### 10.5 Verbes Défectifs
Certains verbes ne se conjuguent pas à tous les aspects (notamment en G1 et certains dérivés passifs TG1). Le système doit retourner une absence de forme (`null` / `—`) pour les combinaisons non attestées.

### 10.6 Optimisations Recommandées
1. **Pré-calcul des types dominants** : G3.1-1, G2-1, G4-1 couvrent > 50 % du lexique.
2. **Génération par règles vs. lookup** : pour les verbes réguliers, privilégier la génération algorithmique. Pour les supplétifs et irréguliers, utiliser une table de lookup.
3. **Validation phonotactique** : implémenter un validateur de syllabation pour filtrer les formes impossibles.

---

## 11. Tableaux Récapitulatifs des Thèmes (Extraits Stratégiques)

### 11.1 Groupe 2 — Types majeurs

| Type | Schème | Exemple | Prét. Aff. | Prét. Nég. | Aor. Simple | Aor. Intensif |
|------|--------|---------|------------|------------|-------------|---------------|
| G2-1 | `C1eC2C2eC3` | cerreg | `cerreg` | `cerreg` | `cerreg` | `ttcerrig` |
| G2-2 | `C1uC2(C2)` | cudd | `cudd` | `cudd` | `cudd` | `ttcuddu` |
| G2-3a | `C1uC2C2eC3` | kuffer | `kuffer` | `kuffer` | `kuffer` | `ttkuffur` |

### 11.2 Groupe 3.1 — Type dominant

| Type | Schème | Exemple | Prét. Aff. | Prét. Nég. | Aor. Simple | Aor. Intensif |
|------|--------|---------|------------|------------|-------------|---------------|
| G3.1-1 | `C1C2eC3` | bder | `bder` | `bdir` | `bder` | `bedder` |

### 11.3 Groupe 4 — Types majeurs

| Type | Schème | Exemple | Prét. Aff. | Prét. Nég. | Aor. Simple | Aor. Intensif |
|------|--------|---------|------------|------------|-------------|---------------|
| G4-1 | `C1C2u` | bḍu | `bḍa` | `bḍi` | `bḍu` | `beṭṭu` |
| G4-3 | `C1eC2` | ɣer | `ɣra` | `ɣri` | `ɣer` | `ɣɣar` ⚠️ *(et non `qqar`)* |
| G4-6 | `aC1u` | aru | `ura` | `uri` | `aru` | `ttaru` |
| G4-8 | `eC1C2` | els | `lsa` | `lsi` | `els` | `ttlusu` |

### 11.4 Dérivés — Transitif-Factitif (`s-`)

| Type | Schème | Exemple | Prét. Aff. | Aor. Intensif |
|------|--------|---------|------------|---------------|
| SG2-1 | `seC1C2eC3` | selmed | `sselmed` | `sselmad` |
| SG3-1 | `seC1C2u` | seḥlu | `sseḥla` | `sseḥluy` |

### 11.5 Dérivés — Passif

| Type | Schème | Exemple | Prét. Aff. | Aor. Intensif |
|------|--------|---------|------------|---------------|
| TG2-1 | `ttwaC1C2eC3` | ttwabder | `ttwabder` | `ttwabdar` |
| TG2-2 | `ttwaC1eC2C2eC3` | ttwacebbeh | `ttwacebbeh` | `ttwacebbah` |

---

## 12. Glossaire et Notation

| Symbole | Signification |
|---------|---------------|
| `C1, C2, C3...` | Consonnes radicales (positions 1, 2, 3...) |
| `V` | Voyelle radicale ou thématique |
| `e` | Schwa (voyelle d'appui instable) |
| `∅` | Suffixe zéro (absence de suffixe) |
| `tt-` | Préfixe d'intensif / réciproque / passif |
| `y-` / `i-` | Allomorphes du préfixe 3sg masc |
| `ur ... ara` | Particules de négation |
| `ad` | Particule d'aoriste |
| `>` | Dérivation / transformation |

---

## 13. Références Bibliographiques Implémentées

1. Bouamara, K. (2026). *Analyse morphologique du verbe kabyle — Régularité, irrégularité et défectivité en taqbaylit*. HAL : hal-05533803.
2. Bouamara, K. (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle. Première partie : Formes de base*. HAL : hal-05647626.
3. Bouamara, K. (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle. Deuxième volume : Formes dérivées*. HAL : hal-05655932.
4. Naït-Zerrad, K. *Amyag.com* — Corpus JSON de 6 198 verbes (vérité terrain pour l'audit v2.0).
5. Naït-Zerrad, K. (1998). *Dictionnaire des verbes kabyles*. L'Harmattan.
6. Dallet, J.-M. (1982). *Dictionnaire kabyle-français*. SELAF.

---

*Document v2.0 rédigé pour la conception d'un conjugueur algorithmique du verbe kabyle. Dernière mise à jour : 2026-08-11.*
*Basé sur l'audit empirique du moteur v8.1 (6 198 verbes vérifiés).*
```
