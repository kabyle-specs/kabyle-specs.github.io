# Spécification d'Orthographe et de Normalisation des Caractères pour le Kabyle (Taqbaylit)

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; recherche documentaire et vérification Unicode.

**Date** : 30 juillet 2026

**Version** : 0.2-draft

**Statut** : En cours de validation — certains points sont marqués **[À VALIDER]** et nécessitent confirmation par le locuteur natif.

**Cible** : Développeurs NLP/TAL, ingénieurs système d'exploitation, mainteneurs de correcteurs orthographiques, traducteurs Weblate, concepteurs de polices de caractères, chercheurs en IA.

---

## Résumé

Le kabyle (Taqbaylit, ISO 639-3 `kab`) utilise l'alphabet latin berbère standardisé par l'INALCO en 1996, composé de 33 lettres dont 11 caractères spécifiques (č, ḍ, ɛ, ǧ, ɣ, ḥ, ṛ, ṣ, ṭ, ẓ) et leurs capitales. L'absence historique de normalisation informatique a conduit à une contamination massive des corpus par des faux amis typographiques — caractères grecs (ε, γ, Γ, Σ), cyrilliques (Ԑ, ԑ), turcs (ğ, ı, İ), et substituts visuels. Cette spécification définit l'inventaire canonique des caractères kabyles avec leurs points de code Unicode vérifiés, établit les règles de normalisation pour le traitement automatique, et propose des barrières qualité pour les corpus d'entraînement de l'IA.

**Mots-clés** : kabyle, taqbaylit, orthographe, normalisation, Unicode, INALCO, faux amis, contamination orthographique, corpus, NLP.

---

## 1. Introduction et périmètre

### 1.1 Le problème : contamination des corpus

L'absence de spécification orthographique informatique pour le kabyle a généré une crise d'encodage documentée dans le rapport CV26 (Mokraoui 2026) : sur 609 940 clips validés de Common Voice 26.0 Kabyle, **13 135 clips (2,15 %) sont contaminés** par des caractères non-kabyles. L'analyse du corpus Tatoeba kabyle confirme ce phénomène à l'échelle textuelle : **25 069 phrases sur 790 617 (3,17 %) contiennent au moins un faux ami**.

Ces contaminations rendent les corpus inutilisables pour l'entraînement de modèles NLP sans un pipeline de nettoyage préalable. Pire, les modèles de langage apprennent des équivalences erronées (ε = ɛ, γ = ɣ) qui se propagent ensuite dans les sorties de génération de texte.

### 1.2 Objectif de cette spec

Fournir une **spécification orthographique de référence** qui :
1. Définisse l'**inventaire canonique** des 33 caractères kabyles avec leurs points de code Unicode exacts.
2. Établisse les **règles de normalisation** pour le traitement automatique (NFC, case folding, rejet des faux amis, ponctuation, espacement).
3. Définisse des **barrières qualité** pour les corpus d'entraînement de l'IA.
4. Guide les **exigences de rendu** pour les polices de caractères et les systèmes d'exploitation.
5. S'intègre avec les outils de validation existants (Weblate KabyleCharactersCheck) et les pipelines NLP.

---

## 2. Inventaire canonique des caractères kabyles

### 2.1 Alphabet complet (33 lettres)

L'alphabet kabyle standard, tel qu'il est utilisé aujourd'hui dans les livres, la presse, les sites web et les logiciels, se compose de 33 lettres.

**22 lettres latines de base :**

| Majuscule | Unicode | Minuscule | Unicode | Nom | Phonème |
|-----------|---------|-----------|---------|-----|---------|
| A | U+0041 | a | U+0061 | A | /a/ |
| B | U+0042 | b | U+0062 | Bé | /b/ |
| C | U+0043 | c | U+0063 | Cé | /ʃ/ ~ /k/ ~ /s/ |
| D | U+0044 | d | U+0064 | Dé | /d/ |
| E | U+0045 | e | U+0065 | E | /ə/ |
| F | U+0046 | f | U+0066 | Éf | /f/ |
| G | U+0047 | g | U+0067 | Gé | /g/ |
| H | U+0048 | h | U+0068 | Hach | /h/ |
| I | U+0049 | i | U+0069 | I | /i/ |
| J | U+004A | j | U+006A | Ji | /ʒ/ |
| K | U+004B | k | U+006B | Ka | /k/ |
| L | U+004C | l | U+006C | El | /l/ |
| M | U+004D | m | U+006D | Em | /m/ |
| N | U+004E | n | U+006E | En | /n/ |
| Q | U+0051 | q | U+0071 | Qaf | /q/ |
| R | U+0052 | r | U+0072 | Er | /r/ |
| S | U+0053 | s | U+0073 | Ess | /s/ |
| T | U+0054 | t | U+0074 | Té | /t/ |
| U | U+0055 | u | U+0075 | U | /u/ |
| W | U+0057 | w | U+0077 | Waw | /w/ |
| X | U+0058 | x | U+0078 | Xa | /χ/ |
| Y | U+0059 | y | U+0079 | Yé | /j/ |
| Z | U+005A | z | U+007A | Zéd | /z/ |

