# L'expression de l'heure et du temps en kabyle (Taqbaylit) : système, sources et implémentation

**Auteurs** : Athmane Mokraoui (boffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles ; recherche documentaire, structuration et optimisation des données.

**Date** : 15 août 2026

**Cible** : Grand public, linguistes, développeurs NLP/TAL

---

## Résumé

Le kabyle (Taqbaylit, code ISO 639-1 `kab`) dispose d'un système autonome pour exprimer l'heure et le temps, documenté depuis au moins 1910 dans les grammaires pédagogiques et attesté dans les dictionnaires lexicographiques modernes. Ce document expose la structure grammaticale de ce système, inventorie ses unites (heures, fractions, minutes, dates, saisons), compare les variantes dialectales et propose une standardisation structuree pour le traitement automatique de la langue (NLP/TAL).

**Mots-clés** : kabyle, taqbaylit, heure, temps, date, grammaire, Boulifa, Dallet, Amazit-Hamidchi, Lounaci, NLP, ovos-date-parser

---

## 1. Introduction

Le système horaire kabyle ne se contente pas d'emprunter à l'arabe ou au français ; il possède une structure grammaticale propre, fondée sur la particule d'affirmation **D** (*d*), des conjonctions de coordination (**u**, **ɣir**), et un lexique mixte où coexistent termes amazighs autochtones et emprunts anciens.

Ce document vise à :
1. Décrire la structure grammaticale horaire pour le grand public.
2. Documenter les sources historiques et lexicographiques.
3. Fournir un jeu de données structuré (variantes et traductions multiples) pour l'implémentation informatique et l'entraînement des IA (parseurs de date/heure).

---

## 2. Sources primaires

### 2.1 Boulifa Si A. Said, *Une première année de langue kabyle*, 1910
Ouvrage historique utilisant une orthographe latine de l'époque (alphabet français enrichi de lettres et signes conventionnels pour noter les phonèmes kabyles). La 37e leçon, consacrée à l'heure, atteste l'utilisation ancienne de ce système :
* **nofc** ou **nofc** (demie) : relie par la conjonction *ou* (ex. *d'lah'dach ou nofc*).
* **roba'** (quart) : (ex. *d'erreba'a ou roba'*).
* **r'ir** (moins) : (ex. *d'lkhamsa r'ir rob'a*).
* **eddeq'iq'a** (minute) : (ex. *d'erreba'a r'ir a'chrin eddeq'aiq'*).

### 2.2 Dictionnaires modernes
* **Jean-Marie Dallet, *Dictionnaire kabyle-français* (1982)** : Documente *azgen* (moitié, demi, nom masculin amazigh) et *nnefs* (moitié, emprunt polysémique).
* **Kamel Bouamara, *Issin* (2010)** : Confirme *azgen* comme terme amazigh de référence.
* **Corpus Glosbe** : Atteste les équivalences *azgen*, *nsaf*, *tazgent*.

### 2.3 Assimil, *Le Kabyle de poche*, 2005
**Fadhma Amazit-Hamidchi** et **Mohand Lounaci**, *Le Kabyle de poche*, collection Assimil Évasion, Chennevières-sur-Marne, 2005 (rééd. 2011). Page 120 : leçon « Demander et donner l'heure ». Atteste :
* **nefs** (demie) : *D juǧ u nefs* = « Il est deux heures et demie ».
* **ɣir** (moins) : *D lweḥda ɣir xemsa* = « Il est une heure moins cinq ».
* **u** (et) : *D juǧ u rbeɛ* = « Il est deux heures et quart ».
* **ddqayeq** (minutes) : *ɣir snat ddqayeq* = « moins deux minutes ».
* **wac** (un peu) : *D lweḥda u wac* = « Il est un peu plus d'une heure ».

### 2.4 Unicode CLDR (Common Locale Data Repository)
Le kabyle (`kab`) est intégré dans le Unicode CLDR depuis la version 32 (2017). Le CLDR v48.2 fournit des formats numériques standardisés et des champs lexicaux techniques (`Tamert` pour « heure », `Tamrect` pour « minute », `n tufat` / `n tmeddit` pour les day periods). Cependant, le CLDR ne documente pas le système oral d'expression de l'heure (particule **D**, conjonctions **u** / **ɣir**, fractions **rbeɛ** / **neṣṣ**) qui fait l'objet du présent article. Les données CLDR sont complémentaires : elles standardisent les interfaces informatiques, tandis que les sources grammaticales et lexicographiques (Boulifa 1910, Dallet 1982, Assimil 2005) et les attestations natives documentent la parole réelle.

**Note importante sur le CLDR** : Certaines versions anciennes du CLDR ont introduit des formes artificielles et erronées pour les jours de la semaine (`Yanass`, `Sanass`, `Kraḍass`, `Kuẓass`, `Samass`, `Sḍisass`, `Sayass`). Il s'agit de néologismes numéraux artificiels non attestés dans l'usage locuteur kabyle. Les formes correctes sont documentées en section 10 de ce document.

---

## 3. Structure grammaticale

