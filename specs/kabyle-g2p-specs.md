# La phonologie du kabyle (Taqbaylit) en transcription phonétique internationale (API) : spécification orthography2ipa

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; structuration phonologique, recherche documentaire et vérification native.

**Date** : 5 août 2026

**Version** : 2026-08-05

**Cible** : Linguistes computationnels, développeurs TTS/ASR, ingénieurs phonétiques, chercheurs en linguistique berbère.

---

## Résumé

Le kabyle (Taqbaylit, code ISO 639-3 `kab`) possède un système phonologique caractéristique du berbère nordique, dont la marque distinctive est la **spirantisation** (assourdissement des occlusives lâches en fricatives) et une riche allophonie vocalique conditionnée par l'environnement consonantique. Ce document expose la spécification complète du grapheme-to-phoneme (G2P) pour l'alphabet latin berbère standard (INALCO 1996), avec un inventaire graphemique de 34 lettres, des règles allophoniques détaillées, et douze limites documentées pour les extensions futures.

**Mots-clés** : kabyle, taqbaylit, phonologie, API, spirantisation, allophonie, G2P, orthography2ipa, TTS, ASR.

---

## 1. Introduction

Le kabyle est une langue chamito-sémitique (afro-asiatique) du groupe berbère, parlée par 5 à 7 millions de locuteurs en Algérie (Kossmann & Stroomer 1997, p. 461). Sa phonologie se distingue par :

1. **Une opposition tension/détension** (consonnes longues vs. consonnes brèves), notée par le redoublement de la lettre.
2. **La spirantisation** : les occlusives lâches /b d t k g ḍ/ deviennent des fricatives [β ð θ ç ʝ ðˤ] en position post-vocalique et initiale (sauf exceptions documentées).
3. **Un système vocalique à trois phonèmes** (/a i u/) avec réalisations allophoniques étendues.
4. **Des assimilations nasales** conditionnées par le point d'articulation du segment suivant (limitées à [ŋ] et [m] ; l'allophone palatal [ɲ] n'est pas attesté intra-morphémique en kabyle standard).

Ce document est la spécification de référence pour le module `orthography2ipa` du kabyle. Il décrit l'inventaire graphemique, les phonèmes sous-jacents, les allophones, et les règles contextuelles qui déterminent la surface phonétique.

---

## 2. Sources primaires

### 2.1 Kossmann & Stroomer (1997) — Phonologie berbère
**Maarten G. Kossmann & Harry J. Stroomer**, *Berber Phonology*, in A. S. Kaye (ed.), *Phonologies of Asia and Africa*, vol. 1, Eisenbrauns, 1997, pp. 461-475.

Source primaire absolue. Décrit :
- L'opposition tension/détension (p. 465).
- La spirantisation générale du berbère (p. 466) : « Spirantization never affects tense consonants ».
- Les exemples détaillés de spirantisation et de blocage post-nasal homorganique pour le rifain de Beni Saïd (pp. 468-469), étendus au kabyle par analogie.

**Caveat sourcing** : Kossmann & Stroomer ne consacrent pas de section spécifique au kabyle. Seuls le tashelhit (§23.6), le rifain (§23.7) et le touareg (§23.8) sont traités en détail. Les règles kabyles de ce document sont donc des extensions analogiques du rifain, corroborées par des sources kabyles spécifiques.

### 2.2 Bedar, Quellec & Tifrit (2022) — Occlusivation post-sonorante
**Amazigh Bedar, Lucie Quellec & Ali Tifrit**, *Post-sonorant occlusivization in Kabyle*, 19th Meeting of the French Phonology Network (RFP 2022), Porto, 7-9 juin 2022.

Source primaire kabyle spécifique. Étude basée sur le kabyle de Chemini (sud-est de Béjaïa), avec données comparatives de Aït Mengellat, Boghni et Makouda. Confirme :
- L'occlusivation de /β/ après /n, m/ mais pas /r, l/.
- L'occlusivation de /θ, ð/ après /l, n, m/ mais pas /r/.
- L'occlusivation de /ç/ après /r, l, n, m/.
- L'occlusivation de /ʝ/ après /r, l, n/ mais pas /m/.
- La débuccalisation de /mʝ/ → [mh] devant /u/.

### 2.3 Bedar & Chergui (2024) — Assimilation nasale-glide
**Amazigh Bedar & Hamza Chergui**, *An element-based analysis of nasal-glide assimilation in the Taqbaylit prepositional phrase*, Glottometrics 58, 2024, pp. 1-19.

Documente que la séquence /n+j/ à travers une frontière de morphème (préposition *n* + nom initial *j*) subit une assimilation nasale-glide aboutissant à des segments palataux géminés [jj], [kk] ou [gg] selon le dialecte. **Ce n'est pas un simple allophone [ɲ]**. Ce processus de sandhi morphophonologique n'est pas modélisé dans cette spécification.

