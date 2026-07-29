# Spécification des Dispositions de Clavier pour le Kabyle (Taqbaylit)

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; structuration technique et recherche documentaire.

**Date** : 29 juillet 2026

**Version** : 0.3-draft

**Statut** : En cours de validation native — certains points sont marqués **[À VALIDER]** et nécessitent confirmation ou correction par le locuteur natif.

**Cible** : Développeurs de dispositions de clavier, ingénieurs système d'exploitation, mainteneurs de correcteurs orthographiques, traducteurs Weblate, chercheurs en NLP.

---

## Résumé

Le kabyle (Taqbaylit, ISO 639-3 `kab`) utilise l'alphabet latin berbère standardisé par l'INALCO en 1996, composé de 33 caractères dont 10 lettres spécifiques (č, ḍ, ɛ, ǧ, ɣ, ḥ, ṛ, ṣ, ṭ, ẓ) et leurs capitales. L'absence historique de dispositions de clavier dédiées a conduit à une contamination massive des corpus par des faux amis typographiques — caractères grecs (ε, γ, Γ, Σ), cyrilliques (Ԑ, ԑ), et autres substituts visuels. Cette spécification inventorie les caractères requis, documente les 29 familles de faux amis identifiées, propose une **méthode de saisie abstraite (KIM)** fondée sur une touche morte standard, et définit des **profils de compatibilité physique** (AZERTY, QWERTY, Bépo, ErgoL) sans en imposer aucun comme unique vérité. Une analyse de fréquence sur le corpus Tatoeba kabyle (790 617 phrases, 18,8M caractères) guide les recommandations de placement.

**Mots-clés** : kabyle, taqbaylit, clavier, keyboard layout, INALCO, Unicode, faux amis, contamination orthographique, touches mortes, dead keys, KIM, analyse de fréquence, Tatoeba.

---

## 1. Introduction

### 1.1 Le problème : contamination des corpus par faux amis

L'absence de dispositions de clavier standardisées pour le kabyle a généré une crise d'encodage documentée dans le rapport CV26 (Mokraoui 2026) : sur 609 940 clips validés de Common Voice 26.0 Kabyle, **13 135 clips (2,15 %) sont contaminés** par des caractères non-kabyles. L'analyse du corpus Tatoeba kabyle (Section 7) confirme ce phénomène à l'échelle textuelle : **25 069 phrases sur 790 617 (3,17 %) contiennent au moins un faux ami**.

| Famille de faux amis | Caractères sources | Cible kabyle | Occurrences Tatoeba | Occurrences CV26 |
|---------------------|-------------------|--------------|---------------------|------------------|
| **Epsilon grec** | ε (U+03B5) | ɛ (U+025B) | 25 395 | 10 806 clips |
| **Sigma grec maj.** | Σ (U+03A3) | Ɛ (U+0190) | 1 453 | 343 clips |
| **Epsilon cyrillique maj.** | Ԑ (U+0510) | Ɛ (U+0190) | 578 | 343 clips |
| **Gamma grec** | γ (U+03B3) | ɣ (U+0263) | 222 | 1 284 clips |
| **Gamma grec maj.** | Γ (U+0393) | Ɣ (U+0194) | 129 | 117 clips |
| **Epsilon cyrillique min.** | ԑ (U+0511) | ɛ (U+025B) | 8 | 155 clips |
| **Autres layouts** | Turkish, Romanian, AZERTY, Spanish, Esperanto... | Divers | ~430 (CV26) | — |

**Total** : 29 types de faux amis identifiés. Ces contaminations rendent les corpus inutilisables pour l'entraînement de modèles NLP sans un pipeline de nettoyage préalable.

### 1.2 Objectif de cette spec

Fournir une **disposition de clavier de référence neutre et performante** qui :
1. Définisse une **méthode de saisie abstraite (KIM)** indépendante du clavier physique.
2. Propose des **profils de compatibilité** pour les systèmes d'exploitation majeurs sans imposer de layout unique.
3. Utilise un mécanisme de **touche morte standard** cohérent avec les pratiques internationales et le layout amazighe de SIL.
4. S'intègre avec les outils de validation (Weblate KabyleCharactersCheck) et les pipelines NLP.
5. Guide le placement optimal des caractères via une **analyse de fréquence corpus** (Tatoeba, Section 7).

---

## 2. Inventaire des caractères kabyle INALCO 1996

### 2.1 Alphabet complet (33 lettres)

