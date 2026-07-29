# Spécifications du Conjugueur Automatique du Verbe Kabyle (Taqbaylit)

> **Référence** : Kamel Bouamara, *Modélisation des types morphologiques et de la conjugaison du verbe kabyle* — Volumes 1 (Formes de base) & 2 (Formes dérivées). HAL 2026.  
> **Objectif** : Fournir un blueprint algorithmique, exhaustif et sans ambiguïté, pour la génération automatique des paradigmes verbaux en kabyle.

---

## 1. Architecture Générale du Système Verbal

### 1.1 Principe Aspectuel
Le kabyle est fondamentalement **aspectuel**, non temporel. L'évaluation porte sur l'accomplissement de l'action au moment de l'énonciation.

### 1.2 Les Quatre Aspects
| Aspect | Abréviation | Thème distinct ? | Notes |
|--------|-------------|------------------|-------|
| Prétérit affirmatif | Prét. Aff. | Oui | Action accomplie |
| Prétérit négatif | Prét. Nég. | Oui | Action non accomplie (passé) |
| Aoriste simple | Aor. Simple | Oui | Action non encore accomplie |
| Aoriste intensif | Aor. Intensif | Oui | Action intensive, répétée ou distributive |

**Règle fondamentale** : L'aoriste intensif négatif **n'est pas** un aspect à part entière. Il se forme mécaniquement par préfixation de `n-` sur le thème de l'aoriste intensif affirmatif, **sans alternance thématique** (ex. `ttcuddu` → `nettcuddu`). Seuls le prétérit négatif et l'aoriste intensif affirmatif possèdent un thème distinct propre.

### 1.3 Forme de Base (Lemme)
La forme canonique d'identification est l'**impératif à la 2e personne du singulier**. Elle s'obtient en retirant le préfixe `t-` et le suffixe `-ḍ` de l'aoriste simple (2sg).

*Exemple* : `ad t-aru-ḍ` → lemme : `aru` (écrire).

### 1.4 Forme Canonique d'Observation des Thèmes
Pour visualiser les alternances thématiques entre les aspects, on utilise la **3e personne du singulier féminin** (préfixe `t-`, suffixe zéro).

*Exemple* : `t-ura` (Prét. Aff.) / `ur t-uri` (Prét. Nég.) / `ad t-aru` (Aor. Simple) / `te-ttaru` (Aor. Intensif).

---

## 2. Groupes Morphologiques de Base (G1–G4)

Le corpus de 1774 verbes de base se répartit en **4 groupes** selon le nombre de thèmes distincts mobilisés.

| Groupe | Nb de thèmes | Définition | Occurrences | Part du corpus |
|--------|--------------|------------|-------------|----------------|
| **G1** | 1 | Un seul thème pour les 3 premiers aspects ; pas d'aoriste intensif (lemme commence déjà par `tt-`). | 18 | 1,01 % |
| **G2** | 2 | Thème 1 pour Prét. Aff., Prét. Nég., Aor. Simple ; Thème 2 pour Aor. Intensif. | 726 | 40,92 % |
| **G3** | 3 | Sous-groupes 3.1 et 3.2 (voir ci-dessous). | 841 | 47,41 % |
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
Trois types couvrent à eux seuls **52,91 %** du corpus :
1. **G3.1-1** (`C1C2eC3`, ex. *lmed*, *bder*) : 480 verbes (27,07 %).
2. **G2-1** (`C1eC2C2eC3`, ex. *cerreg*) : 356 verbes (20,00 %).
3. **G4-1** (`C1C2u`, ex. *bḍu*, *cfu*) : 106 verbes (5,95 %).

---

## 3. Affixes Personnels

### 3.1 Invariance des Affixes (Règle 2)
Les affixes personnels sont **strictement invariants** quel que soient le verbe, le type morphologique ou l'aspect.

#### Préfixes personnels
| Personne | Préfixe | Condition / Remarque |
|----------|---------|---------------------|
| 1sg | — (zéro) | — |
| 2sg | `t-` | — |
| 3sg masc | `y-` (devant voyelle) / `i-` (devant consonne) | Voir Règle 3 |
| 3sg fém | `t-` | — |
| 1pl | `n-` | — |
| 2pl | `t-` | — |
| 3pl masc | — (zéro) | — |
| 3pl fém | — (zéro) | — |

