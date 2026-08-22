# Spécification de la collation kabyle (Taqbaylit) : ordre alphabétique, logique et implémentation

**Auteur principal** : Athmane Mokraoui (butterflyoffire), locuteur natif kabyle, mainteneur des ressources NLP kabyles.

**Date** : 22 août 2026

**Statut** : draft

**Cible** : grand public, linguistes, développeurs FLOSS, mainteneurs de locales, bases de données, moteurs de recherche.

---

## Résumé

Le kabyle (Taqbaylit, code langue `kab`) utilise aujourd’hui un alphabet latin normalisé de **33 lettres**. Cette spécification définit l’ordre alphabétique à utiliser pour la **collation**, c’est-à-dire le tri des mots dans les dictionnaires, annuaires, systèmes de fichiers, bases de données, interfaces logicielles et outils de traitement automatique du langage.

L’objectif est double :

1. permettre à un utilisateur de voir et comprendre rapidement l’ordre alphabétique kabyle ;
2. permettre à un développeur d’implémenter correctement le tri, sans appliquer par erreur une logique française, anglaise ou pan-berbère non adaptée à l’usage kabyle.

**Mots-clés** : kabyle, taqbaylit, collation, ordre alphabétique, tri, locale, `kab_DZ`, ICU, CLDR, glibc, NLP.

---

## 1. Périmètre

Cette spécification concerne uniquement l’**alphabet latin kabyle** utilisé dans les usages modernes, l’enseignement, les dictionnaires, les corpus NLP et les logiciels.

Le néo-tifinagh n’est pas inclus dans cette version. Il pourra faire l’objet d’une spécification distincte lorsque la recherche et l’usage communautaire le justifieront.

---

## 2. Ordre alphabétique kabyle — vue d’ensemble

L’ordre alphabétique kabyle est le suivant :

```text
A Ɛ B C Č D Ḍ E F G Ǧ H Ḥ I J K L M N Q Ɣ R Ṛ S Ṣ T Ṭ U W X Y Z Ẓ
```

Il comporte **33 lettres**.

| Position | Lettre | Unicode minuscule | Repère |
|---:|:---:|:---:|---|
| 1 | A a | U+0061 | lettre de base |
| 2 | Ɛ ɛ | U+025B | après A, consonne radicale |
| 3 | B b | U+0062 | lettre de base |
| 4 | C c | U+0063 | lettre de base |
| 5 | Č č | U+010D | après C |
| 6 | D d | U+0064 | lettre de base |
| 7 | Ḍ ḍ | U+1E0D | après D |
| 8 | E e | U+0065 | lettre de base |
| 9 | F f | U+0066 | lettre de base |
| 10 | G g | U+0067 | lettre de base |
| 11 | Ǧ ǧ | U+01E7 | après G |
| 12 | H h | U+0068 | lettre de base |
| 13 | Ḥ ḥ | U+1E25 | après H |
| 14 | I i | U+0069 | lettre de base |
| 15 | J j | U+006A | lettre de base |
| 16 | K k | U+006B | lettre de base |
| 17 | L l | U+006C | lettre de base |
| 18 | M m | U+006D | lettre de base |
| 19 | N n | U+006E | lettre de base |
| 20 | Q q | U+0071 | lettre de base |
| 21 | Ɣ ɣ | U+0263 | après Q |
| 22 | R r | U+0072 | lettre de base |
| 23 | Ṛ ṛ | U+1E5B | après R |
| 24 | S s | U+0073 | lettre de base |
| 25 | Ṣ ṣ | U+1E63 | après S |
| 26 | T t | U+0074 | lettre de base |
| 27 | Ṭ ṭ | U+1E6D | après T |
| 28 | U u | U+0075 | lettre de base |
| 29 | W w | U+0077 | lettre de base |
| 30 | X x | U+0078 | lettre de base |
| 31 | Y y | U+0079 | lettre de base |
| 32 | Z z | U+007A | lettre de base |
| 33 | Ẓ ẓ | U+1E93 | après Z |

---