**11 lettres spécifiques au kabyle :**

| Majuscule | Unicode | Minuscule | Unicode | Nom | Phonème |
|-----------|---------|-----------|---------|-----|---------|
| Č | U+010C | č | U+010D | Čé | /t͡ʃ/ |
| Ḍ | U+1E0C | ḍ | U+1E0D | Ḍé | /ðˤ/ |
| Ɛ | U+0190 | ɛ | U+025B | Ɛayn | /ʕ/ |
| Ǧ | U+01E6 | ǧ | U+01E7 | Ǧé | /d͡ʒ/ |
| Ɣ | U+0194 | ɣ | U+0263 | Ɣayn | /ɣ/ ~ /ʁ/ |
| Ḥ | U+1E24 | ḥ | U+1E25 | Ḥa | /ħ/ |
| Ṛ | U+1E5A | ṛ | U+1E5B | Ṛa | /rˤ/ |
| Ṣ | U+1E62 | ṣ | U+1E63 | Ṣad | /sˤ/ |
| Ṭ | U+1E6C | ṭ | U+1E6D | Ṭa | /tˤ/ |
| Ẓ | U+1E92 | ẓ | U+1E93 | Ẓa | /zˤ/ |

**Notes :**
- La lettre **E e** représente le schwa /ə/. C'est une lettre de base, pas un diacritique.
- Les lettres **P, O, V** n'existent pas dans l'alphabet kabyle natif. Elles n'apparaissent que dans les emprunts (noms propres, termes techniques).
- Le **tiret** `-` (U+002D) est omniprésent en kabyle (clitiques préverbaux, coordination, noms composés) et doit être traité comme un caractère obligatoire du système d'écriture.
- L'**apostrophe** `'` (U+0027) et ses variantes typographiques `‘` `’` `ʼ` **n'existent pas** en kabyle. Le coup de glotte n'est pas représenté dans l'orthographe.

### 2.2 Phonèmes distincts

Les paires suivantes sont des **phonèmes distincts** et ne doivent jamais être confondus :

- `e` (schwa fermé /ə/) vs `ɛ` (e ouvert /ɛ/) — utiliser `e` à la place de `ɛ` est une **erreur sémantique**, pas seulement typographique.
- `g` (vélaire /g/) vs `ǧ` (affriquée /dʒ/)
- `c` (/k/ ou /s/ selon le contexte) vs `č` (/tʃ/)
- `r` (alvéolaire) vs `ṛ` (emphatique/uvulaire)
- `s` vs `ṣ`, `t` vs `ṭ`, `d` vs `ḍ`, `z` vs `ẓ` — paires emphatiques

### 2.3 Caractères absents de l'orthographe kabyle standard

Les caractères suivants **ne font pas partie** de l'orthographe kabyle telle qu'elle est pratiquée aujourd'hui dans les livres, la presse et le numérique :

| Caractère | Unicode | Usage réel | Statut |
|-----------|---------|------------|--------|
| ʷ | U+02B7 | Notation linguistique uniquement | **Exclu** — jamais utilisé dans le texte courant |
| ř | U+0158 / U+0159 | Autres variétés berbères (tuareg) | **Exclu** — n'appartient pas au kabyle |
| ţ | U+0162 / U+0163 | Pré-Unicode, remplacé par ṭ | **Exclu** — forme obsolète |
| z̧ | — | Pré-Unicode, remplacé par ẓ | **Exclu** — forme obsolète |
| ç / Ç | U+00E7 / U+00C7 | Français | **Exclu** — jamais utilisé en kabyle |
| ' | U+0027 | Apostrophe ASCII | **Exclu** — jamais utilisé en kabyle ; supprimer ou réviser minutieusement |
| ' | U+2018 | Guillemet simple gauche | **Exclu** — jamais utilisé en kabyle ; supprimer ou réviser minutieusement |
| ' | U+2019 | Guillemet simple droit | **Exclu** — jamais utilisé en kabyle ; supprimer ou réviser minutieusement |
| ʼ | U+02BC | Lettre modificateur apostrophe | **Exclu** — jamais utilisé en kabyle ; supprimer ou réviser minutieusement |
| ñ / Ñ | U+00F1 / U+00D1 | Espagnol | **Exclu** — jamais utilisé en kabyle |
| ṇ / Ṇ | U+1E47 / U+1E46 | Non dans l'inventaire | **Exclu** |
| ħ / Ħ | U+0127 / U+0126 | Maltais | **Exclu** — jamais utilisé en kabyle |
| ø / Ø | U+00F8 / U+00D8 | Scandinave | **Exclu** |
| æ / Æ | U+00E6 / U+00C6 | Non dans l'inventaire | **Exclu** |
| œ / Œ | U+0153 / U+0152 | Français | **Exclu** |
| ß | U+00DF | Allemand | **Exclu** |
| þ / Þ | U+00FE / U+00DE | Islandais | **Exclu** |

---

## 3. Les faux amis : typologie et Unicode

Les faux amis sont des caractères d'autres scripts (grec, cyrillique, turc, polonais, espéranto) qui sont visuellement identiques ou proches des lettres kabyles. Ils doivent être **systématiquement rejetés** dans tout corpus, texte ou entrée utilisateur.

### 3.1 Les 6 familles principales

| # | Faux ami | Code | Cible correcte | Code | Source typique | Occ. Tatoeba | Occ. CV26 |
|---|----------|------|---------------|------|----------------|-------------|-----------|
| 1 | ε (epsilon grec minuscule) | U+03B5 | ɛ (latin small letter open e) | U+025B | Clavier grec, copier-coller | **25 395** | 10 806 clips |
| 2 | Σ (sigma grec majuscule) | U+03A3 | Ɛ (latin capital letter open e) | U+0190 | Clavier grec, AZERTY shift | **1 453** | 343 clips |
| 3 | γ (gamma grec minuscule) | U+03B3 | ɣ (latin small letter gamma) | U+0263 | Clavier grec, copier-coller | **222** | 1 284 clips |
| 4 | Γ (gamma grec majuscule) | U+0393 | Ɣ (latin capital letter gamma) | U+0194 | Clavier grec | **129** | 117 clips |
| 5 | Ԑ (epsilon cyrillique majuscule) | U+0510 | Ɛ (latin capital letter open e) | U+0190 | Clavier cyrillique | **578** | 343 clips |
| 6 | ԑ (epsilon cyrillique minuscule) | U+0511 | ɛ (latin small letter open e) | U+025B | Clavier cyrillique | **8** | 155 clips |

### 3.2 Faux amis secondaires

#### 3.2.1 `ǧ` / `Ǧ` (G avec caron)

| Faux ami | Codepoint | Source | Remplacement | Sévérité |
|----------|-----------|--------|--------------|----------|
| `ğ` | U+011F | Turc | `ǧ` | **Critique** |
| `ĝ` | U+011D | Espéranto | `ǧ` | **Critique** |
| `ģ` | U+0123 | Letton | `ǧ` | **Critique** |
| `ġ` | U+0121 | Maltais | `ǧ` | **Critique** |
| `ǵ` | U+01F5 | Polonais | `ǧ` | Modérée |
| `ǧ` | U+0067+U+030C | Décomposé | `ǧ` | Modérée |

#### 3.2.2 `č` / `Č` (C avec caron)

| Faux ami | Codepoint | Source | Remplacement | Sévérité |
|----------|-----------|--------|--------------|----------|
| `ć` | U+0107 | Polonais | `č` | **Critique** |
| `ĉ` | U+0109 | Espéranto | `č` | **Critique** |
| `ç` | U+00E7 | Français | **REJET** | **Critique** |
| `Ç` | U+00C7 | Français | **REJET** | **Critique** |
| `č` | U+0063+U+030C | Décomposé | `č` | Modérée |

#### 3.2.3 `ṭ` / `Ṭ` (T avec point souscrit)

| Faux ami | Codepoint | Source | Remplacement | Sévérité |
|----------|-----------|--------|--------------|----------|
| `ť` | U+0165 | Tchèque | `ṭ` | **Critique** |
| `ţ` | U+0163 | Roumain (legacy) | `ṭ` | **Critique** |
| `ț` | U+021B | Roumain (moderne) | `ṭ` | **Critique** |
| `ṫ` | U+1E6B | Point au-dessus | `ṭ` | **Critique** |
| `ṭ` | U+0074+U+0323 | Décomposé | `ṭ` | Modérée |

#### 3.2.4 `ḍ` / `Ḍ` (D avec point souscrit)

| Faux ami | Codepoint | Source | Remplacement | Sévérité |
|----------|-----------|--------|--------------|----------|
| `ď` | U+010F | Tchèque | `ḍ` | **Critique** |
| `đ` | U+0111 | Croate | `ḍ` | **Critique** |
| `ð` | U+00F0 | Islandais | `ḍ` | **Critique** |
| `ḑ` | U+1E11 | Variante cédille | `ḍ` | **Critique** |
| `ḍ` | U+0064+U+0323 | Décomposé | `ḍ` | Modérée |

#### 3.2.5 `ṣ` / `Ṣ` (S avec point souscrit)

| Faux ami | Codepoint | Source | Remplacement | Sévérité |
|----------|-----------|--------|--------------|----------|
| `š` | U+0161 | Tchèque/Baltique | `ṣ` | **Critique** |
| `ś` | U+015B | Polonais | `ṣ` | **Critique** |
| `ş` | U+015F | Turc | `ṣ` | **Critique** |
| `ș` | U+0219 | Roumain | `ṣ` | **Critique** |
| `ṡ` | U+1E61 | Point au-dessus | `ṣ` | **Critique** |
| `ṣ` | U+0073+U+0323 | Décomposé | `ṣ` | Modérée |

#### 3.2.6 `ẓ` / `Ẓ` (Z avec point souscrit)

| Faux ami | Codepoint | Source | Remplacement | Sévérité |
|----------|-----------|--------|--------------|----------|
| `ž` | U+017E | Tchèque/Baltique | `ẓ` | **Critique** |
| `ź` | U+017A | Polonais | `ẓ` | **Critique** |
| `ż` | U+017C | Polonais | `ẓ` | **Critique** |
| `ƶ` | U+01B6 | Z barré | `ẓ` | Modérée |
| `ẓ` | U+007A+U+0323 | Décomposé | `ẓ` | Modérée |

#### 3.2.7 `ṛ` / `Ṛ` (R avec point souscrit)

| Faux ami | Codepoint | Source | Remplacement | Sévérité |
|----------|-----------|--------|--------------|----------|
| `ř` | U+0159 | Tchèque | `ṛ` | **Critique** |
| `ŕ` | U+0155 | Polonais | `ṛ` | **Critique** |
| `ṙ` | U+1E59 | Point au-dessus | `ṛ` | **Critique** |
| `ŗ` | U+0157 | Letton | `ṛ` | **Critique** |
| `ṛ` | U+0072+U+0323 | Décomposé | `ṛ` | Modérée |

#### 3.2.8 `i` / `I` (Faux amis turcs)

| Faux ami | Codepoint | Source | Remplacement | Sévérité |
|----------|-----------|--------|--------------|----------|
| `ı` | U+0131 | Turc | `i` | **Critique** |
| `İ` | U+0130 | Turc | `I` | **Critique** |

### 3.3 Mécanisme de contamination

La contamination se produit par trois canaux :
1. **Saisie directe** : l'utilisateur dispose d'un clavier grec/cyrillique ou d'anciennes dispositions de claviers kabyles non standardisées et saisit visuellement (ε au lieu de ɛ).
2. **Copier-coller** : texte importé depuis des sources non standardisées (PDF scannés, sites web anciens).
3. **Conversion automatique** : OCR ou transcription automatique produisant des substituts visuels.

---

## 4. Normalisation des diacritiques français

Les emprunts français et la typographie adjacente introduisent souvent des diacritiques absents de l'inventaire kabyle. Dans le texte formel, ceux-ci doivent être normalisés vers leur lettre de base :

| Source | Codepoint(s) | Canonique |
|--------|--------------|-----------|
| `á` `à` `â` `ä` `ã` `å` | divers | `a` |
| `é` `è` `ê` `ë` | divers | `e` |
| `í` `ì` `î` `ï` | divers | `i` |
| `ó` `ò` `ô` `ö` `õ` | divers | `o` |
| `ú` `ù` `û` `ü` | divers | `u` |
| `ý` `ÿ` | divers | `y` |
| `ñ` | U+00F1 | `n` |
| `ç` | U+00E7 | **REJET** (pas `c`) |

---

## 5. Digraphes hérités et substituts ASCII

Les digraphes suivants apparaissent dans la saisie informelle kabyle lorsque le caractère Unicode correct n'est pas disponible. **Ils ne doivent PAS être substitués automatiquement.** Ils doivent être signalés pour révision manuelle, car ils entrent en collision avec des noms propres français/anglais/arabe et des emprunts.

| Digraphe | Cible probable | Collision exemple | Action |
|----------|----------------|-------------------|--------|
| `ch` | `č` | Français *chose* | **Signaler pour révision** |
| `dj` | `ǧ` | Arabe *Djamel* | **Signaler pour révision** |
| `gh` | `ɣ` | Anglais *ghoul* | **Signaler pour révision** |
| `th` | `ṭ` | Anglais *theater* | **Signaler pour révision** |
| `dh` | `ḍ` | Anglais *dharma* | **Signaler pour révision** |
| `sh` | `ṣ` | Anglais *shop* | **Signaler pour révision** |
| `zh` | `ẓ` | Noms étrangers | **Signaler pour révision** |
| `rh` | `ṛ` | Anglais *rhetoric* | **Signaler pour révision** |
| `3` | `ɛ` | Leet/Arabizi | **Signaler pour révision** |

Un outil conforme PEUT proposer un mode de substitution à haute confiance **uniquement** lorsque le contexte ambiant est structurellement kabyle (par exemple, dans un paradigme de conjugaison connu).

---

## 6. Ponctuation et espacement

### 6.1 Espaces
- **Canonique :** ` ` (U+0020) espace ASCII simple uniquement.
- **Interdits :** U+00A0 (NBSP), U+202F (NNBSP), U+2007 (figure space), U+2009 (thin space), et tout autre espace typographique.
- **Règle :** Réduire les espaces multiples (`  `) en un seul espace.

### 6.2 Signes de ponctuation

| Canonique | Codepoint | Alternatives interdites |
|-----------|-----------|------------------------|
| `,` | U+002C | |
| `.` | U+002E | |
| `;` | U+003B | |
| `:` | U+003A | |
| `?` | U+003F | |
| `!` | U+0021 | |
| `-` | U+002D | `‐` U+2010, `‑` U+2011, `–` U+2013, `—` U+2014 |
| `«` | U+00AB | |
| `»` | U+00BB | |
| `"` | U+0022 | `“` U+201C, `”` U+201D |