#### Suffixes personnels
| Personne | Suffixe | Nature |
|----------|---------|--------|
| 1sg | `-eɣ` | Vocalique |
| 2sg | `-eḍ` / `-ḍ` | Vocalique (structurellement `-eḍ`) |
| 3sg masc | `-∅` (zéro) | Zéro |
| 3sg fém | `-∅` (zéro) | Zéro |
| 1pl | `-∅` (zéro) | Zéro |
| 2pl masc | `-em` | Vocalique |
| 2pl fém | `-emt` | Vocalique |
| 3pl masc | `-en` | Vocalique |
| 3pl fém | `-ent` | Vocalique |

**Règle 18 (dérivés)** : Le suffixe de la 2e personne du pluriel `-em` reste **constant** quel que soit l'aspect. Ex. `tsefrefdem` (Prét. Aff.) / `tesferfidem` (Aor. Intensif).

### 3.2 Règle de la 3e Personne du Singulier Masculin (Règle 3)
- `y-` devant une initiale **vocalique** : `y-ufeg`, `y-ecč-a`, `y-awi`, `y-aru`.
- `i-` devant une initiale **consonantique** : `i-rwi`, `i-kteb`, `i-rna`.

**Norme de standardisation** : cette règle est retenue comme norme orthographique et algorithmique.

---

## 4. Règles Morphophonologiques Fondamentales

### 4.1 Comportement du Schwa `e` (Règle 14)
Le schwa `e` est une voyelle d'appui **instable**. Son unique fonction est d'éviter le blocage phonétique (clusters de consonnes). Son comportement dépend de :
1. La nature du suffixe (zéro vs. vocalique).
2. La position du `e` dans le radical.

#### 4.1.1 Devant suffixe zéro (3sg masc, 3sg fém, 1pl)
Le `e` du radical **se maintient** à sa place.

**a. Sans préfixe (3sg masc)** : le radical s'articule seul.  
*Ex.* : `ilmed`, `ikcem`, `iɣiwel`, `iddem`.

**b. Avec préfixe consonantique (`t-` 3sg fém, `n-` 1pl)** :
- Si `e` est entre C1 et C2 : le préfixe s'attache directement. Le `e` médian suffit.  
  *Ex.* : `t-ger` → `tger` ; `n-ger` → `nger`.
- Si `e` est entre C2 et C3, ou si le radical commence par une géminée : un **e d'appui** s'insère obligatoirement entre le préfixe et le radical.  
  *Ex.* : `t-e-lmed` → `telmed` ; `n-e-lmed` → `nelmed` ; `t-e-kcem` → `tekcem` ; `t-e-ddem` → `teddem` ; `t-e-ɣiwel` → `tɣiwel`.

#### 4.1.2 Devant suffixe vocalique (`-eɣ`, `-eḍ`, `-em`, `-emt`, `-en`, `-ent`)
La voyelle du suffixe prend en charge la dernière consonne et libère la fin du mot. Le `e` du radical **s'efface**.

**a. Sans préfixe** :
- `e` entre C1 et C2 (consonnes simples libres ou biliteres) : il s'efface ; les consonnes restantes s'articulent directement avec le suffixe.  
  *Ex.* : `kcem` → `kecmeɣ`, `kecmen` ; `rfed` → `refdeɣ`, `rfeden` ; `els` → `lseɣ`, `lsen` ; `ger` → `greɣ`, `gren`.
- `e` entre C2 et C3 : il s'efface simplement **sans basculer**.  
  *Ex.* : `lmed` → `lemdeɣ`, `lemden`.
- Géminée initiale : il s'efface simplement.  
  *Ex.* : `ddem` → `ddmeɣ`, `ddmen`.
- Semi-voyelle : il s'efface simplement.  
  *Ex.* : `ɣiwel` → `ɣiwleɣ`, `ɣiwlen`.

**b. Avec préfixe consonantique** : un **e d'appui** s'insère **toujours** entre le préfixe et le radical, quelle que soit la structure du radical.  
*Ex.* : `t-e-lmed-eḍ` → `tlemdedḍ` ; `t-e-lmed-em` → `tlemdem` ; `t-e-ddem-eḍ` → `teddmedḍ` ; `t-e-ger-eḍ` → `tgredḍ`, `t-e-ger-em` → `tegrem`.

