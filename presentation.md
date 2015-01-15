---
author: Vik Paruchuri
title: Evolving your own music
---
# Evolving your own music for fun and no profit
## Vik Paruchuri
## DataQuest (www.dataquest.io)
## vik.paruchuri@gmail.com

---
## What's evolving music?

* Take existing music
* Learn patterns from it
* Use the knowledge to make new music
* Judge your new music

---
## Why?

* Because it's cool
* See above reason

---
## Listen in

* Sample tracks: [1](https://s3.amazonaws.com/presentation-files/d5bcbeeb-7bca-446a-b5dd-c85bbb2918a8.ogg) [2](https://s3.amazonaws.com/presentation-files/7a5b8454-2e1c-4b4a-b622-966e85d713e5.ogg) [3](https://s3.amazonaws.com/presentation-files/e49d8301-3963-489f-876f-1229473b93b0.ogg)

---
## Midi format

    [midi.ProgramChangeEvent(tick=0, channel=0, data=[0]),
    midi.NoteOffEvent(tick=20, channel=9, data=[42, 64]),
    midi.NoteOnEvent(tick=28, channel=9, data=[38, 90]),
    midi.NoteOnEvent(tick=0, channel=9, data=[42, 90]),
    midi.NoteOffEvent(tick=20, channel=9, data=[38, 64]),
    midi.NoteOffEvent(tick=0, channel=9, data=[42, 64]),
    midi.NoteOnEvent(tick=28, channel=9, data=[42, 70]),
    midi.NoteOffEvent(tick=20, channel=9, data=[35, 64]),
    midi.NoteOffEvent(tick=0, channel=9, data=[42, 64]),
    midi.NoteOnEvent(tick=28, channel=9, data=[42, 58]),
    midi.EndOfTrackEvent(tick=0, data=[])]

---
## MIDI stored as byte code
 
![midi](https://vik-affirm-assets.s3-us-west-1.amazonaws.com/making-instrumental-music-from-scratch/midi_bytes.png)
 
---
## Python-midi to the rescue!

* Parses midi into python data structures
* Found at https://github.com/vishnubob/python-midi

---
## Step 1: ACQUIRE MUSIC

* Use scrapy -- http://scrapy.org/
* Write crawlers to scrape sites
* Download .mid files

---
## Step 2: Render tracks

* MIDI has to be "rendered"
* Output to .ogg files

---
## Step 45: Markov chain everything!

![Markov chain](https://vik-affirm-assets.s3-us-west-1.amazonaws.com/making-instrumental-music-from-scratch/markov-chain.png)

---
## Construct chains

* Make matrices for pitch, velocity, tick per-instrument
* Rows and columns are numbers
* Cells contain # of transitions from row state to column state

---
## Note "phrases"

* Music has structure
* Phrases are basic units of it
* I have almost no music theory experience
* Let me know after if I'm wrong
* Pull out sequences of 8 notes

---
## Making a track

* Select the instrument
* Randomly select a starting phrase
* Use markov chains to pick next phrase
* Repeat until specified length

---
## Making a song

* Pick instruments that are good together
* Put tracks from those instruments together
* Add a tempo track

---
## Evaluating song

* Render midi to ogg file
* Use fluidsynth, oggenc, and sox

---
## Look at the audio

![audio](https://vik-affirm-assets.s3-us-west-1.amazonaws.com/making-instrumental-music-from-scratch/song_10s.png)

---
## Read audio in

$$\begin{bmatrix}
2.35185598e-05 & -1.04448336e-05\\\\
-3.46823663e-06 & -3.73403673e-05\\\\
-2.69492170e-06 & -1.44758296e-05\\\\
9.47549870e-06 & 2.09419904e-05\\\\
-2.70856035e-05 & 3.44590421e-06\\\\
-3.01332675e-05 & 2.74870854e-05\\\\
-1.44664727e-06 & 7.49632018e-05\\\\
-3.80197125e-05 & 2.56412422e-05\\\\
-5.61815832e-05 & -1.29676855e-05\\\\
-4.73532873e-06 & 3.69851950e-05
\end{bmatrix}$$

---
## Feature calculation

* Calculate a bunch of features
* Split song into "bins"
* ZCR, slope, peak count, and many more
* 154 in all
* Put into matrix

---
## Scoring songs

* Compare generated song with existing songs
* Used random forest at one point to predict
* Simpler to use cosine distance
* Lower distance is better

---
## Semi-genetic algorithm

* Create candidate songs
* Score them all
* Pick the best ones
* Remix songs by swapping tracks
* Generate some new songs

---
## Generations

* Evolve multiple generations
* Try to minimize distance from existing songs
* Algorithm finds local minimum quickly
* More than 3 generations gets duplicate "best" song

---
## Ways to make it better

* Hand-label some songs, and use them for supervised learning
* Get more training data
* Generate better features
* Do more in the genetic algorithm than just remix

---
## Open source

* Available at https://github.com/VikParuchuri/evolve-music2
* This presentation available at: https://github.com/VikParuchuri/pydataboston2015
* Want to learn how to do things like this?  Check out www.dataquest.io!