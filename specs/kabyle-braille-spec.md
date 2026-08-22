# Spécification de transcription braille pour le kabyle (Taqbaylit)

**Version :** 0.3-draft

**Date :** 22 août 2026

**Statut :** proposition de référence en cours de validation

**Domaine :** kabyle écrit en alphabet latin berbère, transcription braille non contractée fondée sur le profil CBFU retenu par Liblouis

**Auteur du document source :** Athmane Mokraoui (`kabyle-specs`)

## Résumé

Cette spécification définit un profil de transcription braille pour le kabyle écrit en alphabet latin. Elle conserve les correspondances CBFU pour les lettres et les signes communs, puis introduit un mode indicateur à deux cellules pour les caractères spécifiques kabyles. Le point 5 (`⠐`) est utilisé comme modificateur devant une lettre de base. Le profil distingue `ǧ` et `ɣ` afin de garantir une rétrotraduction non ambiguë entre ces deux caractères.

La spécification décrit trois niveaux distincts qu’il ne faut pas confondre : la règle linguistique, la représentation Unicode des cellules braille et la syntaxe d’une table Liblouis. Une sortie Unicode telle que `⠐⠉` est une représentation lisible du résultat ; dans un fichier source Liblouis, elle doit être exprimée par un motif numérique tel que `5-14`.

Le document est une **proposition de référence**, et non un standard officiellement adopté. Une validation par des lecteurs braille kabyles, des experts du braille français et des essais sur papier embossé restent nécessaires avant une publication stable.

## 1. Conventions normatives et périmètre

Les termes **DOIT**, **NE DOIT PAS**, **DEVRAIT**, **NE DEVRAIT PAS** et **PEUT** sont employés au sens normatif habituel. Une règle marquée **informative** explique un choix mais ne définit pas à elle seule une exigence de conformité.

Le profil défini ici couvre la transcription graphemique du kabyle latin en braille six points, sans contraction. Il couvre les lettres, les majuscules, les chiffres, la ponctuation courante, les espaces, les tirets, les géminées et les caractères kabyles spécifiques. Il ne définit ni une transcription phonétique, ni un grade 2 kabyle, ni un code pour le néo-tifinaghe, ni un mode braille huit points.

La transcription s’applique à un texte orthographiquement normalisé. Un convertisseur complet DEVRAIT effectuer ou appeler un module de normalisation avant la transcription. La table Liblouis, elle, reste une couche de traduction et ne remplace pas un validateur orthographique.

### 1.1 Profils de sortie

Le profil principal est **CBFU-6-indicator-antoine** : braille six points, mode indicateur pour les lettres kabyles spécifiques et notation Antoine pour les chiffres. Le profil Liblouis de référence utilise `fr-bfu-comp6.utb` comme base dans les distributions qui fournissent cette table.

La notation Louis Braille peut être proposée comme profil d’interopérabilité distinct, mais elle n’est pas le profil numérique par défaut de cette spécification. Une application qui offre plusieurs profils DOIT les annoncer explicitement dans ses métadonnées ou sa configuration.

Le terme **BRF** ne désigne pas ici une représentation universelle unique des caractères étendus. Un export BRF DEVRAIT préciser le codage d’affichage ou la table de conversion utilisée par la plateforme et l’embosseuse. La définition détaillée d’un profil BRF kabyle est reportée à une révision ultérieure.

## 2. Principes de transcription

Le braille kabyle est une transcription de l’orthographe. Un graphème source est converti en une ou plusieurs cellules braille ; les règles de prononciation, de spirantisation, d’allophonie et de phonologie n’altèrent pas la chaîne écrite.

Les 26 lettres latines de base conservent les motifs du profil CBFU choisi. Les lettres `p` et `v`, même lorsqu’elles ne sont pas natives de l’inventaire kabyle retenu, restent disponibles pour les emprunts et les citations. Les dix caractères spécifiques sont traités par le mode indicateur défini à la section 3.

