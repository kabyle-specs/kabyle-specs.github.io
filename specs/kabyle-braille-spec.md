# Spécification de Transcription Braille pour le Kabyle (Taqbaylit)

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; recherche documentaire braille et structuration technique.

**Date** : 20 août 2026

**Version** : 0.2-draft (corrigée)

**Statut** : En cours de validation — certains points sont marqués **[À VALIDER]** et nécessitent confirmation par des experts braille et des locuteurs natifs déficients visuels.

**Cible** : Développeurs de technologies d'assistance, ingénieurs braille, mainteneurs de tables Liblouis, concepteurs de plages braille, chercheurs en accessibilité linguistique.

---

## Résumé

Le kabyle (Taqbaylit, ISO 639-3 `kab`) est une langue berbère parlée par 5 à 7 millions de locuteurs en Algérie. À ce jour, **aucun standard braille kabyle n'existe** — ni dans le *World Braille Usage* de l'UNESCO, ni dans le *Code braille français uniformisé* (CBFU), ni dans les répertoires de l'International Council on English Braille (ICEB). Cette spécification propose un **système de transcription braille de référence** pour l'alphabet latin berbère standard (INALCO 1996), en s'appuyant sur le CBFU comme fondement pour les caractères communs au français et au kabyle, et en définissant un **mode indicateur** pour les 10 caractères spécifiques berbères : un indicateur CBFU standard (modificateur 2, dot 5) précède la lettre de base la plus proche phonétiquement. Elle définit les formats de sortie (Unicode Braille, BRF, table Liblouis), les règles de normalisation préalable, et les barrières qualité pour la production de documents braille kabyles.

**Mots-clés** : kabyle, taqbaylit, braille, CBFU, accessibilité, transcription tactile, Liblouis, Unicode Braille, INALCO, caractères spécifiques berbères.

---

## 1. Introduction et périmètre

### 1.1 Le vide normatif

