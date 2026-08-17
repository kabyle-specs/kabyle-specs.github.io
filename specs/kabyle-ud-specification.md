# Spécification Kabyle Universal Dependencies (UD) — v0.7

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; structuration algorithmique et synthèse bibliographique. Révision et vérification des sources : assistée par Claude (Anthropic)/Qwen 3.8Max. Validation empirique : corpus Tatoeba kabyle (756 774 phrases / 4 881 741 tokens) ; branche `dev` du dépôt `UD_Kabyle-ADPT`, vérifiée directement via l'API GitHub (voir §3.1bis).

**Date** : 17 août 2026

**Version** : 0.7

**Statut** : Révision de correction et de consolidation. Deux apports majeurs par rapport à v0.6 :

1. **Vérification indépendante, confirmée exacte**, de l'ensemble des statistiques empiriques attribuées à la branche `dev` du dépôt `UD_Kabyle-ADPT` (accès direct via l'API GitHub, chiffre pour chiffre). Ce qui était présenté comme une analyse empirique dans les versions précédentes est désormais un fait vérifié de façon reproductible, avec la méthode documentée en §3.1bis.
2. **Correction d'une lacune identifiée par validation native** (Mokraoui, août 2026) : le morphème `ara` est un **homographe**, et non un marqueur unique. La spec jusqu'en v0.6 ne traitait que sa fonction de négation (`ur ... ara`), omettant sa fonction distincte de marqueur d'irréalis/participe positif de l'aoriste (ex. `mi ara`, `asmi ara` = « quand [futur] »). Cette lacune est corrigée en §5.2.2bis, avec mise à jour en cascade des sections dépendantes (§6, §7.2, §10, §11, §12).

Par ailleurs, un point empirique nouveau, non exploité dans les versions précédentes, est signalé : dans les données réelles ADPT, `mačči` est tagué `CCONJ` (et non `PART`/`ADV`), et l'historique des commits de la branche `dev` contient des traces (« Spurious copula », « Spurious auxiliaries », oct. 2022) suggérant que le statut de la copule `d` était déjà une source de friction dans le pipeline d'origine — un indice indirect corroborant l'analyse de Mettouchi (2017) retenue en §5.2.1.

**Cible** : Annotateurs de treebanks, développeurs de parseurs de dépendances, chercheurs en linguistique berbère et typologie syntaxique, comité Universal Dependencies.

---

## Résumé

Cette spécification propose une adaptation du framework **Universal Dependencies (UD)** à la langue kabyle (Taqbaylit, ISO 639-3 `kab`). Elle s'inscrit dans la continuité du treebank **UD_Kabyle-ADPT** (Aliane, v2.8/v2.9, 2021) et des spécifications v0.3 à v0.6, tout en intégrant :
- des conventions d'annotation pragmatiques validées empiriquement sur un corpus de grande taille (Tatoeba kabyle, 756 774 phrases) ;
- une vérification indépendante et reproductible des données ADPT (branche `dev`, confirmée via l'API GitHub) ;
- la correction d'une confusion homographique non détectée par les six révisions précédentes (`ara` négatif vs. `ara` irréalis).

Le kabyle est une langue afro-asiatique (berbère) à ordre de base **VSO**, fortement pro-drop pour le sujet, avec des clitiques pronominaux objets qui doublent ou remplacent les arguments lexicaux — un phénomène de *clitic doubling* démontré empiriquement par Fahloune (2020).

**Nouveautés de la v0.7** :
- Ajout de **§5.2.2bis** : le second `ara` (irréalis/participe aoriste), distinct du `ara` négatif — correction d'une lacune de fond, signalée par validation native, confirmée par la grammaire de référence.
- Ajout de **§3.1bis** : méthode de vérification reproductible des statistiques ADPT via l'API GitHub (contournant le blocage des robots sur l'interface web classique).
- Mise à jour de **§5.2.7** : donnée empirique nouvelle — `mačči` est tagué `CCONJ` dans les données réelles ADPT.
- Mise à jour de **§5.2.1** : indice corroborant tiré de l'historique des commits (« Spurious copula »).
- Mise à jour de **§7.2** : ajout de `Mood=Irr` comme piste de feature pour le `ara` irréalis.
- Mise à jour de **§10, §11, §12** : nouvel item de test T22, nouvelle ligne de différence corrigée, révision de la limite L2.

**Mots-clés** : kabyle, taqbaylit, Universal Dependencies, treebank, syntaxe, VSO, clitiques, copule, état d'annexion, clitic doubling, complémenteur, irréalis, homographie, traits morphologiques, emprunts.

---

## 1. Introduction

### 1.1 Contexte : le kabyle dans l'écosystème UD

À ce jour, aucune langue berbère n'est représentée dans le catalogue Universal Dependencies de manière active et maintenue. Une première tentative — **UD_Kabyle-ADPT** — a été déposée par Lakhdar Aliane dans le cadre de la release UD v2.8/v2.9 (2021). Le dépôt officiel contient sur sa branche `master` un squelette de documentation non rempli (README/LICENSE/CONTRIBUTING sans contenu), mais les fichiers de données réels se trouvent sur la branche `dev`, activement maintenue par Dan Zeman (UD core team) de mai 2021 au **9 septembre 2025** — date confirmée par l'historique des commits (voir §3.1bis). Ce treebank n'a cependant pas été inclus dans les releases publiques principales depuis son intégration initiale, probablement en raison des incohérences documentées ci-dessous (§3).

### 1.2 Pourquoi une spec formelle ?

Les langues à morphologie riche et à clitiques pronominaux (turc, arabe, grec, bulgare) ont montré dans l'écosystème UD que la qualité d'un treebank dépend crucialement de la documentation préalable des choix d'annotation (Çöltekin et al. 2021 pour le turc ; Taguchi et al. 2022 pour le tatar). Sans guidelines spécifiques, les annotateurs divergent sur des points fondamentaux : statut de la copule, traitement du clitic doubling, segmentation des morphèmes, extraction des traits morphologiques, et — comme le montre la présente révision — désambiguïsation des morphèmes homographes tels que `ara`. Cette spec se propose de combler ce vide pour le kabyle, en s'appuyant systématiquement sur la littérature linguistique disponible plutôt que sur l'intuition seule, et en documentant explicitement les conventions d'annotation pragmatiques là où la littérature fait défaut.

### 1.3 Typologie syntaxique du kabyle (résumé)