Une géminée est transcrite par la répétition de la transcription du caractère concerné. Ainsi, `ṭṭ` devient `⠐⠞⠐⠞` et `čč` devient `⠐⠉⠐⠉`. Aucun signe de gémination supplémentaire n’est introduit.

Les espaces, les tirets et la ponctuation sont conservés, sauf lorsqu’un module de normalisation a explicitement défini une autre opération. Les clitiques et les noms composés ne reçoivent pas de traitement spécial au niveau braille.

## 3. Caractères spécifiques kabyles

### 3.1 Règle du mode indicateur

Pour chaque caractère spécifique minuscule, le point 5 (`⠐`) précède la lettre de base retenue. La majuscule correspondante reçoit d’abord l’indicateur de majuscule du profil CBFU (`⠨`), puis la séquence du caractère spécifique.

La règle est donc de la forme suivante :

```text
minuscule spécifique = ⠐ + lettre de base
majuscule spécifique = ⠨ + ⠐ + lettre de base
```

Le choix de `q` comme base de `ɣ` est normatif dans cette version. Il évite que `ǧ` et `ɣ` produisent la même séquence braille.

### 3.2 Tableau normatif

| Caractère | Code Unicode | Base | Motif source | Sortie minuscule | Sortie majuscule |
|---|---:|---|---|---|---|
| `č` | U+010D | `c` | `5-14` | `⠐⠉` | `Č` → `⠨⠐⠉` |
| `ḍ` | U+1E0D | `d` | `5-145` | `⠐⠙` | `Ḍ` → `⠨⠐⠙` |
| `ɛ` | U+025B | `e` | `5-15` | `⠐⠑` | `Ɛ` → `⠨⠐⠑` |
| `ǧ` | U+01E7 | `g` | `5-1245` | `⠐⠛` | `Ǧ` → `⠨⠐⠛` |
| `ɣ` | U+0263 | `q` | `5-12345` | `⠐⠟` | `Ɣ` → `⠨⠐⠟` |
| `ḥ` | U+1E25 | `h` | `5-125` | `⠐⠓` | `Ḥ` → `⠨⠐⠓` |
| `ṛ` | U+1E5B | `r` | `5-1235` | `⠐⠗` | `Ṛ` → `⠨⠐⠗` |
| `ṣ` | U+1E63 | `s` | `5-234` | `⠐⠎` | `Ṣ` → `⠨⠐⠎` |
| `ṭ` | U+1E6D | `t` | `5-2345` | `⠐⠞` | `Ṭ` → `⠨⠐⠞` |
| `ẓ` | U+1E93 | `z` | `5-1356` | `⠐⠵` | `Ẓ` → `⠨⠐⠵` |

Le motif `5-12345` de `ɣ` signifie deux cellules : le modificateur point 5, puis la cellule de `q` (`12345`). Il ne s’agit pas d’une cellule six points unique.

### 3.3 Séquences partagées avec des symboles CBFU

Certaines séquences du mode indicateur sont également utilisées par des symboles CBFU. Dans la table française de référence, `⠐⠉` correspond à `©`, `⠐⠗` à `®` et `⠐⠞` à `™`. Cette situation constitue une **ambiguïté contextuelle de séquence**, mais n’empêche pas la compilation de la table kabyle.

Un document qui mélange kabyle, français et symboles techniques DOIT conserver son contexte de langue ou de profil. Une rétrotraduction réalisée sans cette information peut interpréter une séquence partagée selon le profil actif. Les applications DEVRAIENT donc éviter de prétendre qu’une rétrotraduction brute est toujours bijective entre plusieurs profils linguistiques.

## 4. Lettres et signes communs

### 4.1 Lettres latines de base

Les correspondances suivantes sont celles du profil CBFU six points retenu pour cette proposition.