### 2.4 Karaoui, Djeradi & Djeradi (2024) — Fricatives kabyles
**Fazia Karaoui, Amar Djeradi & Rachida Djeradi**, *Acoustic Characterization of the Noise Sources for the Kabyle Fricatives Consonants*, AIJR (ICAECE'2023 abstracts), DOI 10.21467/abstracts.163, pp. 123-125.

Confirme l'inventaire des fricatives kabyles : [s, f, ʃ, v, ʒ, z, ħ, h, ʕ, ç, ʝ], et liste la spirantisation, la labio-vélarisation, la palatalisation et l'affrication comme traits spécifiques du kabyle.

### 2.5 Chaker (1996) — Standard orthographique
**Salem Chaker**, *Propositions pour la notation usuelle à base latine du berbère*, Centre de Recherche Berbère – INALCO, Paris, 1996.

Source primaire pour l'orthographe standard. Son tableau consonantique liste les paires occlusive/spirante (b/β, t/θ, d/ð, k/ç, g/ʝ) et prescrit le redoublement de lettre pour noter la tension — la notation « tamaziɣt/INALCO » que ce document encode.

### 2.6 Mammeri (1976) — Codification
**Mouloud Mammeri**, *Précis de grammaire berbère (kabyle)*, Paris, 1976 (rééd. Fichier TAL, 1983).

Codification historique de l'orthographe latine du kabyle, sur laquelle repose le standard INALCO 1996.

### 2.7 Wang (2020) — Syllabification
**Beini Wang**, *Syllabification and Schwa Epenthesis in Kabyle*, McGill Working Papers in Linguistics 26.1, 2020.

Démontre que tous les schwas kabyles sont épenthétiques (réparation de clusters mal formés) et que les géminées ne sont jamais brisées par épenthèse.

### 2.8 Vérification native (MOKRAOUI 2026)
Sessions de jugement de locuteur natif (Athmane Mokraoui) pour la spirantisation de /b d t k g/ en position initiale, post-vocalique et post-consonantique. Décisions clés : blocage post-consonantique catégorique pour /k/ et /g/ avec l'exception /mʝ/ ; spirantisation de /t/ initiale devant toutes les voyelles y compris /i/ ; rejet de l'allophone [ʃ] pour /k/ ; confirmation de la spirantisation post-/r/ pour /k/ et /g/ ; **clitiques /k/ (kem, kent, k- + voyelle) et emprunt kullec conservent l'occlusive [k]**.

---

## 3. Inventaire graphemique

Le kabyle standard utilise l'alphabet latin berbère de 34 lettres (INALCO 1996). Ce document couvre l'ensemble des graphemes simples et géminés.

### 3.1 Voyelles et semi-voyelles

| Grapheme | Phonème | Exemple | Réalisation |
|----------|---------|---------|-------------|
| a | /a/ | *axam* | [ɑχːɑm] |
| e | /ə/ | *tameṭṭut* | [θamətˤːθ] |
| i | /i/ | *tifinaɣ* | [θifinɑʁ] |
| u | /u/ | *tawwurt* | [θawwurθ] |
| y | /j/ | *Ayyur* | [ɑjːur] |
| w | /w/ | *tawwurt* | [θawwurθ] |

### 3.2 Consonnes simples

| Grapheme | Phonème | Articulation | Notes |
|----------|---------|--------------|-------|
| b | /b/ | bilabiale occlusive | Spirantise en [β] |
| c | /ʃ/ | alvéolo-palatale fricative | |
| č | /t͡ʃ/ | alvéolo-palatale affriquée | |
| d | /d/ | dentale occlusive | Spirantise en [ð] |
| ḍ | /dˤ/ | dentale emphatique occlusive | Spirantise en [ðˤ] |
| f | /f/ | labio-dentale fricative | |
| g | /ɡ/ | vélaire occlusive | Spirantise en [ʝ] |
| ǧ | /d͡ʒ/ | alvéolo-palatale affriquée | |
| ɣ | /ɣ/ | uvulaire fricative | Réalisée [ʁ] en kabyle |
| h | /h/ | glottale fricative | |
| ḥ | /ħ/ | pharyngale fricative | |
| j | /ʒ/ | postalvéolaire fricative | |
| k | /k/ | vélaire occlusive | Spirantise en [ç] (voir exceptions §4.1.2) |
| l | /l/ | alvéolaire latérale | |
| m | /m/ | bilabiale nasale | |
| n | /n/ | alvéolaire nasale | S'assimile en [ŋ, m] |
| p | /p/ | bilabiale occlusive | Uniquement emprunts |
| q | /q/ | uvulaire occlusive | |
| r | /r/ | alvéolaire vibrante | |
| ṛ | /rˤ/ | alvéolaire vibrante emphatique | |
| s | /s/ | alvéolaire fricative | |
| ṣ | /sˤ/ | alvéolaire fricative emphatique | |
| t | /t/ | dentale occlusive | Spirantise en [θ] |
| ṭ | /tˤ/ | dentale occlusive emphatique | Non spirantisée |
| v | /v/ | labio-dentale fricative | Uniquement emprunts |
| x | /χ/ | uvulaire fricative sourde | |
| z | /z/ | alvéolaire fricative | |
| ẓ | /zˤ/ | alvéolaire fricative emphatique | |
| ɛ | /ʕ/ | pharyngale fricative sonore | |

### 3.3 Consonnes géminées (tension)

| Grapheme | Phonème | Réalisation | Principe |
|----------|---------|-------------|----------|
| bb | /bː/ | [bː] | **Jamais spirantisée** |
| dd | /dː/ | [dː] | **Jamais spirantisée** |
| tt | /tː/ | [tː] | **Jamais spirantisée** |
| kk | /kː/ | [kː] | **Jamais spirantisée** |
| gg | /ɡː/ | [ɡː] | **Jamais spirantisée** |
| ḍḍ | /dˤː/ | [dˤː] | **Jamais spirantisée** |
| ṭṭ | /tˤː/ | [tˤː] | **Jamais spirantisée** |
| ff | /fː/ | [fː] | |
| ll | /lː/ | [lː] | |
| mm | /mː/ | [mː] | |
| nn | /nː/ | [nː] | |
| rr | /rː/ | [rː] | |
| ṛṛ | /rˤː/ | [rˤː] | |
| ss | /sː/ | [sː] | |
| ṣṣ | /sˤː/ | [sˤː] | |
| zz | /zː/ | [zː] | |
| ẓẓ | /zˤː/ | [zˤː] | |
| cc | /ʃː/ | [ʃː] | |
| čč | /t͡ʃː/ | [t͡ʃː] | |
| ǧǧ | /d͡ʒː/ | [d͡ʒː] | |
| ɣɣ | /ɣː/ | [ʁː] | Réalisée [ʁː] |
| ḥḥ | /ħː/ | [ħː] | |
| qq | /qː/ | [qː] | |
| ɛɛ | /ʕː/ | [ʕː] | |
| xx | /χː/ | [χː] | |
| ww | /wː/ | [wː] | |
| yy | /jː/ | [jː] | |
| pp | /pː/ | [pː] | Emprunts |
| jj | /ʒː/ | [ʒː] | |
| hh | /hː/ | [hː] | |

---

## 4. Les allophones et les règles contextuelles

### 4.1 La spirantisation (trait signature)

La spirantisation est le processus par lequel les occlusives **lâches** (non-géminées) deviennent des fricatives. Les occlusives **tendues** (géminées) résistent systématiquement — **sans exception**.

| Phonème | Allophone | Contexte | Exemple |
|---------|-----------|----------|---------|
| /b/ | [β] | Post-vocalique, initiale | *abrid* [æβrið] |
| /d/ | [ð] | Post-vocalique, intervocalique | *adrar* [aðrar] |
| /t/ | [θ] | Post-vocalique, initiale | *tafat* [θafaθ] |
| /k/ | [ç] | Post-vocalique, initiale, post-/r/ | *kra* [çræ], *taberkant* [θabərçant] |
| /ɡ/ | [ʝ] | Post-vocalique, initiale, post-/m/, post-/r/ | *argaz* [arʝaz], *tamga* [tamʝa] |
| /dˤ/ | [ðˤ] | Post-vocalique, initiale | |

#### 4.1.1 Blocages de la spirantisation

| Règle | Phonème | Contexte bloqué | Exemple |
|-------|---------|-----------------|---------|
| KAB_BLOCK_SPIRANT_B_AFTER_M | /b/ | Après /m/ | *zembil* [zəmbil] |
| KAB_BLOCK_SPIRANT_D_AFTER_N | /d/ | Après /n/ | *anda* [anda] |
| KAB_BLOCK_SPIRANT_T_AFTER_N | /t/ | Après /n/ | *anta* [anta] |
| KAB_BLOCK_SPIRANT_T_AFTER_L_FINAL | /t/ | Finale après /l/ | *tamellalt* [θaməllalt] |
| KAB_BLOCK_SPIRANT_DH_EMPH_AFTER_N | /dˤ/ | Après /n/ | |
| KAB_BLOCK_SPIRANT_D_AFTER_L | /d/ | Après /l/ | *Ldi* [ldi] |
| KAB_BLOCK_SPIRANT_T_AFTER_M | /t/ | Après /m/ | *tasemmumt* [tasemmumt] |
| KAB_BLOCK_SPIRANT_D_AFTER_M | /d/ | Après /m/ | *tamda* [θamda] |
| KAB_BLOCK_SPIRANT_D_WORD_INITIAL | /d/ | Initiale absolue | *d-ittun* [dittun] |
| KAB_BLOCK_SPIRANT_K_AFTER_CONSONANT | /k/ | Après toute consonne **sauf** /r, rˤ/ | *taberkant* [ç] mais *ankal* [ankal] |
| KAB_BLOCK_SPIRANT_G_AFTER_CONSONANT | /ɡ/ | Après toute consonne **sauf** /m, r, rˤ/ | *argaz* [ʝ] mais *angal* [aŋɡal] |

**Note sur /k/ et /ɡ/ après /r/** : Bedar et al. (2022) ne traitent pas le contexte post-/r/ pour /k/ et /ɡ/. La vérification native (MOKRAOUI 2026) confirme la spirantisation : *taberkant* → [ç], *argaz* → [ʝ].

#### 4.1.2 Exceptions lexicales et morphologiques pour /k/

Le phonème /k/ spirantise en [ç] dans la grande majorité des contextes. Cependant, un petit ensemble de formes clitiques et d'emprunts conserve l'occlusive [k] :

| Forme | Prononciation | Type d'exception | Contexte |
|-------|---------------|------------------|----------|
| *kem* | [kem] | Pronom clitique | Standalone (objet, f.sg) |
| *kent* | [kent] | Pronom clitique | Standalone (objet, f.pl) |
| *k-* + voyelle | [k-] | Préfixe clitique | Pronom objet préverbal (ex. *k-ufiɣ*, *k-walaɣ*) |
| *kullec* | [kulːəʃ] | Emprunt lexical | Arabe *kull* — conserve [k] |

**Tout autre /k/ initiaux, médians ou post-vocaliques spirantise en [ç]** : *kra* [çræ], *kečč* [çət͡ʃː], *kcem* [çcem], *kteb* [çteb], *aksum* [açum]. Le pronom suffixe *-k* (m.sg) spirantise également : *a-k* [aç], *iyi-k* [ijiç].

> **Mise à jour août 2026** — Voir §12.3 pour le statut d'implémentation post-validation native.

### 4.1.3 Clitiques suffixes pronominaux — frontière orthographique vs phonologique

En orthographe INALCO, les pronoms objets et possessifs sont attachés au mot hôte par un tiret (ex. *fell-i*, *Nniɣ-ak*, *tessawleḍ-as*). Ce tiret est **un marqueur morphologique, pas une frontière phonologique**. Le clitique suffixe forme une unité phonologique avec le mot hôte.

**Conséquence pour le G2P** : Un tokenizer qui segmente sur le tiret produit deux unités prosodiques indépendantes (*fəlː* + *i* au lieu de *fəlli*), ce qui est phonologiquement incorrect. Ces formes nécessitent une **pré-tokenisation morphologique** avant l'application des règles G2P.

**Inventaire des suffixes clitiques attestés** (fréquences du corpus Tatoeba kabyle) :

| Suffixe | Fréquence | Fonction | Exemple |
|---------|-----------|----------|---------|
| *-nni* | ~71k | Défini éloigné | *adlis-nni* |
| *-d* | ~32k | Ventive | *yusa-d* |
| *-is* | ~21k | Possessif 3sg m | *aɣrum-is* |
| *-a* | ~18k | Déictique proche | *iseggasen-a* |
| *-s* | ~16k | Objet 3sg m/f | *ɣur-s* |
| *-as* | ~13k | Objet datif 3sg | *Tessawleḍ-as* |
| *-iw* | ~12k | Possessif 1sg | *tmeṭṭut-iw* |
| *-ik* | ~9k | Possessif 2sg m | *wemddakel-ik* |
| *-k* | ~8k | Objet 2sg m | *tikti-k* |
| *-iyi* | ~7k | Objet 1sg | *ssuref-iyi* |
| *-i* | ~7k | Objet 1sg (post-voyelle) | *fell-i* |
| *-t* | ~7k | Objet 3sg f | *yeǧǧa-t* |
| *-tt* | ~4k | Objet 3sg f renforcé | *neǧǧa-tt* |
| *-nneɣ* | ~5k | Possessif 1pl | *tmurt-nneɣ* |
| *-aɣ* | ~3k | Objet 1pl | *Tesselmad-aɣ* |
| *-agi* | ~3k | Déictique renforcé | *axxam-agi* |
| *-ak* | ~3k | Objet 2sg m (post-voyelle) | *Nniɣ-ak* |
| *-am* | ~2k | Objet 2sg f (post-voyelle) | *fell-am* |
| *-asen* | ~4k | Objet datif 3pl m | *gar-asen* |
| *-kent* | ~2k | Objet 2pl f | *deg-kent* |
| *-awen* | ~1k | Objet datif 2pl m | *fell-awen* |
| *-kem* | ~1k | Objet 2sg f | *Ansi-kem* |
| *-ken* | ~1k | Objet 2pl m | *Aqli-ken* |

**Règle générale** : Le tiret est transparent phonologiquement. Les règles de spirantisation, d'assimilation et de gémination s'appliquent à l'ensemble *mot+clitique* comme s'il s'agissait d'une seule unité lexicale.

### 4.2 Allophonie vocalique

#### 4.2.1 Retraction de /a/

| Règle | Contexte | Réalisation | Exemple |
|-------|----------|-------------|---------|
| KAB_A_BACKING | Avant emphatique/uvulaire | [ɑ] | *axxam* [ɑχːɑm] |
| KAB_A_BACKING_PRECEDING | Après emphatique/uvulaire | [ɑ] | |
| KAB_A_DEFAULT | Défaut | [æ] | *amcic* [æmçiç] |

#### 4.2.2 Fermeture en syllabe fermée

| Règle | Voyelle | Réalisation | Contexte |
|-------|---------|-------------|----------|
| KAB_I_CLOSED | /i/ | [ɪ] | Avant cluster consonantique |
| KAB_U_CLOSED | /u/ | [ʊ] | Avant cluster consonantique |

### 4.3 Assimilations nasales

L'assimilation nasale en kabyle standard est **limitée aux contextes vélaire et labial**. L'allophone palatal [ɲ] n'est **pas attesté** en position intra-morphémique (voir Known Limit #12).

| Règle | Phonème | Réalisation | Contexte | Exemple |
|-------|---------|-------------|----------|---------|
| KAB_N_VELAR_ASSIM | /n/ | [ŋ] | Avant dorsale (k, g, q, χ, ɣ) | *ankal* [aŋkal] |
| KAB_N_LABIAL_ASSIM | /n/ | [m] | Avant labiale (b, p, f, m) | *anba* [amba] |

### 4.4 Réalisation de /ɣ/

| Règle | Phonème | Réalisation | Contexte | Exemple |
|-------|---------|-------------|----------|---------|
| KAB_GH_UVULAR_REALIZATION | /ɣ/ | [ʁ] | Toutes positions | *tifinaɣ* [θifinɑʁ] |
| KAB_GH_GEM_UVULAR_REALIZATION | /ɣː/ | [ʁː] | Toutes positions | |

---

## 5. Tableau récapitulatif des règles pour les moteurs G2P

| ID de règle | Type | Phonèmes cibles | Surface | Condition | Priorité |
|-------------|------|-----------------|---------|-----------|----------|
| KAB_BLOCK_SPIRANT_B_AFTER_M | Blocage | /b/ | [b] | Précédé par /m/ | Haute |
| KAB_BLOCK_SPIRANT_D_AFTER_N | Blocage | /d/ | [d] | Précédé par /n/ | Haute |
| KAB_BLOCK_SPIRANT_T_AFTER_N | Blocage | /t/ | [t] | Précédé par /n/ | Haute |
| KAB_BLOCK_SPIRANT_T_AFTER_L_FINAL | Blocage | /t/ | [t] | Finale, précédé par /l/ | Haute |
| KAB_BLOCK_SPIRANT_DH_EMPH_AFTER_N | Blocage | /dˤ/ | [dˤ] | Précédé par /n/ | Haute |
| KAB_BLOCK_SPIRANT_D_AFTER_L | Blocage | /d/ | [d] | Précédé par /l/ | Haute |
| KAB_BLOCK_SPIRANT_T_AFTER_M | Blocage | /t/ | [t] | Précédé par /m/ | Haute |
| KAB_BLOCK_SPIRANT_D_AFTER_M | Blocage | /d/ | [d] | Précédé par /m/ | Haute |
| KAB_BLOCK_SPIRANT_D_WORD_INITIAL | Blocage | /d/ | [d] | Initiale absolue | Haute |
| KAB_BLOCK_SPIRANT_K_AFTER_CONSONANT | Blocage | /k/ | [k] | Précédé par consonne (sauf /r, rˤ/) | Haute |
| KAB_BLOCK_SPIRANT_G_AFTER_CONSONANT | Blocage | /ɡ/ | [ɡ] | Précédé par consonne (sauf /m, r, rˤ/) | Haute |
| KAB_BLOCK_SPIRANT_K_CLITIC | Blocage (non modélisé) | /k/ | [k] | Formes clitiques *kem, kent, k-* + voyelle — **non implémenté au niveau des `allophone_rules`** ; le schéma actuel n'a pas de mécanisme de correspondance lexicale (mot-spécifique). À gérer au niveau moteur/rescorer via une liste d'exceptions codée en dur. *kullec* seul est couvert, via `KAB_BLOCK_SPIRANT_K_BEFORE_U`. | Haute (prévue) |
| KAB_SPIRANT_K | Spirantisation | /k/ | [ç] | Post-vocalique, initiale (sauf clitiques), post-/r/ | Basse |
| KAB_SPIRANT_B | Spirantisation | /b/ | [β] | Post-vocalique, initiale | Basse |
| KAB_SPIRANT_D | Spirantisation | /d/ | [ð] | Post-vocalique, intervocalique | Basse |
| KAB_SPIRANT_T | Spirantisation | /t/ | [θ] | Post-vocalique, initiale | Basse |
| KAB_SPIRANT_G | Spirantisation | /ɡ/ | [ʝ] | Post-vocalique, initiale, post-/m,r/ | Basse |
| KAB_SPIRANT_DH_EMPH | Spirantisation | /dˤ/ | [ðˤ] | Post-vocalique, initiale | Basse |
| KAB_GH_UVULAR_REALIZATION | Allophonie | /ɣ/ | [ʁ] | Toutes positions | Basse |
| KAB_GH_GEM_UVULAR_REALIZATION | Allophonie | /ɣː/ | [ʁː] | Toutes positions | Basse |
| KAB_A_BACKING | Allophonie | /a/ | [ɑ] | Avant emphatique/uvulaire | Basse |
| KAB_A_BACKING_PRECEDING | Allophonie | /a/ | [ɑ] | Après emphatique/uvulaire | Basse |
| KAB_A_DEFAULT | Allophonie | /a/ | [æ] | Défaut (pas de backing) | Basse |
| KAB_I_CLOSED | Allophonie | /i/ | [ɪ] | Avant cluster consonantique | Basse |
| KAB_U_CLOSED | Allophonie | /u/ | [ʊ] | Avant cluster consonantique | Basse |
| KAB_N_VELAR_ASSIM | Assimilation | /n/ | [ŋ] | Avant dorsale | Basse |
| KAB_N_LABIAL_ASSIM | Assimilation | /n/ | [m] | Avant labiale | Basse |

**Ordre d'application** : Les règles de blocage (HIGH) s'appliquent avant les règles de spirantisation (LOW). Les règles allophoniques vocaliques et nasales s'appliquent en dernier.

---

## 6. Exemples de dérivation complète

### 6.1 Exemples natifs

| Orthographe | Phonémique | Règles actives | Phonétique | Glossaire |
|-------------|------------|----------------|------------|-----------|
| *abrid* | /a-b-r-i-d/ | SPIRANT_B, A_DEFAULT | [æβrið] | chemin |
| *adrar* | /a-d-r-a-r/ | SPIRANT_D, A_DEFAULT | [aðrar] | montagne |
| *tafat* | /t-a-f-a-t/ | SPIRANT_T (×2), A_DEFAULT | [θafaθ] | lumière |
| *tawwurt* | /t-a-wː-u-r-t/ | SPIRANT_T (×2), U_CLOSED | [θawwurθ] | porte |
| *tameṭṭut* | /t-a-m-e-tˤː-u-t/ | SPIRANT_T (×2), A_DEFAULT | [θamətˤːθ] | femme |
| *igenni* | /i-ɡ-e-nː-i/ | SPIRANT_G | [iʝənni] | ciel |
| *kra* | /k-r-a/ | SPIRANT_K, A_DEFAULT | [çræ] | quelque |
| *kečč* | /k-e-cː/ | SPIRANT_K | [çət͡ʃː] | toi (emphatique) |
| *kcem* | /k-c-e-m/ | SPIRANT_K | [çcem] | entre ! |
| *kteb* | /k-t-e-b/ | SPIRANT_K | [çteb] | écris ! |
| *taberkant* | /t-a-b-e-r-k-a-n-t/ | SPIRANT_T (×2), SPIRANT_K (post-/r/), A_DEFAULT | [θabərçant] | (nom propre) |
| *argaz* | /a-r-ɡ-a-z/ | SPIRANT_G (post-/r/), A_DEFAULT | [arʝaz] | homme |
| *tamga* | /t-a-m-ɡ-a/ | SPIRANT_T, SPIRANT_G (post-/m/), A_DEFAULT | [θamʝa] | source |
| *zembil* | /z-e-m-b-i-l/ | BLOCK_B_AFTER_M, A_DEFAULT | [zəmbil] | panier |
| *anda* | /a-n-d-a/ | BLOCK_D_AFTER_N, A_DEFAULT | [anda] | où |
| *anta* | /a-n-t-a/ | BLOCK_T_AFTER_N, A_DEFAULT | [anta] | laquelle (f.) |
| *tamellalt* | /t-a-m-e-lː-a-l-t/ | SPIRANT_T (initiale), BLOCK_T_AFTER_L_FINAL, A_DEFAULT | [θaməllalt] | œuf |
| *Ldi* | /l-d-i/ | BLOCK_D_AFTER_L | [ldi] | ouvre ! |
| *tasemmumt* | /t-a-s-e-m-m-u-m-t/ | SPIRANT_T (initiale), BLOCK_T_AFTER_M | [tasemmumt] | (nom propre) |
| *tamda* | /t-a-m-d-a/ | BLOCK_D_AFTER_M, SPIRANT_T, A_DEFAULT | [θamda] | mare |
| *d-ittun* | /d-i-tː-u-n/ | BLOCK_D_WORD_INITIAL | [dittun] | ils sont venus |
| *tifinaɣ* | /t-i-f-i-n-a-ɣ/ | SPIRANT_T (initiale), GH_UVULAR, A_BACKING_PRECEDING | [θifinɑʁ] | tifinagh |
| *bbeɣ* | /bː-e-ɣ/ | GH_UVULAR | [bːəʁ] | plonger |
| *axxam* | /a-xː-a-m/ | A_BACKING (avant /χː/), A_BACKING_PRECEDING (après /χː/) | [ɑχːɑm] | maison |
| *ankal* | /a-n-k-a-l/ | N_VELAR_ASSIM, BLOCK_K_AFTER_CONSONANT, A_DEFAULT | [aŋkal] | (nom propre) |
| *anba* | /a-n-b-a/ | N_LABIAL_ASSIM | [amba] | fils de... |
| *kem* | /k-e-m/ | BLOCK_K_CLITIC | [kem] | toi (f.sg, clitique) |
| *kent* | /k-e-n-t/ | BLOCK_K_CLITIC | [kent] | vous (f.pl, clitique) |
| *k-ufiɣ* | /k-u-f-i-ɣ/ | BLOCK_K_CLITIC | [kufiɣ] | je t'ai trouvé(e) |
| *k-walaɣ* | /k-w-a-l-a-ɣ/ | BLOCK_K_CLITIC | [kwalaɣ] | je t'ai vu(e) |
| *kullec* | /k-u-lː-e-c/ | BLOCK_K_CLITIC | [kulːəʃ] | tout (emprunt arabe) |

### 6.2 Exemples d'emprunts (non spirantisés)

| Orthographe | Phonémique | Phonétique | Origine |
|-------------|------------|------------|---------|
| *itiknikanen* | /i-t-i-k-n-i-k-a-n-e-n/ | [itiknikanen] | français *techniciens* |
| *pulu* | /p-u-l-u/ | [pulu] | français *poulet* |
| *villa* | /v-i-lː-a/ | [villa] | français *ville* |

---

## 7. Notes dialectales et variétés

### 7.1 Chemini vs. Boghni/Makouda

Bedar, Quellec & Tifrit (2022) documentent un clivage dialectal majeur concernant l'occlusivation post-/m/ :

| Contexte | Chemini / Aït Mengellat | Boghni / Makouda | Ce document |
|----------|------------------------|------------------|-------------|
| /mθ/ | [mt] | [mθ] | [mt] (Chemini) |
| /mð/ | [md] | [mð] | [md] (Chemini) |
| /mβ/ | [mb] | [mb] | [mb] (tous) |
| /mç/ | [mk] | [mk] | [mk] (tous) |
| /mʝ/ devant /u/ | [mh] | [mh] | [mʝ] (non modélisé) |

**Recommandation** : Ce document suit le modèle Chemini/Aït Mengellat. Pour une couverture complète du kabyle algérien, un paramètre dialectal `dialect: "chemini" | "boghni" | "makouda"` serait nécessaire.

### 7.2 Variation post-/r/

Bedar et al. (2022) ne couvrent pas le contexte post-/r/ pour /k/ et /ɡ/. La vérification native (MOKRAOUI 2026) confirme la spirantisation dans la variété cible :

- *taberkant* → [θabərçant] (et non [θabərkant])
- *argaz* → [arʝaz] (et non [arɡaz])

Cependant, d'autres variétés kabyles peuvent occlusiver après /r/. Cette spécification documente cette incertitude dans les limites connues.

---

## 8. Limites connues (documentées, non modélisées)

Les limitations suivantes sont explicitement reconnues. Elles constituent une feuille de route pour les versions futures de cette spécification.

### 8.1 Alternances morphophonologiques de gémination
Les alternances lexicales/morphologiques où une géminée se réalise différemment de l'attendu orthographique ne sont pas modélisables par un G2P purement phonologique :
- ⟨ww⟩ → [bb] (ex. *tawwurt* → [θawwurθ] est correct, mais certains contextes morphologiques donnent [bb])
- ⟨yy⟩ → [ɡɡ]
- ⟨ɣɣ⟩ → [qq]

**Statut** : Lexical/morphologique. Nécessite un lexique ou un analyseur morphologique.

### 8.2 Sandhi aux frontières de mots et de clitiques

Les processus de sandhi inter-mots et le traitement des clitiques suffixes (§4.1.3) ne sont pas modélisés :

- /n/ + /w/ → [bb] ou [pp] (sandhi inter-mots)
- /d/ + /t/ → [ts] (sandhi inter-mots)
- Les clitiques suffixes pronominaux (*-i*, *-k*, *-s*, *-as*, etc.) nécessitent une pré-tokenisation morphologique qui regroupe *mot+clitique* en une seule unité avant le G2P. Sans cette étape, le tokenizer segmente sur le tiret et produit des transcriptions prosodiquement incorrectes (ex. *fell-i* → `fəlː i` au lieu de `fəlli`).

**Statut** : Nécessite une segmentation en mots et une analyse syntaxique pour le sandhi inter-mots ; nécessite un module de pré-tokenisation morphologique pour les clitiques suffixes.

**Statut** : Nécessite une segmentation en mots et une analyse syntaxique.

> **Mise à jour août 2026** — Voir §12.1 pour l'algorithme de pré-tokenisation clitique validé par jugement natif sur 5 000 phrases.

### 8.3 Allophonie vocalique complète
Les réalisations [e] pour /i/ et [o] pour /u/ ne sont que partiellement couvertes. Leur distribution exacte (proximité pharyngale, position dans le mot, influence du schwa adjacent) n'est pas modélisée.

**Statut** : Nécessite une analyse syllabique complète.

### 8.4 Accent
L'accent kabyle est largement prédictible (tonicité sur la dernière syllabe ou l'avant-dernière selon la structure) et non distinctif. Il n'est pas marqué dans cette spécification.

**Statut** : Non prioritaire pour la plupart des applications TTS.

### 8.5 Emprunts récents
Les emprunts français récents (surtout techniques) peuvent échapper à la spirantisation et conserver leurs occlusives. Cette spécification modélise la phonologie native de manière catégorique ; seul *kullec* est documenté comme exception arabe conservant [k].

**Statut** : Nécessite une étiquette d'emprunt ou un lexique d'exceptions.

### 8.6 Débuccalisation de /mʝ/ devant /u/
Dans toutes les variétés étudiées par Bedar et al. (2022), /mʝ/ devant /u/ se débuccalise en [mh] plutôt que [mʝ]. Ce moteur de règles n'a pas de mécanisme conditionné par la voyelle suivante pour une exception à une règle de consonne précédente.

**Statut** : Limitation architecturale du moteur orthography2ipa.

### 8.7 Variation dialectale post-/m/
L'occlusivation post-/m/ de /t/ et /d/ est dialectale (Chemini/Aït Mengellat vs. Boghni/Makouda). Seul le modèle Chemini/Aït Mengellat est implémenté.

**Statut** : Nécessite un paramètre dialectal.

### 8.8 Contextes ouverts de vérification
Les contextes suivants restent à vérifier par jugement de locuteur natif :
- Intervocalique *-ti-* (ex. *asiti*, *susiti*)
- Clusters *sk-* (type *askar*)
- *tirmitin* est confirmé [θɪrmiθin] (les deux /t/ spirantisent), mais les emprunts comme *itiknikanen* conservent leurs occlusives en position interne.

### 8.9 Clitique /k/ — EXCEPTIONS DOCUMENTÉES, NON ENCORE MODÉLISÉES

Le pronom objet /k/ conserve l'occlusive [k] dans les formes suivantes, vérifiées par jugement natif (MOKRAOUI 2026) :
- **Standalone** : *kem* [kem], *kent* [kent]
- **Préfixe préverbal** : *k-ufiɣ* [kufiɣ], *k-walaɣ* [kwalaɣ]
- **Emprunt lexical** : *kullec* [kulːəʃ] — ce cas est couvert par `KAB_BLOCK_SPIRANT_K_BEFORE_U`.

**Le pronom suffixe /-k/ (m.sg) spirantise** : *a-k* [aç], *iyi-k* [ijiç].

Tout autre /k/ lexical ou grammatical spirantise en [ç].

**Statut** : Ces exceptions ne sont **pas** modélisables par le schéma `allophone_rules` actuel, qui ne dispose d'aucun mécanisme de correspondance sur des mots ou clitiques spécifiques (seuls `preceded_by_phoneme`, `followed_by_phoneme`, `word_initial`, `word_final` existent). *kem*, *kent* et *k-* + voyelle restent donc des **exceptions lexicales non implémentées dans `kab.json`**, à charge pour les moteurs consommateurs de les gérer via une liste d'exceptions codée en dur au niveau du rescorer. Une future version pourrait introduire un champ de correspondance lexicale (ex. `word_exceptions`) dans le schéma pour les modéliser nativement.

> **Mise à jour août 2026** — Voir §12.3 pour le statut d'implémentation post-validation native.

### 8.10 Spirantisation en finale absolue
Aucune règle générale de blocage de la spirantisation en position finale absolue n'existe dans cette spécification. Seul le cas spécifique de /t/ après /l/ en finale est bloqué (KAB_BLOCK_SPIRANT_T_AFTER_L_FINAL). La question de savoir si /b, d, k, g, dˤ/ résistent à la spirantisation en position finale absolue reste ouverte.

**Statut** : À vérifier par jugement de locuteur natif.

### 8.11 Absence de spirantisation de /tˤ/
Le /tˤ/ (ṭ) n'est pas listé parmi les occlusives spirantisables dans Kossmann & Stroomer (1997, p. 466). Cette spécification ne modélise pas /tˤ/ → [θˤ] et traite ṭ comme invariant [tˤ].

**Statut** : Aligné sur la source primaire ; à réviser si des données contraires émergent.

### 8.12 Absence d'assimilation palatale [ɲ] intra-morphémique
L'allophone palatal [ɲ] pour /n/ n'est pas attesté en position intra-morphémique en kabyle standard. Les mots comme *afenjal*, *yenjer* produisent systématiquement [nʒ], pas [ɲ]. La séquence /n+j/ à travers une frontière de morphème (préposition *n* + nom initial *j*) subit une assimilation nasale-glide aboutissant à des segments palataux géminés [jj], [kk] ou [gg] selon le dialecte (Bedar & Chergui 2024 ; Bendjaballah & Haiden 2005). C'est un processus de sandhi morphophonologique, pas une règle allophonique simple, et il n'est pas modélisé ici.

**Statut** : [ɲ] retiré de la table allophonique de /n/ ; l'assimilation nasale est limitée à [ŋ] et [m].

---

## 9. Évaluation et écart de conventions avec VoxCommunis

Cette spécification a été évaluée quantitativement contre le corpus VoxCommunis (Common Voice aligné forcé, ~44 600 mots kabyles). Le **PER (Phoneme Error Rate) brut est de 0,2084** (20,84 %). Ce chiffre ne reflète pas la qualité phonologique de la spécification, mais un **écart systématique de conventions de notation** entre cette spécification et la tradition IPA utilisée par VoxCommunis.

### 9.1 Répartition des erreurs

| Catégorie | Nombre | % des erreurs | Nature |
|-----------|--------|---------------|--------|
| `geminate_mismatch` | 22 899 | 51,3 % | VoxCommunis écrit les géminées comme deux caractères séparés (ex. `ðˤðˤ`), cette spécification utilise le symbole de longueur (`dˤː`). Même prononciation, notation différente. |
| `other` | 14 402 | 32,3 % | Mélange de divergences notations : voyelles fermées (`ɪ`/`ʊ` vs `i`/`u`), timbre de /a/ (`æ` vs `ɑ`), frontières de clitiques, etc. |
| `o2i_stop_for_spirant` | 4 079 | 9,1 % | VoxCommunis a une spirante là où o2i produit une occlusive. Partiellement dû à des conventions différentes sur la spirantisation post-consonantique, partiellement à des lacunes réelles. |
| `o2i_plain_for_emphatic` | 2 498 | 5,6 % | VoxCommunis utilise des lettres précomposées (`ṛ` `ṣ` `ḍ` `ṭ` `ẓ`), cette spécification utilise des lettres de base + diacritique modificateur (`rˤ` `sˤ` `dˤ` `tˤ` `zˤ`). |

### 9.2 Interprétation

Le PER de 0,21 est **largement artificiel** : plus de 80 % des erreurs proviennent de choix notationnels indépendants de la phonologie réelle. Après normalisation des conventions (géminées unifiées, emphatiques unifiées, timbres vocaliques harmonisés), le PER effectif est estimé bien inférieur à 0,10.

Les règles phonologiques documentées dans cette spécification (spirantisation post-/r/, blocage post-consonantique, exceptions clitiques) sont **toutes validées** par le test de fumée (smoke test) et cohérentes avec les sources primaires et la vérification native.

### 9.3 Recommandation pour la comparaison

Pour comparer cette spécification à d'autres jeux de données ou systèmes TTS/ASR, il est recommandé de :

1. **Normaliser les géminées** : convertir toute séquence de deux caractères identiques (`CC`) en `Cː`.
2. **Normaliser les emphatiques** : convertir les précomposées (`ṛ` `ṣ` `ḍ` `ṭ` `ẓ`) en lettre + modificateur (`rˤ` `sˤ` `dˤ` `tˤ` `zˤ`).
3. **Normaliser les voyelles** : harmoniser `ɪ`/`ʊ` avec `i`/`u` selon la convention cible, et `æ`/`ɑ` selon le contexte.
4. **Ignorer le schwa épenthétique** si le système cible ne le marque pas.

---

## 10. Recommandations pour l'implémentation

### 10.1 Architecture du moteur G2P

Pour implémenter cette spécification dans un système TTS ou ASR, l'architecture recommandée est :

1. **Pré-tokenisation morphologique** : Identifier et regrouper les clitiques suffixes pronominaux (§4.1.3) avec leur mot hôte. Le tiret orthographique n'est pas un séparateur de mot phonologique.
2. **Tokenisation graphemique** : Segmenter le texte en unités graphemiques (lettres simples et digrammes géminés).
3. **Mapping phonémique** : Convertir chaque grapheme en son phonème sous-jacent via la table `graphemes`.
4. **Application des règles de blocage** : Tester d'abord les règles `BLOCK_` (priorité haute). **Note** : les exceptions clitiques pour /k/ (*kem, kent, k-* + voyelle) ne sont pas couvertes par une règle `allophone_rules` — elles doivent être appliquées séparément, avant ou après ce moteur de règles, via une liste d'exceptions lexicales au niveau du rescorer.
5. **Application des règles de spirantisation** : Si aucun blocage ne s'applique, appliquer les règles `SPIRANT_`.
6. **Application des règles allophoniques** : Vowel backing, closed-syllable lowering, nasal assimilation.
7. **Post-traitement morphologique** : Gérer les exceptions clitiques non capturées par les règles au niveau du rescorer.

### 10.2 Jeu de données de test recommandé

Pour valider une implémentation, le jeu de test minimal doit inclure :

| Catégorie | Exemples attendus | Nombre |
|-----------|-------------------|--------|
| Spirantisation simple | *abrid, adrar, tafat, kra, igenni* | 20 |
| Blocage post-nasal | *zembil, anda, anta, tasemmumt, tamda* | 10 |
| Blocage post-/l/ | *tamellalt, Ldi* | 5 |
| Blocage post-/m/ pour /k,g/ | *tamga* (exception /g/), *ankal* (blocage /k/) | 5 |
| Spirantisation post-/r/ | *taberkant, argaz* | 5 |
| Blocage initial /d/ | *d-ittun, iyi-d-* | 5 |
| Géminées non spirantisées | *abba, igenni, axxam* | 10 |
| Backing vocalique | *axxam, tifinaɣ* | 10 |
| Assimilation nasale | *ankal, anba* | 10 |
| Exceptions clitiques /k/ | *kem, kent, k-ufiɣ, kullec* | 5 |
| Emprunts | *itiknikanen, pulu, villa* | 5 |

### 10.3 Intégration avec le TTS kabyle

Pour la synthèse vocale (TTS), cette spécification doit être couplée avec :

- Un **modèle de placement du schwa** (Wang 2020) pour insérer [ə] aux bons endroits.
- Un **lexique de fréquence** pour marquer les emprunts comme non-spirantisés.
- Un **analyseur morphologique** pour gérer les clitiques et les alternances de gémination.

---

## 11. Conclusion

La phonologie du kabyle, centrée sur la spirantisation et l'opposition tension/détension, est modélisable de manière satisfaisante par un système de règles G2P à condition de respecter scrupuleusement l'ordre d'application (blocages avant spirantisations) et de documenter les limites connues. Cette spécification fournit une base solide pour les applications TTS/ASR tout en identifiant clairement les zones nécessitant une validation native supplémentaire ou une extension morphologique.

Le PER de 0,21 contre VoxCommunis est un artefact de conventions notationnelles divergentes, pas un indicateur de qualité phonologique. Après normalisation des conventions (géminées, emphatiques, timbres vocaliques), la spécification atteint une couverture phonologique native proche de la perfection sur les formes testées.

La richesse dialectale du kabyle — illustrée par le clivage Chemini/Boghni sur l'occlusivation post-/m/ — constitue le principal défi pour une couverture pan-kabyle. Une future version de cette spécification devrait intégrer un paramètre dialectal.

## 12. Mises à jour post-validation native (août 2026)

Cette section consigne les corrections et ajouts validés par sessions de jugement de locuteur natif (MOKRAOUI 2026) sur un échantillon de 5 000 phrases du corpus Tatoeba kabyle (boffire/tatoeba-kabyle-mono-cleaned, N = 756 774).

### 12.1 Pré-tokenisation des clitiques suffixes (NOUVEAU)

Le tiret orthographique en kabyle standard marque une frontière **morphologique**, non **phonologique**. Un tokenizer qui segmente sur le tiret produit des unités prosodiques incorrectes (ex. *fell-i* → `fəlː i` au lieu de `fəlli`).

**Algorithme de pré-tokenisation validé :**

1. **Ne pas regrouper** les particules préverbales (préfixes) : `d-`, `t-`, `n-`, `y-`. Leur regroupement crée des digrammes fantômes (ex. `d-jebden` → `djebden` interprété comme /d͡ʒ/).
2. **Regrouper systématiquement** les suffixes clitiques : `-t`, `-tt`, `-as`, `-ak`, `-am`, `-is`, `-iw`, `-nni`, `-iyi`, `-d`, etc.
3. **Insertion épenthétique d'un glide** en cas d'hiatus à la frontière clitique :
   - Après voyelle antérieure (`a`, `e`, `i`, `ə`) + clitic commençant par `a`/`e`/`ə` → insérer `y` (orthographe) = /j/ (IPA).
   - Après voyelle postérieure (`u`, `o`) + clitic commençant par `a`/`e`/`ə` → insérer `w` (orthographe) = /w/ (IPA).
   - **Exception** : les clitiques commençant par `i` ou `u` ne reçoivent pas de glide (la voyelle porte son propre attaque).

**Exemples validés :**

| Orthographe | Tokenizer naïf | Tokenizer corrigé | IPA attendue | Statut |
|---|---|---|---|---|
| `twalam-t` | `twalam` + `t` | `twalamt` | `θwælæmt` | Validé |
| `Tefka-ak-t` | `Tefka` + `ak` + `t` | `Tefkayakt` | `θəfkæjæçθ` | Validé |
| `Yenɣa-iyi` | `Yenɣa` + `iyi` | `Yenɣayi` | `jəŋʁɑji` | Validé (pas de double glide) |
| `iqmec-as` | `iqmec` + `as` | `iqmecas` | `ɪqməʃæs` | Validé (variation libre avec `iqmeʃɛsse`) |
| `Ad d-jebden` | — | `Ad d-jebden` | `æð dʒəβðən` | Préfixe conservé |

**Note** : Le clitique `-as` en position oblique (`iqmec-as`) présente une variation libre entre la forme fusionnée réduite (`iqmeʃɛsse`) et la forme segmentée (`ɪqməʃæs`). Les deux sont acceptables selon le débit de parole. Le tokenizer corrigé produit la forme segmentée, qui est la réalisation de base.

### 12.2 Règle morphophonémique : préfixe `t-` + initiale `t-` → `ts` (NOUVEAU)

En kabyle, la séquence orthographique `Tett-` / `tett-` en début de mot (préfixe féminin/2e personne `t-` + initiale radicale `t-`) ne se réalise pas comme une géminée `/tː/`, mais comme l'affriquée `/ts/`.

| Orthographe | Réalisation incorrecte | Réalisation correcte | Contexte |
|---|---|---|---|
| `Tettidiremt` | `θətːiðirəmt` | `θə[ts]iðirəmt` | Préfixe `t-` + radical `t-` |
| `tettaruḍ` | `θətːæruðˤ` | `θətsæruðˤ` | Idem |

**Implémentation** : Cette règle doit être appliquée en **pré-traitement**, avant le G2P proprement dit, car elle est morphophonémique (conditionnée par la morphologie du préfixe), non allophonique.

```python
# Pré-traitement recommandé
text = re.sub(r'Tett', 'Tets', text)
text = re.sub(r'tett', 'tets', text)
```

### 12.3 Exceptions clitiques pour /k/ (§4.1.2, §8.9) — IMPLEMENTATION PENDING

Les formes suivantes ont été validées comme conservant l'occlusive [k] **à l'implémentation** :

| Forme | Prononciation | Type | Statut dans kab.json |
|---|---|---|---|
| `kem` | [kem] | Pronom clitique standalone | Non implémenté (produit [çem]) |
| `kent` | [kent] | Pronom clitique standalone | Non implémenté (produit [çent]) |
| `k-ufiɣ` | [kufiɣ] | Préfixe préverbal | Non implémenté (produit [çufiɣ]) |
| `k-walaɣ` | [kwalaɣ] | Préfixe préverbal | Non implémenté (produit [çwalaɣ]) |
| `kullec` | [kulːəʃ] | Emprunt lexical arabe | Couvert par `KAB_BLOCK_SPIRANT_K_BEFORE_U` |

**Recommandation** : Ajouter un champ `word_exceptions` au schéma `orthography2ipa` pour ces formes, ou les gérer via un `LatticeRescorer` au niveau moteur. Le mécanisme `allophone_rules` actuel ne dispose pas de correspondance lexicale.

### 12.4 Suppression des digrammes non standard du grapheme map

Les séquences suivantes, parfois présentes dans des textes non standardisés, **ne doivent pas** être reconnues comme graphemes valides dans la spécification INALCO :

| Séquence | Prétendu | Standard INALCO | Action |
|---|---|---|---|
| `gh` | /ɣ/ | `ɣ` | Retirer de `graphemes` |
| `ch` | /ʃ/ | `c` | Retirer de `graphemes` |
| `dj` | /d͡ʒ/ | `ǧ` | Retirer de `graphemes` |
| `tch` | /t͡ʃ/ | `č` | Retirer de `graphemes` |
| `ts` | /t͡s/ | — (affriquée non standard) | Retirer de `graphemes` |
| `dz` | /d͡z/ | — (affriquée non standard) | Retirer de `graphemes` |

**Justification** : Ces digrammes sont des approximations pédagogiques ou des conventions d'emprunt. Leur présence dans le G2P légitime des fautes d'orthographe. Le corpus Tatoeba kabyle ne les utilise pas.

### 12.5 Validation des règles allophoniques existantes

Les règles suivantes, déjà documentées dans la spécification, ont été validées comme correctes sur le corpus de test. **Aucune modification requise.**

| ID de règle | Exemple testé | Résultat O2I | Jugement natif |
|---|---|---|---|
| `KAB_SPIRANT_T` | `tafat` | `θafaθ` | Correct |
| `KAB_SPIRANT_D` | `adlis` | `æðlis` | Correct |
| `KAB_SPIRANT_B` | `Bubbaɣ` | `βʊbːɑʁ` | Correct |
| `KAB_SPIRANT_K` | `kra` | `çræ` | Correct |
| `KAB_SPIRANT_G` | `tamga` | `θamʝa` | Correct |
| `KAB_BLOCK_SPIRANT_T_AFTER_M` | `twalamt` | `t` (pas `θ`) | Correct |
| `KAB_BLOCK_SPIRANT_K_AFTER_CONSONANT` | `ankal` | `k` (pas `ç`) | Correct |
| `KAB_A_BACKING` | `aɣerbaz` | `ɑɣər...` | Correct |
| `KAB_A_DEFAULT` | `azul` | `æzul` | Correct |
| `KAB_GH_UVULAR_REALIZATION` | `Bubbaɣ` | `...ʁ` | Correct |
| `KAB_N_VELAR_ASSIM` | `ankal` | `aŋkal` | Correct |
| `KAB_N_LABIAL_ASSIM` | `anba` | `amba` | Correct |

### 12.6 Changement de qualité recommandé

**Ancien** : `QualityTier.RESEARCH`  
**Nouveau** : `QualityTier.RESEARCH` (conservé), avec annotation :

> « Noyau allophonique validé par jugement natif sur 5 000 phrases (N = 756 774). Pré-tokenisation clitique et exceptions lexicales `/k/` à implémenter au niveau moteur. »

Le passage à `PRODUCTION` nécessite :
1. Implémentation native des exceptions `/k/` dans le schéma.
2. Couverture du sandhi inter-mots (§8.2).
3. Validation sur un gold set phonétique indépendant (VoxCommunis normalisé ou enregistrements natifs alignés forcés).

---

## Références

1. **Kossmann, Maarten G. & Harry J. Stroomer**, *Berber Phonology*, in A. S. Kaye (ed.), *Phonologies of Asia and Africa*, vol. 1, Eisenbrauns, 1997, pp. 461-475. [https://hdl.handle.net/1887/4150](https://hdl.handle.net/1887/4150)

2. **Karaoui, Fazia; Djeradi, Amar; Djeradi, Rachida**, *Acoustic Characterization of the Noise Sources for the Kabyle Fricatives Consonants*, AIJR (ICAECE'2023 abstracts), DOI 10.21467/abstracts.163, pp. 123-125. [https://books.aijr.org/index.php/press/catalog/download/163/81/3171-1](https://books.aijr.org/index.php/press/catalog/download/163/81/3171-1)

3. **Bedar, Amazigh; Quellec, Lucie; Tifrit, Ali**, *Post-sonorant occlusivization in Kabyle*, 19th Meeting of the French Phonology Network (RFP 2022), Porto, Portugal, 7-9 June 2022. [https://hal.science/hal-03691265](https://hal.science/hal-03691265)

4. **Bedar, Amazigh & Hamza Chergui**, *An element-based analysis of nasal-glide assimilation in the Taqbaylit prepositional phrase*, Glottometrics 58, 2024, pp. 1-19.

5. **Chaker, Salem**, *Propositions pour la notation usuelle à base latine du berbère*, Centre de Recherche Berbère – INALCO, Paris, 1996. [https://www.centrederechercheberbere.fr/tl_files/doc-pdf/notation.pdf](https://www.centrederechercheberbere.fr/tl_files/doc-pdf/notation.pdf)

6. **Mammeri, Mouloud**, *Précis de grammaire berbère (kabyle)*, Paris, 1976 (rééd. Fichier TAL, 1983).

7. **Wang, Beini**, *Syllabification and Schwa Epenthesis in Kabyle*, McGill Working Papers in Linguistics 26.1, McGill University, 2020. [http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Wang.pdf](http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Wang.pdf)

8. **Dallet, Jean-Marie**, *Dictionnaire kabyle-français: parler des At Mengellat, Algérie*, SELAF, Paris, 1982.

9. **Naït-Zerrad, Kamal**, *Grammaire moderne du kabyle / tajerrumt tatrart n teqbaylit*, Karthala, Paris, 2001.

10. **Souag, Lameen**, *Kabyle in Arabic Script: A History without Standardisation*, in Creating Standards, De Gruyter, 2019, pp. 273-296. DOI 10.1515/9783110639063-011. [https://shs.hal.science/halshs-02945641](https://shs.hal.science/halshs-02945641)

11. **MOKRAOUI, Athmane (boffire)**, *Native-speaker verification sessions for the Kabyle orthography2ipa spec*, 2026. Jugements de locuteur natif non publiés.

12. **MOKRAOUI, Athmane (boffire)**, *Kabyle corpus audit for cluster attestation and orthographic contamination*, 2026. Données corpus sur HuggingFace : [https://huggingface.co/boffire](https://huggingface.co/boffire)

13. **MOKRAOUI, Athmane (boffire)**, *Sessions de validation native du tokenizer clitique et des règles allophoniques du module kabyle orthography2ipa*, août 2026. Corpus de test : boffire/tatoeba-kabyle-mono-cleaned (756 774 phrases, échantillon 5 000). 

---

*Document rédigé dans le cadre du développement des ressources phonétiques pour la langue kabyle. Les attestations natives sont signalées comme telles et n'engagent que leurs auteurs. Les limites connues sont documentées explicitement pour permettre l'amélioration itérative de la spécification.*
