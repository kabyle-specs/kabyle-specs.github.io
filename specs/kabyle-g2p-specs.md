# La phonologie du kabyle (Taqbaylit) en transcription phonétique internationale (API) : spécification orthography2ipa

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; structuration phonologique, recherche documentaire et vérification native.

**Date** : 27 juillet 2026

**Version** : 2026-07-27

**Cible** : Linguistes computationnels, développeurs TTS/ASR, ingénieurs phonétiques, chercheurs en linguistique berbère.

---

## Résumé

Le kabyle (Taqbaylit, code ISO 639-3 `kab`) possède un système phonologique caractéristique du berbère nordique, dont la marque distinctive est la **spirantisation** (assourdissement des occlusives lâches en fricatives) et une riche allophonie vocalique conditionnée par l'environnement consonantique. Ce document expose la spécification complète du grapheme-to-phoneme (G2P) pour l'alphabet latin berbère standard (INALCO 1996), avec un inventaire graphemique de 34 lettres, des règles allophoniques détaillées, et douze limites documentées pour les extensions futures.

**Mots-clés** : kabyle, taqbaylit, phonologie, API, spirantisation, allophonie, G2P, orthography2ipa, TTS, ASR.

---

## 1. Introduction

Le kabyle est une langue chamito-sémitique (afro-asiatique) du groupe berbère, parlée par 5 à 7 millions de locuteurs en Algérie (Kossmann & Stroomer 1997, p. 461). Sa phonologie se distingue par :

1. **Une opposition tension/détension** (consonnes longues vs. consonnes brèves), notée par le redoublement de la lettre.
2. **La spirantisation** : les occlusives lâches /b d t k g ḍ/ deviennent des fricatives [β ð θ ç ʝ ðˤ] en position post-vocalique et initiale (sauf exceptions).
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
Sessions de jugement de locuteur natif (Athmane Mokraoui) pour la spirantisation de /b d t k g/ en position initiale, post-vocalique et post-consonantique. Décisions clés : blocage post-consonantique catégorique pour /k/ et /g/ avec l'exception /mʝ/ ; spirantisation de /t/ initiale devant toutes les voyelles y compris /i/ ; rejet de l'allophone [ʃ] pour /k/ ; confirmation de la spirantisation post-/r/ pour /k/ et /g/.

---

## 3. Inventaire graphemique

Le kabyle standard utilise l'alphabet latin berbère de 34 lettres (INALCO 1996). Ce document couvre l'ensemble des graphemes simples et géminés.

### 3.1 Voyelles et semi-voyelles

| Grapheme | Phonème | Exemple | Réalisation |
|----------|---------|---------|-------------|
| a | /a/ | *axam* | [ɑχːɑm] |
| e | /ə/ | *tameṭṭut* | [θametˤːθ] |
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
| k | /k/ | vélaire occlusive | Spirantise en [ç] |
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
| bb | /bː/ | [bː] | Jamais spirantisée |
| dd | /dː/ | [dː] | Jamais spirantisée |
| tt | /tː/ | [tː] | Jamais spirantisée |
| kk | /kː/ | [kː] | Jamais spirantisée |
| gg | /ɡː/ | [ɡː] | Jamais spirantisée |
| ḍḍ | /dˤː/ | [dˤː] | Jamais spirantisée |
| ṭṭ | /tˤː/ | [tˤː] | Jamais spirantisée |
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

La spirantisation est le processus par lequel les occlusives **lâches** (non-géminées) deviennent des fricatives. Les occlusives **tendues** (géminées) résistent systématiquement.

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
| KAB_BLOCK_SPIRANT_D_AFTER_M | /d/ | Après /m/ | *θamda* [θamda] |
| KAB_BLOCK_SPIRANT_D_WORD_INITIAL | /d/ | Initiale absolue | *d-ittun* [dittun] |
| KAB_BLOCK_SPIRANT_K_AFTER_CONSONANT | /k/ | Après toute consonne **sauf** /r, rˤ/ | *taberkant* [ç] mais *ankal* [ankal] |
| KAB_BLOCK_SPIRANT_G_AFTER_CONSONANT | /ɡ/ | Après toute consonne **sauf** /m, r, rˤ/ | *argaz* [ʝ] mais *angal* [aŋɡal] |

