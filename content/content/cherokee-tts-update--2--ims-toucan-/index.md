+++
draft = false
date = 2022-04-18T21:52:39Z
title = "Cherokee TTS Update #2 (IMS-Toucan)"
weight = 1650318750

[taxonomies]
authors = ["Michael Conrad"]
categories = ["TTS"]
tags = ["IMS-Toucan", "FastSpeech", "TTS", "Python", "Demo Audio"]

[extra]

featured_image = "content/cherokee-tts-update--2--ims-toucan-/04-08.jpg"

+++

I've been working with using the [IMS-Toucan](https://github.com/DigitalPhonetics/IMS-Toucan) to
create a TTS for Cherokee. My previous attempts with the Tacotron model kept resulting in
problematic models that had a tendency to "ramble" on with "mumblings" in a not entirely
predictable fashion. I maintain a fork of the repo at [Cherokee Language IMS-Toucan](https://github.com/CherokeeLanguage/IMS-Toucan).

The results I've obtained from the IMS-Toucan FastSpeech2 model are much more reliable.

I've attached a copy of three different TTS voices reading the *Two Hunters* story from the **C.E.D.**, pp354-355. [Cherokee Dictionary](https://cherokeedictionary.net/grammar)

<!-- more -->

Unfortunately, the availability of Western Cherokee audio with correct MCO transcriptions is sparse. As much of it is sourced from tape, it is generally unusable for TTS work. I am not aware of any Eastern Cherokee audio with correct MCO transcriptions.

What audio I do have I've manually transcribed into MCO using the **C.E.D.** as a guide to get
pronunciations (hopefully) correctly represented in the transcripts. Github: [Cherokee Audio Data](https://github.com/CherokeeLanguage/cherokee-audio-data).

### Audio and story transcript

Voice template: German Speaker #41 (male)

{{audio (file="_bragging-hunter-ref-de-41.m4a")}}
<a href="_bragging-hunter-ref-de-41.m4a" download>⤓ download</a>

Voice template: American English Speaker #310 (female)

{{audio (file="_bragging-hunter-ref-vctk-p310.m4a")}}
<a href="_bragging-hunter-ref-vctk-p310.m4a" download>⤓ download</a>

Voice template: American English Speaker #341 (female)

{{audio (file="_bragging-hunter-ref-vctk-p341.m4a")}}
<a href="_bragging-hunter-ref-vctk-p341.m4a" download>⤓ download</a>


#### Syllabary

> ᎠᏂᏔᎵ ᎠᏂᏍᎦᏯ ᎠᏂᏃᎭᎵᏙᎮ, ᎠᏫ ᏚᏂᏲᎮ. ᏌᏊᏃ ᎠᏍᎦᏯ ᎦᎶᏇ ᎦᏁᎮ ᏐᎢᏅ Ꮭ. ᎾᏅ ᎦᎶᏇ ᎦᏁᎯ ᎤᏢᏈᏌᏘ ᎨᏎ, ᎠᎴ ᎪᎱᏍᏗ ᏳᏛᏁᎳ ᎠᏢᏈᏍᎨ. ᎠᎾᎢᏒᏅ ᎪᎱᏍᏗ ᏭᏂᎪᎮ ᏙᏧᏩᎢᏢ, ᎾᏅ ᎦᎶᏇ ᎦᏁᎯ ᎩᎳᏊ
> ᎢᏴᏓ ᏫᏚᏍᏓᏲᏞ ᎣᏍᏓᏊ ᏄᎦᏎᏍᏔᏅᎾ ᎢᏳᏍᏗ ᏓᏍᏓᏲᎯᎲ. ᎤᏙᎯᏳᏅ ᏭᏲᏞ ᎠᎴ ᎤᏂᎪᎮ ᎦᏅᎬ. ᎾᏅ ᎦᎶᏇ ᏂᎦᏁᎲᎾ "ᎠᏫ ᎡᏂᏲᎠ!" ᎤᏛᏁ. "ᏥᏲᎡᎦ" ᎤᏛᏁ Ꮎ ᎦᎶᏇ ᎦᏁᎯ, ᎠᏢᏈᏍᎬ. ᎤᎾᏁᎷᎩᏎ ᏙᏧᏩᎢᏢ ᏗᏜ, ᎾᎥᏂᎨᏅ ᏭᏂᎷᏣ ᎤᏂᎪᎮ ᏐᏈᎵ ᎦᏅ. "ᏐᏈᎵᎴ ᎢᎾᏓᎯᏏ" ᎤᏛᏁ Ꮎ ᎤᏲᏢ. "ᎭᏓᎯᏎᎦ" ᎠᎪᏎᎴ.

#### Modified Community Orthography

> Anǐ:táɂli ani:sgaya à:ni:no:halǐ:dô:he, ahwi dù:ni:hyohe. Sà:gwű:hno asgaya galò:gwé ga:ne:he sóɂíhnv́ hlā. Ná:hnv́ galò:gwé ga:ne̋:hi u:dlv̌:kwsati gè:sé, ale go:hű:sdi yǔ:dv̂:ne̋:la à:dlv̌:kwsgé. À:ná:ɂi:sv̋:hnv go:hű:sdi wǔ:ní:go:he do:juwáɂihlv, ná:hnv́ galò:gwé ga:ne̋:hi kilagwu iyv̋:da widǔ:sdáyo:hlé ǒ:sdagwu nǔ:ksestanv̋:na iyú:sdi dà:sdayo:hihv̋. U:do:hiyű:hnv́ wǔ:yó:hlé ale ù:ni:go:hé ganv́:gv̋. Ná:hnv́ galò:gwé nigǎ:né:hv̋:na "Ahwi è:ni:yó:ɂa!", ù:dv:hne. "Ji:yó:ɂê:ga", ù:dv:hne ná galò:gwé ga:ne̋:hi, à:dlv̌:kwsgv́. Ù:ná:ne:lǔ:gî:se do:juwáɂihlv́ dí:dla, naɂv̌:hníge̋:hnv wǔ:ní:luhja ù:ni:go:hé sǒ:gwíli gáɂnv̋. "Sǒ:gwílílê ì:nada:hísi", ù:dv:hné ná u:yo:hlv̋. "Hada:hísê:gá", à:gò:sě:lé.

#### Free translation

> Two men were hunting, looking for deer. One of the men had a gun and the other one didn't. The one with the gun was a braggart and when he did something he bragged about it. Walking along, they saw something moving around in the bushes, and the man with the gun shot at once toward the bushes without paying attention to what he was shooting at. And sure enough he shot it and they saw it fall to the ground. The one without the gun said "we shot a deer." "I shot it," said the one with the gun, bragging. They raced toward the bushes and when they got closer they saw a horse lying there. "We killed someone's horse," said the one who shot it. "You killed it," he was told.

## Citation 🐧

### Introduction of the Toolkit
```
@inproceedings{lux2021toucan,
  title={{The IMS Toucan system for the Blizzard Challenge 2021}},
  author={Florian Lux and Julia Koch and Antje Schweitzer and Ngoc Thang Vu},
  year={2021},
  booktitle={Proc. Blizzard Challenge Workshop},
  volume={2021},
  publisher={{Speech Synthesis SIG}}
}
```

### Adding Articulatory Features and Meta-Learning Pretraining
```
@article{lux2022laml,
  title={{Language-Agnostic Meta-Learning for Low-Resource Text-to-Speech with Articulatory Features}},
  author={Florian Lux and Ngoc Thang Vu},
  year={2022},
  journal={arXiv preprint arXiv:2203.03191},
}
```