| Majuscule | Unicode | Minuscule | Unicode | Nom | Phonème |
|-----------|---------|-----------|---------|-----|---------|
| A | U+0041 | a | U+0061 | A | /a/ |
| B | U+0042 | b | U+0062 | Bé | /b/ |
| C | U+0043 | c | U+0063 | Cé | /ʃ/ |
| Č | U+010C | č | U+010D | Čé | /t͡ʃ/ |
| D | U+0044 | d | U+0064 | Dé | /d/ |
| Ḍ | U+1E0C | ḍ | U+1E0D | Ḍé | /dˤ/ |
| E | U+0045 | e | U+0065 | E | /ə/ |
| Ɛ | U+0190 | ɛ | U+025B | Ɛayn | /ʕ/ |
| F | U+0046 | f | U+0066 | Éf | /f/ |
| G | U+0047 | g | U+0067 | Gé | /ɡ/ |
| Ǧ | U+01E6 | ǧ | U+01E7 | Ǧé | /d͡ʒ/ |
| Ɣ | U+0194 | ɣ | U+0263 | Ɣayn | /ɣ/ [ʁ] |
| H | U+0048 | h | U+0068 | Hach | /h/ |
| Ḥ | U+1E24 | ḥ | U+1E25 | Ḥa | /ħ/ |
| I | U+0049 | i | U+0069 | I | /i/ |
| J | U+004A | j | U+006A | Ji | /ʒ/ |
| K | U+004B | k | U+006B | Ka | /k/ |
| L | U+004C | l | U+006C | El | /l/ |
| M | U+004D | m | U+006D | Em | /m/ |
| N | U+004E | n | U+006E | En | /n/ |
| Q | U+0051 | q | U+0071 | Qaf | /q/ |
| R | U+0052 | r | U+0072 | Er | /r/ |
| Ṛ | U+1E5A | ṛ | U+1E5B | Ṛa | /rˤ/ |
| S | U+0053 | s | U+0073 | Ess | /s/ |
| Ṣ | U+1E62 | ṣ | U+1E63 | Ṣad | /sˤ/ |
| T | U+0054 | t | U+0074 | Té | /t/ |
| Ṭ | U+1E6C | ṭ | U+1E6D | Ṭa | /tˤ/ |
| U | U+0055 | u | U+0075 | U | /u/ |
| W | U+0057 | w | U+0077 | Waw | /w/ |
| X | U+0058 | x | U+0078 | Xa | /χ/ |
| Y | U+0059 | y | U+0079 | Yé | /j/ |
| Z | U+005A | z | U+007A | Zéd | /z/ |
| Ẓ | U+1E92 | ẓ | U+1E93 | Ẓa | /zˤ/ |

### 2.2 Caractères absents de l'INALCO 1996 mais utilisés

| Caractère | Unicode | Usage | Statut |
|-----------|---------|-------|--------|
| V | U+0056 | Emprunts uniquement | Latin standard |
| P | U+0050 | Emprunts uniquement | Latin standard |
| O | U+004F | Non utilisé en kabyle natif | Latin standard |
| - | U+002D | Clitiques, coordination, composés | **Obligatoire** |

**Note** : Les lettres `p` et `v` n'existent qu'en position d'emprunt (français, arabe). La lettre `o` n'est pas utilisée en kabyle standard. Le **tiret** `-` est omniprésent en kabyle (clitiques préverbaux `a-`, `ad-`, `i-`, coordination, noms composés) et doit être pleinement accessible sur toutes les plateformes, y compris Android.

---

## 3. Les faux amis : typologie et Unicode

### 3.1 Les 6 familles principales (d'après CV26 + Tatoeba)

| # | Faux ami | Code | Cible correcte | Code | Source typique | Occ. Tatoeba | Occ. CV26 |
|---|----------|------|---------------|------|----------------|-------------|-----------|
| 1 | ε (epsilon grec minuscule) | U+03B5 | ɛ (latin small letter open e) | U+025B | Clavier grec, copier-coller | **25 395** | 10 806 clips |
| 2 | Σ (sigma grec majuscule) | U+03A3 | Ɛ (latin capital letter open e) | U+0190 | Clavier grec, AZERTY shift | **1 453** | 343 clips |
| 3 | γ (gamma grec minuscule) | U+03B3 | ɣ (latin small letter gamma) | U+0263 | Clavier grec, copier-coller | **222** | 1 284 clips |
| 4 | Γ (gamma grec majuscule) | U+0393 | Ɣ (latin capital letter gamma) | U+0194 | Clavier grec | **129** | 117 clips |
| 5 | Ԑ (epsilon cyrillique majuscule) | U+0510 | Ɛ (latin capital letter open e) | U+0190 | Clavier cyrillique | **578** | 343 clips |
| 6 | ԑ (epsilon cyrillique minuscule) | U+0511 | ɛ (latin small letter open e) | U+025B | Clavier cyrillique | **8** | 155 clips |

### 3.2 Les 23 familles secondaires

D'après le rapport CV26, les autres faux amis proviennent de :
- **Turc** : caractères avec cédille et breve différents
- **Roumain** : caractères diacritiques proches
- **AZERTY français** : accents aigus/graves sur les voyelles
- **Espagnol** : tilde et accents
- **Espéranto** : caractères avec hat-check
- **Autres** : caractères mathématiques, symboles, lettres phonétiques

**[À VALIDER]** : La liste complète des 23 familles restantes n'est pas détaillée dans le rapport CV26 publié. Une extension de cette spec avec l'inventaire exact est souhaitable.

### 3.3 Mécanisme de contamination

La contamination se produit par trois canaux :
1. **Saisie directe** : l'utilisateur dispose d'un clavier grec/cyrillique et saisit visuellement (ε au lieu de ɛ).
2. **Copier-coller** : texte importé depuis des sources non standardisées (PDF scannés, sites web anciens).
3. **Conversion automatique** : OCR ou transcription automatique produisant des substituts visuels.

---

## 4. Kabyle Input Method (KIM) — Abstraction de saisie

### 4.1 Principe

Le **KIM** définit un mapping logique entre séquences de frappe et caractères Unicode, **indépendamment du clavier physique**. Il s'agit d'une couche d'abstraction qui garantit la portabilité et la neutralité de la spec.

