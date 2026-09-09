# Taqbaylit-ToBI : Spécification Prosodique du Kabyle

> **Status** : `draft` · **Version** : `1.0.0-rc1` · **Updated** : `2026-09-09`  
> **Domaine** : Phonétique acoustique, intonation, modélisation prosodique TTS/ASR  
> **Dépendances** : `orthography-spec.md` (Alphabet standard INALCO), `clitics-spec.md`  
>
> **Taxonomie de conformité épistémique :**
> - `[ATTESTÉ]` : Validé par des mesures acoustiques ou la littérature empirique vérifiée (Chaker, Mettouchi, Tigziri, Louali).
> - `[ADAPTÉ]` : Cadre autosegmental-métrique standard (ToBI, Cat_ToBI) transposé aux régularités du kabyle.
> - `[PROPOSITION]` : Convention d'ingénierie soumise à étalonnage sur corpus (à ne pas figer sans test d'alignement).

---

## 0. Périmètre et Fondements Théoriques

Le système **Taqbaylit-ToBI** est une formalisation autosegmentale-métrique (AM) de l'intonation et du phrasé prosodique du kabyle (*Taqbaylit*), conçue pour les moteurs TTS neuronaux (FastSpeech 2, VITS, Matcha-TTS), la segmentation ASR et l'annotation de corpus oraux.

Contrairement aux langues germaniques dont le système ToBI historique est issu, le kabyle ne possède **pas d'accent lexical fixe** contrastif (*stress-accent*). L'accentuation y est **démarcative et phrastique** :
1. **Nature de l'accent** : Purement mélodique (F₀), corrélé à la structure morphosyntaxique et aux frontières de syntagmes (Chaker 1995, Louali 1999).
2. **Organisation prosodique** : Structurée en **Unités Intonatives (UI / Intonation Units)** délimitées par des mouvements mélodiques périphériques, des réinitialisations de registre (*pitch reset*) et des pauses (Mettouchi & Lacheret-Dujour 2006 ; Mettouchi 2018).
3. **Structure informationnelle** : Déplacement systématique de l'énergie prosodique sur l'élément focalisé dans les structures clivées (*d X i/ay...*), suivi d'un aplatissement mélodique post-focal (Mettouchi 2003).

---

## 1. Architecture des Tiers d'Annotation

Une annotation Taqbaylit-ToBI standard sous Praat (`.TextGrid`) ou en pipeline d'ingénierie comporte 4 pistes (Tiers) synchronisées :

```text
Tier 1 [Interval] : Ortho      ──> Transcription NFC normalisée (spec dépôt)
Tier 2 [Point]    : Tones      ──> Tons AM (Pitch Accents et Boundary Tones)
Tier 3 [Point]    : Breaks     ──> Degrés de rupture prosodique (0 à 4)
Tier 4 [Interval] : Dialect    ──> Tag de zone diatopique (optionnel / métadonnée)
```

---

## 2. Tier 1 : Orthographe Canonique et Alignement

Le Tier 1 applique strictement la spécification orthographique du dépôt (alphabet latin INALCO, 33 caractères, géminées explicites `tt`, `kk`, `ss`, etc.).

1. **Normalisation Unicode** : Chaîne strictement normalisée en **NFC** exempte de sosies typographiques (ex. interdiction des lettres grecques ε, γ).
2. **Segmentation clitique** : Les clitiques préverbaux et postverbaux reliés par des traits d'union (`-`) dans la norme orthographique constituent une unité graphique unique dans ce Tier (`Ur-ten-id-fkiɣ-ara`).

---

## 3. Tier 2 : Tons et Intonation

### 3.1 Unité porteuse de ton (TBU - Tone-Bearing Unit)
En kabyle, le contraste phonologique repose sur trois voyelles stables /a, i, u/. Le schwa [ə] (noté *e*) est un élément de transition sans cible vocalique stable (Kossmann 1995, Bendjaballah 2001).
* **Règle d'ancrage TBU** `[ATTESTÉ]` : Les cibles tonales ($H, L$) s'ancrent prioritairement sur les **voyelles pleines** (/a, i, u/).
* **En l'absence de voyelle pleine** `[ADAPTÉ]` : L'ancrage F₀ s'effectue sur la consonne sonante noyau ([m, n, l, r]) ou s'interpole linéairement entre les voyelles pleines adjacentes.

### 3.2 Accents de hauteur (Pitch Accents)

Le kabyle n'étant pas une langue à accent de mot lexical récurrent, Taqbaylit-ToBI traite l'accent de hauteur comme un marqueur de tête de syntagme ou de focus :

