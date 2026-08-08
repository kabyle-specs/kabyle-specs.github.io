# Spécification Kabyle Universal Dependencies (UD) — v0.5

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; structuration algorithmique et synthèse bibliographique. Révision et vérification des sources : assistée par Claude (Anthropic).

**Date** : 08 août 2026

**Version** : 0.5

**Statut** : Révisé après vérification empirique des données ADPT (branche `dev`) et confrontation aux sources de la littérature berbériste (McGill Working Papers in Linguistics 26.1, 2020 ; CorTypo/Mettouchi 2017). La majorité des points marqués [À VALIDER] en v0.4 sont désormais tranchés et sourcés. De nouveaux points sont apparus au cours de cette vérification (marqueur `mačči`, constructions présentatives) et sont marqués **[NOUVEAU — À VALIDER]**.

**Cible** : Annotateurs de treebanks, développeurs de parseurs de dépendances, chercheurs en linguistique berbère et typologie syntaxique, comité Universal Dependencies.

---

## Résumé

Cette spécification propose une adaptation du framework **Universal Dependencies (UD)** à la langue kabyle (Taqbaylit, ISO 639-3 `kab`). Elle s'inscrit dans la continuité du treebank **UD_Kabyle-ADPT** (Aliane, v2.8, 2021 — données réelles vérifiées sur la branche `dev` du dépôt GitHub officiel, 1930 phrases / 19965 tokens / 23761 mots), tout en corrigeant les incohérences identifiées par l'analyse empirique des données et en ancrant chaque choix d'annotation dans la littérature linguistique disponible sur le kabyle. Le kabyle est une langue afro-asiatique (berbère) à ordre de base **VSO**, fortement pro-drop pour le sujet, avec des clitiques pronominaux objets (datif, accusatif, directionnel) qui doublent ou remplacent les arguments lexicaux — un phénomène de *clitic doubling* démontré empiriquement par Fahloune (2020).

**Mots-clés** : kabyle, taqbaylit, Universal Dependencies, treebank, syntaxe, VSO, clitiques, copule, état d'annexion, clitic doubling, complémenteur.

---

## 1. Introduction

### 1.1 Contexte : le kabyle dans l'écosystème UD

À ce jour, aucune langue berbère n'est représentée dans le catalogue Universal Dependencies de manière active et maintenue. Une première tentative — **UD_Kabyle-ADPT** — a été déposée par Lakhdar Aliane dans le cadre de la release UD v2.8 (2021). Le dépôt officiel (https://github.com/UniversalDependencies/UD_Kabyle-ADPT) contient sur sa branche `master` un squelette de documentation (README/LICENSE/CONTRIBUTING) non rempli, mais les fichiers de données réels (`kab_adpt-ud-train.conllu`, `kab_adpt-ud-test.conllu`, `stats.xml`) se trouvent sur la branche `dev`, activement maintenue par Dan Zeman (UD core team) jusqu'en septembre 2025. Ce treebank n'a cependant pas été inclus dans les releases publiques depuis la v2.9, probablement en raison des incohérences documentées ci-dessous (§3).

### 1.2 Pourquoi une spec formelle ?

Les langues à morphologie riche et à clitiques pronominaux (turc, arabe, grec, bulgare) ont montré dans l'écosystème UD que la qualité d'un treebank dépend crucialement de la documentation préalable des choix d'annotation (Çöltekin et al. 2021 pour le turc ; Taguchi et al. 2022 pour le tatar). Sans guidelines spécifiques, les annotateurs divergent sur des points fondamentaux : statut de la copule, traitement du clitic doubling, segmentation des morphèmes. Cette spec se propose de combler ce vide pour le kabyle, en s'appuyant systématiquement sur la littérature linguistique disponible plutôt que sur l'intuition seule.

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

### 2.2 Treebank antérieur
**Aliane, Lakhdar** (2021). *UD_Kabyle-ADPT*. Universal Dependencies v2.8. Dépôt : https://github.com/UniversalDependencies/UD_Kabyle-ADPT (données sur la branche `dev`).

**Note d'analyse empirique** (vérifiée directement sur les fichiers `.conllu` et `stats.xml`, branche `dev`, 1930 phrases / 19965 tokens / 23761 mots / 3241 fusions) :
- Absence totale de `AUX`, `cop`, `expl` dans les données (0 occurrence).
- Copule `d` : annotée `PART advmod` (440 occ.) ou `ADV advmod` (224 occ.), jamais `cop`.
- Préposition `deg` : segmentée systématiquement (100 % des 171 occurrences) en MWT `ad` (PART) + `ag` (ADP) — sur-segmentation à corriger.
- Clitique possessif `ines` : segmenté systématiquement (100 % des occurrences vérifiées) en `in` (ADP, case) + `as` (PRON, nmod).
- Marqueur relatif `i` : majoritairement `PRON` dans diverses relations (nsubj, obl, obj, mark, acl, iobj...), avec une seule occurrence `SCONJ mark` sur l'ensemble du corpus.

