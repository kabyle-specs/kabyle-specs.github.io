# Spécification Kabyle Universal Dependencies (UD) — Draft v0.3

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; structuration algorithmique et synthèse bibliographique.

**Date** : 29 juillet 2026

**Version** : 0.3-draft

**Statut** : En cours de validation native — certains points sont marqués **[À VALIDER]** et nécessitent confirmation ou correction par le locuteur natif.

**Cible** : Annotateurs de treebanks, développeurs de parseurs de dépendances, chercheurs en linguistique berbère et typologie syntaxique, comité Universal Dependencies.

---

## Résumé

Cette spécification propose une adaptation du framework **Universal Dependencies (UD)** à la langue kabyle (Taqbaylit, ISO 639-3 `kab`). Elle se situe dans la continuité de la première tentative documentée — le treebank **UD_Kabyle-ADPT** (Aliane, v2.8, 2021) — tout en cherchant à lever les ambiguïtés syntaxiques propres au kabyle que les métadonnées de ce treebank ne permettent pas de résoudre. Le kabyle est une langue afro-asiatique (berbère) à ordre de base **VSO**, fortement pro-drop pour le sujet, avec des clitiques pronominaux objets (datif, accusatif, directionnel) qui doublent ou remplacent les arguments lexicaux. Cette spec formalise les choix d'annotation pour la tokenization, les POS tags (UPOS), les relations de dépendances, et les features morphologiques, en s'appuyant sur les guidelines UD v2 (Nivre et al. 2020 ; de Marneffe et al. 2021), la littérature syntaxique berbère (Fahloune 2020, Felice 2020, Ouhalla 2005, Mettouchi), et les ressources internes du locuteur natif (conjugueur, tokenizer morphologique, G2P).

**Mots-clés** : kabyle, taqbaylit, Universal Dependencies, treebank, syntaxe, VSO, clitiques, copule, état d'annexion, clitic doubling.

---

## 1. Introduction

### 1.1 Contexte : le kabyle dans l'écosystème UD

