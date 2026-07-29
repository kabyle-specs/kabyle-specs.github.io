# Spécification d'Orthographe et de Normalisation des Caractères pour le Kabyle (Taqbaylit)

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; recherche documentaire et vérification Unicode.

**Date** : 29 juillet 2026

**Version** : 0.1-draft

**Statut** : En cours de validation — certains points sont marqués **[À VALIDER]** et nécessitent confirmation par le locuteur natif.

**Cible** : Développeurs NLP/TAL, ingénieurs système d'exploitation, mainteneurs de correcteurs orthographiques, traducteurs Weblate, concepteurs de polices de caractères, chercheurs en IA.

---

## Résumé

Le kabyle (Taqbaylit, ISO 639-3 `kab`) utilise l'alphabet latin berbère standardisé par l'INALCO en 1996, composé de 33 lettres dont 10 caractères spécifiques (č, ḍ, ɛ, ǧ, ɣ, ḥ, ṛ, ṣ, ṭ, ẓ) et leurs capitales. L'absence historique de normalisation informatique a conduit à une contamination massive des corpus par des faux amis typographiques — caractères grecs (ε, γ, Γ, Σ), cyrilliques (Ԑ, ԑ), et substituts visuels. Cette spécification définit l'inventaire canonique des caractères kabyles avec leurs point de code Unicode vérifiés, établit les règles de normalisation pour le traitement automatique, et propose des barrières qualité pour les corpus d'entraînement de l'IA.

**Mots-clés** : kabyle, taqbaylit, orthographe, normalisation, Unicode, INALCO, faux amis, contamination orthographique, corpus, NLP.

---

## 1. Introduction et périmètre

### 1.1 Le problème : contamination des corpus

L'absence de spécification orthographique informatique pour le kabyle a généré une crise d'encodage documentée dans le rapport CV26 (Mokraoui 2026) : sur 609 940 clips validés de Common Voice 26.0 Kabyle, **13 135 clips (2,15 %) sont contaminés** par des caractères non-kabyles. L'analyse du corpus Tatoeba kabyle confirme ce phénomène à l'échelle textuelle : **25 069 phrases sur 790 617 (3,17 %) contiennent au moins un faux ami**.

Ces contaminations rendent les corpus inutilisables pour l'entraînement de modèles NLP sans un pipeline de nettoyage préalable. Pire, les modèles de langage apprennent des équivalences erronées (ε = ɛ, γ = ɣ) qui se propagent ensuite dans les sorties de génération de texte.

### 1.2 Objectif de cette spec

Fournir une **spécification orthographique de référence** qui :
1. Définisse l'**inventaire canonique** des 33 caractères kabyles avec leurs point de code Unicode exacts.
2. Établisse les **règles de normalisation** pour le traitement automatique (NFC, case folding, rejet des faux amis).
3. Définisse des **barrières qualité** pour les corpus d'entraînement de l'IA.
4. Guide les **exigences de rendu** pour les polices de caractères et les systèmes d'exploitation.
5. S'intègre avec les outils de validation existants (Weblate KabyleCharactersCheck) et les pipelines NLP.

---

## 2. Inventaire canonique des caractères kabyles

### 2.1 Alphabet complet (33 lettres)

L'alphabet kabyle standard, tel qu'il est utilisé aujourd'hui dans les livres, la presse, les sites web et les logiciels, se compose de 33 lettres.

**23 lettres latines de base :**

| Majuscule | Unicode | Minuscule | Unicode | Nom | Phonème |
|-----------|---------|-----------|---------|-----|---------|
| A | U+0041 | a | U+0061 | A | /a/ |
| B | U+0042 | b | U+0062 | Bé | /b/ |
| C | U+0043 | c | U+0063 | Cé | /ʃ/ |
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

**10 lettres spécifiques au kabyle :**

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

### 2.2 Caractères absents de l'orthographe kabyle standard

Les caractères suivants **ne font pas partie** de l'orthographe kabyle telle qu'elle est pratiquée aujourd'hui dans les livres, la presse et le numérique :