### 4.2 Nature Vocalique des Suffixes `-eɣ` et `-eḍ` (Règle 15.1)
Les suffixes 1sg (`-eɣ`) et 2sg (`-eḍ`) sont structurellement des **suffixes vocaliques**. C'est la voyelle initiale de ces suffixes qui provoque l'effacement du schwa final du radical et déclenche sa **bascule automatique** vers le début du mot (conformément à Règle 14, cas A).

*Exemple obligatoire* : `kcem` → `kecmeɣ` (et **non** `kcmeɣ`) ; `t-kcem-eḍ` → `tkecmedḍ` (et **non** `tkcmedḍ`).

### 4.3 Collision des Dentales `tett-` à l'Aoriste Intensif (Règle 15.2)
À l'aoriste intensif, de nombreux types présentent un thème commençant par la géminée `tt-` (ex. `ttcuddu`). Lorsqu'on adjoint le préfixe personnel `t-` (2sg, 3sg fém, 2pl), le système refuse la succession de trois consonnes identiques (`*tttc...`). On insère systématiquement un **schwa d'appui `e`** pour dissocier le préfixe du thème.

*Ex.* : `t-` + `ttcuddu` + `-ḍ` → `tettcuddudḍ` (tu noues).  
S'applique uniformément à : `tettcuddudḍ` (2sg), `tettcuddu` (3sg fém.), `tettcuddum` (2pl masc.), `tettcuddumt` (2pl fém.).

### 4.4 Loi de l'Environnement Post-Schwa et Dynamique des Biliteres (Règle 16)

#### 4.4.1 Contrainte de Fermeture Syllabique (Schwa non final)
Tout schwa `e` non final doit être immédiatement verrouillé en aval selon deux options exclusives :
- **Option A** : par une consonne tendue (`e + C1C1`) → `tessuden`, `tametṭṭut`.
- **Option B** : par deux consonnes distinctes (`e + C1C2`) → `els`.

**Exception** : le schwa structurel des verbes biliteres au prétérit (`azzel`), où la tension est inhérente au radical et située en amont.

#### 4.4.2 Typologie des Biliteres : `eC1C2` vs `C1eC2`
La position du schwa au lemme (aoriste) divise les verbes biliteres à voyelle zéro en deux catégories strictes, déterminant entièrement la formation de l'aoriste intensif.

**A. Structure avec schwa initial (`eC1C2`)**  
*Exemples* : `els`, `ens`, `ers`, `enz`.
- **Aoriste simple** :
  - Devant suffixe vocalique : la voyelle `e` initiale disparaît.  
    *Ex.* : `lseɣ` (ad lseɣ), `lsen` (ad lsen).
  - Devant préfixe consonantique : la voyelle `e` reste en place.  
    *Ex.* : `telsedḍ` (ad telsedḍ), `nels` (ad nels), `yels` (ad yels).
- **Aoriste intensif** : impossible de simplement doubler la dernière consonne. On ajoute le préfixe `tt-` au début + changement de voyelle en `u`.  
  Modèle : `tt-` + `C1` + `u` + `C2` (+ `u` optionnel).  
  *Ex.* : `els` → `ttlusu` ; `ens` → `ttnus` / `ttnusu` ; `ers` → `ttrus` / `ttrusu` ; `enz` → `ttnuz` / `ttnuzu`.

**B. Structure `C1eC2` (schwa médian)**  
*Exemples* : `ger`, `gen`, `ẓer`, `ḍer`, `ɣez`.
- **Aoriste intensif** : la structure se densifie par la fin en redoublant C2. Le schwa est conservé et bascule sous l'effet de l'Option A (`e + C1C1`).  
  *Ex.* : `ger` → `ggar` ; `gen` → `ggan` ; `ẓer` → `ẓerr` / `ẓẓar` ; `ḍer` → `ṭṭar` ; `ɣez` → `ɣɣaz` / `qqaz`.
- **Aoriste simple** :
  - Suffixe vocalique : effacement du schwa médian (`ger` → `gr-`).
    - Sans préfixe : articulation directe. *Ex.* : `ad greɣ` (1sg), `ad gren` (3pl masc).
    - Avec préfixe consonantique : insertion d'un schwa d'appui. *Ex.* : `ad tegredḍ` (2sg, et non `*tgredḍ`), `ad tegrem` (2pl masc), `ad negrem` (1pl).