### 3.1 La particule D (Présentatif)
L'heure s'introduit obligatoirement par la particule d'affirmation invariante **D** (« c'est », « il est »).
* **D lweḥda** = Il est une heure (1:00)
* **D lɛecra** = Il est dix heures (10:00)

**Variante avec démonstratif** : Le démonstratif **Ha-tt-an** peut être employé pour désigner explicitement l'heure au féminin (*ssaɛa*).
* **Ha-tt-an d lweḥda** = « Voici qu'il est une heure » / « Il est une heure » (avec mise en relief du moment présent).

### 3.2 Les conjonctions et l'exactitude
* **u** : « et » — marque l'addition (quart, demie, minutes).
* **ɣir** : « moins, sauf » — marque la soustraction.
* **swaswa** : « exactement, juste » — marque l'heure pile. Atteste comme synonyme de *gedged* dans **J.-M. Dallet, *Dictionnaire kabyle-français* (1982)** ; l'usage contemporain du terme est par ailleurs largement confirmé par le corpus de traductions Glosbe/Tatoeba, où *swaswa* traduit systématiquement « exactement » / « précisément ».

### 3.3 Formuler la question (Demander l'heure)
Pour l'entraînement des assistants vocaux et des modèles de langage, il est crucial de reconnaître les formulations interrogatives natives, qui varient du plus direct au plus poli :
* **Acḥal ssaɛa ?** (Quelle heure est-il ?) — La formulation la plus standard et directe.
* **Acḥal ssaɛa, tura ?** (Quelle heure est-il, maintenant ?) — Ajoute un marqueur temporel explicite.
* **D acu-tt tura ?** (Qu'est-ce que c'est, maintenant ?) — Formulation très naturelle et colloquiale, où le suffixe pronominal « -tt » reprend le nom féminin « ssaɛa » sous-entendu.
* **Ttxil, acḥal ssaɛa ?** / **Ttxil-k, acḥal ssaɛa ?** / **Ttxil-m, acḥal ssaɛa ?** (S'il te plaît, quelle heure est-il ?) — Marqueurs de politesse essentiels pour un assistant vocal, avec la distinction neutre, masculin (-k) et féminin (-m).
* **Tzemreḍ ad iyi-d-iniḍ acḥal d ssaɛa ?** (Peux-tu me dire quelle heure il est ?) — Formulation interrogative indirecte, plus élaborée.

---

## 4. Le lexique horaire et l'expression des minutes

Les heures pleines utilisent la particule **D** suivie du nombre (ex. *D kuẓ* pour 4:00, *D ttnac* pour 12:00). Le mot « heure » (*ssaɛa*) est sous-entendu. Les fractions et les minutes exploitent une richesse lexicale allant de l'emprunt courant à la création académique.

### 4.1 Le quart et la demie
* **Le quart** : *rbeɛ* (usage moderne), *roba'* (orthographe latine de l'époque, Boulifa 1910).
* **La demie** : Ce créneau présente un doublet linguistique et une chaîne évolutive documentée :
  * *azgen* : Amazigh autochtone (Dallet 1982, Bouamara 2010).
  * *nofc* ou *nofc* : Emprunt arabe ancien, forme notée dans l'orthographe latine de l'époque (Boulifa 1910).
  * *nefs* : Forme intermédiaire simplifiée (Assimil 2005).
  * *neṣṣ* / *nefṣ* : Formes contemporaines avec spirantisation (attestation native, Mokraoui 2026).