### 2.3 Syntaxe et morphosyntaxe berbère (vérifiées, texte intégral consulté)
**Fahloune, Khokha** (2020). *On the status of subject and object markers in Kabyle: New evidence*. McGill Working Papers in Linguistics, 26.1, pp. 1–17. Accès libre : http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Fahloune.pdf
> Démontre par plusieurs diagnostics indépendants (invariance aspectuelle, granularité des traits phi, Person-Case-Constraint, empilement de clitiques) que les marqueurs sujet en kabyle sont un véritable accord verbal, tandis que les marqueurs objet sont des instances de *clitic doubling*. L'exemple d'empilement de clitiques donné (« y-fka-as-tt wqcic tktuvt-nni i Ales », §4.2.1, ex. 19) est structurellement identique aux exemples de la présente spec (§6.3.2).

**Achab, Karim** (2020). *Anti-Agreement in Amazigh (Berber) as Genitive Constructions*. McGill Working Papers in Linguistics, 26.1. Accès libre : http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Achab.pdf
> Analyse le complémenteur `i` (glosé COMP) comme l'élément commun aux constructions relatives, clivées et génitives-possessives en kabyle. Établit l'alternance `i` (perfectif) / `a` (imperfectif, aoriste) comme complémenteurs conditionnés par l'aspect (§ « In Kabyle, the complementizer can be i, if the aspect involved corresponds to perfective... but it can also be a, if the aspect corresponds to the imperfective... or the aorist »). Analyse les formes possessives longues (ex. `axxam-i-n-u`, « ma maison ») comme composées du complémenteur `i` + particule génitive `n` + suffixe personnel.

**Baier, Nico** (2020). *The Person Case Constraint in Kabyle*. McGill Working Papers in Linguistics, 26.1. Accès libre : http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Baier.pdf
> Confirme l'analyse de `ad` (futur), `ur` (négation) et `i` (complémenteur A-bar) comme des « preverbal particles » du kabyle. Analyse le clitic doubling en détail pour les constructions ditransitives.

**Felice, Lydia** (2020). *On the Case System of Kabyle*, McGill Working Papers in Linguistics 26.1. Analyse le kabyle comme langue à nominatif marqué : état d'annexion (CS) = nominatif (sujet), état libre (FS) = accusatif (objet).

**Ouhalla, Jamal** (2005). *Clitic placement in Berber*. Les clitiques obéissent à la loi de la seconde position et peuvent s'attacher au verbe (V-CL) ou à une catégorie fonctionnelle (F-CL V), notamment `ad`, `ur`.

### 2.4 Prédication et copule (vérifiée, texte intégral consulté)
**Mettouchi, Amina** (2017). *Predication in Kabyle (Berber), KAB*. In Mettouchi, Frajzyngier & Chanard (eds), *Corpus-based cross-linguistic studies on Predication* (CorTypo). http://cortypo.huma-num.fr/Publication
> Source déterminante pour le statut de `d`. Section 1.3 : « Kabyle has a number of non-verbal predicates: **a non-verbal copula ('d')** followed by a nominal, a negative existential predicate... ». La copule `d` est glosée systématiquement **COP** / **PRED** dans toutes les prédications ascriptives (affirmatives et négatives), et est catégoriquement **distincte** des constructions présentatives (`ha`, `aql`, `a`+suffixe distal/proximal), qui ne font jamais intervenir `d`. La négation ascriptive utilise un marqueur dédié, `mačči` (glosé NEG.ATTR), distinct de la négation verbale `ur...ara`.

**Mettouchi, Amina & Frajzyngier, Zygmunt** (2013). *A previously unrecognized typological category: The state distinction in Kabyle (Berber)*. Linguistic Typology 17(1), pp. 1–30.

### 2.5 Négation verbale (référence trouvée, texte non encore consulté — cf. §12 L2)
**Mettouchi, Amina** (2001). *La grammaticalisation de ara en kabyle, négation et subordination relative*. Travaux du CerLiCO n°14, pp. 215–235.
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

---

## 3. État de l'art et positionnement

### 3.1 UD_Kabyle-ADPT : données empiriques vérifiées

D'après l'analyse directe des fichiers ADPT (branche `dev`, stats.xml et fichiers `.conllu`) :
- **Taille** : 1930 phrases, 19965 tokens, 23761 mots syntaxiques, 3241 fusions.
- **Tags UPOS utilisés** (15) : ADJ (162), ADP (3148), ADV (1829), CCONJ (131), DET (67), INTJ (81), NOUN (4428), NUM (111), PART (2104), PRON (3394), PROPN (192), PUNCT (3949), SCONJ (142), VERB (3937), X (86).
- **Aucun tag `AUX`** n'est présent dans les données.
- **Relations utilisées** (28) : acl, acl:relcl, advcl, advmod, amod, appos, case, cc, ccomp, compound, conj, dep, det, discourse, dislocated, iobj, mark, nmod, nsubj, nummod, obj, obl, obl:arg, parataxis, punct, root, vocative, xcomp.
- **Aucune relation `cop` ni `expl`** n'est présente.