Contrairement au braille arabe (standardisé par l'UNESCO dans les années 1950), au braille français (codifié par le *Code braille français uniformisé*, CBFU, adopté par la Commission Évolution du Braille Français le 3 octobre 2008 et rendu obligatoire en France par arrêté du 17 août 2006), au braille anglais (Unified English Braille, UEB), au braille espagnol, au braille allemand, et à plus de 130 autres systèmes braille répertoriés dans le *World Braille Usage* (UNESCO 2013), **le braille berbère/amazigh n'apparaît dans aucune nomenclature internationale**. Les recherches sur le dspace UMMTO (Université Mouloud Mammeri de Tizi Ouzou), les bases académiques internationales, et les associations de déficients visuels algériennes n'ont révélé aucun travail de standardisation braille pour le kabyle en alphabet latin.

Un travail académique documenté existe pour le **tifinaghe marocain** (Yakoubi et al., 2016) : il propose un système braille complet pour les 33 caractères tifinaghes IRCAM, avec un indicateur de graphie spécifique, une règle d'emphase par ajout du point 6, et un convertisseur automatique avec clavier virtuel parlant. Cependant, **aucun équivalent n'existe pour l'alphabet latin berbère** (INALCO 1996) utilisé en Kabylie. De plus, le système tifinaghe-braille repose sur une logique de points propres au tifinaghe (fréquence, emphase par point 6) qui n'est pas directement transposable à l'alphabet latin kabyle sans adaptation.

Un article académique isolé mentionne un « convertisseur automatique de braille vers tifinaghe et vice-versa » (Ataa Allah & Frain, 2013), mais il ne semble pas avoir abouti à une norme publique ni à une table de mapping documentée pour l'alphabet latin.

### 1.2 Objectif de cette spec

Fournir une **spécification de transcription braille de référence** qui :

1. S'appuie sur le **CBFU** (*Code braille français uniformisé*) pour tous les caractères communs au français et au kabyle (22 lettres de base, ponctuation, chiffres, tiret).
2. Définisse un **mode indicateur** pour les 10 caractères spécifiques berbères (č, ḍ, ɛ, ǧ, ɣ, ḥ, ṛ, ṣ, ṭ, ẓ), documentés avec justification phonétique et en utilisant un mécanisme d'extension compatible avec le CBFU (modificateur 2, dot 5).
3. Établisse les **règles de normalisation préalable** (alignement sur la spec orthography) pour garantir que le texte source est canonique avant transcription.
4. Définisse les **formats de sortie** : Unicode Braille (U+2800–U+28FF), BRF (Braille Ready Format), et table Liblouis.
5. Documente les **barrières qualité** pour la production de documents braille kabyles.
6. S'intègre avec le stack kabyle existant (orthography, keyboard, G2P) comme couche de sortie tactile.

### 1.3 Principe directeur : ne pas réinventer la roue

Le braille est un **code d'encodage tactile**, pas une écriture autonome. La règle d'or de la conception braille est la **compatibilité ascendante** : tout lecteur braille francophone doit pouvoir lire le kabyle en braille sans apprentissage préalable des 22 lettres de base. Seuls les 10 caractères spécifiques nécessitent un apprentissage additionnel.

---

## 2. Inventaire des caractères braille kabyles

### 2.1 Base CBFU (22 lettres + ponctuation + chiffres)

Les caractères suivants sont transcrits **à l'identique** du CBFU grade 1 (non abrégé). Tout lecteur braille francophone les reconnaît instantanément.

| Caractère | Nom | Cellule Braille | Unicode | Dots |
|-----------|-----|-----------------|---------|------|
| a | A | ⠁ | U+2801 | 1 |
| b | B | ⠃ | U+2803 | 12 |
| c | C | ⠉ | U+2809 | 14 |
| d | D | ⠙ | U+2819 | 145 |
| e | E | ⠑ | U+2811 | 15 |
| f | F | ⠋ | U+280B | 124 |
| g | G | ⠛ | U+281B | 1245 |
| h | H | ⠓ | U+2813 | 125 |
| i | I | ⠊ | U+280A | 24 |
| j | J | ⠚ | U+281A | 245 |
| k | K | ⠅ | U+2805 | 13 |
| l | L | ⠇ | U+2807 | 123 |
| m | M | ⠍ | U+280D | 134 |
| n | N | ⠝ | U+281D | 1345 |
| o | O | ⠕ | U+2815 | 135 |
| p | P | ⠏ | U+280F | 1234 |
| q | Q | ⠟ | U+281F | 12345 |
| r | R | ⠗ | U+2817 | 1235 |
| s | S | ⠎ | U+280E | 234 |
| t | T | ⠞ | U+281E | 2345 |
| u | U | ⠥ | U+2825 | 136 |
| v | V | ⠧ | U+2827 | 1236 |
| w | W | ⠺ | U+283A | 2456 |
| x | X | ⠭ | U+282D | 1346 |
| y | Y | ⠽ | U+283D | 13456 |
| z | Z | ⠵ | U+2835 | 1356 |

**Note sur le v** : la lettre `v` n'est pas native à l'alphabet kabyle INALCO, mais elle est présente dans le CBFU et peut apparaître dans des emprunts français. Elle se transcrit `⠧`.

**Ponctuation (CBFU)** :

| Caractère | Nom | Cellule Braille | Unicode | Dots |
|-----------|-----|-----------------|---------|------|
| , | Virgule | ⠂ | U+2802 | 2 |
| ; | Point-virgule | ⠆ | U+2806 | 23 |
| : | Deux-points | ⠒ | U+2812 | 25 |
| . | Point | ⠲ | U+2832 | 256 |
| ? | Point d'interrogation | ⠦ | U+2826 | 236 |
| ! | Point d'exclamation | ⠖ | U+2816 | 235 |
| « | Guillemet ouvrant | ⠶ | U+2836 | 2356 |
| » | Guillemet fermant | ⠶ | U+2836 | 2356 |
| - | Tiret | ⠤ | U+2824 | 36 |
| ( | Parenthèse ouvrante | ⠐⠣ | U+2810 U+2823 | 5 + 126 |
| ) | Parenthèse fermante | ⠐⠜ | U+2810 U+281C | 5 + 345 |

**Source CBFU** : Dans l'immense majorité des cas, le CBFU emploie le symbole (points 2-3-5-6) pour représenter le guillemet ouvrant ou fermant, quel que soit le caractère typographique utilisé dans le document d'origine. cite🛠web_search:6#6:~:text=Dans l'immense majorité des cas, on emploie le symbole...quel que soit le caractere typographique utilisé dans le document d'origine.

**Chiffres (CBFU)** :

Le CBFU prescrit la **notation Antoine** comme système principal : le modificateur mathématique `⠠` (dot 6, U+2820) précède les caractères braille spécifiques aux chiffres. Cependant, la **notation Louis Braille** (indicateur numérique `⠼` suivi des lettres a–j) reste largement utilisée dans les outils internationaux et les contextes plurilingues. Les deux systèmes sont documentés ci-dessous.

**Notation Louis Braille** (compatibilité internationale, Liblouis) :

| Chiffre | Séquence Braille | Unicode |
|---------|------------------|---------|
| 1 | ⠼⠁ | U+283C U+2801 |
| 2 | ⠼⠃ | U+283C U+2803 |
| 3 | ⠼⠉ | U+283C U+2809 |
| 4 | ⠼⠙ | U+283C U+2819 |
| 5 | ⠼⠑ | U+283C U+2811 |
| 6 | ⠼⠋ | U+283C U+280B |
| 7 | ⠼⠛ | U+283C U+281B |
| 8 | ⠼⠓ | U+283C U+2813 |
| 9 | ⠼⠊ | U+283C U+280A |
| 0 | ⠼⠚ | U+283C U+281A |

**Notation Antoine** (CBFU obligatoire en France) :

| Chiffre | Séquence Braille | Dots |
|---------|------------------|------|
| 1 | ⠠⠡ | 6 + 16 |
| 2 | ⠠⠣ | 6 + 126 |
| 3 | ⠠⠩ | 6 + 146 |
| 4 | ⠠⠹ | 6 + 1456 |
| 5 | ⠠⠱ | 6 + 156 |
| 6 | ⠠⠫ | 6 + 1246 |
| 7 | ⠠⠻ | 6 + 12456 |
| 8 | ⠠⠳ | 6 + 1256 |
| 9 | ⠠⠪ | 6 + 246 |
| 0 | ⠠⠼ | 6 + 3456 |

**[À VALIDER]** : La table Liblouis kabyle devra préciser si elle adopte la notation Antoine (conformité CBFU France) ou la notation Louis (compatibilité internationale maximale).

**Majuscules** :

| Règle | Séquence Braille | Dots |
|-------|------------------|------|
| Majuscule simple | ⠨ | 46 |
| Majuscules multiples (sigle, mot entier) | ⠨⠨ | 46 + 46 |
| Début de passage majuscule (≥ 4 mots) | ⠘⠨ | 2-5 + 4-6 |
| Fin de passage majuscule | ⠨ devant le dernier mot | 46 |

**Source CBFU** : L'indicateur de majuscule simple (points 4-6) précède immédiatement le mot dont l'initiale ou dont toutes les lettres sont en majuscules. À partir de quatre mots consécutifs entièrement en majuscule, on place le symbole (points 2-5, 4-6) devant le premier mot et le symbole (points 4-6) devant le dernier mot. cite🛠web_search:2#2:~:text=L'indicateur de majuscule...points 4-6...À partir de quatre mots...points 2-5, 4-6...points 4-6

### 2.2 Caractères spécifiques berbères : mode indicateur

Les 10 caractères spécifiques au kabyle (INALCO 1996) n'ont pas d'équivalent direct en CBFU. Cette spécification définit un **mode indicateur** de transcription, compatible CBFU grade 1.

#### 2.2.1 Principe

Un **modificateur** CBFU standard — le **modificateur 2** (`⠐`, dot 5, U+2810) — précède la lettre de base la plus proche phonétiquement. Cela produit deux cellules braille par caractère spécial, mais garantit une **compatibilité totale** avec les lecteurs CBFU existants : un lecteur francophone peut, au pire, ignorer le modificateur et reconnaître la lettre de base.

**Justification du choix du modificateur 2** : Le CBFU définit le modificateur 2 (point 5) comme un symbole qui change la valeur du ou des caractères qui suivent immédiatement, notamment pour les symboles composés (©, °, §, ®, ™, etc.) et les lettres étrangères (§2.5). cite🛠web_search:2#2:~:text=Les modificateurs 1 et 2...servent à former des symboles composes... Le CBFU prévoit explicitement que « de nouveaux symboles composés pourront être créés ultérieurement à l'aide de ces modificateurs si nécessaire ». cite🛠web_search:3#5:~:text=De nouveaux symboles composes pourront etre crees ulterieurement a l'aide de ces modificateurs si necessaire. L'utilisation du modificateur 2 pour les lettres kabyles s'inscrit donc dans la logique d'extension du CBFU.

**Vérification des collisions** : Le tableau des symboles composés du CBFU (Tableau 3) liste les combinaisons existantes avec le modificateur 2. Les combinaisons retenues ci-dessous (`⠐⠙`, `⠐⠑`, `⠐⠛`, `⠐⠟`, `⠐⠓`, `⠐⠗`, `⠐⠎`, `⠐⠞`, `⠐⠵`) ne figurent pas dans le CBFU 2008 comme symboles assignés. cite🛠web_search:3#5:~:text=Tableau 3 Les symboles composees...

| Caractère | Phonème | Séquence Braille | Justification |
|-----------|---------|------------------|---------------|
| č | /t͡ʃ/ | ⠐⠉ (modificateur 2 + c) | c de base + modification. **[À VALIDER]** : `⠐⠉` n'est pas utilisé en CBFU grade 1, mais `⠐` + `⠉` (©) est une séquence documentée. Vérifier qu'aucun lecteur CBFU n'interprète `⠐⠉` comme © dans un contexte littéraire. |
| ḍ | /dˤ/ | ⠐⠙ (modificateur 2 + d) | d de base + modification. `⠐⠙` n'est pas assigné en CBFU. |
| ɛ | /ʕ/ | ⠐⠑ (modificateur 2 + e) | e de base + modification. `⠐⠑` n'est pas assigné en CBFU. |
| ǧ | /d͡ʒ/ | ⠐⠛ (modificateur 2 + g) | g de base + modification. `⠐⠛` n'est pas assigné en CBFU. |
| ɣ | /ɣ/ | ⠐⠟ (modificateur 2 + q) | Voir note ci-dessous : q est utilisé à la place de g pour éviter la collision avec ǧ. `⠐⠟` n'est pas assigné en CBFU. |
| ḥ | /ħ/ | ⠐⠓ (modificateur 2 + h) | h de base + modification. `⠐⠓` n'est pas assigné en CBFU. |
| ṛ | /rˤ/ | ⠐⠗ (modificateur 2 + r) | r de base + modification. **[À VALIDER]** : `⠐⠗` n'est pas assigné en CBFU, mais `⠐` + `⠗` (®) est une séquence documentée. Vérifier l'absence d'ambiguïté. |
| ṣ | /sˤ/ | ⠐⠎ (modificateur 2 + s) | s de base + modification. `⠐⠎` n'est pas assigné en CBFU. |
| ṭ | /tˤ/ | ⠐⠞ (modificateur 2 + t) | t de base + modification. **[À VALIDER]** : `⠐⠞` n'est pas assigné en CBFU, mais `⠐` + `⠞` (™) est une séquence documentée. Vérifier l'absence d'ambiguïté. |
| ẓ | /zˤ/ | ⠐⠵ (modificateur 2 + z) | z de base + modification. `⠐⠵` n'est pas assigné en CBFU. |

**Note sur ɣ (résolution de la collision ǧ/ɣ)** : phonétiquement, `ǧ` (/d͡ʒ/) et `ɣ` (/ɣ/) seraient tous deux les plus proches de `g`, ce qui produirait la même séquence (`⠐⠛`) pour deux caractères distincts. Cette spécification retient donc `⠐⠟` (modificateur 2 + q) pour `ɣ`, par analogie avec la touche `q` du KIM (*Kabyle Input Method*, voir `kabyle-keyboard-layout-spec.md` §4.2). **[À VALIDER]** : cette substitution est une proposition non testée auprès de locuteurs et de lecteurs braille kabyles ; elle est appliquée de façon cohérente dans le reste de cette spécification (§5.3, §6.1).

#### 2.2.2 Mode dédié (option avancée) — reporté

Une transcription en **une seule cellule braille** par caractère spécial kabyle serait souhaitable pour optimiser la vitesse de lecture et réduire le volume. Cependant, le CBFU grade 1 (6 points) utilise les 63 cellules non vides : les 26 lettres de base, 13 voyelles accentuées, et les symboles de ponctuation et de composition. cite🛠web_search:2#2:~:text=Caractère braille : chacune des 63 combinaisons de points qu'offre la cellule braille. Aucune cellule 6-dot n'est disponible sans entrer en collision avec un caractère CBFU existant (notamment les voyelles accentuées françaises : â=⠡, ê=⠣, î=⠩, ô=⠹, û=⠱, ë=⠫, ï=⠻, ü=⠳, œ=⠪, é=⠿, à=⠷, è=⠮, ù=⠾). cite🛠web_search:3#0:~:text=â, ⠡, 16...ê, ⠣, 126...î, ⠩, 146...ô, ⠹, 1456...û, ⠱, 156...ë, ⠫, 1246...ï, ⠻, 12456...ü, ⠳, 1256...œ, ⠪, 246...é, ⠿, 123456...à, ⠷, 12356...è, ⠮, 2346...ù, ⠾, 23456

Par conséquent, le mode dédié 6-dot est **hors périmètre** de cette version de la spécification. Une extension future pourra explorer :
- L'utilisation de cellules **8-dot** (plage U+2840–U+28FF, dots 7 et/ou 8), garanties hors périmètre CBFU 6-dot et supportées par les afficheurs braille informatiques modernes.
- La définition d'un grade 2 (abrégé) kabyle, qui libérerait potentiellement certaines cellules pour un usage dédié par redéfinition contextuelle.

---

## 3. Règles de transcription

### 3.1 Principe fondamental : 1 grapheme = 1 ou n cellules

Le braille kabyle est une **transcription graphemique**, pas une transcription phonétique. Chaque caractère de l'orthographe INALCO se transcrit en une ou plusieurs cellules braille selon le mode choisi. La spirantisation, l'allophonie vocalique, et les autres règles phonologiques (documentées dans `kabyle-g2p-specs.md`) **ne s'appliquent pas** au niveau braille. Le lecteur braille lit l'orthographe exacte.

**Exemple** : `tafat` se transcrit `⠞⠁⠋⠁⠞` (5 cellules) en mode base, quelle que soit la spirantisation phonétique réelle.

### 3.2 Géminées

Les géminées en kabyle (bb, dd, tt, kk, gg, ḍḍ, ṭṭ, ff, ll, mm, nn, rr, ṛṛ, ss, ṣṣ, zz, ẓẓ, cc, čč, ǧǧ, ɣɣ, ḥḥ, qq, ɛɛ, xx, ww, yy, pp, jj, hh) se transcritent comme **deux cellules identiques successives**. Aucun signe de gémination spécifique n'est requis.

| Orthographe | Mode Indicateur | Attestation (Bouamara, *Ussan di Tmurt*) |
|-------------|-------------------|------------------------------------------|
| tebbeẓ | ⠞⠑⠃⠃⠑⠐⠵ | « Mi tebbeẓ (...) asagem-nni amaynut » (elle presse) |
| tameṭṭut | ⠞⠁⠍⠑⠐⠞⠐⠞⠥⠞ | « tameṭṭut-is, ad tekk ala yiwet » (sa femme) |
| axxam | ⠁⠭⠭⠁⠍ | « d axxam n Ccix » (la maison) |

### 3.3 Tiret et clitiques

Le tiret `-` (U+002D) est transcrit `⠤` (dots 36, U+2824), identique au CBFU. Il est omniprésent en kabyle pour les clitiques préverbaux, la coordination, et les noms composés.

| Orthographe | Braille | Attestation (Bouamara, *Ussan di Tmurt*) |
|-------------|---------|------------------------------------------|
| wid-nni | ⠺⠊⠙⠤⠝⠝⠊ | « seg wid-nni yettɣuddun taddart » (ceux-là, clitique démonstratif -nni) |
| baba-s | ⠃⠁⠃⠁⠤⠎ | « Asmi yemmut baba-s » (son père, clitique possessif -s) |
| aɣrum-nsen | ⠁⠐⠟⠗⠥⠍⠤⠝⠎⠑⠝ | « aɣrum-nsen » (leur pain, clitique possessif -nsen + caractère spécial ɣ) |

### 3.4 Pas d'apostrophe

Conformément à `kabyle-orthography-specs.md` §6.4, le kabyle **n'utilise pas** l'apostrophe. Aucune cellule braille pour l'apostrophe n'est nécessaire. Si un texte source contient une apostrophe, elle doit être supprimée lors de la normalisation préalable (voir §4).

### 3.5 Espaces

L'espace ` ` (U+0020) se transcrit en `⠀` (U+2800, braille pattern blank). Les espaces multiples sont réduits à un seul `⠀` avant transcription.

### 3.6 Majuscules

L'indicateur de majuscule `⠨` (dot 46, U+2828) précède la lettre concernée. Pour un passage en majuscules (ex. acronyme, titre), l'indicateur double `⠨⠨` est utilisé pour les majuscules multiples, et le passage étendu utilise `⠘⠨` en début et `⠨` devant le dernier mot.

| Orthographe | Braille |
|-------------|---------|
| Taqbaylit | ⠨⠞⠁⠟⠃⠁⠽⠇⠊⠞ |
| Ɛemmi | ⠨⠐⠑⠍⠍⠊ |
| IRCAM | ⠘⠨⠊⠗⠉⠁⠍⠨ |

« Taqbaylit » est attesté tel quel, capitalisé, dans *Ussan di Tmurt* (Bouamara). « Ɛemmi » est une application de la règle de majuscule ci-dessus au mot attesté « ɛemmi » (§6.1) en position de début de phrase — cette forme capitalisée précise n'est pas elle-même citée du texte source, mais résulte de l'application mécanique de la règle d'écriture standard.

---

## 4. Normalisation préalable

Avant toute transcription braille, le texte source doit être normalisé conformément à `kabyle-orthography-specs.md`. Les étapes suivantes sont **obligatoires** :

| Étape | Règle | Référence |
|-------|-------|-----------|
| N1 | **NFC obligatoire** — formes précomposées uniquement | Orthography §8.1 R3 |
| N2 | **Rejet des faux amis** — ε, Σ, γ, Γ, Ԑ, ԑ, ğ, ĝ, etc. | Orthography §3 |
| N3 | **Substitution des formes obsolètes** — ţ → ṭ, z̧ → ẓ | Orthography §12 |
| N4 | **Espace simple uniquement** — U+0020 seul | Orthography §6.1 |
| N5 | **Pas d'apostrophe** — supprimer ou réviser | Orthography §6.4 |
| N6 | **Pas de diacritiques français** — á→a, é→e, etc. | Orthography §4 |
| N7 | **Validation de l'inventaire** — seuls les 33 caractères + ponctuation + tiret + chiffres sont autorisés | Orthography §2 |

**Conséquence** : un convertisseur braille kabyle conforme **ne transcrit pas** du texte non normalisé. Il doit d'abord passer par le pipeline de normalisation ou rejeter le texte avec un diagnostic d'erreur.

---

## 5. Formats de sortie

### 5.1 Unicode Braille (affichage à l'écran)

**Plage** : U+2800 à U+28FF (256 caractères braille).

**Utilisation** : affichage sur écran, stockage en base de données, échange entre applications. Chaque cellule braille est un caractère Unicode indépendant.

**Exemple** : `ɣer` en mode indicateur → `⠐⠟⠑⠗` (3 caractères Unicode).

### 5.2 BRF — Braille Ready Format

**Format** : fichier texte ASCII où chaque cellule braille est encodée selon une table de translation spécifique à la plateforme (généralement North American Computer Braille Code, NACB).

**Utilisation** : envoi vers des embosseuses braille (imprimantes tactiles). Le BRF est le format standard de l'industrie braille.

**[À VALIDER]** : Le BRF utilise-t-il une table NACB spécifique pour les caractères étendus ? Les 10 caractères kabyles nécessiteront une extension de la table BRF standard.

### 5.3 Table Liblouis

**Format** : fichier `.ctb` ou `.utb` (uncontracted translation table) pour la bibliothèque Liblouis.

**Utilisation** : intégration avec les lecteurs d'écran (NVDA, Orca), les embosseuses, et les outils de transcription automatique.

**Structure minimale** :

```
# kabyle.ctb — Table braille kabyle (mode indicateur)
# Basé sur CBFU + extensions berbères via modificateur 2
# Auteur: kabyle-specs
# Version: 0.2-draft

include fr-bfu-comp6.utb  # Inclusion de la base CBFU grade 1

# Mode Indicateur (par défaut)
# Utilisation du modificateur 2 (dot 5, U+2810) comme préfixe
# pour les caractères spécifiques berbères, conformément au
# mécanisme d'extension du CBFU §1.9 et §2.5.

always č ⠐⠉  # c caron — [À VALIDER] collision potentielle avec ©
always ḍ ⠐⠙  # d point souscrit
always ɛ ⠐⠑  # e ouvert
always ǧ ⠐⠛  # g caron
always ɣ ⠐⠟  # gamma — modificateur 2 + q, résolution de la collision avec ǧ
always ḥ ⠐⠓  # h point souscrit
always ṛ ⠐⠗  # r point souscrit — [À VALIDER] collision potentielle avec ®
always ṣ ⠐⠎  # s point souscrit
always ṭ ⠐⠞  # t point souscrit — [À VALIDER] collision potentielle avec ™
always ẓ ⠐⠵  # z point souscrit
```

**[À VALIDER]** : le mapping `ɣ` → `⠐⠟` (voir §2.2.1) est une proposition non testée qui reste à valider auprès d'experts CBFU et de locuteurs kabyles déficients visuels.

---

## 6. Exemples de transcription complète

### 6.1 Mode Indicateur (par défaut)

Tous les exemples ci-dessous sont **directement attestés** dans *Ussan di Tmurt* (Kamal Bouamara, traduction de *Jours de Kabylie* de Mouloud Feraoun), à l'exception de Taqbaylit et Tmurt qui figurent respectivement dans le corps du texte et dans le titre de l'ouvrage.

| Orthographe | Braille | Nombre de cellules | Attestation (Bouamara, *Ussan di Tmurt*) |
|-------------|---------|---------------------|-------------------------------------------|
| Tmurt | ⠨⠞⠍⠥⠗⠞ | 6 | Titre de l'ouvrage : « le Pays » (la Kabylie) |
| Taqbaylit | ⠨⠞⠁⠟⠃⠁⠽⠇⠊⠞ | 10 | « Mačči d Taqbaylit i am-d-ǧǧan ! » |
| aɣrum | ⠁⠐⠟⠗⠥⠍ | 6 | « ɣer wanda i d-ttsewwiren medden aɣrum-nsen » (le pain) |
| aḍar | ⠁⠐⠙⠁⠗ | 5 | « netta d uccen iɣeẓẓan aḍar-is » (le pied) |
| iḥemmel | ⠊⠐⠓⠑⠍⠍⠑⠇ | 8 | « iḥemmel adebder-a » (il aime) |
| aṭas | ⠁⠐⠞⠁⠎ | 5 | « Aṭas n smayem i daɣ-yezdin » (beaucoup) |
| laẓ | ⠇⠁⠐⠵ | 4 | « Yettili laẓ d usemmiḍ » (la faim) |
| ɛemmi | ⠐⠑⠑⠍⠍⠊ | 6 | « senṭeḍen-aɣ-d ɛemmi ara yawin i xemsa » (oncle paternel) |
| iǧeǧǧigen | ⠊⠐⠛⠑⠐⠛⠐⠛⠊⠛⠑⠝ | 12 | « akk d yiǧeǧǧigen icebḥanen » (les fleurs — geminée ǧǧ) |
| ṛeggmen | ⠐⠗⠑⠛⠛⠍⠑⠝ | 8 | « neɣ ad ṛeggmen » (ils insultent) |
| ṣbeḥ | ⠐⠎⠃⠑⠐⠓ | 6 | « ṣbeḥ zik neɣ deg yiḍ » (le matin) |
| ččuren | ⠐⠉⠐⠉⠥⠗⠑⠝ | 8 | « Ččuren-d merra yiẓrawen-nneɣ d imeṭṭawen » (ils se sont remplis — geminée čč) |
| baba-s | ⠃⠁⠃⠁⠤⠎ | 6 | « Asmi yemmut baba-s » (son père — exemple de clitique avec tiret) |

---

## 7. Barrières qualité

### 7.1 Pipeline de validation obligatoire

Tout document braille kabyle produit par un système automatique doit passer par les barrières suivantes :

| Étape | Contrôle | Seuil / Action |
|-------|----------|----------------|
| B1 | Texte source normalisé | Passage obligatoire par le pipeline §4 |
| B2 | Caractères braille valides | Seules les cellules définies dans cette spec sont autorisées |
| B3 | Pas de cellules CBFU ambiguës | Vérifier qu'aucune cellule de grade 2 (abrégé) n'est injectée par erreur |
| B4 | Longueur cohérente | Le nombre de cellules braille doit être ≥ au nombre de caractères source (mode indicateur) |
| B5 | Géminées correctes | Vérifier que les digrammes bb, dd, tt, etc. produisent bien deux cellules identiques |
| B6 | Tirets préservés | Vérifier que les clitiques avec tiret conservent le `⠤` |

### 7.2 Tests de non-régression CBFU

Un texte français inséré dans un document kabyle (citation, emprunt) doit se transcrire **à l'identique** en CBFU grade 1. Les règles kabyles ne doivent pas affecter le français.

| Texte français | Braille attendu | Test |
|----------------|-----------------|------|
| Bonjour | ⠨⠃⠕⠝⠚⠕⠥⠗ | CBFU standard |
| café | ⠉⠁⠋⠿ | CBFU standard (é = ⠿, dots 123456) |
| œuvre | ⠪⠧⠗⠑ | CBFU standard (œ = ⠪, dots 246) |
| à | ⠷ | CBFU standard (à = ⠷, dots 12356) |

**Note** : Les voyelles accentuées françaises (à, â, ç, é, è, ê, ë, î, ï, ô, œ, ù, ü) ont des cellules CBFU fixes et ne doivent en aucun cas être réassignées à des caractères kabyles. cite🛠web_search:3#0:~:text=à, ⠷, 12356...â, ⠡, 16...ç, ⠯, 12346...é, ⠿, 123456...è, ⠮, 2346...ê, ⠣, 126...ë, ⠫, 1246...î, ⠩, 146...ï, ⠻, 12456...ô, ⠹, 1456...œ, ⠪, 246...ù, ⠾, 23456...ü, ⠳, 1256

---

## 8. Intégration avec le stack kabyle

### 8.1 Dépendances

Cette spécification dépend des specs suivantes du stack kabyle :

| Spec | Dépendance | Justification |
|------|------------|---------------|
| `kabyle-orthography-specs.md` | **Obligatoire** | Inventaire des 33 caractères, normalisation, rejet des faux amis |
| `kabyle-keyboard-layout-spec.md` | Informative | Logique du KIM (touche morte `^`) pour l'assignation des caractères spéciaux |
| `kabyle-g2p-specs.md` | Informative | Phonèmes sous-jacents pour la justification des mappings |

### 8.2 Ordre dans le pipeline

```
Texte brut
    ↓
[kabyle-orthography-specs.md] → Normalisation (NFC, faux amis, espaces)
    ↓
[kabyle-braille-spec.md] → Transcription braille (mode indicateur)
    ↓
Unicode Braille / BRF / Liblouis table
    ↓
Affichage écran / Embosseuse / Lecteur d'écran
```

---

## 9. Limites connues et feuille de route

| ID | Limite | Statut |
|----|--------|--------|
| L1 | **Aucune validation par expert braille** — cette spec est une proposition théorique. Aucun expert CBFU ni utilisateur déficient visuel kabyle n'a validé l'ergonomie tactile des cellules proposées. | **[À VALIDER]** — Contact expert requis |
| L2 | **Aucun test sur embosseuse** — les cellules proposées n'ont pas été testées sur du papier braille. La lisibilité tactile de certaines combinaisons reste à vérifier. | **[À VALIDER]** |
| L3 | **Collision ɣ/ǧ en mode indicateur** — toutes deux utilisent la lettre g de base. La solution `⠐⠟` (q) pour ɣ est une proposition non testée. | **[À VALIDER]** |
| L4 | **Collisions potentielles modificateur 2** — `⠐⠉` (©), `⠐⠗` (®), `⠐⠞` (™) existent en CBFU. Les combinaisons kabyles correspondantes doivent être validées pour éviter toute ambiguïté contextuelle. | **[À VALIDER]** — Vérification CBFU complète requise |
| L5 | **Pas de grade 2 (abrégé)** — cette spec ne définit pas de système abrégé (contractions, logogrammes) pour le kabyle. Un grade 2 kabyle serait souhaitable à long terme pour réduire le volume braille. | Extension future |
| L6 | **Pas de mode dédié 6-dot** — toutes les cellules 6-dot sont utilisées par le CBFU grade 1. Un mode dédié nécessitera du braille 8-dot ou une réforme du CBFU. | Extension future |
| L7 | **Pas de support Tifinagh** — cette spec ne couvre que l'alphabet latin berbère (INALCO). Une spec braille pour le néo-tifinagh (IRCAM) nécessiterait un travail séparé. | Spec séparée souhaitable |
| L8 | **Pas de test avec Liblouis** — la table Liblouis proposée en §5.3 n'a pas été compilée ni testée. | Implémentation requise |
| L9 | **Indicateur de majuscule pour caractères spéciaux** — `⠨` + `⠐` + lettre = 3 cellules pour une majuscule spéciale en mode indicateur. C'est lourd. Un mode dédié 8-dot résoudrait ce problème (`⠨` + cellule dédiée = 2 cellules). | Optimisation future |

---

## 10. Conclusion

Cette spécification établit la première base de transcription braille pour le kabyle (Taqbaylit), en s'appuyant sur le CBFU comme fondement compatible et en proposant un mode indicateur via le modificateur 2 (dot 5) pour les 10 caractères spécifiques berbères. Elle s'intègre comme couche de sortie tactile dans le stack kabyle existant, en aval de la normalisation orthographique.

La principale avancée est la **formalisation d'un mapping complet** de l'alphabet INALCO vers le braille, documenté avec justification phonétique et en adéquation avec les mécanismes d'extension du CBFU. La principale limite est l'**absence de validation par des experts braille et des utilisateurs déficients visuels kabyles** — cette spec est une **proposition de recherche** en attente de validation communautaire.

L'adoption de cette spécification par les outils de transcription (Liblouis, embosseuses, lecteurs d'écran) et sa validation par la communauté braille francophone et kabyle constitue la prochaine étape critique.