| Événement prosodique | Description phonologique | Étiquette ToBI | Statut |
|---|---|---|---|
| **Prominence nominale** | Élévation mélodique sur la pénultième syllabe du syntagme nominal isolé ou en tête. | `H*` (ou `L+H*`) | `[ATTESTÉ]` (Chaker 1995, Tigziri 2000) |
| **Prominence verbale** | Élévation mélodique sur la dernière voyelle pleine du syntagme verbal. | `H*` | `[ATTESTÉ]` (Chaker 1995) |
| **Focus / Clivée** (*d X i/ay...*) | Pic d'intensité et de F₀ majeur sur le constituant clivé $X$, avec réinitialisation de registre (*pitch reset*). | `L+H*` | `[ATTESTÉ]` (Mettouchi 2003) |
| **Zone post-focale** | Aplatissement mélodique et compression dynamique sur la proposition relative/subordonnée suivant le focus clivé. | `_` (désaccentuation) / cible basse `L` | `[ATTESTÉ]` (Mettouchi 2003) |

> **Directive d'implémentation G2P/TTS** : Ne **jamais** pré-calculer un accent de mot lexical dans le dictionnaire de prononciation. L'accentuation s'infère en aval du parseur morphosyntaxique :
> - Syntagme nominal → Pénultième syllabe.
> - Complexe verbal → Dernière voyelle pleine du radical/affixe.

### 3.3 Tons de frontière (Boundary Tones)

Les tons de frontière clôturent les Unités Intonatives (UI / IP) au niveau Break 3 et Break 4 :

| Type d'énoncé | Patron mélodique | Étiquette ToBI | Statut |
|---|---|---|---|
| **Déclaration conclusive** | Chute finale sous la ligne de déclinaison. | `L-L%` | `[ATTESTÉ]` (Tigziri 2000) |
| **Continuation / Énumération** | Montée modérée en fin d'Unité Intonative non finale. | `L-H%` | `[ATTESTÉ]` (Mettouchi 2018) |
| **Interrogation totale (oui/non)** | Montée abrupte terminale en fin d'IP. | `H-H%` | `[ADAPTÉ]` |
| **Interrogation partielle (mots-Q)** | Structure clivée sous-jacente (*D acu...*, *Anwa...*) : pic focal initial sur le mot interrogatif puis retombée basse. | `L+H*` initial, terminal `L-L%` | `[ATTESTÉ]` (Mettouchi 2003) |
| **Injonction / Exclamation** | Pic haut suivi d'une chute abrupte en fin de mot prédicatif. | `H+L* L-L%` | `[PROPOSITION]` |

---

## 4. Tier 3 : Indices de Rupture (Break Indices)

L'échelle des ruptures s'aligne sur la syntaxe prosodique kabyle (Mettouchi 2018, Bader 1985) :

| Break | Définition fonctionnelle | Exemples types | Statut |
|:---:|---|---|:---:|
| **0** | **Cohésion clitique absolue.** Aucune frontière prosodique possible. Unité du mot prosodique (ω). Concerne les indices pronominaux, directionnels et morphèmes pré/post-verbaux. | `Ur-ten-id-fkiɣ-ara` | `[ATTESTÉ]` (Mettouchi 2001) |
| **1** | **Frontière de mot standard.** Séparation lexicale ordinaire sans dissociation intonative. | `argaz` | `ameqqran` | `[ADAPTÉ]` |
| **2** | **Frontière syntagmatique faible.** Limite de syntagme phonologique (φ) interne à une Unité Intonative (ex. pause structurelle après un topique court ou avant un syntagme prépositionnel lourd). | `[axxam]` | `[n wergaz]` | `[ATTESTÉ]` (Bader 1985) |
| **3** | **Frontière intermédiaire d'Unité Intonative (UI).** Pause mineure, allongement pré-pausal sans chute F₀ terminale (continuation). | `Mi d-yekcem,` | `yufa-ten...` | `[ATTESTÉ]` (Mettouchi & Lacheret 2006) |
| **4** | **Frontière majeure d'Unité Intonative (IP).** Clôture complète d'un tour de parole ou d'une phrase prosodique avec ton de frontière définitif et silence. | Fin de phrase achevée | `[ADAPTÉ]` |