## 3. Logique de l’ordre alphabétique

L’ordre kabyle n’est pas une simple copie de l’ordre alphabétique latin (anglais ou français). Il repose sur une logique accessible à un public non spécialiste, mais suffisamment précise pour être implémentée dans des logiciels.

### 3.1. Lettre de base, puis lettre modifiée

Les lettres portant un signe diacritique ou représentant une emphatique sont placées immédiatement après leur lettre de base.

Exemples :

```text
C < Č
D < Ḍ
G < Ǧ
H < Ḥ
R < Ṛ
S < Ṣ
T < Ṭ
Z < Ẓ
```

Cette règle permet de garder ensemble des mots qui appartiennent à une même famille phonétique.

---

### 3.2. Le cas de Ɛ : une consonne radicale, pas une simple voyelle

Dans l’écriture kabyle, `Ɛ` ne doit pas être traité comme une variante de la voyelle `E`. Il note une consonne radicale, généralement pharyngale, proche du `ع` dans les langues sémitiques.

Pour un locuteur natif, `Ɛ` fonctionne comme une lettre forte de la racine, pas comme une voyelle ordinaire.

C’est pourquoi il est placé en tête d’alphabet, après `A` :

```text
A < Ɛ
```

Erreurs à éviter dans un logiciel :

```text
- trier ɛ comme e
- supprimer ɛ lors de la normalisation
- fusionner ɛ avec e dans un index de recherche
```

---

### 3.3. Le cas de Ɣ : proximité avec Q

La lettre `Ɣ` est placée après `Q` :

```text
Q < Ɣ
```

Cette position correspond à une réalité linguistique du kabyle : `Q` et `Ɣ` peuvent s’alterner selon les régions, les parlers ou les formes verbales.

Exemples :

```text
yeɣra   — il a lu
yeqqar  — il lit / il dit
yeɣɣar  — variante
```

Ces formes relèvent d’une même dynamique radicale. Regrouper `Q` et `Ɣ` dans la collation aide les dictionnaires, les moteurs de recherche et les outils NLP à rapprocher des variantes dialectales ou morphologiques d’une même racine.

Cette proximité n’est pas qu’une impression de locuteur : elle est documentée sur le plan articulatoire. Les manuels de phonétique kabyle notent que `q` et `ɣ` partagent à peu près le même point d’articulation (au niveau du voile du palais / de la luette), et que c’est précisément pour cette raison que la gémination de `ɣ` se réalise phonétiquement comme `qq` plutôt que comme `ɣɣ` dans certains parlers (source 7). Le même type de document insiste, par des paires minimales telles que `yeɣra` (il a lu, étudié) et `yeṛɣa` (il a brûlé), sur la nécessité de bien distinguer `r`, `ṛ` et `ɣ`, ce qui recoupe et renforce les exemples cités plus haut.

Erreurs à éviter dans un logiciel :

```text
- trier ɣ comme g
- trier ɣ comme un caractère isolé en fin d’alphabet
- ignorer l’alternance Q / Ɣ dans un lemmatiseur ou un moteur de recherche
```

---

## 4. Règles pour les développeurs

### 4.1. Règle ICU / CLDR recommandée

Pour les bibliothèques ICU, les moteurs de recherche, les bases de données et les environnements supportant la collation Unicode, la règle de personnalisation recommandée est :

```icu
& A < ɛ <<< Ɛ
& C < č <<< Č
& D < ḍ <<< Ḍ
& G < ǧ <<< Ǧ
& H < ḥ <<< Ḥ
& Q < ɣ <<< Ɣ
& R < ṛ <<< Ṛ
& S < ṣ <<< Ṣ
& T < ṭ <<< Ṭ
& Z < ẓ <<< Ẓ
```

Cette règle respecte :

- la distinction entre lettres de base et lettres modifiées ;
- la position de `Ɛ` après `A` ;
- la position de `Ɣ` après `Q` ;
- la casse comme niveau secondaire ou tertiaire, selon l’implémentation.

---

### 4.2. Recommandation pour glibc / POSIX