**Règles d'espacement (style anglais) :**
- Pas d'espace avant `?`, `!`, `:`, `;`, `.`, `,`.
- Un espace après `?`, `!`, `:`, `;`, `.`, `,`.
- Pas d'espace à l'intérieur des guillemets : `«texte»` ou `"texte"` (pas `« texte »`).

### 6.3 Usage du tiret

Le tiret `-` (U+002D) est utilisé pour :
- Les limites de clitiques (ex. `ad-id`, `ur-igi`)
- Les mots composés
- La césure en fin de ligne

### 6.4 Interdiction de l'apostrophe

Le kabyle **n'utilise pas** l'apostrophe `'` (U+0027) ni ses variantes typographiques `‘` `’` `ʼ` pour aucun phonème, clitique ou signe de ponctuation. Le coup de glotte n'est pas représenté dans l'orthographe. Toute apostrophe rencontrée dans du texte kabyle est une contamination et doit être supprimée ou revue minutieusement.

---

## 7. Nombres

Le kabyle utilise les chiffres arabes `0 1 2 3 4 5 6 7 8 9` (U+0030–U+0039). Aucune autre forme numérale n'est canonique.

---

## 8. Règles de normalisation

### 8.1 Formes canoniques

Tout texte kabyle destiné au stockage, à l'échange ou à l'entraînement de modèles doit respecter les règles suivantes :