| Caractère | Unicode | Usage réel | Statut |
|-----------|---------|------------|--------|
| ʷ | U+02B7 | Notation linguistique uniquement | **Exclu** — jamais utilisé dans le texte courant |
| ř | U+0158 / U+0159 | Autres variétés berbères (tuareg) | **Exclu** — n'appartient pas au kabyle |
| ţ | U+0162 / U+0163 | Pré-Unicode, remplacé par ṭ | **Exclu** — forme obsolète |
| z̧ | — | Pré-Unicode, remplacé par ẓ | **Exclu** — forme obsolète |

---

## 3. Les faux amis : typologie et Unicode

Les faux amis sont des caractères d'autres scripts (grec, cyrillique) qui sont visuellement identiques ou proches des lettres kabyles. Ils doivent être **systématiquement rejetés** dans tout corpus, texte ou entrée utilisateur.

### 3.1 Les 6 familles principales

| # | Faux ami | Code | Cible correcte | Code | Source typique | Occ. Tatoeba | Occ. CV26 |
|---|----------|------|---------------|------|----------------|-------------|-----------|
| 1 | ε (epsilon grec minuscule) | U+03B5 | ɛ (latin small letter open e) | U+025B | Clavier grec, copier-coller | **25 395** | 10 806 clips |
| 2 | Σ (sigma grec majuscule) | U+03A3 | Ɛ (latin capital letter open e) | U+0190 | Clavier grec, AZERTY shift | **1 453** | 343 clips |
| 3 | γ (gamma grec minuscule) | U+03B3 | ɣ (latin small letter gamma) | U+0263 | Clavier grec, copier-coller | **222** | 1 284 clips |
| 4 | Γ (gamma grec majuscule) | U+0393 | Ɣ (latin capital letter gamma) | U+0194 | Clavier grec | **129** | 117 clips |
| 5 | Ԑ (epsilon cyrillique majuscule) | U+0510 | Ɛ (latin capital letter open e) | U+0190 | Clavier cyrillique | **578** | 343 clips |
| 6 | ԑ (epsilon cyrillique minuscule) | U+0511 | ɛ (latin small letter open e) | U+025B | Clavier cyrillique | **8** | 155 clips |

### 3.2 Mécanisme de contamination

La contamination se produit par trois canaux :
1. **Saisie directe** : l'utilisateur dispose d'un clavier grec/cyrillique et saisit visuellement (ε au lieu de ɛ).
2. **Copier-coller** : texte importé depuis des sources non standardisées (PDF scannés, sites web anciens).
3. **Conversion automatique** : OCR ou transcription automatique produisant des substituts visuels.

---

## 4. Règles de normalisation

### 4.1 Formes canoniques

Tout texte kabyle destiné au stockage, à l'échange ou à l'entraînement de modèles doit respecter les règles suivantes :

| Règle | Description | Justification |
|-------|-------------|---------------|
| **R1** | **Formes précomposées uniquement.** Utiliser `č` U+010D, jamais `c` + caron combiné. | Toutes les lettres spécifiques kabyles possèdent un point de code Unicode dédié. |
| **R2** | **Case mapping explicite.** `Ɛ` U+0190 ↔ `ɛ` U+025B et `Ɣ` U+0194 ↔ `ɣ` U+0263 ne sont pas des paires standard Unicode. | Les implémentations doivent gérer manuellement ces mappings. |
| **R3** | **NFC obligatoire.** Tout texte doit être normalisé en NFC avant stockage. | Les caractères kabyles sont déjà précomposés ; NFC ne les modifie pas mais uniformise le reste. |
| **R4** | **UTF-8 obligatoire.** Interdiction des encodages Latin-1, Windows-1252, ou tout autre encodage ne couvrant pas l'Extended Latin Additional. | Les lettres à point souscrit (ḍ, ḥ, ṛ, ṣ, ṭ, ẓ) se trouvent dans le bloc Latin Extended Additional (U+1E00–U+1EFF). |
| **R5** | **Rejet systématique des faux amis.** Tout texte contenant ε, Σ, γ, Γ, Ԑ, ԑ doit être rejeté ou corrigé avant intégration à un corpus. | Ces caractères appartiennent à d'autres scripts et ne sont jamais valides en kabyle. |

