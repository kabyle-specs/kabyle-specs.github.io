# Spécification GEC (correction grammaticale automatique) pour le kabyle (Taqbaylit)

- **Identifiant :** `kabyle-gec-spec`
- **Version :** 0.6-draft
- **Statut :** Draft — proposition de spécification technique, non normative tant qu'aucun jeu de test annoté n'a produit de métriques (cf. §6)
- **Auteurs :** Athmane Mokraoui (boffire) ; structuration algorithmique et rédaction assistée par IA
- **Date :** 12 septembre 2026
- **Code ISO 639-3 :** `kab`
- **Dépendances :**
  - `kabyle-orthography-specs.md`
  - `kabyle-negation-spec.md`
  - `kabyle-nominal-state-specs.md`
  - `kabyle-pronouns-clitics-specs.md`
  - `kabyle-conjugations-specs.md`
  - `kabyle-morphological-tokenizer-spec.md`
  - `kabyle-lemmatization-spec.md`
  - `kabyle-ud-specification.md`

**Convention épistémique** (reprise des specs existantes du dépôt) : `verified` / `[attesté]` = confirmé par une source faisant autorité · `candidate` = généralisé à partir d'une source, à confirmer · `[NEEDS REVIEW]` = hypothèse de travail non attestée · `[disputed]` = point sur lequel les sources ou traditions divergent, à ne jamais trancher arbitrairement.

---

## Changelog

### v0.6-draft (12 septembre 2026 — ajout de la politique anti-hallucination et abstention)
- Ajout du §3.3, qui définit les règles de non-invention, les conditions d'abstention, les modes `conservative`/`standard`/`research`, le contrat de sortie et les tests anti-hallucination.
- Le mode `conservative` est recommandé par défaut afin de préserver les variantes attestées et les formes inconnues.

### v0.5-draft (12 septembre 2026 — résultat négatif sur l'hypothèse `raw_text`/`text`)
- **Stratégie 0 du §4.1 réfutée par l'expérience.** `raw_text` et `text` sont identiques sur 756 756/756 774 lignes (99,998 %). Les 18 différences restantes sont uniquement des normalisations d'espaces/tabulations/retours à la ligne (`'  '` → `' '`, `\n` → ` `, `\tkab\t` → ` kab `) — **aucune correction grammaticale ou orthographique réelle**. `raw_text` n'est donc pas le texte « avant nettoyage orthographique » espéré, seulement le texte avant normalisation des espaces. Cette source est abandonnée pour la constitution de paires GEC (voir §4.1, révisé).
- **L'énigme des 3,17 %/3,66 % reste non résolue.** La contamination par homoglyphes est de 0,00 % sur `raw_text` **et** sur `text` — le nettoyage orthographique a donc eu lieu en amont de ce dépôt HuggingFace, à une étape non conservée dans `boffire/tatoeba-kabyle-mono-cleaned`. Les deux chiffres cités historiquement décrivent probablement un autre instantané du corpus ou un des datasets bruts listés dans le projet d'entraînement (`boffire/kabyle-corpus` notamment) — à vérifier sur ces sources séparément si le chiffre exact importe.
- **Conclusion opérationnelle pour §4.1** : ce corpus mono déjà nettoyé ne contient pas de paires (fautif, correct) réelles exploitables — confirmation empirique de la limite déjà notée en §7.3 (« le corpus est déjà nettoyé »). La génération synthétique par injection de bruit contrôlé (stratégie 2, ci-dessous) redevient donc la voie prioritaire à court terme, plutôt que l'espoir d'une source gratuite.