| Règle | Description | Justification |
|-------|-------------|---------------|
| **R1** | **Formes précomposées uniquement.** Utiliser `č` U+010D, jamais `c` + caron combiné. | Toutes les lettres spécifiques kabyles possèdent un point de code Unicode dédié. |
| **R2** | **Case mapping explicite.** `Ɛ` U+0190 ↔ `ɛ` U+025B et `Ɣ` U+0194 ↔ `ɣ` U+0263 ne sont pas des paires standard Unicode. | Les implémentations doivent gérer manuellement ces mappings. |
| **R3** | **NFC obligatoire.** Tout texte doit être normalisé en NFC avant stockage. | Les caractères kabyles sont déjà précomposés ; NFC ne les modifie pas mais uniformise le reste. |
| **R4** | **UTF-8 obligatoire.** Interdiction des encodages Latin-1, Windows-1252, ou tout autre encodage ne couvrant pas l'Extended Latin Additional. | Les lettres à point souscrit (ḍ, ḥ, ṛ, ṣ, ṭ, ẓ) se trouvent dans le bloc Latin Extended Additional (U+1E00–U+1EFF). |
| **R5** | **Rejet systématique des faux amis.** Tout texte contenant ε, Σ, γ, Γ, Ԑ, ԑ, ğ, ĝ, ć, ĉ, š, ž, ř, ı, İ, etc. doit être rejeté ou corrigé avant intégration à un corpus. | Ces caractères appartiennent à d'autres scripts et ne sont jamais valides en kabyle. |
| **R6** | **Espace simple uniquement.** Pas d'espace insécable, pas d'espace fine insécable. | Le kabyle utilise l'espace simple ASCII comme l'anglais. |
| **R7** | **Pas d'apostrophe.** Toute apostrophe (ASCII ou typographique) est une contamination. | Le kabyle n'a pas de grapheme pour le coup de glotte ; supprimer ou réviser minutieusement. |