### 4.2 Traitement des emprunts

Les lettres **P, O, V** sont valides uniquement dans les contextes suivants :
- Noms propres étrangers non transcrits (`Paris`, `Omar`, `Ville`)
- Terminologie technique non intégrée (`PDF`, `OVH`, `VPN`)
- Sigles et acronymes internationaux

Dans tout le reste du texte kabyle, leur présence doit déclencher un avertissement de qualité.

---

## 5. Barrières qualité pour les corpus et l'IA

### 5.1 Pipeline de validation obligatoire

Tout corpus kabyle destiné à l'entraînement de modèles de langage doit passer par les barrières suivantes :

| Étape | Contrôle | Seuil / Action |
|-------|----------|----------------|
| **B1** | Whitelist caractères | Seuls les 33 lettres + tiret + ponctuation standard + chiffres sont autorisés. |
| **B2** | Détection des faux amis | Rejet immédiat si ε, Σ, γ, Γ, Ԑ, ԑ détectés. |
| **B3** | Détection des formes obsolètes | Flag si `ţ`, `z̧`, `ř` détectés ; correction auto vers `ṭ`, `ẓ`. |
| **B4** | Langue identifiée | GlotLID `kab_Latn` ≥ 0.95 (standard établi par Mokraoui 2026). |
| **B5** | Longueur minimale | ≥ 3 caractères après suppression des espaces. |
| **B6** | Normalisation NFC | Vérification que le texte est bien en NFC. |
| **B7** | Encodage UTF-8 | Vérification que le fichier est encodé en UTF-8 sans BOM. |

### 5.2 Métriques de qualité attendues

Sur la base du rapport CV26 et de l'analyse Tatoeba :

| Métrique | Valeur cible | Tolérance maximale |
|----------|--------------|--------------------|
| Taux de faux amis | 0 % | &lt; 0,01 % |
| Taux de formes obsolètes | 0 % | &lt; 0,05 % |
| Taux de caractères non-kabyles | &lt; 0,1 % | &lt; 0,5 % |
| Taux de textes non-kabyle (GlotLID) | 0 % | &lt; 1 % |

---

## 6. Exigences de rendu des polices de caractères

### 6.1 Support Unicode minimal

Toute police de caractères destinée au kabyle doit supporter au minimum les blocs suivants :
- **Basic Latin** (U+0000–U+007F) : lettres de base, chiffres, ponctuation, tiret.
- **Latin Extended-A** (U+0100–U+017F) : č, Č.
- **Latin Extended-B** (U+0180–U+024F) : ɛ, Ɛ, ɣ, Ɣ, ǧ, Ǧ.
- **Latin Extended Additional** (U+1E00–U+1EFF) : ḍ, Ḍ, ḥ, Ḥ, ṛ, Ṛ, ṣ, Ṣ, ṭ, Ṭ, ẓ, Ẓ.

### 6.2 Exigences de lisibilité

| Exigence | Description | Critère |
|----------|-------------|---------|
| **E1** | Le point souscrit doit être clairement visible et distinct du corps de la lettre. | À 11 px et au-dessus, `ḍ` et `d` doivent rester distinguables. |
| **E2** | Le caron sur `č` et `ǧ` ne doit pas toucher la hampe de la lettre. | Pas de fusion visuelle à taille réduite. |
| **E3** | L'open E `ɛ` et `Ɛ` ne doivent pas être confondus avec le epsilon grec `ε` ou le E latin `e`. | Forme ouverte en C, pas de barre médiane. |
| **E4** | Le gamma latin `ɣ` et `Ɣ` ne doivent pas être confondus avec le gamma grec `γ` ou le g latin `g`. | Boucle fermée en bas, pas de crochet. |

---

## 7. Intégration avec les outils existants