**Mécanisme principal** : touche morte `^` (circonflexe, U+005E), inspirée du layout amazighe de SIL. Le circonflexe est une touche morte standard ISO présente nativement sur AZERTY, QWERTY, Bépo et la plupart des layouts européens. Il n'a aucun usage natif en kabyle, ce qui évite les conflits.

### 4.2 Table KIM : séquences ^ + lettre

| Séquence | Résultat | Unicode | Nom | Logique |
|----------|----------|---------|-----|---------|
| `^` + `a` | ɛ | U+025B | e ouvert | `a` = voyelle ouverte, proximité phonétique avec /ʕ/ |
| `^` + `A` | Ɛ | U+0190 | E ouvert | Majuscule |
| `^` + `q` | ɣ | U+0263 | gamma latin | `q` = touche disponible (emprunts uniquement) |
| `^` + `Q` | Ɣ | U+0194 | Gamma latin | Majuscule |
| `^` + `c` | č | U+010D | c caron | `c` de base + caron |
| `^` + `C` | Č | U+010C | C caron | Majuscule |
| `^` + `d` | ḍ | U+1E0D | d point souscrit | `d` de base + point souscrit |
| `^` + `D` | Ḍ | U+1E0C | D point souscrit | Majuscule |
| `^` + `g` | ǧ | U+01E7 | g caron | `g` de base + caron |
| `^` + `G` | Ǧ | U+01E6 | G caron | Majuscule |
| `^` + `h` | ḥ | U+1E25 | h point souscrit | `h` de base + point souscrit |
| `^` + `H` | Ḥ | U+1E24 | H point souscrit | Majuscule |
| `^` + `r` | ṛ | U+1E5B | r point souscrit | `r` de base + point souscrit |
| `^` + `R` | Ṛ | U+1E5A | R point souscrit | Majuscule |
| `^` + `s` | ṣ | U+1E63 | s point souscrit | `s` de base + point souscrit |
| `^` + `S` | Ṣ | U+1E62 | S point souscrit | Majuscule |
| `^` + `t` | ṭ | U+1E6D | t point souscrit | `t` de base + point souscrit |
| `^` + `T` | Ṭ | U+1E6C | T point souscrit | Majuscule |
| `^` + `z` | ẓ | U+1E93 | z point souscrit | `z` de base + point souscrit |
| `^` + `Z` | Ẓ | U+1E92 | Z point souscrit | Majuscule |

### 4.3 Règles de casse

- `Shift` + `^` + `lettre` → majuscule spéciale (équivalent à `^` + `Shift` + `lettre`).
- `Caps Lock` active la couche majuscule standard ; les caractères spéciaux nécessitent toujours la touche morte `^`.
- Aucun caractère spécial kabyle n'est accessible directement sans touche morte, ce qui garantit la cohérence du KIM.

### 4.4 Pourquoi `^` plutôt qu'AltGr ou `=` ?