### 8.2 Pipeline de normalisation

Un préprocesseur conforme doit appliquer les étapes suivantes dans l'ordre :

#### Étape 1 : Normalisation Unicode NFC
Appliquer Unicode NFC pour que tous les caractères précomposés (ex. `ṭ` U+1E6D) soient en forme canonique composée. Les séquences décomposées (ex. `ṭ` U+0074+U+0323) doivent être normalisées vers leur équivalent précomposé.

#### Étape 2 : Substitution des faux amis
Appliquer les tables de substitution des §3.1 et §3.2 de manière déterministe. Cette étape est **destructive** ; la chaîne originale doit être conservée dans un champ séparé à des fins d'audit.

#### Étape 3 : Suppression des diacritiques français
Appliquer la table de normalisation du §4.

#### Étape 4 : Signalement des digraphes
Scanner les séquences du §5. Enregistrer chaque occurrence pour révision manuelle. Ne **pas** substituer automatiquement.

#### Étape 5 : Validation du script
Après toutes les substitutions, chaque point de code du texte doit appartenir à l'un des sous-ensembles suivants :

| Bloc Unicode | Points de code autorisés |
|--------------|-------------------------|
| Basic Latin | `U+0000–U+007F` (exclut `'` U+0027) |
| Latin Extended-A | `U+010C–U+010D` (`Č č`) uniquement |
| Latin Extended-B | `U+0190` (`Ɛ`), `U+025B` (`ɛ`), `U+0194` (`Ɣ`), `U+0263` (`ɣ`), `U+01E6–U+01E7` (`Ǧ ǧ`) |
| Latin Extended Additional | `U+1E0C–U+1E0D` (`Ḍ ḍ`), `U+1E24–U+1E25` (`Ḥ ḥ`), `U+1E5A–U+1E5B` (`Ṛ ṛ`), `U+1E62–U+1E63` (`Ṣ ṣ`), `U+1E6C–U+1E6D` (`Ṭ ṭ`), `U+1E92–U+1E93` (`Ẓ ẓ`) |