| Lettre | Braille | Points | Lettre | Braille | Points |
|---|---|---:|---|---|---:|
| `a` | `⠁` | 1 | `n` | `⠝` | 1345 |
| `b` | `⠃` | 12 | `o` | `⠕` | 135 |
| `c` | `⠉` | 14 | `p` | `⠏` | 1234 |
| `d` | `⠙` | 145 | `q` | `⠟` | 12345 |
| `e` | `⠑` | 15 | `r` | `⠗` | 1235 |
| `f` | `⠋` | 124 | `s` | `⠎` | 234 |
| `g` | `⠛` | 1245 | `t` | `⠞` | 2345 |
| `h` | `⠓` | 125 | `u` | `⠥` | 136 |
| `i` | `⠊` | 24 | `v` | `⠧` | 1236 |
| `j` | `⠚` | 245 | `w` | `⠺` | 2456 |
| `k` | `⠅` | 13 | `x` | `⠭` | 1346 |
| `l` | `⠇` | 123 | `y` | `⠽` | 13456 |
| `m` | `⠍` | 134 | `z` | `⠵` | 1356 |

Les majuscules ordinaires suivent le mécanisme du profil CBFU. Une majuscule simple est précédée de `⠨` (`46`). Un mot ou un sigle entièrement en majuscules utilise le mécanisme de majuscule du profil choisi ; avec la table Liblouis de référence, `IRCAM` devient `⠨⠨⠊⠗⠉⠁⠍`.

Les indicateurs de passage majuscule étendu ne sont pas redéfinis par la table kabyle. Ils relèvent du profil CBFU et doivent être testés séparément si une application les expose.

### 4.2 Ponctuation et espaces

La ponctuation ci-dessous suit la base CBFU/Liblouis `fr-bfu-comp6.utb` retenue pour l’implémentation. Cette décision remplace, dans la version 0.2, les parenthèses à deux cellules `⠐⠣` et `⠐⠜`.

| Caractère source | Sortie braille | Points |
|---|---|---:|
| espace | `⠀` | 0 |
| `,` | `⠂` | 2 |
| `;` | `⠆` | 23 |
| `:` | `⠒` | 25 |
| `.` | `⠲` | 256 |
| `?` | `⠦` | 236 |
| `!` | `⠖` | 235 |
| `«` ou `»` | `⠶` | 2356 |
| `-` | `⠤` | 36 |
| `(` | `⠦` | 236 |
| `)` | `⠴` | 356 |

La similarité entre certaines cellules de ponctuation est une propriété du profil CBFU et doit être interprétée selon le contexte syntaxique. Les espaces multiples DEVRAIENT être normalisés en espaces simples avant la transcription lorsque le document cible le profil linguistique kabyle canonique.

Le kabyle orthographique de référence n’utilise pas l’apostrophe. Un normaliseur DEVRAIT signaler ou traiter une apostrophe avant la transcription. La table Liblouis peut néanmoins conserver la règle CBFU héritée afin de rester utilisable avec des citations ou des textes mixtes.

### 4.3 Chiffres

Le profil principal utilise la notation Antoine. L’indicateur numérique est `⠠` (`6`) et précède les cellules numériques.

| Chiffre | Sortie Antoine | Points |
|---:|---|---:|
| 1 | `⠠⠡` | 6 + 16 |
| 2 | `⠠⠣` | 6 + 126 |
| 3 | `⠠⠩` | 6 + 146 |
| 4 | `⠠⠹` | 6 + 1456 |
| 5 | `⠠⠱` | 6 + 156 |
| 6 | `⠠⠫` | 6 + 1246 |
| 7 | `⠠⠻` | 6 + 12456 |
| 8 | `⠠⠳` | 6 + 1256 |
| 9 | `⠠⠪` | 6 + 246 |
| 0 | `⠠⠼` | 6 + 3456 |

La notation Louis Braille, `⠼` suivie des lettres `a` à `j`, PEUT être exposée dans un profil distinct destiné à l’interopérabilité internationale. Elle ne doit pas être mélangée silencieusement avec la notation Antoine dans un même document.

## 5. Normalisation du texte source

La normalisation est une étape préalable au profil braille. Elle ne doit pas être confondue avec la table Liblouis, qui traduit la chaîne reçue.

