# Spécification de l'Opposition d'État en Kabyle (Taqbaylit) : État Libre vs. État d'Annexion

**Auteurs** : Athmane Mokraoui (butterflyoffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; structuration algorithmique et synthèse bibliographique.  
**Date** : Septembre 2026  
**Version** : 0.1-draft  
**Statut** : Document de spécification normative et algorithmique.  
**Cible** : Développeurs NLP/TAL, ingénieurs en tokenization et lemmatisation, annotateurs de treebanks (Universal Dependencies), concepteurs de filtres anti-hallucination pour LLM.

---

## Résumé

En kabyle (Taqbaylit, ISO 639-3 `kab`), le nom et l'adjectif connaissent une variation morphologique initiale fondamentale opposant deux états : **l'État Libre (*Addad ilelli*, EL / Abs)** et **l'État d'Annexion (*Addad amaruz*, EA / Cons)**. Loin d'être un simple ornement morphophonologique ou une déclinaison casuelle classique, cette opposition constitue le pivot syntaxique de la langue : elle identifie de manière non ambiguë le sujet post-verbal dans l'ordre de base VSO, marque la dépendance prépositionnelle, gouverne l'expansion génitive et résout les homographies fonctionnelles (notamment la particule `d`). 

Cette spécification formalise :
1. Les règles morphophonologiques déterministes de mutation de l'initiale nominale ($EL \to EA$).
2. La matrice syntaxique binaire régissant l'alternance d'état, éliminant les hallucinations courantes des modèles neuronaux (inversion sujet/objet, calques prépositionnels).
3. La désambiguïsation formelle de l'homographe `d` (copule ascriptive régissant l'EL vs. coordinateur régissant l'EA).
4. Le schéma d'encodage pour Universal Dependencies via la feature officielle `State=Abs|Cons`.
5. Un jeu de test unitaire fondé sur des paires minimales obligatoires pour l'évaluation CI/CD.

**Mots-clés** : kabyle, taqbaylit, état libre, état d'annexion, addad ilelli, addad amaruz, morphophonologie, VSO, syntaxe, Universal Dependencies, State, NLP.

---

## 1. Introduction et Principes d'Ingénierie NLP

### 1.1 Pourquoi cette spécification ?
Les modèles de langue actuels (LLM) et les parseurs statistiques échouent massivement sur la morphosyntaxe nominale kabyle pour deux raisons :
* **Confusion sujet/objet** : En ordre VSO canonique (*Yekcem weqcic*), l'absence de flexion casuelle terminale amène les modèles à calquer les structures sans flexion ou à produire la forme d'état libre (*\*Yekcem aqcic*), créant des énoncés perçus comme agrammaticaux ou sémantiquement ambigus par les locuteurs natifs.
* **Calques prépositionnels** : Les modèles omettent systématiquement la mutation après préposition (*\*deg axxam* au lieu de *deg wexxam*).

Cette spécification comble directement la **Limite L1** identifiée dans la *Spécification du Tokenizer Morphologique pour le Kabyle* (Mokraoui, 2026).

### 1.2 Principes fondamentaux d'implémentation
1. **Intégrité du mot graphique (Pas de sur-segmentation)** : 
   La marque d'annexion ($w-$, $y-$, $u-$, $t-$, $te-$) fait corps avec la base nominale. **Le morphème d'état ne doit jamais être détaché par le tokenizer**. La forme `wexxam` constitue un token unique.
2. **Lemmatisation canonique** :
   Le lemme d'entrée dans les dictionnaires, lemmatiseurs et lexiques d'analyse est **toujours l'État Libre (EL)**.
   * `wexxam` $\to$ lemme : `axxam`
   * `tmettut` $\to$ lemme : `tamettut`
   * `yirgazen` $\to$ lemme : `argaz` (ou pluriel `irgazen`)
3. **Typologie de la feature** :
   L'opposition d'état est encodée via le trait officiel UD **`State`** (`State=Abs` pour l'état libre, `State=Cons` pour l'état d'annexion), découplé du trait `Case`.

---

## 2. Sources d'Autorité et Cadre Théorique

La présente spécification s'appuie sur le consensus établi par les travaux majeurs de la linguistique berbère :

1. **Mammeri, Mouloud (1976)** — *Tajeṛṛumt n tmaziɣt (tantala taqbaylit)*, Maspero.
   > Source terminologique originelle fixant les concepts de *Addad ilelli* et *Addad amaruz*, et explicitant la règle syntaxique cardinale de la postposition du sujet (*ifsi udfel* vs *adfel ifsi*).
2. **Chaker, Salem (1983, 1988, 1995)** — *Un parler berbère d'Algérie (Kabylie) : syntaxe* (Thèse d'État) ; *Annexion (État d', linguistique)*, in *Encyclopédie berbère*, V, pp. 686–695 ; *Linguistique berbère : études de syntaxe et de diachronie*, Peeters.
   > Analyse exhaustive de l'état d'annexion comme marqueur de dépendance syntaxique étroite (expansion liée) et description détaillée des classes phonologiques et des invariables.
3. **Mettouchi, Amina & Frajzyngier, Zygmunt (2013)** — *A previously unrecognized typological category: The state distinction in Kabyle (Berber)*, *Linguistic Typology* 17(1), pp. 1–30.
   > Démonstration empirique sur corpus oral que l'alternance d'état n'est pas réductible à un cas morphologique classique (nominatif/accusatif), mais constitue une catégorie typologique autonome régissant la référence et la syntaxe de l'énoncé.
4. **Naït-Zerrad, Kamal (2001)** — *Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit*, Éditions Karthala.
   > Référence normative pour la transcription orthographique standard INALCO et le traitement des groupes consonantiques au féminin ($ta- \to t-$ vs. $te-$).
5. **Achab, Karim (2003, 2012)** — *Alternation of state in Berber*, in J. Lecarme (ed.), *Research in Afroasiatic Grammar II* ; *La morphologie du nom en kabyle*.
   > Modélisation formelle du statut préfixal de la voyelle initiale nominale et des relations de rection génitive ($n$).
6. **Galand, Lionel (1964, 2002)** — *L'énoncé verbal en berbère* ; *Études de linguistique berbère*, Peeters.
   > Théorisation du nom à l'état d'annexion comme « expansion de référence » explicitant l'affixe personnel verbal.

---

## 3. Morphophonologie de l'Alternance d'État ($EL \to EA$)

Dans la quasi-totalité des noms kabyles natifs, le passage de l'État Libre (*Addad ilelli*) à l'État d'Annexion (*Addad amaruz*) s'effectue par une modification de la voyelle initiale (pour les masculins) ou de la voyelle post-initiale (pour les féminins préfixés en $t-$).

### 3.1 Noms masculins

L'initiale de l'État Libre ($a-$, $i-$, $u-$) est soumise aux règles de transformation suivantes :

| Voyelle EL | Environnement phonologique de la base | Mutation $EL \to EA$ | Exemple EL | Exemple EA |
| :--- | :--- | :--- | :--- | :--- |
| **`a-`** | Base $a + CC...$ (groupe consonantique ou géminée) | $a- \to \mathbf{we-}$ | `axxam` (maison)<br>`aslem` (poisson) | `wexxam`<br>`weslem` |
| **`a-`** | Base $a + CVC...$ (consonne simple suivie d'une voyelle pleine) | $a- \to \mathbf{u-}$ (chute de $a$, vocalisation) | `adrar` (montagne)<br>`afus` (main) | `udrar`<br>`ufus` |
| **`a-`** | Mots courts, racines labiales ou voyelles stables | $a- \to \mathbf{wa-}$ | `aman` (eau)<br>`awal` (parole) | `waman`<br>`wawal` |
| **`a-`** | Cas particulier avec alternance libre selon le sous-dialecte | $a- \to \mathbf{wer-} / \mathbf{ur-}$ | `argaz` (homme) | `wergaz` / `urgaz` |
| **`i-`** | Devant consonne simple ou groupe consonantique | $i- \to \mathbf{yi-}$ | `isem` (nom)<br>`iles` (langue) | `yisem`<br>`yiles` |
| **`i-`** | Pluriels masculins réguliers en `i-...-en` | $i- \to \mathbf{yi-}$ | `irgazen` (hommes)<br>`iḍan` (chiens) | `yirgazen`<br>`yiḍan` |
| **`i-`** | Quelques bases spécifiques (maintien de schwa) | $i- \to \mathbf{ye-}$ | `irdi` (blé) | `yirdi` |
| **`u-`** | Tous contextes (insertion de la semi-voyelle homorganique) | $u- \to \mathbf{wu-}$ | `ul` (cœur)<br>`uccen` (chacal) | `wul`<br>`wuccen` |

### 3.2 Noms féminins

Les noms féminins commencent par le morphème discontinu $t-...(-t)$. La mutation d'état affecte la voyelle suivant le $t-$ initial :

| Préfixe EL | Environnement phonologique | Mutation $EL \to EA$ | Exemple EL | Exemple EA |
| :--- | :--- | :--- | :--- | :--- |
| **`ta-`** | Base débutant par une consonne simple (chute régulière de la voyelle) | $ta- \to \mathbf{t-}$ + consonne | `tamettut` (femme)<br>`tamurt` (pays) | `tmettut`<br>`tmurt` |
| **`ta-`** | Base débutant par une géminée ou un groupe de consonnes lourd | $ta- \to \mathbf{te-}$ (maintien du schwa d'appui) | `tagrest` (hiver) | `tegrest` |
| **`ti-`** | Chute de la voyelle ou relâchement en schwa | $ti- \to \mathbf{te-} / \mathbf{t-}$ | `tislit` (mariée)<br>`tiddi` (taille) | `teslit`<br>`teddi` |
| **`ti-`** | Pluriels féminins réguliers | $ti- \to \mathbf{t-} / \mathbf{te-}$ (souvent invariable orthographiquement) | `tilawin` (femmes)<br>`tullas` (filles) | `tlawin` / `tilawin`<br>`tullas` / `tellas` |
| **`tu-`** | Invariable (stabilité du timbre vocalique $u$) | $tu- \to \mathbf{tu-}$ (invariable) | `tuccent` (chacale)<br>`tuga` (herbe) | `tuccent`<br>`tuga` |

### 3.3 Noms sans alternance d'état (Invariables)

Les catégories suivantes **ne subissent jamais de mutation d'état** et conservent leur forme unique dans tous les environnements syntaxiques :

1. **Termes de parenté dépourvus de voyelle initiale** :
   * `baba` (père), `yemma` (mère), `gma` (frère), `xali` (oncle maternel), `ɛemmi` (oncle paternel).
   * *Exemple* : *Yusa-d **baba*** (sujet post-verbal en forme identique à la citation).
2. **Noms à initiale consonantique native constante** :
   * `fad` (soif), `laz` (faim), `seksu` (couscous).
   * *Exemple* : *Yewwi-t **laz***.
3. **Noms à initiale $ta-$ constante sans chute** :
   * `tala` (fontaine), `tama` (côté).
   * *Exemple* : *ɣer **tala*** (pas de mutation en `*tla`).
4. **Emprunts récents intégrés avec article arabe figé (`l-`, `ṣ-`, `ṭ-`)** :
   * `lweqt` (temps), `lqahwa` (café), `ṭṭabla` (table), `ssuq` (marché).
   * *Exemple* : *deg **ssuq***, *tasarut n **ṭṭabla***.
5. **Noms féminins invariables en `ta-` (classe lexicalisée, Naït-Zerrad 2001)** :
   * `taddart` (village), `tafat` (lumière), `tasa` (foie), `tadimt` (couvercle), `tadla` (petite gerbe), `tasga` (côté/région).
   * Ces noms conservent leur forme d'État Libre à l'identique en contexte d'annexion, **sans aucune mutation de la voyelle initiale**, contrairement à des noms de structure syllabique apparemment comparable comme `tagrest` → `tegrest` (§3.2).
   * *Exemple* : *seg **taddart*** (jamais `*seg teddart`), *deg **tafat*** (jamais `*deg tefat`).
   * **Statut** : `LEXICALISÉ — NON DÉRIVABLE`. Naït-Zerrad présente cette classe par énumération plutôt que par règle phonologique productive ; aucune règle connue ne prédit de façon fiable pourquoi ces noms échappent à la mutation alors que d'autres noms féminins de structure syllabique voisine (`tagrest`) y sont soumis. Cette liste doit être traitée comme un **lexique fermé à valider et compléter par comptage sur corpus** (voir §8, L05), et non comme le résultat d'une règle à généraliser à tout nom féminin en `ta-` + consonne géminée ou groupe lourd.

---

## 4. Matrice Syntaxique Déterministe

Le choix entre État Libre (*Addad ilelli*) et État d'Annexion (*Addad amaruz*) est **strictement conditionné par la syntaxe de la proposition**. Le schéma suivant résume la distribution complémentaire :

```
                                  RÔLE DU NOM DANS L'ÉNONCÉ
                                              │
                    ┌─────────────────────────┴─────────────────────────┐
                    ▼                                                   ▼
         [ ÉTAT D'ANNEXION (EA) ]                            [ ÉTAT LIBRE (EL) ]
         - Sujet post-verbal (VSO)                           - Objet direct verbal (DO)
         - Régime de préposition (sauf 'ar')                 - Sujet pré-verbal / topicalisé (SVO)
         - Complément du nom avec 'n'                        - Prédicat après la copule 'd'
         - Complément après numéraux                         - Régime de la préposition 'ar'
         - Après le 'd' de coordination ("et")              - Forme isolée / Vocatif avec 'a'
```

### 4.1 Déclencheurs OBLIGATOIRES de l'État d'Annexion (EA)

Toute occurrence d'un nom dans l'un des contextes suivants **doit** porter les traits morphologiques de l'État d'Annexion (`State=Cons`) :

#### 1. Le sujet lexical post-verbal (Ordre VSO canonique)
Lorsque le sujet lexical suit le verbe dont il est l'argument, il apparaît obligatoirement à l'EA (Galand 1964 ; Chaker 1983 ; Mammeri 1976).
* **Correct** : *Yekcem **w**eqcic.* / *Tekcem **t**mettut.*
* **Violation IA** : `*Yekcem aqcic.` / `*Tekcem tamettut.`

#### 2. Le complément d'une préposition simple liée
Toutes les prépositions kabyles régissent l'état d'annexion (à l'exception unique de *ar*, voir §4.2) : `deg` (dans), `seg` / `si` (de), `ɣer` (vers), `ɣef` (sur), `fell` (sur), `s` (au moyen de), `ger` (entre), `zdat` (devant), `ddaw` (sous), `nnig` (au-dessus).
* **Correct** : *deg **w**exxam*, *ɣer **t**murt*, *seg **y**isariwen*, *s **w**uzzal*.
* **Violation IA** : `*deg axxam`, `*ɣer tamurt`, `*s uzzal`.

#### 3. Le complément déterminatif d'un nom relié par `n` (Génitif)
Le second terme d'un syntagme nominal génitif introduit par la particule de relation $n$ est obligatoirement à l'EA (Achab 2003).
* **Correct** : *axxam n **w**ergaz*, *tasarut n **t**burt*, *aman n **w**anẓar*.
* **Violation IA** : `*axxam n argaz`, `*tasarut n tabburt`.

#### 4. Le complément quantifié après un nom de nombre (Cardinaux)
* Directement après les nombres 1 et 2 : *sin **w**ussan* (deux jours), *snat **n t**lawin* (deux femmes).
* Après les cardinaux supérieurs reliés par la préposition $n$ : *tlata n **w**ussan* (trois jours), *semmus n **y**irgazen* (cinq hommes).
* **Violation IA** : `*sin ussan`, `*tlata n ussan`.

#### 5. Après la particule de coordination `d` (« et », « avec »)
Le nom coordonné à un premier constituant via la conjonction `d` se met obligatoirement à l'EA (Chaker 1983).
* **Correct** : *argaz d **w**emcic-is* (l'homme et son chat), *nekk d **t**mettut-iw* (moi et ma femme).
* **Violation IA** : `*argaz d amcic-is`, `*nekk d tamettut-iw`.

---

### 4.2 Déclencheurs OBLIGATOIRES de l'État Libre (EL)

Toute occurrence d'un nom dans l'un des contextes suivants **doit** conserver sa voyelle d'État Libre (`State=Abs`) :

#### 1. Le complément d'objet direct du verbe (Accusatif)
L'objet direct verbal direct n'est jamais à l'état d'annexion, quelle que soit sa position par rapport au verbe.
* **Correct** : *Iwala **a**qcic.* (Il a vu le garçon) / *Yečča **a**ɣrum.* (Il a mangé le pain).
* **Violation IA** : `*Iwala weqcic.` (Confusion fatale entre sujet et objet).

#### 2. Le sujet pré-verbal topicalisé (Ordre SVO ou clivée)
Lorsque le sujet est placé avant le verbe pour des raisons discursives ou dans une clivée, il est à l'État Libre (Mammeri 1976 ; Achab 2020).
* **Correct** : ***A**qcic yekcem.* / *D **a**rgaz i d-yusan.*
* **Violation IA** : `*Weqcic yekcem.` / `*D wergaz i d-yusan.`

#### 3. Le prédicat après la copule ascriptive `d`
Dans les prédications non-verbales d'identification ou d'ascription introduites par la copule `d` (Mettouchi 2017), le nom attribut est obligatoirement à l'EL.
* **Correct** : *D **a**qcic.* (C'est un garçon) / *D **t**amettut.* (C'est une femme).
* **Violation IA** : `*D weqcic.` (Agrammatical au sens de "c'est un garçon").

#### 4. Le complément de la préposition d'orientation limitative `ar` (« jusqu'à »)
La préposition `ar` est la **seule exception** du système prépositionnel kabyle : elle régit obligatoirement l'État Libre (Chaker 1988).
* **Correct** : *ar **a**zekka* (à demain / jusqu'à demain), *ar **t**ameddit* (jusqu'au soir).
* **Violation IA** : `*ar wezekka`.

#### 5. La forme d'isolation, de citation et le vocatif
Le mot cité de manière isolée ou précédé de l'interpellation vocative `a` est obligatoirement à l'EL.
* **Correct** : *A **a**qcic !* / *A **t**aqcict !*
* **Violation IA** : `*A weqcic !`

---

## 5. La Règle de Désambiguïsation de l'Homographe `d`

La particule `d` est la source de la majorité des erreurs d'analyse syntaxique automatique en kabyle. L'état du nom subséquent permet une **désambiguïsation fonctionnelle stricte** :

| Morphème de surface | Catégorie UPOS | Valeur sémantique | État du nom gouverné | Exemple CoNLL-U |
| :--- | :--- | :--- | :--- | :--- |
| **`d` (copule)** | `AUX` (`PartType=Cop`) | Prédication ascriptive : *« c'est », « est »* | **ÉTAT LIBRE (`State=Abs`)** | `D aqcic.`<br>*(C'est un garçon)* |
| **`d` (coordinateur)** | `CCONJ` | Conjonction : *« et », « avec »* | **ÉTAT D'ANNEXION (`State=Cons`)** | `Argaz d weqcic.`<br>*(L'homme et le garçon)* |

### Règle d'or algorithmique pour les parseurs et linters :
$$\text{Si } [d] + \text{Nom}[State=Abs] \implies [d] = \text{\textbf{AUX (copule ascriptive)}}$$
$$\text{Si } [d] + \text{Nom}[State=Cons] \implies [d] = \text{\textbf{CCONJ (coordination)}}$$

---

## 6. Formalisation dans Universal Dependencies (UD)

En accord avec la révision v0.7 de la *Spécification Kabyle Universal Dependencies* (Mokraoui, 2026), la gestion de l'état nominal est formalisée comme suit :

### 6.1 Features morphologiques (`FEATS`)
* Le trait standard UD **`State`** est obligatoirement renseigné sur tout `NOUN`, `ADJ` ou `PROPN` susceptible d'alternance :
  * `State=Abs` : État Libre (*Addad ilelli*).
  * `State=Cons` : État d'Annexion (*Addad amaruz*).
* Le trait `Case` est articulé avec `State` :
  * Sujet post-verbal : `Case=Nom|State=Cons`
  * Sujet pré-verbal / topicalisé : `Case=Nom|State=Abs`
  * Objet direct : `Case=Acc|State=Abs`
  * Régime prépositionnel oblique : `Case=Dat|State=Cons` ou `Case=Acc|State=Cons`

### 6.2 Exemples d'annotation CoNLL-U

#### Exemple 1 : VSO canonique (Sujet post-verbal à l'EA vs. Objet direct à l'EL)
```conllu
# sent_id = state-vso-001
# text = Yečča weqcic aɣrum.
1   Yečča   ečč     VERB   _   Gender=Masc|Mood=Ind|Number=Sing|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
2   weqcic  aqcic   NOUN   _   Gender=Masc|Number=Sing|Case=Nom|State=Cons                          1   nsubj  _   _
3   aɣrum   aɣrum   NOUN   _   Gender=Masc|Number=Sing|Case=Acc|State=Abs                           1   obj    _   _
4   .       .       PUNCT  _   _                                                                    1   punct  _   _
```

#### Exemple 2 : Régime prépositionnel à l'EA
```conllu
# sent_id = state-prep-002
# text = Yekcem ɣer wexxam.
1   Yekcem  ekcem   VERB   _   Gender=Masc|Number=Sing|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
2   ɣer     ɣer     ADP    _   _                                                           3   case   _   _
3   wexxam  axxam   NOUN   _   Gender=Masc|Number=Sing|Case=Acc|State=Cons                 1   obl    _   _
4   .       .       PUNCT  _   _                                                           1   punct  _   _
```

#### Exemple 3 : Désambiguïsation de la copule `d` (+ EL)
```conllu
# sent_id = state-cop-003
# text = D argaz.
1   D       d       AUX    _   PartType=Cop                        2   cop    _   _
2   argaz   argaz   NOUN   _   Gender=Masc|Number=Sing|State=Abs   0   root   _   _
3   .       .       PUNCT  _   _                                   2   punct  _   _
```

#### Exemple 4 : Désambiguïsation du coordinateur `d` (+ EA)
```conllu
# sent_id = state-coord-004
# text = Yusa-d urgaz d weqcic.
1   Yusa    as      VERB   _   Gender=Masc|Mood=Ind|Number=Sing|Person=3|Tense=Past|VerbForm=Fin   0   root   _   _
2   -d      d       PART   _   _                                                                    1   advmod _   _
3   urgaz   argaz   NOUN   _   Gender=Masc|Number=Sing|Case=Nom|State=Cons                          1   nsubj  _   _
4   d       d       CCONJ  _   _                                                                    5   cc     _   _
5   weqcic  aqcic   NOUN   _   Gender=Masc|Number=Sing|Case=Nom|State=Cons                          3   conj   _   _
6   .       .       PUNCT  _   _                                                                    1   punct  _   _
```

---

## 7. Jeu de Test Obligatoire et Paires Minimales (CI/CD)

Pour garantir la non-régression des parseurs et évaluer l'absence d'hallucinations dans les LLM, l'implémentation doit satisfaire le banc de tests unitaires suivant :

| ID | Phrase valide (Normative) | Forme erronée (Hallucination bloquée) | Phénomène testé | Règle violée |
| :--- | :--- | :--- | :--- | :--- |
| **TS01** | `Yekcem weqcic.` | `*Yekcem aqcic.` | Sujet post-verbal (VSO) | Le sujet postposé doit porter `State=Cons` |
| **TS02** | `Aqcic yekcem.` | `*Weqcic yekcem.` | Sujet pré-verbal (SVO) | Le sujet antéposé doit porter `State=Abs` |
| **TS03** | `Iwala weqcic amcic.` | `*Iwala weqcic wemcic.` | Objet direct (Accusatif) | L'objet direct doit porter `State=Abs` |
| **TS04** | `Iwala weqcic amcic.` | `*Iwala aqcic amcic.` | Contraste Sujet vs. Objet | Le sujet VSO doit porter `State=Cons`, l'objet `State=Abs` |
| **TS05** | `Yekcem ɣer wexxam.` | `*Yekcem ɣer axxam.` | Préposition `ɣer` | Régime prépositionnel obligatoirement en `State=Cons` |
| **TS06** | `Seg tmurt ɣer temdint.` | `*Seg tamurt ɣer temdint.` | Prépositions `seg` / `ɣer` | Chute de voyelle féminine obligatoire en `State=Cons` |
| **TS07** | `Ar azekka.` | `*Ar wezekka.` | Préposition d'orientation `ar` | La préposition `ar` exige rigoureusement `State=Abs` |
| **TS08** | `Axxam n wergaz.` | `*Axxam n argaz.` | Complément du nom avec `n` | Régime génitif obligatoirement en `State=Cons` |
| **TS09** | `Sin wussan.` | `*Sin ussan.` | Numéral cardinal direct | Quantifié post-numéral obligatoirement en `State=Cons` |
| **TS10** | `Tlata n wussan.` | `*Tlata n ussan.` | Numéral cardinal avec relateur `n` | Expansion quantifiée obligatoirement en `State=Cons` |
| **TS11** | `D argaz.` | `*D wergaz.` | Copule ascriptive `d` | La copule `d` exige rigoureusement `State=Abs` |
| **TS12** | `Argaz d weqcic.` | `*Argaz d aqcic.` | Coordination `d` | Le coordonné post-`d` exige rigoureusement `State=Cons` |
| **TS13** | `A aqcic !` | `*A weqcic !` | Vocatif avec interpellation `a` | L'interpellation vocative exige `State=Abs` |
| **TS14** | `Yusa-d baba.` | `*Yusa-d wbaba.` | Invariable (nom de parenté) | Pas de mutation sur les termes de parenté nus |
| **TS15** | `Deg lweqt-nni.` | `*Deg welweqt-nni.` | Invariable (emprunt avec article) | Pas de mutation sur les emprunts figés en `l-` |
| **TS16** | `Yusa-d si taddart.` | `*Yusa-d si teddart.` | Invariable (classe lexicalisée Naït-Zerrad) | `taddart`, `tafat`, `tasa`, `tadla`, `tadimt`, `tasga` ne mutent jamais après préposition |

---

## 8. Limites Connues et Feuille de Route

| ID | Phénomène | Statut | Solution préconisée pour v0.2 |
| :--- | :--- | :--- | :--- |
| **L01** | **Alternance libre $urgaz / wergaz$** | Documenté | Autoriser les deux formes en surface, pointant vers le même lemme `argaz`. |
| **L02** | **Adjectifs qualificatifs épithètes** | Partiel | Préciser les règles d'accord d'état : l'adjectif épithète suit généralement l'état du nom qu'il qualifie (*argaz ameqqran* vs *wergaz umeqqran*). |
| **L03** | **Degré d'intégration des emprunts** | Conventionnel | Classifier la frontière exacte entre emprunts intégrés mutables (*ṭṭabla* $\to$ *n ṭṭabla*) et emprunts récents traités en `X`. |
| **L04** | **Collision acoustique avec préposition $s$** | Phonologique | Traiter la fusion graphique et le sandhi $s + w- \to [f]$ ou $[sw]$ dans une spec G2P dédiée. |
| **L05** | **Classe des féminins invariables en `ta-`** (§3.3.5 : `taddart`, `tafat`, `tasa`, `tadla`, `tadimt`, `tasga`) | Documenté (Naït-Zerrad 2001), non dérivable par règle | Interroger le corpus de 700k phrases pour chaque lemme dans les contextes `deg/seg/ɣer/s + N` afin de mesurer le taux réel de non-mutation et repérer d'éventuels lemmes supplémentaires de la même classe. |

---

## Références

1. **Achab, Karim** (2003). *Alternation of state in Berber*. In Jacqueline Lecarme (ed.), *Research in Afroasiatic Grammar II*. Amsterdam: John Benjamins, pp. 1–18.
2. **Achab, Karim** (2012). *La morphologie du nom en kabyle*. Paris: L'Harmattan.
3. **Chaker, Salem** (1983). *Un parler berbère d'Algérie (Kabylie) : syntaxe*. Thèse de doctorat d'État, Université de Provence.
4. **Chaker, Salem** (1988). *Annexion (État d', linguistique)*. In *Encyclopédie berbère*, fascicule V, Aix-en-Provence: Édisud, pp. 686–695.
5. **Chaker, Salem** (1995). *Linguistique berbère : études de syntaxe et de diachronie*. Paris/Louvain: Peeters.
6. **Galand, Lionel** (1964). *L'énoncé verbal en berbère*. *Cahiers Ferdinand de Saussure*, 21, pp. 33–53.
7. **Galand, Lionel** (2002). *Études de linguistique berbère*. Louvain/Paris: Peeters.
8. **Mammeri, Mouloud** (1976). *Tajeṛṛumt n tmaziɣt (tantala taqbaylit)*. Paris: Maspero.
9. **Mettouchi, Amina & Frajzyngier, Zygmunt** (2013). *A previously unrecognized typological category: The state distinction in Kabyle (Berber)*. *Linguistic Typology*, 17(1), pp. 1–30.
10. **Mettouchi, Amina** (2017). *Predication in Kabyle (Berber), KAB*. In Mettouchi, Frajzyngier & Chanard (eds), *Corpus-based cross-linguistic studies on Predication* (CorTypo).
11. **Mokraoui, Athmane (boffire)** (2026). *Spécification du Tokenizer Morphologique pour le Kabyle (Taqbaylit)*, v0.3-draft.
12. **Mokraoui, Athmane (boffire)** (2026). *Spécification Kabyle Universal Dependencies (UD)*, v0.7.
13. **Naït-Zerrad, Kamal** (2001). *Grammaire moderne du kabyle, tajerrumt tatrart n teqbaylit*. Paris: Éditions Karthala.