> ⚠️ **Règle critique sur l'état d'annexion (*addad amaruz*)** `[ATTESTÉ]` :  
> La préposition de génitif `n` est proclitique et forme une unité prosodique insécable avec le substantif à l'état d'annexion qui la suit (Bader 1985).  
> - **Interdit :** `*axxam n | wergaz` (Break 2 ou 3 entre `n` et le nom).  
> - **Correct :** `axxam | n wergaz` (Rupture éventuelle Break 1 ou 2 *avant* la préposition `n`).

---

## 5. Tier 4 : Diatopie (Optionnel)

Afin d'éviter une prolifération injustifiée d'inventaires tonals, la variation régionale ne crée pas de règles de tons distinctes, mais renseigne une métadonnée d'énoncé :

| Tag | Zone dialectologique (Naït-Zerrad 2004) | Parler de référence |
|---|---|---|
| `OC` | Occidentale | At Manguellat (Dallet) |
| `EOC` | Extrême-Occidentale | Tizi-Ghenif, Draa El Mizan |
| `OR-abbas` | Orientale (Petite Kabylie) | At Abbas |
| `OR-tiwal` | Orientale (Vallée de la Soummam) | Tiwal, At Aïdel |
| `EOR` | Extrême-Orientale | Aokas (*tasaḥlit*), Melbou |

---

## 6. Phonétique Acoustique & Variance Adaptors (TTS)

Valeurs numériques physiques pour l'initialisation des prédicteurs de durée des moteurs neuronaux :

| Événement acoustique | Multiplicateur de durée | Base de calcul | Validation empirique |
|---|:---:|---|---|
| **Consonne géminée / tendue** (`tt`, `kk`, `dd`...) | **×1.8 à ×2.3** | Consonne simple équivalente | `[ATTESTÉ]` (Tigziri 2000, Louali 1999) |
| **Allongement pré-pausal final** | **×1.3 à ×1.5** | Dernière voyelle pleine avant Break 3/4 | `[ATTESTÉ]` (Louali & Boë 1993) |
| **Pause de frontière Break 4** | ≥ 250 ms | Silence intercalaire | `[ADAPTÉ]` (CorpAfroAs) |
| **Pause de frontière Break 3** | 80 ms - 180 ms | Silence intercalaire | `[ADAPTÉ]` (CorpAfroAs) |
| **Frontières Break 0 et 1** | 0 ms | Continuité acoustique absolue | `[ATTESTÉ]` |

> ❌ **Note phonétique éliminatoire** : Le kabyle ne possédant aucun contraste de durée vocalique phonologique (Louali & Boë 1993), aucune règle d'allongement intrinsèque ne doit être associée aux voyelles isolées en dehors des allongements pré-pausaux de frontière.

---

## 7. Directives de Corpus & Données d'Étalonnage

1. **Corpus de référence étalon** : L'entraînement et l'étalonnage de Taqbaylit-ToBI doivent s'appuyer prioritairement sur les enregistrements en accès ouvert du corpus **CorpAfroAs - Volet Kabyle (CNRS-LLACAN / Huma-Num)**, déjà segmentés et transcrits sous Praat en Unités Intonatives réelles.
2. **Filtrage des corpus de synthèse (Tatoeba, Common Voice)** :
   - Écarter tout énoncé non conforme au validateur `validate_kabyle.py` (`orthography_gate: FAIL`).
   - Marquer les calques syntaxiques d'un tag `source_type: translated` : les structures syntaxiques calquées sur le français faussent les patrons de focalisation indigènes.

---

## 8. Exemples de Référence Annotés (Praat TextGrid)

### Exemple 1 : Énoncé déclaratif neutre avec syntagme génitif

> *Axxam n wergaz-nni meqqwer.* (« La maison de cet homme est grande. »)

```text
File type = "ooTextFile"
Object class = "TextGrid"

xmin = 0.0
xmax = 1.95
tiers? <exists>
size = 3
item []:
    item [1]:
        class = "IntervalTier"
        name = "Ortho"
        intervals: size = 4
            [1]: xmin = 0.00 xmax = 0.45 text = "Axxam"
            [2]: xmin = 0.45 xmax = 1.30 text = "n wergaz-nni"
            [3]: xmin = 1.30 xmax = 1.95 text = "meqqwer"
    item [2]:
        class = "TextTier"
        name = "Tones"
        points: size = 4
            [1]: time = 0.20 mark = "H*"     ; Pénultième de syntagme nominal
            [2]: time = 0.85 mark = "H*"     ; Pénultième de syntagme déterminé
            [3]: time = 1.55 mark = "H*"     ; Prédicat verbal/adjectival
            [4]: time = 1.95 mark = "L-L%"   ; Ton de frontière déclaratif
    item [3]:
        class = "TextTier"
        name = "Breaks"
        points: size = 3
            [1]: time = 0.45 mark = "1"      ; Frontière nominale avant préposition
            [2]: time = 1.30 mark = "1"      ; Pas de rupture forte
            [3]: time = 1.95 mark = "4"      ; Clôture d'Unité Intonative
```