| Identifiant | Contrôle | Exigence |
|---|---|---|
| N1 | Unicode | Le texte DOIT être normalisé en NFC afin de privilégier les caractères précomposés. |
| N2 | Inventaire | Les caractères kabyles spécifiques DOIVENT appartenir à l’inventaire défini à la section 3. |
| N3 | Confusables | Les caractères visuellement ou phonétiquement proches mais non retenus, tels que `ε`, `γ`, `ğ` ou `ĝ`, DEVRAIENT être rejetés ou corrigés explicitement. |
| N4 | Casse | Les formes majuscules DOIVENT être les correspondantes Unicode définies dans le tableau normatif. |
| N5 | Espaces | Les espaces multiples DEVRAIENT être réduits à une espace U+0020 dans le profil canonique. |
| N6 | Apostrophe | Une apostrophe DOIT être signalée par le normaliseur du profil kabyle, sauf si le document est déclaré mixte ou citationnel. |
| N7 | Accents étrangers | Les accents français ne doivent pas être supprimés automatiquement dans une citation française. Toute suppression dans un texte kabyle doit être une règle de normalisation explicitement déclarée. |

Un convertisseur conforme DEVRAIT produire un diagnostic comprenant la position, le caractère problématique et la correction proposée. Il ne devrait pas remplacer silencieusement un caractère inconnu par une cellule braille arbitraire.

## 6. Exemples normatifs

Les exemples suivants fixent les sorties attendues pour le profil principal.

| Texte source | Sortie braille |
|---|---|
| `Taqbaylit` | `⠨⠞⠁⠟⠃⠁⠽⠇⠊⠞` |
| `Ɛemmi` | `⠨⠐⠑⠑⠍⠍⠊` |
| `aɣrum` | `⠁⠐⠟⠗⠥⠍` |
| `ɣer` | `⠐⠟⠑⠗` |
| `aḍar` | `⠁⠐⠙⠁⠗` |
| `iḥemmel` | `⠊⠐⠓⠑⠍⠍⠑⠇` |
| `aṭas` | `⠁⠐⠞⠁⠎` |
| `laẓ` | `⠇⠁⠐⠵` |
| `iǧeǧǧigen` | `⠊⠐⠛⠑⠐⠛⠐⠛⠊⠛⠑⠝` |
| `tameṭṭut` | `⠞⠁⠍⠑⠐⠞⠐⠞⠥⠞` |
| `ččuren` | `⠐⠉⠐⠉⠥⠗⠑⠝` |
| `aɣrum-nsen` | `⠁⠐⠟⠗⠥⠍⠤⠝⠎⠑⠝` |
| `baba-s` | `⠃⠁⠃⠁⠤⠎` |
| `Bonjour` | `⠨⠃⠕⠝⠚⠕⠥⠗` |
| `café` | `⠉⠁⠋⠿` |
| `œuvre` | `⠪⠥⠧⠗⠑` |
| `à` | `⠷` |
| `(` | `⠦` |
| `)` | `⠴` |
| `123` | `⠠⠡⠣⠩` |
| `2026` | `⠠⠣⠼⠣⠫` |

La chaîne `œuvre` contient bien les cinq cellules correspondant à `œ`, `u`, `v`, `r` et `e`. La chaîne `aɣrum` utilise `⠐⠟`, conformément à la distinction normative entre `ǧ` et `ɣ`.

## 7. Profil Liblouis de référence

### 7.1 Dépendance et compatibilité

La table de référence s’appelle `kabyle.ctb`. Elle cible Liblouis 3.20 ou une version ultérieure, car elle utilise l’opcode `base` pour associer les majuscules à leurs minuscules. La table `uplow` n’est pas utilisée.

Le nom `fr-bfu-g1.utb` ne doit pas être utilisé comme dépendance obligatoire : il n’est pas fourni par certaines distributions récentes, dont le paquet Liblouis 3.29 testé sous Linux Mint. Le profil validé utilise `fr-bfu-comp6.utb`.

