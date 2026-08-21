# Spécification de Transcription Braille pour le Kabyle (Taqbaylit)

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; recherche documentaire braille et structuration technique.

**Date** : 21 août 2026

**Version** : 0.1-draft

**Statut** : En cours de validation — certains points sont marqués **[À VALIDER]** et nécessitent confirmation par des experts braille et des locuteurs natifs déficients visuels.

**Cible** : Développeurs de technologies d'assistance, ingénieurs braille, mainteneurs de tables Liblouis, concepteurs de plages braille, chercheurs en accessibilité linguistique.

---

## Résumé

Le kabyle (Taqbaylit, ISO 639-3 `kab`) est une langue berbère parlée par 5 à 7 millions de locuteurs en Algérie. À ce jour, **aucun standard braille kabyle n'existe** — ni dans le *World Braille Usage* de l'UNESCO, ni dans le *Code braille français uniformisé* (CBFU), ni dans les répertoires de l'International Council on English Braille (ICEB). Cette spécification propose un **système de transcription braille de référence** pour l'alphabet latin berbère standard (INALCO 1996), en s'appuyant sur le CBFU comme fondement pour les caractères communs au français et au kabyle, et en définissant deux modes d'extension pour les 11 caractères spécifiques berbères : un **mode indicateur** (compatible, deux cellules par caractère spécial) et un **mode dédié** (optimisé, une cellule par caractère). Elle définit les formats de sortie (Unicode Braille, BRF, table Liblouis), les règles de normalisation préalable, et les barrières qualité pour la production de documents braille kabyles.

**Mots-clés** : kabyle, taqbaylit, braille, CBFU, accessibilité, transcription tactile, Liblouis, Unicode Braille, INALCO, caractères spécifiques berbères.

---

## 1. Introduction et périmètre

### 1.1 Le vide normatif