---

## 5. Formes Dérivées

### 5.1 Classification Générale
Les formes dérivées obéissent aux mêmes principes de classification que les formes de base (groupes selon le nombre de thèmes). Elles se répartissent en :
- **Formes simples** : adjonction d'un seul préfixe dérivationnel.
- **Formes complexes** : combinaison de plusieurs préfixes.

Corpus : 1112 verbes dérivés, 103 types morphologiques.

### 5.2 Les Quatre Formes Simples

| Forme | Préfixe | Fonction | Occurrences | Nb types |
|-------|---------|----------|-------------|----------|
| **Transitif-Factitif** | `s-` | Faire faire / rendre transitif | 611 | 43 |
| **Passif** | `ttwa-`, `ttu-`, `mm-` | Subir l'action | 273 | 20 |
| **Réfléchi** | `nn-` | Action sur soi-même | 63 | 10 |
| **Réciproque** | `my-` / `mm-` | Action partagée (pluriel exclusif) | 125 | 20 |

#### 5.2.1 Transitif-Factitif (`s-`)
- **SG2** : 27 types, 458 occurrences.
- **SG3** : 16 types, 153 occurrences.
- Schémas dominants : `seC1C2eC3` (SG2-1, 144 occ.), `sC1eC2eC3eC4` (SG2-2, 85 occ.), `sC1uC2C2eC3` (SG2-3, 47 occ.), `seC1C2u` (SG3-1, 31 occ.), `sC1iC2C2eC3` (SG3-2, 31 occ.).