Le fichier source DOIT être enregistré sans BOM UTF-8. Pour maximiser la portabilité, les caractères spécifiques sont écrits avec des échappements ASCII `\xNNNN` et les motifs braille sont écrits avec des nombres de points.

### 7.2 Table minimale conforme

Le bloc suivant est la base normative de l’implémentation Liblouis. Les définitions kabyles précèdent l’inclusion française afin que `ɛ`, également présent dans les définitions IPA héritées, reçoive bien le mapping kabyle.

```ctb
# Kabyle (Taqbaylit) — CBFU grade 1, mode indicateur
# Source ASCII sans BOM; Liblouis >= 3.20

lowercase \x010D 5-14       # č
lowercase \x1E0D 5-145      # ḍ
lowercase \x025B 5-15       # ɛ
lowercase \x01E7 5-1245    # ǧ
lowercase \x0263 5-12345   # ɣ = point 5 + q
lowercase \x1E25 5-125      # ḥ
lowercase \x1E5B 5-1235     # ṛ
lowercase \x1E63 5-234      # ṣ
lowercase \x1E6D 5-2345     # ṭ
lowercase \x1E93 5-1356     # ẓ

base uppercase \x010C \x010D # Č / č
base uppercase \x1E0C \x1E0D # Ḍ / ḍ
base uppercase \x0190 \x025B # Ɛ / ɛ
base uppercase \x01E6 \x01E7 # Ǧ / ǧ
base uppercase \x0194 \x0263 # Ɣ / ɣ
base uppercase \x1E24 \x1E25 # Ḥ / ḥ
base uppercase \x1E5A \x1E5B # Ṛ / ṛ
base uppercase \x1E62 \x1E63 # Ṣ / ṣ
base uppercase \x1E6C \x1E6D # Ṭ / ṭ
base uppercase \x1E92 \x1E93 # Ẓ / ẓ

include fr-bfu-comp6.utb
```

Les caractères braille Unicode ne doivent pas être placés dans la troisième colonne d’une règle Liblouis telle que `always č ⠐⠉`. Cette écriture produit une erreur de compilation, car l’opérande attend un motif de points. Pour afficher une sortie sous forme de cellules Unicode, la table d’affichage `unicode.dis` doit être utilisée au moment de l’exécution.

### 7.3 Commandes de validation

Depuis le répertoire qui contient `kabyle.ctb`, la commande suivante DOIT réussir :

```bash
lou_checktable kabyle.ctb
```

Une sortie Unicode lisible s’obtient avec :

```bash
printf 'Ačuṛu\n' | lou_translate unicode.dis,kabyle.ctb
```

La sortie attendue est :

```text
⠨⠁⠐⠉⠥⠐⠗⠥
```

La distinction `ǧ`/`ɣ` se vérifie avec :

```bash
printf 'ǧ ɣ Ǧ Ɣ\n' | lou_translate unicode.dis,kabyle.ctb
```

La sortie attendue est :

```text
⠐⠛⠀⠐⠟⠀⠨⠐⠛⠀⠨⠐⠟
```

Une installation applicative peut conserver `kabyle.ctb` dans son propre répertoire. Une installation système peut le copier vers le répertoire des tables Liblouis, généralement `/usr/share/liblouis/tables/`, mais cette opération doit être gérée par le paquet ou l’administrateur de la machine.

## 8. Critères de conformité

Une implémentation est conforme au profil principal si elle respecte simultanément les points suivants :