À ce jour, aucune langue berbère n'est représentée dans le catalogue Universal Dependencies de manière active et maintenue. Une première tentative — **UD_Kabyle-ADPT** — a été déposée par Lakhdar Aliane dans le cadre de la release UD v2.8 (2021), avec des métadonnées indiquant des données « nonfiction news », une licence CC BY-SA 4.0, et une conversion depuis un schéma d'annotation manuel antérieur (Aliane 2021). Cependant, ce treebank n'a pas été mis à jour depuis la v2.9 et son contenu n'a pas fait l'objet d'une documentation publique détaillée (guidelines spécifiques, choix d'annotation, inter-annotator agreement). La présente spécification vise à fournir un **cadre de référence explicite** pour une annotation UD du kabyle, qu'il s'agisse de réviser l'existant ou de créer un nouveau treebank.

### 1.2 Pourquoi une spec formelle ?

Les langues à morphologie riche et à clitiques pronominaux (turc, arabe, grec, bulgare) ont montré dans l'écosystème UD que la qualité d'un treebank dépend crucialement de la documentation préalable des choix d'annotation (Çöltekin et al. 2021 pour le turc ; Taguchi et al. 2022 pour le tatar). Sans guidelines spécifiques, les annotateurs divergent sur des points fondamentaux : statut de la copule, traitement du clitic doubling, segmentation des morphèmes. Cette spec se propose de combler ce vide pour le kabyle.

### 1.3 Typologie syntaxique du kabyle (résumé)

| Trait | Description | Référence |
|-------|-------------|-----------|
| Ordre de base | VSO (Verbe-Sujet-Objet) | Felice (2020) ; Mettouchi |
| Ordres marqués | SVO, VOS, OVS attestés | Shlonsky (1987) ; Felice (2020) |
| Pro-drop sujet | Fort (le sujet lexical est optionnel) | Fahloune (2020) |
| Clitiques objets | Optionnels, doublent les arguments lexicaux | Fahloune (2020) ; Ouhalla (2005) |
| Cas | Système à état libre (FS) / état d'annexion (CS) | Felice (2020) ; Achab (2003) |
| Négation | Discontiguë : `ur` ... `ara` | — |
| Copule | Particule `D` (présentatif/copule) | — |

---

## 2. Sources primaires

### 2.1 Documents internes (Mokraoui 2026)
- **Tokenizer morphologique** (spec v0.3) : segmentation des clitiques, affixes, préfixes dérivationnels.
- **Conjugueur algorithmique** : 64 types morphologiques, 344K formes conjuguées (Bouamara 2026).
- **G2P** : 34 graphemes, règles de spirantisation/blocage.
- **Expression du temps** : particules `D`, `u`, `ɣiṛ`, `swaswa`.

### 2.2 Treebank antérieur
**Aliane, Lakhdar** (2021). *UD_Kabyle-ADPT*. Universal Dependencies v2.8. Métadonnées : genre nonfiction news, annotation « converted from manual », licence CC BY-SA 4.0. Contact : lakhdar.aliane@live.fr. Dépôt GitHub : https://github.com/UniversalDependencies/UD_Kabyle-ADPT. **Note** : les choix d'annotation spécifiques de ce treebank ne sont pas documentés publiquement ; sa qualité et sa couverture n'ont pas été évaluées dans la littérature.

### 2.3 Syntaxe berbère et clitiques
**Fahloune, Khokha** (2020). *On the status of subject and object markers in Kabyle: New evidence*, McGill Working Papers in Linguistics 26.1, pp. 1-17. Distingue affixes d'accord sujet (obligatoires, variables selon l'aspect pour les verbes d'état) et clitiques objets (optionnels, invariants, stackables DAT-ACC-DIR).

**Felice, Lydia** (2020). *On the Case System of Kabyle*, McGill Working Papers in Linguistics 26.1. Analyse le kabyle comme langue à nominatif marqué de Type 2 : état d'annexion (CS) = nominatif (sujet), état libre (FS) = accusatif (objet).

**Ouhalla, Jamal** (2005). *Clitic placement in Berber*. Les clitiques obéissent à la loi de la seconde position : un clitique ne peut pas être le premier mot de la proposition. Il peut s'attacher au verbe (V-CL) ou à une catégorie fonctionnelle (F-CL V), notamment `ad`, `ur`.

### 2.4 Phonologie et morphologie des frontières
**Bedar, Amazigh ; Quellec, Lucie ; Voeltzel, Laurence** (2021). *Epenthetic glides in Taqbaylit*, Journal of African Languages and Literatures 2/2021, pp. 1-29. Paradigme complet des clitiques pronominaux et phonologie des frontières morphémiques.

### 2.5 Guidelines UD
**Nivre, Joakim ; de Marneffe, Marie-Catherine ; et al.** (2020). *Universal Dependencies v2: An annotation scheme for multilingual dependency parsing*. LREC.

**de Marneffe, Marie-Catherine ; Manning, Christopher D. ; et al.** (2021). *Universal Dependencies*. Computational Linguistics 47(2), pp. 255-308.

### 2.6 Treebanks de référence typologiquement proches
**Çöltekin, Çağrı ; et al.** (2021). *Improving the Annotations in the Turkish Universal Dependency Treebank*. Proceedings of the International Conference on Recent Advances in Natural Language Processing (RANLP). Révision du treebank turc IMST-UD : introduction d'`advcl`, `iobj`, `xcomp`, `dislocated`, `orphan`, `clf`, `goeswith`, `dep`. Le turc, comme le kabyle, est pro-drop, agglutinant, avec des cas lexicalement sélectionnés.

**Taguchi, Chihiro ; et al.** (2022). *UD-Tatar NMCTT Treebank*. Première ressource UD pour une langue turcique de petite taille (148 phrases), avec problématiques de tokenization et de code-switching.

---

## 3. État de l'art et positionnement

### 3.1 UD_Kabyle-ADPT : ce que nous savons

D'après les métadonnées du dépôt officiel UD :
- **Release** : v2.8 (2021-05-15), dernière présence v2.9.
- **Genre** : nonfiction news.
- **Annotation** : lemmas, UPOS, XPOS, features, relations — toutes marquées « converted from manual », ce qui suggère une conversion automatique depuis un schéma d'annotation propriétaire vers UD.
- **Contributeur** : Lakhdar Aliane.
- **Licence** : CC BY-SA 4.0.

### 3.2 Ce que nous ne savons pas

Aucun article académique, rapport technique ou guidelines spécifiques n'accompagnent ce treebank. Il est donc impossible de connaître :
- Les choix d'annotation pour la copule `D` (relation `cop` vs `root`).
- Le traitement des clitiques pronominaux (`expl` vs `obj`/`iobj`).
- La tokenization adoptée (clitiques fusionnés ou segmentés ?).
- La gestion de la négation discontiguë `ur ... ara`.
- La taille exacte du corpus en phrases et tokens.
- Les scores d'inter-annotator agreement.

### 3.3 Positionnement de cette spec

Cette spécification se distingue par :
1. **Une documentation explicite** de chaque choix d'annotation, avec justification linguistique et alternatives rejetées.
2. **Un ancrage dans la littérature berbériste récente** (Fahloune 2020, Felice 2020, Bedar et al. 2021), absente des métadonnées ADPT.
3. **Une cohérence avec les ressources internes** (tokenizer morphologique v0.3, conjugueur, G2P) pour garantir l'interopérabilité.
4. **Une ouverture vers la communauté UD** : les points [À VALIDER] sont formulés comme des questions de décision collective.

---

## 4. Tokenization et segmentation

### 4.1 Principe général

UD requiert que chaque **token** corresponde à un mot graphique, sauf exceptions justifiées (multi-word tokens, MWT). Pour le kabyle, nous adoptons une segmentation intermédiaire calquée sur les pratiques des treebanks turcs (Çöltekin et al. 2021) et tatar (Taguchi et al. 2022), langues agglutinantes avec des affixes et clitiques :

- Les **affixes d'accord sujet** (préfixes `y-`, `i-`, `t-`, `n-` ; suffixes `-eɣ`, `-eḍ`, `-en`, `-ent`, `-em`, `-emt`) restent **fusionnés** avec le verbe en un seul token. Ils sont obligatoires, infixés/suffixés au radical, et leur séparation systématique créerait des tokens non autonomes phonologiquement.
- Les **clitiques pronominaux objets** (`-iyi`, `-ak`, `-as`, `-tt`, `-d`, etc.) sont **segmentés en tokens séparés** lorsqu'ils apparaissent comme éléments graphiquement distincts ou analysables. Ils sont optionnels, stackables, et peuvent migrer sur des têtes fonctionnelles (`ad-tt`, `ur-as`). **En orthographe kabyle standard, les clitiques sont séparés du verbe ou de la particule fonctionnelle par un tiret `-`.**
- Les **particules modales** (`ad`, `ur`, `wa`) sont des tokens indépendants.

### 4.2 Règles de tokenization

| Élément | Tokenization | Exemple |
|---------|--------------|---------|
| Verbe + affixes sujet | 1 token | `yekcem`, `tkecmeḍ`, `kecmeɣ` |
| Verbe + clitiques objets | Verbe = 1 token ; clitiques = tokens séparés | `y-fka` `-as` `-tt` |
| Particule + clitique | Particule = 1 token ; clitique = 1 token | `ad` `-tt` |
| Négation `ur ... ara` | `ur` = 1 token ; `ara` = 1 token | `ur` `yekcem` `ara` |
| Copule `D` | 1 token | `D` |
| Coordination `u`, `ɣiṛ` | 1 token chacune | `u` |
| Préposition `n`, `s`, `ɣer`, `deg` | 1 token | `n` |

**[À VALIDER]** : Les clitiques graphiquement fusionnés au verbe (ex. `yfkas` vs `y-fka-as`) doivent-ils être segmentés ? En l'absence d'orthographe standardisant l'usage du tiret, le tokenizer morphologique (spec v0.3) peut produire une couche d'annotation intermédiaire avec des tokens multi-mots (MWT) en CoNLL-U, comme pratiqué pour les verbes à particule en anglais ou les pronoms clitiques en français.

---

## 5. POS tags (UPOS)

### 5.1 Inventaire UPOS utilisé

| UPOS | Usage kabyle | Exemples |
|------|--------------|----------|
| `VERB` | Verbes de base et dérivés (prétérit, aoriste, intensif) | `yekcem`, `sselmed`, `ttwabder` |
| `AUX` | Particules modales aspectuo-temporelles | `ad` (aoriste), `ur` (négation) **[À VALIDER]** |
| `PART` | Particules discursives, emphatiques, négation secondaire | `ara` (négation), `D` (copule) **[À VALIDER]** |
| `NOUN` | Noms communs (état libre et état d'annexion) | `aɣrem`, `w-qcic` |
| `PROPN` | Noms propres | `Ales`, `Tizi-Wezzu` |
| `PRON` | Pronoms indépendants | `nekk`, `kečč`, `netta` |
| `DET` | Déterminants/démonstratifs | `a` (préfixe FS), `-nni` (démonstratif suffixe) **[À VALIDER]** |
| `ADJ` | Adjectifs qualificatifs | `amellal` |
| `NUM` | Numéraux | `yiwen`, `sin`, `tlata` |
| `ADV` | Adverbes | `tura`, `swaswa`, `ɣas` |
| `ADP` | Adpositions (prépositions/postpositions) | `n`, `s`, `deg`, `ɣer` |
| `CCONJ` | Conjonctions de coordination | `u` (et), `ɣiṛ` (mais/sauf) |
| `SCONJ` | Conjonctions de subordination | `ay` (relatif), `mi` (quand) **[À VALIDER]** |
| `INTJ` | Interjections | `wa`, `a-` |
| `PUNCT` | Ponctuation | `.`, `?`, `!` |
| `SYM` | Symboles | — |
| `X` | Éléments non analysés/emprunts non intégrés | — |

### 5.2 Cas particuliers

#### 5.2.1 La particule `D` (copule/présentatif)

**[À VALIDER]** — Deux analyses possibles, avec implications majeures pour la structure des arbres :

- **Analyse A (copule)** : `D` est taggé `AUX` (ou `PART` selon la grammaticalisation), avec relation `cop` vers le prédicat nominal. Le prédicat nominal est la tête (`root`).
  - Ex. : `D lweḥda` → `cop(lweḥda, D)`, `root(lweḥda)`
  - Justification : alignement avec les langues à copule (arabe, hébreu, russe dans UD). La copule `D` est grammaticalisée et ne porte pas de contenu sémantique lexical.
- **Analyse B (présentatif)** : `D` est taggé `PART` et est la tête (`root`) de la phrase, avec le prédicat nominal comme `nsubj`.
  - Ex. : `D lweḥda` → `root(D)`, `nsubj(D, lweḥda)`
  - Justification : dans les énoncés purement présentatifs (`D a!` = « Voici ! »), `D` est le seul élément predicatif.

**Recommandation préliminaire** : Analyse A (`AUX` + `cop`) pour les phrases nominales standard, car c'est l'analyse UD dominante pour les langues à copule zéro. Cependant, dans les énoncés présentatifs purs sans prédicat nominal explicite, `D` serait `root`.

#### 5.2.2 `ad` et `ur`

- `ad` (marqueur d'aoriste) : `AUX` avec relation `aux` vers le verbe, suivant le modèle des auxiliaires modaux dans les langues à marqueurs d'aspect (turc, finnois).
- `ur` (négation) : `PART` avec relation `advmod:neg` vers le verbe, ou `AUX` si on considère la négation comme tête fonctionnelle.

**[À VALIDER]** : `ur` est-il mieux analysé comme `advmod:neg` (modificateur adverbial négatif) ou comme `AUX` avec sous-type négation ? Le turc IMST-UD utilise `aux` pour les auxiliaires mais `advmod:neg` pour la négation lexicale. Le kabyle, avec sa négation discontiguë `ur ... ara`, pose la question de savoir si `ur` et `ara` forment une unité fonctionnelle distribuée.

#### 5.2.3 État libre (FS) vs État d'annexion (CS)

Les préfixes nominaux (`a-`, `ta-`, `i-`, `ti-`, `w-`, `t-`, `y-`) ne sont pas segmentés en tokens séparés dans un premier temps. Ils sont intégrés au token `NOUN`/`PROPN`. Leur statut FS/CS est encodé dans les features morphologiques (`State=Free|Annex`), comme le fait le turc pour le cas nominatif vs accusatif.

---

## 6. Relations de dépendances

### 6.1 Ordre VSO et sujet

En ordre canonique VSO, le verbe est la tête de la phrase (`root`). Le sujet post-verbal est relié par `nsubj`.

```sdparse
Yekcem w-qcic ɣer wexxam .
nsubj(Yekcem, w-qcic)
obl(Yekcem, wexxam)
case(wexxam, ɣer)
```

En ordre SVO (marqué, topicalisation), le sujet pré-verbal reste `nsubj`.

```sdparse
W-qcic yekcem ɣer wexxam .
nsubj(yekcem, w-qcic)
obl(yekcem, wexxam)
case(wexxam, ɣer)
```

### 6.2 Objets directs et indirects

#### 6.2.1 Objet lexical sans clitique

```sdparse
Y-fka w-qcic aɣrum i wemcic .
nsubj(Y-fka, w-qcic)
obj(Y-fka, aɣrum)
obl(Y-fka, wemcic)
case(wemcic, i)
```

#### 6.2.2 Clitic doubling (objet lexical + clitique)

D'après Fahloune (2020), les clitiques objets sont des instances de *clitic doubling*. En UD, lorsqu'un argument lexical et un clitique co-occurrent, l'argument lexical reçoit la relation sémantique (`obj`, `iobj`), et le clitique est annoté `expl` (expletive/pronominal copy). C'est l'analyse adoptée pour le grec, le bulgare et le roumain dans les guidelines UD.

```sdparse
Y-fka-as-tt w-qcic aɣrum i wemcic .
nsubj(Y-fka, w-qcic)
obj(Y-fka, aɣrum)
iobj(Y-fka, wemcic)
case(wemcic, i)
expl(Y-fka, -as)
expl(Y-fka, -tt)
```

**[À VALIDER]** : Cette analyse UD standard pour le clitic doubling convient-elle au kabyle ? Alternative : traiter le clitique comme `obj` ou `iobj` quand l'argument lexical est absent, et comme `expl` quand il est présent. C'est la solution recommandée préliminairement.

#### 6.2.3 Objet pronominal (clitique seul)

Quand le clitique est le seul représentant de l'argument, il reçoit la relation sémantique.

```sdparse
Y-fka-as-tt w-qcic .
nsubj(Y-fka, w-qcic)
iobj(Y-fka, -as)
obj(Y-fka, -tt)
```

### 6.3 Clitiques attachés à une catégorie fonctionnelle (F-CL)

D'après Ouhalla (2005), les clitiques peuvent s'attacher à `ad` (futur) ou `ur` (négation). Dans ce cas, le clitique est dépendant de la particule fonctionnelle dans la tokenization, mais la relation sémantique remonte au verbe.

```sdparse
Ad-tt y-ekcem w-qcic .
aux(y-ekcem, Ad)
obj(y-ekcem, -tt)
nsubj(y-ekcem, w-qcic)
```

**[À VALIDER]** : La relation entre `Ad` et `-tt` est-elle `dep`, `expl`, ou doit-on considérer `Ad-tt` comme un seul token MWT ? Le turc IMST-UD segmente les suffixes pronominaux mais les traite comme des dépendants du verbe, pas de l'auxiliaire.

### 6.4 Négation discontiguë `ur ... ara`

```sdparse
Ur yekcem ara w-qcic .
advmod:neg(yekcem, Ur)
advmod:neg(yekcem, ara)
nsubj(yekcem, w-qcic)
```

**[À VALIDER]** : `ara` est-il `advmod:neg` ou une particule de polarité (`PART`) ? Dans les treebanks turcs, les marqueurs de négation multiples sont traités comme des `advmod:neg` coordonnés.

### 6.5 La particule `D` (copule/présentatif)

#### 6.5.1 Prédicat nominal simple

```sdparse
D lweḥda .
cop(lweḥda, D)
root(lweḥda)
```

#### 6.5.2 Expression temporelle

```sdparse
D ttnac n uzal .
cop(ttnac, D)
nmod(ttnac, uzal)
case(uzal, n)
root(ttnac)
```

**[À VALIDER]** : L'analyse `cop` est-elle préférable à `root(D)` + `nsubj(D, ttnac)` ?

### 6.6 Coordination

```sdparse
Yekcem w-qcic u y-erra tameṭṭut .
nsubj(yekcem, w-qcic)
conj(yekcem, y-erra)
cc(y-erra, u)
nsubj(y-erra, tameṭṭut)
```

### 6.7 Subordination relative

```sdparse
W-qcic ay y-ekcem .
nsubj(y-ekcem, W-qcic)
mark(y-ekcem, ay)
root(y-ekcem)
```

**[À VALIDER]** : `ay` (relatif) est-il `mark` ou `nsubj` dans une relative sujet ? En berbère, `ay` est souvent analysé comme un complémentiseur.

### 6.8 Possession (état d'annexion)

```sdparse
axxam n w-qcic
nmod(axxam, w-qcic)
case(w-qcic, n)
```

La préposition `n` (génitif) est taggée `ADP` avec relation `case`. Le nom au CS (`w-qcic`) est `nmod` du nom au FS (`axxam`).

---

## 7. Features morphologiques (feats)

### 7.1 Verbes

| Feature | Valeurs | Description |
|---------|---------|-------------|
| `Aspect` | `Perf`, `Imp`, `Aor`, `Int` | Prétérit, imperfectif, aoriste, intensif |
| `Polarity` | `Pos`, `Neg` | Affirmatif, négatif |
| `Person` | `1`, `2`, `3` | Personne |
| `Number` | `Sing`, `Plur` | Nombre |
| `Gender` | `Masc`, `Fem` | Genre (3e personne) |
| `VerbForm` | `Fin`, `Part` | Forme finie, participe |
| `Mood` | `Ind`, `Imp` | Indicatif, impératif |

### 7.2 Noms

| Feature | Valeurs | Description |
|---------|---------|-------------|
| `Gender` | `Masc`, `Fem` | Genre |
| `Number` | `Sing`, `Plur` | Nombre |
| `State` | `Free`, `Annex` | État libre (FS) / État d'annexion (CS) |
| `Definite` | `Def`, `Ind` | Défini (avec démonstratif), indéfini |

### 7.3 Pronoms et clitiques

| Feature | Valeurs | Description |
|---------|---------|-------------|
| `PronType` | `Prs`, `Dem`, `Rel`, `Int` | Personnel, démonstratif, relatif, interrogatif |
| `Person` | `1`, `2`, `3` | Personne |
| `Number` | `Sing`, `Plur` | Nombre |
| `Gender` | `Masc`, `Fem` | Genre |
| `Clitic` | `Yes` | Marqueur de clitique |

### 7.4 Particules

| Feature | Valeurs | Description |
|---------|---------|-------------|
| `PartType` | `Cop`, `Neg`, `Mod`, `Agr` | Copule, négation, modal, accord |

---

## 8. Cas particuliers et ambiguïtés

### 8.1 Verbes d'état

Les verbes d'état (`mellul`, `zur`) ont un paradigme sujet spécifique au parfait (suffixe `-it` pour le pluriel). Syntaxiquement, ils se comportent comme des verbes intransitifs.

```sdparse
Mellul w-qcic .
nsubj(Mellul, w-qcic)
```

### 8.2 Supplétisme (`efk`, `ini`, `ecč`)

L'intensif supplétif (`ttakk`, `qqaṛ`, `ttett`) est annoté comme forme du même lemme, avec `Aspect=Int`.

```sdparse
Ittakk w-qcic aɣrum .
nsubj(Ittakk, w-qcic)
obj(Ittakk, aɣrum)
```

### 8.3 Emprunts

Les emprunts français/arabe non intégrés morphologiquement sont taggés `X` (ou leur catégorie approximative si intégrés).

---

## 9. Exemples CoNLL-U complets

### Exemple 1 : Phrase verbale transitive (VSO)

```conllu
# sent_id = kab-001
# text = Y-fka w-qcic aɣrum i wemcic.
# gloss = 3MS.S-give.PERF CS-boy bread to cat
# translation = 'The boy gave bread to the cat.'

1	Y-fka	vfka	VERB	_	Aspect=Perf|Gender=Masc|Number=Sing|Person=3|Polarity=Pos|VerbForm=Fin	0	root	_	_
2	w-qcic	qcic	NOUN	_	Gender=Masc|Number=Sing|State=Annex	1	nsubj	_	_
3	aɣrum	aɣrum	NOUN	_	Gender=Masc|Number=Sing|State=Free	1	obj	_	_
4	i	i	ADP	_	_	5	case	_	_
5	wemcic	emcic	NOUN	_	Gender=Masc|Number=Sing|State=Free	1	obl	_	_
6	.	.	PUNCT	_	_	1	punct	_	_
```

### Exemple 2 : Clitic doubling

```conllu
# sent_id = kab-002
# text = Y-fka-as-tt w-qcic aɣrum i wemcic.
# gloss = 3MS.S-give.PERF-3SG.DAT-3SG.ACC boy bread to cat
# translation = 'The boy gave it (the bread) to him (the cat).'

1	Y-fka	vfka	VERB	_	Aspect=Perf|Gender=Masc|Number=Sing|Person=3|Polarity=Pos|VerbForm=Fin	0	root	_	_
2	-as	as	PRON	_	Clitic=Yes|Gender=Masc|Number=Sing|Person=3|PronType=Prs	1	expl	_	_
3	-tt	tt	PRON	_	Clitic=Yes|Gender=Fem|Number=Sing|Person=3|PronType=Prs	1	expl	_	_
4	w-qcic	qcic	NOUN	_	Gender=Masc|Number=Sing|State=Annex	1	nsubj	_	_
5	aɣrum	aɣrum	NOUN	_	Gender=Masc|Number=Sing|State=Free	1	obj	_	_
6	i	i	ADP	_	_	7	case	_	_
7	wemcic	emcic	NOUN	_	Gender=Masc|Number=Sing|State=Free	1	obl	_	_
8	.	.	PUNCT	_	_	1	punct	_	_
```

**[À VALIDER]** : Les clitiques `-as` et `-tt` sont-ils bien `expl` (copies pronominales) ou doivent-ils être `iobj`/`obj` même en présence de l'argument lexical ?

### Exemple 3 : Copule `D`

```conllu
# sent_id = kab-003
# text = D lweḥda.
# gloss = COP one
# translation = 'It is one o'clock.'

1	D	D	AUX	_	PartType=Cop	2	cop	_	_
2	lweḥda	weḥda	NOUN	_	Gender=Fem|Number=Sing|State=Free	0	root	_	_
3	.	.	PUNCT	_	_	2	punct	_	_
```

**[À VALIDER]** : `D` comme `AUX` + `cop` vs `PART` + `root`.

### Exemple 4 : Négation

```conllu
# sent_id = kab-004
# text = Ur yekcem ara w-qcic.
# gloss = NEG enter.PERF NEG boy
# translation = 'The boy did not enter.'

1	Ur	ur	PART	_	PartType=Neg|Polarity=Neg	2	advmod:neg	_	_
2	yekcem	ekcem	VERB	_	Aspect=Perf|Gender=Masc|Number=Sing|Person=3|Polarity=Neg|VerbForm=Fin	0	root	_	_
3	ara	ara	PART	_	PartType=Neg|Polarity=Neg	2	advmod:neg	_	_
4	w-qcic	qcic	NOUN	_	Gender=Masc|Number=Sing|State=Annex	2	nsubj	_	_
5	.	.	PUNCT	_	_	2	punct	_	_
```

### Exemple 5 : Expression temporelle

```conllu
# sent_id = kab-005
# text = D ttnac n uzal.
# gloss = COP twelve of noon
# translation = 'It is twelve noon.'

1	D	D	AUX	_	PartType=Cop	2	cop	_	_
2	ttnac	tnac	NUM	_	NumType=Card	0	root	_	_
3	n	n	ADP	_	_	4	case	_	_
4	uzal	uzal	NOUN	_	Gender=Masc|Number=Sing|State=Free	2	nmod	_	_
5	.	.	PUNCT	_	_	2	punct	_	_
```

---

## 10. Jeu de test obligatoire

| ID | Phrase | Phénomène testé |
|----|--------|-----------------|
| T01 | `Yekcem w-qcic.` | VSO canonique, sujet post-verbal |
| T02 | `W-qcic yekcem.` | SVO (topicalisation), sujet pré-verbal |
| T03 | `Y-fka-as-tt w-qcic aɣrum.` | Clitic doubling DAT-ACC |
| T04 | `Ad-tt y-ekcem w-qcic.` | Clitique attaché à F (futur) |
| T05 | `Ur-as-tt y-fka ara.` | Clitiques attachés à Neg + négation discontiguë |
| T06 | `D lweḥda.` | Copule/présentatif |
| T07 | `D ttnac n uzal.` | Expression temporelle avec génitif |
| T08 | `Axxam n w-qcic.` | Possession (état d'annexion) |
| T09 | `Yekcem w-qcic u y-erra tameṭṭut.` | Coordination de verbes |
| T10 | `W-qcic ay y-ekcem.` | Relative sujet |
| T11 | `Mellul w-qcic.` | Verbe d'état |
| T12 | `Ittakk w-qcic aɣrum.` | Supplétisme intensif |
| T13 | `Acḥal ssaɛa?` | Phrase interrogative (wh-in-situ) |
| T14 | `Tura, acḥal ssaɛa?` | Dislocation temporelle |
| T15 | `Y-fka-as-tt-id w-qcic aɣrum i wemcic.` | Stacking clitique DAT-ACC-DIR |

---

## 11. Différences attendues avec UD_Kabyle-ADPT (hypothèses)

Compte tenu du manque de documentation publique d'ADPT, les différences probables avec cette spec portent sur :

| Domaine | Cette spec (v0.3) | Hypothèse sur ADPT |
|---------|-----------------|-------------------|
| **Tokenization des clitiques** | Segmentation explicite `-as`, `-tt` | Inconnue (peut-être fusionnés au verbe) |
| **Copule `D`** | `AUX` + `cop` (analyse préliminaire) | Inconnue |
| **Négation `ur ... ara`** | `advmod:neg` + `advmod:neg` | Inconnue |
| **Clitic doubling** | `expl` pour les clitiques, `obj`/`iobj` pour les arguments lexicaux | Inconnue |
| **État libre / annexion** | Feature `State=Free|Annex` | Inconnue |
| **Verbes d'état** | Traités comme `VERB` intransitifs | Inconnue |
| **Genre du corpus** | Prêt à couvrir Tatoeba/Weblate (parlé + technique) | Nonfiction news uniquement |

**Recommandation** : une comparaison formelle avec les fichiers CoNLL-U d'ADPT serait nécessaire pour identifier les convergences et divergences précises. Cette spec se présente comme un cadre de référence indépendant, révisable à la lumière de cette comparaison.

---

## 12. Limites connues et feuille de route

| ID | Limite | Statut |
|----|--------|--------|
| L1 | **Segmentation des clitiques** : faut-il segmenter `-as`, `-tt` en tokens séparés ou les laisser fusionnés au verbe ? | **[À VALIDER]** — Impact sur la tokenization CoNLL-U |
| L2 | **Statut de `D`** : `AUX`+`cop` vs `PART`+`root` | **[À VALIDER]** — Décision syntaxique fondamentale |
| L3 | **Statut de `ur` et `ara`** : `advmod:neg` vs `aux:neg` | **[À VALIDER]** |
| L4 | **Clitiques en présence d'arguments lexicaux** : `expl` vs `obj`/`iobj` | **[À VALIDER]** — Doit être cohérent avec la typologie UD |
| L5 | **État libre vs état d'annexion** : feature `State` suffisant ou faut-il segmenter les préfixes ? | **[À VALIDER]** |
| L6 | **Interrogatives** : `acḥal` est-il `ADV` ou `PRON` interrogatif ? | **[À VALIDER]** |
| L7 | **Participe `n`** : `y-V-n` (forme neutre) — `VerbForm=Part` ou `VerbForm=Fin` avec `Mood=Ind` ? | **[À VALIDER]** |
| L8 | **Adjectifs vs noms d'état** : `amellal` (blanc) est-il `ADJ` ou `NOUN` en état libre ? | **[À VALIDER]** |
| L9 | **Prépositions complexes** : `ɣer`, `deg`, `si` (emprunt) — toutes `ADP` ? | **[À VALIDER]** |
| L10 | **Absence de comparaison avec ADPT** : les fichiers CoNLL-U d'ADPT n'ont pas été analysés | Prochaine étape |
| L11 | **Taille du treebank** : cette spec nécessite un corpus annoté de 5 000–10 000 phrases pour validation | Prochaine étape |

---

## 13. Implémentation recommandée

### 13.1 Pipeline d'annotation

```
Corpus brut (Tatoeba / Weblate / texte natif)
    ↓
Tokenization morphologique (spec v0.3)
    ↓
POS tagging + features (Stanza / spaCy / règles)
    ↓
Annotation syntaxique manuelle (Brat / INCEpTION)
    ↓
Adjudication (locuteur natif + linguiste)
    ↓
Conversion CoNLL-U + validation UD
    ↓
Soumission au comité UD
```

### 13.2 Outils

- **Annotation** : INCEpTION (recommandé par l'Uzbek UD, NCBI 2026) ou Brat.
- **Validation** : `udapy` (Popel et al. 2017) pour la validation CoNLL-U.
- **Parsing** : Stanza (pré-entraîné sur vos tokenizers), Trankit (transformer-based, meilleures performances UAS/LAS selon Sung & Shin 2024).

---

## Références

1. **Aliane, Lakhdar** (2021). *UD_Kabyle-ADPT*. Universal Dependencies v2.8. https://github.com/UniversalDependencies/UD_Kabyle-ADPT
2. **Bedar, Amazigh ; Quellec, Lucie ; Voeltzel, Laurence** (2021). *Epenthetic glides in Taqbaylit*, Journal of African Languages and Literatures 2/2021, pp. 1-29.
3. **Bouamara, K.** (2026). *Modélisation des types morphologiques et de la conjugaison du verbe kabyle*. HAL.
4. **Çöltekin, Çağrı ; et al.** (2021). *Improving the Annotations in the Turkish Universal Dependency Treebank*. RANLP 2021.
5. **de Marneffe, Marie-Catherine ; Manning, Christopher D. ; et al.** (2021). *Universal Dependencies*. Computational Linguistics 47(2), pp. 255-308.
6. **Fahloune, Khokha** (2020). *On the status of subject and object markers in Kabyle: New evidence*, McGill Working Papers in Linguistics 26.1, pp. 1-17.
7. **Felice, Lydia** (2020). *On the Case System of Kabyle*, McGill Working Papers in Linguistics 26.1.
8. **Mokraoui, Athmane (boffire)** (2026). *Spécification du Tokenizer Morphologique pour le Kabyle*, v0.3.
9. **Mokraoui, Athmane (boffire)** (2026). *Conjugueur algorithmique du verbe kabyle*.
10. **Nivre, Joakim ; et al.** (2020). *Universal Dependencies v2: An annotation scheme for multilingual dependency parsing*. LREC.
11. **Ouhalla, Jamal** (2005). *Clitic placement in Berber*.
12. **Shlonsky, Ur** (1987). *The parametric variation of the verbal clause in Berber*. In Studies in Berber Syntax, Université de Genève.
13. **Sung, Youkyung ; Shin, Jeong-Min** (2024). Cited in : *Second language Korean Universal Dependency treebank v1.2*. arXiv:2503.14718.
14. **Taguchi, Chihiro ; et al.** (2022). *UD-Tatar NMCTT Treebank*. UD v2.11.

---

*Document rédigé dans le cadre du développement des ressources NLP pour la langue kabyle. Les points marqués [À VALIDER] nécessitent une décision du locuteur natif ou une consultation de la communauté UD avant publication définitive. UD_Kabyle-ADPT est cité comme antécédent documenté mais non évalué.*
