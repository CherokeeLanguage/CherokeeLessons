+++
draft = false
date = 2022-06-27T01:21:43Z
title = "Bound Pronouns App"
weight = 1656292900

[taxonomies]
authors = ["Michael Conrad"]
categories = ["Apps", "Grammar"]
tags = ["Bound Pronouns", "Flash Cards", "Android", "iOS", "Linux", "MacOS", "Windows"]

[extra]
featured_image = "../static/images/bound-pronouns.png"

+++

Learn the bound pronouns of the Cherokee language with spaced-repetition flashcards — free on itch.io.

<!-- more -->

Bound Pronouns is a flashcard challenge trainer for Cherokee bound pronouns. Every challenge comes with synthesized voice audio — neural-network speech generated with IMS-Toucan — so you hear the pitch and cadence of each conjugated form. Headphones are strongly recommended.

**What you practice:**

- Challenges display in Syllabary, Latin, or both — with or without pronunciation markings
- Practice sets include both "bare" and conjugated forms
- An audio-only practice mode, using only the conjugated forms, makes for the real learning experience
- Session lengths: short (5 minutes), standard (10 minutes), or long (15 minutes)
- Per-card time limits you set: infinite, 10, 15, 30, or 60 seconds
- Create several named study sessions — rename or delete them as you like

**How it schedules your study:**

- Tracks your progress and shows only the cards you actually need that day
- Missed a day? The deck never punishes you — scheduling jumps at most one day forward, so it never dumps the whole deck on you at once
- New cards only join your active deck after you've successfully handled your pending cards within the minimum session time

**Platforms:** Windows, macOS, Linux, and Android. The Windows/macOS/Linux download is a plain Java `.jar` file — you need a Java runtime, version 8 or newer, installed to run it. The Android download is a standard APK; install it directly on your device.

## Screenshots

![Bound Pronouns — review screen](/images/bound-pronouns-screenshot-1.png)
![Bound Pronouns — practice screen](/images/bound-pronouns-screenshot-2.png)
![Bound Pronouns — practice screen 2](/images/bound-pronouns-screenshot-3.png)

## Get it

<iframe class="itch-embed" src="https://itch.io/embed/5308063" width="552" height="167" frameborder="0"></iframe>

* [Cherokee Bound Pronouns on itch.io](https://cherokee-language.itch.io/cherokee-bound-pronouns) — free download, name your own price. Visit the page to download for Windows, macOS, Linux, or Android.

Find an issue with the app or the training data? Please report it at the project's GitHub: [https://github.com/CherokeeLanguage/BoundPronouns/issues](https://github.com/CherokeeLanguage/BoundPronouns/issues)

## Changelog

### 3.41

Slower speech to better match normal Cherokee speech rate.

### 3.40

Much improved IMS-Toucan replacement audio. Fixed two card sets with incorrect conjugations.

### 3.36

Tacotron2 replacement audio. No longer using espeak-ng.
Update pronunciations to be displayed in Modified Community Orthography as used by Uchihara, etc.

### 3.35

Conjugation fixes.

### 3.34

Misc adjustments to deck.

### 3.33

Misc adjustments to deck.

### 3.32

Leaner and better focused deck with more core verbs.
Misc conjugated form fixes. Misc English fixes.

### 3.31

Adjust sort order of master deck to better spread challenges across pronoun and verb combinations.

### 3.30

Fix issue with "bound pronouns" list screen caused by "csv" to "tsv" filename extension change.

### 3.29

Change minimum Android SDK requirements.

### 3.28

Change "csv" to "tsv" to correctly identify file contents for editing.

### 3.27

Add missing audio files.
Use "alveolar" version of "d" instead of "dental" version of "d" for espeak-ng generated audio.

### 3.26

Fixes for the h/ɂ alternations in stems based on persons.

### 3.25

Reduce cards needed for ᎢᏨ-, ᎢᏍᎩ-, ᎢᏍᏛ-, and ᏍᎩᏂ- by doing "a or b" forms in the English.
Also updated English wording replacement to hopefully produce better English answer sets.

### 3.24

Fix for loading embedded master deck on android.

### 3.23

Switch to using pre-conjugated master deck. Boost volume on audio files to max.

### 3.22

Switch to Brazilian Portuguese &~ instead of French V~ for "v" and "ṿ".
Some adjustments for when to use become acquainted instead of recognize.

### 3.21

Update espeak.txt file.

### 3.20

Master deck sort order fix.

### 3.19

Pronunciation fix for prefix ᎦᏥ- (I+them-animate).
The master entry in the pronouns table was missing a tone mark.

### 3.18

Adjust master deck sorting so that challenges are no longer grouped by bound pronoun or verb.
Each new challenge introduced and added to your active cards should now be a different bound pronoun.

Does not change the ordering of cards already active in a session slot which are solely ordered based on
your accuracy with correct answers and last show time.

### 3.17

Indicate how many total cards are in the deck.

Updated to newest espeak-ng output using most recently updated Cherokee phoneme definitions.

### 3.06

Updated to newer espeak-ng output using updated phonemes. (Improvements one can hopefully hear made to 'a', 'e', and 'i').

### 3.05

Updated to newer espeak-ng output using updated phonemes. (Improvements one can hopefully hear made to 'h', 'g', 'd', 's', and 'v').

Final nasal vowels at the end of words still need adjustment.

Short vowels which should be devoiced when followed by "h" aren't at this time.

### 3.04

Updated list of audio files in app so that it can see the 11 added files.

#### 3.03

Added additional display options for "No Marks" to hide all tone and cadence marks to provide plain Syllabary and Latin forms.

Added additional display option for "Audio Only" for those not wanting visual clues for the answers.

Added 11 missing audio challenges. Updated to newer espeak-ng output using updated phonemes.

#### 3.02

Updated audio with latest espeak-ng generated output using updated phoneme files.

#### 3.01

Initial version with synthetic voice audio for each of the challenges. Mute will disable the voice auto play.

#### 2.32

Update to latest available libGDX and MobiVM's RoboVM.
Removed Sync due to labor involved with tracking API changes for OAUTH and WEBVIEWS.