| Critère | `^` (circonflexe) | AltGr | `=` (Lexilogos) |
|---------|-------------------|-------|-----------------|
| Standard ISO | ✅ Touche morte native | ⚠️ Modificateur, pas touche morte | ❌ Astuce web |
| Portabilité OS | ✅ Windows, Linux, macOS | ⚠️ Conflits TSF sous Windows | ❌ Nécessite JavaScript |
| AZERTY | ✅ Présent nativement | ⚠️ Saturé (€, {, [, \|) | ✅ Présent |
| QWERTY | ✅ Présent nativement | ⚠️ Saturé | ✅ Présent |
| Bépo / ErgoL | ✅ Présent nativement | ⚠️ Mapping différent | ⚠️ Non standard |
| Conflit kabyle | ✅ Aucun (pas de voyelles circonflexes) | ⚠️ Conflits possibles | ⚠️ Utilisé en mathématiques |

---

## 5. Profils de compatibilité physique

La spec ne privilégie aucun layout physique. Elle définit des **profils de compatibilité** qui implémentent le KIM sur des bases existantes.

### 5.1 Tableau des profils

| Profil | Base physique | Public cible | Statut | Priorité |
|--------|--------------|--------------|--------|----------|
| **AZERTY-kab** | AZERTY | Algérie, France, Belgique | Profil primaire | Haute |
| **QWERTY-kab** | QWERTY | Diaspora nord-américaine, anglophone | Profil secondaire | Moyenne |
| **BÉPO-kab** | Bépo | Utilisateurs ergonomiques francophones | Profil optionnel | Basse |
| **ErgoL-kab** | ErgoL | Utilisateurs optimisation extrême | Profil expérimental | Basse |

### 5.2 AZERTY-kab (profil primaire)

L'Algérie utilise officiellement le clavier AZERTY. La diaspora kabyle en France et en Belgique est majoritairement sur AZERTY. Ce profil est le **référent de compatibilité par défaut**.

**Placement de la touche morte `^`** : sur AZERTY, le circonflexe est en haut à gauche (touche `9`, accessible sans Shift sur la plupart des AZERTY français). C'est une position peu ergonomique mais standard.

**[À VALIDER]** : Faut-il déplacer la touche morte `^` vers une position plus accessible (ex. point-virgule `;`) sur le profil AZERTY-kab, ou conserver le placement standard pour ne pas perturber les habitudes ?

### 5.3 QWERTY-kab (profil secondaire)

Pour la diaspora kabyle en Amérique du Nord et dans les pays anglophones. Le circonflexe est sur la touche `6` en QWERTY US, accessible sans Shift.

### 5.4 BÉPO-kab et ErgoL-kab (profils optionnels)

**Bépo** est un layout ergonomique francophone (type Dvorak) optimisé pour la frappe française. **ErgoL** est un layout français récent optimisé par analyse algorithmique de fréquences et de digrammes.

Ces profils sont **optionnels et expérimentaux**. Ils sont mentionnés dans cette spec pour :
- Encourager la recherche sur l'ergonomie de la saisie kabyle.
- Fournir une base pour les utilisateurs déjà convertis à ces layouts.
- Ne pas enfermer la standardisation kabyle dans les limitations ergonomiques d'AZERTY.

**Avertissement** : l'adoption de Bépo ou ErgoL représente une barrière d'apprentissage significative. Ces profils ne doivent pas être promus comme solution principale pour les locuteurs kabyles non-initiés.

---

## 6. Implémentation technique par système d'exploitation

### 6.1 Linux (XKB — X Keyboard Extension)

**Format** : Fichier de symboles XKB (`/usr/share/X11/xkb/symbols/kab`)

**Avantages** :
- XKB supporte nativement les **touches mortes** et les **niveaux** (Shift, AltGr, Shift+AltGr).
- Intégration transparente avec tous les environnements de bureau (GNOME, KDE, etc.).

**Exemple de configuration XKB (touche morte `^`)** :
```
partial alphanumeric_keys
xkb_symbols "kab" {
    include "fr(azerty)"
    name[Group1] = "Kabyle (Taqbaylit)";

    // Déclaration de la touche morte ^ (circonflexe) comme dead key
    // Sur AZERTY, la touche 9 produit ^ en tant que dead key
    // Mapping KIM : ^ + lettre → caractère spécial

    key <AE01> { [ ampersand, 1, dead_circumflex, dead_caron ] };

    // Niveau 3 (AltGr) et 4 (Shift+AltGr) pour accès direct optionnel
    key <AD01> { [ a, A, U025B, U0190 ] };      // a → ɛ, Ɛ
    key <AD02> { [ z, Z, U1E93, U1E92 ] };      // z → ẓ, Ẓ
    key <AD03> { [ e, E, EuroSign, U0190 ] };   // e standard
    key <AD04> { [ r, R, U1E5B, U1E5A ] };      // r → ṛ, Ṛ
    key <AD05> { [ t, T, U1E6D, U1E6C ] };      // t → ṭ, Ṭ
    key <AD06> { [ y, Y, U0263, U0194 ] };      // y → ɣ, Ɣ
    key <AC02> { [ s, S, U1E63, U1E62 ] };      // s → ṣ, Ṣ
    key <AC03> { [ d, D, U1E0D, U1E0C ] };      // d → ḍ, Ḍ
    key <AC05> { [ g, G, U01E7, U01E6 ] };      // g → ǧ, Ǧ
    key <AC06> { [ h, H, U1E25, U1E24 ] };      // h → ḥ, Ḥ
    key <AB03> { [ c, C, U010D, U010C ] };      // c → č, Č
};
```

**Note** : La touche morte `^` en XKB utilise le symbole `dead_circumflex`. Le mapping exact des séquences `dead_circumflex + <key>` se définit dans le fichier `compose` ou via les tables de dead keys XKB.

### 6.2 Windows (MSKLC — Microsoft Keyboard Layout Creator)

**Format** : Fichier `.klc` (Keyboard Layout Creator) ou `.dll` compilé.

**Problèmes connus** :
- Les touches mortes complexes peuvent entrer en conflit avec le **TSF** (Text Services Framework) de Windows.
- Windows 10/11 ne supporte pas nativement les touches mortes personnalisées sans outil tiers (MSKLC).

**Solution recommandée** :
- Utiliser **Microsoft Keyboard Layout Creator (MSKLC)** v1.4 pour générer le fichier `.dll`.
- Définir le circonflexe comme **touche morte native** (`dead key`) dans MSKLC.
- Si TSF pose problème, alternative : utiliser **AutoHotkey** pour le mapping dynamique `^ + lettre`.

**[À VALIDER]** : Le mécanisme de touches mortes natives est-il fonctionnellement préférable aux raccourcis directs (AltGr+lettre) sous Windows ?

### 6.3 macOS (Ukelele / .keylayout)

**Format** : Fichier `.keylayout` XML ou `.bundle` via Ukelele.

**Particularités** :
- macOS utilise le système de **keylayouts** XML avec support des touches mortes via `<deadKey>`.
- L'outil **Ukelele** (SIL International) permet de créer des dispositions graphiquement.

**Exemple de touche morte macOS** :
```xml
<keyMapSelect mapIndex="4"> <!-- Option (AltGr) -->
  <key code="0" output="ɛ"/>   <!-- a → ɛ -->
  <key code="6" output="ɣ"/>   <!-- z → ɣ -->
  <!-- etc. -->
</keyMapSelect>

<keyMapSelect mapIndex="5"> <!-- Shift + Option -->
  <key code="0" output="Ɛ"/>   <!-- A → Ɛ -->
  <key code="6" output="Ɣ"/>   <!-- Z → Ɣ -->
  <!-- etc. -->
</keyMapSelect>
```

### 6.4 Android / iOS

**Recommandation** : Création d'un clavier virtuel via le **Gboard Keyboard API** ou une application Flutter dédiée.

**Contrainte critique** : Le **tiret** `-` (U+002D) doit être **visible et accessible en permanence** sur la rangée principale du clavier virtuel Android. Le kabyle utilise intensivement le tiret pour :
- Les clitiques préverbaux : `a-`, `ad-`, `i-`, `t-`
- La coordination : `d-` (et)
- Les noms composés : `Ameṛṛan-nneɣ`, `Taqbaylit-Aqerru`

Un clavier kabyle qui masque le tiret derrière une touche `?123` ou un long-press est **inacceptable** pour la saisie fluide.

---

## 7. Analyse de fréquence : corpus Tatoeba kabyle

### 7.1 Avertissement méthodologique

Cette analyse est basée **uniquement** sur le corpus Tatoeba kabyle (`sentences.csv`, dump officiel du 29 juillet 2026). Tatoeba est un corpus de phrases isolées, majoritairement traduites depuis d'autres langues. Il ne reflète pas :
- La distribution réelle des caractères en texte continu (paragraphes, sections)
- Les variations dialectales ou stylistiques du kabyle natif
- Les domaines spécialisés (technique, littéraire, oral transcrit)

**Les fréquences présentées ici sont indicatives et doivent être complétées par des corpus plus représentatifs** (Weblate, Common Voice transcriptions, corpus littéraires kabyles) avant toute spécification définitive.

| Métrique | Valeur |
|----------|--------|
| Phrases kabyles | **790 617** |
| Caractères analysés | **18 796 562** |
| Caractères uniques | 73 |
| Digrammes uniques | 3 492 |
| Trigrammes uniques | 37 291 |
| Phrases avec faux amis | **25 069 (3,17 %)** |

### 7.2 Fréquence unigramme — caractères spéciaux INALCO

| Caractère | Unicode | Occurrences | Fréquence (%) | Seuil | Recommandation | Position dominante |
|-----------|---------|-------------|---------------|-------|----------------|-------------------|
| **ɣ** | U+0263 | 531 010 | **2,825** | > 2% | 🟡 Level 3 / Dead key accessible | Initiale (38,96%) |
| **ḍ** | U+1E0D | 173 277 | **0,922** | 0,1–1% | 🔴 Level 4 / Compose | Finale (57,78%) |
| **ḥ** | U+1E25 | 125 087 | **0,665** | 0,1–1% | 🔴 Level 4 / Compose | Médiane (75,99%) |
| **ɛ** | U+025B | 90 896 | **0,484** | 0,1–1% | 🔴 Level 4 / Compose | Médiane (87,33%) |
| **ṛ** | U+1E5B | 80 332 | **0,427** | 0,1–1% | 🔴 Level 4 / Compose | Médiane (76,15%) |
| **ṭ** | U+1E6D | 66 407 | **0,353** | 0,1–1% | 🔴 Level 4 / Compose | Médiane (86,16%) |
| **č** | U+010D | 54 975 | **0,292** | 0,1–1% | 🔴 Level 4 / Compose | Médiane (85,92%) |
| **ẓ** | U+1E93 | 52 385 | **0,279** | 0,1–1% | 🔴 Level 4 / Compose | Médiane (84,09%) |
| **ǧ** | U+01E7 | 50 317 | **0,268** | 0,1–1% | 🔴 Level 4 / Compose | Médiane (87,60%) |
| **ṣ** | U+1E63 | 30 281 | **0,161** | 0,1–1% | 🔴 Level 4 / Compose | Médiane (82,74%) |
| **Ɛ** | U+0190 | 13 792 | **0,073** | < 0,1% | ⚪ RARE — Compose | Initiale (>99%) |
| **Ḥ** | U+1E24 | 10 539 | **0,056** | < 0,1% | ⚪ RARE — Compose | Initiale (>99%) |
| **Ɣ** | U+0194 | 10 408 | **0,055** | < 0,1% | ⚪ RARE — Compose | Initiale (>99%) |
| **Ṛ** | U+1E5A | 4 513 | **0,024** | < 0,1% | ⚪ RARE — Compose | Initiale (>99%) |
| **Ẓ** | U+1E92 | 4 173 | **0,022** | < 0,1% | ⚪ RARE — Compose | Initiale (>99%) |
| **Ṭ** | U+1E6C | 3 085 | **0,016** | < 0,1% | ⚪ RARE — Compose | Initiale (>99%) |
| **Č** | U+010C | 1 862 | **0,010** | < 0,1% | ⚪ RARE — Compose | Initiale (100%) |
| **Ǧ** | U+01E6 | 1 533 | **0,008** | < 0,1% | ⚪ RARE — Compose | Initiale (>99%) |
| **Ḍ** | U+1E0C | 1 132 | **0,006** | < 0,1% | ⚪ RARE — Compose | Initiale (>98%) |
| **Ṣ** | U+1E62 | 726 | **0,004** | < 0,1% | ⚪ RARE — Compose | Initiale (>99%) |

**Observations clés** :
- **ɣ** est le seul caractère spécial à dépasser le seuil de 2 % (2,825 %). Il mérite un accès **Level 3** (AltGr) ou une touche morte très accessible.
- Tous les autres spéciaux sont sous 1 %, ce qui justifie pleinement le mécanisme de **touche morte unique** `^` pour l'ensemble.
- Les **majuscules spéciales** sont quasi-exclusivement en position initiale (>99 %), ce qui confirme leur placement sur `Shift` + touche morte + lettre.

### 7.3 Digrammes impliquant des caractères spéciaux (top 15)

| Digramme | Occurrences | Fréquence (%) | Interprétation morphologique |
|----------|-------------|---------------|------------------------------|
| **ɣe** | 228 741 | 1,270 | Préposition / racine √ɣr, √ɣl |
| **eɣ** | 123 467 | 0,686 | Suffixe / racine √ɣr |
| **ɣa** | 93 579 | 0,520 | Préposition `ɣer` tronqué |
| **iɣ** | 86 590 | 0,481 | Préfixe verbal / racine √ɣr |
| **aɣ** | 77 766 | 0,432 | Racine √ɣr avec préfixe `a-` |
| **eḍ** | 75 799 | 0,421 | Suffixe / racine √ḍr |
| **tɣ** | 44 568 | 0,248 | Préfixe `t-` + racine √ɣr |
| **ḥe** | 44 283 | 0,246 | Racine √ḥm, √ḥd |
| **iḍ** | 41 878 | 0,233 | Préfixe `i-` + racine √ḍr |
| **ɣi** | 38 354 | 0,213 | Préposition `ɣer` / racine √ɣl |
| **ḍa** | 36 813 | 0,204 | Racine √ḍr avec suffixe |
| **uɣ** | 34 239 | 0,190 | Préfixe `u-` + racine √ɣr |
| **ɛe** | 33 363 | 0,185 | Racine √ʕm, √ʕd |
| **ḍe** | 30 091 | 0,167 | Suffixe / racine √ḍr |
| **ɣ-** | 29 384 | 0,163 | Préposition `ɣer` + clitique |

**Observation** : Les digrammes `ɣe`, `eɣ`, `ɣa`, `iɣ`, `aɣ` dominent, confirmant que **ɣ** est le caractère spécial le plus connecté morphologiquement. Le digramme `ɣ-` (préposition + clitique) justifie l'accessibilité du tiret en saisie fluide.

### 7.4 Trigrammes impliquant des caractères spéciaux (top 15)

| Trigramme | Occurrences | Fréquence (%) | Interprétation |
|-----------|-------------|---------------|----------------|
| **ɣer** | 184 819 | 1,074 | Préposition "vers/sur" |
| **aɣe** | 43 038 | 0,250 | Racine √ɣr avec préfixe `a-` |
| **tɣe** | 37 175 | 0,216 | Préfixe `t-` + racine √ɣr |
| **iɣe** | 31 686 | 0,184 | Préfixe `i-` + racine √ɣr |
| **raɣ** | 31 581 | 0,183 | Racine √ɣr avec suffixe |
| **eɣa** | 30 897 | 0,180 | Suffixe + racine √ɣr |
| **ɣar** | 21 654 | 0,126 | Variante de `ɣer` |
| **neɣ** | 21 312 | 0,124 | Conjonction "ou" |
| **ɣef** | 19 870 | 0,115 | Préposition "sur" |
| **niɣ** | 19 820 | 0,115 | Forme verbale |
| **nɣe** | 19 195 | 0,112 | Racine √ɣr avec préfixe `n-` |
| **eḍa** | 18 943 | 0,110 | Racine √ḍr avec suffixe |
| **iɣa** | 16 322 | 0,095 | Forme verbale complète |
| **leɣ** | 16 226 | 0,094 | Suffixe possessif |
| **mtɣ** | 15 903 | 0,092 | Forme verbale complexe |

**Observation** : Le trigramme **ɣer** (préposition "vers/sur") est le plus fréquent de tout le corpus (1,074 %), devançant même certains trigrammes de base latine. Cela confirme l'importance critique de la saisie fluide de **ɣ**.

### 7.5 Distribution positionnelle des caractères spéciaux

| Caractère | % Initiale | % Médiane | % Finale | Implication clavier |
|-----------|-----------|-----------|----------|---------------------|
| **ɣ** | 38,96 | 30,34 | 30,70 | Accès rapide (initiale fréquente) |
| **ḍ** | 0,44 | 41,78 | **57,78** | Touche morte + `d`, finale dominante |
| **ḥ** | 5,17 | **75,99** | 18,84 | Touche morte + `h`, médiane dominante |
| **ɛ** | 5,94 | **87,33** | 6,73 | Touche morte + `a`, médiane dominante |
| **ṛ** | 3,10 | **76,15** | 20,75 | Touche morte + `r`, médiane dominante |
| **ṭ** | 6,96 | **86,16** | 6,88 | Touche morte + `t`, médiane dominante |
| **č** | 5,03 | **85,92** | 9,06 | Touche morte + `c`, médiane dominante |
| **ẓ** | 10,07 | **84,09** | 5,84 | Touche morte + `z`, médiane dominante |
| **ǧ** | 1,26 | **87,60** | 11,14 | Touche morte + `g`, médiane dominante |
| **ṣ** | 7,71 | **82,74** | 9,55 | Touche morte + `s`, médiane dominante |

**Implication** : La quasi-totalité des spéciaux sont dominants en position **médiane** (82–88 %), ce qui signifie qu'ils apparaissent au milieu des mots. Cela justifie un mécanisme de saisie qui ne perturbe pas le flux de frappe (touche morte rapide, pas de combinaison à trois touches).

### 7.6 Synthèse et recommandations de placement

Sur la base des données Tatoeba et des seuils heuristiques (Carpalx/ErgoL) :

| Caractère | Fréquence | Recommandation KIM | Justification |
|-----------|-----------|-------------------|---------------|
| **ɣ / Ɣ** | 2,83 % / 0,06 % | `^` + `q` / `Q` | Seul spécial > 2 % ; digrammes très fréquents (ɣe, eɣ, iɣ) |
| **ḍ / Ḍ** | 0,92 % / 0,006 % | `^` + `d` / `D` | Finale dominante (57,78 %) ; racines verbales fréquentes |
| **ḥ / Ḥ** | 0,67 % / 0,06 % | `^` + `h` / `H` | Médiane dominante (76 %) ; racines sémantiques |
| **ɛ / Ɛ** | 0,48 % / 0,07 % | `^` + `a` / `A` | Médiane dominante (87 %) ; présentatif / racines √ʕ |
| **ṛ / Ṛ** | 0,43 % / 0,02 % | `^` + `r` / `R` | Médiane dominante (76 %) ; emphatique |
| **ṭ / Ṭ** | 0,35 % / 0,02 % | `^` + `t` / `T` | Médiane dominante (86 %) ; emphatique |
| **č / Č** | 0,29 % / 0,01 % | `^` + `c` / `C` | Médiane dominante (86 %) ; affriquée |
| **ẓ / Ẓ** | 0,28 % / 0,02 % | `^` + `z` / `Z` | Médiane dominante (84 %) ; emphatique |
| **ǧ / Ǧ** | 0,27 % / 0,008 % | `^` + `g` / `G` | Médiane dominante (88 %) ; affriquée |
| **ṣ / Ṣ** | 0,16 % / 0,004 % | `^` + `s` / `S` | Médiane dominante (83 %) ; emphatique |

---

## 8. Intégration avec les pipelines NLP

### 8.1 Weblate

Le **KabyleCharactersCheck** (v5.12+) doit être complété par une **recommandation de clavier** dans la documentation du projet :
- Détecter le système d'exploitation du traducteur.
- Proposer le lien de téléchargement de la disposition appropriée.
- Afficher un avertissement si le traducteur saisit un faux ami (détection en temps réel).

### 8.2 Common Voice

Intégrer la disposition de clavier dans le **workflow de contribution** :
- Page d'aide avec les raccourcis de saisie (`^ + a = ɛ`, etc.).
- Script de nettoyage post-soumission (déjà implémenté dans CV26).
- Détection côté client (JavaScript) des faux amis avant soumission.

### 8.3 HuggingFace Datasets

Ajouter un **tag `keyboard-layout`** aux datasets kabyle pour indiquer la disposition recommandée, et un script de validation dans le `dataset card`.

---

## 9. Tableau récapitulatif Unicode

| Caractère | Unicode | Bloc Unicode | Nom | Utilisé en kabyle ? |
|-----------|---------|--------------|-----|---------------------|
| ɛ | U+025B | Latin Extended-B | Latin Small Letter Open E | Oui — phonème /ʕ/ |
| Ɛ | U+0190 | Latin Extended-B | Latin Capital Letter Open E | Oui — début de phrase |
| ɣ | U+0263 | IPA Extensions | Latin Small Letter Gamma | Oui — phonème /ɣ/ [ʁ] |
| Ɣ | U+0194 | Latin Extended-B | Latin Capital Letter Gamma | Oui — début de phrase |
| č | U+010D | Latin Extended-A | Latin Small Letter C With Caron | Oui — phonème /t͡ʃ/ |
| Č | U+010C | Latin Extended-A | Latin Capital Letter C With Caron | Oui |
| ḍ | U+1E0D | Latin Extended Additional | Latin Small Letter D With Dot Below | Oui — phonème /dˤ/ |
| Ḍ | U+1E0C | Latin Extended Additional | Latin Capital Letter D With Dot Below | Oui |
| ǧ | U+01E7 | Latin Extended-B | Latin Small Letter G With Caron | Oui — phonème /d͡ʒ/ |
| Ǧ | U+01E6 | Latin Extended-B | Latin Capital Letter G With Caron | Oui |
| ḥ | U+1E25 | Latin Extended Additional | Latin Small Letter H With Dot Below | Oui — phonème /ħ/ |
| Ḥ | U+1E24 | Latin Extended Additional | Latin Capital Letter H With Dot Below | Oui |
| ṛ | U+1E5B | Latin Extended Additional | Latin Small Letter R With Dot Below | Oui — phonème /rˤ/ |
| Ṛ | U+1E5A | Latin Extended Additional | Latin Capital Letter R With Dot Below | Oui |
| ṣ | U+1E63 | Latin Extended Additional | Latin Small Letter S With Dot Below | Oui — phonème /sˤ/ |
| Ṣ | U+1E62 | Latin Extended Additional | Latin Capital Letter S With Dot Below | Oui |
| ṭ | U+1E6D | Latin Extended Additional | Latin Small Letter T With Dot Below | Oui — phonème /tˤ/ |
| Ṭ | U+1E6C | Latin Extended Additional | Latin Capital Letter T With Dot Below | Oui |
| ẓ | U+1E93 | Latin Extended Additional | Latin Small Letter Z With Dot Below | Oui — phonème /zˤ/ |
| Ẓ | U+1E92 | Latin Extended Additional | Latin Capital Letter Z With Dot Below | Oui |
| - | U+002D | Basic Latin | Hyphen-Minus | Oui — clitiques, coordination |
| ε | U+03B5 | Greek and Coptic | Greek Small Letter Epsilon | **Non** — faux ami |
| Σ | U+03A3 | Greek and Coptic | Greek Capital Letter Sigma | **Non** — faux ami |
| γ | U+03B3 | Greek and Coptic | Greek Small Letter Gamma | **Non** — faux ami |
| Γ | U+0393 | Greek and Coptic | Greek Capital Letter Gamma | **Non** — faux ami |
| Ԑ | U+0510 | Cyrillic | Cyrillic Capital Letter Reversed Ze | **Non** — faux ami |
| ԑ | U+0511 | Cyrillic | Cyrillic Small Letter Reversed Ze | **Non** — faux ami |

---

## 10. Recommandations

### 10.1 Pour les développeurs de clavier

1. **Prioriser Linux/XKB** : c'est le système le plus ouvert pour les dispositions personnalisées.
2. **Fournir un installeur Windows** (.exe ou .msi) car MSKLC est technique pour l'utilisateur lambda.
3. **Documenter les raccourcis** sur une page web dédiée avec visuel du clavier.
4. **Tester la compatibilité** avec les navigateurs (saisie dans les champs de texte web).
5. **Ne jamais masquer le tiret** sur les claviers virtuels mobiles.

### 10.2 Pour les mainteneurs de corpus

1. **Intégrer la disposition recommandée** dans la documentation de contribution (Common Voice, Tatoeba, Weblate).
2. **Maintenir le script de nettoyage** des faux amis (CV26 pipeline).
3. **Ajouter un préambule** dans les datasets indiquant la disposition de clavier utilisée pour la saisie.

### 10.3 Pour les chercheurs NLP

1. **Normaliser les corpus d'entraînement** avec le mapping faux ami → caractère correct avant tokenization.
2. **Inclure les caractères spéciaux** dans le vocabulaire du tokenizer (déjà fait dans vos tokenizers BPE).
3. **Vérifier l'encodage** : tous les caractères doivent être en UTF-8 (pas de Latin-1, pas de Windows-1252).

---

## 11. Limites connues et feuille de route

| ID | Limite | Statut |
|----|--------|--------|
| L1 | **Windows TSF** : les touches mortes personnalisées peuvent entrer en conflit avec le Text Services Framework | À tester sur Windows 10/11 |
| L2 | **macOS** : la création de bundles `.keylayout` nécessite Ukelele et une signature éventuelle | À développer |
| L3 | **Mobile** : iOS ne permet pas les claviers tiers natifs sans app store | Alternative : clavier web |
| L4 | **Liste complète des 29 faux amis** : seuls les 6 principaux sont documentés ici | Extension nécessaire |
| L5 | **Tifinagh** : cette spec ne couvre pas le clavier Tifinagh (écriture berbère originelle) | Spec séparée souhaitable |
| L6 | **Corpus représentatif** : l'analyse de fréquence repose uniquement sur Tatoeba (790K phrases isolées) | À compléter avec Weblate, CV transcriptions, corpus littéraires |
| L7 | **Placement AZERTY-kab** : la touche morte `^` est en haut à gauche sur AZERTY, peu ergonomique | [À VALIDER] : déplacement optionnel ? |
| L8 | **Bépo/ErgoL** : profils définis conceptuellement mais non implémentés | Contribution communautaire souhaitée |

---

## Références

1. **Chaker, Salem** (1996). *Propositions pour la notation usuelle à base latine du berbère*. INALCO, Paris. https://www.centrederechercheberbere.fr/tl_files/doc-pdf/notation.pdf
2. **Mokraoui, Athmane (boffire)** (2026). *CV26 Kabyle Contamination Report*. https://butterflyoffire.codeberg.page/cv26/
3. **Mokraoui, Athmane (boffire)** (2026). *KabyleCharactersCheck*. Weblate v5.12+.
4. **Lexilogos** (2026). *Clavier berbère en ligne*. https://www.lexilogos.com/clavier/berbere.htm
5. **Akufi.org** (2026). *Clavier Tamaziɣt-Tamazeq*. https://www.akufi.org/clavier
6. **Kabyle.com** (2026). *Clavier Tifinagh*. https://www.kabyle.com/
7. **Wikipedia** (2026). *Alphabet berbère latin*. https://fr.wikipedia.org/wiki/Alphabet_berb%C3%A8re_latin
8. **Unicode Consortium** (2026). *Unicode Standard 16.0*. https://unicode.org/
9. **Microsoft** (2026). *Microsoft Keyboard Layout Creator (MSKLC)*. https://www.microsoft.com/download/details.aspx?id=102134
10. **SIL International** (2026). *Ukelele*. https://software.sil.org/ukelele/
11. **Association Bépo** (2026). *Disposition de clavier Bépo*. https://bepo.fr/
12. **ErgoL** (2026). *Disposition ergonomique optimisée*. https://ergol.org/
13. **Tatoeba Project** (2026). *Sentences dump*. https://downloads.tatoeba.org/exports/sentences.tar.bz2

---

*Document rédigé dans le cadre du développement des ressources NLP pour la langue kabyle. Les points marqués [À VALIDER] nécessitent une décision du locuteur natif ou des tests utilisateur avant publication définitive. L'analyse de fréquence de la Section 7 est un test pilote sur corpus Tatoeba et ne prétend pas à l'exhaustivité.*