### Exemple 2 : Structure clivée / Focalisation (*d X ay...*)

> *D argaz ay yuran tabrat.* (« C'est l'homme qui a écrit la lettre. »)

```text
File type = "ooTextFile"
Object class = "TextGrid"

xmin = 0.0
xmax = 1.80
tiers? <exists>
size = 3
item []:
    item [1]:
        class = "IntervalTier"
        name = "Ortho"
        intervals: size = 4
            [1]: xmin = 0.00 xmax = 0.60 text = "D argaz"
            [2]: xmin = 0.60 xmax = 0.75 text = "ay"
            [3]: xmin = 0.75 xmax = 1.20 text = "yuran"
            [4]: xmin = 1.20 xmax = 1.80 text = "tabrat"
    item [2]:
        class = "TextTier"
        name = "Tones"
        points: size = 3
            [1]: time = 0.35 mark = "L+H*"   ; Pic focal majeur sur le clivé
            [2]: time = 0.95 mark = "L"      ; Post-focal compression (désaccentuation)
            [3]: time = 1.80 mark = "L-L%"   ; Clôture basse
    item [3]:
        class = "TextTier"
        name = "Breaks"
        points: size = 4
            [1]: time = 0.60 mark = "1"
            [2]: time = 0.75 mark = "0"      ; Clitique d'extraction
            [3]: time = 1.20 mark = "1"
            [4]: time = 1.80 mark = "4"
```

---

## 9. Références Bibliographiques Normatives

1. **Bader, Yousef (1985).** *Kabyle Berber Phonology and Morphology*, PhD Dissertation, University of Kansas.
2. **Bendjaballah, Sabrina (2001).** *« The internal structure of the vowel system of Kabyle Berber »*, in Nicolaï, R. (ed.), *Current Approaches to African Linguistics*.
3. **Caron, Bernard & Mettouchi, Amina (2010).** *« Unités prosodiques et unités syntaxiques dans deux langues afro-asiatiques : le haoussa et le kabyle »*, *Congrès Mondial de Linguistique Française (CMLF)*.
4. **Chaker, Salem (1995).** *« Données exploratoires en prosodie berbère. I : L'accent en kabyle ; II : Intonation et syntaxe en kabyle »*, *Comptes rendus du GLECS*, vol. 31, p. 27–82.
5. **Kossmann, Maarten (1995).** *« Schwa en berbère »*, *Afrikanistische Arbeitspapiere (AAP)*, vol. 43, p. 89–106.
6. **Louali, Nadia & Boë, Louis-Jean (1993).** *« Mesures acoustiques des voyelles du berbère : application au système vocalique kabyle »*, *Actes des Journées d'Étude sur la Parole (JEP)*, p. 251–256.
7. **Louali, Nadia (1999).** *« L’accent en berbère : contraintes phonétiques et pertinence phonologique »*, *Études et Documents Berbères*, vol. 17, p. 107–123.
8. **Mettouchi, Amina (2003).** *« Focus, prosodie et subordination en kabyle »*, *Faits de Langues*, vol. 21, p. 197–208.
9. **Mettouchi, Amina & Lacheret-Dujour, Anne (2006).** *« Prédication seconde et intonation en kabyle »*, *Travaux linguistiques du CerLiCO*, vol. 19.
10. **Mettouchi, Amina (2018).** *« Prosody and Syntax in Berber »*, in Tucker Childs, G. (ed.), *The Oxford Handbook of African Languages*, Oxford University Press.
11. **Mettouchi, A., Frajzyngier, Z., & Mengozzi, A. (2015).** *Corpus-based Studies of Lesser-described Languages: The CorpAfroAs corpus of spoken Afroasiatic languages*, John Benjamins.
12. **Naït-Zerrad, Kamal (2004).** *« Kabylie : dialectologie »*, *Encyclopédie berbère*, vol. 26, p. 4067–4070.
13. **Tigziri, Naïma (2000).** *« Étude acoustique descriptive d'un parler berbère (kabyle). Accentuation, intonation et morphosyntaxe »*, *Travaux de Linguistique*, vol. 26, Rijksuniversiteit Gent, p. 21–69.