La locale kabyle utilisée dans les systèmes GNU/Linux est généralement :

```text
kab_DZ.UTF-8
```

Les implémentations doivent faire attention au caractère `ǧ`.

Le caractère correct pour le kabyle est :

```text
ǧ  Ǧ  U+01E7 / U+01E6
```

Il ne doit pas être confondu avec le caractère turc :

```text
ğ  Ğ  U+011F / U+011E
```

Certaines versions anciennes ou incomplètes de locales utilisent `ğ` à la place de `ǧ`. Cela produit un tri incorrect pour les mots kabyles contenant `ǧ`.

---

### 4.3. Lettres géminées

Le kabyle utilise fréquemment des consonnes géminées :

```text
bb, gg, kk, qq, tt, ss, etc.
```

Pour la collation générale, la règle recommandée est le tri séquentiel :

```text
b < bb < c
```

Exemple :

```text
ab < abb < ac
```

Les dictionnaires spécialisés peuvent choisir une règle de contraction plus fine, mais pour un système d’exploitation, un navigateur ou une base de données, le tri séquentiel est la valeur sûre.

---

### 4.4. Lettres hors alphabet kabyle

Les lettres telles que `o`, `p`, `v` peuvent apparaître dans des emprunts récents.

Pour les textes mixtes, elles conservent leur position latine standard, intercalées dans la séquence kabyle sans interagir avec les lettres modifiées (qui suivent, elles, la règle du §3.1) :

```text
... N O P Q Ɣ R ...
... U V W ...
```

Chaque lettre empruntée a ainsi une position unique et non ambiguë, avec une borne inférieure et une borne supérieure explicites, ce qui est nécessaire pour une implémentation ICU ou glibc totalement déterministe :

- **O** : entre N et P
- **P** : entre O et Q
- **V** : entre U et W

La séquence complète, lettres empruntées incluses, est donc :

```text
A Ɛ B C Č D Ḍ E F G Ǧ H Ḥ I J K L M N O P Q Ɣ R Ṛ S Ṣ T Ṭ U V W X Y Z Ẓ
```

Ces trois lettres ne possèdent pas de variante diacritique kabyle propre (contrairement à C→Č, G→Ǧ, etc.) : leur insertion ne pose donc pas de question de type « lettre de base puis lettre modifiée », seulement une position fixe dans la séquence globale. Elles ne font pas partie des 33 lettres fondamentales de l’alphabet kabyle, mais elles ne doivent pas casser le tri dans les logiciels.

---

## 5. Recommandations pour les IA et le NLP

Pour les modèles de langage, les moteurs de recherche, les lemmatiseurs et les systèmes RAG, il est recommandé de :

1. **Ne jamais supprimer les lettres spécifiques** lors de la normalisation primaire.
2. **Ne jamais fusionner** :
   - `ɛ` avec `e` ;
   - `ɣ` avec `g` ;
   - `č` avec `c` ;
   - `ṭ` avec `t`.
3. **Conserver les diacritiques** dans les index de recherche.
4. **Prévoir des équivalences optionnelles** uniquement pour la recherche floue ou la normalisation historique :

```text
ch  ↔ c / č
gh  ↔ ɣ
dj  ↔ ǧ
```

5. **Respecter l’ordre suivant pour le tri** :

```text
A Ɛ B C Č D Ḍ E F G Ǧ H Ḥ I J K L M N Q Ɣ R Ṛ S Ṣ T Ṭ U W X Y Z Ẓ
```

---

## 6. Exemple de tri correct

Avec la collation kabyle correcte, une liste de mots doit respecter l’ordre suivant :

```text
Awi
Ɛrek
Ger
Ǧelleb
Qim
Ɣur
Rnu
Ṛkeḍ
Zwir
Ẓer
```