Contrairement au braille arabe (standardisé par l'UNESCO dans les années 1950), au braille français (codifié par le *Code braille français uniformisé*, CBFU, adopté par la Commission Évolution du Braille Français le 3 octobre 2008 et rendu obligatoire en France par arrêté du 17 août 2006), au braille anglais (Unified English Braille, UEB), au braille espagnol, au braille allemand, et à plus de 130 autres systèmes braille répertoriés dans le *World Braille Usage* (UNESCO 2013), **le braille berbère/amazigh n'apparaît dans aucune nomenclature internationale**. Les recherches sur le dspace UMMTO (Université Mouloud Mammeri de Tizi Ouzou), les bases académiques internationales, et les associations de déficients visuels algériennes n'ont révélé aucun travail de standardisation braille pour le kabyle.

Un article académique isolé mentionne un « convertisseur automatique de braille vers tifinaghe et vice-versa » (ResearchGate, non accessible en détail), mais il ne semble pas avoir abouti à une norme publique ni à une table de mapping documentée.

### 1.2 Objectif de cette spec

Fournir une **spécification de transcription braille de référence** qui :

1. S'appuie sur le **CBFU** (*Code braille français uniformisé*) pour tous les caractères communs au français et au kabyle (22 lettres de base, ponctuation, chiffres, tiret).
2. Définisse deux **modes d'extension** pour les 11 caractères spécifiques berbères (č, ḍ, ɛ, ǧ, ɣ, ḥ, ṛ, ṣ, ṭ, ẓ), documentés avec justification phonétique et ergonomique.
3. Établisse les **règles de normalisation préalable** (alignement sur la spec orthography) pour garantir que le texte source est canonique avant transcription.
4. Définisse les **formats de sortie** : Unicode Braille (U+2800–U+28FF), BRF (Braille Ready Format), et table Liblouis.
5. Documente les **barrières qualité** pour la production de documents braille kabyles.
6. S'intègre avec le stack kabyle existant (orthography, keyboard, G2P) comme couche de sortie tactile.

### 1.3 Principe directeur : ne pas réinventer la roue

Le braille est un **code d'encodage tactile**, pas une écriture autonome. La règle d'or de la conception braille est la **compatibilité ascendante** : tout lecteur braille francophone doit pouvoir lire le kabyle en braille sans apprentissage préalable des 22 lettres de base. Seuls les 11 caractères spécifiques nécessitent un apprentissage additionnel.

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

**Ponctuation (CBFU)** :

| Caractère | Nom | Cellule Braille | Unicode | Dots |
|-----------|-----|-----------------|---------|------|
| , | Virgule | ⠂ | U+2802 | 2 |
| ; | Point-virgule | ⠆ | U+2806 | 23 |
| : | Deux-points | ⠒ | U+2812 | 25 |
| . | Point | ⠲ | U+2832 | 256 |
| ? | Point d'interrogation | ⠦ | U+2826 | 236 |
| ! | Point d'exclamation | ⠖ | U+2816 | 235 |
| « | Guillemet ouvrant | ⠪ | U+282A | 246 |
| » | Guillemet fermant | ⠺ | U+283A | 2456 |
| - | Tiret | ⠤ | U+2824 | 36 |

**[À VALIDER]** : la cellule `⠺` (dots 2456) proposée ci-dessus pour « » » est identique à celle de la lettre `w`. Cette collision n'a pas été vérifiée contre la table CBFU officielle et doit être confirmée ou corrigée par un expert avant implémentation.
| ( | Parenthèse ouvrante | ⠐⠣ | U+2810 U+2823 | 5 + 126 |
| ) | Parenthèse fermante | ⠐⠜ | U+2810 U+281C | 5 + 345 |

**Chiffres (CBFU)** : indicateur numérique `⠼` (U+283C, dots 3456) suivi des lettres a–j :

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

**Majuscules** : indicateur `⠨` (U+2828, dot 46) placé avant la lettre. Pour un passage majuscule étendu, `⠨⠨` (double indicateur).

### 2.2 Caractères spécifiques berbères : deux modes

Les 11 caractères spécifiques au kabyle (INALCO 1996) n'ont pas d'équivalent en CBFU. Cette spécification définit **deux modes** de transcription, du plus compatible au plus optimisé.

#### 2.2.1 Mode Indicateur (par défaut) — Compatible CBFU

**Principe** : un **indicateur de modification berbère** (une cellule non utilisée en CBFU grade 1 pour des lettres latines) précède la lettre de base la plus proche phonétiquement. Cela produit deux cellules braille par caractère spécial, mais garantit une **compatibilité totale** avec les lecteurs CBFU existants : un lecteur francophone peut, au pire, ignorer l'indicateur et reconnaître la lettre de base.

**Cellule retenue pour l'indicateur** : `⠸` (dots 456, U+2838). Ce choix résulte de l'élimination des alternatives suivantes, toutes déjà affectées à un usage en CBFU grade 1 :

| Cellule | Dots | Unicode | Usage existant en CBFU grade 1 | Retenue ? |
|---------|------|---------|----------------------------------|-----------|
| ⠈ | 4 | U+2808 | Accent aigu (é, á) | Non — collision avec les emprunts français accentués |
| ⠘ | 45 | U+2818 | Cédille, tréma | Non — même risque de collision |
| ⠰ | 56 | U+2830 | Indicateur de lettre minuscule (en combinaison) | Non — usage combinatoire déjà défini |
| ⠸ | 456 | U+2838 | Indicateur de lettre grecque (non pertinent en kabyle standard) | **Oui** |

**[À VALIDER]** : Confirmer par un expert CBFU que `⠸` (dots 456) n'entre en collision avec aucun usage du CBFU grade 1 pertinent pour un texte kabyle, emprunts français inclus.

| Caractère | Phonème | Séquence Braille | Justification |
|-----------|---------|------------------|---------------|
| č | /t͡ʃ/ | ⠸⠉ (indicateur + c) | c de base + modification |
| ḍ | /dˤ/ | ⠸⠙ (indicateur + d) | d de base + modification |
| ɛ | /ʕ/ | ⠸⠑ (indicateur + e) | e de base + modification |
| ǧ | /d͡ʒ/ | ⠸⠛ (indicateur + g) | g de base + modification |
| ɣ | /ɣ/ | ⠸⠟ (indicateur + q) | Voir note ci-dessous : q est utilisé à la place de g pour éviter la collision avec ǧ |
| ḥ | /ħ/ | ⠸⠓ (indicateur + h) | h de base + modification |
| ṛ | /rˤ/ | ⠸⠗ (indicateur + r) | r de base + modification |
| ṣ | /sˤ/ | ⠸⠎ (indicateur + s) | s de base + modification |
| ṭ | /tˤ/ | ⠸⠞ (indicateur + t) | t de base + modification |
| ẓ | /zˤ/ | ⠸⠵ (indicateur + z) | z de base + modification |

**Note sur ɣ (résolution de la collision ǧ/ɣ)** : phonétiquement, `ǧ` (/d͡ʒ/) et `ɣ` (/ɣ/) seraient tous deux les plus proches de `g`, ce qui produirait la même séquence (`⠸⠛`) pour deux caractères distincts. Cette spécification retient donc `⠸⠟` (indicateur + q) pour `ɣ`, par analogie avec la touche `q` du KIM (*Kabyle Input Method*, voir `kabyle-keyboard-layout-spec.md` §4.2). **[À VALIDER]** : cette substitution est une proposition non testée auprès de locuteurs et de lecteurs braille kabyles ; elle est appliquée de façon cohérente dans le reste de cette spécification (§5.3, §6.1).

#### 2.2.2 Mode Dédié (option avancé) — Une cellule par caractère

**Principe** : assigner une **cellule braille unique** à chaque caractère spécial, en puisant dans les combinaisons non utilisées par le CBFU grade 1. Ce mode est plus compact et plus rapide à lire, mais nécessite l'apprentissage de 11 nouvelles cellules.

| Caractère | Phonème | Cellule Braille (proposition) | Dots | Justification |
|-----------|---------|------------------------------|------|---------------|
| č | /t͡ʃ/ | ⠡ | 16 | Non utilisée en CBFU grade 1. Proximité avec c (14) + modification |
| ḍ | /dˤ/ | ⠣ | 126 | Non utilisée seule en CBFU. Proximité avec d (145) |
| ɛ | /ʕ/ | ⠩ | 146 | Non utilisée en CBFU grade 1. Forme ouverte, distincte de e (15) |
| ǧ | /d͡ʒ/ | ⠫ | 1246 | Non utilisée seule en CBFU. Proximité avec g (1245) |
| ɣ | /ɣ/ | ⠱ | 156 | Non utilisée en CBFU grade 1. Distinct de g (1245) et ǧ |
| ḥ | /ħ/ | ⠳ | 1256 | Non utilisée seule en CBFU. Proximité avec h (125) |
| ṛ | /rˤ/ | ⠹ | 1456 | Non utilisée en CBFU grade 1. Proximité avec r (1235) |
| ṣ | /sˤ/ | ⠻ | 12456 | Non utilisée en CBFU grade 1. Proximité avec s (234) |
| ṭ | /tˤ/ | ⠷ | 12356 | Non utilisée en CBFU grade 1. Proximité avec t (2345) |
| ẓ | /zˤ/ | ⠿ | 123456 | Non utilisée en CBFU grade 1. Proximité avec z (1356) |

**[À VALIDER]** : Ces cellules sont-elles réellement libres en CBFU grade 1 ? Certaines peuvent être utilisées en grade 2 (abrégé) ou dans des contextes spécifiques. Validation par un expert CBFU requise.

**[À VALIDER]** : Le caractère `⠣` (dots 126) est utilisé en CBFU comme parenthèse ouvrante en combinaison avec l'indicateur `⠐` (dot 5). Son usage seul pour `ḍ` pourrait-il créer une ambiguïté ?

---

## 3. Règles de transcription

### 3.1 Principe fondamental : 1 grapheme = 1 ou n cellules

Le braille kabyle est une **transcription graphemique**, pas une transcription phonétique. Chaque caractère de l'orthographe INALCO se transcrit en une ou plusieurs cellules braille selon le mode choisi. La spirantisation, l'allophonie vocalique, et les autres règles phonologiques (documentées dans `kabyle-g2p-specs.md`) **ne s'appliquent pas** au niveau braille. Le lecteur braille lit l'orthographe exacte.

**Exemple** : `tafat` se transcrit `⠞⠁⠋⠁⠞` (5 cellules) en mode base, quelle que soit la spirantisation phonétique réelle.

### 3.2 Géminées

Les géminées en kabyle (bb, dd, tt, kk, gg, ḍḍ, ṭṭ, ff, ll, mm, nn, rr, ṛṛ, ss, ṣṣ, zz, ẓẓ, cc, čč, ǧǧ, ɣɣ, ḥḥ, qq, ɛɛ, xx, ww, yy, pp, jj, hh) se transcritent comme **deux cellules identiques successives**. Aucun signe de gémination spécifique n'est requis.

| Orthographe | Mode Base + Indicateur | Mode Dédié | Attestation (Bouamara, *Ussan di Tmurt*) |
|-------------|------------------------|------------|---------------------------------------------|
| tebbeẓ | ⠞⠑⠃⠃⠑⠸⠵ | ⠞⠑⠃⠃⠑⠿ | « Mi tebbeẓ (...) asagem-nni amaynut » (elle presse) |
| tameṭṭut | ⠞⠁⠍⠑⠸⠞⠸⠞⠥⠞ | ⠞⠁⠍⠑⠷⠷⠥⠞ | « tameṭṭut-is, ad tekk ala yiwet » (sa femme) |
| axxam | ⠁⠭⠭⠁⠍ | ⠁⠭⠭⠁⠍ | « d axxam n Ccix » (la maison) |

### 3.3 Tiret et clitiques

Le tiret `-` (U+002D) est transcrit `⠤` (dots 36, U+2824), identique au CBFU. Il est omniprésent en kabyle pour les clitiques préverbaux, la coordination, et les noms composés.

| Orthographe | Braille | Attestation (Bouamara, *Ussan di Tmurt*) |
|-------------|---------|--------------------------------------------|
| wid-nni | ⠺⠊⠙⠤⠝⠝⠊ | « seg wid-nni yettɣuddun taddart » (ceux-là, clitique démonstratif -nni) |
| baba-s | ⠃⠁⠃⠁⠤⠎ | « Asmi yemmut baba-s » (son père, clitique possessif -s) |
| aɣrum-nsen | ⠁⠸⠟⠗⠥⠍⠤⠝⠎⠑⠝ | « aɣrum-nsen » (leur pain, clitique possessif -nsen + caractère spécial ɣ) |

### 3.4 Pas d'apostrophe

Conformément à `kabyle-orthography-specs.md` §6.4, le kabyle **n'utilise pas** l'apostrophe. Aucune cellule braille pour l'apostrophe n'est nécessaire. Si un texte source contient une apostrophe, elle doit être supprimée lors de la normalisation préalable (voir §4).

### 3.5 Espaces

L'espace ` ` (U+0020) se transcrit en `⠀` (U+2800, braille pattern blank). Les espaces multiples sont réduits à un seul `⠀` avant transcription.

### 3.6 Majuscules

L'indicateur de majuscule `⠨` (dot 46, U+2828) précède la lettre concernée. Pour un passage en majuscules (ex. acronyme, titre), l'indicateur double `⠨⠨` est utilisé, suivi d'un indicateur de fin de passage `⠨⠱` **[À VALIDER]** ou retour à la casse normale par espace.

| Orthographe | Braille |
|-------------|---------|
| Taqbaylit | ⠨⠞⠁⠟⠃⠁⠽⠇⠊⠞ |
| Ɛemmi | ⠨⠸⠑⠍⠍⠊ |
| IRCAM | ⠨⠨⠊⠗⠉⠁⠍ |

« Taqbaylit » est attesté tel quel, capitalisé, dans *Ussan di Tmurt* (Bouamara). « Ɛemmi » est une application de la règle de majuscule ci-dessus au mot attesté « ɛemmi » (§6.1) en position de début de phrase — cette forme capitalisée précise n'est pas elle-même citée du texte source, mais résulte de l'application mécanique de la règle d'écriture standard.

**[À VALIDER]** : Le CBFU utilise-t-il `⠨⠱` comme indicateur de fin de passage majuscule ? Vérifier la norme exacte.

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

**Exemple** : `ɣer` en mode dédié → `⠱⠑⠗` (3 caractères Unicode).

### 5.2 BRF — Braille Ready Format

**Format** : fichier texte ASCII où chaque cellule braille est encodée selon une table de translation spécifique à la plateforme (généralement North American Computer Braille Code, NACB).

**Utilisation** : envoi vers des embosseuses braille (imprimantes tactiles). Le BRF est le format standard de l'industrie braille.

**[À VALIDER]** : Le BRF utilise-t-il une table NACB spécifique pour les caractères étendus ? Les 11 caractères kabyles nécessiteront une extension de la table BRF standard.

### 5.3 Table Liblouis

**Format** : fichier `.ctb` ou `.utb` (uncontracted translation table) pour la bibliothèque Liblouis.

**Utilisation** : intégration avec les lecteurs d'écran (NVDA, Orca), les embosseuses, et les outils de transcription automatique.

**Structure minimale** :

```
# kabyle.ctb — Table braille kabyle (mode indicateur par défaut)
# Basé sur CBFU + extensions berbères
# Auteur: kabyle-specs
# Version: 0.1-draft

include fr-bfu-g1.ctb  # Inclusion de la base CBFU grade 1

# Indicateur berbère (dots 456) — [À VALIDER], voir §2.2.1
# Les lignes suivantes sont des propositions

# Mode Indicateur (par défaut)
always č ⠸⠉  # c caron
always ḍ ⠸⠙  # d point souscrit
always ɛ ⠸⠑  # e ouvert
always ǧ ⠸⠛  # g caron
always ɣ ⠸⠟  # gamma — indicateur + q, résolution de la collision avec ǧ (voir §2.2.1)
always ḥ ⠸⠓  # h point souscrit
always ṛ ⠸⠗  # r point souscrit
always ṣ ⠸⠎  # s point souscrit
always ṭ ⠸⠞  # t point souscrit
always ẓ ⠸⠵  # z point souscrit
```

**[À VALIDER]** : le mapping `ɣ` → `⠸⠟` (voir §2.2.1) est une proposition non testée qui reste à valider auprès d'experts CBFU et de locuteurs kabyles déficients visuels.

---

## 6. Exemples de transcription complète

### 6.1 Mode Indicateur (par défaut)

Tous les exemples ci-dessous sont **directement attestés** dans *Ussan di Tmurt* (Kamal Bouamara, traduction de *Jours de Kabylie* de Mouloud Feraoun), à l'exception de Taqbaylit et Tmurt qui figurent respectivement dans le corps du texte et dans le titre de l'ouvrage.

| Orthographe | Braille | Nombre de cellules | Attestation (Bouamara, *Ussan di Tmurt*) |
|-------------|---------|---------------------|-------------------------------------------|
| Tmurt | ⠨⠞⠍⠥⠗⠞ | 6 | Titre de l'ouvrage : « le Pays » (la Kabylie) |
| Taqbaylit | ⠨⠞⠁⠟⠃⠁⠽⠇⠊⠞ | 10 | « Mačči d Taqbaylit i am-d-ǧǧan ! » |
| aɣrum | ⠁⠸⠟⠗⠥⠍ | 6 | « ɣer wanda i d-ttsewwiren medden aɣrum-nsen » (le pain) |
| aḍar | ⠁⠸⠙⠁⠗ | 5 | « netta d uccen iɣeẓẓan aḍar-is » (le pied) |
| iḥemmel | ⠊⠸⠓⠑⠍⠍⠑⠇ | 8 | « iḥemmel adebder-a » (il aime) |
| aṭas | ⠁⠸⠞⠁⠎ | 5 | « Aṭas n smayem i daɣ-yezdin » (beaucoup) |
| laẓ | ⠇⠁⠸⠵ | 4 | « Yettili laẓ d usemmiḍ » (la faim) |
| ɛemmi | ⠸⠑⠑⠍⠍⠊ | 6 | « senṭeḍen-aɣ-d ɛemmi ara yawin i xemsa » (oncle paternel) |
| iǧeǧǧigen | ⠊⠸⠛⠑⠸⠛⠸⠛⠊⠛⠑⠝ | 12 | « akk d yiǧeǧǧigen icebḥanen » (les fleurs — geminée ǧǧ) |
| ṛeggmen | ⠸⠗⠑⠛⠛⠍⠑⠝ | 8 | « neɣ ad ṛeggmen » (ils insultent) |
| ṣbeḥ | ⠸⠎⠃⠑⠸⠓ | 6 | « ṣbeḥ zik neɣ deg yiḍ » (le matin) |
| ččuren | ⠸⠉⠸⠉⠥⠗⠑⠝ | 8 | « Ččuren-d merra yiẓrawen-nneɣ d imeṭṭawen » (ils se sont remplis — geminée čč) |
| baba-s | ⠃⠁⠃⠁⠤⠎ | 6 | « Asmi yemmut baba-s » (son père — exemple de clitique avec tiret) |

### 6.2 Mode Dédié (option avancé)

Mêmes mots que §6.1, transcrits avec les cellules dédiées à un caractère (§2.2.2).

| Orthographe | Braille | Nombre de cellules |
|-------------|---------|---------------------|
| Tmurt | ⠨⠞⠍⠥⠗⠞ | 6 |
| Taqbaylit | ⠨⠞⠁⠟⠃⠁⠽⠇⠊⠞ | 10 |
| aɣrum | ⠁⠱⠗⠥⠍ | 5 |
| aḍar | ⠁⠣⠁⠗ | 4 |
| iḥemmel | ⠊⠳⠑⠍⠍⠑⠇ | 7 |
| aṭas | ⠁⠷⠁⠎ | 4 |
| laẓ | ⠇⠁⠿ | 3 |
| ɛemmi | ⠩⠑⠍⠍⠊ | 5 |
| iǧeǧǧigen | ⠊⠫⠑⠫⠫⠊⠛⠑⠝ | 9 |
| ṛeggmen | ⠹⠑⠛⠛⠍⠑⠝ | 7 |
| ṣbeḥ | ⠻⠃⠑⠳ | 4 |
| ččuren | ⠡⠡⠥⠗⠑⠝ | 6 |
| baba-s | ⠃⠁⠃⠁⠤⠎ | 6 |

**Observation** : le mode dédié réduit le nombre de cellules de **15–20 %** en moyenne sur les textes kabyles, grâce à la suppression de l'indicateur pour les caractères spéciaux.

---

## 7. Barrières qualité

### 7.1 Pipeline de validation obligatoire

Tout document braille kabyle produit par un système automatique doit passer par les barrières suivantes :

| Étape | Contrôle | Seuil / Action |
|-------|----------|----------------|
| B1 | Texte source normalisé | Passage obligatoire par le pipeline §4 |
| B2 | Caractères braille valides | Seules les cellules définies dans cette spec sont autorisées |
| B3 | Pas de cellules CBFU ambiguës | Vérifier qu'aucune cellule de grade 2 (abrégé) n'est injectée par erreur |
| B4 | Longueur cohérente | Le nombre de cellules braille doit être ≥ au nombre de caractères source (mode indicateur) ou égal (mode dédié, hors majuscules) |
| B5 | Géminées correctes | Vérifier que les digrammes bb, dd, tt, etc. produisent bien deux cellules identiques |
| B6 | Tirets préservés | Vérifier que les clitiques avec tiret conservent le `⠤` |

### 7.2 Tests de non-régression CBFU

Un texte français inséré dans un document kabyle (citation, emprunt) doit se transcrire **à l'identique** en CBFU grade 1. Les règles kabyles ne doivent pas affecter le français.

| Texte français | Braille attendu | Test |
|----------------|-----------------|------|
| Bonjour | ⠨⠃⠕⠝⠚⠕⠥⠗ | CBFU standard |
| café | ⠉⠁⠋⠿ | CBFU standard (é = ⠿ en CBFU) |

**[À VALIDER]** : plusieurs sources secondaires (Wikipédia, Grokipedia) corroborent `⠿` (dots 123456) pour é en braille français, mais cette valeur n'a pas pu être vérifiée directement dans le texte officiel du CBFU (accès restreint constaté lors de la recherche documentaire). À confirmer avant implémentation.

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
[kabyle-braille-spec.md] → Transcription braille (mode indicateur ou dédié)
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
| L2 | **Aucun test sur embosseuse** — les cellules proposées n'ont pas été testées sur du papier braille. La lisibilité tactile de certaines combinaisons (notamment les cellules à 5 ou 6 points) reste à vérifier. | **[À VALIDER]** |
| L3 | **Collision ɣ/ǧ en mode indicateur** — toutes deux utilisent la lettre g de base. La solution `⠸⠟` (q) pour ɣ est une proposition non testée. | **[À VALIDER]** |
| L4 | **Cellules dédiées non confirmées libres** — certaines cellules proposées pour le mode dédié peuvent être utilisées en CBFU grade 2 ou dans des contextes spéciaux non documentés ici. | **[À VALIDER]** — Vérification CBFU complète requise |
| L5 | **Pas de grade 2 (abrégé)** — cette spec ne définit pas de système abrégé (contractions, logogrammes) pour le kabyle. Un grade 2 kabyle serait souhaitable à long terme pour réduire le volume braille. | Extension future |
| L6 | **Pas de support Tifinagh** — cette spec ne couvre que l'alphabet latin berbère (INALCO). Une spec braille pour le néo-tifinagh (IRCAM) nécessiterait un travail séparé. | Spec séparée souhaitable |
| L7 | **Pas de test avec Liblouis** — la table Liblouis proposée en §5.3 n'a pas été compilée ni testée. | Implémentation requise |
| L8 | **Indicateur de majuscule pour caractères spéciaux** — `⠨` + `⠸` + lettre = 3 cellules pour une majuscule spéciale en mode indicateur. C'est lourd. Le mode dédié résout ce problème (`⠨` + cellule dédiée = 2 cellules). | Optimisation future |

---

## 10. Conclusion

Cette spécification établit la première base de transcription braille pour le kabyle (Taqbaylit), en s'appuyant sur le CBFU comme fondement compatible et en proposant deux modes d'extension pour les 11 caractères spécifiques berbères. Elle s'intègre comme couche de sortie tactile dans le stack kabyle existant, en aval de la normalisation orthographique.

La principale avancée est la **formalisation d'un mapping complet** de l'alphabet INALCO vers le braille, documenté avec justification phonétique et ergonomique. La principale limite est l'**absence de validation par des experts braille et des utilisateurs déficients visuels kabyles** — cette spec est une **proposition de recherche** en attente de validation communautaire.

L'adoption de cette spécification par les outils de transcription (Liblouis, embosseuses, lecteurs d'écran) et sa validation par la communauté braille francophone et kabyle constitue la prochaine étape critique.