### 4.2 L'indefini approximatif
Pour exprimer une heure approximative (« une heure et quelques », « un peu plus d'une heure »), le kabyle utilise **u wac** ou **u ci** (littéralement « et un peu »).
* **D lweḥda u wac** = « Il est une heure et quelques » / « Un peu plus d'une heure » (ex. ~1:05–1:15)
* **D lweḥda u ci** = « Il est une heure et quelque chose » / « Il est une heure passée » (variante régionale)
* **D ttnac u wac** = « Il est midi et quelques » / « Un peu plus de midi »

Ces constructions expriment une **indétermination bienveillante** — l'heure n'est pas fixée avec précision, mais située dans un intervalle flou après l'heure pleine.

### 4.3 L'indétermination soustractive
Pour exprimer qu'il manque un peu pour atteindre l'heure pleine, sans préciser la quantité exacte, le kabyle peut employer **ɣir** seul, sans complément numérique.
* **D lɛecra ɣir** = « Il est dix heures moins » / « Il est bientôt dix heures » / « Il manque un peu pour dix heures »

Cette construction elliptique fonctionne comme une **approximation soustractive vague** — l'interlocuteur comprend que l'heure approche de 10:00 sans savoir exactement de combien de minutes il s'agit. C'est l'équivalent kabyle de « il est presque dix heures » ou « dix heures moins quelque chose ».

### 4.4 Variantes régionales pour « deux heures »
Le nombre « deux » présente une variation dialectale notable dans le contexte horaire :
* **juǧ** : Forme d'emprunt (attestée dans Assimil 2005 : *D juǧ u nefs*).
* **ssaɛtin** : Forme propre à certaines régions, notamment la **vallée de la Soummam** (ex. *D ssaɛtin ɣir xemsa* = « Il est deux heures moins cinq »). Dans cette variante, le mot *ssaɛa* (« heure ») est intégré dans le syntagme numéral au pluriel (*ssaɛtin* = « des heures »), ce qui distingue cette région des parlers où le nombre seul suffit.

### 4.5 L'expression de la soustraction (les minutes)
Pour exprimer les minutes avant l'heure pleine (moins 5, moins 10, etc.), le kabyle dispose de plusieurs registres linguistiques. Voici les 5 meilleures traductions pour chaque intervalle afin de couvrir tous les usages (du plus familier au plus académique) :

**« Moins 5 » (Exemple : 9h55 / Dix heures moins cinq)**
1. **D lɛecra ɣir xemsa** (Usage courant oral, emprunt)
2. **D lɛecra ɣir xemsa n ddqayeq** (Usage courant avec précision de l'unité)
3. **D lɛecra ɣir semmus** (Variante purement amazighe)
4. **D lɛecra ɣir xemsa n tesdidin** (Variante hybride standard NLP)
5. **D lɛecra ɣir semmus n tesdidin** (Variante académique complète)

**« Moins 10 » (Exemple : 9h50 / Dix heures moins dix)**
1. **D lɛecra ɣir ɛecra** (Usage courant oral, emprunt)
2. **D lɛecra ɣir ɛecra n ddqayeq** (Usage courant avec précision de l'unité)
3. **D lɛecra ɣir mraw** (Variante purement amazighe)
4. **D lɛecra ɣir ɛecra n tesdidin** (Variante hybride standard NLP)
5. **D lɛecra ɣir mraw n tesdidin** (Variante académique complète)

**« Moins 20 » (Exemple : 9h40 / Dix heures moins vingt)**
1. **D lɛecra ɣir ɛecrin** (Usage courant oral, emprunt)
2. **D lɛecra ɣir ɛecrin n ddqayeq** (Usage courant avec précision de l'unité)
3. **D lɛecra ɣir snat tmerwin** (Variante purement amazighe)
4. **D lɛecra ɣir ɛecrin n tesdidin** (Variante hybride standard NLP)
5. **D lɛecra ɣir snat tmerwin n tesdidin** (Variante académique complète)

**« Moins 25 » (Exemple : 9h35 / Dix heures moins vingt-cinq)**
1. **D lɛecra ɣir xemsa u ɛecrin** (Usage courant oral, emprunt)
2. **D lɛecra ɣir xemsa u ɛecrin n ddqayeq** (Usage courant avec précision de l'unité)
3. **D lɛecra ɣir semmus d snat tmerwin** (Variante purement amazighe)
4. **D lɛecra ɣir xemsa u ɛecrin n tesdidin** (Variante hybride standard NLP)
5. **D lɛecra ɣir semmus d snat tmerwin n tesdidin** (Variante académique complète)

### 4.6 L'expression additive des minutes (1 à 59)
Pour exprimer les minutes après l'heure pleine, le kabyle suit une structure morphologique stricte : **`D [heure] + u + [minutes] + n ddqayeq`**.
La formation des nombres de 11 à 59 obéit à des règles précises que le parseur doit connaître :

1. **Accord féminin** : Le mot minute (*ddqiqa*) étant féminin, les unités prennent la forme féminine (*yiwet, snat, tlata, tmanya, tesɛa*, etc.).
2. **11 et 12** : Formes uniques, non composees (*hḍac*, *tnac*).
3. **13 à 19** : Formes fusionnées (portemanteaux) en *-ṭṭac* (*telṭac*, *rbeɛṭac*, *xemseṭṭac*, *seṭṭac*, *sbeɛṭac*, *tmenṭac*, *tseɛṭac*).
4. **20 à 59** : Structure inversée **[unité] + u + [dizaine]**. L'unité précède la dizaine, reliée par la conjonction *u* (et).
   - 21 min = *waḥed u ɛecrin* (et non *ɛecrin d waḥed*)
   - 32 min = *tnayn u tlatin*
   - 45 min = *xemsa u rebɛin*
   - 59 min = *tesɛa u xemsin*

**Exemple complet** : 13h21 se dira **D lweḥda u waḥed u ɛecrin n ddqayeq n uzal**.

### 4.7 Lecture des heures numériques (Cas TTS)
Lorsqu'une interface affiche une heure numérique (ex: `13:04`), deux modes de lecture coexistent en kabyle :

1. **Lecture naturelle (recommandée)** : Conversion en cycle de 12h avec la grammaire orale. -> *D lweḥda u rebɛa n ddqayeq n uzal*.
2. **Lecture chiffre par chiffre (digit-by-digit)** : Parfois utilisée pour les horaires de transport ou les contextes très formels. -> *Yiwen, tlata, u rebɛa* (1, 3, et 4) ou *Telṭac u rebɛa* (treize et quatre).

*Recommandation NLP* : Privilégier systématiquement la **lecture naturelle** (mode 1) pour la synthèse vocale (TTS), car elle correspond à la parole réelle et évite les sorties robotiques.

---

## 5. Les parties du jour (Marqueurs AM / PM)

Le kabyle distingue les périodes de la journée par des termes spécifiques qui fonctionnent comme des marqueurs AM/PM. La structure standard utilise **n** (génitif) pour lier l'heure à la période :

| Période | Terme | Exemple | Plage approximative |
|---------|-------|---------|---------------------|
| **Aube / petit matin** | **ṣṣbeḥ** | *Af ttlata n ṣṣbeḥ* | 4h–8h |
| **Matin** | **ssbeḥ** | *D ttnac n ssbeḥ* | 8h–12h |
| **Midi (lumière intense)** | **uzal** | *D ttnac n uzal* | 12h–14h (lumière solaire maximale) |
| **Après-midi / soir** | **tameddit** | *D lxemsa n tmeddit* | 15h–19h |
| **Nuit** | **iḍ** | *D ttnac n yiḍ* | 20h–4h |

### Notes sur les marqueurs de période

* **ṣṣbeḥ** (avec redoublement de ṣ) désigne le **petit matin**, les premières heures du jour (approximativement 4h–8h). L'expression *Af ttlata n ṣṣbeḥ* (« à trois du petit matin ») est courante pour les heures nocturnes/matinales précoces.

* Désigne également le **matin** proprement dit (approximativement 8h–12h). *D ttnac n ṣṣbeḥ* = « il est douze du matin » = midi (12:00 PM).

* **uzal** désigne la **période où la lumière du soleil est la plus intense**, approximativement de **12h à 14h**. Ce terme ne se limite pas à l'instant de midi : il couvre les heures où le soleil est au zénith. *D ttnac n uzal* = « il est douze de la pleine lumière » = 12:00 pile. On peut aussi dire *D juǧ n uzal* pour 14:00 (deux heures de l'après-midi intense). À partir de **15h**, on passe systématiquement à **tameddit**.

* **tameddit** couvre l'**après-midi et le début de soirée** (approximativement 15h–19h). Le seuil de transition est marqué : dès que la lumière solaire commence à décliner significativement (après 14h–15h), on quitte *uzal* pour *tameddit*. *D lxemsa n tmeddit* = « il est cinq de l'après-midi » = 17:00.

* **iḍ** désigne la **nuit** (approximativement 20h–4h). *D ttnac n yiḍ* = « il est douze de la nuit » = minuit.

### 5.1 Le milieu de la nuit : variantes régionales

Pour désigner le milieu de la nuit (minuit ou aux alentours), certaines régions utilisent des expressions basées sur **neṣṣ** (demie) ou sa variante féminine **ttnaṣfa** :

* **Nṣaf n yiḍ** = « Demie de la nuit » = minuit (variante régionale)
* **Ttnaṣfa n yiḍ** = « La demie de la nuit » = minuit (autre variante régionale)

Ces expressions parallélisent la structure *D ttnac n yiḍ* (« douze de la nuit ») mais remplacent le nombre par la fraction, soulignant que l'on se situe au **milieu** de la période nocturne plutôt qu'à une heure précise du calendrier.

### 5.2 Azal azaylal — La torpeur de la pleine chaleur

* **Définition** : Moment de la journée (vers 10h30–11h30) où la chaleur et la lumière solaire sont à leur apogée, engendrant une torpeur physiologique.
* **Étymologie** : *azal* (clarté du jour, pleine chaleur) + *azaylal* (stupeur, somnolence).
* **Usage culturel** : Dans la vie pastorale traditionnelle, ce moment imposait une pause (bergers, agriculteurs).
* **Attestation** : Encyclopédie berbère, article « Temps » : « Aux alentours de onze heures... les bergers et leur troupeau cherchent l'ombre et le repos. »
* **Statut NLP** : Expression temporelle floue. À traiter comme un intervalle approximatif `[10:30, 11:30]` ou comme un événement culturel (`EVENT`), selon le contexte d'implémentation.

---

## 6. Les durées et le temps écoulé

Pour exprimer qu'une certaine quantité de temps s'est écoulée, le kabyle utilise le verbe **yekka** (« il a passé, il a consommé ») suivi du nombre d'heures.

* **Yekka kraḍ n swayeɛ** = « Il a passé trois heures » / « Trois heures se sont écoulées »
* **Yekka snat n swayeɛ** = « Deux heures se sont écoulées »

**Note sur la syntaxe** : Après le verbe *yekka*, le nombre « deux » prend la forme féminine **snat** (et non *juǧ*), et le mot « heure » apparaît au pluriel **swayeɛ** (et non *ssaɛtin*). La construction utilise la préposition **n** (génitif) : *kraḍ n swayeɛ* = « trois d'heures » = « trois heures [de temps] ».

Cette structure diffère de l'expression horaire ponctuelle (où le nombre seul suffit : *D juǧ u rbeɛ* = « Il est deux heures et quart »).

---

## 7. Jours de la semaine

**Correction importante** : Les formes `Yanass`, `Sanass`, `Kraḍass`, `Kuẓass`, `Samass`, `Sḍisass`, `Sayass` présentes dans certaines versions anciennes de CLDR sont erronées. Ce sont des néologismes numéraux artificiels (ex. `Kraḍ-ass` = « trois-jour ») non attestés en kabyle. Elles ne figurent dans aucun dictionnaire, grammaire ou cours de kabyle. Les formes ci-dessous sont les seules validées par l'usage locuteur.

### 7.1 Formes usuelles (emprunts arabes)

C'est la série utilisée naturellement dans la parole courante en Kabylie. Les locuteurs emploient ces termes dans la vie quotidienne sans marqueur d'emprunt.

| Jour | Kabyle (usage courant) | Abrégé |
|------|------------------------|--------|
| Lundi | **letnayen** | let |
| Mardi | **ttlata** | ttl |
| Mercredi | **larebɛa** | lar |
| Jeudi | **lexmis** | lex |
| Vendredi | **lǧemɛa** | lǧe |
| Samedi | **ssebt** | sse |
| Dimanche | **lḥedd** | lḥe |

**Attestation** : *Apprendre le kabyle* (cours de référence) : « les jours de la semaine en kabyle sont empruntés à la langue arabe, en Kabylie on continue toujours à utiliser les emprunts ».

### 7.2 Néologismes berbères (Académie berbère)

Ces formes ont été créées par l'**Académie berbère** pour « réparer » l'absence de termes autochtones. Elles sont utilisées dans les calendriers militants et certaines publications culturelles. Elles ne sont pas usuelles dans la parole spontanée.

| Jour | Néologisme berbère | Abrégé |
|------|-------------------|--------|
| Lundi | **Arim** | Ari |
| Mardi | **Aram** | Ara |
| Mercredi | **Ahad** | Aha |
| Jeudi | **Amhad** | Amh |
| Vendredi | **Sem** | Sem |
| Samedi | **Sed** | Sed |
| Dimanche | **Acer** | Ace |

**Attestation** : Wikipedia — *Calendrier berbère*, section « Néologisme et traditions » ; Académie berbère.

### 7.3 Recommandation pour l'implémentation

| Fonction | Forme par défaut | Formes alternatives |
|----------|-----------------|---------------------|
| `nice_weekday` | **Emprunts arabes** (letnayen, ttlata...) | Néologismes berbères (Arim, Aram...) |
| `extract_datetime` | Reconnaître les deux séries | — |
| `nice_date` | Emprunts arabes | Néologismes berbères (option `variant`) |

---

## 8. Mois de l'année

Les noms des mois en kabyle sont hérités du calendrier berbère traditionnel, alignés sur le calendrier grégorien. Ils sont en usage dans l'administration, l'école et la vie quotidienne.

| Mois grégorien | Kabyle (complet) | Abrégé | Narrow |
|----------------|-----------------|--------|--------|
| Janvier | **Yennayer** | Yen | Y |
| Février | **Furar** | Fur | F |
| Mars | **Meɣres** | Meɣ | M |
| Avril | **Yebrir** | Yeb | Y |
| Mai | **Mayyu** | May | M |
| Juin | **Yunyu** | Yun | Y |
| Juillet | **Yulyu(z)** | Yul | Y |
| Août | **Ɣuct** | Ɣuc | Ɣ |
| Septembre | **Ctember** | Cte | C |
| Octobre | **Tuber** | Tub | T |
| Novembre | **Wamber** | Wam | W |
| Décembre | **Duǧember** | Duğ | D |

**Variante kabyle** : `Buǧember` (Décembre, forme alternative usuelle).

**Attestation** : Wikipedia — *Calendrier berbère* (tableau des variations linguistiques, colonne Kabyle) ; CLDR locale `kab` (versions corrigées) ; ScriptSource — Entry « Names of Months and Days in Kabyle ».

---

## 9. Formats de date

### 9.1 Formats grégoriens standardisés

Conformément aux usages kabyles et à la locale CLDR `kab` (corrigée), les formats suivants sont recommandés pour l'affichage et le parsing :

| Style | Pattern | Exemple |
|-------|---------|---------|
| Full | `EEEE d MMMM y` | `ttlata 5 Furar 2008` |
| Long | `d MMMM y` | `5 Furar 2008` |
| Medium | `d MMM, y` | `5 Fur, 2008` |
| Short | `d/M/y` | `5/2/2008` |

**Règles de formatage** :
- L'ordre des éléments est **jour / mois / année** (JJ/MM/AAAA) dans le format numérique court.
- Le mois est exprimé par son nom berbère, pas par son équivalent arabe ou français.
- L'année s'écrit sur 4 chiffres dans les formats longs.

### 9.2 Formats temporels combinés (date + heure)

| Style | Pattern | Exemple |
|-------|---------|---------|
| Full | `EEEE d MMMM y HH:mm:ss zzzz` | `ttlata 5 Furar 2008 18:30:30 UTC` |
| Long | `d MMMM y HH:mm:ss zzzz` | `5 Furar 2008 18:30:30 UTC` |
| Medium | `d MMM, y HH:mm:ss` | `5 Fur, 2008 18:30:30` |
| Short | `d/M/y HH:mm` | `5/2/2008 18:30` |

---

## 10. Expressions temporelles relatives

Ces expressions sont essentielles pour le parsing NLP (extraction de dates à partir de texte conversationnel).

### 10.1 Références au jour

| Expression | Sens | Exemple d'usage | Source |
|------------|------|-----------------|--------|
| **ass-a**  | aujourd'hui | *Ass-a yelha lhal.* | Glosbe, cours kabyle, these Kessai 2019 |
| **azekka** | demain | *Azekka ad d-uğaleɣ.* | Glosbe, cours kabyle, these Kessai 2019 |
| **iḍelli** | hier | *Iḍelli, ur d-ufiɣ ara.* | Glosbe, cours kabyle |
| **tura** | maintenant | *Bğiɣ adebsi tura.* | Cours kabyle |
| **ticki** | plus tard | *Ad d-aseɣ ticki.* | Cours kabyle |
| **zik** | tôt / autrefois | *Yekker zik.* | Cours kabyle |

### 10.2 Références à la période du jour

| Expression | Sens | Plage approximative | Source |
|------------|------|---------------------|--------|
| **taṣebḥit** | matinée | 6h–10h | Cours kabyle (taṣbeht = matinée) |
| **tameddit** | après-midi / soir | 15h–19h | Document actuel + Dallet 1982 |

### 10.3 Références à la semaine

| Expression | Sens | Source |
|------------|------|--------|
| **d lǧemɛa** | vendredi (jour de la semaine) | Usage courant |
| **d lǧemɛa i d-iteddun** | vendredi prochain | Construction analytique |
| **d lǧemɛa iɛeddan** | vendredi dernier | Construction analytique |

---

## 11. Saisons et calendrier traditionnel

### 11.1 Les saisons

| Saison | Nom kabyle | Période grégorienne approximative | Source |
|--------|-----------|-----------------------------------|--------|
| Hiver | **Tagrest** | Déc–Janv–Fév | Wikipedia — Calendrier berbère |
| Printemps | **Tafsut** | Mars–Avr–Mai | Wikipedia — Calendrier berbère |
| Été | **Anebdu** | Juin–Juil–Août | Wikipedia — Calendrier berbère |
| Automne | **Lexṛif** | Sept–Oct–Nov | Wikipedia — Calendrier berbère |

### 11.2 Le calendrier berbère traditionnel

**Note culturelle** : Le calendrier berbère agricole traditionnel dérive du calendrier julien. Il comporte un **décalage de 12 jours** avec le calendrier grégorien.

Le nouvel an berbère (**Yennayer**) correspond au **~12 janvier grégorien**

**Source** : Wikipedia — *Calendrier berbère*, section « Opération ».

| Événement | Date grégorienne | Nom kabyle | Source |
|-----------|-----------------|------------|--------|
| Nouvel an berbère | ~12 janvier | **Yennayer** / **Ixf useggwas** | Wikipedia — Calendrier berbère |
| Nuits noires (période froide) | 25 déc – 13 janv | **Uḍan iberkanen** | Wikipedia — Calendrier berbère |
| Nuits blanches | 14 janv – 2 fév | **Uḍan imellalen** | Wikipedia — Calendrier berbère |

---

## 12. Tableau récapitulatif pour les parseurs (NLP)

| Heure / Date | Expression kabyle normalisée | Structure syntaxique | Type NLP |
|--------------|------------------------------|----------------------|----------|
| 10:00 | D lɛecra | `D` + `[nombre]` | `TIME_EXACT` |
| 10:00 | D lɛecra swaswa | `D` + `[nombre]` + `swaswa` | `TIME_EXACT` |
| ~10:05–10:15 | D lɛecra u wac | `D` + `[nombre]` + `u` + `wac` | `TIME_APPROX` |
| ~10:05–10:15 | D lɛecra u ci | `D` + `[nombre]` + `u` + `ci` | `TIME_APPROX` |
| ~10:45–10:55 | D lɛecra ɣir | `D` + `[nombre]` + `ɣir` (elliptique) | `TIME_APPROX` |
| 10:15 | D lɛecra u rbeɛ | `D` + `[nombre]` + `u` + `rbeɛ` | `TIME_EXACT` |
| 10:30 | D lɛecra u neṣṣ | `D` + `[nombre]` + `u` + `neṣṣ` | `TIME_EXACT` |
| 10:45 | D lɛecra ɣir rbeɛ | `D` + `[nombre]` + `ɣir` + `rbeɛ` | `TIME_EXACT` |
| 9:55 | D lɛecra ɣir xemsa | `D` + `[nombre]` + `ɣir` + `xemsa` | `TIME_EXACT` |
| 03:00 | Af ttlata n ṣṣbeḥ | `Af` + `[nombre]` + `n` + `ṣṣbeḥ` | `TIME_EXACT` |
| 12:00 | D ttnac n uzal | `D` + `[nombre]` + `n` + `uzal` | `TIME_EXACT` |
| 14:00 | D juǧ n uzal | `D` + `juǧ` + `n` + `uzal` | `TIME_EXACT` |
| 17:00 | D lxemsa n tmeddit | `D` + `[nombre]` + `n` + `tameddit` | `TIME_EXACT` |
| 00:00 | D ttnac n yiḍ | `D` + `[nombre]` + `n` + `iḍ` | `TIME_EXACT` |
| 00:00 | Nṣaf n yiḍ | `Nṣaf` + `n` + `iḍ` (variante régionale) | `TIME_EXACT` |
| 00:00 | Ttnaṣfa n yiḍ | `Ttnaṣfa` + `n` + `iḍ` (variante régionale) | `TIME_EXACT` |
| ~10:30–11:30 | Azal azaylal | `[nom] + [nom]` | `TIME_INTERVAL` / `EVENT` |
| Aujourd'hui | Ass-a / Assa | `[nom] + [dem]` | `DATE_RELATIVE` |
| Demain | Azekka | `[adv]` | `DATE_RELATIVE` |
| Hier | Iḍelli | `[adv]` | `DATE_RELATIVE` |
| Maintenant | Tura | `[adv]` | `DATE_RELATIVE` |
| Plus tard | Ticki | `[adv]` | `DATE_RELATIVE` |
| Durée | Yekka kraḍ n swayeɛ | `Yekka` + `[nombre]` + `n` + `swayeɛ` | `DURATION` |

---

## 13. Recommandations pour le développement de l'outil (ovos-date-parser)

Le module actuel `dates_kab.py` génère des heures numériques strictes et manque de flexibilité pour la reconnaissance vocale ou textuelle naturelle. Pour rendre l'outil robuste, il est impératif de :

1. **Intégrer les dictionnaires de synonymes (Synsets)** dans la phase d'extraction pour capter la variabilité dialectale et diachronique :
   * `HALF_PAST = {"neṣṣ", "nefṣ", "nefs", "azgen", "nofc", "nofç", "nnefs", "nsaf"}`
   * `QUARTER = {"rbeɛ", "roba'"}`
   * `MINUS = {"ɣir", "ɣir", "r'ir"}`
   * `EXACT = {"swaswa", "gedged"}`
   * `APPROX = {"wac", "ci"}`
   * `MINUTES = {"ddqiqa", "ddqayeq", "tesdidin", "tisdidin", "eddeq'iq'a"}`
   * `TWO_HOURS = {"juǧ", "sin", "ssaɛtin"}` (variante régionale Soummam)

2. **Intégrer les marqueurs de période** pour la désambiguïsation AM/PM :
   * `DAY_PERIODS = {"ṣṣbeḥ", "ssbeḥ", "uzal", "tameddit", "iḍ"}`
   * `PREPOSITION_TIME = {"n"}` (genitif : *D lxemsa n tmeddit*)
   * `PREPOSITION_AT = {"af"}` (*Af ttlata n ṣṣbeḥ*)

3. **Intégrer les durées** pour l'extraction de laps de temps :
   * `DURATION_VERB = {"yekka"}`
   * `HOURS_PLURAL = {"swayeɛ"}` (duree ecoulee)

4. **Intégrer les variantes du milieu de nuit** :
   * `MIDNIGHT_ALT = {"nṣaf n yiḍ", "ttnaṣfa n yiḍ"}`

5. **Intégrer les expressions temporelles relatives** :
   * `RELATIVE_DATES = {"ass-a", "azekka", "iḍelli", "tura", "ticki", "zik"}`

6. **Intégrer les jours et mois** :
   * `WEEKDAYS = {"letnayen", "ttlata", "larebɛa", "lexmis", "lğemɛa", "ssebt", "lḥedd"}`
   * `WEEKDAYS_ALT = {"Arim", "Aram", "Ahad", "Amhad", "Sem", "Sed", "Acer"}`
   * `MONTHS = {"Yennayer", "Furar", "Meɣres", "Yebrir", "Mayyu", "Yunyu", "Yulyu", "Ɣuct", "Ctember", "Tuber", "Wamber", "Duğember"}`

7. **Désambiguïser le format 12h/24h — piège « D juǧ »** :
   * La forme *D juǧ* (« il est deux heures ») est ambiguë entre 2h et 14h. Seule la présence explicite d'un marqueur de période (*n uzal*, *n tmeddit*, etc.) permet de trancher : *D juǧ n uzal* = 14:00, alors que *D juǧ* seul doit être interprété par défaut comme 2h (format 12h).
   * Règle pratique pour le parseur : en l'absence de marqueur de période explicite, toujours résoudre vers le format 12h ; ne convertir vers le format 24h que si un marqueur de période est présent dans l'énoncé.

8. **Gérer la syntaxe directionnelle** :
   * Modèle additif : `D [heure] u [fraction/minutes]` -> Addition au temps T.
   * Modèle soustractif : `D [heure+1] ɣir [fraction/minutes]` -> Soustraction au temps T+1.
   * Modèle approximatif additif : `D [heure] u wac` / `D [heure] u ci` -> Heure T + indéfini (~5–15 min).
   * Modèle approximatif soustractif : `D [heure] ɣir` -> Heure T - indéfini (~5–15 min avant T).
   * Modèle période : `D [heure] n [période]` -> Heure T dans la période P.
   * Modèle milieu de nuit : `[nṣaf/ttnaṣfa] n yiḍ` -> Minuit (variante régionale).
   * Modèle durée : `[verbe] [nombre] n swayeɛ` -> Durée écoulée.
   * Variante régionale : `D ssaɛtin ɣir [minutes]` -> Deux heures moins minutes (Soummam).
   * Modèle date relative : `[ass-a/azekka/iḍelli]` -> Date relative au jour présent.

9. **Standardiser la sortie textuelle (TTS)** vers une forme canonique unique (par exemple privilégier *neṣṣ* pour la demie et *rbeɛ* pour le quart) tout en restant capable de lire toutes les entrées utilisateur en phase d'analyse (STT). La conversion 24h -> 12h naturelle est obligatoire pour la sortie parlée.

---

## 14. Conclusion

Le système kabyle de l'heure est une construction grammaticale autonome et structurée, documentée depuis plus d'un siècle et vivant encore dans l'usage contemporain. Sa complexité pour l'intelligence artificielle réside dans sa richesse synonymique et ses variantes dialectales. En cartographiant précisément ces nuances — de l'amazigh pur (*azgen*) aux emprunts arabes anciens (*nofc*, *nofç*, *nefs*, *neṣṣ*) en passant par les créations académiques modernes — ce document fournit la base nécessaire pour créer des outils NLP inclusifs, performants et respectueux de la réalité linguistique de la langue kabyle.

Ce document couvre désormais non seulement l'expression de l'heure, mais aussi l'expression des dates (jours, mois, formats), des durées, des expressions temporelles relatives et des moments solaires culturels. Il constitue une référence complète pour l'implémentation du kabyle dans les logiciels de traitement automatique du langage.

---

## 15. Références

1. **Boulifa Si A. Said**, *Une première année de langue kabyle (dialecte zouaoua) à l'usage des candidats à la prime et au brevet de kabyle*, Adolphe Jourdan, Alger, **1910**. Numérisation : [Médiathèque MMSH-Aix](https://cinumedpub.mmsh.fr/Imprimes_manuel-langue/FR_MMSH_MDQ_LANG_MG_033/files/assets/common/downloads/FR_MMSH_MDQ_LANG_MG_033.pdf)

2. **Jean-Marie Dallet**, *Dictionnaire kabyle-français (parler des At Mengellat, Algérie)*, SELAF, Paris, **1982**. Source également des entrées *azgen*, *nnefs* et *swaswa* (syn. *gedged*, « exactement, juste »).

3. **Kamel Bouamara**, *Issin : Asegzawal n teqbaylit s teqbaylit*, Éditions l'Odyssée, Tizi-Ouzou, **2010**.

4. **Glosbe / Tatoeba**, corpus de traductions bilingues français–kabyle, [fr.glosbe.com](https://fr.glosbe.com). Confirme notamment l'usage contemporain de *swaswa* (« exactement, précisément ») dans des phrases attestées hors contexte horaire, en cohérence avec l'emploi décrit en section 3.2.

5. **Fadhma Amazit-Hamidchi** & **Mohand Lounaci**, *Le Kabyle de poche*, collection Assimil Évasion, Assimil, Chennevières-sur-Marne, **2005** (rééd. 2011). Leçon « Demander et donner l'heure » (p. 120). Contenu vérifié par capture d'écran.

6. **Unicode CLDR**, Common Locale Data Repository, version **48.2**, [unicode.org/cldr](https://unicode.org/cldr). Locale `kab` intégrée depuis la version 32 (2017).

7. **Athmane Mokraoui** (boffire), locuteur natif kabyle, contributions au projet **ovos-date-parser**, [github.com/athmanemokraoui/ovos-date-parser](https://github.com/athmanemokraoui/ovos-date-parser), 2024–2026.

8. **Encyclopédie berbère**, article « Temps », [journals.openedition.org/encyclopedieberbere/3865](https://journals.openedition.org/encyclopedieberbere/3865). Attestation de *azal azaylal* et des rythmes temporels de la vie pastorale kabyle.

9. **Wikipedia** — *Calendrier berbère*, [fr.wikipedia.org/wiki/Calendrier_berbere](https://fr.wikipedia.org/wiki/Calendrier_berbere). Tableau des noms de mois (colonne Kabyle), saisons, et calendrier traditionnel.

10. **Kessai Meriem**, *Élaboration d'un dictionnaire électronique de berbère*, thèse de doctorat, Université de Béjaïa, **2019**. Données sur les expressions temporelles relatives (*ass-a*, *azekka*, *dimarday*) et l'écriture segmentée en kabyle.

11. **Apprendre le kabyle** (cours en ligne), [apprendre-le-kabyle.com](https://apprendre-le-kabyle.com). Attestation des jours de la semaine (emprunts arabes) et des expressions usuelles.

12. **Académie berbère**, néologismes des jours de la semaine (Arim, Aram, Ahad, Amhad, Sem, Sed, Acer).

13. **ovos-date-parser** documentation, [github.com/OpenVoiceOS/ovos-date-parser](https://github.com/OpenVoiceOS/ovos-date-parser). État de l'implémentation logicielle pour la locale kabyle.

---

*Document rédigé dans le cadre du développement des ressources NLP pour la langue kabyle. Les attestations natives sont signalées comme telles et n'engagent que leurs auteurs.*