Tout caractère hors de ces plages doit être enregistré et l'échantillon mis en quarantaine.

#### Étape 6 : Normalisation des espaces
Réduire tous les espaces blancs à `U+0020`. Réduire les espaces multiples à un seul.

#### Étape 7 : Vérification de la casse
Signaler les phrases avec un mélange anormal de casse (ex. `Ɛ` en position médiane sans justification de nom propre).

---

## 9. Barrières qualité pour les corpus et l'IA

### 9.1 Pipeline de validation obligatoire

Tout corpus kabyle destiné à l'entraînement de modèles de langage doit passer par les barrières suivantes :

| Étape | Contrôle | Seuil / Action |
|-------|----------|----------------|
| **B1** | Whitelist caractères | Seuls les 33 lettres + tiret + ponctuation standard + chiffres sont autorisés. |
| **B2** | Détection des faux amis | Rejet immédiat si ε, Σ, γ, Γ, Ԑ, ԑ, ğ, ĝ, ć, ĉ, š, ž, ř, ı, İ, etc. détectés. |
| **B3** | Détection des formes obsolètes | Flag si `ţ`, `z̧`, `ř` détectés ; correction auto vers `ṭ`, `ẓ`. |
| **B4** | Langue identifiée | GlotLID `kab_Latn` ≥ 0.95 (standard établi par Mokraoui 2026). |
| **B5** | Longueur minimale | ≥ 3 caractères après suppression des espaces. |
| **B6** | Normalisation NFC | Vérification que le texte est bien en NFC. |
| **B7** | Encodage UTF-8 | Vérification que le fichier est encodé en UTF-8 sans BOM. |
| **B8** | Espace simple | Vérification qu'aucun espace insécable n'est présent. |
| **B9** | Pas d'apostrophe | Vérification qu'aucune apostrophe (ASCII ou typographique) n'est présente ; supprimer ou réviser minutieusement. |

### 9.2 Profils de sévérité par corpus

| Profil | Cas d'usage | Substitution auto | Digraphes | Normalisation espaces |
|--------|-------------|-------------------|-----------|----------------------|
| `strict` | Prompts TTS, entraînement ASR | Requise | Signaler + révision manuelle | Requise |
| `standard` | Données parallèles MT, treebanks UD | Requise | Signaler + révision manuelle | Requise |
| `lenient` | Crawling web, réseaux sociaux | Requise | Signaler uniquement | Requise |

### 9.3 Métriques de qualité attendues

Sur la base du rapport CV26 et de l'analyse Tatoeba :

