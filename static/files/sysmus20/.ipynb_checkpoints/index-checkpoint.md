
class: middle, center

# Data-Driven Music History

Fabian C. Moss ([@fabianmoss](https://twitter.com/fabianmoss))<br />
Digital and Cognitive Musicology Lab<br />
École Polytechnique Fédérale de Lausanne

14 September 2020, 2-3:30 PM

International Conference of Students of Systematic Musicology (SysMus)

<img src="img/EPFL.png" width=200 align="middle" hspace="30">
<img src="img/SNF.png" width=300 align="middle" hspace="30">

???
## Research at the Digital and Cognitive Musicology Lab

Digital
- corpus creation
- corpus analysis, e.g. specific composers & genres
- computational modeling

Cognitive
- musical syntax
- perception of music
- music and emotion

- Here: focus on the first aspect

---

class: middle

## Background

- traditionally: fundamental division historical / systematic music research .cite[Honing, 2006; Cook, 2006]
- differences in methods and questions
- new developments might bridge this gap .cite[Pugin, 2005; Huron, 2013]
- digital humanities: quantitative approaches to humanist research agendas
- machine-readable datasets

???

Traditionally, musicology has been divided into historical and systematic research agendas, encompassing qualitative-hermeneutic and quantitative-empirical methodologies, respectively. Innovations in the emerging and rapidly growing field of musical corpus studies question this fundamental divide and address, for instance, inherently historical questions with quantitative methods, fueled by the creation of ever larger and more appropriate datasets. 

---

## This workshop

### Outline

1. methodological/epistemological introduction
2. hands-on case study
  - you can follow this presentation and observe **and/or** go along with the exercises in ***this notebook***
3. critical discussion

--

### Outcomes (hopefully)

- reflections about the scientific study of music, useful/lessness of disciplinarysubdivisions
- simulation of a research cycle from initial idea to final result
- introduction (in passing) of concrete quantitative methods

???

This workshop first introduces some methodological and epistemological issues regarding empirical approaches to music history. It then presents a hands-on exercise on a case study. Finally, it invites critical discussion about the implications and relevance of the results for other subfields such as music psychology. In doing so, the workshop simulates (nearly) the entire life cycle of a research project, from an initial idea via selecting appropriate operationalisations and measures up to choosing suitable visualisations to communicate the results, e.g. in a research article or a blog post. At each point, participants will be invited to critically reflect the decisions taken. Along the way, more general methods for data analysis (e.g. data transformation, clustering, dimensionality reduction, and plotting) will be introduced. This is expected to benefit participants in a vast number of future projects. 

---

class: middle

## Technical aspects

- Python 3
- Jupyter notebooks
- no prior knowledge required
- let's hope the connection holds! (if not see the **slides**)

???

We will use Python as a programming language within the framework of an interactive coding environment. The participants are not expected to have any prior experience with either, and do not need to install any software on their laptops. A web browser and a stable internet connection are the only prerequisites.

---

class: middle

## Outline

1. Who are we? 
  - Who am I?
  - Who are you?
2. Data-Driven Music History: Methodological Considerations
  - Intro/Background: Historical Musical Corpus Research
  - Sampling
  - Representativity
  - Questions! (Hypotheses)
3. Case Study: Historical Changes of Tonality
  1. Distributions of Tonal Pitch Classes, probability distributions, entropy
  2. Hypothesis: similar pieces have similar distributions, operationalization: distances in vector space
  3. Assumption (for coloring): tonal center is approximated by most frequent note
  4. Taking **tonal** spaces into account: the line of fifths (dimensionality reduction, clustering, t-SNE, PCA)
  5. historical expansion on line of fifths, LOWESS

---

class: center, middle

## Methodological Considerations

---

class: center

<img src="img/meyer_hierarchy.jpg" width="75%">

.ref[Jan (2007). *Memetics and Music*. <br /> Meyer, L.B. (1989). *Music and Style*.]

---

class: center, middle

# Many thanks to...

.left-col50[<img src="img/dcml.jpg" width=500>]

.right-col50[.left[
- Diana Kayser for organizing SysMus this year

- my colleagues at the DCML

- Claude Latour for supporting the EPFL Chair in Digital Musicology

- SNSF for funding the _Distant Listening_ project
]]

.center[<img src="img/SNF.png" width=200>]

.left[
.right-col50[
These slides can be found at <br /> [http://fabian-moss.de/talk/](http://fabian-moss.de/talk/)
]
]


---

## Discussion

and Feedback: email/twitter/forms?

---

class: center, middle
# I. Problems

---

## I Problems: The peculiar case of music

**Practical problems**
- inferring musical structures/textual representations reliably from audio is very difficult
--

- OMR relatively bad; small mistakes can make huge difference
--

- scarcity of (text) corpora (as opposed to audio recordings)

--

Empirical basis for Digital Musicology usually _much_ smaller than for computational linguistics
or literary studies (sources like Wikipedia or the web as a whole don't exist)

--

**Methodological problems**
- What is musical text? How does it differ from linguistic text?
--

- Seemingly 2-dimensional (pitch/time) but pitch-dimension actually multidimensional
<!-- - the score can be understood as dimensionality reduction for performance -->

---
## I. Problems: The peculiar case of music
.center[
<img src="img/spem.png" width=38%><br />
.caption[Thomas Tallis, _Spem in Alium_, 1570.]]


---

class: center, middle
# II. Practices
## 1. Inferring Latent Tonal Spaces

---

## 1. Inferring Latent Tonal Spaces

.left-col50[
Note frequencies in the _Tonal Pitch-Class Counts Corpus_ .cite[(Moss, Neuwirth, & Rohrmeier, 2020)]
.left[.ref[Moss, F. C., Neuwirth, M., & Rohrmeier, M. (2020).
Tonal Pitch-Class Counts Corpus (TP3C) (Version v1.0.0) [Data set].
_Zenodo_. http://doi.org/10.5281/zenodo.3600088]]

- 2012 musical pieces
- 75 composers
- ca. 600 years

**An example piece**
.center[
<img src="img/alkan_dist.png" width=90%>
.caption[Note distribution of Alkan, _Concerto for Solo
Piano_, op. 39/8., mov. 1.]
]

]
--

.right-col50[
Do the frequencies of notes in this dataset entail information about an underlying
latent space?

**Procedure**
1. represent each piece as note distribution (normalized)
2. pieces span a `\(V\)`-dimensional vector space
3. perform dimensionality reduction (PCA)

]

---
## 1. Inferring Latent Tonal Spaces

.left-col50[
.center[
<img src="img/dim_reduct.png" width=80%>]

.center[
<img src="img/lof.png" width=100%>
.caption[The Line of Fifths .cite[(Temperley, 2000)].]]

.ref[
Temperley, D. (2000). The line of fifths. _Music Analysis, 19_(3), 289–319. doi:10.2307/854457
]
]

--

.right-col50[

First two principal components:

<img src="img/principal_components.png" width=100%>

]

---
## 1. Inferring Latent Tonal Spaces

Rearranging the notes of the example piece on the line of fifths reveals a much more
systematic usage.

.center[
<img src="img/tpc_dist.png" width=75%>
.caption[Note distribution of Alkan, _Concerto for Solo
Piano_, op. 39/8., mov. 1.]
]

---

class: center, middle
# II. Practices
## 2. Annotating Corpora with Harmonic Labels

---

## 2. Annotating Corpora with Harmonic Labels

.center[<img src="img/musescore_screenshot.png" width=66%><br />
.caption[Annotation interface _MuseScore_ (v2.0) showing the beginning of Beethoven's op. 74/1.]]

.center[
<audio controls>
  <source src="audio/beethoven_74_beginning.wav" type="audio/wav">
  Your browser does not support the audio element.
</audio>]

???

# Listen carefully and try to hear what changes in the music when there is a new label.

---

## 2. Annotating Corpora with Harmonic Labels

- chord symbols, e.g. `I`, `i`, `V` with certain features
- defined by a regular expression .cite[(Neuwirth, et al., 2018)]

.ref[Neuwirth, M., Harasim, D., Moss, F. C., & Rohrmeier, M. (2018).
The Annotated Beethoven Corpus (ABC): A Dataset of Harmonic Analyses of All Beethoven String Quartets.
*Frontiers in Digital Humanities, 5* (16).
DOI: [10.3389/fdigh.2018.00016](https://doi.org/10.3389/fdigh.2018.00016)]
--

```parser3
REGEX = r"""^
        (\.)?
        ((?P<key>[a-gA-G](b*|\#*)|(b*|\#*)(VII|VI|V|IV|III|II|I|vii|vi|v|iv|iii|ii|i))\.)?
        ((?P<pedal>(b*|\#*)(VII|VI|V|IV|III|II|I|vii|vi|v|iv|iii|ii|i))\[)?
        (?P<numeral>(b*|\#*)(VII|VI|V|IV|III|II|I|vii|vi|v|iv|iii|ii|i|Ger|It|Fr))
        (?P<form>[%o+M])?
        (?P<figbass>(9|7|65|43|42|2|64|6))?
        (\((?P<changes>(\+?(b*|\#*)\d)+)\))?
        (/\.?(?P<relativeroot>(b*|\#*)(VII|VI|V|IV|III|II|I|vii|vi|v|iv|iii|ii|i)))?
        (?P<pedalend>\])?
        (?P<phraseend>\\\\)?$
        """x
```
--

Questions
1. How are the chords distributed?
2. What is the significance of chord features for chord prediction?

--

Corpus
- 289 annotated pieces
- nine 19th-century composers
- over 75,000 labels


---
## 2. Annotating Corpora with Harmonic Labels
### Chord Frequencies

The frequency-rank distribution of chord symbols follows a power law.
.center[
  <img src="img/ABC_zipf_mandelbrot.png" width=75%><br />
  .caption[Chord symbols in Beethoven's string quartets in the major (left) and the minor mode (right).]
]


---
## 2. Annotating Corpora with Harmonic Labels
### Chord Transitions

.left-col50[
  .center[<img src="img/ABC_progressions_major.png" width=90%>]
]

--

.right-col50[
Chord transitions (bigrams) are asymmetric, e.g.
$$p(\\mathsf{V7} \\rightarrow \\mathsf{I}) \\gg p(\\mathsf{I} \\rightarrow \\mathsf{V7})$$
]

--

.right-col50[Conditional entropies (black bars) vary considerably]
--

.right-col50[- High entropies (uncertain continuation):
.center[`I`, `i`, `V`, `IV`, `vi`, `ii`, ...]]
--
.right-col50[- Low entropies (more certain continuation):
.center[`V2`, `V(64)`, `V65/IV`, ...]
]

--
.right-col50[
Variability seems to be related to chord features.
]
---

## Practices: affect of features for chord prediction

.left-col33[
1. Select all chords with a certain feature.
2. Calculate average normalized conditional entropy `\(\bar{H}_{avg}\)`.
3. Draw large number of bootstrap samples .cite[(Efron, 1979)].
4. The proportion of bootstrap samples more extreme than `\(\bar{H}_{avg}\)` determines statistical significance.

]

.ref[
Moss, F. C., Neuwirth, M., Harasim, D., & Rohrmeier, M. (2019).
Statistical characteristics of tonal harmony: A corpus study of Beethoven's string quartets.
_PLOS ONE 14_(6): e0217242. DOI: [10.1371/journal.pone.0217242](https://doi.org/10.1371/journal.pone.0217242)

Efron, B. (1979). Bootstrap methods: Another look at the Jackknife. _The Annals of
Statistics, 7_(1), 1–26. DOI: [10.1214/aos/1176344552](https://doi.org/10.1214/aos/1176344552)
]

--

.right-col66[

.center[<img src="img/ABC_feature_entropies.png" width=85%>]

.center[<img src="img/medtner_feature_entropies.png" width=85%>]

]


---

class: center, middle
# III. Prospects

---

## Prospects
### *Distant Listening* - The Development of Harmony over Three Centuries (1700–2000)

- SNSF funded project
- 1 PI (Martin Rohrmeier), 2 PostDocs (Andrew McLeod, Fabian C. Moss), 1 PhD (Johannes Hentschel)
- 4 years (2019-22)

<img src="img/SNF.png" width=200>

**Scope**
1. Data: corpus creation/expansion/annotation
2. Computational modeling: harmonic inference (using the annotated corpora as ground truth)
3. Theoretical advancement: music theory (re-evaluate musical theories of tonality, digital critique)

More information: [http://dcml.epfl.ch/projects/distant-listening](http://dcml.epfl.ch/projects/distant-listening)

???

But compare to [RISM database](https://opac.rism.info):
- 1,198,605 musical sources (as of 12 Feb)
- ca. 36,000 composers
- mostly between 1650 and 1850 (printed music)

---

class: center, middle

# Many thanks to...

.left-col50[<img src="img/dcml.jpg" width=500>]

.right-col50[.left[
- Nils Reiter for inviting me here

- my colleagues at the DCML

- Claude Latour for supporting the EPFL Chair in Digital Musicology

- SNSF for funding the _Distant Listening_ project
]]

.center[<img src="img/SNF.png" width=200>]

.left[
.right-col50[
These slides can be found at <br /> [http://fabian-moss.de/talk/](http://fabian-moss.de/talk/)
]
]


---

class: center, middle

# Computational Musicology and the Digital Humanities
## Problems, Practices, and Prospects

Fabian C. Moss ([@fabianmoss](https://twitter.com/fabianmoss))<br />
Digital and Cognitive Musicology Lab<br />
École Polytechnique Fédérale de Lausanne

18 February, 2020

CRETA-Werkstatt #9<br />
Center for Reflected Text Analytics, Universität Stuttgart

<img src="img/EPFL.png" width=200 align="middle" hspace="30">
<img src="img/SNF.png" width=300 align="middle" hspace="30">

---
class: center, middle
# II. Practices
## 3. Topic Modeling and Tonality

---
## 3. Topic Modeling and Tonality
--

Topic Modeling with Latent Dirichlet Allocation (LDA)
.center[<img src="img/lda.png" width=75%><br />
.caption[Graphical Model for Latent Dirichlet Allocation .cite[(Blei et al., 2003)].]
]

.ref[
Blei, D. M., Ng, A. Y., & Jordan, M. I. (2003). Latent Dirichlet Allocation. _Journal of Machine
Learning Research_, 3, 993–1022. DOI: [10.1162/jmlr.2003.3.4-5.993](https://doi.org/10.1162/jmlr.2003.3.4-5.993)
]

---
## 3. Topic Modeling and Tonality

--

.left-col50[
Some topics reflect regions in tonal space.
.center[<img src="img/topic_7-2.png" width=100%><br />
.caption["Sharper" notes.]]
.center[<img src="img/topic_7-3.png" width=100%><br />
.caption["Flatter" notes.]]]

--

.right-col50[
Others exhibit a more hierarchical structure.
.center[<img src="img/topic_7-4.png" width=100%><br />
.caption[Central notes emphasizing a C-major triad.]]
.center[<img src="img/topic_7-5.png" width=100%><br />
.caption[Central notes emphasizing a D-minor triad.]]]

--

Topic modeling identifies regions in tonal space (central, flat, sharp)
and moreover points towards the importance of triads and tonal hierarchies.