### 3.2 Divergences identifiées, sourcées et corrigées

| Divergence | Données ADPT | Correction v0.5 | Source de la correction |
|-----------|-------------|-----------------|--------------------------|
| Copule `d` | PART/ADV + advmod | **AUX + cop** | Mettouchi (2017) : « non-verbal copula » distincte du présentatif |
| Clitic doubling | obj/iobj/obl pour les clitiques | **expl** pour les clitiques redondants | Fahloune (2020) : démonstration du clitic doubling |
| Marqueur relatif `i` | PRON | **SCONJ + mark** (avec alternance `i`/`a` selon l'aspect) | Achab (2020) : `i`/`a` glosés COMP |
| Prépositions composées | Segmentées (ex : `deg` → `ad`+`ag`) | **Token unique ADP** | Recommandation communautaire UD (pas de source Kabyle spécifique) |
| Clitique possessif `ines` | Segmenté `in`+`as` | **Confirmé** : segmentation `in` (ADP) + `as` (PRON) | Convergence Achab (2020, analyse théorique) + pratique ADPT |
| Tag `AUX` | Absent | Introduit, réservé à la copule `d` | Mettouchi (2017) |
| Particules `ad`/`ur` | — | **PART** (pas AUX) | Convergence Fahloune, Baier, Achab (2020) : les trois emploient systématiquement *particle* |
| Négation ascriptive `mačči` | Non distinguée de `ur`/`ara` dans la v0.4 | **[NOUVEAU]** Marqueur distinct à ajouter | Mettouchi (2017) : NEG.ATTR, construction propre |
| Présentatifs (`ha`, `aql`, `a`+suffixe) | Non traités | **[NOUVEAU]** Construction non couverte, à spécifier | Mettouchi (2017) : constructions formellement distinctes de la copule |

### 3.3 Positionnement de cette spec

Cette spécification (v0.5) se distingue par :
1. **Une vérification empirique directe** des données ADPT (fichiers `.conllu` réels, pas seulement les métadonnées du dépôt).
2. **Un sourçage systématique** de chaque décision d'annotation contestée, avec citation du texte exact et évaluation de la pertinence de la source (voir §2.6 pour un exemple de source écartée après vérification).
3. **Une identification de lacunes nouvelles** (marqueur `mačči`, constructions présentatives) découvertes en creusant la littérature, plutôt que simplement corriger les points déjà identifiés.
4. **Une transparence sur les limites** : les points encore non sourcés sont clairement distingués de ceux qui le sont (voir §12).

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

**Décision (résout l'ancien point [À VALIDER] L1 de la v0.4)** : `ines` est segmenté en MWT `in` (ADP, relation `case`) + `as` (PRON, relation `nmod`), conformément à la pratique ADPT (100 % des occurrences vérifiées) et à l'analyse théorique d'Achab (2020), qui montre que les formes possessives longues du kabyle sont compositionnelles (complémenteur-relateur + génitif + personne), et non des pronoms possessifs synthétiques non analysables.

**Exemple corrigé** :
```
3   lbiru   lbiru   NOUN   _   Gender=Masc|Number=Sing              8   obl   _   _
4-5 ines    _       _      _   _                                    _   _     _   _
4   in      in      ADP    _   _                                    5   case  _   _
5   as      netta   PRON   _   Case=Acc|Number=Sing|Person=3|Poss=Yes|PronType=Rel  3  nmod  _  _
```

#### 4.2.3 Clitiques verbaux (objets)

Les clitiques objets verbaux (`-t`, `-tt`, `-as`, `-asent`, `-ten`, `-tent`, `-iyi`, `-ak`, `-am`, `-aɣ`, `-awen`) sont **segmentés** en tokens PRON séparés — analyse confirmée par Fahloune (2020) : ce sont des clitiques doublés (*doubled clitics*), pas des affixes d'accord.

| Forme graphique | Tokens | UPOS | Notes |
|----------------|--------|------|-------|
| `yefka-yas` | `yefka` + `yas` | VERB + PRON | Datif 3SG |
| `yefka-t` | `yefka` + `t` | VERB + PRON | Accusatif 3SG.M |
| `yefka-tt` | `yefka` + `tt` | VERB + PRON | Accusatif 3SG.F |

#### 4.2.4 Clitiques directionnels et adverbiaux

Les clitiques directionnels (`-d`, `-n`) sont segmentés en tokens PART séparés — attestés chez Fahloune (2020, ex. 21) qui documente jusqu'à trois clitiques empilés (DAT-ACC-DIR) sur un même verbe.

| Forme graphique | Tokens | UPOS | Notes |
|----------------|--------|------|-------|
| `yefka-d` | `yefka` + `d` | VERB + PART | Directionnel « vers le locuteur » |

#### 4.2.5 Affixes d'accord sujet

Les affixes d'accord sujet (préfixes `y-`, `i-`, `t-`, `n-` ; suffixes `-eɣ`, `-eḍ`, `-en`, `-ent`, `-em`, `-emt`) restent **fusionnés** avec le verbe en un seul token VERB. Décision confirmée par Fahloune (2020) : contrairement aux marqueurs objet, les marqueurs sujet sont un véritable accord verbal (obligatoire, sensible à l'aspect pour les verbes de qualité), pas des clitiques — leur fusion au radical verbal est donc justifiée linguistiquement, pas seulement pratiquement.

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
| Négation ascriptive `mačči` | 1 token | `mačči` — **[NOUVEAU, À VALIDER]** |
| Copule `d` | 1 token AUX | `d` |
| Présentatifs `ha`/`aql`/`a`+suffixe | non spécifié | **[NOUVEAU, À VALIDER]** — cf. §5.2.6 |
| Coordination `neɣ` | 1 token CCONJ | `neɣ` |

---

## 5. POS tags (UPOS)

### 5.1 Inventaire UPOS utilisé

| UPOS | Usage kabyle | Exemples | Fréquence ADPT |
|------|--------------|----------|-------------------|
| `VERB` | Verbes de base et dérivés | `yekcem`, `yelli`, `yenna`, `eddu` | 3937 |
| `AUX` | Copule `d` uniquement | `d` (copule ascriptive) | Introduit — 0 dans ADPT |
| `PART` | Particules TAM (`ad`, `ur`), directionnelles (`d` directionnel), clitiques verbaux non-pronominaux | `ad`, `ur`, `agi` | 2104 |
| `NOUN` | Noms communs | `ass`, `iman`, `abrid`, `awal` | 4428 |
| `PROPN` | Noms propres | `Ṭiṭem`, `azwaw`, `wejda` | 192 |
| `PRON` | Pronoms indépendants et clitiques | `netta`, `win`, `ayen` | 3394 |
| `DET` | Déterminants | `yal`, `le` | 67 |
| `ADJ` | Adjectifs qualificatifs | `anezmar`, `amenzu`, `amellal` | 162 |
| `NUM` | Numéraux | `yiwen`, `yiwet`, `sin`, `snat` | 111 |
| `ADV` | Adverbes | `kan`, `mi`, `tura` | 1829 |
| `ADP` | Adpositions | `i` (datif), `n`, `ag`, `ɣef`, `ɣer` | 3148 |
| `CCONJ` | Conjonctions de coordination | `neɣ`, `maca`, `acku` | 131 |
| `SCONJ` | Complémenteurs relatifs/clivées | `i` (perfectif), `a` (imperfectif/aoriste), `ma`, `imi`, `lemmer` | 142 |
| `INTJ` | Interjections | `ah`, `ay`, `ih` | 81 |
| `PUNCT` | Ponctuation | `.`, `?`, `!`, `,`, `;`, `:`, `«`, `»` | 3949 |
| `X` | Éléments non analysés | `_` | 86 |

**Correction apportée en v0.5** : la particule `ad` n'apparaît plus sous trois catégories différentes (DET/ADV/PART, incohérence de la v0.4) — elle est fixée sous **PART** uniquement, conformément à la convergence de trois sources (Fahloune, Baier, Achab 2020) qui emploient systématiquement le terme *particle*. La ligne DET « ad » de la v0.4 était une erreur de recopie et a été supprimée.

### 5.2 Cas particuliers

#### 5.2.1 La particule `d` (copule ascriptive) — RÉSOLU

**Décision, sourcée** : `d` est annoté **AUX** avec la relation **`cop`**. Cette décision s'appuie directement sur Mettouchi (2017), qui décrit `d` comme *« a non-verbal copula »* et la glose systématiquement **COP/PRED** dans toutes les prédications ascriptives (affirmatives et négatives) de son corpus annoté CorTypo.

- **Ascriptive affirmative** : `d` = AUX, relation `cop`. Ex. : « *wagi d lḥağ ṭaḥaṛ* » (« celui-ci est Hadj Tahar »).
- **Ascriptive dans les clivées** : même analyse. Ex. : « *D nekk i y-ldi-n tabburt* » (« C'est moi qui ai ouvert la porte »), cf. aussi Achab (2020).

**Clarification majeure apportée par cette révision** : le dilemme « copule vs présentatif » soulevé dans la v0.3 (§5.2.1) reposait sur une fausse alternative. Mettouchi (2017) montre que la copule ascriptive `d` et les constructions **présentatives** (§5.2.6, ci-dessous) sont deux constructions grammaticales **distinctes**, marquées par des morphèmes différents (`d` pour l'une, `ha`/`aql`/`a`+suffixe pour l'autre). Il n'y a donc pas d'ambiguïté d'analyse pour `d` lui-même — il est toujours copule ascriptive.

**Exemple corrigé** :
```
5   d   d   AUX   _   PartType=Cop   6   cop   _   _
6   amẓallu   amẓallu   NOUN   _   Gender=Masc|Number=Sing|Case=Nom   0   root   _   SpaceAfter=No
```
(Phrase authentique du corpus ADPT test, ligne 322 : « *Bab n lbiru-nni d amẓallu, d ineslem.* »)

#### 5.2.2 La particule `ur` et le marqueur `ara` (négation verbale)

**Décision** : `ur` et `ara` sont annotés **ADV** avec `Polarity=Neg` et la relation `advmod`, conformément aux données ADPT.

**Statut de la source** : partiellement sourcé. Les trois articles McGill WP 26.1 (Fahloune, Baier, Achab 2020) désignent `ur` comme une *particule*, ce qui exclut AUX mais ne tranche pas entre ADV et PART. Mettouchi a des travaux dédiés à `ara` (2001, 2004) qui n'ont pas encore été consultés en texte intégral — cf. §12, L2.

```
1   ur   ur   ADV   _   Polarity=Neg   3   advmod   _   _
2   ǧin  eǧǧ  VERB  _   _              3   advmod   _   _
3   isugen esug VERB _   _             0   root     _   _
```

#### 5.2.3 Le complémenteur relatif/clivée `i` / `a` — RÉSOLU, avec nuance ajoutée

**Décision, sourcée** : le complémenteur relatif et clivé est annoté **SCONJ** avec la relation **`mark`**. Cette décision s'appuie sur Achab (2020), qui glose systématiquement cet élément **COMP** dans les questions-WH, relatives et clivées.

**Nuance ajoutée en v0.5, absente de la v0.4** : le complémenteur **alterne selon l'aspect** :
- `i` au perfectif : « *Anta i y-ldi-n tawwurt?* » (« Qui a ouvert la porte ? »)
- `a` à l'imperfectif ou à l'aoriste : « *D nekk a y-leddi-n tabburt* » (« C'est moi qui ouvre la porte »)

Il faut donc traiter `a` comme second membre du même paradigme SCONJ, en plus de `i`. **[À VALIDER]** : confirmer que `a` (SCONJ) est bien distinct de l'homographe `a-` (préfixe d'état libre nominal, non segmenté, cf. §5.2.5) et de l'`a` vocatif (§8.4 des versions précédentes).

**Distinction cruciale avec le `i` prépositionnel** : le `i` complémenteur (SCONJ) est un morphème distinct du `i` préposition datif (« à/pour »), qui reste ADP. Les deux morphèmes sont homographes mais fonctionnellement indépendants — l'ADPT ne les distingue pas systématiquement (113+ occurrences de `i` restent taguées PRON dans des fonctions variées), ce qui explique la faible fréquence de SCONJ pour `i` dans les données actuelles (1 seule occurrence).

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

**Statut de la source** : trois articles indépendants (Fahloune, Baier, Achab, McGill WP 26.1 2020) désignent systématiquement `ad` comme *particle*, jamais *auxiliary*. C'est un faisceau convergent, mais aucun des trois n'argumente explicitement pour exclure une analyse AUX — le choix PART repose donc sur l'usage terminologique constant de la littérature plutôt que sur une démonstration dédiée. **[À VALIDER — confiance modérée]** : l'intuition du locuteur natif sur le comportement syntaxique de `ad` (proche du verbe comme un auxiliaire modal, ou clairement invariable comme une particule) reste la pièce manquante pour clore ce point avec certitude.

```
1   ad   ad   PART   _   _   3   advmod   _   _
2   ad   ad   PART   _   _   3   advmod   _   _
3   nbeddel   ebeddel   VERB   _   _   0   root   _   _
```

#### 5.2.5 État libre (FS) vs État d'annexion (CS)

Les préfixes nominaux (`a-`, `ta-`, `i-`, `ti-`, `w-`, `t-`, `y-`) ne sont pas segmentés en tokens séparés. Leur statut FS/CS est encodé dans les features morphologiques (`Case=Nom|Acc|Dat`), conformément aux données ADPT et à l'analyse de Felice (2020).

#### 5.2.6 Constructions présentatives — **[NOUVEAU, NON SPÉCIFIÉ]**

Découverte au cours de cette révision (Mettouchi 2017) : le kabyle distingue la copule ascriptive `d` (§5.2.1) de constructions **présentatives** dédiées, jamais marquées par `d` :

| Forme | Personne | Construction |
|-------|----------|--------------|
| `ha` + pronom absolutif clitique | 3e personne (sg/pl) | Peut être suivi d'un NP à l'état d'annexion |
| `a` + pronom absolutif + `-an`/`-ad` | 3e personne (sg/pl) | `-an` distal, `-ad` proximal |
| `aql` + pronom absolutif clitique | 1re/2e personne (sg/pl) | Peut être suivi d'un NP à l'état d'annexion |

Exemple (Mettouchi 2017) : « *ḥaṭan a Amina* » (« La voici, Amina »/« Here it was, Amina »).

**Cette construction n'est traitée nulle part dans les versions précédentes de la spec (v0.3, v0.4).** Elle mérite son propre sous-ensemble de règles d'annotation (UPOS pour `ha`/`aql`/`a`+suffixe — probablement PART ou VERB selon le degré de grammaticalisation — et relation, `root` ou construction dédiée). **[À VALIDER]** : ce point nécessite un travail de spécification à part entière, hors du périmètre de cette révision.

#### 5.2.7 La négation ascriptive `mačči` — **[NOUVEAU, NON SPÉCIFIÉ]**

Découverte au cours de cette révision (Mettouchi 2017) : la négation des prédications ascriptives (copule `d`) ne se fait **pas** avec `ur...ara`, mais avec un marqueur dédié `mačči` (glosé NEG.ATTR), qui précède la copule.

Exemple (Mettouchi 2017) : « *ma d aqdim nəɣ mačči d aqdim* » (« qu'il soit vieux ou non »).

**[À VALIDER]** : tag UPOS de `mačči` (PART par analogie avec `ur`, ou ADV) et relation (`advmod` ou relation dédiée à la négation ascriptive, en cohérence avec le traitement de `cop`).

---

## 6. Relations de dépendances

### 6.1 Inventaire des relations utilisées (ADPT, vérifié)

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

**Décision, sourcée** : conformément à Fahloune (2020), qui démontre que les marqueurs objet du kabyle sont des instances de *clitic doubling* (invariance aspectuelle, empilement multiple, Person-Case-Constraint, granularité des traits phi), l'argument lexical reçoit la relation sémantique (`obj`, `iobj`), et le clitique co-occurrent est annoté **`expl`**.

L'exemple ci-dessous est structurellement identique à l'exemple (19) de Fahloune (2020) : « *y-fka-as-tt wqcic tktuvt-nni i Ales* » (« il le lui a donné, le livre, à Ales »).

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

**Résidu ouvert** : Fahloune établit le statut linguistique (*clitic doubling*), pas le choix de la relation UD `expl` elle-même — ce choix reste emprunté aux conventions communautaires (grec, bulgare, roumain) sans vérification directe des treebanks correspondants (cf. §12, L3).

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

### 6.6 Négation discontiguë `ur ... ara`
```
Ur yekcem ara w-qcic .
advmod(yekcem, ur)
advmod(yekcem, ara)
nsubj(yekcem, w-qcic)
```

### 6.7 Négation ascriptive `mačči` — **[NOUVEAU, À VALIDER]**
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
| `Mood` | `Imp`, `Ind` | Imp: 99, Ind: 3366 | Mode verbal |
| `Number` | `Plur`, `Sing` | Plur: 2797, Sing: 7947 | Nombre |
| `Person` | `1`, `2`, `3` | 1: 374, 2: 302, 3: 5256 | Personne |
| `Polarity` | `Neg` | 406 | Négation verbale |
| `Poss` | `Yes` | 1434 | Possessif |
| `PronType` | `Art`, `Dem`, `Int`, `Rel` | Art: 15, Dem: 485, Int: 370, Rel: 2414 | Type de pronom |
| `Tense` | `Fut`, `Past`, `Pres` | Fut: 328, Past: 2484, Pres: 1105 | Temps |
| `VerbForm` | `Fin`, `Part` | Fin: 3438, Part: 474 | Forme verbale |
| `Voice` | `Pass` | 46 | Voix passive |

### 7.2 Verbes

| Feature | Valeurs | Description |
|---------|---------|-------------|
| `Tense` | `Past`, `Pres`, `Fut` | Passé, présent, futur |
| `Mood` | `Ind`, `Imp` | Indicatif, impératif |
| `VerbForm` | `Fin`, `Part` | Forme finie, participe |
| `Voice` | `Pass` | Voix passive |
| `Polarity` | `Neg` | Négation |
| `Person` / `Number` / `Gender` | — | Accord (véritable accord, cf. Fahloune 2020) |

### 7.3 Noms

| Feature | Valeurs | Description |
|---------|---------|-------------|
| `Gender` | `Masc`, `Fem` | Genre |
| `Number` | `Sing`, `Plur` | Nombre |
| `Case` | `Nom`, `Acc`, `Dat` | Cas (encode FS/CS) |
| `Definite` | `Ind` | Défini/indéfini |

### 7.4 Pronoms et clitiques

| Feature | Valeurs | Description |
|---------|---------|-------------|
| `PronType` | `Prs`, `Dem`, `Rel`, `Int`, `Art` | Personnel, démonstratif, relatif, interrogatif, article |
| `Person` / `Number` / `Gender` | — | — |
| `Poss` | `Yes` | Possessif |
| `Case` | `Nom`, `Acc`, `Dat` | Cas |

### 7.5 Particules

| Feature | Valeurs | Description |
|---------|---------|-------------|
| `Polarity` | `Neg` | Négation verbale (`ur`) |
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
Les emprunts français/arabe non intégrés morphologiquement sont taggés `X`.

### 8.4 Vocatif
```
A nnbi !
vocative(nnbi, a)
root(nnbi)
```
**Point de vigilance ajouté en v0.5** : le `a` vocatif est à distinguer du `a` complémenteur imperfectif/aoriste (§5.2.3) et du `a-` préfixe d'état libre nominal (§5.2.5) — trois morphèmes homographes distincts.

### 8.5 Éléments disloqués
```
Netta, yekcem .
dislocated(yekcem, netta)
root(yekcem)
```

---

## 9. Exemples CoNLL-U complets (mis à jour v0.5)

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

### Exemple 4 : Négation verbale
```conllu
# sent_id = kab-004
# text = Ur yekcem ara w-qcic.
1   ur      ur     ADV    _   Polarity=Neg   3   advmod   _   _
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

### Exemple 6 : Marqueur relatif/clivée (imperfectif — NOUVEAU en v0.5)
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
*(Analyse provisoire pour la forme participiale y-leddi-n dans une clivée à l'imperfectif — cf. §12, L7.)*

### Exemple 7 : Préposition unique
```conllu
# sent_id = kab-007
# text = deg inegmirs.
1   deg       deg    ADP    _   _   2   case   _   _
2   inegmirs  inegmirs NOUN _   Gender=Masc|Number=Plur|Case=Acc   0   root   _   _
3   .         .      PUNCT  _   _   2   punct  _   _
```

---

## 10. Jeu de test obligatoire

| ID | Phrase | Phénomène testé | Statut |
|----|--------|-----------------|--------|
| T01 | `Yekcem w-qcic.` | VSO canonique, sujet post-verbal | Stable |
| T02 | `W-qcic yekcem.` | SVO (topicalisation), sujet pré-verbal | Stable |
| T03 | `Yefka-yas lqahwa i Xira.` | Clitic doubling DAT → expl | **Résolu (Fahloune 2020)** |
| T04 | `D lweḥda.` | Copule → AUX + cop | **Résolu (Mettouchi 2017)** |
| T05 | `Ur yekcem ara w-qcic.` | Négation verbale discontiguë → ADV | Partiel |
| T06 | `win i d-yekksen.` | Marqueur relatif (perfectif) → SCONJ + mark | **Résolu (Achab 2020)** |
| T07 | `D nekk a y-leddi-n tabburt.` | Marqueur relatif/clivée (imperfectif) — **NOUVEAU** | À tester |
| T08 | `deg inegmirs.` | Préposition → token unique ADP | Stable |
| T09 | `axxam n w-qcic.` | Possession (état d'annexion) | Stable |
| T10 | `lbiru-ines` | Clitique possessif → segmentation in+as | **Résolu (Achab 2020 + ADPT)** |
| T11 | `Yekcem w-qcic neɣ tekcem teqcict.` | Coordination | Stable |
| T12 | `A nnbi !` | Vocatif | Stable |
| T13 | `Netta, yekcem.` | Dislocation | Stable |
| T14 | `Yettakki w-qcic aɣrum.` | Supplétisme | Stable |
| T15 | `Llan wussan.` | Verbe d'état | Stable |
| T16 | `Mačči d aqdim.` | Négation ascriptive — **NOUVEAU** | À spécifier |
| T17 | `Ḥaṭan a Amina.` | Présentatif — **NOUVEAU** | À spécifier |

---

## 11. Différences corrigées avec UD_Kabyle-ADPT

| Domaine | ADPT (ancien) | v0.5 (corrigé) | Source de la correction |
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
| Négation ascriptive `mačči` | Non distinguée | Identifiée comme construction à part | Mettouchi (2017) |
| Présentatifs | Non traités | Identifiés comme construction à spécifier | Mettouchi (2017) |

---

## 12. Limites connues et feuille de route

| ID | Limite | Statut |
|----|--------|--------|
| L1 | ~~Segmentation de `ines`~~ | **RÉSOLU** — segmentation `in`+`as` confirmée (Achab 2020 + ADPT) |
| L2 | Statut exact de `ara` (ADV vs PART) et son origine grammaticale | **[À VALIDER]** — sources identifiées (Mettouchi 2001, 2004) mais texte intégral non consulté |
| L3 | Statut de `ad` (PART, confiance modérée — faisceau convergent mais pas de démonstration dédiée) | **[À VALIDER — confiance modérée]** |
| L4 | Convention `expl` : vérifier directement sur des treebanks UD roumain/grec/bulgare réels que c'est bien l'usage établi pour le clitic doubling | **[À VALIDER]** — non encore vérifié empiriquement |
| L5 | Alternance `i`/`a` du complémenteur : couvrir systématiquement dans le jeu de test et les guidelines | **[À VALIDER]** — nouvellement identifié |
| L6 | Homographie du `a` (vocatif / complémenteur imperfectif-aoriste / préfixe état libre) | **[À VALIDER]** — nouvellement identifié |
| L7 | Constructions présentatives (`ha`, `aql`, `a`+suffixe) : UPOS et relations à spécifier entièrement | **[NOUVEAU — non spécifié]** |
| L8 | Négation ascriptive `mačči` : UPOS et relation à spécifier | **[NOUVEAU — non spécifié]** |
| L9 | Interrogatives : `acḥal` ADV ou PRON ? | **[À VALIDER]** — non traité dans cette révision |
| L10 | Participe `VerbForm=Part` dans les clivées à l'imperfectif/aoriste (cf. Exemple 6, §9) | **[À VALIDER]** |
| L11 | Adjectifs vs noms d'état : confirmer la distinction ADJ/NOUN | **[À VALIDER]** — non traité dans cette révision |
| L12 | Voix passive `Voice=Pass` : confirmer l'usage | **[À VALIDER]** — non traité dans cette révision |
| L13 | Taille du treebank : étendre à 5 000–10 000 phrases pour validation | Prochaine étape |
| L14 | Bedar, Quellec & Voeltzel (2021) : source écartée pour les questions syntaxiques (traite d'épenthèse phonologique, pas de statut grammatical) — reste utile uniquement pour les règles de segmentation phonologique des frontières | Noté, pour éviter une réutilisation erronée |

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

---

## Références

1. **Achab, Karim** (2003). *Alternation of state in Berber*. In Jacqueline Lecarme (ed.), *Research in Afroasiatic Grammar II*. Amsterdam: John Benjamins.
2. **Achab, Karim** (2020). *Anti-Agreement in Amazigh (Berber) as Genitive Constructions*. McGill Working Papers in Linguistics, 26.1. http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Achab.pdf
3. **Aliane, Lakhdar** (2021). *UD_Kabyle-ADPT*. Universal Dependencies v2.8. https://github.com/UniversalDependencies/UD_Kabyle-ADPT
4. **Baier, Nico** (2020). *The Person Case Constraint in Kabyle*. McGill Working Papers in Linguistics, 26.1. http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Baier.pdf
5. **Bedar, Amazigh ; Quellec, Lucie ; Voeltzel, Laurence** (2021). *Epenthetic glides in Taqbaylit*, Journal of African Languages and Literatures 2/2021, pp. 1-29. (Note : source phonologique, non syntaxique — cf. §2.6, §12 L14)
6. **Bouamara, K.** (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle*. HAL.
7. **Çöltekin, Çağrı ; et al.** (2021). *Improving the Annotations in the Turkish Universal Dependency Treebank*. RANLP 2021.
8. **de Marneffe, Marie-Catherine ; Manning, Christopher D. ; et al.** (2021). *Universal Dependencies*. Computational Linguistics 47(2), pp. 255-308.
9. **Fahloune, Khokha** (2020). *On the status of subject and object markers in Kabyle: New evidence*. McGill Working Papers in Linguistics, 26.1, pp. 1–17. http://people.linguistics.mcgill.ca/~mcgwpl/McGWPL/2020v26n01/2020_26_1_Fahloune.pdf
10. **Felice, Lydia** (2020). *On the Case System of Kabyle*, McGill Working Papers in Linguistics 26.1.
11. **Mettouchi, Amina** (2001). *La grammaticalisation de ara en kabyle, négation et subordination relative*. Travaux du CerLiCO n°14, pp. 215–235. (texte intégral non consulté)
12. **Mettouchi, Amina** (2004). *Les négations non-verbales en kabyle (berbère)*. Verbum XXVI(3), pp. 269–280. (texte intégral non consulté)
13. **Mettouchi, Amina** (2017). *Predication in Kabyle (Berber), KAB*. In Mettouchi, Frajzyngier & Chanard (eds), *Corpus-based cross-linguistic studies on Predication* (CorTypo). http://cortypo.huma-num.fr/Publication
14. **Mettouchi, Amina** (2017). *Relative (Proposition - Syntaxe)*. Encyclopédie berbère vol. XL, pp. 6815–6825.
15. **Mettouchi, Amina & Frajzyngier, Zygmunt** (2013). *A previously unrecognized typological category: The state distinction in Kabyle (Berber)*. Linguistic Typology 17(1), pp. 1–30.
16. **Mokraoui, Athmane (boffire)** (2026). *Spécification du Tokenizer Morphologique pour le Kabyle*, v0.3.
17. **Mokraoui, Athmane (boffire)** (2026). *Conjugueur algorithmique du verbe kabyle*.
18. **Nivre, Joakim ; et al.** (2020). *Universal Dependencies v2: An annotation scheme for multilingual dependency parsing*. LREC.
19. **Ouhalla, Jamal** (2005). *Clitic placement in Berber*.
20. **Taguchi, Chihiro ; et al.** (2022). *UD-Tatar NMCTT Treebank*. UD v2.11.

---

*Document révisé v0.5 après vérification empirique directe des données ADPT (branche `dev` du dépôt GitHub officiel) et consultation en texte intégral de cinq sources de la littérature berbériste (Fahloune 2020, Achab 2020, Baier 2020, Mettouchi 2017). Chaque décision d'annotation contestée est désormais accompagnée d'une évaluation explicite de son statut de sourçage (résolu / partiellement sourcé / non sourcé). Les points marqués [À VALIDER] nécessitent une validation native ou une consultation supplémentaire de la littérature avant publication définitive.*