### 7.1 Weblate

Le **KabyleCharactersCheck** (v5.12+) implémente déjà le rejet des faux amis grecs et cyrilliques. Cette spécification complète le check en :
- Définissant l'inventaire exact des 33 caractères valides.
- Spécifiant les règles de normalisation NFC et UTF-8.
- Fournissant la liste des formes obsolètes à corriger automatiquement.

### 7.2 Common Voice

Le pipeline de nettoyage CV26 (Mokraoui 2026) doit être aligné sur cette spécification :
- La whitelist de caractères de la Section 5.1 remplace toute liste ad hoc.
- Les 29 familles de faux amis sont réduites aux 6 principales documentées ici (les 23 secondaires restent à inventorier).

### 7.3 HuggingFace Datasets

Les datasets kabyles doivent inclure dans leur `dataset card` :
- La référence à cette spécification.
- Le statut de normalisation (NFC, UTF-8, faux amis corrigés).
- Le score GlotLID moyen du corpus.

---

## 8. Formes obsolètes et héritées (Contexte historique)

Cette section est informative. Elle recense les formes qui apparaissent dans des textes anciens ou informels mais qui **ne sont pas l'orthographe standard actuelle**.

| Forme obsolète | Forme actuelle | Contexte d'apparition |
|----------------|----------------|----------------------|
| `gh` | `ɣ` | Textes numériques anciens, saisie sans clavier kabyle |
| `dj` | `ǧ` | Textes numériques anciens, saisie sans clavier kabyle |
| `ε` (grec) | `ɛ` | Erreur de clavier ou copier-coller |
| `γ` (grec) | `ɣ` | Erreur de clavier ou copier-coller |
| `ţ` / `ţ` | `ṭ` | Pré-Unicode, cédille au lieu de point souscrit |
| `z̧` | `ẓ` | Pré-Unicode, cédille au lieu de point souscrit |
| `e=` / `y=` | `ɛ` / `ɣ` | Séquences de saisie clavier (Lexilogos), non destinées au stockage |
| `Γ` (grec) | `Ɣ` | Erreur de clavier majuscule |
| `Σ` (grec) | `Ɛ` | Erreur de clavier majuscule |

---

## 9. Limites connues et feuille de route

| ID | Limite | Statut |
|----|--------|--------|
| L1 | **23 familles secondaires de faux amis** : seuls les 6 principaux sont documentés ici. L'inventaire complet des 29 familles identifiées dans CV26 reste à formaliser. | Extension nécessaire |
| L2 | **Ordre de collation** : aucune source ne définit explicitement si `ɛ` se classe après `e` ou à la fin de l'alphabet. | Spécification souhaitée |
| L3 | **Tifinagh** : cette spec ne couvre pas l'écriture Tifinagh (Neo-Tifinagh). | Spec séparée souhaitable |
| L4 | **Ponctuation et espacement** : les règles de ponctuation (guillemets, points de suspension, espaces insécables) ne sont pas encore standardisées. | Extension nécessaire |
| L5 | **Majuscules spéciales** : la fréquence des capitales spéciales est quasi-nulle en position non-initiale. Leur placement sur `Shift` + touche morte est validé mais non testé en usage réel. | [À VALIDER] |

---

## 10. Conclusion

Cette spécification établit l'inventaire canonique de 33 caractères pour l'orthographe kabyle standard, avec leurs point de code Unicode vérifiés, et définit les règles de normalisation nécessaires au traitement automatique de la langue. Elle constitue la brique fondamentale sur laquelle reposent toutes les autres spécifications du stack kabyle : clavier, tokenization, annotation syntaxique, et synthèse vocale.

La principale avancée par rapport à l'état actuel est la **formalisation du rejet des faux amis** et l'établissement de barrières qualité mesurables pour les corpus d'entraînement de l'IA. L'adoption de cette spécification par les plateformes de contribution (Weblate, Common Voice, Tatoeba) et les pipelines de dataset (HuggingFace) garantira la cohérence orthographique des ressources numériques kabyles.

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