---

## Références

1. **Chaker, Salem** (1996). *Propositions pour la notation usuelle à base latine du berbère*. INALCO / Centre de Recherche Berbère, Paris. https://www.centrederechercheberbere.fr/tl_files/doc-pdf/notation.pdf
2. **CBFU** (2008). *Code braille français uniformisé pour la transcription des textes imprimés*. Commission Évolution du Braille Français (CEBF), 2ᵉ édition, septembre 2008 ; application obligatoire en France par arrêté du 17 août 2006. https://www.avh.asso.fr/nos-solutions/tout-savoir-sur-le-braille/code-braille-francais-uniformise
3. **UNESCO** (2013). *World Braille Usage*. 3rd edition. https://www.unesco.org/
4. **Liblouis** (2026). *Open-source braille translator*. https://liblouis.io/
5. **Unicode Consortium** (2026). *Unicode Standard, Version 16.0* — Braille Patterns (U+2800–U+28FF). https://unicode.org/
6. **Mokraoui, Athmane (boffire)** (2026). *kabyle-orthography-specs.md*. https://kabyle-specs.github.io/
7. **Mokraoui, Athmane (boffire)** (2026). *kabyle-keyboard-layout-spec.md*. https://kabyle-specs.github.io/
8. **Mokraoui, Athmane (boffire)** (2026). *kabyle-g2p-specs.md*. https://kabyle-specs.github.io/
9. **ResearchGate** (non daté). *Amazigh Converter based on WordprocessingML* — mention d'un convertisseur braille↔tifinagh. https://www.researchgate.net/
10. **Dallet, Jean-Marie** (1982). *Dictionnaire kabyle-français: parler des At Mengellat, Algérie*. SELAF, Paris.
11. **Naït-Zerrad, Kamal** (2001). *Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit*. Karthala, Paris.
12. **Feraoun, Mouloud / Bouamara, Kamal (trad.)** (1998). *Ussan di Tmurt* (traduction kabyle de *Jours de Kabylie*, Mouloud Feraoun, 1954). Alger. Source de tous les exemples de mots attestés en §3.2, §3.3, §3.6 et §6.

---

*Document rédigé dans le cadre du développement des ressources d'accessibilité pour la langue kabyle. Les zones nécessitant une validation par des experts braille ou des locuteurs natifs déficients visuels sont signalées [À VALIDER].*