---

## Références

1. **Chaker, Salem** (1996). *Propositions pour la notation usuelle à base latine du berbère*. INALCO / Centre de Recherche Berbère, Paris. https://www.centrederechercheberbere.fr/tl_files/doc-pdf/notation.pdf
2. **CBFU** (2008). *Code braille français uniformisé pour la transcription des textes imprimés*. Commission Évolution du Braille Français (CEBF), 2ᵉ édition, septembre 2008 ; application obligatoire en France par arrêté du 17 août 2006. https://www.pharmabraille.com/wp-content/uploads/2015/01/CBFU_edition_internationale.pdf
3. **UNESCO** (2013). *World Braille Usage*. 3rd edition. https://www.unesco.org/
4. **Liblouis** (2026). *Open-source braille translator*. https://liblouis.io/
5. **Unicode Consortium** (2026). *Unicode Standard, Version 16.0* — Braille Patterns (U+2800–U+28FF). https://unicode.org/
6. **Mokraoui, Athmane (boffire)** (2026). *kabyle-orthography-specs.md*. https://kabyle-specs.github.io/
7. **Mokraoui, Athmane (boffire)** (2026). *kabyle-keyboard-layout-spec.md*. https://kabyle-specs.github.io/
8. **Mokraoui, Athmane (boffire)** (2026). *kabyle-g2p-specs.md*. https://kabyle-specs.github.io/
9. **Yakoubi, N., Frain, J., Ataa Allah, F.** (2016). *Convertisseur numérique : Tifinaghe – Braille*. TICAM 2016, Centre des Etudes Informatiques, Systèmes d'Information et de Communication, IRCAM, Maroc.
10. **Ataa Allah, F. & Frain, J.** (2013). *Amazigh Converter based on WordprocessingML*. Actes de la 6ᵉ édition de Language & Technology Conference (LTC 2013), Poznań, Pologne.
11. **Dallet, Jean-Marie** (1982). *Dictionnaire kabyle-français: parler des At Mengellat, Algérie*. SELAF, Paris.
12. **Naït-Zerrad, Kamal** (2001). *Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit*. Karthala, Paris.
13. **Feraoun, Mouloud / Bouamara, Kamal (trad.)** (1998). *Ussan di Tmurt* (traduction kabyle de *Jours de Kabylie*, Mouloud Feraoun, 1954). Alger. Source de tous les exemples de mots attestés en §3.2, §3.3, §3.6 et §6.
14. **Wikipedia** (2026). *French Braille* — table des correspondances CBFU grade 1. https://en.wikipedia.org/wiki/French_Braille
15. **PharmaBraille** (2016). *France Braille Code* — table de référence CBFU 2ᵉ édition. https://www.pharmabraille.com/braille-codes/france-braille-code/

---

*Document rédigé dans le cadre du développement des ressources d'accessibilité pour la langue kabyle. Les zones nécessitant une validation par des experts braille ou des locuteurs natifs déficients visuels sont signalées [À VALIDER].*