| Métrique | Valeur cible | Tolérance maximale |
|----------|--------------|--------------------|
| Taux de faux amis | 0 % | &lt; 0,01 % |
| Taux de formes obsolètes | 0 % | &lt; 0,05 % |
| Taux de caractères non-kabyles | &lt; 0,1 % | &lt; 0,5 % |
| Taux de textes non-kabyle (GlotLID) | 0 % | &lt; 1 % |

---

## 10. Exigences de rendu des polices de caractères

### 10.1 Support Unicode minimal

Toute police de caractères destinée au kabyle doit supporter au minimum les blocs suivants :
- **Basic Latin** (U+0000–U+007F) : lettres de base, chiffres, ponctuation, tiret.
- **Latin Extended-A** (U+0100–U+017F) : č, Č.
- **Latin Extended-B** (U+0180–U+024F) : ɛ, Ɛ, ɣ, Ɣ, ǧ, Ǧ.
- **Latin Extended Additional** (U+1E00–U+1EFF) : ḍ, Ḍ, ḥ, Ḥ, ṛ, Ṛ, ṣ, Ṣ, ṭ, Ṭ, ẓ, Ẓ.

### 10.2 Exigences de lisibilité

| Exigence | Description | Critère |
|----------|-------------|---------|
| **E1** | Le point souscrit doit être clairement visible et distinct du corps de la lettre. | À 11 px et au-dessus, `ḍ` et `d` doivent rester distinguables. |
| **E2** | Le caron sur `č` et `ǧ` ne doit pas toucher la hampe de la lettre. | Pas de fusion visuelle à taille réduite. |
| **E3** | L'open E `ɛ` et `Ɛ` ne doivent pas être confondus avec le epsilon grec `ε` ou le E latin `e`. | Forme ouverte en C, pas de barre médiane. |
| **E4** | Le gamma latin `ɣ` et `Ɣ` ne doivent pas être confondus avec le gamma grec `γ` ou le g latin `g`. | Boucle fermée en bas, pas de crochet. |
| **E5** | Le `ḥ` et le `h` doivent rester distinguables à taille réduite. | Le point souscrit du `ḥ` doit être visible. |

---

## 11. Intégration avec les outils existants

### 11.1 Weblate

Le **KabyleCharactersCheck** (v5.12+) implémente déjà le rejet des faux amis grecs et cyrilliques. Cette spécification complète le check en :
- Définissant l'inventaire exact des 33 caractères valides.
- Spécifiant les règles de normalisation NFC et UTF-8.
- Fournissant la liste des formes obsolètes à corriger automatiquement.
- Ajoutant le rejet des espaces insécables et des apostrophes.

### 11.2 Common Voice

Le pipeline de nettoyage CV26 (Mokraoui 2026) doit être aligné sur cette spécification :
- La whitelist de caractères de la Section 9.1 remplace toute liste ad hoc.
- Les 29 familles de faux amis sont réduites aux 6 principales documentées ici (les 23 secondaires restent à inventorier).
- L'interdiction de l'apostrophe et des espaces insécables est ajoutée.

### 11.3 HuggingFace Datasets

Les datasets kabyles doivent inclure dans leur `dataset card` :
- La référence à cette spécification.
- Le statut de normalisation (NFC, UTF-8, faux amis corrigés, espaces normalisés).
- Le score GlotLID moyen du corpus.

---

## 12. Formes obsolètes et héritées (Contexte historique)

Cette section est informative. Elle recense les formes qui apparaissent dans des textes anciens ou informels mais qui **ne sont pas l'orthographe standard actuelle**.

| Forme obsolète | Forme actuelle | Contexte d'apparition |
|----------------|----------------|----------------------|
| `gh` | `ɣ` | Textes numériques anciens, saisie sans clavier kabyle |
| `dj` | `ǧ` | Textes numériques anciens, saisie sans clavier kabyle |
| `ch` | `č` | Textes numériques anciens, saisie sans clavier kabyle |
| `th` | `ṭ` | Textes numériques anciens, saisie sans clavier kabyle |
| `sh` | `ṣ` | Textes numériques anciens, saisie sans clavier kabyle |
| `dh` | `ḍ` | Textes numériques anciens, saisie sans clavier kabyle |
| `rh` | `ṛ` | Textes numériques anciens, saisie sans clavier kabyle |
| `zh` | `ẓ` | Textes numériques anciens, saisie sans clavier kabyle |
| `ε` (grec) | `ɛ` | Erreur de clavier ou copier-coller |
| `γ` (grec) | `ɣ` | Erreur de clavier ou copier-coller |
| `ţ` / `ţ` | `ṭ` | Pré-Unicode, cédille au lieu de point souscrit |
| `z̧` | `ẓ` | Pré-Unicode, cédille au lieu de point souscrit |
| `e=` / `y=` | `ɛ` / `ɣ` | Séquences de saisie clavier (Lexilogos), non destinées au stockage |
| `Γ` (grec) | `Ɣ` | Erreur de clavier majuscule |
| `Σ` (grec) | `Ɛ` | Erreur de clavier majuscule |