### v0.4-draft (12 septembre 2026 — premiers résultats expérimentaux sur `boffire/tatoeba-kabyle-mono-cleaned`, 756 774 phrases)
- **Découverte structurelle majeure (§4.1)** : le corpus contient une colonne `text` (nettoyée) **et** une colonne `raw_text` (originale), en plus de métadonnées de détection de langue (`glot_lang`, `berber_label`, `dominant_lang`...). C'est une source de paires (fautif, correct) déjà alignées, gratuite, à exploiter en priorité avant toute génération synthétique — voir §4.1 point 0 (nouveau).
- **§2.4 (contamination orthographique) — chiffre reproductible, mais pas celui attendu.** Sur la colonne `text` : **0,00 %** de contamination par homoglyphes (0/756 774). Les chiffres 3,17 %/3,66 % cités jusqu'ici décrivaient donc très probablement le corpus **avant** le nettoyage qui a produit `text` — hypothèse à confirmer en relançant la même mesure sur `raw_text` (voir §4.1 point 0). En l'état, ni 3,17 % ni 3,66 % n'est confirmé sur les données que le pipeline GEC verrait réellement en entrée si le nettoyage en amont reste identique.
- **§2.1 (état nominal, classe invariable en `ta-`) — extension et une correction.** `tudert` (2505/2505, 100 %) et `tikkelt` (1774/1775, 99,9 %) rejoignent la classe invariable avec un support statistique aussi solide que les 6 membres déjà `verified` ; `tegnit` confirmé (612/612, 100 %). `tasga`, déjà dans la liste, se révèle à 87,8 % seulement (352/401) — non-catégorique. **Correction potentielle à remonter à `kabyle-nominal-state-specs.md`** : `tadla` (déjà listée comme invariable, L05) n'obtient que 21,1 % de non-mutation (4/19) — la majorité des occurrences **mutent**, ce qui contredit directement son statut actuel dans la spec d'état.
- **§2.2 (ordre des clitiques) — réplication indépendante.** Extraction indépendante : 32 026 DAT-avant-ABS vs 28 ABS-avant-DAT (contre 32 026/26 dans `kabyle-pronouns-clitics-specs.md`, écart de 2 dû à des cas limites de tokenisation). Mêmes verbes en cause (`awi`, `fket`/`fkemt`, `Semceḥem`/`semcaḥem`, `Ini` à l'impératif ; `fukken`, `ttaran`, `Ɣlin` à l'indicatif) — **aucun nouveau verbe** ne rejoint le groupe `[disputed]` du §8.3 de la spec clitiques. Le statut `verified` de la règle générale en sort renforcé par une deuxième extraction indépendante.
- **§2.3 (taux de `ara`) — écart notable avec Mettouchi 2021 à investiguer.** Mesure brute sur notre corpus : 63,2 % des phrases avec `ur` contiennent aussi `ara` (21 797/34 480), contre ~52 % rapporté par Mettouchi 2021. **Cette mesure est probablement biaisée par une limite méthodologique connue** : elle compte la co-occurrence dans la phrase entière, pas dans la même proposition — une phrase à plusieurs propositions peut faire compter un `ara` sans lien avec le `ur` détecté. Un nouveau chiffre fiable nécessite une analyse au niveau de la proposition (dépend du tokenizer morphologique/parseur, pas seulement de regex).
- **§2.2 (`ara`/`wara`) — précision apportée, statut `[NEEDS REVIEW]` maintenu mais affiné.** `wara` existe bien dans le corpus (194 occurrences). Sur un échantillon de 10, la quasi-totalité suit directement le verbe `yuɣ` (« arriver à, affecter ») ou `iban` (« apparaître ») — motif `ur X-yuɣ wara`. Ceci suggère une **collocation lexicalisée avec un petit nombre de verbes**, plutôt qu'une opposition sémantique générale `ara`/`wara` applicable à toute négation comme le suggérait la v0.1. Confirmation par locuteur natif toujours nécessaire avant toute règle de correction.
- **§2.6 (choix lexical) — chiffres réels.** `amnafeq` : 7 occurrences ; `anazbay` : 1 occurrence (non nul, contrairement à l'hypothèse implicite de la v0.1-v0.3). Les deux sont donc attestés dans le corpus, mais avec un écart de fréquence net qui confirme le statut de `amnafeq` comme forme dominante.
- **Limite méthodologique identifiée (§2.4, ṛ)** : la méthode de comparaison par simple substitution `ṛ`→`r` produit un nombre important de faux positifs — des paires comme `uṛ`/`ur` ou `aṛ`/`ar` opposent en réalité des morphèmes distincts non apparentés (`ur` = négation, `ar` = préposition), pas des variantes phonologiques d'un même mot. Cette expérience ne permet donc pas de sortir `ṛ` du statut `[disputed]` ; elle indique en revanche que certaines formes à forte proportion de `ṛ` (`ṛebbi`, `iṛuḥ`/`iruḥ`, `ẓṛiɣ`/`zriɣ`) pourraient relever d'un phénomène de propagation d'emphase plutôt que d'une variation libre — hypothèse à creuser avec un filtrage par lemme réel, hors de portée d'un script regex seul.

### v0.3-draft (12 septembre 2026 — révision contre la littérature GEC publiée)
- **Motivation renforcée (§1.1)** : ajout d'une preuve empirique externe que la correction orthographique/grammaticale d'un texte berbère améliore mesurablement la qualité de traduction automatique (Öktem et al. 2025, WMT — correction de FLORES+/OLDI Seed en tamazight, +6,05 chrF en→zgh et +2,32 chrF zgh→en après fine-tuning de NLLB sur les données corrigées).
- **Taxonomie (§2, note ajoutée)** : signalement que la taxonomie à 7 classes d'ARETA (Belkebir & Habash 2021, arabe — langue elle aussi morphologiquement riche et à ambiguïté orthographique) constitue un précédent validé, transférable comme grille de plus haut niveau pour l'outillage d'évaluation futur.
- **Données d'entraînement (§4.1)** : ajout de deux stratégies supplémentaires — extraction d'éditions Wikipédia (précédent HiWikiEdits pour le hindi) et correction manuelle d'un corpus de référence existant façon Öktem et al. 2025 (IRCAM/FLORES+/OLDI Seed pour le tamazight).
- **Modélisation (§5.2)** : ajout de précédents directement pertinents — correcteur orthographique amazigh existant (Damerau-Levenshtein + N-gram, El Ouahabi et al., ScienceDirect 2021) et approches à base d'analyseur morphologique à états finis pour langues agglutinantes (Oflazer et al., turc/finnois).
- **Évaluation (§6.1)** : ajout de la critique documentée du score M2 sur les langues morphologiquement riches (sur-regroupement des éditions, sous-estimation des performances — Habash et al.) et recommandation d'un annotateur automatique de types d'erreurs façon ARETA plutôt qu'un ERRANT/GLEU nu ; ajout du CER et de la divergence de tokens (Abdulmumin et al. 2024, repris par Öktem et al. 2025) comme métriques alternatives pour l'évaluation d'un corpus corrigé dans son ensemble.
- **Limites (§7, nouveau point)** : la revue de littérature 2025 sur le GEC low-resource confirme explicitement l'absence de métriques d'évaluation robustes pour les langues agglutinantes/morphologiquement riches — ce n'est donc pas une lacune propre à cette spec mais un problème de recherche ouvert.

### v0.2-draft (12 septembre 2026 — révision contre les specs publiées du dépôt)
- **Correction terminologique majeure (§2.1)** : remplacement du vocabulaire improvisé (« erreurs d'accord/annexion » générique) par la terminologie officielle du dépôt — État Libre (EL, *Addad ilelli*) / État d'Annexion (EA, *Addad amaruz*), trait UD `Definite=Ind|Red` (et non `Case=Construct`, qui n'existe pas dans les guidelines UD) — alignement sur `kabyle-nominal-state-specs.md` v0.2-draft.
- **Correction sourcée (§2.2, §2.3)** : les chiffres de la distribution de `ara` sont repris tels que publiés dans `kabyle-negation-spec.md` (~52 % présent / ~48 % absent, Mettouchi 2021), au lieu de l'approximation « ~48 % vs ~52 % » précédemment citée sans source vérifiée.
- **Signalement d'un point non confirmé (§2.2)** : l'opposition `ara` (renforçateur) / `wara` (négation totale) figurait dans une version précédente de ce document sur la base d'une source non primaire. `kabyle-negation-spec.md` (v0.2.0, section de référence sur `ara`) ne mentionne pas `wara` et documente `ara` uniquement comme homographe renforçateur/modal-aoriste (codes `E013 HOMOGRAPHE_ARA_MISCLASSIFIED`). Ce point est donc rétrogradé au statut `[NEEDS REVIEW]` plutôt que présenté comme un fait établi.
- **Ajout (§2.2)** : nouvelle sous-catégorie d'erreur d'ordre des clitiques, absente de la v0.1 — inversion datif/absolutif, réglée à 99,9 % par `kabyle-pronouns-clitics-specs.md` §8 (Mettouchi 2018), avec le sous-cas d'inversion obligatoire à l'impératif.
- **Format d'annotation (§3)** : le schéma JSON est aligné sur le style déjà en usage dans `kabyle-negation-spec.md` (champs `token`/`lemma`/`pos`/`morph`/`status`/`confidence`/`source`/`error`) et sur le format `{"morpheme", "type", "subtype"}` de `kabyle-morphological-tokenizer-spec.md`, au lieu d'un schéma ad hoc.
- **Codes d'erreur (§8, nouveau)** : réservation d'une plage `E020`–`E029` propre à ce document pour éviter tout conflit avec `E001`–`E013` déjà utilisés par `kabyle-lemmatization-spec.md` et `kabyle-negation-spec.md`.
- **Références** : mise à jour avec les citations exactes vérifiées dans les fichiers sources du dépôt (Mettouchi 2018/2021, Bedar/Quellec/Voeltzel 2021, Achab/Mihuc/Ben Si Saïd 2020, Naït-Zerrad 2001).

### v0.1-draft (11 septembre 2026)
- Version initiale, rédigée à partir du skill `kabyle-language-expert` sans consultation directe des fichiers sources du dépôt `kabyle-specs.github.io/specs/`.

---

## Résumé

Cette spécification décrit un module de correction grammaticale automatique (GEC — *Grammatical Error Correction*) pour le kabyle, conçu comme un **composant modulaire, agnostique du modèle de traduction**, pouvant être branché en pré-traitement ou en post-traitement de n'importe quel moteur de traduction (NLLB, modèle fine-tuné, API commerciale). Le document reprend et référence les règles déjà établies dans les autres spécifications du dépôt (état nominal, négation, pronoms/clitiques, tokenizer morphologique) plutôt que de les redéfinir, et formalise :

1. le périmètre et le contrat d'interface du module (§1) ;
2. une taxonomie des erreurs propre au kabyle (§2) ;
3. un format d'annotation compatible avec le tokenizer morphologique et l'export UD/CoNLL-U (§3) ;
4. une stratégie de constitution de données d'entraînement et d'évaluation (§4) ;
5. des approches de modélisation adaptées à un contexte low-resource (§5) ;
6. une méthodologie d'évaluation (§6) ;
7. les limites connues (§7) et un registre de codes d'erreur (§8).

---

## Table des matières

1. [Périmètre et positionnement](#1-périmètre-et-positionnement)
2. [Taxonomie des erreurs](#2-taxonomie-des-erreurs)
3. [Format de représentation des erreurs](#3-format-de-représentation-des-erreurs)
3.3. [Politique anti-hallucination et abstention](#33-politique-anti-hallucination-et-abstention)
4. [Données d'entraînement et d'évaluation](#4-données-dentraînement-et-dévaluation)
5. [Approches de modélisation](#5-approches-de-modélisation)
6. [Évaluation](#6-évaluation)
7. [Limites connues et zones d'incertitude](#7-limites-connues-et-zones-dincertitude)
8. [Codes d'erreur](#8-codes-derreur)
9. [Références](#9-références)
10. [Historique des versions](#10-historique-des-versions)

---

## 1. Périmètre et positionnement

### 1.1 Ce qu'est le GEC pour une langue comme le kabyle

Le GEC pour l'anglais ou le français traite majoritairement des erreurs de mots isolés sur une morphologie relativement pauvre et une segmentation en mots déjà stable. Le kabyle pose un problème structurellement différent :

- **Langue à clitiques** : les clitiques objets, les particules directionnelles (`-d`/`-n`) et certains marqueurs s'attachent au verbe ou à la préposition par trait d'union documenté (`Ččiɣ-t`, `fell-as`, `ɣur-s`) — `kabyle-pronouns-clitics-specs.md` §3–§9. Une erreur de segmentation ou d'ordre des clitiques est une catégorie d'erreur à part entière, absente du GEC anglais/français classique.
- **Opposition d'état nominal (EL/EA)** : le nom change d'initiale selon qu'il est en État Libre ou en État d'Annexion, un phénomène « typologiquement autonome, non réductible à un cas morphologique classique » (Mettouchi & Frajzyngier 2013, cité par `kabyle-nominal-state-specs.md`). Une correction par simple substitution de caractères est insuffisante : le modèle doit connaître, ou inférer, la matrice syntaxique EL/EA (sujet postverbal, régime prépositionnel, régime génitif en `n`).
- **Système aspectuel, non temporel** : aoriste / prétérit / intensif plutôt que temps grammatical (`kabyle-conjugations-specs.md`, `kabyle-lemmatization-spec.md` §2).
- **Norme variée** : plusieurs formes concurrentes sont attestées comme également correctes (`ičča`/`yečča`, `afus`/`ufus`, `kent`/`went`, présence/absence de `ṛ`). Un correcteur GEC pour le kabyle ne doit **pas** traiter toute variante comme une erreur ; il doit distinguer *variation dialectale/registre légitime* et *erreur*.

**Preuve empirique externe que ce chantier a un impact mesurable** : pour le tamazight (zgh, langue berbère standardisée du Maroc, proche du kabyle par la famille mais distincte), Öktem, Farhi, Essaidi, Jabouja & Boudichat (2025, WMT) ont corrigé manuellement les fautes d'orthographe, translittérations non standard et emprunts mal formés des portions tamazight de FLORES+ et OLDI Seed, puis fine-tuné NLLB-0,6B sur les données corrigées : gain de **+6,05 chrF** en anglais→tamazight et **+2,32 chrF** en tamazight→anglais par rapport au même modèle entraîné sur les données non corrigées. C'est la preuve la plus directe disponible à ce jour que nettoyer la qualité grammaticale/orthographique du texte kabyle, en amont ou en aval d'un moteur de traduction, améliore mesurablement la traduction elle-même — et non un simple gain cosmétique de lisibilité.

### 1.2 Agnosticisme vis-à-vis du modèle de traduction

Contrat d'interface :

```
entrée  : texte kabyle brut (chaîne de caractères, NFC, une ou plusieurs phrases)
sortie  : texte kabyle corrigé + liste d'éditions structurées (voir §3)
```

Le module peut être branché :

- **en pré-traitement**, pour nettoyer une entrée kabyle bruitée avant qu'elle serve de pivot ou de référence ;
- **en post-traitement**, pour corriger la sortie kabyle d'un moteur de traduction quel qu'il soit, uniquement à partir du texte produit, sans connaître son architecture interne ;
- **en évaluation**, comme composant auxiliaire pour quantifier la qualité grammaticale d'une sortie de traduction indépendamment des métriques de similarité sémantique (BLEU/chrF/COMET).

`candidate` — Interface recommandée : une fonction pure `correct(text: str, options: dict) -> GecResult`, où `options` permet d'activer/désactiver des catégories d'erreurs — utile pour désactiver, en pré-traitement, les catégories qui touchent à des points `[disputed]` de la norme variée (voir §7.2).

---

## 2. Taxonomie des erreurs

`candidate` — **Précédent transférable.** Le kabyle n'a, à ce jour, aucun système de GEC ni aucune taxonomie d'erreurs publiée. La langue morphologiquement riche et à ambiguïté orthographique la mieux dotée en la matière est l'arabe standard, via ARETA (*Automatic Error Type Annotation*, Belkebir & Habash 2021, ACL CoNLL), qui structure ses annotations en 7 classes de haut niveau : Orthographique, Morphologique, Syntaxique, Sémantique, Ponctuation, Fusion (*merge*), Scission (*split*). Cette grille est un bon candidat de structuration de plus haut niveau pour un futur outillage d'évaluation automatique du kabyle (au-dessus des sous-catégories fines §2.1–§2.6 ci-dessous, qui restent spécifiques au kabyle et sourcées indépendamment) : Orthographique ↔ §2.4, Morphologique ↔ §2.1/§2.3, Syntaxique ↔ §2.2/§2.5, Sémantique ↔ §2.6. Cette correspondance est proposée à titre d'inspiration structurelle ; elle ne doit pas se substituer aux règles sourcées spécifiquement pour le kabyle.

### 2.1 État nominal : État Libre (EL) vs État d'Annexion (EA)

Reprise directe de `kabyle-nominal-state-specs.md` (Mammeri 1976 ; Chaker 1983/1988/1995 ; Mettouchi & Frajzyngier 2013 ; Naït-Zerrad 2001 ; Achab 2003/2012/2020) : le trait UD à utiliser est `Definite=Ind` (EL) / `Definite=Red` (EA) — il n'existe **pas** de feature UD nommée `Case=Construct` ni `State`.

| Sous-type | Règle violée | Exemple fautif → corrigé | Statut |
|---|---|---|---|
| Sujet postverbal en EL au lieu de l'EA | Le sujet postposé (ordre VSO) doit porter `Definite=Red` | `*Yekcem aqcic.` → `Yekcem weqcic.` | `verified` (TS01 de `kabyle-nominal-state-specs.md`) |
| Sujet préverbal en EA au lieu de l'EL | Le sujet antéposé (topicalisé, SVO) doit porter `Definite=Ind` | `*Weqcic yekcem.` → `Aqcic yekcem.` | `verified` (TS02) |
| Objet direct en EA au lieu de l'EL | L'objet direct porte `Definite=Ind`, à ne pas confondre avec le sujet VSO | `*Iwala weqcic wemcic.` → `Iwala weqcic amcic.` | `verified` (TS03/TS04) |
| Annexion omise après préposition régissante (`ɣer`, `seg`, `n`) | `ar` et `s` directionnel/instrumental n'en déclenchent **pas** ; un numéral seul non plus — c'est le `n` qui gouverne dans `yiwet n tmeṭṭut` | `*Yekcem ɣer axxam.` → `Yekcem ɣer wexxam.` ; `*axxam n ilmeẓiyen` → `axxam n yilmeẓiyen` | `verified` (TS05/TS08, Naït-Zerrad 2001:54, Mammeri 1990:93) |
| Annexion appliquée à tort après `ar`, `s` directionnel, `qbel`, `mebla`, `amzun d` | Ces prépositions **exigent** l'EL | `*Ar wemeddit.` → `Ar tameddit.` | `verified` (TS07, L06 résolu v0.2 de la spec d'état) |
| Copule `d` traitée comme coordinateur (ou l'inverse) | La copule ascriptive `d` exige l'EL ; le coordinateur `d` exige l'EA sur le second terme | `*D wergaz.` → `D argaz.` ; `*Argaz d aqcic.` → `Argaz d weqcic.` | `verified` (TS11/TS12) |
| Mutation appliquée à un invariable | Noms de parenté nus, emprunts figés en `l-`, classe lexicalisée (`taddart`, `tafat`, `tasa`, `tadimt`, `tudert`, `tikkelt`, `tegnit`) ne mutent jamais | `*Yusa-d wbaba.` → `Yusa-d baba.` | `verified` (TS14/TS15/TS16 + expérience corpus 2026-09-12 : `taddart` 99,9 %, `tudert` 100 %, `tikkelt` 99,9 %, `tafat` 99,7 %, `tasa` 100 %, `tegnit` 100 % de non-mutation, n≥600 chacun sauf mention contraire) |

`candidate` — `tasga` (352/401 non-mutée, 87,8 %) reste probablement dans la classe invariable mais n'atteint pas le seuil catégorique des autres membres — à traiter comme `candidate` plutôt que `verified` tant que les 12,2 % de contre-exemples n'ont pas été examinés individuellement (erreur de nettoyage résiduelle vs vraie variation).

`[disputed]` — **`tadla`**, listée comme invariable dans `kabyle-nominal-state-specs.md` §3.3.5 (L05), n'obtient que 21,1 % de non-mutation sur 19 occurrences (échantillon faible mais net) — la majorité mute réellement. Ce résultat contredit le statut actuel de la spec d'état ; à signaler comme correction candidate sur `kabyle-nominal-state-specs.md` avant de s'appuyer dessus ici. Ne pas traiter `tadla` comme invariable en attendant.

`[disputed]` — l'alternance `urgaz`/`wergaz` est documentée comme libre (L01 de la spec d'état) : **ne jamais la corriger**. De même, le statut adjectival épithète (L02) et la classe des invariables `ta-` hors liste fermée (L05) restent partiels — ne pas généraliser au-delà des formes listées sans consultation du corpus de 700 000+ phrases de l'utilisateur.

### 2.2 Segmentation, agglutination et ordre des clitiques

| Sous-type | Exemple fautif → corrigé | Statut |
|---|---|---|
| Futur `ad` collé au verbe | `*adyekcem` → `ad yekcem` | `verified` (*Ilugan n tira n tmaziɣt*) |
| Clitique objet/directionnel non lié par trait d'union | `*Ččiɣ t` → `Ččiɣ-t` ; `*fell as` → `fell-as` | `verified` (`kabyle-pronouns-clitics-specs.md` §3.1) |
| **Ordre des clitiques inversé (datif/absolutif)** | Le clitique **datif** précède toujours l'**absolutif** dans la chaîne verbale, sauf à l'impératif où l'ordre s'inverse | `*as-t-fka` → à corriger selon le mode : `as-t-fka` est en fait l'ordre correct à l'indicatif (DAT avant ABS) ; à l'impératif, l'ordre correct est ABS avant DAT (`fket-t-asen`, pas `*fket-asen-t`) | `verified` à 99,9 % à l'indicatif (32 026 / 32 052 occurrences, Mettouchi 2018) ; inversion à l'impératif également `verified` (`kabyle-pronouns-clitics-specs.md` §8.1–§8.2) |
| Épenthèse glide `y-` omise ou ajoutée à tort sur `ak`/`am` après consonne | Après consonne, `ak`/`am` sont **obligatoires** (jamais `*yak`/`*yam`) ; après voyelle, les deux formes sont correctes (~66 % nu / ~34 % épenthétique) | `*budd-yak` (après consonne) → `budd-ak` | `verified` (`kabyle-pronouns-clitics-specs.md` §9.3, Bedar/Quellec/Voeltzel 2021) — **ne pas corriger** la variante après voyelle, les deux formes sont attestées |
| Particules directionnelles `-d`/`-n` mal placées (post- vs pré-verbal selon polarité) | Post-verbal en affirmatif, pré-verbal sous négation | `*Ur nniɣ-d ara` → `Ur d-nniɣ ara` | `verified` |
| `ara` mal classé (négation vs particule modale aoriste) | Homographe : renforçateur négatif postverbal *si* `ur` est présent dans la clause ; particule modale aoriste sinon | voir §2.3 | `verified` structure ; désambiguïsation `candidate` (code `E013` de `kabyle-negation-spec.md`) |

`[NEEDS REVIEW]` (affiné par expérience corpus 2026-09-12) — une opposition `ara`/`wara` (« je n'ai pas dit » vs « je n'ai rien dit ») figurait en v0.1 de ce document sur la base du skill `kabyle-language-expert`. `kabyle-negation-spec.md` v0.2.0, qui fait référence sur ce point, ne mentionne pas `wara`. Le corpus confirme que `wara` existe (194 occurrences), mais son emploi est fortement concentré après un petit nombre de verbes (`yuɣ` « atteindre/affecter », `iban` « apparaître ») dans le motif `ur X-yuɣ wara` — ce qui ressemble davantage à une **collocation lexicalisée** qu'à une opposition sémantique générale applicable à toute négation. Ne pas implémenter de règle de correction générale sur `wara` sans vérification auprès d'un locuteur natif ; si une règle est implémentée, la restreindre au motif collocationnel observé plutôt qu'à `wara` en général.

### 2.3 Négation

Reprise directe de `kabyle-negation-spec.md` v0.2.0 :

| Sous-type | Règle | Statut |
|---|---|---|
| `ara` postverbal absent alors qu'il est obligatoire | Obligatoire dans les conditionnelles négatives et les réponses négatives informatives ; ailleurs, renforçateur optionnel — Mettouchi 2021 rapporte ~52 % de présence sur son corpus oral ; **une mesure brute sur `tatoeba-kabyle-mono-cleaned` donne 63,2 %** (21 797/34 480 phrases avec `ur`), écart probablement dû à une mesure au niveau phrase plutôt que proposition (voir note ci-dessous) — à recalculer proprement avant de choisir quel chiffre citer en production | `verified` (structure) / `candidate` (chiffre exact) — **ne pas signaler comme erreur** l'absence de `ara` hors des contextes obligatoires, quel que soit le chiffre retenu |
| `ara` ajouté à tort | Bloqué avec un item à polarité négative, dans les serments/déclarations polémiques, la coordination négative, la subordination négative, les relatives restrictives | `verified` |
| Confusion des deux fonctions homographes de `ara` (négatif postverbal vs modal aoriste non négatif) | Désambiguïsation par présence/absence de `ur` dans la clause | `verified` (règle) / `candidate` (implémentation) — code `E013 HOMOGRAPHE_ARA_MISCLASSIFIED` |
| Verbe négatif fusionné à tort en lemme distinct | Le verbe en contexte négatif conserve son lemme positif, sauf cas lexicalisé sourcé (aucun recensé à ce jour) | `verified` |
| `ičča`/`yečča` (3sg masc. prétérit) traité comme faute | Les deux formes sont correctes (norme variée) | `verified` — **ne pas corriger** |

`candidate` — **Note méthodologique (expérience corpus 2026-09-12).** La mesure de 63,2 % ci-dessus compte toute phrase contenant à la fois `ur` et `ara`, sans vérifier qu'ils appartiennent à la même proposition. Une phrase à propositions multiples peut donc gonfler artificiellement ce taux. Un recalcul fiable nécessite de segmenter en propositions (via le tokenizer morphologique/un parseur de dépendances) avant de recompter — à faire avant de remplacer le chiffre de Mettouchi 2021 dans une version normative de cette spec.

### 2.4 Erreurs orthographiques

Reprise de la carte des caractères contaminants documentée dans `kabyle-orthography-specs.md` et dans le skill `kabyle-language-expert` :

| Sous-type | Exemple | Statut |
|---|---|---|
| Homoglyphes grecs/cyrilliques/turcs pour `ɛ`, `ɣ` | `ε` (U+03B5) → `ɛ` (U+025B) ; `γ` (U+03B3) → `ɣ` (U+0263) ; `ğ` → `ǧ`/`ɣ` ; `ı`/`İ` → `i`/`I` | `verified` (règle) — **chiffre de prévalence à reprendre** : 0,00 % mesuré sur la colonne `text` de `tatoeba-kabyle-mono-cleaned` (voir §4.1 point 0) ; les 3,17–3,66 % précédemment cités décrivaient vraisemblablement le corpus avant nettoyage (`raw_text`), à confirmer |
| Schwa final ajouté à tort à un mot natif | `*aɣrume` → `aɣrum` | `verified` — les emprunts intégrés bruts gardent légitimement leur `e` final : **ne pas corriger** ce cas |
| Gémination de `ḍ` non notée `ṭṭ` | `*beḍu` → `beṭṭu` | `verified` (*Ilugan*, note 21 à Alugen 2c) |
| Assimilation consonantique notée à l'écrit au lieu de la forme sous-jacente | `*temɣart` prononcé [t-temɣart] écrit avec l'assimilation | `verified` — écrire toujours la forme sous-jacente |

`[disputed]` — présence/absence de `ṛ` hors voisinage d'emphatiques, position alphabétique de `ɛ`, statut de `v` en emprunt : ne jamais traiter comme des fautes sans configuration explicite (option désactivable, §1.2).

### 2.5 Erreurs d'ordre des mots par calque du français/anglais

`[NEEDS REVIEW]` — catégorie à instruire à partir d'une analyse d'erreurs réelle sur corpus de traduction automatique EN/FR→kab (voir §4.1), et non à déduire par intuition contrastive :
- ordre VSO du kabyle non respecté par calque SVO ;
- pro-drop du sujet non appliqué (sujet explicite redondant calqué du français) ;
- ordre des clitiques/satellites calqué sur la position des pronoms français plutôt que sur la règle datif-avant-absolutif du §2.2.

### 2.6 Erreurs de choix lexical (homonymie, variantes dialectales)

`[NEEDS REVIEW]` — un correcteur GEC ne doit jamais remplacer un mot attesté par un néologisme non sourcé, même « plus pur » étymologiquement (schéma d'échec documenté de l'ère Amawal, ex. `anazbay` proposé pour l'attesté `amnafeq`).

---

## 3. Format de représentation des erreurs

### 3.1 Schéma d'édition

Le schéma reprend le style déjà en usage dans `kabyle-negation-spec.md` (objet token avec `lemma`/`pos`/`morph`/`status`/`confidence`/`source`/`error`) plutôt qu'un format inventé :

```json
{
  "span_start_token": 3,
  "span_end_token": 4,
  "original": "adyekcem",
  "corrected": "ad yekcem",
  "morph": { "type": "particle", "subtype": "aorist_future_marker" },
  "error_category": "clitic_segmentation.future_marker",
  "status": "verified",
  "confidence": 0.95,
  "source": "Ilugan n tira n tmaziɣt (Béjaïa 2005/2009)",
  "error": null
}
```

Champs :

- `span_start_token` / `span_end_token` : indices dans la tokenisation **morphologique** produite par `kabyle-morphological-tokenizer-spec.md` (pas la tokenisation par espaces).
- `morph` : objet `{"type", "subtype", ...}` repris tel quel du format de sortie du tokenizer (§7 de cette spec), pour rester interopérable sans traduction de schéma.
- `error_category` : identifiant hiérarchique reprenant la taxonomie §2 (`nominal_state.postverbal_subject`, `clitic_segmentation.future_marker`, `clitic_order.dat_before_abs`, `negation.ara_homograph`, `orthography.homoglyph`, `word_order.calque`, `lexical.choice`).
- `status` : marqueur épistémique de la règle appliquée (`verified`/`candidate`/`[NEEDS REVIEW]`/`[disputed]`), reporté depuis la présente spec ou la spec source référencée.
- `confidence` : score du modèle, **distinct** du statut épistémique de la règle — un modèle peut être confiant sur une règle marquée `[NEEDS REVIEW]`, ce qui doit rester visible.
- `source` : référence bibliographique précise, ou nom de la spec du dépôt qui fait foi.
- `error` : code d'erreur de la présente spec (§8) si l'édition correspond à un cas limite documenté (ex. sur-correction potentielle d'une variante de la norme variée).

### 3.2 Compatibilité avec le tokenizer morphologique et l'export UD/CoNLL-U

Le tokenizer morphologique kabyle segmente déjà les clitiques et exporte les traits `Definite=Ind|Red` (état nominal), `Polarity=Neg` (négation) suivant `kabyle-ud-specification.md`. Le schéma d'édition ci-dessus référence les mêmes identifiants de tokens que cet export, pour superposer directement les éditions GEC sur les colonnes `MISC`/`FEATS` d'un fichier CoNLL-U existant sans re-tokeniser, et pour réutiliser la nomenclature déjà en place (`Definite`, `Polarity`) plutôt que d'introduire un vocabulaire concurrent.

---

## 3.3 Politique anti-hallucination et abstention

### 3.3.1 Objectif et périmètre

Le module GEC ne doit pas seulement maximiser le nombre de corrections produites. Il doit surtout éviter :

- d'inventer des formes lexicales ou morphologiques ;
- de transformer une variante attestée en erreur ;
- d'appliquer au kabyle une règle provenant du français, de l'anglais ou d'une autre variété amazighe ;
- de présenter comme certaine une analyse seulement candidate ;
- de produire une correction lorsque le contexte ou les sources disponibles ne permettent pas de trancher.

Dans cette spécification, une **hallucination linguistique** désigne toute forme, règle, analyse ou correction produite par le système qui n'est pas soutenue par une source, un lexique attesté, une règle validée ou un contexte suffisamment contraignant.

Cette politique concerne principalement les hallucinations de **forme linguistique** : orthographe, morphologie, segmentation, clitiques, état nominal, négation et choix lexical. Elle ne garantit pas à elle seule la véracité factuelle, historique ou sémantique du contenu exprimé.

> **Principe directeur : en cas d'incertitude non résolue, l'abstention est préférable à une correction inventée.**

### 3.3.2 Hiérarchie des décisions

Toute proposition de correction doit être classée selon le statut épistémique de la règle utilisée et selon la confiance opérationnelle du système.

Le statut épistémique décrit la solidité de la règle ou de la source. Le score `confidence` décrit la confiance du système dans son application au contexte observé. Ces deux dimensions ne doivent jamais être fusionnées.

| Statut de la règle | Comportement par défaut |
|---|---|
| `verified` | Correction autorisée si le contexte satisfait les conditions de la règle et si le seuil de confiance est atteint. |
| `candidate` | Correction autorisée uniquement dans un mode explicitement permissif ; sinon signalement ou abstention. |
| `[NEEDS REVIEW]` | Aucune correction silencieuse. Le système doit signaler le cas et conserver la forme d'origine. |
| `[disputed]` | Aucune correction automatique par défaut. Le système doit préserver la forme, éventuellement présenter les analyses concurrentes et indiquer le point de désaccord. |

Le paramètre `confidence` doit être compris comme une valeur dans l'intervalle `[0,1]`. Il représente la confiance opérationnelle dans l'édition proposée, et non la validité scientifique de la règle. Les seuils doivent être calibrés sur un jeu de développement annoté.

Une implémentation peut définir des seuils par catégorie :

```json
{
  "thresholds": {
    "orthography.homoglyph": 0.80,
    "clitic_segmentation.future_marker": 0.90,
    "nominal_state": 0.95,
    "negation.ara_homograph": 0.98,
    "lexical.choice": 1.00
  }
}
```

Une catégorie dont le seuil est égal à `1.00` est, en pratique, désactivée tant qu'une validation externe n'est pas disponible.

### 3.3.3 Règles obligatoires de non-invention

Le système doit respecter les règles suivantes :

1. **Aucune forme lexicale ne doit être inventée par analogie.** Si un lemme, une conjugaison ou une dérivation n'est pas attesté, le système doit utiliser une paraphrase attestée, conserver le terme source ou s'abstenir. Il ne doit pas créer une forme simplement parce qu'elle paraît morphologiquement plausible.

2. **Aucune conjugaison ne doit être extrapolée lorsqu'une forme irrégulière ou suppletive est possible.** Les verbes et paradigmes doivent être vérifiés contre une ressource lexicale ou grammaticale appropriée.

3. **Une forme attestée ne doit pas être remplacée par une forme supposée plus pure ou plus standard.** La fréquence faible d'une forme ne suffit pas à la déclarer fautive.

4. **Les variantes documentées doivent être conservées.** Le système ne doit pas corriger automatiquement les oppositions ou alternances marquées comme relevant de la norme variée, notamment `ičča/yečča`, `urgaz/wergaz`, `kent/went`, certaines réalisations des clitiques et la présence ou l'absence de `ṛ`.

5. **Une règle ne doit pas être généralisée au-delà de son domaine attesté.** Une observation portant sur un verbe, un nom, une aire dialectale ou un contexte syntaxique donné ne doit pas être transformée en règle générale sans preuve indépendante.

6. **Les formes inconnues ne doivent pas être normalisées silencieusement.** Cela concerne notamment les noms propres, les emprunts, les toponymes, les termes techniques, les mots dialectaux et les formes absentes du lexique de référence.

7. **Le système ne doit pas produire de justification fictive.** Il ne doit pas attribuer une règle à une source qui ne la documente pas, ni déclarer une validation par locuteur natif qui n'a pas eu lieu.

### 3.3.4 Conditions d'abstention

Le système doit s'abstenir de corriger lorsque l'une des conditions suivantes est satisfaite :

- la règle applicable est marquée `[disputed]` ou `[NEEDS REVIEW]` ;
- plusieurs corrections concurrentes sont compatibles avec le contexte et aucune préférence n'est documentée ;
- la phrase ne contient pas suffisamment de contexte pour désambiguïser la forme ;
- la correction modifierait une variante dialectale, de registre ou d'auteur attestée ;
- la forme cible n'est pas attestée dans le lexique ou le corpus de référence ;
- la correction nécessite une décision sémantique non couverte par la présente spécification ;
- la segmentation morphologique source est incertaine ;
- l'opération produirait une nouvelle forme dont la validité ne peut pas être vérifiée ;
- la confiance opérationnelle est inférieure au seuil défini pour la catégorie ;
- l'entrée contient un script, une convention orthographique ou une variété dialectale non prise en charge par la configuration active.

L'abstention ne doit pas être assimilée à une absence de détection. Le système peut détecter un cas suspect sans appliquer de correction.

### 3.3.5 Modes d'exécution

L'interface doit permettre au client de choisir un niveau de conservatisme :

```json
{
  "mode": "conservative",
  "allow_candidate_rules": false,
  "allow_disputed_rules": false,
  "preserve_unknown_forms": true,
  "require_source_for_lexical_changes": true
}
```

Les modes recommandés sont les suivants :

| Mode | Règles `verified` | Règles `candidate` | Règles disputées | Usage recommandé |
|---|---|---|---|---|
| `conservative` | Oui, avec seuil élevé | Non | Non | Production, traduction publiée, évaluation |
| `standard` | Oui | Signalement ou correction avec option explicite | Non | Expérimentation contrôlée |
| `research` | Oui | Oui, marquées comme telles | Oui, jamais silencieuses | Recherche et annotation humaine |

Le mode `conservative` doit être le mode par défaut.

### 3.3.6 Contrat de sortie

Toute correction appliquée doit être accompagnée d'une trace structurée. Toute abstention sur un cas détecté doit également être représentée.

Exemple de correction appliquée :

```json
{
  "action": "correct",
  "original": "adyekcem",
  "corrected": "ad yekcem",
  "error_category": "clitic_segmentation.future_marker",
  "status": "verified",
  "confidence": 0.97,
  "source": "Ilugan n tira n tmaziɣt (2005/2009)",
  "error": null
}
```

Exemple d'abstention sur une variante ou un point disputé :

```json
{
  "action": "abstain",
  "original": "urgaz",
  "corrected": null,
  "error_category": "nominal_state.free_annexed_alternation",
  "status": "[disputed]",
  "confidence": 0.91,
  "source": "kabyle-nominal-state-specs.md",
  "reason": "documented_free_variation",
  "error": "E020"
}
```

Exemple de forme inconnue conservée :

```json
{
  "action": "preserve",
  "original": "⟨forme inconnue⟩",
  "corrected": null,
  "error_category": null,
  "status": "[NEEDS REVIEW]",
  "confidence": 0.00,
  "source": null,
  "reason": "unverified_lexical_form",
  "error": null
}
```

Les valeurs possibles de `action` sont :

- `correct` : une correction a été appliquée ;
- `preserve` : la forme a été conservée sans anomalie détectée ;
- `abstain` : un cas potentiellement problématique a été détecté, mais aucune correction fiable n'a été appliquée ;
- `flag` : le cas est transmis pour révision humaine.

### 3.3.7 Séparation entre erreur d'entrée et diagnostic du système

Les codes d'erreur linguistique et les codes de diagnostic du système doivent être distingués.

Un code linguistique décrit le phénomène détecté dans le texte source, par exemple :

```text
negation.ara_homograph
clitic_segmentation.future_marker
nominal_state.postverbal_subject
```

Un code de diagnostic décrit la décision ou la limite du système, par exemple :

```text
D001 LOW_CONFIDENCE
D002 DISPUTED_RULE
D003 UNKNOWN_LEXICAL_FORM
D004 MULTIPLE_VALID_ANALYSES
D005 DIALECTAL_VARIANT_PRESERVED
D006 INSUFFICIENT_CONTEXT
```

Les codes `E020`–`E029` existants doivent conserver leur rôle défini par la présente spécification. Si certains décrivent en réalité une sur-correction ou une décision du système plutôt qu'une erreur d'entrée, ils doivent être progressivement migrés vers une plage de diagnostics distincte.

### 3.3.8 Tests anti-hallucination

Le jeu de non-régression doit contenir, pour chaque règle, au moins quatre types de cas :

1. une erreur attestée qui doit être corrigée ;
2. une phrase correcte qui ne doit pas être modifiée ;
3. une variante légitime qui doit être conservée ;
4. un cas ambigu ou disputé qui doit déclencher une abstention.

Les critères minimaux d'acceptation sont :

- aucune correction automatique sur une règle `[disputed]` en mode `conservative` ;
- aucune invention de lemme absent des ressources autorisées ;
- conservation des formes inconnues et des noms propres ;
- conservation des variantes explicitement documentées ;
- justification et source présentes pour chaque correction ;
- abstention lorsque le contexte ne permet pas de trancher ;
- absence de régression sur les tests `TS-GEC-*` des catégories déjà validées.

Le système doit être évalué non seulement sur son taux de correction, mais aussi sur son :

- **taux de sur-correction** ;
- **taux de préservation des variantes légitimes** ;
- **taux d'abstention appropriée** ;
- **taux de corrections sans source** ;
- **taux d'invention lexicale**.

> Pour un correcteur kabyle, une correction non appliquée dans un cas incertain est généralement moins grave qu'une correction fausse présentée comme normative.

### 3.3.9 Limites de la politique

Cette politique réduit les hallucinations linguistiques, mais ne constitue pas un mécanisme général de vérification factuelle. Elle ne garantit pas qu'une phrase corrigée soit vraie, idiomatique dans toutes les aires dialectales ou adaptée à tous les registres.

Toute sortie générée librement par un modèle doit donc rester soumise au principe **AI-drafted, native-verified**. Une sortie non relue par un locuteur compétent ne doit pas être présentée comme définitivement validée lorsque son contenu comporte des formes `candidate`, `[NEEDS REVIEW]` ou `[disputed]`.

## 4. Données d'entraînement et d'évaluation

### 4.1 Constitution du corpus de paires (fautif, correct)

`[disputed]` — **Stratégie 0, réfutée par expérience (12 septembre 2026).** `boffire/tatoeba-kabyle-mono-cleaned` contient une colonne `raw_text` en plus de `text`, mais les deux sont identiques à 99,998 % (756 756/756 774 lignes) — les 18 différences restantes sont uniquement des normalisations d'espaces/tabulations, aucune correction grammaticale ou orthographique. **Cette source ne fournit pas de paires (fautif, correct) exploitables** : `raw_text` correspond au texte avant normalisation d'espacement, pas au texte avant nettoyage orthographique. À ne pas réessayer sur ce dataset précis ; si une source « brute » utile existe, ce serait plutôt un des autres datasets HuggingFace du projet (`boffire/kabyle-corpus`) plutôt que ce corpus déjà nettoyé.

Trois stratégies complémentaires, par ordre de fiabilité décroissante mais de coût croissant en volume — **stratégie 2 recommandée en priorité à court terme** vu l'échec de la stratégie 0 ci-dessus :

1. **Correction manuelle d'un sous-ensemble du corpus kabyle existant.** Échantillonner un sous-ensemble stratifié du corpus `boffire/tatoeba-kabyle-mono-cleaned` (756 774 phrases, contamination orthographique documentée à 3,66 % selon `kabyle-pronouns-clitics-specs.md`) et le faire corriger par des locuteurs natifs suivant la taxonomie §2, en associant chaque édition à sa catégorie.
2. **Génération synthétique par injection de bruit contrôlé.** Partir de phrases correctes et leur appliquer des règles de perturbation dérivées *une à une* de la taxonomie §2 (détacher un clitique normalement lié, inverser l'ordre datif/absolutif hors impératif, substituer un homoglyphe documenté, retirer l'annexion après `n`). Chaque règle de bruitage doit pointer vers la règle source correspondante, pour rester traçable.
3. **Rétro-traduction bruitée.** Traduire kabyle→langue pivot→kabyle avec un modèle de traduction imparfait pour produire des erreurs plus « naturelles » (calques §2.5) que le bruit synthétique ne couvre pas bien. Ce sous-ensemble doit être marqué séparément, sa distribution d'erreurs étant dépendante du modèle utilisé pour le générer.
4. **Extraction d'éditions Wikipédia.** `[NEEDS REVIEW]` Précédent documenté pour une langue low-resource morphologiquement complexe : Chunngai et al. ont constitué `HiWikiEdits` (8 137 paires) pour le hindi en extrayant des éditions humaines de Wikipédia, annotées avec ERRANT. Aucun équivalent kabyle n'est connu à ce jour (le kabyle n'a pas de Wikipédia de taille comparable) ; à évaluer néanmoins comme source d'appoint si un corpus éditorial kabyle suffisant existe ou émerge.
5. **Correction manuelle d'un corpus de référence existant, façon Öktem et al. 2025.** Plutôt que de générer des paires (fautif, correct) isolées, corriger manuellement, catégorie d'erreur par catégorie d'erreur, un corpus de référence déjà utilisé pour l'évaluation de traduction (ex. le sous-ensemble kabyle de FLORES+ ou d'un corpus équivalent), en s'appuyant sur des ressources faisant autorité (pour le tamazight : IRCAM ; pour le kabyle : les specs du présent dépôt). Cette approche a l'avantage de produire à la fois des paires d'entraînement GEC et un jeu d'évaluation de traduction amélioré, mesurable directement en gain de chrF/BLEU sur un modèle fine-tuné (cf. §1.1).

### 4.2 Validation humaine

`candidate` — Tout sous-ensemble synthétique (stratégies 2 et 3) doit passer par une relecture de locuteurs natifs avant usage en entraînement, suivant le principe *AI-drafted, native-verified* déjà appliqué dans l'écosystème : marquer chaque paire `verified` / `[NEEDS REVIEW]` / `[disputed]` avant intégration au jeu d'entraînement définitif.

### 4.3 Splits et anti-fuite

`candidate` — Reprendre le garde-fou anti-fuite déjà en place sur le pipeline de traduction de l'utilisateur (`boffire/kabyle-mt-clean`) : s'assurer qu'aucune phrase source utilisée pour générer des paires d'entraînement ne réapparaisse, même bruitée différemment, dans les splits dev/test, en réutilisant les mêmes splits Tatoeba/TranslateWiki déjà isolés dans ce pipeline.

---

## 5. Approches de modélisation

### 5.1 Approche seq2seq fine-tunée depuis un modèle multilingue

`candidate` — Fine-tuner un modèle seq2seq multilingue existant sur les paires du §4, en sortie texte-à-texte.

- **Avantages** : capture des erreurs contextuelles complexes (§2.5, §2.6) sans écrire de règles exhaustives.
- **Inconvénients en low-resource** : risque élevé d'hallucination sur les phénomènes rares (verbes suppletifs, formes de la norme variée) si le volume annoté est faible ; opacité sur *pourquoi* une correction est proposée, ce qui complique la traçabilité épistémique exigée en §3.1 — nécessite un post-hoc mapping vers une catégorie de la taxonomie §2.

### 5.2 Approche par règles/transducteurs combinée à un correcteur statistique

`candidate` — Module de règles déterministes pour les catégories **hautement attestées et mécaniques** (§2.1 état nominal après régisseurs fermés, §2.2 segmentation et ordre des clitiques, §2.4 orthographe), combiné à un modèle statistique/neuronal léger uniquement pour les catégories contextuelles (§2.3 désambiguïsation `ara`, §2.5, §2.6).

- **Avantages** : les règles déterministes héritent directement du statut `verified` de leur source, sont auditables, ne peuvent pas halluciner de correction non sourcée sur les catégories qu'elles couvrent, fonctionnent avec très peu de données.
- **Inconvénients** : ne couvre pas les erreurs contextuelles complexes ; travail d'ingénierie linguistique initial plus lourd.

**Précédents directement pertinents pour ce choix :**
- Un correcteur orthographique amazigh existe déjà (El Ouahabi et al., *ScienceDirect* 2021), combinant distance de Damerau-Levenshtein et modèle N-gramme, testé avec succès sur un corpus amazigh. C'est le précédent le plus proche à l'intérieur même de la famille berbère ; à évaluer comme brique de départ ou source d'inspiration pour §2.4, plutôt que de repartir de zéro.
- Pour les langues agglutinantes à morphologie riche comparable (turc, finnois), l'approche dominante en correction orthographique/morphologique combine un analyseur morphologique à états finis avec un modèle statistique de rang (Oflazer et al.) — cohérent avec la recommandation de coupler transducteurs déterministes (état nominal, clitiques) et modèle statistique léger (contexte) proposée ci-dessus.

`candidate` — Recommandation par défaut pour ce projet low-resource : démarrer par 5.2 pour les catégories mécaniques (rendement immédiat, zéro hallucination), n'introduire 5.1 que pour les catégories non couvertes, avec un seuil de confiance minimal sous lequel le modèle statistique s'abstient plutôt que de proposer une correction non vérifiable.

### 5.3 Interfaces d'entrée/sortie pour le branchement en pipeline

```
[texte source] → [traduction (n'importe quel moteur)] → [GEC post-traitement] → [texte final]
```

ou symétriquement en pré-traitement — voir contrat d'interface §1.2.

---

## 6. Évaluation

### 6.1 Métriques

`candidate` — Adapter une métrique de type ERRANT/GLEU (Bryant, Felice & Briscoe 2017 — méthodologie générale de GEC, non spécifique au kabyle) en tenant compte de deux contraintes propres au kabyle :
- tokenisation de référence = tokenisation **morphologique** (§3.2), pas par espaces, sous peine de sur/sous-pénaliser les erreurs de segmentation de clitiques (§2.2) ;
- la métrique doit **ignorer** les divergences qui relèvent de la norme variée (`ičča`/`yečča`, `urgaz`/`wergaz`, `kent`/`went`, présence/absence de `ṛ`) plutôt que de les compter comme faux positifs ou faux négatifs.

**Limite documentée du score M2, pertinente ici.** Sur l'arabe (langue morphologiquement riche comparable), il est établi que le score M2 standard sur-regroupe plusieurs éditions distinctes en une seule lors de l'alignement et ne compte pas les correspondances partielles, ce qui **sous-estime** la performance réelle des systèmes (Felice & Briscoe 2015, cité par les travaux d'évaluation GEC arabe récents). Pour le kabyle, où une seule édition de surface peut recouvrir plusieurs phénomènes simultanés (ex. correction d'état nominal + réinsertion d'un clitique dans la même forme), ce biais serait probablement encore plus marqué. `candidate` — préférer, en complément du score M2/ERRANT, un **annotateur automatique de types d'erreurs façon ARETA** (§2, note), jugé par la littérature « plus utile que les métriques M2 opaques » pour diagnostiquer les forces et faiblesses d'un système par catégorie plutôt que par un score global.

**Métrique alternative pour l'évaluation d'un corpus corrigé dans son ensemble** (plutôt que d'un système phrase par phrase) : Öktem et al. (2025) utilisent le taux d'erreur de caractères (CER) et la divergence de tokens (méthode Abdulmumin et al. 2024) pour quantifier l'ampleur des corrections apportées à un corpus de référence tamazight. Cette approche est directement transposable pour mesurer, en amont d'un entraînement, l'ampleur du bruit présent dans un sous-ensemble du corpus kabyle avant/après correction, indépendamment de toute métrique de traduction.

### 6.2 Jeu de test de non-régression

`candidate` — Réutiliser directement, comme socle initial, le jeu de paires minimales déjà publié dans `kabyle-nominal-state-specs.md` §7 (TS01–TS16) pour la catégorie §2.1, et construire des suites analogues (`TS-GEC-CLI-xx` pour §2.2, `TS-GEC-NEG-xx` pour §2.3, `TS-GEC-ORT-xx` pour §2.4) suivant le même format : phrase valide / forme erronée bloquée / phénomène testé / règle violée. Chaque catégorie doit inclure au moins un exemple négatif (variante dialectale légitime **non corrigée**) en plus des exemples positifs.

---

## 7. Limites connues et zones d'incertitude

1. **Volume de données annotées.** Aucune estimation chiffrée disponible pour le kabyle spécifiquement ; à déterminer empiriquement (courbes d'apprentissage).
2. **Risque de sur-correction de la norme variée.** Toute règle touchant un point `[disputed]` dans les specs existantes (`ṛ`, position de `ɛ`, statut de `v`, `kent`/`went`, `ad`/`a`, sous-cas d'ordre des clitiques du §8.3 de `kabyle-pronouns-clitics-specs.md`) doit rester désactivable via les `options` de l'interface (§1.2).
3. **Catégories §2.5 et §2.6 sous-documentées.** À peupler empiriquement à partir d'un corpus d'erreurs réelles avant formalisation en règles, sous peine d'halluciner des généralisations non attestées.
4. **Point non résolu signalé en v0.2 (`ara`/`wara`, §2.2).** À vérifier auprès d'un locuteur natif ou d'une source primaire avant toute implémentation.
5. **Attribution de `amyag.com`.** Question ouverte entre la spec de conjugaison (créditant Naït-Zerrad) et Bouamara 2024 pour toute donnée lexicale verbale utilisée en §2.3 — à vérifier avant usage en production.
6. **Absence de métriques d'évaluation robustes pour les langues agglutinantes/morphologiquement riches.** Une revue de littérature 2025 sur le GEC low-resource (*Grammatical error correction for low-resource languages: a review of challenges, strategies, computational and future directions*, PeerJ Computer Science) confirme explicitement que ce manque est un problème de recherche ouvert et général, non spécifique au kabyle — ce n'est donc pas une lacune de la présente spec à combler seule, mais un axe sur lequel suivre l'état de l'art plutôt que d'improviser une métrique isolée.
7. **Résolution des désaccords.** Pour tout point où les sources se contredisent sans que cette spec ne tranche : (a) consultation de locuteurs natifs couvrant si possible plusieurs des quatre aires dialectales (OC, EOC, OR, EOR — Naït-Zerrad 2004), (b) ouverture d'une issue sur `kabyle-specs.github.io` pour arbitrage communautaire.

---

## 8. Codes d'erreur

Plage réservée à ce document : `E020`–`E029` (les codes `E001`–`E013` sont déjà attribués par `kabyle-lemmatization-spec.md` et `kabyle-negation-spec.md` — voir note de compatibilité ci-dessous).

| Code | Nom | Déclencheur |
|---|---|---|
| E020 | `NOMINAL_STATE_MISCORRECTION` | Correction EL/EA appliquée sur un invariable ou une alternance libre documentée (`urgaz`/`wergaz`, classe `ta-` invariable) |
| E021 | `CLITIC_ORDER_UNRESOLVED_CASE` | Inversion datif/absolutif rencontrée dans un des cas `[disputed]` de `kabyle-pronouns-clitics-specs.md` §8.3 (`fukken`, `ttaran`, `Ɣlin`) — ne pas corriger automatiquement |
| E022 | `ARA_WARA_UNVERIFIED_RULE` | Tentative d'application d'une règle de distinction `ara`/`wara` non confirmée par `kabyle-negation-spec.md` (voir §2.2) |
| E023 | `DIALECTAL_VARIANT_FLAGGED_AS_ERROR` | Correction proposée sur un point relevant de la norme variée (§7.2) sans que l'option de désactivation correspondante n'ait été consultée |

> **Note de compatibilité** : héritée de `kabyle-lemmatization-spec.md` §7bis — les codes `E001`–`E013` sont stables depuis la v1.3.0 de cette spec et le v0.2.0 de `kabyle-negation-spec.md` ; `E012` est réservé à la négation, `E013` est utilisé par les deux documents pour des déclencheurs différents (conflit non résolu, hors périmètre du présent document). La présente spec n'utilise aucun code inférieur à `E020`.

---

## 9. Références

### 9.1 Kabyle et berbère

- Achab, Karim (2003, 2012, 2020). *Alternation of state in Berber* ; *La morphologie du nom en kabyle* ; *Anti-Agreement in Amazigh (Berber) as Genitive Constructions*.
- Bedar, Amazigh, Quellec, Lucie & Voeltzel, Laurence (2021). *Epenthetic glides in Taqbaylit*. Journal of African Languages and Literatures, 2/2021, pp. 1-29.
- Bryant, C., Felice, M., & Briscoe, T. (2017). *Automatic Annotation and Evaluation of Error Types for Grammatical Error Correction* (ERRANT) — méthodologie générale de GEC, non spécifique au kabyle.
- Chaker, Salem (1983, 1988, 1995). *Un parler berbère d'Algérie (Kabylie) : syntaxe* ; « Annexion (État d', linguistique) », *Encyclopédie berbère* V ; *Linguistique berbère : études de syntaxe et de diachronie*.
- Ilugan n tira n tmaziɣt/taqbaylit [Règles orthographiques] (Béjaïa, 2005, rééd. 2009), HAL hal-05530877.
- Mammeri, Mouloud (1976). *Tajeṛṛumt n tmaziɣt (tantala taqbaylit)*, Maspero.
- Mettouchi, Amina (2018). *Prosodic Segmentation and Grammatical Relations: The Direct Object in Kabyle (Berber)* ; *The Interaction of State, Prosody and Linear Order in Kabyle (Berber)*.
- Mettouchi, Amina (2001). « La grammaticalisation de *ara* en kabyle, négation et subordination relative », *Travaux du CerLiCO* n°14, pp. 215-235.
- Mettouchi, Amina (2021). *Negation in Kabyle (Berber)*, JaLaLit n°2, pp. 30-79. https://doi.org/10.6092/jalalit.v2i2.8059
- Mettouchi, Amina & Frajzyngier, Zygmunt (2013). *A previously unrecognized typological category: The state distinction in Kabyle (Berber)*, Linguistic Typology 17(1), pp. 1-30.
- Naït-Zerrad, Kamal (2001). *Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit*, Karthala.
- Naït-Zerrad, Kamal (2004). « Kabylie : dialectologie », Encyclopédie berbère 26, pp. 4067-4070.
- kabyle-specs.github.io — `kabyle-nominal-state-specs.md` v0.2-draft, `kabyle-negation-spec.md` v0.2.0, `kabyle-pronouns-clitics-specs.md` v0.3-draft, `kabyle-morphological-tokenizer-spec.md` v0.3-draft, `kabyle-lemmatization-spec.md` v1.3.2.

### 9.2 GEC et NLP pour langues morphologiquement riches / low-resource (ajoutées en v0.3)

- Belkebir, Riadh & Habash, Nizar (2021). *Automatic Error Type Annotation for Arabic* (ARETA), Proceedings of CoNLL 2021, pp. 596-606. https://aclanthology.org/2021.conll-1.47/
- Bryant, C., Felice, M., & Briscoe, T. (2017). *Automatic Annotation and Evaluation of Error Types for Grammatical Error Correction* (ERRANT).
- El Ouahabi, S. et al. (2021). *Amazigh spell checker using Damerau-Levenshtein algorithm and N-gram*, ScienceDirect / Journal of King Saud University.
- Felice, M. & Briscoe, T. (2015). Cité dans les travaux d'évaluation GEC arabe récents pour la limite du score M2 (sur-regroupement des éditions, sous-comptage des correspondances partielles).
- Oktem, Alp, Farhi, Mohamed Aymane, Essaidi, Brahim, Jabouja, Naceur & Boudichat, Farida (2025). *Correcting the Tamazight Portions of FLORES+ and OLDI Seed Datasets*, Proceedings of the Tenth Conference on Machine Translation (WMT 2025), pp. 1072-1080. https://aclanthology.org/2025.wmt-1.82/
- Revue non attribuée à un auteur unique identifié (2025). *Grammatical error correction for low-resource languages: a review of challenges, strategies, computational and future directions*, PeerJ Computer Science. https://peerj.com/articles/cs-3044/
- Chunngai et al. — `HiWikiEdits`, jeu de données GEC hindi extrait d'éditions Wikipédia, annoté avec ERRANT (dépôt GitHub `gec-papers`).

*Vérifier les détails bibliographiques et versions actuelles avant citation dans une publication formelle.*

---

## 10. Historique des versions

| Version | Date | Modifications |
|---|---|---|
| 0.1-draft | 2026-09-11 | Version initiale, rédigée à partir du skill `kabyle-language-expert` sans consultation directe des specs sources. |
| 0.2-draft | 2026-09-12 | Révision complète contre les fichiers réels de `kabyle-specs.github.io/specs/` : terminologie EL/EA et `Definite=Ind|Red` corrigée, chiffres `ara` sourcés précisément, point `ara`/`wara` rétrogradé en `[NEEDS REVIEW]`, ajout de la règle d'ordre datif/absolutif, schéma JSON aligné sur le style du dépôt, registre de codes d'erreur `E020`–`E029` ajouté. |
| 0.3-draft | 2026-09-12 | Révision contre la littérature GEC/NLP publiée (recherche web) : preuve empirique externe de l'impact traduction (Öktem et al. 2025, +6,05/+2,32 chrF), taxonomie ARETA citée comme grille de haut niveau transférable, deux stratégies de données supplémentaires (§4.1), précédents concrets de modélisation pour langues agglutinantes et pour l'amazigh spécifiquement (§5.2), critique sourcée du score M2 et métriques alternatives CER/divergence de tokens (§6.1), confirmation par une revue 2025 que l'absence de métriques pour langues agglutinantes est un problème de recherche ouvert et non une lacune propre à cette spec (§7). |
| 0.4-draft | 2026-09-12 | Premiers résultats expérimentaux réels sur `boffire/tatoeba-kabyle-mono-cleaned` (756 774 phrases) : découverte de la paire `raw_text`/`text` comme source de données GEC gratuite (§4.1) ; contamination orthographique mesurée à 0,00 % sur `text` (§2.4, à recouper avec `raw_text`) ; extension de la classe invariable `ta-` (`tudert`, `tikkelt`, `tegnit` ajoutés) et correction candidate sur `tadla` (79 % de mutation observée, contredit `kabyle-nominal-state-specs.md` L05) ; réplication indépendante de la règle d'ordre datif/absolutif (32 026 vs 28, aucun nouveau verbe) ; écart 63,2 % vs 52 % sur le taux de `ara` signalé comme probablement biaisé par une mesure au niveau phrase plutôt que proposition ; `wara` confirmé existant (194 occ.) et rattaché à une collocation avec `yuɣ`/`iban` plutôt qu'à une opposition sémantique générale ; limite méthodologique documentée sur les faux positifs de la comparaison `ṛ`/`r` par simple substitution de caractères. |

---

*Cette spécification est conçue pour être améliorée par la communauté, en cohérence avec les autres documents de `kabyle-specs.github.io`. Toute contribution validée par corpus ou par source académique peut être intégrée dans une version ultérieure.*