#### 5.2.2 Passif (`ttwa-` / `ttu-` / `mm-`)
- **TG1** (défectif, sans aoriste intensif) : 20 verbes, 5 types, préfixe `m-` (verbes d'état passivés : `mecṭuḥ`, `mucaɛ`, etc.).
- **TG2** : 252 verbes, 14 types, préfixes `ttwa-` / `ttu-` / `mm-`.
- **TG3** : 1 verbe (`mmag`), préfixe `mma-`.
- Schémas dominants : `ttwaC1C2eC3` (TG2-1, 112 occ.), `ttwaC1eC2C2eC3` (TG2-2, 100 occ.).

#### 5.2.3 Réfléchi (`nn-`)
- **NG2** : 55 verbes, 8 types.
- **NG3** : 8 verbes, 2 types.
- Schéma dominant : `nneC1C2aC3` (NG2-1, 32 occ.).

#### 5.2.4 Réciproque (`my-` / `mm-`)
- Conjugaison **exclusivement plurielle** par définition (l'action est partagée).
- **MG2** : 110 verbes, 14 types.
- **MG3** : 15 verbes, 6 types.
- Schémas dominants : `mC1aC2aC3` (MG2-1, 44 occ.), `mmiC1C2aC3` / `myeC1C2aC3` (MG2-2, 17 occ.), `mC1eC2aC3` (MG2-3, 14 occ.), `mC1aC2i` (MG3-1, 7 occ.).
- **Exception** : certains réciproques admettent une conjugaison au singulier avec complément explicite (`yid-s` / `yid-sen`). Ex. `msefhameɣ yid-s` (je me suis entendu avec lui).

### 5.3 Formes Complexes
Combinaisons de deux (voire trois) préfixes. Seul le **réciproque du transitif** (`m-` + `s-`) est représentatif dans le corpus.

- **MSG2** : 28 occurrences, 8 types. Schéma dominant : `mseC1C2aC3` (MSG2-1, 12 occ.).
- **MSG3** : 12 occurrences, 2 types. Schéma dominant : `mseC1C2u` (MSG3-2, 11 occ.).

*Exemples lexicalisés* : `msebɣu` (s'entendre, s'aimer), `mseddu` (vivre ensemble en couple), `msufaɣ` (se séparer à l'amiable), `msawad` (en venir aux mains / se retrouver devant la justice), `mseɣli` (faire match nul).

### 5.4 Règle 17 : Extension de la Règle 14 aux Formes Dérivées
La syllabation de droite à gauche et l'insertion du schwa pour briser tout groupe de trois consonnes ou plus en initiale s'appliquent également aux formes dérivées, préfixe personnel inclus.

Lorsque le thème dérivé présente une géminée initiale distinctive (ex. `ssew`, `sseḥlu`), celle-ci ne se réalise qu'après `e` ; en l'absence de `e`, elle se réduit à la consonne simple correspondante.

*Ex.* : `sferfed` + `-eɣ` → `sefrefdeɣ` ; `sseḥla` + `-eɣ` → `seḥlaɣ` ; `sseḥla` + `te-` → `tesseḥla`.

---

## 6. Verbes d'État

### 6.1 Définition et Comportement
Les verbes d'état ont une conjugaison particulière au prétérit (affirmatif et négatif). Aux deux aoristes, ils se conjuguent comme les verbes ordinaires.

**Particularité au prétérit** :
- Absence des préfixes personnels en début de forme.
- Conjugaison reposant sur **5 suffixes uniquement** :
  - 1sg : `-eɣ`
  - 2sg : `-eḍ`
  - 3sg masc : `-∅`
  - 3sg fém : `-et`
  - Pluriel (toutes personnes) : `-it` (forme syncrétique).

*Exemples* : `mellul` (être blanc), `zur` (être gros).

| Personne | `mellul` | `zur` |
|----------|----------|-------|
| 1sg | `melluɣ` | `zureɣ` |
| 2sg | `melluleḍ` | `zureḍ` |
| 3sg masc | `mellul` | `zur` |
| 3sg fém | `mellulet` | `zuret` |
| 1pl / 2pl / 3pl | `mellulit` | `zurit` |

Au prétérit négatif, on ajoute simplement la particule `ur` devant la forme affirmative.

### 6.2 Répartition
- Verbes d'état communs : 6 types, 42 occurrences.
- Formes passives en `m-` : 5 types, 20 occurrences (états résultant d'une action subie : `mectuḥ`, `mechur`, `mussnaw`, etc.).
- Total : 11 types, 62 occurrences (~3,5 % du corpus de base).

### 6.3 Types les plus représentatifs
- **G3.2-7a** (`iC1C2uC3`, ex. `ifsus`, être léger) : 23 occurrences.
- **TG1-1** (`C1eC2C3uC4`, ex. `mectuḥ`, être petit) : 15 occurrences.

---

## 7. Irrégularités et Supplétisme (Règle 4b)

### 7.1 Définition
Un verbe est irrégulier lorsqu'il possède l'un de ses thèmes complètement différent des autres, provenant d'une **autre racine** (phénomène de supplétisme).

### 7.2 Contraintes du Supplétisme
- Affecte **exclusivement l'aoriste intensif**.
- Le thème hétérogène est **totalement stable** et **généralisable à toutes les personnes** au sein de l'aspect.

### 7.3 Les Trois Verbes Supplétifs du Corpus
Sur 1774 verbes de base, ils représentent **moins de 0,17 %** :

| Verbe | Racine de base | Thème supplétif (Aor. Intensif) | Paradigme |
|-------|----------------|----------------------------------|-----------|
| `efk` (donner) | `√fk` | `ttakk` | `ittakk`, `tettakk`, `nettakk`, `ttakken`, etc. |
| `ini` (dire) | `√ni` | `qqaṛ` | `iqqaṛ`, `teqqaṛ`, `neqqaṛ`, `qqaṛen`, etc. |
| `ecč` (manger) | `√cč` | `ttett` (racine `√tt`) | `ittett`, `tettett`, `nettett`, `ttetten`, etc. |

**Principe typologique** : ces trois verbes figurent parmi les plus fréquemment employés en taqbaylit, conformément au principe universel selon lequel les verbes les plus usités sont les plus résistants à la régularisation.

---

## 8. Participes (Règle 13)

Le verbe kabyle dispose d'au moins **quatre formes participiales** (une par aspect affirmatif), invariables selon les personnes.

### 8.1 Formation
- **Aspects affirmatifs** (Prét. Aff., Aor. Simple, Aor. Intensif) : ajout du suffixe **`-n`** à la 3e personne du masculin singulier.
- **Aspects négatifs** (Prét. Nég., Aor. Intensif Nég.) : ajout du préfixe **`n-`** à cette même forme.

Dans les deux cas, `n` est un affixe intégré à la forme verbale, non un élément graphiquement détaché.

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

*Exemples* (G2-1 `cerreg`) :
- Simple : `cerreg` (2sg), `cergem` (2pl masc.), `cergemt` (2pl fém.).
- Intensif : `ttcerrig` (2sg), `ttcergem` (2pl masc.), `ttcergemt` (2pl fém.).

---

## 10. Spécifications Algorithmiques

### 10.1 Structure de Données Requise

#### Dictionnaire de verbes
Chaque entrée verbale doit contenir au minimum :
```yaml
lemme: "aru"               # Forme de base (impératif 2sg)
racine: "√rw"              # Racine abstraite (optionnel mais utile)
groupe: "G4"               # G1 | G2 | G3.1 | G3.2 | G4
stype: "G4-1"              # Type morphologique précis
themes:                    # Liste des thèmes par aspect
  pret_aff: "ura"
  pret_neg: "uri"
  aor_simple: "aru"
  aor_intensif: "ttaru"
est_verbe_etat: false      # true pour les verbes d'état
est_suppletif: false       # true pour efk, ini, ecč
est_derive: false          # true pour les formes dérivées
  prefixe_derivation: null # s- | ttwa- | ttu- | mm- | nn- | my- | mm- | m-s- ...
  groupe_derive: null      # SG2 | SG3 | TG1 | TG2 | TG3 | NG2 | NG3 | MG2 | MG3 | MSG2 | MSG3
```

#### Tables de types morphologiques
Une table référentielle par type (64 types de base + types dérivés) indiquant :
- Le schème abstrait (ex. `C1C2eC3`, `C1eC2C2eC3`).
- Les règles de dérivation thématique entre les aspects (patterns de voyelles/consonnes).
- Les contraintes phonotactiques spécifiques.

### 10.2 Pipeline de Génération d'une Forme Conjuguée

```
Entrée : Lemme + Type + Aspect + Personne + Genre + Nombre

Étape 1 : Récupération du thème correspondant (Aspect + Type)
Étape 2 : Application des règles de dérivation thématique si nécessaire
Étape 3 : Détermination des affixes (préfixe + suffixe) selon Personne/Genre/Nombre
Étape 4 : Assemblage préfixe + thème + suffixe
Étape 5 : Application des règles morphophonologiques
  5a. Règle 3 (préfixe 3sg masc : y- vs i-)
  5b. Règle 14 (comportement du schwa e)
  5c. Règle 15.1 (bascule du schwa devant -eɣ / -eḍ)
  5d. Règle 15.2 (collision dentales tett-)
  5e. Règle 16 (loi d'environnement post-schwa / biliteres)
  5f. Règle 17 (extension aux dérivés)
  5g. Règle 18 (invariance du suffixe -em)
Étape 6 : Post-traitement orthographique (normalisation des caractères kabyles)
Sortie : Forme conjuguée
```

### 10.3 Gestion des Caractères Kabyles
Le conjugueur doit utiliser l'alphabet latin standardisé pour le kabyle, avec les caractères spécifiques suivants :
- Consonnes : `ɛ` (latin small letter open e), `ɣ` (gamma latin), `č`, `ḍ`, `ǧ`, `ḥ`, `ṛ`, `ṣ`, `ṭ`, `ẓ`.
- Voyelles longues : `ā`, `ē`, `ī`, `ō`, `ū` (si nécessaire selon la modélisation).
- Géminées : représentées par la duplication de la consonne (`tt`, `kk`, `cc`, `bb`, etc.).

**Important** : le système doit rejeter les faux amis grecs (`ε` → `ɛ`, `γ`/`Γ` → `ɣ`/`Ɣ`, `Σ` → `Ɛ`) conformément aux normes Weblate/KabyleCharactersCheck.

### 10.4 Verbes Défectifs
Certains verbes ne se conjuguent pas à tous les aspects (notamment en G1 et certains dérivés passifs TG1). Le système doit retourner une absence de forme (`null` / `—`) pour les combinaisons non attestées.

### 10.5 Optimisations Recommandées
1. **Pré-calcul des types dominants** : G3.1-1, G2-1, G4-1 couvrent > 50 % du lexique.
2. **Génération par règles vs. lookup** : pour les verbes réguliers, privilégier la génération algorithmique à partir du lemme et du type. Pour les supplétifs et irréguliers, utiliser une table de lookup.
3. **Validation phonotactique** : implémenter un validateur de syllabation (contrainte de fermeture post-schwa) pour filtrer les formes impossibles.

---

## 11. Tableaux Récapitulatifs des Thèmes (Extraits Stratégiques)

### 11.1 Groupe 2 — Types majeurs
| Type | Schème | Exemple | Prét. Aff. | Prét. Nég. | Aor. Simple | Aor. Intensif |
|------|--------|---------|------------|------------|-------------|---------------|
| G2-1 | `C1eC2C2eC3` | cerreg | `cerreg` | `cerreg` | `cerreg` | `ttcerrig` |
| G2-2 | `C1uC2(C2)` | cudd | `cudd` | `cudd` | `cudd` | `ttcuddu` |
| G2-3a | `C1uC2C2eC3` | kuffer | `kuffer` | `kuffer` | `kuffer` | `ttkuffur` |
| G2-15 | `C1C1i` | zzi | `zzi` | `zzi` | `zzi` | `ttezzi` |

### 11.2 Groupe 3.1 — Type dominant
| Type | Schème | Exemple | Prét. Aff. | Prét. Nég. | Aor. Simple | Aor. Intensif |
|------|--------|---------|------------|------------|-------------|---------------|
| G3.1-1 | `C1C2eC3` | bder | `bder` | `bdir` | `bder` | `bedder` |

### 11.3 Groupe 4 — Types majeurs
| Type | Schème | Exemple | Prét. Aff. | Prét. Nég. | Aor. Simple | Aor. Intensif |
|------|--------|---------|------------|------------|-------------|---------------|
| G4-1 | `C1C2u` | bḍu | `bḍa` | `bḍi` | `bḍu` | `beṭṭu` |
| G4-3 | `C1eC2` | ɣer | `ɣra` | `ɣri` | `ɣer` | `ɣɣar` / `qqar` |
| G4-6 | `aC1u` | aru | `ura` | `uri` | `aru` | `ttaru` |
| G4-8 | `eC1C2` | els | `lsa` | `lsi` | `els` | `ttlusu` |

### 11.4 Dérivés — Transitif-Factitif (`s-`)
| Type | Schème | Exemple | Prét. Aff. | Prét. Nég. | Aor. Simple | Aor. Intensif |
|------|--------|---------|------------|------------|-------------|---------------|
| SG2-1 | `seC1C2eC3` | selmed | `sselmed` | `sselmed` | `sselmed` | `sselmad` |
| SG2-6 | `seC1C2eC3` | sefsi | `ssefsi` | `ssefsi` | `ssefsi` | `ssefsay` |
| SG3-1 | `seC1C2u` | seḥlu | `sseḥla` | `sseḥla` | `sseḥlu` | `sseḥluy` |

### 11.5 Dérivés — Passif
| Type | Schème | Exemple | Prét. Aff. | Prét. Nég. | Aor. Simple | Aor. Intensif |
|------|--------|---------|------------|------------|-------------|---------------|
| TG2-1 | `ttwaC1C2eC3` | ttwabder | `ttwabder` | `ttwabder` | `ttwabder` | `ttwabdar` |
| TG2-2 | `ttwaC1eC2C2eC3` | ttwacebbeh | `ttwacebbeh` | `ttwacebbeh` | `ttwacebbeh` | `ttwacebbah` |

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
| `ur` | Particule de négation du prétérit |
| `ad` | Particule d'aoriste |
| `>` | Dérivation / transformation |

---

## 13. Références Bibliographiques Implémentées

1. Bouamara, K. (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle. Première partie : Formes de base*. HAL : hal-05647626.
2. Bouamara, K. (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle. Deuxième volume : Formes dérivées*. HAL : hal-05655932.
3. Bouamara, K. (2024). *Amyag n teqbaylit. Asesmel n talɣiwin n yimyagen akked tseftay-nsen*. Éditions Boussekine.
4. Dallet, J.-M. (1982). *Dictionnaire kabyle-français*. SELAF.

---

*Document généré pour la conception d'un conjugueur algorithmique du verbe kabyle. Dernière mise à jour : 2026-07-28.*