| Mot | Position attendue | Règle appliquée |
|---|---|---|
| Awi | après A | lettre de base A |
| Ɛrek | après A, avant B | Ɛ est une consonne radicale, placée juste après A |
| Ger | après G | lettre de base G |
| Ǧelleb | après G | Ǧ suit G |
| Qim | après N | lettre de base Q |
| Ɣur | après Q | Ɣ suit Q |
| Rnu | après Ɣ | lettre de base R |
| Ṛkeḍ | après R | Ṛ suit R |
| Zwir | après Y | lettre de base Z |
| Ẓer | après Z | Ẓ suit Z |

---

## 7. Résumé court pour implémentation rapide

Pour un logiciel, la collation kabyle peut être résumée ainsi :

```text
1. Utiliser les 33 lettres latines kabyles.
2. Placer chaque lettre modifiée après sa lettre de base.
3. Placer Ɛ immédiatement après A, car c’est une consonne radicale (position 2 sur 33, pas une variante de E).
4. Placer Ɣ après Q, en raison des alternances Q / Ɣ, phonétiquement documentées.
5. Ne pas supprimer ɛ, ɣ, č, ḍ, ǧ, ḥ, ṛ, ṣ, ṭ, ẓ lors du tri.
6. Utiliser ǧ U+01E7, pas ğ U+011F.
```

---

## Sources

1. **GNU C Library (glibc)** — fichier `localedata/locales/kab_DZ`, section `LC_COLLATE`.  
   Source : [https://raw.githubusercontent.com/bminor/glibc/master/localedata/locales/kab_DZ](https://raw.githubusercontent.com/bminor/glibc/master/localedata/locales/kab_DZ)  
   Consulté le 22 juin 2026.  
   Intérêt : montre l’état actuel de la locale `kab_DZ`, notamment les règles `A < ɛ` et `Q < ɣ`, ainsi que la confusion possible entre `ğ` et `ǧ`.

2. **Mouloud Mammeri**, *Tajeṛṛumt n tmaziɣt (Grammaire berbère)*, 1976.  
   Intérêt : référence historique pour la normalisation de l’alphabet latin berbère/kabyle.

3. **Jean-Marie Dallet**, *Dictionnaire kabyle-français*, SELAF, 1982.  
   Intérêt : référence lexicographique majeure pour le kabyle.

4. **Athmane Mokraoui (boffire)**, locuteur natif kabyle, mainteneur des ressources NLP kabyles, 2026.  
   Intérêt : attestation native concernant le statut de `Ɛ` comme consonne radicale et l’alternance entre `Q` et `Ɣ` dans des formes comme `yeɣra`, `yeqqar`, `yeɣɣar`.

5. **Unicode ICU**, documentation sur la collation et la personnalisation des règles de tri.  
   Source : [https://unicode-org.github.io/icu/userguide/collation/](https://unicode-org.github.io/icu/userguide/collation/)  
   Consulté le 22 juin 2026.  
   Intérêt : syntaxe et principes des règles de collation Unicode.

6. **Unicode CLDR**, Common Locale Data Repository.  
   Source : [https://github.com/unicode-org/cldr](https://github.com/unicode-org/cldr)  
   Consulté le 22 juin 2026.  
   Intérêt : dépôt des données de locales utilisées par les systèmes d’exploitation, les navigateurs, les bibliothèques logicielles et les outils NLP.

7. **Manuel de phonétique et transcription du kabyle**, *Initiation à la langue berbère de Kabylie*.  
   Consulté le 22 août 2026.  
   Intérêt : source phonétique indépendante confirmant que `q` et `ɣ` partagent à peu près le même point d’articulation, et que la gémination de `ɣ` se réalise phonétiquement comme `qq` dans certains parlers ; illustre également par des paires minimales (`yeɣra` / `yeṛɣa`) l’importance de distinguer `r`, `ṛ` et `ɣ`. Corrobore et renforce l’attestation native de la source 4 concernant l’alternance Q/Ɣ.

8. **Salem Chaker**, « Le berbère de Kabylie (Algérie) », in *Encyclopédie berbère*, XXVI, INALCO, 2004, p. 4055-4066.  
   Intérêt : référence académique de référence sur la phonologie et la description linguistique du kabyle, utile en appui général de la section 3.3.