| Domaine | Critère |
|---|---|
| Caractères spécifiques | Les dix minuscules et les dix majuscules produisent exactement les séquences de la section 3.2. |
| Distinction `ǧ`/`ɣ` | `ǧ` utilise `⠐⠛` et `ɣ` utilise `⠐⠟`; les majuscules conservent cette distinction. |
| Casse | Les majuscules ordinaires et spéciales utilisent l’indicateur du profil CBFU. |
| Géminées | Deux occurrences successives restent deux transcriptions successives. |
| Tirets | Le tiret U+002D produit `⠤` dans le profil de base. |
| Chiffres | La notation Antoine est utilisée par défaut. |
| Ponctuation | Les parenthèses suivent `⠦` et `⠴` dans le profil CBFU/Liblouis retenu. |
| Liblouis | `lou_checktable kabyle.ctb` retourne un code de succès et ne signale aucune erreur. |
| Source | La table ne contient pas de BOM et n’emploie pas `\yNNNN` dans les opérandes de caractères. |
| Rétrotraduction | Les caractères spécifiques distincts sont réversibles dans le profil kabyle; les séquences partagées avec des symboles CBFU restent dépendantes du contexte de profil. |

Les tests de non-régression DEVRAIENT également inclure un échantillon français comprenant `Bonjour`, `café`, `œuvre`, `à`, les chiffres, les parenthèses et les symboles `©`, `®` et `™` hors contexte kabyle.

## 9. Limites et validation humaine

Cette proposition ne constitue pas encore un standard braille officiellement reconnu. La validation suivante est nécessaire avant la version 1.0 :

1. Des lecteurs braille kabyles doivent évaluer la lisibilité, la mémorisation et la vitesse de lecture des dix séquences spécifiques.
2. Des experts du CBFU doivent confirmer la politique des séquences partagées avec `©`, `®` et `™`, ainsi que la compatibilité avec les documents mixtes.
3. Des essais sur embosseuse doivent vérifier les sorties Unicode et BRF sur papier, notamment pour les géminées et les caractères utilisant deux cellules.
4. Les tests doivent être exécutés sur plusieurs versions et distributions de Liblouis, car les noms de tables incluses et les profils d’affichage peuvent varier.
5. La politique Antoine/Louis doit rester explicite dans chaque export numérique ou BRF.

Une future version pourra définir un profil braille huit points, un grade 2 kabyle ou une table tifinaghe séparée. Ces extensions ne doivent pas modifier silencieusement le profil six points défini ici.

## 10. Historique des changements

| Version | Évolution |
|---|---|
| 0.2-draft | Première proposition du mode indicateur et des dix caractères spécifiques. |
| 0.3-draft | Distinction normative `ǧ → ⠐⠛` / `ɣ → ⠐⠟`; dépendance Liblouis corrigée vers `fr-bfu-comp6.utb`; syntaxe Liblouis rendue compilable; parenthèses alignées sur le profil CBFU/Liblouis retenu; exemple `œuvre` corrigé; politique Antoine rendue explicite; limites et critères de conformité précisés. |

## Références

[1]: https://kabyle-specs.github.io/specs/kabyle-braille-spec.md "Spécification braille kabyle source"
[2]: https://liblouis.io/documentation/liblouis/How-to-Write-Translation-Tables.html "Liblouis — How to Write Translation Tables"
[3]: https://liblouis.io/documentation/liblouis/Translation-Opcodes.html "Liblouis — Translation Opcodes"
[4]: https://liblouis.io/liblouis/2021/12/06/liblouis-release-3.20.0.html "Liblouis 3.20.0 — opcode base et remplacement de uplow"
[5]: https://liblouis.io/braille-specs/french/ "Liblouis — Unified French Braille"
[6]: https://www.pharmabraille.com/braille-codes/france-braille-code/ "PharmaBraille — France Braille Code"

La syntaxe des règles Liblouis, l’emploi de `\xNNNN` et la nécessité de motifs de points sont définis dans le manuel Liblouis [2] [3]. Le remplacement de `uplow` par `base` est documenté dans l’annonce de Liblouis 3.20 [4]. La page de référence CBFU publiée par Liblouis et le tableau public France Braille Code confirment le profil français utilisé ici pour les chiffres, les lettres de base et les parenthèses [5] [6].

---

*Document révisé pour le projet de ressources d’accessibilité linguistique kabyle. Les choix marqués comme proposition ou profil doivent être validés par des utilisateurs braille et des experts compétents avant adoption stable.*
