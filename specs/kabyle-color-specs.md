---
title: Couleurs
status: draft
summary: Termes de couleur canoniques pour le kabyle (Taqbaylit), leurs flexions de genre, nombre et état, les constructions utilisées pour les attacher aux noms, et les variantes attestées que les implémenteurs doivent déclarer plutôt que mélanger silencieusement.
---

# Les termes de couleur en kabyle (Taqbaylit) : spécification des adjectifs chromatiques

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; recherche documentaire, structuration morphologique et vérification native.

**Date** :  15 août 2026

**Version** : 2026-08-15

**Cible** : Lexicographes, développeurs d'applications d'apprentissage, ingénieurs i18n/l10n, linguistes computationnels, annotateurs de corpus.

---

## Résumé

Le kabyle (Taqbaylit, code ISO 639-3 `kab`) dispose d'un inventaire de termes de couleur composé de racines berbères natives, d'emprunts arabes et français intégrés, et de formations déverbales sur verbes d'état. Ce document expose la spécification complète des adjectifs de couleur : leur flexion pour le genre, le nombre et l'état (libre vs. d'annexion), les règles d'accord avec le nom, les variantes dialectales et empruntées documentées, ainsi que les ambiguïtés sémantiques notoires — notamment la fusion bleu/vert dans la racine `azegzaw` et la concurrence entre emprunts pour l'orange et le violet. Douze barrières de qualité sont fournies pour valider les implémentations.

**Mots-clés** : kabyle, taqbaylit, couleurs, adjectifs chromatiques, genre, nombre, état d'annexion, emprunts lexicaux, bleu/vert, barrières de qualité.

---

## État

`draft` — non encore discuté ni adopté. Corrections et vérification par locuteur natif bienvenues.

## Motivation

Les adjectifs de couleur kabyles se fléchissent comme les adjectifs kabyles ordinaires (accord en genre, nombre et état), et plusieurs d'entre eux présentent plus d'une graphie ou racine attestée selon la source, la région, et selon que l'auteur suit une convention « traditionnelle » ou « puriste/néologique ». Sans référence, deux implémenteurs décrivant « une chemise verte » peuvent aboutir à des mots différents mais tous deux défendables. Cette spécification fixe une forme canonique recommandée par couleur tout en documentant les variantes connues, plutôt que de faire comme si la variation n'existait pas.

## Périmètre

**En périmètre :**
- Les termes de couleur de base et leurs formes masculin/féminin, singulier/pluriel, libre/annexion
- La règle d'accord qui produit ces formes
- Les deux constructions utilisées pour attacher une couleur à un nom (attributive, prédicative avec `d`)
- Les variantes attestées et les conventions que les implémenteurs doivent déclarer lorsqu'ils choisissent parmi elles
- Les modificateurs de nuance natifs (clair, foncé, blanchâtre)

**Hors périmètre** (noté uniquement pour référence croisée) :
- Les formes verbales (inchoatives) de couleur, ex. « devenir rouge »
- Les détails d'encodage tifinagh — reportés à une future spécification d'orthographe
- Les idiotismes et emplois figurés de couleur
- Les mots évaluatifs/esthétiques qui apparaissent dans les études d'idiomes aux côtés des termes de couleur mais ne sont pas eux-mêmes des termes de couleur (voir §3.7)

---

## Spécification

### 1. Règle de flexion

Les adjectifs de couleur kabyles sont construits sur un radical et se fléchissent pour le genre, le nombre et l'état de la même manière que la plupart des adjectifs kabyles.

#### 1.1 Genre et nombre

| Forme | Modèle | Exemple (blanc) |
|---|---|---|
| Masculin singulier (état libre) | `a-` + radical | `amellal` |
| Féminin singulier (état libre) | `ta-` + radical + `-t` (circumfixe) | `tamellalt` |
| Masculin pluriel (état libre) | `i-` + radical + `-en` (+ changement vocalique interne) | `imellalen` |
| Féminin pluriel (état libre) | `ti-` + radical + `-in` | `timellalin` |

La règle du féminin singulier (« envelopper le radical masculin dans `t...t` ») est productive et s'applique à pratiquement tous les termes de couleur ci-dessous.

#### 1.2 Alternance d'état (libre vs. d'annexion)

Comme tous les noms et adjectifs kabyles, les termes de couleur alternent entre **état libre** (non marqué, utilisé en prédicat et comme objet post-verbal) et **état d'annexion** (état construit, utilisé après des prépositions, comme sujet post-verbal, et dans les constructions génitivales).

Les règles pour les adjectifs de couleur sont identiques à celles des autres adjectifs commençant par `a-` :

| État libre | État d'annexion | Contexte déclencheur |
|---|---|---|
| `amellal` | `umellal` | après préposition `n`, `deg`, etc. |
| `aberkan` | `uberkan` | après préposition |
| `azeggaɣ` | `uzeggaɣ` | après préposition |
| `awraɣ` | `uwraɣ` | après préposition |
| `azegzaw` | `uzegzaw` | après préposition |
| `aẓeṛqaq` | `uẓeṛqaq` | après préposition |
| `aqahwi` | `uqahwi` | après préposition |
| `axuxi` | `uxuxi` | après préposition |
| `arẓaẓ` | `urẓaẓ` | après préposition |
| `aṛanǧi` | `uṛanǧi` | après préposition |
| `ačini` | `učini` | après préposition |
| `amelliɣdi` | `umelliɣdi` | après préposition |
| `aberqac` | `uberqac` | après préposition |
| `aɣiɣdan` | `uɣiɣdan` | après préposition |
| `aḥcayci` | `uḥcayci` | après préposition |
| `acemlax` | `ucemlax` | après préposition |
| `aqesḥan` | `uqesḥan` | après préposition |
| `acamlal` | `ucamlal` | après préposition |

Les adjectifs de couleur féminins en `ta-…-t` perdent la voyelle initiale à l'état d'annexion :

| État libre | État d'annexion |
|---|---|
| `tamellalt` | `tmellalt` |
| `taberkant` | `tberkant` |
| `tazeggaɣt` | `tzeggaɣt` |
| `tawraɣt` | `twraɣt` |
| `tazegzawt` | `tzegzawt` |
| `taẓeṛqaqt` | `tẓeṛqaqt` |
| `taberqact` | `tberqact` |
| `taɣiɣdant` | `tɣiɣdant` |
| `taḥcaycit` | `tḥcaycit` |
| `tacemlaxt` | `tcemlaxt` |
| `taqesḥant` | `tqesḥant` |
| `tacamlalt` | `tcamlalt` |

Exemples en contexte :

```
Aseggas amellal.                « Une année blanche. » (état libre, prédicat)
Aseggas n umellal.              « Une année de blanc. » (annexion après n)
Taqcict tazegzawt.              « Une fille verte. » (état libre)
Taqcict n tzegzawt.             « Une fille de vert. » (annexion après n)
```

### 2. Table des termes canoniques

| Français | Masc. sg. (libre) | Fém. sg. (libre) | Masc. pl. (libre) | Fém. pl. (libre) | Notes |
|---|---|---|---|---|---|
| blanc | `amellal` | `tamellalt` | `imellalen` | `timellalin` | |
| noir | `aberkan` | `taberkant` | `iberkanen` | `tiberkanin` | |
| rouge | `azeggaɣ` | `tazeggaɣt` | `izeggaɣen` | `tizeggaɣin` | Aussi graphié `azegwaɣ`, `azggaɣ` selon les sources ; voir §3.8 |
| jaune | `awraɣ` | `tawraɣt` | `iwraɣen` | `tiwraɣin` | Aussi graphié `aweṛaɣ` ; voir §3.8 |
| bleu-vert | `azegzaw` | `tazegzawt` | `izegzawen` | `tizegzawin` | Voir §3.1 (bleu/vert) et §3.2 (variante régionale) |
| gris | `aɣiɣdan` | `taɣiɣdant` | `iɣiɣden` | *non confirmé* | Natif, dérivé de `iɣed` « cendre ». Variante non fléchie : `amelliɣdi` (§3.5) |
| bariolé, multicolore, tacheté | `aberqac` | `taberqact` | `iberqacen` | `tiberqacin` | Natif. Source : Olivier (1878). Variante régionale `aberbac` |
| vert (alt.) | `aḥcayci` | `taḥcaycit` | `iḥcayciyen` | `tiḥcayciyin` | Natif, dérivé de `leḥcic` « herbe verte ». Variante régionale du Soummam : `adal` (§3.2) |
| marron | `aqahwi` | `taqahwit` | `iqahwiyen` | `tiqahwiyin` | **Emprunt arabe** : `qahwa`, « café ». Variante fém. `taqahwitt` attestée mais non recommandée |
| rose | `axuxi` | `taxuxit` | `ixuxiyen` | `tixuxiyin` | **Emprunt arabe** : `xux`, « pêche » |
| violet | `arẓaẓ` | `tarẓaẓt` | `irẓaẓen` | `tirẓaẓin` | Natif, mais ambigu avec « guêpe » ; peu standardisé. `azenǧari`/`aǧenǧari` attesté comme synonyme régional, voir §3.3 |
| orange | `aṛanǧi` (emprunt du nom de la couleur) ou `ačini`/`atchini` (emprunt via le nom du fruit) | `taṛanǧit` | `iṛanǧiyen` | `tiṛanǧiyin` | **Emprunts arabes**. Deux emprunts indépendamment attestés, non un seul terme établi avec variantes graphiques ; voir §3.4 |

Les entrées marquées *non confirmé* ne doivent pas être traitées comme établies tant qu'elles ne sont pas corroborées par une source de locuteur natif ou un corpus ; voir Questions ouvertes.

### 3. Variantes attestées et conventions

#### 3.1 La distinction bleu/vert

Les variétés berbères, kabyle comprise, utilisent classiquement une seule racine — `azegzaw` — pour le « vert » et le « bleu » : la couleur de la végétation et celle du ciel sont désignées par le même mot. C'est une caractéristique bien documentée du berbère, pas une omission.

Certains matériels pédagogiques modernes assignent plutôt `azegzaw` au vert seul et introduisent `aẓeṛqaq` comme mot dédié pour le bleu. `Aẓeṛqaq` est lui-même un emprunt : il dérive de l'arabe أزرق (*azraq*, « bleu »), adapté au modèle de l'adjectif kabyle. Ainsi, la convention « séparée » n'introduit pas un mot kabyle natif pour le bleu — elle emprunte un mot, par-dessus l'`azegzaw` hérité qui couvrait déjà bleu/vert.

Les implémenteurs doivent choisir l'une des options suivantes et la déclarer :
- **Traditionnelle / unifiée** : `azegzaw` = bleu-ou-vert (le contexte désambiguïse) ; pas de terme bleu séparé.
- **Séparée** : `azegzaw` = vert, `aẓeṛqaq` = bleu, traités comme deux entrées distinctes.

Cette spécification n'en impose pas une plutôt que l'autre — elle exige que celle utilisée soit déclarée, car mélanger silencieusement les deux conventions dans un même jeu de données produit des résultats incohérents.

#### 3.2 Variante régionale : `adal` vs. `tizegzewt` (Soummam)

Dans certaines parties de la région du Soummam, les locuteurs distinguent deux mots différents pour « vert » selon *ce qui* est vert, plutôt que d'utiliser `azegzaw`/`tazegzawt` pour tout :

- **`adal`** — le vert comme couleur abstraite ou appliquée : peinture, teinture, un objet, un uniforme. Une source en langue kabyle gloss `adal` comme littéralement « mousse », étendu métonymiquement à « vert, de couleur verte » ; une discussion de dictionnaire kabyle le gloss indépendamment comme « vert militaire » (*vert militaire*). Paradigme complet : `adal` (m.sg.), `tadalt` (f.sg.), `idalen` (m.pl.), `tidalin` (f.pl.).

  Note : une source lexicale distincte donne les formes plurielles `adalen`/`tadalin` — sans le changement vocalique `a→i`. C'est un véritable désaccord entre sources, pas seulement une différence graphique, car il s'agit de deux règles différentes pour former le pluriel. Tant que ce n'est pas résolu, traitez les deux comme attestées et choisissez l'une selon la même approche « déclarez votre convention » utilisée ailleurs dans cette spécification. La forme `idalen`/`tidalin` (changement vocalique régulier) est recommandée par défaut.
- **`tizegzewt`** (aussi graphié `tizgzewt`) — la verdure de la nature vivante spécifiquement : un champ, un arbre, un paysage. Il est glossé comme « verdure » plutôt que comme adjectif de couleur à usage général, et est un dérivé nominal de la racine `azegzaw` plutôt que la simple forme féminine ordinaire de `tazegzawt`.

En d'autres termes, ce n'est pas un second synonyme de « vert » — c'est une coupure sémantique : `adal` pour la couleur comme propriété qu'on applique ou nomme en abstrait, `tizegzewt` pour l'état d'une chose naturelle étant verte/luxuriante. Que cette coupure soit utilisée, contre un `azegzaw`/`tazegzawt` unique pour les deux sens, semble varier selon la région et le locuteur ; traitez-la de la même manière que la coupure bleu/vert du §3.1 — les implémenteurs doivent déclarer quelle convention ils suivent plutôt que de les mélanger silencieusement.

Ceci n'a pas été vérifié au-delà des sources ci-dessous et de l'usage régional du rapporteur ; une couverture dialectale plus large aiderait à confirmer sa diffusion en dehors du Soummam.

#### 3.3 Violet : `arẓaẓ` vs. `azenǧari`/`aǧenǧari`

Deux termes sont attestés pour le violet, sans coupure régionale clairement documentée (contrairement aux cas des §3.1 et §3.2) :

- **`arẓaẓ`** — apparaît dans les listes de vocabulaire général comme le terme de base pour le violet.
- **`azenǧari`** (aussi graphié `aǧenǧari`, l'alternance `z`/`ǧ` reflétant la réalisation du phonème) — glossé indépendamment comme « violet » dans une source de discussion de dictionnaire kabyle.

Notez que `azenǧari`/`aǧenǧari` **n'est pas** un terme pour l'orange, malgré la ressemblance superficielle avec `aṛanǧi` (voir §3.4) — ce sont deux mots sans relation pour des couleurs sans relation, qui partagent par hasard un squelette consonantique similaire. Traitez `arẓaẓ` et `azenǧari`/`aǧenǧari` comme des synonymes en attendant des informations plus claires sur leur distribution régionale.

`Arẓaẓ` est aussi, séparément, le mot kabyle ordinaire pour « guêpe » — un insecte commun, pas un sens rare ou archaïque. Une source note que le sens de couleur (particulièrement le pluriel féminin `tirẓaẓin`) est aujourd'hui largement restreint aux expressions figées, tandis que le sens insecte de `arẓaẓ` est d'usage courant. Cela signifie que `arẓaẓ` seul est plus susceptible d'être compris comme « guêpe » que comme « violet » par un locuteur actuel ; les implémenteurs doivent le désambiguïser par le contexte (par ex. en le jumelant avec un nom qu'il modifie) plutôt que de le présenter comme un mot de couleur autonome.

#### 3.4 Orange : deux emprunts sans relation

Le kabyle ne semble pas disposer d'un terme de base hérité (non emprunté) pour l'orange, et deux emprunts différents sont en usage :

- **`aṛanǧi`** — un emprunt direct du nom de la couleur (cf. français/international « orange », lui-même ultimement de l'arabe/persan *nāranj*). Attesté avec un paradigme genre/nombre complet : `aṛanǧi` (m.sg.), `taṛanǧit` (f.sg.), `iṛanǧiyen` (m.pl.), `tiṛanǧiyin` (f.pl.).
- **`ačini`** (aussi graphié `atchini`) — un emprunt via le nom du *fruit* plutôt que le nom de la couleur. Il dérive directement de `ččina`/`ccina`, le nom kabyle du fruit orange (attesté dans l'usage quotidien, par ex. « ils ont pressé des oranges » utilise `ccina` comme forme collective/massive), avec une forme singulative `tačinatt` (« une orange ») construite comme le sont généralement les singulatifs berbères — enveloppant le nom dans `t...t`, qui ressemble par hasard au circumfixe de l'adjectif féminin du §1 mais est ici une catégorie grammaticale différente (marque singulative sur un nom, pas accord de genre sur un adjectif). Le nom du fruit lui-même échole le mot arabe nord-africain référant à « Chine » (China), comme plusieurs langues nomment les agrumes d'après une origine commerciale plutôt qu'une couleur. Ainsi, `ačini` le terme de couleur est à un degré dérivationnel de `ččina` le fruit, qui est lui-même l'emprunt.

Les deux sont attestés indépendamment l'un de l'autre comme traductions de « orange », il s'agit donc de deux emprunts concurrents plutôt que d'un seul mot établi avec variantes graphiques. Comme pour la coupure bleu/vert, les implémenteurs doivent en choisir un et le déclarer plutôt que de mélanger les deux dans le même jeu de données.

#### 3.5 Gris : `aɣiɣdan`

Le gris ne dispose pas d'un terme natif universel dans les sources lexicographiques anciennes. La forme `amelliɣdi` (aussi `amlliɣdi`, `abraɣdi`) apparaît dans des listes de vocabulaire amazigh général, mais son étymologie (`am` + `iɣed` « cendre ») n'a pas pu être confirmée indépendamment comme mot kabyle autonome, et son paradigme de flexion n'est attesté dans aucune source consultée.

L'enquête de terrain de la thèse UMMTO (géolinguistique, 30 points en Kabylie) donne **`aɣiɣdan`** (m.sg.), **`taɣiɣdant`** (f.sg.), **`iɣiɣden`** (m.pl.), dérivé nominal **natif** de `iɣed` « cendre ». Cette spécification recommande **`aɣiɣdan`** comme terme canonique pour le gris. La forme `amelliɣdi` est conservée en référence comme variante non fléchie issue de sources lexicales générales amazighes.

#### 3.6 `aẓeṛqaq` utilisé généralement pour la couleur des yeux

Au-delà de son usage comme alternative « séparée » à `azegzaw` pour le bleu en général (§3.1), `aẓeṛqaq`/`taẓeṛqaqt` est utilisé spécifiquement pour décrire la couleur des yeux sans distinguer le bleu du vert — quelqu'un aux yeux verts et quelqu'un aux yeux bleus sont tous deux décrits comme `d aẓeṛqaq (wallen-is)`, approximativement « (ses yeux sont) `aẓeṛqaq` », sans qu'un mot distinct ne soit proposé pour le cas vert.

C'est un phénomène distinct de la fusion générale bleu/vert dans `azegzaw` (§3.1) : ici c'est spécifiquement l'*emprunt* qui porte l'ambiguïté, dans un domaine spécifique (les yeux), plutôt que le terme de couleur hérité qui couvre les deux teintes partout. Les implémenteurs construisant quoi que ce soit devant rendre « yeux bleus » vs. « yeux verts » comme valeurs distinctes doivent être conscients qu'un terme élicité unique (`aẓeṛqaq`) peut ne pas résoudre cette distinction pour les descriptions oculaires, alors qu'il le pourrait pour, disons, un vêtement.

#### 3.7 N'est pas un terme de couleur : `acebḥan`

`Acebḥan` n'est pas un terme de couleur de base, bien qu'il apparaisse fréquemment jumelé à `amellal` (« blanc ») dans la littérature idiomatique kabyle. Il appartient à une famille de mots entièrement différente : `ccbaḥa` (« beauté », nom), `cebḥ`/`tcebeḥ` (« être beau », verbe — ex. `Tecbeḥ tqenduṛt`, « la robe est belle » ; `Acḥal i tcebḥeḍ!`, « que tu es belle ! »). `Acebḥan` est la forme adjectivale de cette même racine, signifiant « beau » ou « de bonne qualité » — pas « blanc ».

Il apparaît à côté de `amellal` dans les collections d'idiomes parce que les idiotismes d'éloge kabyles exploitent une association culturelle entre la blancheur et la beauté (attestée remontant à des descriptions de la blancheur comme critère de beauté traditionnel), pas parce que les deux mots sont synonymes. Un exemple représentatif : `D ucbiḥ iɣil` (« son bras est `ucbiḥ` ») ne signifie pas « son bras est blanc » — cela signifie « il est très habile ». Le mot fait un travail évaluatif (« excellent », « admirable »), pas chromatique.

**N'ajoutez pas `acebḥan` à la table des termes du §2 comme synonyme de blanc.** Plus généralement : les études d'idiomes de couleur collectent et discutent nécessairement de mots *aux côtés* des termes de couleur parce que c'est leur sujet, mais la co-occurrence dans cette littérature n'est pas une preuve qu'un mot est lui-même un terme de couleur. Tout ajout futur sourcé d'une étude d'idiome/phraseologie doit être vérifié contre ceci avant d'être intégré à la table canonique.

#### 3.8 Variantes orthographiques : `azeggaɣ` / `azegwaɣ` / `azggaɣ` et `awraɣ` / `aweṛaɣ`

Le terme rouge présente trois graphies attestées : `azeggaɣ` (canonique dans cette spécification), `azegwaɣ`, et `azggaɣ`. Le verbe d'état sous-jacent est `izwiɣ` (« être rouge »), mais la dérivation adjectivale n'est pas entièrement transparente d'une source à l'autre. Le `gg` de `azeggaɣ` représente une géminée /gː/ ; le `ɣ` final est la consonne radicale. `Azegwaɣ` insère une glide `w` qui peut refléter une prononciation dialectale ou une analyse radicale alternative. `Azggaɣ` supprime la voyelle épenthétique. Cette spécification traite `azeggaɣ` comme canonique et les autres comme variantes orthographiques ; les implémenteurs doivent lemmatiser les trois vers `azeggaɣ`.

Le terme jaune présente `awraɣ` (canonique) et `aweṛaɣ`. Le `e` de `aweṛaɣ` est épenthétique, rompant le groupe `w-r`. Le féminin `tawraɣt` confirme `awraɣ` comme radical de base. Les implémenteurs doivent accepter les deux graphies mais lemmatiser vers `awraɣ`.

#### 3.9 Nuances : clair, foncé et blanchâtre

Le kabyle dispose de modificateurs natifs pour exprimer les nuances d'une couleur. Ils suivent la couleur qu'ils modifient (postposition) et s'accordent en genre et nombre avec le nom tête.

| Nuance | Masc. sg. | Fém. sg. | Masc. pl. | Fém. pl. | Sens |
|---|---|---|---|---|---|
| clair | `acemlax` | `tacemlaxt` | `icemlaxen` | `ticemlaxin` | teinte atténuée |
| foncé | `aqesḥan` | `taqesḥant` | `iqesḥanen` | `tiqesḥanin` | teinte soutenue, sombre |
| blanchâtre | `acamlal` | `tacamlalt` | `icamlalen` | `ticamlalin` | blanc cassé |

**Syntaxe** : `[Nom] [Couleur] [Nuance]` — les trois éléments s'accordent en genre et nombre.

```
tajeǧǧigt-nni tazeggaɣt taqesḥant    « la fleur rouge foncée »
aberkan aqesḥan                         « noir foncé »
azeggaɣ acemlax                         « rouge clair »
azegzaw aqesḥan                         « vert foncé »
amellal acemlax                         « blanc clair / blanc cassé »
```

**Note sur `acamlal`** : en dehors du domaine chromatique, `acamlal` est attesté comme nom de variété d'olivier. En contexte de couleur, il désigne la nuance blanchâtre de `amellal`.

#### 3.10 Bariolé, multicolore, tacheté

Le kabyle ne dispose pas d'un adjectif de couleur de base signifiant « multicolore » au sens abstrait. Le terme natif **`aberqac`** (m.sg.), **`taberqact`** (f.sg.), **`iberqacen`** (m.pl.), **`tiberqacin`** (f.pl.) est employé avec les sens de « bariolé, bigarré, tacheté ».

Attesté chez Olivier (1878) sous les graphies `aberqac` et `aberquc`, avec le pluriel `ib-cen` (résolu en `iberqacen`). Le féminin et le pluriel féminin ont été confirmés par locuteur natif. Variante régionale `aberbac` (spirantisation / affrication du `q`).

**Usage** : le terme s'applique initialement aux tissus et étoffes, mais des locuteurs de certaines régions l'étendent à tout objet présentant plusieurs couleurs.

#### 3.11 Annexe : emprunts arabes attestés

Les termes suivants sont attestés dans la littérature ou les enquêtes de terrain, mais sont d'origine arabe. Ils sont documentés ici pour référence, mais ne font pas partie du tableau canonique §2.

| Couleur | Masc. sg. | Fém. sg. | Masc. pl. | Fém. pl. | Origine arabe |
|---|---|---|---|---|---|
| marron | `aqahwi` | `taqahwit` | `iqahwiyen` | `tiqahwiyin` | `qahwa` |
| rose | `axuxi` | `taxuxit` | `ixuxiyen` | `tixuxiyin` | `xawx` |
| rose (alt.) | `awerdi` | `tawerditt` | `iwerdiyen` | `tiwerdiyin` | `ward` |
| orange | `aṛanǧi` | `taṛanǧit` | `iṛanǧiyen` | `tiṛanǧiyin` | `nāranj` |
| orange (alt.) | `ačinawi` | `tačinawitt` | `ičinawiyen` | `tičinawiyin` | via `ččina` |
| bleu (distinct) | `aẓeṛqaq` | `taẓeṛqaqt` | `iẓeṛqaqen` | `tiẓeṛqanin` | `azraq` |
| mauve / indigo | `anili` | `tanilit` | `iniliyen` | `tiniliyin` | `nīla` |
| doré | `adehbi` | `tadehbit` | `idehbiyen` | `tidehbiyin` | `dhahab` |
| grenat | `aṛemmani` | `taremmanit` | `iṛemmaniyen` | `tiremmaniyin` | `rummān` |
| violet (alt.) | `abanafsaji` | `tabanafsajit` | `ibanafsajiyen` | `tibanafsajiyin` | `banafsaj` |

### 4. Attacher une couleur à un nom

Deux constructions sont attestées :

**Attributive** — l'adjectif de couleur suit le nom et s'accorde avec lui en genre, nombre et état :

```
axxam amellal      « une/la maison blanche »   (axxam : masc. sg., état libre)
tamdint tazegzawt  « une/la ville verte »      (tamdint : fém. sg., état libre)
```

**Prédicative**, utilisant la particule invariante `d` (« étant / est ») entre le nom et la couleur :

```
Γuṛ-i amcic d amellal.   « J'ai un chat blanc. »   (litt. « à-moi chat est blanc »)
```

`d` elle-même ne se fléchit pas ; seul l'adjectif de couleur s'accorde avec le nom qu'il décrit.

### 5. La couleur comme nom

Les termes de couleur peuvent fonctionner comme des noms signifiant « le blanc », « le rouge », etc. Dans cet usage, ils prennent le paradigme d'accord nominal complet, y compris l'état et l'inflection prépositionnelle :

```
Amellal yelha.              « Le blanc est beau. »
Fki-yi-d umellal.           « Donne-moi le blanc. » (annexion après verbe impératif)
```

Utilisés comme noms, les termes de couleur féminins désignent souvent des animaux ou des objets dont le type est récupérable dans le contexte :

```
Tazegzawt                   « la verte » (f.) — aussi le nom de la race ovine kabyle
```

Les implémenteurs doivent distinguer l'usage nominal (une entrée lexicale pour le sens nominal) de l'usage adjectival (accord avec un nom tête), car les cibles de lemmatisation diffèrent.

### 6. Notes d'orthographe

- La fricative uvulaire de `awraɣ`/`azeggaɣ` s'écrit `ɣ` (gamma latin) dans l'orthographe latine standard ; certaines sources substituent le visuellement similaire `γ` (gamma grec) ou l'informel `gh`. Choisissez un glyphe et utilisez-le de manière cohérente ; `ɣ` est recommandé car c'est la lettre standard IRCAM/latin-kabyle.
- Les consonnes emphatiques (`ẓ`, `ṛ`, `ṭ`, etc.) sont des lettres distinctes, pas des variantes décorées de `z`, `r`, `t` — ne supprimez pas les diacritiques.
- Cette spécification ne redérive pas l'inventaire complet des lettres kabyles ; une future spécification d'orthographe devra être la référence canonique et ce document devra y pointer une fois publiée.

### 7. Hors périmètre : formes verbales (inchoatives)

Le kabyle dispose aussi de formes verbales signifiant « être/devenir [couleur] », distinctes des adjectifs ci-dessus, par ex. `imlul` (« être/devenir blanc »), `ibrik` (« être/devenir noir »), `izwiɣ` (« être/devenir rouge »), `zegzew` (« être/devenir bleu-vert »), `iwriɣ` (« être/devenir jaune »). Celles-ci relèvent d'une spécification de morphologie verbale, pas de celle-ci, et sont listées ici uniquement pour que les implémenteurs sachent qu'elles existent et ne les confondent pas avec les adjectifs.

## Barrières de qualité

Pour les implémenteurs construisant des validateurs, des dictionnaires ou des logiciels éducatifs, les contrôles suivants sont recommandés :

| Barrière | Règle | Sévérité |
|---|---|---|
| `GENDER_AGREEMENT` | Le féminin d'une couleur doit être `ta-` + radical + `-t`. | erreur |
| `STATE_ALTERNATION` | L'état d'annexion du masculin en `a-` doit utiliser `u-` (pas `a-`). | erreur |
| `FEMININE_STATE_DROP` | L'état d'annexion du féminin en `ta-` doit perdre la voyelle initiale : `ta-…-t` → `t-…-t`. | erreur |
| `LOAN_INTEGRATION` | Les emprunts arabe/français non intégrés dans un texte formel doivent être signalés. | avertissement |
| `PLURAL_FORM` | Accepter les pluriels réguliers (`i-…-en` / `ti-…-in`) et les pluriels irréguliers attestés ; lemmatiser vers la forme masculine singulière libre. | info |
| `POSITION` | L'adjectif de couleur doit suivre le nom, pas le précéder. | erreur |
| `HOMONYM_CHECK` | `arẓaẓ` ne doit pas être lemmatisé vers « violet » sans contexte (désambiguïser de « guêpe »). | avertissement |
| `CONVENTION_DECLARATION` | Le jeu de données doit déclarer s'il utilise le bleu/vert unifié (`azegzaw` seul) ou séparé (`azegzaw` + `aẓeṛqaq`). | info |
| `NOT_A_COLOR` | `acebḥan` ne doit pas être admis dans un lexique de couleur comme synonyme de blanc. | erreur |
| `NUANCE_AGREEMENT` | Le modificateur de nuance (`acemlax`, `aqesḥan`, `acamlal`) doit suivre la couleur et s'accorder en genre/nombre avec le nom tête. | erreur |
| `NATIVE_CANONICAL` | Le tableau §2 ne doit contenir que des termes natifs berbères ; les emprunts arabes doivent être relégués au §3.11. | erreur |
| `NUANCE_PREDICTABLE` | Les formes fléchies de `acamlal` sont prédictibles mais non confirmées par locuteur natif ; les validateurs doivent les accepter avec une note. | avertissement |

## Questions ouvertes

- La racine `iɣed`/`iɣd` (« cendre ») sous-jacente à `aɣiɣdan` (§3.5) n'a pas pu être confirmée indépendamment comme mot kabyle autonome dans les sources consultées — il vaut la peine de vérifier dans un dictionnaire (par ex. Dallet 1982) avant de traiter l'étymologie comme établie plutôt que plausible.
- Les formes plurielles de `amelliɣdi` (gris) ne sont toujours pas confirmées dans les sources consultées — remplacé par `aɣiɣdan` dans le tableau canonique.
- Le bleu/vert devrait-il constituer une seule ligne de table avec une étiquette dialectale, ou deux lignes séparées ? Nécessite une décision, pas seulement une documentation (voir §3.1).
- Pour le violet et l'orange, le choix entre synonymes (`arẓaẓ`/`azenǧari` ; `aṛanǧi`/`ačini`) est-il réellement régional, ou simplement une variation libre/générationnelle (emprunt ancien vs. emprunt récent) ? Il vaut la peine de vérifier dans un atlas dialectal ou en interrogeant des locuteurs de plusieurs régions avant de le traiter comme une coupure formelle comme le font les §3.1 et §3.2.
- L'usage bleu/vert-neutre de `aẓeṛqaq` s'étend-il au-delà de la couleur des yeux (§3.6) à d'autres domaines, ou est-il spécifique aux yeux ? Il vaut la peine de vérifier avant de généraliser la note.
- Le pluriel de `adal` : est-ce `adalen`/`tadalin` (sans changement vocalique) ou `idalen`/`tidalin` (changement vocalique, conforme à la règle générale du §1) ? Deux sources sont en désaccord ; cela nécessite une résolution plutôt qu'un choix silencieux, car cela affecte le traitement de `adal` comme pluriel régulier ou irrégulier. Cette spécification recommande `idalen`/`tidalin` par défaut.
- Un exemple de phrase d'un dictionnaire kabyle-français décrivant les couleurs d'une robe traditionnelle a listé plusieurs termes non encore couverts par cette spécification : `aẓerfan` (« argent »), `agennaw` (« turquoise »), `ademdam` (« violet » — un troisième terme pour le violet, distinct à la fois de `arẓaẓ` et de `azenǧari`/`aǧenǧari` du §3.3), et `amekzay` (« mauve »). Ceux-ci sont hors du périmètre actuel (le §2 ne couvre que les couleurs déjà discutées) mais méritent un passage dédié ultérieurement plutôt que d'être silencieusement absorbés dans des lignes existantes — `ademdam` en particulier signifie que cette spécification pourrait nécessiter un traitement à trois voies, pas deux, du « violet ».
- `qessiḥen` — rapport exact avec `aqesḥan` (participe, variante dialectale, ou terme distinct ?) Nécessite clarification.
- `amidadi` — attesté oralement comme « bleu foncé », non trouvé dans les sources écrites d'autorité (Dallet, Olivier, UMMTO). À vérifier avant intégration.
- Étymologie de `aẓerwal` (bleu clair), `aṣenfari` (jaune très clair), `amuri` (bleu marine) — natifs berbères ou emprunts ? À vérifier.
- Pluriel féminin de `aɣiɣdan` — prédictible (`tiɣiɣdin` ?) mais non attesté par écrit dans la thèse UMMTO.

## Références

- KabyleMag, [« Les couleurs en kabyle »](https://kabylemag.com/les-couleurs-en-kabyle/)
- PolyglotClub, [« Kabyle Vocabulary – Colors »](https://polyglotclub.com/wiki/Language/Kabyle/Vocabulary/Colors)
- Aleph, [« À propos de la couleur dans les expressions idiomatiques kabyles »](https://aleph.edinum.org/15719)
- Wikipedia, [« Kabyle grammar »](https://en.wikipedia.org/wiki/Kabyle_grammar)
- La Dépêche de Kabylie, [« Les changements de sens (2) »](https://depechedekabylie.com/evenement/21437-les-changements-de-sens-2) — coupure sémantique `adal`/`tizgzewt`
- Wiktionnaire (fr), [discussion sur la catégorie Couleurs en kabyle](https://fr.wiktionary.org/wiki/Discussion_cat%C3%A9gorie:Couleurs_en_kabyle) — `adal` glossé comme « vert militaire » ; `atchini` = orange ; `azendjari` = violet
- Wiktionnaire (fr), [entrée « orange », table de traduction](https://fr.wiktionary.org/wiki/orange) — kabyle `aṛanǧi`, `aččini`
- Wiktionnaire (fr), [Catégorie:Couleurs en kabyle](https://fr.wiktionary.org/wiki/Cat%C3%A9gorie:Couleurs_en_kabyle)
- Glosbe / DiKab, [dictionnaire kabyle](https://fr.glosbe.com/fr/kab/)
- Scribd, *Irman n yiniten deg tmaziɣt* (Couleurs en Tamazight et Kabyle)
- Tagounits village blog, [« février 2009 »](http://tagounitsvillage.blogspot.com/2009_02_01_archive.html) — formes plurielles `arẓaẓ`/`tirẓaẓin` et note sur le sens de couleur largement archaïque vs. le sens insecte d'usage courant
- Wiktionary, [أزرق (azraq)](https://en.wiktionary.org/wiki/%D8%A3%D8%B2%D8%B1%D9%82) — arabe « bleu », source de `aẓeṛqaq`
- KALIMAH, [« Colors in Arabic »](https://kalimah-center.com/colors-in-arabic/) — رمادي (*ramadi*, « gris ») comme « de couleur cendre », cité pour le parallèle de motif de dénomination translinguistique avec `amelliɣdi`
- Olivier (A.), *Dictionnaire français-kabyle*, 1878 — `aberqac`, `aberquc`
- Thèse UMMTO, *Traitement géolinguistique des noms des couleurs en kabyle* — `aɣiɣdan`, `aḥcayci`, `awerdi`, `anili`, `adehbi`, `aṛemmani`, `abanafsaji`, `ačinawi`
- Encyclopédie berbère, S. Chaker — `uraɣ` (or)
- ANAMEK / DiKab — `aẓref` (argent)

---

*Cette spécification fait partie du dépôt [kabyle-specs](https://kabyle-specs.github.io/). Elle est publiée sous la même licence que le dépôt.*