| Trait | Description | Référence |
|-------|-------------|-----------|
| Ordre de base | VSO (Verbe-Sujet-Objet) | Felice (2020) ; Achab (2020) |
| Ordres marqués | SVO, VOS, OVS attestés | Achab (2020) |
| Pro-drop sujet | Fort (le sujet lexical est optionnel) | Fahloune (2020) |
| Marqueurs sujet | Véritable accord verbal (obligatoire, variable selon l'aspect pour les verbes de qualité) | Fahloune (2020), diagnostics multiples |
| Clitiques objets | Instances de *clitic doubling* (optionnels, invariants, empilables DAT-ACC-DIR) | Fahloune (2020) |
| Cas | Système à état libre (FS) / état d'annexion (CS) | Felice (2020) ; Achab (2003, 2020) |
| Négation verbale | Discontiguë : `ur` ... `ara` | Mettouchi (2001, 2004) — sources partielles, cf. §12 L2 |
| Irréalis/participe aoriste | Marqueur `ara` **homographe** du `ara` négatif, sans négation | Grammaire de référence (Naït-Zerrad et al.), cf. §5.2.2bis — **NOUVEAU v0.7** |
| Négation ascriptive | Marqueur dédié `mačči` (distinct de `ur`/`ara`) | Mettouchi (2017), CorTypo |
| Copule ascriptive | Particule invariable `d`, copule non-verbale | Mettouchi (2017), CorTypo |
| Présentatifs | Constructions distinctes de la copule : `ha`, `aql`, `a`+suffixe (jamais `d`) | Mettouchi (2017), CorTypo |
| Complémenteur relatif/clivée | `i` (perfectif) / `a` (imperfectif, aoriste) — alternance conditionnée par l'aspect | Achab (2020) |

---

## 2. Sources primaires

### 2.1 Documents internes (Mokraoui 2026)
- **Tokenizer morphologique** (spec v0.3) : segmentation des clitiques, affixes, préfixes dérivationnels.
- **Conjugueur algorithmique** : 64 types morphologiques, 344K formes conjuguées (Bouamara 2026).
- **G2P** : 34 graphèmes, règles de spirantisation/blocage.
- **Corpus Tatoeba kabyle nettoyé** : 756 774 phrases, 4 881 741 tokens, validation linguistique GlotLID v3 + DistilBERT + MaskLID (Mokraoui 2026).

### 2.2 Treebank antérieur

**Aliane, Lakhdar** (2021). *UD_Kabyle-ADPT*. Universal Dependencies. Dépôt : https://github.com/UniversalDependencies/UD_Kabyle-ADPT (données sur la branche `dev`).

**Note d'analyse empirique — vérifiée et reproductible (v0.7)** : voir méthode complète en §3.1bis. Chiffres confirmés directement depuis `stats.xml` sur la branche `dev` (1930 phrases / 19965 tokens / 23761 mots / 3241 fusions) :
- Absence totale de `AUX`, `cop`, `expl` dans les données finales (0 occurrence).
- Copule `d` : annotée `PART advmod` (440 occ.) ou `ADV advmod` (224 occ.), jamais `cop`.
- Préposition `deg` : segmentée systématiquement (100 % des 171 occurrences) en MWT `ad` (PART) + `ag` (ADP) — sur-segmentation à corriger.
- Clitique possessif `ines` : segmenté systématiquement (100 % des occurrences vérifiées) en `in` (ADP, case) + `as` (PRON, nmod).
- Marqueur relatif `i` : majoritairement `PRON` dans diverses relations, avec une seule occurrence `SCONJ mark` sur l'ensemble du corpus.
- **[NOUVEAU v0.7]** `mačči` : tagué `CCONJ` dans les données réelles (liste des lemmes les plus fréquents de la catégorie CCONJ : `neɣ, maca, acku, wala, mačči, mi`). Donnée à intégrer dans la réflexion sur son UPOS définitif, cf. §5.2.7.
- **[NOUVEAU v0.7]** `ara` : figure parmi les lemmes les plus fréquents de la catégorie `ADV` et parmi les exemples de la feature `Polarity=Neg` (« ur, ara, war, awer, wara, xaṭi... »). Cela documente sa fonction négative dans le corpus ADPT, mais ne renseigne pas — le corpus ADPT n'ayant pas été annoté avec cette distinction à l'esprit — sur les occurrences potentiellement mal étiquetées de son homographe irréalis. Cf. §5.2.2bis.

### 2.3 Syntaxe et morphosyntaxe berbère (vérifiées, texte intégral consulté)

**Fahloune, Khokha** (2020). *On the status of subject and object markers in Kabyle: New evidence*. McGill Working Papers in Linguistics, 26.1, pp. 1–17. Accès libre : http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Fahloune.pdf
> Démontre par plusieurs diagnostics indépendants (invariance aspectuelle, granularité des traits phi, Person-Case-Constraint, empilement de clitiques) que les marqueurs sujet en kabyle sont un véritable accord verbal, tandis que les marqueurs objet sont des instances de *clitic doubling*. L'exemple d'empilement de clitiques donné (« y-fka-as-tt wqcic tktuvt-nni i Ales », §4.2.1, ex. 19) est structurellement identique aux exemples de la présente spec (§6.3.2).

**Achab, Karim** (2020). *Anti-Agreement in Amazigh (Berber) as Genitive Constructions*. McGill Working Papers in Linguistics, 26.1. Accès libre : http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Achab.pdf
> Analyse le complémenteur `i` (glosé COMP) comme l'élément commun aux constructions relatives, clivées et génitives-possessives en kabyle. Établit l'alternance `i` (perfectif) / `a` (imperfectif, aoriste) comme complémenteurs conditionnés par l'aspect. Analyse les formes possessives longues (ex. `axxam-i-n-u`, « ma maison ») comme composées du complémenteur `i` + particule génitive `n` + suffixe personnel.

**Baier, Nico** (2020). *The Person Case Constraint in Kabyle*. McGill Working Papers in Linguistics, 26.1. Accès libre : http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Baier.pdf
> Confirme l'analyse de `ad` (futur), `ur` (négation) et `i` (complémenteur A-bar) comme des « preverbal particles » du kabyle. Analyse le clitic doubling en détail pour les constructions ditransitives.

**Felice, Lydia** (2020). *On the Case System of Kabyle*, McGill Working Papers in Linguistics 26.1. Analyse le kabyle comme langue à nominatif marqué : état d'annexion (CS) = nominatif (sujet), état libre (FS) = accusatif (objet).

**Ouhalla, Jamal** (2005). *Clitic placement in Berber*. Les clitiques obéissent à la loi de la seconde position et peuvent s'attacher au verbe (V-CL) ou à une catégorie fonctionnelle (F-CL V), notamment `ad`, `ur`.

### 2.4 Prédication et copule (vérifiée, texte intégral consulté)

**Mettouchi, Amina** (2017). *Predication in Kabyle (Berber), KAB*. In Mettouchi, Frajzyngier & Chanard (eds), *Corpus-based cross-linguistic studies on Predication* (CorTypo). http://cortypo.huma-num.fr/Publication
> Source déterminante pour le statut de `d`. Section 1.3 : « Kabyle has a number of non-verbal predicates: **a non-verbal copula ('d')** followed by a nominal, a negative existential predicate... ». Citation vérifiée mot pour mot dans le PDF source (v0.6/v0.7). La copule `d` est glosée systématiquement **COP** / **PRED** dans toutes les prédications ascriptives (affirmatives et négatives), et est catégoriquement **distincte** des constructions présentatives (`ha`, `aql`, `a`+suffixe distal/proximal), qui ne font jamais intervenir `d`. La négation ascriptive utilise un marqueur dédié, `mačči` (glosé NEG.ATTR), distinct de la négation verbale `ur...ara`.

**Mettouchi, Amina & Frajzyngier, Zygmunt** (2013). *A previously unrecognized typological category: The state distinction in Kabyle (Berber)*. Linguistic Typology 17(1), pp. 1–30.

### 2.5 Négation verbale (référence trouvée, texte non encore consulté en intégralité — cf. §12 L2)

**Mettouchi, Amina** (2001). *La grammaticalisation de ara en kabyle, négation et subordination relative*. Travaux du CerLiCO n°14, pp. 215–235.
> Titre directement pertinent pour la question du homographe `ara` traitée en §5.2.2bis : la « grammaticalisation de ara » et son lien avec la « subordination relative » suggèrent que Mettouchi traite précisément de la double fonction (négation / marqueur subordonné-irréalis) identifiée dans cette révision. **Priorité de lecture pour la v0.8.**

**Mettouchi, Amina** (2004). *Les négations non-verbales en kabyle (berbère)*. Verbum XXVI(3), pp. 269–280.
**Mettouchi, Amina** (2017). *Relative (Proposition - Syntaxe)*. Encyclopédie berbère vol. XL, pp. 6815–6825.

### 2.6 Phonologie et morphologie des frontières

**Bedar, Amazigh ; Quellec, Lucie ; Voeltzel, Laurence** (2021). *Epenthetic glides in Taqbaylit*, Journal of African Languages and Literatures 2/2021, pp. 1-29. **Note de vérification** : cet article traite exclusivement de l'épenthèse phonologique de glides aux jonctions nom/verbe-clitique ; il ne discute pas du statut syntaxique de la copule ou d'autres particules fonctionnelles. Sa pertinence pour cette spec se limite aux règles de segmentation phonologique des frontières morphémiques, pas aux choix d'annotation syntaxique.

### 2.7 Guidelines UD

**Nivre, Joakim ; de Marneffe, Marie-Catherine ; et al.** (2020). *Universal Dependencies v2: An annotation scheme for multilingual dependency parsing*. LREC.
**de Marneffe, Marie-Catherine ; Manning, Christopher D. ; et al.** (2021). *Universal Dependencies*. Computational Linguistics 47(2), pp. 255-308.

### 2.8 Treebanks de référence typologiquement proches

**Çöltekin, Çağrı ; et al.** (2021). *Improving the Annotations in the Turkish Universal Dependency Treebank*. RANLP 2021.
**Taguchi, Chihiro ; et al.** (2022). *UD-Tatar NMCTT Treebank*. UD v2.11.

### 2.9 Ressources morphologiques kabyles

**Bouamara, K.** (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle*. HAL.
> Documente 64 types morphologiques verbaux et 344 000 formes conjuguées, avec paradigmes complets incluant les marqueurs aspectuels (tt-/tte- pour l'imperfectif) et dérivationnels (ttwa-/ttu- pour le passif, ss-/sse- pour le causatif, mye-/mya- pour le réciproque). Utilisé comme référence pour §7.2.1.

### 2.10 Grammaire de référence générale — **[NOUVEAU v0.7]**

**Naït-Zerrad, Kamal** (2001). *Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit*. Éditions Karthala.
**Mammeri, Mouloud** (1976). *Tajerrumt n tmaziɣt (tantala taqbaylit)*. Maspero.
**Chaker, Salem** (1983). *Un parler berbère d'Algérie (Kabyle) : syntaxe*. Université de Provence.
> Ces trois références de grammaire descriptive générale documentent le paradigme du participe de l'aoriste kabyle (formes positive et négative), qui est la source de la correction apportée en §5.2.2bis : la forme positive du participe aoriste est marquée par `ara` (ex. *ara yafgen*, « qui volera »), tandis que la forme négative correspondante est marquée par `ur n(e)-` (ex. *ur nufig*). Ce paradigme, distinct de la négation discontinue `ur ... ara` de la proposition principale, est la preuve grammaticale que `ara` est un homographe et non un marqueur unique.

---

## 3. État de l'art et positionnement

### 3.1 UD_Kabyle-ADPT : données empiriques vérifiées

D'après l'analyse directe des fichiers ADPT (branche `dev`, `stats.xml` et fichiers `.conllu`) :
- **Taille** : 1930 phrases, 19965 tokens, 23761 mots syntaxiques, 3241 fusions.
- **Tags UPOS utilisés** (15) : ADJ (162), ADP (3148), ADV (1829), CCONJ (131), DET (67), INTJ (81), NOUN (4428), NUM (111), PART (2104), PRON (3394), PROPN (192), PUNCT (3949), SCONJ (142), VERB (3937), X (86).
- **Aucun tag `AUX`** n'est présent dans les données.
- **Relations utilisées** (28) : acl, acl:relcl, advcl, advmod, amod, appos, case, cc, ccomp, compound, conj, dep, det, discourse, dislocated, iobj, mark, nmod, nsubj, nummod, obj, obl, obl:arg, parataxis, punct, root, vocative, xcomp.
- **Aucune relation `cop` ni `expl`** n'est présente.

### 3.1bis Méthode de vérification reproductible — **[NOUVEAU v0.7]**

Les chiffres ci-dessus, présentés dans les versions v0.3 à v0.6 comme le résultat d'une « analyse empirique directe », ont fait l'objet d'une **vérification indépendante** lors de la préparation de cette révision. L'interface web standard de GitHub bloque l'accès automatisé aux pages de type `/tree/<branche>` (règles robots), ce qui rend une vérification superficielle difficile. La méthode suivante, reproductible par quiconque, contourne cette limite :

```bash
# 1. Lister les branches du dépôt via l'API GitHub (pas l'interface web)
curl -s "https://api.github.com/repos/UniversalDependencies/UD_Kabyle-ADPT/branches"
# → confirme l'existence de "dev" (protégée) et "master"

# 2. Lister le contenu de la branche dev
curl -s "https://api.github.com/repos/UniversalDependencies/UD_Kabyle-ADPT/contents/?ref=dev"
# → confirme la présence de kab_adpt-ud-train.conllu, kab_adpt-ud-test.conllu, stats.xml

# 3. Récupérer stats.xml directement
curl -s "https://raw.githubusercontent.com/UniversalDependencies/UD_Kabyle-ADPT/dev/stats.xml"
# → chiffres vérifiés exacts au caractère près (voir ci-dessous)

# 4. Récupérer l'historique des commits pour la datation de la maintenance
curl -s "https://api.github.com/repos/UniversalDependencies/UD_Kabyle-ADPT/commits?sha=dev&per_page=100"
# → confirme Dan Zeman comme auteur de la majorité des commits,
#   du 2021-05-26 au 2025-09-09
```

**Résultat de la vérification** : chaque chiffre annoncé dans les tableaux de §3.1 — total de phrases/tokens/mots/fusions, décompte par UPOS, décompte par relation — correspond exactement au contenu de `stats.xml` sur la branche `dev`. La date de fin de maintenance par Dan Zeman (septembre 2025) est également confirmée par le dernier commit de son historique (9 septembre 2025, message : « Added Parallel to README »).

**Donnée empirique supplémentaire, tirée de cette vérification et absente des versions précédentes** : l'historique des commits contient plusieurs messages de correction directement pertinents pour cette spec, notamment (dates originales, octobre 2022) :
- *« Spurious copula »*
- *« Spurious auxiliaries »*
- *« SCONJ vs. ADV »*
- *« PronType=Det --> PronType=Dem »*

Ces messages suggèrent que Dan Zeman a lui-même identifié et retiré, à un stade antérieur du pipeline de conversion, des candidats `cop`/`AUX` jugés erronés selon les critères de validation UD standard (probablement des heuristiques de conversion automatique trop agressives, non fondées sur l'analyse linguistique de Mettouchi 2017). Cela ne contredit pas la décision de cette spec de réintroduire `AUX`/`cop` pour `d` — cette décision repose sur une source linguistique dédiée (Mettouchi 2017), alors que les candidats retirés par Zeman relevaient probablement d'une conversion automatique non sourcée. Mais l'épisode confirme, de façon indépendante, que le statut de `d` était déjà identifié comme problématique dans le pipeline ADPT d'origine.

### 3.2 Divergences identifiées, sourcées et corrigées

| Divergence | Données ADPT | Correction v0.5–v0.7 | Source de la correction |
|-----------|-------------|-----------------|--------------------------|
| Copule `d` | PART/ADV + advmod | **AUX + cop** | Mettouchi (2017) : « non-verbal copula » distincte du présentatif |
| Clitic doubling | obj/iobj/obl pour les clitiques | **expl** pour les clitiques redondants | Fahloune (2020) : démonstration du clitic doubling |
| Marqueur relatif `i` | PRON | **SCONJ + mark** (avec alternance `i`/`a` selon l'aspect) | Achab (2020) : `i`/`a` glosés COMP |
| Prépositions composées | Segmentées (ex : `deg` → `ad`+`ag`) | **Token unique ADP** | Recommandation communautaire UD (pas de source Kabyle spécifique) |
| Clitique possessif `ines` | Segmenté `in`+`as` | **Confirmé** : segmentation `in` (ADP) + `as` (PRON) | Convergence Achab (2020, analyse théorique) + pratique ADPT |
| Tag `AUX` | Absent | Introduit, réservé à la copule `d` | Mettouchi (2017) |
| Particules `ad`/`ur` | — | **PART** (`ad`) / **ADV** (`ur`) | Convergence Fahloune, Baier, Achab (2020) |
| Négation ascriptive `mačči` | Taguée CCONJ dans ADPT (donnée empirique, cf. §2.2) | **[À VALIDER]** Marqueur distinct, UPOS à trancher (PART/ADV/CCONJ) | Mettouchi (2017) : NEG.ATTR, construction propre |
| Présentatifs (`ha`, `aql`, `a`+suffixe) | Non traités | **[NOUVEAU]** Construction non couverte, à spécifier | Mettouchi (2017) : constructions formellement distinctes de la copule |
| **`ara` négatif vs. `ara` irréalis** | Confondus (ADV Polarity=Neg systématique) | **[NOUVEAU v0.7]** Homographe distingué, second `ara` en PART provisoire | Grammaire de référence (Naït-Zerrad, Mammeri, Chaker) ; validation native (Mokraoui 2026) |

### 3.3 Validation empirique sur corpus Tatoeba

Pour valider les conventions d'annotation pragmatiques (§7.2.1, §5.2.5, §8.3.1), un corpus de grande taille a été utilisé :

**Corpus** : Tatoeba kabyle nettoyé (Mokraoui 2026)
- **Taille** : 756 774 phrases, 4 881 741 tokens
- **Validation linguistique** : GlotLID v3 (détection de langue), DistilBERT (classification kab/tach), MaskLID (distribution token-level)
- **Couverture lexicale** : 98.14% (seulement 90 683 tokens X sur 4 881 741)
- **Distribution UPOS observée** :
  - VERB : 23.7% (1 156 225 tokens)
  - NOUN : 13.8% (675 744 tokens)
  - PRON : 10.2% (495 951 tokens)
  - ADP : 10.1% (494 428 tokens)
  - PROPN : 3.1% (157 424 tokens)
  - ADV : 1.9% (93 200 tokens)
  - DET : 1.6% (80 334 tokens)
  - ADJ : 0.1% (2 840 tokens)

**Note méthodologique** : Ce corpus est utilisé uniquement pour **valider la fréquence et la distribution** des phénomènes linguistiques, pas pour définir les règles grammaticales elles-mêmes, qui restent ancrées dans la littérature linguistique (§2). Contrairement aux statistiques ADPT (§3.1bis), les chiffres de ce corpus n'ont pas fait l'objet d'une vérification indépendante externe — ils proviennent du pipeline interne de l'auteur et sont présentés comme tels, sans même le niveau de confirmation dont bénéficient désormais les statistiques ADPT.

### 3.4 Positionnement de cette spec

Cette spécification (v0.7) se distingue par :
1. **Une vérification empirique directe et désormais reproductible** des données ADPT (méthode documentée en §3.1bis, chiffres confirmés exacts par accès API indépendant).
2. **Un sourçage systématique** de chaque décision d'annotation contestée, avec citation du texte exact et évaluation de la pertinence de la source.
3. **Une identification de lacunes nouvelles** — non seulement de constructions non couvertes (`mačči`, présentatifs, déjà signalées en v0.5), mais aussi, pour la première fois en v0.7, **d'une confusion homographique non détectée pendant six révisions successives** (`ara` négatif vs. irréalis), corrigée grâce à une relecture par un locuteur natif.
4. **Une validation empirique sur corpus de grande taille** (756k phrases) pour les conventions pragmatiques, avec transparence sur le statut de chaque décision (résolu / partiellement sourcé / convention pragmatique / vérifié indépendamment).
5. **Une transparence sur les limites**, y compris méthodologiques : la v0.7 documente explicitement pourquoi une vérification antérieure avait initialement échoué à confirmer les données ADPT (blocage robots de l'interface web), pour que la méthode de contournement (API) profite aux révisions futures.

---

## 4. Tokenization et segmentation

### 4.1 Principe général

UD requiert que chaque **token** corresponde à un mot graphique, sauf exceptions justifiées (multi-word tokens, MWT).

### 4.2 Règles de tokenization

#### 4.2.1 Prépositions et adpositions

Les prépositions et adpositions kabyles restent des **tokens uniques ADP**. La segmentation en composants internes (ex : `deg` → `ad` + `ag`, pratiquée à 100 % dans ADPT) est **rejetée**, conformément aux recommandations communautaires UD sur la sur-segmentation.

| Forme graphique | Token | UPOS | Notes |
|----------------|-------|------|-------|
| `deg` | `deg` | ADP | Token unique (pas `ad`+`ag`) |
| `di` | `di` | ADP | Token unique |
| `ɣef` | `ɣef` | ADP | Token unique |
| `fell` | `fell` | ADP | Token unique |
| `ɣer` | `ɣer` | ADP | Token unique |
| `seg` / `si` | `seg` / `si` | ADP | Token unique |
| `s` | `s` | ADP | Token unique (instrumental) |
| `ger` | `ger` | ADP | Token unique |
| `zdat`, `nnig`, `ddaw`, `ar` | — | ADP | Tokens uniques |
| `i` | `i` | ADP | Token unique — **prépositionnel** (datif « à/pour »), distinct du `i` complémenteur (cf. 5.2.3) |
| `n` | `n` | ADP | Token unique (génitif) |

#### 4.2.2 Clitiques possessifs

Les clitiques possessifs (`-is`, `-ik`, `-im`, `-nneɣ`, `-nnwen`, `-nnsen`, `-nnsent`) sont **segmentés** en tokens séparés, conformément à la pratique UD standard pour les clitiques et à l'analyse d'Achab (2020) qui identifie une compositionnalité morphologique réelle (complémenteur relateur + particule génitive + suffixe personnel) dans ces formes.

| Forme graphique | Tokens | UPOS | Notes |
|----------------|--------|------|-------|
| `axxam-is` | `axxam` + `is` | NOUN + PRON | Possessif 3SG |
| `lbiru-ines` | `lbiru` + `ines` → `in`+`as` | NOUN + ADP + PRON | Voir décision ci-dessous |

**Décision** : `ines` est segmenté en MWT `in` (ADP, relation `case`) + `as` (PRON, relation `nmod`), conformément à la pratique ADPT (100 % des occurrences vérifiées) et à l'analyse théorique d'Achab (2020).

**Exemple corrigé** :
```
3   lbiru   lbiru   NOUN   _   Gender=Masc|Number=Sing              8   obl   _   _
4-5 ines    _       _      _   _                                    _   _     _   _
4   in      in      ADP    _   _                                    5   case  _   _
5   as      netta   PRON   _   Case=Acc|Number=Sing|Person=3|Poss=Yes|PronType=Rel  3  nmod  _  _
```

#### 4.2.3 Clitiques verbaux (objets)

Les clitiques objets verbaux (`-t`, `-tt`, `-as`, `-asent`, `-ten`, `-tent`, `-iyi`, `-ak`, `-am`, `-aɣ`, `-awen`) sont **segmentés** en tokens PRON séparés — analyse confirmée par Fahloune (2020).

| Forme graphique | Tokens | UPOS | Notes |
|----------------|--------|------|-------|
| `yefka-yas` | `yefka` + `yas` | VERB + PRON | Datif 3SG |
| `yefka-t` | `yefka` + `t` | VERB + PRON | Accusatif 3SG.M |
| `yefka-tt` | `yefka` + `tt` | VERB + PRON | Accusatif 3SG.F |

#### 4.2.4 Clitiques directionnels et adverbiaux

Les clitiques directionnels (`-d`, `-n`) sont segmentés en tokens PART séparés — attestés chez Fahloune (2020, ex. 21).

| Forme graphique | Tokens | UPOS | Notes |
|----------------|--------|------|-------|
| `yefka-d` | `yefka` + `d` | VERB + PART | Directionnel « vers le locuteur » |

#### 4.2.5 Affixes d'accord sujet

Les affixes d'accord sujet (préfixes `y-`, `i-`, `t-`, `n-` ; suffixes `-eɣ`, `-eḍ`, `-en`, `-ent`, `-em`, `-emt`) restent **fusionnés** avec le verbe en un seul token VERB. Décision confirmée par Fahloune (2020).

#### 4.2.6 Le second `ara` — **[NOUVEAU v0.7]**

Le marqueur `ara` irréalis/participe (§5.2.2bis) reste un **token unique séparé**, distinct du verbe, à l'instar du `ara` négatif. La distinction entre les deux fonctions se fait au niveau du UPOS/de la feature, pas de la tokenization.

### 4.3 Tableau récapitulatif des règles de tokenization

| Élément | Tokenization | Exemple |
|---------|--------------|---------|
| Verbe + affixes sujet | 1 token | `yekcem`, `tekcem`, `kcemeɣ` |
| Verbe + clitiques objets | Verbe + clitiques séparés | `yefka` `yas` `tt` |
| Verbe + clitique directionnel | Verbe + clitique séparé | `yefka` `d` |
| Nom + clitique possessif | Nom + clitique(s) séparé(s) | `axxam` `is` ; `lbiru` `in` `as` |
| Préposition | 1 token ADP | `deg`, `di`, `ɣef`, `fell`, `i` (datif) |
| Complémenteur relatif/clivée | 1 token SCONJ | `i` (perfectif) / `a` (imperfectif, aoriste) |
| Négation verbale `ur ... ara` | `ur` + `ara` séparés | `ur` `yekcem` `ara` |
| **`ara` irréalis/participe** | 1 token, séparé du verbe | `mi` `ara` `yili` — **NOUVEAU v0.7** |
| Négation ascriptive `mačči` | 1 token | `mačči` — **[À VALIDER]** |
| Copule `d` | 1 token AUX | `d` |
| Présentatifs `ha`/`aql`/`a`+suffixe | non spécifié | **[À VALIDER]** — cf. §5.2.6 |
| Coordination `neɣ` | 1 token CCONJ | `neɣ` |

---

## 5. POS tags (UPOS)

### 5.1 Inventaire UPOS utilisé

| UPOS | Usage kabyle | Exemples | Fréquence ADPT |
|------|--------------|----------|-------------------|
| `VERB` | Verbes de base et dérivés | `yekcem`, `yelli`, `yenna`, `eddu` | 3937 |
| `AUX` | Copule `d` uniquement | `d` (copule ascriptive) | Introduit — 0 dans ADPT |
| `PART` | Particules TAM (`ad`, `ur`), directionnelles (`d` directionnel), clitiques verbaux non-pronominaux, `ara` irréalis (provisoire, cf. §5.2.2bis) | `ad`, `ur`, `agi` | 2104 |
| `NOUN` | Noms communs | `ass`, `iman`, `abrid`, `awal` | 4428 |
| `PROPN` | Noms propres | `Ṭiṭem`, `azwaw`, `wejda` | 192 |
| `PRON` | Pronoms indépendants et clitiques | `netta`, `win`, `ayen` | 3394 |
| `DET` | Déterminants | `yal`, `le` | 67 |
| `ADJ` | Adjectifs qualificatifs | `anezmar`, `amenzu`, `amellal` | 162 |
| `NUM` | Numéraux | `yiwen`, `yiwet`, `sin`, `snat` | 111 |
| `ADV` | Adverbes ; `ara` négatif (cf. §5.2.2) | `kan`, `mi`, `tura`, `ur`, `ara` (négatif) | 1829 |
| `ADP` | Adpositions | `i` (datif), `n`, `ag`, `ɣef`, `ɣer` | 3148 |
| `CCONJ` | Conjonctions de coordination | `neɣ`, `maca`, `acku` | 131 |
| `SCONJ` | Complémenteurs relatifs/clivées | `i` (perfectif), `a` (imperfectif/aoriste), `ma`, `imi`, `lemmer` | 142 |
| `INTJ` | Interjections | `ah`, `ay`, `ih` | 81 |
| `PUNCT` | Ponctuation | `.`, `?`, `!`, `,`, `;`, `:`, `«`, `»` | 3949 |
| `X` | Éléments non analysés | `_` | 86 |

**Correction apportée en v0.5, maintenue** : la particule `ad` est fixée sous **PART** uniquement, conformément à la convergence de trois sources (Fahloune, Baier, Achab 2020).

**Point de vigilance ajouté en v0.7** : la table ci-dessus liste `ara` à deux endroits différents (sous ADV pour sa fonction négative, sous PART pour sa fonction irréalis provisoire). Ce n'est pas une incohérence : c'est la conséquence directe de son statut homographe, développé en détail en §5.2.2bis. Les annotateurs doivent impérativement lire cette section avant de tagger toute occurrence de `ara`.

### 5.2 Cas particuliers

#### 5.2.1 La particule `d` (copule ascriptive) — RÉSOLU

**Décision, sourcée** : `d` est annoté **AUX** avec la relation **`cop`**. Cette décision s'appuie directement sur Mettouchi (2017), qui décrit `d` comme *« a non-verbal copula »* et la glose systématiquement **COP/PRED** dans toutes les prédications ascriptives (affirmatives et négatives) de son corpus annoté CorTypo. Citation vérifiée mot pour mot dans le PDF source.

- **Ascriptive affirmative** : `d` = AUX, relation `cop`. Ex. : « *wagi d lḥağ ṭaḥaṛ* » (« celui-ci est Hadj Tahar »).
- **Ascriptive dans les clivées** : même analyse. Ex. : « *D nekk i y-ldi-n tabburt* » (« C'est moi qui ai ouvert la porte »), cf. aussi Achab (2020).

**Clarification** : le dilemme « copule vs présentatif » soulevé dans la v0.3 reposait sur une fausse alternative. Mettouchi (2017) montre que la copule ascriptive `d` et les constructions **présentatives** (§5.2.6) sont deux constructions grammaticales **distinctes**, marquées par des morphèmes différents.

**Indice corroborant, ajouté en v0.7** : l'historique des commits de la branche `dev` d'ADPT (Dan Zeman, octobre 2022) contient des messages *« Spurious copula »* et *« Spurious auxiliaries »*, documentant le retrait de candidats `cop`/`AUX` jugés erronés dans une étape antérieure du pipeline. Cela confirme, indépendamment de Mettouchi, que le statut de `d` était déjà identifié comme une zone de friction dans le traitement automatique du kabyle — cohérent avec le constat de cette spec que la question mérite une décision sourcée plutôt qu'une heuristique de conversion.

**Exemple corrigé** :
```
5   d   d   AUX   _   PartType=Cop   6   cop   _   _
6   amẓallu   amẓallu   NOUN   _   Gender=Masc|Number=Sing|Case=Nom   0   root   _   SpaceAfter=No
```
(Phrase authentique du corpus ADPT test, ligne 322 : « *Bab n lbiru-nni d amẓallu, d ineslem.* »)

#### 5.2.2 La particule `ur` et le `ara` **négatif** (négation verbale)

**Décision** : `ur` et le `ara` négatif sont annotés **ADV** avec `Polarity=Neg` et la relation `advmod`, conformément aux données ADPT.

**Statut de la source** : partiellement sourcé. Les trois articles McGill WP 26.1 (Fahloune, Baier, Achab 2020) désignent `ur` comme une *particule*, ce qui exclut AUX mais ne tranche pas entre ADV et PART. Mettouchi a des travaux dédiés à `ara` (2001, 2004) qui n'ont pas encore été consultés en texte intégral — cf. §12, L2. **Le titre de Mettouchi (2001), « La grammaticalisation de ara en kabyle, négation et subordination relative », suggère fortement que cette source traite précisément de la distinction développée en §5.2.2bis ci-dessous — lecture prioritaire pour la v0.8.**

**Important — ne pas confondre avec le `ara` irréalis (§5.2.2bis)** : cette section ne couvre que l'emploi de `ara` comme second membre de la négation discontinue `ur ... ara`, où il co-apparaît systématiquement avec `ur` dans la même proposition. Tout `ara` apparaissant sans `ur` dans la même proposition relève très probablement de l'autre fonction — voir la règle de désambiguïsation en §5.2.2bis.

```
1   ur   ur   ADV   _   Polarity=Neg   3   advmod   _   _
2   ǧin  eǧǧ  VERB  _   _              3   advmod   _   _
3   isugen esug VERB _   _             0   root     _   _
```

```
# text = Ur yekcem ara w-qcic.
1   ur      ur     ADV    _   Polarity=Neg   2   advmod   _   _
2   yekcem  ekcem  VERB   _   Gender=Masc|Mood=Ind|Number=Sing|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
3   ara     ara    ADV    _   Polarity=Neg   2   advmod   _   _
4   w-qcic  qcic   NOUN   _   Gender=Masc|Number=Sing|Case=Nom   2   nsubj   _   _
5   .       .      PUNCT  _   _   2   punct   _   _
```

#### 5.2.2bis Le second `ara` : marqueur d'irréalis/participe aoriste — **[NOUVEAU v0.7, CORRECTION MAJEURE]**

**Statut** : correction d'une lacune identifiée par validation native (Mokraoui, août 2026), confirmée par la grammaire de référence sur le kabyle (Naït-Zerrad 2001 ; Mammeri 1976 ; Chaker 1983 ; cf. §2.10). Cette lacune n'avait été détectée par aucune des six révisions précédentes de cette spec (v0.1 à v0.6), malgré la consultation de cinq sources académiques en texte intégral — ce qui illustre les limites d'une vérification purement bibliographique face à une intuition native bien informée.

**Le problème.** `ara` est un **morphème homographe** en kabyle. Les versions précédentes de cette spec (§5.2.2, §6.6, §12 L2) ne traitaient que sa fonction de négation, ce qui revenait à tagger systématiquement toute occurrence de la chaîne `ara` comme `ADV Polarity=Neg` — une règle **fausse** pour une partie substantielle des occurrences réelles.

`ara` recouvre en réalité deux morphèmes distincts, à ne jamais confondre :

1. **`ara` négatif** (§5.2.2) : second membre de la négation discontinue `ur ... ara`, co-occurrent obligatoire de `ur` dans la même proposition. UPOS `ADV`, feature `Polarity=Neg`, relation `advmod`.

2. **`ara` irréalis/participe** (ce paragraphe) : apparaît (a) dans les subordonnées temporelles ou conditionnelles introduites par `mi`, `asmi`, `ma`, où il marque un futur/irréalis (« quand..., lorsque... [événement futur ou hypothétique] ») ; et (b) comme marqueur du **participe positif de l'aoriste**, en alternance avec le participe négatif marqué par `ur n(e)-`. N'apparaît **jamais** avec `ur` dans la même proposition et ne porte **aucune** valeur de négation — la proposition où il figure est pleinement affirmative.

**Exemples attestés** (grammaire de référence et matériel pédagogique kabyle) :
- *Mi ara yili yisem d unti...* — « Lorsque le nom est féminin... » (aucune négation ; `mi` = « quand », `ara` = irréalis, `yili` = « soit/sera »)
- *Asmi ara yili yisem d asemmad...* — « Quand le nom est un complément... » (même construction)
- *ara yafgen* — « qui volera » (participe aoriste positif, face au participe négatif *ur nufig* « qui ne vole pas »)

**Justification linguistique du homographe (et non d'un simple polysémie d'un même marqueur)** : dans la négation discontinue `ur ... ara`, `ara` n'a pas de sens propre — c'est un renforçateur obligatoire de la négation portée par `ur`, sans lien synchronique transparent avec le futur/irréalis. Dans les constructions `mi ara` / `asmi ara` / au participe positif de l'aoriste, `ara` porte au contraire une valeur modale-temporelle autonome (irréalis, futur), totalement indépendante de la polarité de la phrase. Le fait que les deux emplois partagent l'étiquette graphique et probablement une origine diachronique commune (grammaticalisation, cf. le titre de Mettouchi 2001) n'empêche pas qu'ils doivent être traités, du point de vue de l'annotation synchronique UD, comme deux unités fonctionnelles distinctes — de la même manière que UD distingue systématiquement les homographes fonctionnellement différents dans d'autres langues (ex. l'anglais *that* complémenteur vs. déterminant démonstratif).

**Décision d'annotation — [À VALIDER, confiance modérée]** :

- **UPOS** : `PART`, par analogie avec `ad` (autre marqueur TAM préverbal invariable, cf. §5.2.4), en l'absence d'une source dédiée tranchant explicitement la question. Une lecture de Mettouchi (2001, 2004) pourrait imposer une révision de ce choix — voir §12 L2 mis à jour.
- **Feature** : `Mood=Irr` pour la fonction irréalis en subordonnée (nouvelle valeur de feature à ajouter à l'inventaire, cf. §7.2 mis à jour) ; `VerbForm=Part` porté par le verbe lui-même reste la feature appropriée quand `ara` accompagne un participe de l'aoriste.
- **Relation** : `advmod` par défaut (rattaché au verbe qu'il module), ou `aux` si une analyse future établit un comportement plus proche de l'auxiliaire modal — question ouverte, à trancher avec la littérature dédiée.

**Règle de désambiguïsation obligatoire pour les annotateurs** :

> Si la chaîne `ara` apparaît **sans `ur` dans la même proposition**, ce n'est **pas** la négation. Ne jamais tagger `Polarity=Neg` par défaut sur toute occurrence de `ara`. Vérifier systématiquement la présence ou l'absence de `ur` co-occurrent avant de choisir l'analyse.

**Exemple CoNLL-U (nouveau)** :
```conllu
# sent_id = kab-012
# text = Mi ara yili yisem d unti.
1   Mi      mi      SCONJ  _   _                              3   mark    _   _
2   ara     ara     PART   _   Mood=Irr                        3   advmod  _   _
3   yili    ili     VERB   _   Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
4   yisem   isem    NOUN   _   Gender=Masc|Number=Sing|Case=Nom   3   nsubj   _   _
5   d       d       AUX    _   PartType=Cop                    6   cop     _   _
6   unti    tunṭ    NOUN   _   Gender=Fem|Number=Sing|Case=Nom  4   conj    _   _
7   .       .       PUNCT  _   _                                3   punct   _   _
```
*(Analyse provisoire, notamment pour l'articulation `yisem d unti` — cf. §12, nouvel item ouvert L18.)*

**Ce que cette correction ne règle pas** : le statut exact de `mi` dans cette construction (SCONJ, comme proposé ci-dessus par analogie avec les autres subordonnants temporels, mais non vérifié en détail dans cette révision), et l'articulation précise entre `ara` irréalis et le système aspectuel décrit en §7.2.1. Ces points restent ouverts pour une v0.8, après lecture de Mettouchi (2001, 2004).

#### 5.2.3 Le complémenteur relatif/clivée `i` / `a` — RÉSOLU, avec nuance ajoutée

**Décision, sourcée** : le complémenteur relatif et clivé est annoté **SCONJ** avec la relation **`mark`**. Cette décision s'appuie sur Achab (2020), qui glose systématiquement cet élément **COMP** dans les questions-WH, relatives et clivées.

**Nuance ajoutée en v0.5** : le complémenteur **alterne selon l'aspect** :
- `i` au perfectif : « *Anta i y-ldi-n tawwurt?* » (« Qui a ouvert la porte ? »)
- `a` à l'imperfectif ou à l'aoriste : « *D nekk a y-leddi-n tabburt* » (« C'est moi qui ouvre la porte »)

**[À VALIDER]** : confirmer que ce `a` complémenteur (SCONJ) est bien distinct de l'homographe `a-` (préfixe d'état libre nominal, §5.2.5), de l'`a` vocatif (§8.4), et — point de vigilance ajouté en v0.7 par analogie avec le cas `ara` — s'assurer qu'aucune quatrième confusion homographique n'a échappé à cette révision. La découverte du homographe `ara` en v0.7 invite à une relecture systématique de tous les morphèmes monosyllabiques de la spec avant la v0.8 (cf. §12, nouvel item L19).

**Distinction cruciale avec le `i` prépositionnel** : le `i` complémenteur (SCONJ) est un morphème distinct du `i` préposition datif (« à/pour »), qui reste ADP.

**Exemple corrigé** :
```
1   win   win   PRON   _   Gender=Masc|Number=Sing|Person=3|PronType=Dem   4   nsubj   _   _
2   i     i     SCONJ  _   _                                                4   mark    _   _
3   d     d     PART   _   _                                                4   advmod  _   _
4   yekksen  ekkes  VERB  _  Gender=Masc|Mood=Ind|Number=Plur|Person=3|Tense=Past|VerbForm=Fin  0  root  _  _
5   .   .   PUNCT   _   _                                                    4   punct   _   _
```

#### 5.2.4 La particule `ad` (futur/aoriste) — tranché en PART, source partielle

**Décision** : `ad` est annotée **PART** avec la relation `advmod`, jamais AUX.

**Statut de la source** : trois articles indépendants (Fahloune, Baier, Achab, McGill WP 26.1 2020) désignent systématiquement `ad` comme *particle*. **[À VALIDER — confiance modérée]**.

**Point de vigilance ajouté en v0.7** : `ad` (futur) et `ara` (irréalis, §5.2.2bis) sont deux marqueurs TAM préverbaux distincts, jamais interchangeables — ne pas les confondre malgré leur fonction modale-temporelle apparentée. `ad` marque le futur de l'aoriste en proposition principale ; `ara` marque l'irréalis en subordonnée ou le participe aoriste positif. Leur distribution syntaxique ne se recouvre pas.

```
1   ad   ad   PART   _   _   3   advmod   _   _
2   ad   ad   PART   _   _   3   advmod   _   _
3   nbeddel   ebeddel   VERB   _   _   0   root   _   _
```

#### 5.2.5 État libre (FS) vs État d'annexion (CS)

**Décision, sourcée** : Les préfixes nominaux (`a-`, `ta-`, `i-`, `ti-`, `w-`, `t-`, `y-`) ne sont pas segmentés en tokens séparés. Leur statut FS/CS est encodé dans les features morphologiques (`Case=Nom|Acc|Dat`), conformément aux données ADPT et à l'analyse de Felice (2020).

##### 5.2.5.1 Règles de formation de l'état d'annexion

| État libre (FS) | État d'annexion (CS) | Transformation | Exemple |
|-----------------|----------------------|----------------|---------|
| `a-` (masc sing) | `w-` | Préfixe `a-` → `w-` | `axxam` → `wexxam` |
| `ta-` (fem sing) | `t-` | Préfixe `ta-` → `t-` (inchangé orthographiquement) | `taddart` → `taddart` |
| `i-` (masc plur) | `y-` | Préfixe `i-` → `y-` | `irgazen` → `yirgazen` |
| `ti-` (fem plur) | `t-` | Préfixe `ti-` → `t-` (inchangé orthographiquement) | `tilawin` → `tilawin` |

**Note** : Les transformations orthographiques `a-` → `w-` et `i-` → `y-` sont des réalisations phonologiques de l'état d'annexion, documentées par Felice (2020) et Mettouchi & Frajzyngier (2013). Elles ne justifient pas une segmentation en tokens séparés.

##### 5.2.5.2 Contextes déclencheurs de l'état d'annexion

L'état d'annexion est **obligatoire** dans les contextes suivants (Felice 2020, §3) :

1. **Après préposition** : `ɣer wexxam` "vers la maison" (pas `ɣer axxam`)
2. **Dans les constructions possessives** : `axxam n w-qcic` "la maison du garçon"
3. **Après numéraux** : `sin wussan` "deux jours" (pas `sin assen`)
4. **Après certains adjectifs** : `amɣar ameqqran` "le vieil homme"
5. **En position sujet post-verbal** (optionnel mais fréquent en VSO) : `yekcem w-qcic` "le garçon est entré"

##### 5.2.5.3 Convention d'annotation

**Lemme** : Toujours l'état libre (forme de citation du dictionnaire).
**Forme de surface** : Reflète l'état d'annexion si le contexte l'exige.
**Feature `Case`** : Encode le statut FS/CS (`Case=Nom` pour CS, `Case=Acc` pour FS, `Case=Dat` après préposition datif).

**Exemple CoNLL-U** :
```conllu
# sent_id = kab-cs-001
# text = Yekcem ɣer wexxam.
1   Yekcem   ekcem   VERB   _   Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
2   ɣer      ɣer     ADP    _   _                                                3   case   _   _
3   wexxam   axxam   NOUN   _   Gender=Masc|Number=Sing|Case=Acc                 1   obl    _   _
4   .        .       PUNCT  _   _                                                1   punct   _   _
```

#### 5.2.6 Constructions présentatives — **[NON SPÉCIFIÉ]**

Le kabyle distingue la copule ascriptive `d` (§5.2.1) de constructions **présentatives** dédiées, jamais marquées par `d` :

| Forme | Personne | Construction |
|-------|----------|--------------|
| `ha` + pronom absolutif clitique | 3e personne (sg/pl) | Peut être suivi d'un NP à l'état d'annexion |
| `a` + pronom absolutif + `-an`/`-ad` | 3e personne (sg/pl) | `-an` distal, `-ad` proximal |
| `aql` + pronom absolutif clitique | 1re/2e personne (sg/pl) | Peut être suivi d'un NP à l'état d'annexion |

Exemple (Mettouchi 2017) : « *ḥaṭan a Amina* » (« La voici, Amina »).

**[À VALIDER]** : ce point nécessite un travail de spécification à part entière. **Point de vigilance ajouté en v0.7** : le `a` de cette construction (« présentatif distal ») est un **cinquième** candidat homographe potentiel avec le `a` complémenteur (§5.2.3), le `a-` préfixe d'état libre (§5.2.5) et le `a` vocatif (§8.4) — à traiter avec la même rigueur que le cas `ara` lors de la spécification complète de cette construction.

#### 5.2.7 La négation ascriptive `mačči` — **[NON SPÉCIFIÉ, DONNÉE EMPIRIQUE NOUVELLE]**

La négation des prédications ascriptives (copule `d`) ne se fait **pas** avec `ur...ara`, mais avec un marqueur dédié `mačči` (glosé NEG.ATTR par Mettouchi 2017), qui précède la copule. Ce fait est corroboré indépendamment par une seconde source de Mettouchi (compte rendu de conférence EPHE) qui situe `mačči` parmi les marqueurs de « négation équative ou attributive » du kabyle, en face de `ulac` (négation d'existence).

Exemple (Mettouchi 2017) : « *ma d aqdim nəɣ mačči d aqdim* » (« qu'il soit vieux ou non »).

**Donnée empirique nouvelle (v0.7)** : dans les données réelles ADPT (branche `dev`, vérifiée via §3.1bis), `mačči` figure parmi les lemmes les plus fréquents de la catégorie **CCONJ**, aux côtés de `neɣ`, `maca`, `acku`, `wala`, `mi`. Ce n'est pas une décision d'annotation qualifiée — c'est un artefact du pipeline de conversion ADPT (probablement une heuristique de conversion automatique, comme suggéré par les commits « Spurious... » discutés en §3.1bis) — mais c'est une donnée à intégrer dans la réflexion sur l'UPOS définitif de `mačči`.

**[À VALIDER]** : tag UPOS de `mačči` (candidats : PART par analogie avec `ur` ; ADV ; ou CCONJ par fidélité à la pratique ADPT observée, bien que cette dernière ne semble pas fondée sur une analyse linguistique dédiée) et relation (`advmod` ou relation dédiée à la négation ascriptive, en cohérence avec le traitement de `cop`).

---

## 6. Relations de dépendances

### 6.1 Inventaire des relations utilisées (ADPT, vérifié — voir méthode §3.1bis)

| Relation | Fréquence ADPT | Usage |
|----------|-------------------|-------|
| `acl` | 1657 | Modificateur clausal d'un nom |
| `acl:relcl` | 4 | Proposition relative |
| `advcl` | 88 | Modificateur clausal adverbial |
| `advmod` | 3734 | Modificateur adverbial |
| `amod` | 140 | Modificateur adjectival |
| `appos` | 4 | Apposition |
| `case` | 3594 | Marqueur de cas (préposition) |
| `cc` | 90 | Conjonction de coordination |
| `ccomp` | 28 | Complément clausal |
| `compound` | 20 | Composé |
| `conj` | 1234 | Conjonction |
| `dep` | 1 | Dépendance non classée |
| `det` | 859 | Déterminant |
| `discourse` | 7 | Élément discursif |
| `dislocated` | 14 | Élément disloqué |
| `iobj` | 303 | Objet indirect |
| `mark` | 318 | Marqueur de subordination |
| `nmod` | 1287 | Modificateur nominal |
| `nsubj` | 941 | Sujet nominal |
| `nummod` | 22 | Modificateur numéral |
| `obj` | 1156 | Objet direct |
| `obl` | 1896 | Modificateur oblique |
| `obl:arg` | 1 | Modificateur oblique argumental |
| `parataxis` | 1 | Parataxe |
| `punct` | 3949 | Ponctuation |
| `root` | 1930 | Racine |
| `vocative` | 62 | Vocatif |
| `xcomp` | 421 | Complément clausal ouvert |
| `cop` | **Introduit** | Copule (sourcé, Mettouchi 2017) |
| `expl` | **Introduit** | Expletive / clitic doubling (sourcé, Fahloune 2020) |

### 6.2 Ordre VSO et sujet

```
Yekcem w-qcic ɣer wexxam .
nsubj(Yekcem, w-qcic)
obl(Yekcem, wexxam)
case(wexxam, ɣer)
```

### 6.3 Objets directs et indirects

#### 6.3.1 Objet lexical sans clitique
```
Yefka w-qcic aɣrum i wemcic .
nsubj(Yefka, w-qcic)
obj(Yefka, aɣrum)
obl(Yefka, wemcic)
case(wemcic, i)
```

#### 6.3.2 Clitic doubling (objet lexical + clitique) — RÉSOLU

**Décision, sourcée** : conformément à Fahloune (2020), l'argument lexical reçoit la relation sémantique (`obj`, `iobj`), et le clitique co-occurrent est annoté **`expl`**.

```
Yefka-yas lqahwa i Xira .
nsubj(Yefka, sujet_implicite_ou_lexical)
obj(Yefka, lqahwa)
iobj(Yefka, Xira)
case(Xira, i)
expl(Yefka, yas)
```

**Exemple CoNLL-U** :
```
1   Yefka   efk   VERB   _   Gender=Masc|Mood=Ind|Number=Sing|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
2   yas     yas   PRON   _   Gender=Fem|Number=Sing|Person=3|Poss=Yes|PronType=Prs                1   expl   _   _
3   lqahwa  lqahwa NOUN  _   Gender=Fem|Number=Sing|Case=Acc                                       1   obj    _   _
4   i       i     ADP    _   _                                                                     5   case   _   _
5   Xira    Xira  PROPN  _   Gender=Fem|Number=Sing                                                1   iobj   _   _
6   .       .     PUNCT  _   _                                                                     1   punct  _   _
```

**Résidu ouvert** : le choix de la relation UD `expl` elle-même reste emprunté aux conventions communautaires (grec, bulgare, roumain) sans vérification directe des treebanks correspondants (cf. §12, L3).

#### 6.3.3 Objet pronominal (clitique seul)
```
Yefka-yas lqahwa .
obj(Yefka, lqahwa)
iobj(Yefka, yas)   # pas expl, car pas d'argument lexical
```

### 6.4 Copule `d` — RÉSOLU (cf. §5.2.1)

```
D lweḥda .
cop(lweḥda, d)
root(lweḥda)

Netta d amedyaz .
nsubj(amedyaz, netta)
cop(amedyaz, d)
root(amedyaz)
```

### 6.5 Marqueur relatif/clivée `i`/`a` — RÉSOLU (cf. §5.2.3)
```
win i d-yekksen .
nsubj(yekksen, win)
mark(yekksen, i)
root(yekksen)
```

### 6.6 Négation discontiguë `ur ... ara` (fonction négative uniquement)

```
Ur yekcem ara w-qcic .
advmod(yekcem, ur)
advmod(yekcem, ara)
nsubj(yekcem, w-qcic)
```

**Rappel v0.7** : ce patron ne s'applique qu'à la fonction négative de `ara`. Pour la fonction irréalis, voir §6.6bis.

### 6.6bis `ara` irréalis en subordonnée — **[NOUVEAU v0.7]**

```
Mi ara yili yisem d unti .
mark(yili, Mi)
advmod(yili, ara)
cop(unti, d)
nsubj(unti, yisem)
```

Ce patron ne comporte **aucune** relation de polarité négative — `ara` module ici la modalité (irréalis) sans nier la proposition.

### 6.7 Négation ascriptive `mačči` — **[À VALIDER]**
```
Mačči d aqdim .
advmod(aqdim, mačči)   # provisoire — à trancher, cf. §5.2.7
cop(aqdim, d)
root(aqdim)
```

### 6.8 Coordination
```
Yekcem w-qcic neɣ tekcem teqcict .
nsubj(yekcem, w-qcic)
conj(yekcem, tekcem)
cc(yekcem, neɣ)
nsubj(tekcem, teqcict)
```

### 6.9 Possession (état d'annexion)
```
axxam n w-qcic
nmod(axxam, w-qcic)
case(w-qcic, n)
```

---

## 7. Features morphologiques (feats)

### 7.1 Features utilisées dans les données ADPT

| Feature | Valeurs | Fréquence | Notes |
|---------|---------|-----------|-------|
| `Case` | `Acc`, `Dat`, `Nom` | Acc: 2147, Dat: 2116, Nom: 2392 | Encode FS/CS |
| `Definite` | `Ind` | 10 | Pour les déterminants indéfinis |
| `Gender` | `Fem`, `Masc` | Fem: 2756, Masc: 6666 | Genre grammatical |
| `Mood` | `Imp`, `Ind` | Imp: 99, Ind: 3366 | Mode verbal (ADPT ne connaît pas encore `Irr`, cf. §7.2 v0.7) |
| `Number` | `Plur`, `Sing` | Plur: 2797, Sing: 7947 | Nombre |
| `Person` | `1`, `2`, `3` | 1: 374, 2: 302, 3: 5256 | Personne |
| `Polarity` | `Neg` | 406 | Négation verbale (inclut le `ara` négatif, mais possiblement contaminée par des occurrences mal étiquetées du `ara` irréalis — cf. §5.2.2bis) |
| `Poss` | `Yes` | 1434 | Possessif |
| `PronType` | `Art`, `Dem`, `Int`, `Rel` | Art: 15, Dem: 485, Int: 370, Rel: 2414 | Type de pronom |
| `Tense` | `Fut`, `Past`, `Pres` | Fut: 328, Past: 2484, Pres: 1105 | Temps |
| `VerbForm` | `Fin`, `Part` | Fin: 3438, Part: 474 | Forme verbale |
| `Voice` | `Pass` | 46 | Voix passive |

**Point de vigilance ajouté en v0.7** : les 406 occurrences de `Polarity=Neg` dans ADPT n'ont pas été auditées une par une pour vérifier qu'aucune ne recouvre en réalité un `ara` irréalis mal étiqueté (le corpus ADPT ayant été annoté sans que cette distinction soit connue de ses annotateurs). Un audit de ce sous-ensemble est recommandé avant la v0.8 — voir §12, nouvel item L20.

### 7.2 Verbes

| Feature | Valeurs | Description |
|---------|---------|--------------|
| `Tense` | `Past`, `Pres`, `Fut` | Passé, présent, futur |
| `Mood` | `Ind`, `Imp` | Indicatif, impératif |
| `Mood` | `Irr` **[NOUVEAU v0.7, À VALIDER]** | Irréalis — porté par la particule `ara` (§5.2.2bis), pas nécessairement par le verbe lui-même ; valeur ajoutée ici pour discussion, sa localisation exacte (sur le verbe ou sur la particule) reste à trancher |
| `VerbForm` | `Fin`, `Part` | Forme finie, participe |
| `Voice` | `Pass` | Voix passive |
| `Polarity` | `Neg` | Négation |
| `Person` / `Number` / `Gender` | — | Accord (véritable accord, cf. Fahloune 2020) |
| `Aspect` | `Prog`, `Perf` | Cf. §7.2.1 |

#### 7.2.1 Extraction des traits verbaux

**Statut** : Convention d'annotation pragmatique, sourcée par Fahloune (2020) pour l'accord sujet et Bouamara (2026) pour les paradigmes verbaux. Validée empiriquement sur le corpus Tatoeba (756k phrases, couverture 98.14%).

##### 7.2.1.1 Préfixes/suffixes de personne (accord sujet obligatoire)

| Personne | Préfixe | Suffixe | Exemple | Traduction |
|----------|---------|---------|---------|------------|
| 1sg | `n-`/`ne-` | `-eɣ` | `nekrez` | "je laboure" |
| 2sg.m | `t-`/`te-` | `-eḍ` | `tekrezeḍ` | "tu (m) laboures" |
| 2sg.f | `t-`/`te-` | `-eḍ` | `tekrezeḍ` | "tu (f) laboures" |
| 3sg.m | `y-`/`ye-` | — | `yekrez` | "il laboure" |
| 3sg.f | `t-`/`te-` | — | `tekrez` | "elle laboure" |
| 1pl | `n-`/`ne-` | — | `nekrez` | "nous labourons" |
| 2pl.m | `t-`/`te-` | `-em` | `tekrezen` | "vous (m) labourez" |
| 2pl.f | `t-`/`te-` | `-emt` | `tekrezenmt` | "vous (f) labourez" |
| 3pl.m | — | `-en` | `krezen` | "ils labourent" |
| 3pl.f | — | `-ent` | `krezent` | "elles labourent" |

**Exemple CoNLL-U** :
```conllu
1   yekrez   krez   VERB   _   Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
```

##### 7.2.1.2 Marqueurs aspectuels

| Marqueur | Aspect | Exemple | Traduction | Feature UD |
|----------|--------|---------|------------|------------|
| `tt-`/`tte-` | Imperfectif/Progressif | `yettawi` | "il porte (en cours)" | `Aspect=Prog` |
| `la` (particule) | Progressif (variante) | `la ttedduɣ` | "je suis en train de marcher" | `Aspect=Prog` sur le verbe |
| (absence) | Perfectif/Aoriste | `yekrez` | "il a labouré" | `Aspect=Perf` (implicite) |

**Exemple CoNLL-U** :
```conllu
1   yettawi   awi   VERB   _   Aspect=Prog|Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
```

##### 7.2.1.3 Marqueurs de voix dérivationnels

| Préfixe | Voix | Exemple | Traduction | Feature UD |
|---------|------|---------|------------|------------|
| `ttwa-`/`ttu-` | Passif | `yettwaqqen` | "il est attaché" | `Voice=Pass` |
| `ss-`/`sse-` | Causatif | `ssneɣ` | "je fais savoir" | `Voice=Caus` **[À VALIDER]** |
| `mye-`/`mya-` | Réciproque | `myeɣ` | "nous nous battons" | `Voice=Rcp` **[À VALIDER]** |

**Exemple CoNLL-U** :
```conllu
1   yettwaqqen   qqen   VERB   _   Aspect=Prog|Gender=Masc|Number=Sing|Person=3|VerbForm=Fin|Voice=Pass   0   root   _   _
```

##### 7.2.1.4 Limites et résidus ouverts

- **[À VALIDER]** : Les verbes de qualité ont des paradigmes d'accord différents selon l'aspect (Fahloune 2020, §3.2).
- **[À VALIDER]** : L'interaction entre aspect et voix nécessite une validation linguistique formelle.
- **[À VALIDER]** : Les voix causative et réciproque ne sont pas des valeurs UD standard.
- **[NOUVEAU v0.7, À VALIDER]** : l'interaction entre `Mood=Irr` (porté par `ara`, §5.2.2bis) et le système aspectuel décrit ici n'a pas été spécifiée — en particulier, le participe aoriste positif en `ara` (ex. *ara yafgen*) combine potentiellement `VerbForm=Part` sur le verbe et `Mood=Irr` sur la particule, une articulation qui reste à valider.

### 7.3 Noms

| Feature | Valeurs | Description |
|---------|---------|--------------|
| `Gender` | `Masc`, `Fem` | Genre |
| `Number` | `Sing`, `Plur` | Nombre |
| `Case` | `Nom`, `Acc`, `Dat` | Cas (encode FS/CS, cf. §5.2.5) |
| `Definite` | `Ind` | Défini/indéfini |

### 7.4 Pronoms et clitiques

| Feature | Valeurs | Description |
|---------|---------|--------------|
| `PronType` | `Prs`, `Dem`, `Rel`, `Int`, `Art` | Personnel, démonstratif, relatif, interrogatif, article |
| `Person` / `Number` / `Gender` | — | — |
| `Poss` | `Yes` | Possessif |
| `Case` | `Nom`, `Acc`, `Dat` | Cas |

### 7.5 Particules

| Feature | Valeurs | Description |
|---------|---------|--------------|
| `Polarity` | `Neg` | Négation verbale (`ur`, `ara` négatif) |
| `Mood` | `Irr` **[NOUVEAU v0.7]** | Irréalis (`ara` irréalis, cf. §5.2.2bis) — jamais combiné avec `Polarity=Neg` sur le même token |
| `PronType` | `Rel` | Relatif (`i`/`a`) |
| `PartType` | `Cop` | Copule (`d`) — appliqué à AUX, pas PART |

---

## 8. Cas particuliers et ambiguïtés

### 8.1 Verbes d'état
```
Llan wussan .
nsubj(llan, wussan)
root(llan)
```

### 8.2 Supplétisme
```
Yettakki w-qcic aɣrum .
nsubj(yettakki, w-qcic)
obj(yettakki, aɣrum)
```

### 8.3 Emprunts

#### 8.3.1 Emprunts intégrés vs non-intégrés

**Statut** : Convention d'annotation pragmatique, validée empiriquement sur le corpus Tatoeba (756k phrases).

**Critère de distinction** : Un emprunt est considéré comme **intégré morphologiquement** s'il participe aux paradigmes flexionnels du kabyle (état d'annexion, pluriel, accord adjectival). Sinon, il est considéré comme **non-intégré**.

**Convention d'annotation** :
- **Emprunts intégrés** : Annotés avec leur UPOS approprié et les features morphologiques correspondantes.
- **Emprunts non-intégrés** : Annotés `X` si aucune analyse morphologique n'est possible.

**Exemples d'emprunts intégrés** (fréquents dans le corpus Tatoeba, >100 occurrences chacun) :

| Emprunt | Origine | UPOS | Justification |
|---------|---------|------|----------------|
| `leqdic` | Arabe dialectal | NOUN | Participe à l'état d'annexion (`n leqdic`), a un pluriel |
| `ṣṣbeḥ` | Arabe classique | NOUN | Participe à l'état d'annexion (`n ṣṣbeḥ`) |
| `lbaṭaṭa` | Arabe dialectal | NOUN | Participe à l'état d'annexion, a un pluriel |
| `ṭṭabla` | Français | NOUN | Participe à l'état d'annexion (`n ṭṭabla`) |
| `lqahwa` | Arabe classique | NOUN | Participe à l'état d'annexion, très fréquent |

**Exemple CoNLL-U** :
```conllu
# sent_id = kab-loan-001
# text = Ssiweḍ lqahwa i wemcic.
1   Ssiweḍ    ssiweḍ    VERB   _   Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
2   lqahwa    lqahwa    NOUN   _   Gender=Fem|Number=Sing|Case=Acc                  1   obj    _   _
3   i         i         ADP    _   _                                                4   case   _   _
4   wemcic    emcic     NOUN   _   Gender=Masc|Number=Sing|Case=Dat                 1   iobj   _   _
5   .         .         PUNCT  _   _                                                1   punct  _   _
```

**Exemples d'emprunts non-intégrés** :
- Noms propres étrangers non adaptés : `Mary`, `John` → `PROPN`
- Interjections françaises : `merci`, `bonjour` → `INTJ` ou `X` selon le contexte
- Code-switching complet → `X` pour les tokens français

##### 8.3.1.1 Limites et résidus ouverts

- **[À VALIDER]** : Le degré d'intégration morphologique est un continuum, pas une dichotomie.
- **[À VALIDER]** : La distinction emprunt intégré vs mot kabyle natif est parfois arbitraire pour les emprunts anciens.
- **[À VALIDER]** : Cette convention n'est pas validée par la littérature linguistique sur les emprunts en berbère.

### 8.4 Vocatif
```
A nnbi !
vocative(nnbi, a)
root(nnbi)
```
**Point de vigilance (v0.5, renforcé en v0.7)** : le `a` vocatif est à distinguer du `a` complémenteur imperfectif/aoriste (§5.2.3), du `a-` préfixe d'état libre nominal (§5.2.5), et du `a` présentatif (§5.2.6) — au moins quatre morphèmes homographes distincts identifiés à ce stade. La découverte du homographe `ara` (§5.2.2bis) confirme que le kabyle présente une charge homographique importante sur ses morphèmes fonctionnels monosyllabiques ou bisyllabiques, justifiant une vigilance méthodologique systématique pour toute future extension de cette spec.

### 8.5 Éléments disloqués
```
Netta, yekcem .
dislocated(yekcem, netta)
root(yekcem)
```

---

## 9. Exemples CoNLL-U complets

### Exemple 1 : Phrase verbale transitive (VSO)
```conllu
# sent_id = kab-001
# text = Yefka w-qcic aɣrum i wemcic.
1   Yefka   efk   VERB   _   Gender=Masc|Mood=Ind|Number=Sing|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
2   w-qcic  qcic  NOUN   _   Gender=Masc|Number=Sing|Case=Nom                                      1   nsubj  _   _
3   aɣrum   aɣrum NOUN   _   Gender=Masc|Number=Sing|Case=Acc                                      1   obj    _   _
4   i       i     ADP    _   _                                                                      5   case   _   _
5   wemcic  emcic NOUN   _   Gender=Masc|Number=Sing|Case=Dat                                      1   iobj   _   _
6   .       .     PUNCT  _   _                                                                      1   punct  _   _
```

### Exemple 2 : Clitic doubling (source : Fahloune 2020, structure identique)
```conllu
# sent_id = kab-002
# text = Yefka-yas lqahwa i Xira.
1   Yefka   efk    VERB   _   Gender=Masc|Mood=Ind|Number=Sing|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
2   yas     yas    PRON   _   Gender=Fem|Number=Sing|Person=3|Poss=Yes|PronType=Prs                 1   expl   _   _
3   lqahwa  lqahwa NOUN   _   Gender=Fem|Number=Sing|Case=Acc                                       1   obj    _   _
4   i       i      ADP    _   _                                                                      5   case   _   _
5   Xira    Xira   PROPN  _   Gender=Fem|Number=Sing                                                1   iobj   _   _
6   .       .      PUNCT  _   _                                                                      1   punct  _   _
```

### Exemple 3 : Copule (source : ADPT test.conllu l.322, authentique)
```conllu
# sent_id = kab-003
# text = Bab n lbiru-nni d amẓallu.
1   Bab      bab    NOUN   _   Gender=Masc|Number=Sing|Case=Nom   0   root   _   _
2   n        an     ADP    _   _                                   3   case   _   _
3   lbiru    lbiru  NOUN   _   Gender=Masc|Number=Sing|Case=Dat    1   nmod   _   _
4   nni      nni    DET    _   PronType=Dem                        3   det    _   SpaceAfter=No
5   d        d      AUX    _   PartType=Cop                        6   cop    _   _
6   amẓallu  amẓallu NOUN  _   Gender=Masc|Number=Sing|Case=Nom    0   conj   _   _
7   .        .      PUNCT  _   _                                   6   punct  _   _
```

### Exemple 4 : Négation verbale (`ara` négatif)
```conllu
# sent_id = kab-004
# text = Ur yekcem ara w-qcic.
1   ur      ur     ADV    _   Polarity=Neg   2   advmod   _   _
2   yekcem  ekcem  VERB   _   Gender=Masc|Mood=Ind|Number=Sing|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
3   ara     ara    ADV    _   Polarity=Neg   2   advmod   _   _
4   w-qcic  qcic   NOUN   _   Gender=Masc|Number=Sing|Case=Nom   2   nsubj   _   _
5   .       .      PUNCT  _   _   2   punct   _   _
```

### Exemple 5 : Marqueur relatif (perfectif)
```conllu
# sent_id = kab-005
# text = win i d-yekksen.
1   win      win    PRON   _   Gender=Masc|Number=Sing|Person=3|PronType=Dem   4   nsubj   _   _
2   i        i      SCONJ  _   _                                                4   mark    _   _
3   d        d      PART   _   _                                                4   advmod  _   _
4   yekksen  ekkes  VERB   _   Gender=Masc|Mood=Ind|Number=Plur|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
5   .        .      PUNCT  _   _                                                4   punct   _   _
```

### Exemple 6 : Marqueur relatif/clivée (imperfectif)
```conllu
# sent_id = kab-006
# text = D nekk a y-leddi-n tabburt.
1   D        d      AUX    _   PartType=Cop   2   cop    _   _
2   nekk     nekk   PRON   _   Person=1|Number=Sing|PronType=Prs   0   root   _   _
3   a        a      SCONJ  _   _              4   mark   _   _
4   yleddin  leddi  VERB   _   VerbForm=Part  2   acl    _   _
5   tabburt  tabburt NOUN  _   Gender=Fem|Number=Sing|Case=Acc   4   obj   _   _
6   .        .      PUNCT  _   _              2   punct  _   _
```

### Exemple 7 : Préposition unique
```conllu
# sent_id = kab-007
# text = deg inegmirs.
1   deg       deg    ADP    _   _   2   case   _   _
2   inegmirs  inegmirs NOUN _   Gender=Masc|Number=Plur|Case=Acc   0   root   _   _
3   .         .      PUNCT  _   _   2   punct  _   _
```

### Exemple 8 : Extraction des traits verbaux
```conllu
# sent_id = kab-008
# text = Yettawi lqahwa i wemcic.
1   Yettawi   awi     VERB   _   Aspect=Prog|Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
2   lqahwa    lqahwa  NOUN   _   Gender=Fem|Number=Sing|Case=Acc                              1   obj    _   _
3   i         i       ADP    _   _                                                            4   case   _   _
4   wemcic    emcic   NOUN   _   Gender=Masc|Number=Sing|Case=Dat                             1   iobj   _   _
5   .         .       PUNCT  _   _                                                            1   punct  _   _
```

### Exemple 9 : Voix passive
```conllu
# sent_id = kab-009
# text = Yettwaqqen w-qcic.
1   Yettwaqqen   qqen   VERB   _   Aspect=Prog|Gender=Masc|Number=Sing|Person=3|VerbForm=Fin|Voice=Pass   0   root   _   _
2   w-qcic       qcic   NOUN   _   Gender=Masc|Number=Sing|Case=Nom                                        1   nsubj  _   _
3   .            .      PUNCT  _   _                                                                       1   punct  _   _
```

### Exemple 10 : État d'annexion
```conllu
# sent_id = kab-010
# text = Yekcem ɣer wexxam.
1   Yekcem   ekcem   VERB   _   Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
2   ɣer      ɣer     ADP    _   _                                                3   case   _   _
3   wexxam   axxam   NOUN   _   Gender=Masc|Number=Sing|Case=Acc                 1   obl    _   _
4   .        .       PUNCT  _   _                                                1   punct  _   _
```

### Exemple 11 : Emprunt intégré
```conllu
# sent_id = kab-011
# text = Ssiweḍ lqahwa i wemcic.
1   Ssiweḍ    ssiweḍ    VERB   _   Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
2   lqahwa    lqahwa    NOUN   _   Gender=Fem|Number=Sing|Case=Acc                  1   obj    _   _
3   i         i         ADP    _   _                                                4   case   _   _
4   wemcic    emcic     NOUN   _   Gender=Masc|Number=Sing|Case=Dat                 1   iobj   _   _
5   .         .         PUNCT  _   _                                                1   punct  _   _
```

### Exemple 12 : `ara` irréalis en subordonnée temporelle — **[NOUVEAU v0.7]**
```conllu
# sent_id = kab-012
# text = Mi ara yili yisem d unti.
1   Mi      mi      SCONJ  _   _                              3   mark    _   _
2   ara     ara     PART   _   Mood=Irr                        3   advmod  _   _
3   yili    ili     VERB   _   Gender=Masc|Number=Sing|Person=3|VerbForm=Fin   0   root   _   _
4   yisem   isem    NOUN   _   Gender=Masc|Number=Sing|Case=Nom   3   nsubj   _   _
5   d       d       AUX    _   PartType=Cop                    6   cop     _   _
6   unti    tunṭ    NOUN   _   Gender=Fem|Number=Sing|Case=Nom  4   conj    _   _
7   .       .       PUNCT  _   _                                3   punct   _   _
```
*Note : aucune relation de polarité négative dans cette phrase, malgré la présence de `ara` — cf. §5.2.2bis pour la règle de désambiguïsation.*

### Exemple 13 : `ara` marqueur du participe positif de l'aoriste — **[NOUVEAU v0.7]**
```conllu
# sent_id = kab-013
# text = argaz ara yafgen
1   argaz   argaz   NOUN   _   Gender=Masc|Number=Sing|Case=Nom   3   nsubj   _   _
2   ara     ara     PART   _   Mood=Irr                            3   advmod  _   _
3   yafgen  afeg    VERB   _   Gender=Masc|Number=Sing|Person=3|VerbForm=Part   0   root   _   _
```
*Note : « l'homme qui volera » — participe positif de l'aoriste, à mettre en regard du participe négatif correspondant `ur nufig` (« qui ne vole pas »), qui lui reste analysé selon le patron §5.2.2/§6.6.*

---

## 10. Jeu de test obligatoire

| ID | Phrase | Phénomène testé | Statut |
|----|--------|-----------------|--------|
| T01 | `Yekcem w-qcic.` | VSO canonique, sujet post-verbal | Stable |
| T02 | `W-qcic yekcem.` | SVO (topicalisation), sujet pré-verbal | Stable |
| T03 | `Yefka-yas lqahwa i Xira.` | Clitic doubling DAT → expl | **Résolu (Fahloune 2020)** |
| T04 | `D lweḥda.` | Copule → AUX + cop | **Résolu (Mettouchi 2017)** |
| T05 | `Ur yekcem ara w-qcic.` | Négation verbale discontiguë (`ara` négatif) → ADV | Partiel |
| T06 | `win i d-yekksen.` | Marqueur relatif (perfectif) → SCONJ + mark | **Résolu (Achab 2020)** |
| T07 | `D nekk a y-leddi-n tabburt.` | Marqueur relatif/clivée (imperfectif) | À tester |
| T08 | `deg inegmirs.` | Préposition → token unique ADP | Stable |
| T09 | `axxam n w-qcic.` | Possession (état d'annexion) | Stable |
| T10 | `lbiru-ines` | Clitique possessif → segmentation in+as | **Résolu (Achab 2020 + ADPT)** |
| T11 | `Yekcem w-qcic neɣ tekcem teqcict.` | Coordination | Stable |
| T12 | `A nnbi !` | Vocatif | Stable |
| T13 | `Netta, yekcem.` | Dislocation | Stable |
| T14 | `Yettakki w-qcic aɣrum.` | Supplétisme | Stable |
| T15 | `Llan wussan.` | Verbe d'état | Stable |
| T16 | `Mačči d aqdim.` | Négation ascriptive | À spécifier |
| T17 | `Ḥaṭan a Amina.` | Présentatif | À spécifier |
| T18 | `Yettawi lqahwa i wemcic.` | Extraction traits verbaux (Aspect=Prog) | Convention pragmatique, §7.2.1 |
| T19 | `Yettwaqqen w-qcic.` | Voix passive (Voice=Pass) | Convention pragmatique, §7.2.1 |
| T20 | `Yekcem ɣer wexxam.` | État d'annexion | Convention pragmatique, §5.2.5 |
| T21 | `Ssiweḍ lqahwa i wemcic.` | Emprunt intégré | Convention pragmatique, §8.3.1 |
| **T22** | **`Mi ara yili yisem d unti.`** | **`ara` irréalis vs. `ara` négatif — NOUVEAU v0.7** | **Test critique de non-régression : vérifie qu'aucune négation n'est taguée** |
| **T23** | **`argaz ara yafgen`** | **`ara` marqueur du participe positif de l'aoriste — NOUVEAU v0.7** | **À tester ; met en regard avec le participe négatif `ur nufig`** |

---

## 11. Différences corrigées avec UD_Kabyle-ADPT

| Domaine | ADPT (ancien) | v0.5–v0.7 (corrigé) | Source de la correction |
|---------|--------------|----------------|---------------------------|
| Tokenization prépositions | Segmentées (`deg` → `ad`+`ag`, 100% des cas) | Token unique ADP | Convention communautaire UD |
| Copule `d` | PART/ADV + advmod | AUX + cop | Mettouchi (2017) |
| Clitic doubling | obj/iobj/obl | expl | Fahloune (2020) |
| Marqueur relatif `i` | PRON | SCONJ + mark (+ `a` imperfectif/aoriste) | Achab (2020) |
| Particule `ad`/`ur` | PART/ADV incohérents | PART (`ad`), ADV (`ur`) fixés | Fahloune, Baier, Achab (2020) |
| Clitique `ines` | segmenté `in`+`as` | confirmé | Achab (2020) + pratique ADPT |
| Tag `AUX` | Absent | Introduit (copule uniquement) | Mettouchi (2017) |
| Relation `cop` | Absente | Introduite | Mettouchi (2017) |
| Relation `expl` | Absente | Introduite | Fahloune (2020) |
| Négation ascriptive `mačči` | Taguée CCONJ (donnée empirique confirmée v0.7) | Identifiée comme construction à part, UPOS encore ouvert | Mettouchi (2017) |
| Présentatifs | Non traités | Identifiés comme construction à spécifier | Mettouchi (2017) |
| Traits verbaux (Aspect, Voice) | Non extraits systématiquement | Conventions d'extraction ajoutées | Fahloune (2020) + Bouamara (2026) |
| État d'annexion | Mentionné mais non documenté | Règles de formation documentées | Felice (2020) + Mettouchi & Frajzyngier (2013) |
| Emprunts | Non distingués | Distinction intégrés vs non-intégrés | Convention pragmatique, validation empirique |
| **`ara` (négation)** | **Traité comme marqueur unique de négation, jamais distingué de sa fonction irréalis** | **Homographe explicitement distingué : `ara` négatif (§5.2.2) vs. `ara` irréalis/participe (§5.2.2bis) — NOUVEAU v0.7** | **Grammaire de référence (Naït-Zerrad, Mammeri, Chaker) ; validation native (Mokraoui 2026)** |
| Statistiques ADPT | Présentées comme « analyse empirique » sans méthode reproductible documentée | Méthode de vérification via API GitHub documentée et exécutée, résultats confirmés exacts | **NOUVEAU v0.7, §3.1bis** |

---

## 12. Limites connues et feuille de route

| ID | Limite | Statut |
|----|--------|--------|
| L1 | ~~Segmentation de `ines`~~ | **RÉSOLU** — segmentation `in`+`as` confirmée (Achab 2020 + ADPT) |
| L2 | Statut exact du `ara` négatif (ADV vs PART) et de son origine grammaticale ; **et désormais, statut exact du `ara` irréalis (§5.2.2bis)** | **[À VALIDER]** — Mettouchi (2001, 2004) identifiées comme sources probablement décisives sur les deux points (le titre de 2001, « grammaticalisation de ara..., négation et subordination relative », couvre exactement cette distinction) ; texte intégral non encore consulté — **priorité de lecture pour la v0.8** |
| L3 | Statut de `ad` (PART, confiance modérée) | **[À VALIDER — confiance modérée]** |
| L4 | Convention `expl` : vérifier directement sur des treebanks UD roumain/grec/bulgare réels | **[À VALIDER]** — non encore vérifié empiriquement |
| L5 | Alternance `i`/`a` du complémenteur : couvrir systématiquement dans le jeu de test | **[À VALIDER]** |
| L6 | Homographie du `a` (vocatif / complémenteur imperfectif-aoriste / préfixe état libre / **présentatif, §5.2.6**) | **[À VALIDER]** — désormais quatre candidats homographes identifiés, cf. §8.4 |
| L7 | Constructions présentatives (`ha`, `aql`, `a`+suffixe) : UPOS et relations à spécifier entièrement | **[NON SPÉCIFIÉ]** |
| L8 | Négation ascriptive `mačči` : UPOS et relation à spécifier | **[NON SPÉCIFIÉ]** — donnée empirique nouvelle en v0.7 : tagué CCONJ dans ADPT (§5.2.7), à évaluer |
| L9 | Interrogatives : `acḥal` ADV ou PRON ? | **[À VALIDER]** |
| L10 | Participe `VerbForm=Part` dans les clivées à l'imperfectif/aoriste | **[À VALIDER]** |
| L11 | Adjectifs vs noms d'état : confirmer la distinction ADJ/NOUN | **[À VALIDER]** |
| L12 | Voix passive `Voice=Pass` : confirmer l'usage | **[À VALIDER]** |
| L13 | Taille du treebank : étendre à 5 000–10 000 phrases pour validation | Prochaine étape |
| L14 | Bedar, Quellec & Voeltzel (2021) : source écartée pour les questions syntaxiques | Noté, pour éviter une réutilisation erronée |
| L15 | Traits verbaux (Aspect, Voice) : conventions pragmatiques, validation linguistique formelle requise | **[NOUVEAU v0.6]** |
| L16 | État d'annexion : interaction avec la syntaxe (sujet post-verbal optionnel) non entièrement spécifiée | **[NOUVEAU v0.6]** — documentation partielle |
| L17 | Emprunts : distinction intégrés vs non-intégrés est une convention pragmatique | **[NOUVEAU v0.6]** — convention opérationnelle |
| L18 | Articulation exacte de `ara` irréalis avec la copule dans des constructions comme `mi ara yili X d Y` (cf. exemple kab-012) | **[NOUVEAU v0.7]** — non spécifié |
| L19 | Revue systématique de tous les morphèmes monosyllabiques/bisyllabiques de la spec, pour détecter d'éventuels homographes non identifiés (par analogie avec la découverte de `ara`) | **[NOUVEAU v0.7]** — audit méthodologique recommandé avant v0.8 |
| L20 | Audit des 406 occurrences `Polarity=Neg` dans ADPT, pour vérifier qu'aucune ne recouvre un `ara` irréalis mal étiqueté par le pipeline de conversion automatique | **[NOUVEAU v0.7]** — non réalisé |

---

## 13. Implémentation recommandée

### 13.1 Pipeline d'annotation
```
Corpus brut (Tatoeba / Weblate / texte natif)
    ↓
Tokenization morphologique (spec v0.3, à réviser à la lumière de §4)
    ↓
POS tagging + features (Stanza / spaCy / règles)
    ↓
Extraction des traits verbaux (heuristiques §7.2.1)
    ↓
Désambiguïsation du homographe `ara` (règle §5.2.2bis) — **NOUVEAU v0.7,
   étape critique avant toute annotation syntaxique en aval**
    ↓
Annotation syntaxique manuelle (Brat / INCEpTION)
    ↓
Adjudication (locuteur natif + linguiste)
    ↓
Conversion CoNLL-U + validation UD (udapy)
    ↓
Soumission au comité UD
```

### 13.2 Outils
- **Annotation** : INCEpTION ou Brat.
- **Validation** : `udapy` (Popel et al. 2017) pour la validation CoNLL-U.
- **Parsing** : Stanza, Trankit.
- **Extraction des traits verbaux** : heuristiques documentées en §7.2.1.
- **Vérification de dépôts UD** : méthode API GitHub documentée en §3.1bis, réutilisable pour toute vérification future de treebanks tiers (contourne le blocage robots des pages web `/tree/...`).

---

## Références

1. **Achab, Karim** (2003). *Alternation of state in Berber*. In Jacqueline Lecarme (ed.), *Research in Afroasiatic Grammar II*. Amsterdam: John Benjamins.
2. **Achab, Karim** (2020). *Anti-Agreement in Amazigh (Berber) as Genitive Constructions*. McGill Working Papers in Linguistics, 26.1. http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Achab.pdf
3. **Aliane, Lakhdar** (2021). *UD_Kabyle-ADPT*. Universal Dependencies. https://github.com/UniversalDependencies/UD_Kabyle-ADPT
4. **Baier, Nico** (2020). *The Person Case Constraint in Kabyle*. McGill Working Papers in Linguistics, 26.1. http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Baier.pdf
5. **Bedar, Amazigh ; Quellec, Lucie ; Voeltzel, Laurence** (2021). *Epenthetic glides in Taqbaylit*, Journal of African Languages and Literatures 2/2021, pp. 1-29. (Note : source phonologique, non syntaxique — cf. §2.6, §12 L14)
6. **Bouamara, K.** (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle*. HAL.
7. **Chaker, Salem** (1983). *Un parler berbère d'Algérie (Kabyle) : syntaxe*. Université de Provence. **[NOUVEAU v0.7]**
8. **Çöltekin, Çağrı ; et al.** (2021). *Improving the Annotations in the Turkish Universal Dependency Treebank*. RANLP 2021.
9. **de Marneffe, Marie-Catherine ; Manning, Christopher D. ; et al.** (2021). *Universal Dependencies*. Computational Linguistics 47(2), pp. 255-308.
10. **Fahloune, Khokha** (2020). *On the status of subject and object markers in Kabyle: New evidence*. McGill Working Papers in Linguistics, 26.1, pp. 1–17. http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Fahloune.pdf
11. **Felice, Lydia** (2020). *On the Case System of Kabyle*, McGill Working Papers in Linguistics 26.1.
12. **Mammeri, Mouloud** (1976). *Tajerrumt n tmaziɣt (tantala taqbaylit)*. Maspero, Paris. **[NOUVEAU v0.7]**
13. **Mettouchi, Amina** (2001). *La grammaticalisation de ara en kabyle, négation et subordination relative*. Travaux du CerLiCO n°14, pp. 215–235. (texte intégral non consulté — priorité v0.8)
14. **Mettouchi, Amina** (2004). *Les négations non-verbales en kabyle (berbère)*. Verbum XXVI(3), pp. 269–280. (texte intégral non consulté)
15. **Mettouchi, Amina** (2017). *Predication in Kabyle (Berber), KAB*. In Mettouchi, Frajzyngier & Chanard (eds), *Corpus-based cross-linguistic studies on Predication* (CorTypo). http://cortypo.huma-num.fr/Publication
16. **Mettouchi, Amina** (2017). *Relative (Proposition - Syntaxe)*. Encyclopédie berbère vol. XL, pp. 6815–6825.
17. **Mettouchi, Amina & Frajzyngier, Zygmunt** (2013). *A previously unrecognized typological category: The state distinction in Kabyle (Berber)*. Linguistic Typology 17(1), pp. 1–30.
18. **Mokraoui, Athmane (boffire)** (2026). *Spécification du Tokenizer Morphologique pour le Kabyle*, v0.3.
19. **Mokraoui, Athmane (boffire)** (2026). *Conjugueur algorithmique du verbe kabyle*.
20. **Mokraoui, Athmane (boffire)** (2026). *Corpus Tatoeba kabyle nettoyé : validation linguistique et distribution UPOS*.
21. **Naït-Zerrad, Kamal** (2001). *Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit*. Éditions Karthala. **[NOUVEAU v0.7]**
22. **Nivre, Joakim ; et al.** (2020). *Universal Dependencies v2: An annotation scheme for multilingual dependency parsing*. LREC.
23. **Ouhalla, Jamal** (2005). *Clitic placement in Berber*.
24. **Popel, Martin ; et al.** (2017). *Udapi : Universal Dependencies API*.
25. **Taguchi, Chihiro ; et al.** (2022). *UD-Tatar NMCTT Treebank*. UD v2.11.

---

## Annexe : journal des révisions

| Version | Date | Changement principal |
|---|---|---|
| v0.3 | — | Tokenizer morphologique, premières règles de segmentation |
| v0.4 | — | Première tentative d'intégration de la littérature académique (incohérences résiduelles) |
| v0.5 | 08/08/2026 | Vérification empirique des données ADPT ; sourçage systématique ; identification de `mačči` et des présentatifs comme lacunes |
| v0.6 | 17/08/2026 | Ajout des conventions pragmatiques (traits verbaux, état d'annexion, emprunts), validées sur corpus Tatoeba |
| **v0.7** | **17/08/2026** | **Vérification indépendante et reproductible des statistiques ADPT (confirmées exactes, méthode API documentée) ; correction du homographe `ara` (négatif vs. irréalis/participe), identifiée par validation native et confirmée par la grammaire de référence ; mise à jour en cascade de §4, §5, §6, §7, §9, §10, §11, §12** |

*Document révisé v0.7. Chaque décision d'annotation est accompagnée d'une évaluation explicite de son statut (résolu / partiellement sourcé / convention pragmatique / vérifié indépendamment). Les points marqués [À VALIDER] nécessitent une validation native ou une consultation supplémentaire de la littérature avant publication définitive. La lecture de Mettouchi (2001, 2004) en texte intégral est la priorité identifiée pour la v0.8, susceptible de trancher définitivement le statut du homographe `ara` (§5.2.2bis) ainsi que celui de `mačči` (§5.2.7).*