---

## 13. Implémentation de référence (Informative)

Une implémentation de référence conforme doit exposer :

```python
def normalize_kabyle(text: str, profile: str = "standard") -> str:
    """Retourne le texte kabyle normalisé (NFC, faux amis corrigés, espaces normalisés)."""
    ...

def is_canonical_kabyle(text: str) -> bool:
    """Retourne True si le texte utilise uniquement l'inventaire des 33 lettres et la ponctuation autorisée."""
    ...

def list_contaminants(text: str) -> list[Contaminant]:
    """Retourne tous les points de code non canoniques avec leurs positions et remplacements suggérés."""
    ...

def flag_digraphs(text: str) -> list[DigraphMatch]:
    """Retourne tous les digraphes hérités nécessitant une révision manuelle."""
    ...
```

---

## 14. Limites connues et feuille de route

| ID | Limite | Statut |
|----|--------|--------|
| L1 | **23 familles secondaires de faux amis** : seuls les 6 principaux et les familles secondaires étendues (§3.2) sont documentés ici. L'inventaire complet des 29 familles identifiées dans CV26 reste à formaliser. | Extension nécessaire |
| L2 | **Ordre de collation** : aucune source ne définit explicitement si `ɛ` se classe après `e` ou à la fin de l'alphabet. | Spécification souhaitée |
| L3 | **Tifinagh** : cette spec ne couvre pas l'écriture Tifinagh (Neo-Tifinagh). | Spec séparée souhaitable |
| L4 | **Majuscules spéciales** : la fréquence des capitales spéciales est quasi-nulle en position non-initiale. Leur placement sur `Shift` + touche morte est validé mais non testé en usage réel. | [À VALIDER] |
| L5 | **Règles de césure** : les règles de coupure de mots en fin de ligne ne sont pas encore standardisées. | Extension nécessaire |

---

## 15. Conclusion

Cette spécification établit l'inventaire canonique de 33 caractères pour l'orthographe kabyle standard, avec leurs points de code Unicode vérifiés, et définit les règles de normalisation nécessaires au traitement automatique de la langue. Elle constitue la brique fondamentale sur laquelle reposent toutes les autres spécifications du stack kabyle : clavier, tokenization, annotation syntaxique, et synthèse vocale.

La principale avancée par rapport à l'état actuel est la **formalisation du rejet des faux amis**, la **normalisation des espaces et de la ponctuation**, et l'établissement de barrières qualité mesurables pour les corpus d'entraînement de l'IA. L'adoption de cette spécification par les plateformes de contribution (Weblate, Common Voice, Tatoeba) et les pipelines de dataset (HuggingFace) garantira la cohérence orthographique des ressources numériques kabyles.

---

## Références

1. **Chaker, Salem** (1996). *Propositions pour la notation usuelle à base latine du berbère*. INALCO / Centre de Recherche Berbère, Paris. Synthèse de l'atelier du 24–25 juin 1996. https://www.centrederechercheberbere.fr/tl_files/doc-pdf/notation.pdf
2. **Naït-Zerrad, Kamal** (2001). *Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit*. Karthala, Paris.
3. **Adjed, F.** *Vers une Normalisation du Kabyle: Alphabet*. HAL Archives ouvertes. https://hal.science/
4. **Kabyle.com** (2024). *L'alphabet kabyle*. https://www.kabyle.com/
5. **Unicode Consortium** (2026). *Unicode Standard, Version 16.0*. https://unicode.org/versions/Unicode16.0.0/
6. **Weblate** (2026). *KabyleCharactersCheck*. Version 5.12+. https://docs.weblate.org/
7. **Mokraoui, Athmane (boffire)** (2026). *CV26 Kabyle Contamination Report*. https://butterflyoffire.codeberg.page/cv26/
8. **Mokraoui, Athmane (boffire)** (2026). *Common Voice Scripted Speech Kabyle 26.0*. HuggingFace. https://huggingface.co/datasets/boffire/common-voice-scripted-speech-kab-26
9. **Mokraoui, Athmane (boffire)** (2026). *Tatoeba English-Kabyle Parallel Corpus*. HuggingFace. https://huggingface.co/datasets/boffire/tatoeba-en-kab
10. **Tatoeba Project** (2026). *Sentences dump*. https://downloads.tatoeba.org/exports/sentences.tar.bz2

---

*Document rédigé dans le cadre du développement des ressources NLP pour la langue kabyle. Les zones nécessitant une validation native supplémentaire sont signalées [À VALIDER].*