**Note sur /k/ et /ɡ/ après /r/** : Bedar et al. (2022) ne traitent pas le contexte post-/r/ pour /k/ et /ɡ/. La vérification native (MOKRAOUI 2026) confirme la spirantisation : *taberkant* → [ç], *argaz* → [ʝ].

### 4.2 Allophonie vocalique

#### 4.2.1 Retraction de /a/

| Règle | Contexte | Réalisation | Exemple |
|-------|----------|-------------|---------|
| KAB_A_BACKING | Avant emphatique/uvulaire | [ɑ] | *axxam* [ɑχːɑm] |
| KAB_A_BACKING_PRECEDING | Après emphatique/uvulaire | [ɑ] | *qaci* [ɑ] (si /a/ suit /q/) |
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
| KAB_SPIRANT_K | Spirantisation | /k/ | [ç] | Post-vocalique, initiale, post-/r/ | Basse |
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
| *tameṭṭut* | /t-a-m-e-tˤː-u-t/ | SPIRANT_T (×2), A_DEFAULT | [θametˤːθ] | femme |
| *igenni* | /i-ɡ-e-nː-i/ | SPIRANT_G | [iʝənni] | ciel |
| *kra* | /k-r-a/ | SPIRANT_K, A_DEFAULT | [çræ] | quelque |
| *kullec* | /k-u-lː-e-c/ | SPIRANT_K | [çʊlːəʃ] | tout |
| *kečč* | /k-e-cː/ | SPIRANT_K | [çət͡ʃː] | toi (emphatique) |
| *taberkant* | /t-a-b-e-r-k-a-n-t/ | SPIRANT_T (×2), SPIRANT_K (post-/r/), A_DEFAULT | [θabərçant] | (nom propre) |
| *argaz* | /a-r-ɡ-a-z/ | SPIRANT_G (post-/r/), A_DEFAULT | [arʝaz] | homme |
| *tamga* | /t-a-m-ɡ-a/ | SPIRANT_T, SPIRANT_G (post-/m/), A_DEFAULT | [θamʝa] | source |
| *zembil* | /z-e-m-b-i-l/ | BLOCK_B_AFTER_M, A_DEFAULT | [zəmbil] | panier |
| *anda* | /a-n-d-a/ | BLOCK_D_AFTER_N, A_DEFAULT | [anda] | où |
| *anta* | /a-n-t-a/ | BLOCK_T_AFTER_N, A_DEFAULT | [anta] | laquelle (f.) |
| *tamellalt* | /t-a-m-e-lː-a-l-t/ | SPIRANT_T (initiale), BLOCK_T_AFTER_L_FINAL, A_DEFAULT | [θaməllalt] | œuf |
| *Ldi* | /l-d-i/ | BLOCK_D_AFTER_L | [ldi] | ouvre ! |
| *tasemmumt* | /t-a-s-e-m-m-u-m-t/ | SPIRANT_T (initiale), BLOCK_T_AFTER_M | [tasemmumt] | (nom propre) |
| *θamda* | /θ-a-m-d-a/ | BLOCK_D_AFTER_M, A_DEFAULT | [θamda] | mare |
| *d-ittun* | /d-i-tː-u-n/ | BLOCK_D_WORD_INITIAL | [dittun] | ils sont venus |
| *tifinaɣ* | /t-i-f-i-n-a-ɣ/ | SPIRANT_T (initiale), GH_UVULAR, A_BACKING_PRECEDING | [θifinɑʁ] | tifinagh |
| *bbeɣ* | /bː-e-ɣ/ | GH_UVULAR | [bːəʁ] | plonger |
| *axxam* | /a-xː-a-m/ | A_BACKING (avant /χː/), A_BACKING_PRECEDING (après /χː/) | [ɑχːɑm] | maison |
| *ankal* | /a-n-k-a-l/ | N_VELAR_ASSIM, BLOCK_K_AFTER_CONSONANT, A_DEFAULT | [aŋkal] | (nom propre) |
| *anba* | /a-n-b-a/ | N_LABIAL_ASSIM | [amba] | fils de... |

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

### 8.2 Sandhi aux frontières de mots
Les processus de sandhi inter-mots ne sont pas modélisés :
- /n/ + /w/ → [bb] ou [pp]
- /d/ + /t/ → [ts]

**Statut** : Nécessite une segmentation en mots et une analyse syntaxique.

### 8.3 Allophonie vocalique complète
Les réalisations [e] pour /i/ et [o] pour /u/ ne sont que partiellement couvertes. Leur distribution exacte (proximité pharyngale, position dans le mot, influence du schwa adjacent) n'est pas modélisée.

**Statut** : Nécessite une analyse syllabique complète.

### 8.4 Accent
L'accent kabyle est largement prédictible (tonicité sur la dernière syllabe ou l'avant-dernière selon la structure) et non distinctif. Il n'est pas marqué dans cette spécification.

**Statut** : Non prioritaire pour la plupart des applications TTS.

### 8.5 Emprunts récents
Les emprunts français récents (surtout techniques) peuvent échapper à la spirantisation et conserver leurs occlusives. Cette spécification modélise la phonologie native de manière catégorique.

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

### 8.9 Clitique /k/
Le pronom objet /k/ (k-, -k, kem-, kent-, ak-, akem-, akent-) conserve l'occlusive [k] en parole naturelle, mais c'est une exception morphologique, pas une règle phonologique générale. Le /k/ initial lexical (*kra*, *kullec*, *kečč*) spirantise correctement en [ç].

**Statut** : Doit être géré au niveau du moteur/rescorer, pas par une règle phonologique globale.

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

## 9. Recommandations pour l'implémentation

### 9.1 Architecture du moteur G2P

Pour implémenter cette spécification dans un système TTS ou ASR, l'architecture recommandée est :

1. **Tokenisation graphemique** : Segmenter le texte en unités graphemiques (lettres simples et digrammes géminés).
2. **Mapping phonémique** : Convertir chaque grapheme en son phonème sous-jacent via la table `graphemes`.
3. **Application des règles de blocage** : Tester d'abord les règles `BLOCK_` (priorité haute).
4. **Application des règles de spirantisation** : Si aucun blocage ne s'applique, appliquer les règles `SPIRANT_`.
5. **Application des règles allophoniques** : Vowel backing, closed-syllable lowering, nasal assimilation.
6. **Post-traitement morphologique** : Gérer les exceptions clitiques (/k/ objet) au niveau du rescorer.

### 9.2 Jeu de données de test recommandé

Pour valider une implémentation, le jeu de test minimal doit inclure :

| Catégorie | Exemples attendus | Nombre |
|-----------|-------------------|--------|
| Spirantisation simple | *abrid, adrar, tafat, kra, igenni* | 20 |
| Blocage post-nasal | *zembil, anda, anta, tasemmumt, θamda* | 10 |
| Blocage post-/l/ | *tamellalt, Ldi* | 5 |
| Blocage post-/m/ pour /k,g/ | *tamga* (exception /g/), *ankal* (blocage /k/) | 5 |
| Spirantisation post-/r/ | *taberkant, argaz* | 5 |
| Blocage initial /d/ | *d-ittun, iyi-d-* | 5 |
| Géminées non spirantisées | *abba, igenni, axxam* | 10 |
| Backing vocalique | *axxam, tifinaɣ* | 10 |
| Assimilation nasale | *ankal, anba* | 10 |
| Emprunts | *itiknikanen, pulu, villa* | 5 |

### 9.3 Intégration avec le TTS kabyle

Pour la synthèse vocale (TTS), cette spécification doit être couplée avec :

- Un **modèle de placement du schwa** (Wang 2020) pour insérer [ə] aux bons endroits.
- Un **lexique de fréquence** pour marquer les emprunts comme non-spirantisés.
- Un **analyseur morphologique** pour gérer les clitiques et les alternances de gémination.

---

## 10. Conclusion

La phonologie du kabyle, centrée sur la spirantisation et l'opposition tension/détension, est modélisable de manière satisfaisante par un système de règles G2P à condition de respecter scrupuleusement l'ordre d'application (blocages avant spirantisations) et de documenter les limites connues. Cette spécification fournit une base solide pour les applications TTS/ASR tout en identifiant clairement les zones nécessitant une validation native supplémentaire ou une extension morphologique.

La richesse dialectale du kabyle — illustrée par le clivage Chemini/Boghni sur l'occlusivation post-/m/ — constitue le principal défi pour une couverture pan-kabyle. Une future version de cette spécification devrait intégrer un paramètre dialectal.

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

---

*Document rédigé dans le cadre du développement des ressources phonétiques pour la langue kabyle. Les attestations natives sont signalées comme telles et n'engagent que leurs auteurs. Les limites connues sont documentées explicitement pour permettre l'amélioration itérative de la spécification.*