---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Translating Chords"
subtitle: ""
summary: ""
authors: []
tags: [Music Theory]
categories: []
date: 2020-03-10T10:31:31+01:00
lastmod: 2020-03-10T10:31:31+01:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

In this post I will describe how to translate absolute chord symbols, as they can be found, for instance
on community websites such as GuitarPro, TabSite, etc. (also Billboard corpus), to relative Roman numeral representation,
using insights from music theory about the structure of tonal space.

This is the code I wrote for the Choro project (in `custom_functions.py`):

```python
def PC_to_RN(chord, key):

    ### DICTIONARIES

    # line of fifts
    # F=0 because then numbers mod 7 represent the diatonic scale

    lof = {
        'Fb':-7,
        'Cb':-6,
        'Gb': -5,
        'Db': -4,
        'Ab': -3,
        'Eb': -2,
        'Bb': -1,
        'F' : 0,
        'C' : 1,
        'G' : 2,
        'D' : 3,
        'A' : 4,
        'E' : 5,
        'B' : 6,
        'F#': 7,
        'C#': 8,
        'G#': 9,
        'D#': 10,
        'A#':11,
        'E#':12
    }

    # roman numerals
    roman_numerals = {
        0 : 'IV',
        1 : 'I',
        2 : 'V',
        3 : 'II',
        4 : 'VI',
        5 : 'III',
        6 : 'VII'
        }

    # accidentals
    acc = {
        -3 : 'bbb',
        -2 : 'bb',
        -1 : 'b',
         0 : '',
         1 : '#',
         2 : '##',
         3 : '###'
    }

    ### FUNCTIONS

    def get_chord_root(chord):
        chord_regex = re.compile(r"""(?P<root>[NC]?[A-G]+[#b]?)
                                   (?P<type>[m+%o]?)
                                   (?P<added>[67M]*)
                                   (?P<omit3>[omit3]*)
                                   (?P<extensions>(\(?[add]*[#b]?\d*M?\)?)*)\/?
                                   (?P<bass>[A-G]+[#b]*)?
                                 """, re.VERBOSE)

        g = re.match(chord_regex, chord).groups()

        return g

    def get_key_root_mode(key):
        key_regex = re.compile(r"""
                                 (?P<key_root>([A-G])+([#b]*))
                                 (?P<mode>m?)?
                                 """, re.VERBOSE)

        g = re.match(key_regex, key).groups()

        return g

    def lof_distance(chord_root, key_root):
        return lof[chord_root] - lof[key_root]

    def diatonic(lof_distance):
        accidentals, step = divmod(lof_distance, 7)
        return accidentals, step

    ### CALCULATIONS

    # extract chord parts
    chord_parts = get_chord_root(chord)
    c_root = chord_parts[0]

    # NC exception

    if c_root == 'NC':
        return 'NC', 'NC'

    # extract key parts
    key_parts = get_key_root_mode(key)
    k_root, modus = key_parts[0], key_parts[3]

    # calculate distance on line of fifths
    # and transform to mod7 distance (still in fifths order)
    dist = lof_distance(c_root, k_root)
    step = roman_numerals[(dist + 1) % 7]

    # shift on line of fifth for major/minor
    if modus == '':
        shift = 1
    elif modus == 'm':
        shift = 4

    accidental = divmod(dist + shift, 7)[0]

    # transform bass note to scale degree representation
    bass = ''
    if chord_parts[6] is not None:
        bass_note, alt = re.match(r'([A-G])([b#]*)', chord_parts[6]).groups()
        bass = '/' + str((lof[bass_note] - lof[c_root]) * 4 % 7 + 1) + str(alt)

    # outputs
    RN = acc[ accidental ] + step
    RN_symbol = RN + chord_parts[1] + chord_parts[2] + chord_parts[3] + chord_parts[4] + chord_parts[5] + bass

    return RN, RN_symbol
```

## The line of fifths

Generate the (segment of the) line of fifths not as pure dictionary,
but generically construct it from the seven pitches `list(FCGDAEB)`.

Like this methods from the TDM code:

```python
class Tone:

    ### Functions to generate arbitrary line-of-fifths (lof) segment
    steps = {s:idx for s, idx in zip(list('FCGDAEB'), range(7))}
    int_strings = ['+P5', '-P5', '+m3', '-m3', '+M3', '-M3']

    @staticmethod
    def get_lof_no(tpc):
        """Returns tpc from lof number"""
        step = Tone.steps[tpc[0]]
        accs = tpc[1:].count('#') - tpc[1:].count('b')

        return step + 7 * accs

    @staticmethod
    def get_tpc(lof_no):
        """Returns tpc for lof number"""
        a, b = divmod(lof_no, 7)
        d = {v:k for k, v in Tone.steps.items()}
        tpc = d[b]
        if a < 0:
            tpc += abs(a) * 'b'
        if a > 0:
            tpc += abs(a) * '#'
        return tpc

    @staticmethod
    def get_lof(min_tpc, max_tpc):
        """Returns number range on from tpcs"""
        min = Tone.get_lof_no(min_tpc)
        max = Tone.get_lof_no(max_tpc)

        if max < min:
            raise UserWarning(f"{min_tpc} is not lower than {max_tpc}.")
        return [ Tone.get_tpc(l) for l in np.arange(min, max + 1) ]
```
