---
layout: post
title: How Sparse is Sign Language Data? - Or, Is 1080p Video Really Just 39 bits Per Second, When Signing?
---
In which I muse on the dimensionality and Information Density of sign language data, looking at it from a few angles, and guesstimate a signal-to-noise ratio that is very, very small. I estimate an information transfer rate of 40-50 bits per second, and calculate that the resulting signal-to-noise ratio could be incredibly low, somewhere vaguely in the range of 0.00000043.

OK: So you are watching a video, maybe from the [ASLV Bible](https://deafbible.com/ASLV/JHN.1.1). And you get to thinking: there's millions of bits of pixel info in this video, right? Somehow, people can watch a video with millions of bits, and distill the important meaning of what is being signed!

An HD video of an utterance in a sign language contains many, many millions of bits. But the _meaning_ encoded within is... probably much more compact, right?

What even is the ratio of data to meaning here? Let's have a look.

### Caveats and Limitations and Assumptions

Things to consider before taking my word for any of this.

* I don't know ASL or any other sign language first-hand, I'm largely working off secondhand info.
* There's so much more meaning to actual sign language conversations than just identifying individual signs. If that's all you do you are missing out on things like body language, pointing/referencing, role-shifting, etc. etc
* As far as I can tell, the question of exactly how large the ASL lexicon is, is still open. One dictionary collected 4500 signs, and the online ASL-LEX has about 2700, and various online sources give various numbers. But of course there might be more or less depending how you count. And the frequencies matter a lot, etc.
  * [This forum post](https://www.quora.com/How-large-is-the-vocabulary-of-ASL) mentions "many signs" not listed in the dictionaries
* There is a big lack of data or corpora for sign languages! Especially in ML dataset formats.
* I am not a mathematician! I have likely made mistakes!

[Let me know here about anything I messed up](https://forms.gle/E3U6zibEzJVRMfr86)

### Information Theory: How much does ___ narrow things down?

How information is in a message of, say, someone signing the phrase "what is your name" in ASL? There's a lot of answers to that question. You could answer this a number of ways:

1. Just count bits in the raw video. "The video is this many frames long, each frame has this many pixels, each pixel has RGB values...". Counting it this way, you could get a number in the hundreds of millions of bits.
2. Look at the video file size on your hard drive. This will probably be smaller, there's a lot of clever tricks in video formats to save space.
3. You can think of sign languages as 4 dimensional. XYZ coordinates of the hand/fingers/face, etc. plus time. Up to you how many points you want to track there, but MediaPipe tracks [543 points](https://developers.google.com/mediapipe/solutions/vision/holistic_landmarker). Much smaller but still big. Napkin math: 543 points x 3 coordinates x bits per coordinate x number of frames works out to about 1.4 million bits for a 107-frame video!
4. Finally, look at it from a linguistics and information theory standpoint: count "Entropy" for the signs themselves and add them up. What is that exactly?

Well, as I understand it, it's basically something like this: In 1951, Shannon analyzed English text, and found that each word was worth about 11.82 bits per word. Later, in 1964 Grignetti did a followup analysis, finding that it was more like 9.8 bits per word. In both cases the basic idea is that you count up all the words people use, how often each one is used, and then calculate based on that. More common words have got, in a sense, less information in them. They're easier to predict. But uncommon words are more surprising, you get more information.

Or maybe you could think about it like this: Someone starts talking - what are they talking about, specifically? A very common word like "a" or "the" doesn't narrow it down very much. They could be talking about almost any topic. But something _uncommon_ like "Kolmogorov" narrows it down a LOT. In fact, if I'm in a noisy room and that's ALL I hear "something something something Kolmogorov something", there's a good chance they're talking about [Kolmogorov complexity](https://en.wikipedia.org/wiki/Kolmogorov_complexity), or at least some sort of probability thing because that's about the only time [that name](https://en.wikipedia.org/wiki/Andrey_Kolmogorov) comes up. The word "Kolmogorov" gives you a lot of information, and is worth a lot of bits!

"Gauss", on the other hand, [doesn't narrow it down very much at all](https://en.wikipedia.org/wiki/List_of_things_named_after_Carl_Friedrich_Gauss). There's a hundred articles on that list at least. So hearing "something something Gauss" doesn't narrow down the topic as much.

Don't even get started on [Euler](https://en.wikipedia.org/wiki/List_of_things_named_after_Leonhard_Euler). The joke is that in Math [everything is named after the second person to discover it](https://www.flyingcoloursmaths.co.uk/dome-euler-brick/), because otherwise everything would be named after the guy. If you mention Euler you could be talking about anything in the realm of math.

### Information Density of Signs and Sign Languages

OK so we know how much information is in a word, how much information is being transmitted when people communicate, and how quickly? What is the rate, or density? I predict that it is around 40 bits per second, same as spoken languages, and so each sign is much higher in information on average than your typical English word. So maybe 2x, or about 20+ bits per sign?

Pieces of the puzzle:

#### Transmission rate corresponds to spoken languages? So 39.1?

* Bellugi et al (1972) show that each sign takes about twice as much time to produce, but also that it takes the same amount of time to convey the same proposition, so the overall information transmission rate is the same.
* Fischer et al (1999) speed up speech and sign, and they find that comprehension starts breaking down at about the same speed for both. So again, the overall transmission rate seems to be about the same.
* [Coupé et al](https://www.science.org/doi/10.1126/sciadv.aaw2594), analyzing 17 languages, found the "information density" in read speech to be largely consistent across all of them, at around 39.1 bits per second.

So maybe sign languages are also 39.1 bits/second?

#### Entropy Per Sign: hard to calculate but maybe around 10?

* As noted before, Shannon and Grignetti came up with numbers around 10 bits per word for English words.
* Chong et al (2009) analyzed handshape frequencies in ASL, which they compare to analyze phonemes in spoken languages. They studied online vlogs from various websites, and counted, apparently manually - an impressive amount of effort. They note that it is very hard to calculate entropy per _sign_, but they give it a go with handshapes, coming up with a number around 5 bits per handshape.
* Moryossef et al (2023) analyze sign corpora, calculating statistics on the number of handshapes per sign. Which they then apply to the task of segmenting videos into signs and phrases. Out of 705k signs, they found that the vast majority have 2 handshapes, and 3-shape signs are very rare, and 4 even rarer still.

So if I approximate wildly based on all the above, and there are 2 shapes per sign and each shape is worth 5 bits, then does that mean that that two of them together is worth 10? That seems low!

The math seems very suspect on that. Need to check with a mathematician! This is ignoring all the meaning in facial expressions, reference points in space, pointing at things, body posture etc. Given that I can't do the math correctly and I am missing a lot of critical info, I would guess the value of each sign to be higher than this taking all that into account.

#### People can consciously ingest about 40-50 bits of information per second

How much information can people consciously process? Depends how you measure but briefly...

* Miller 1956 was one of the early works looking at human capacity to process information, and how to quantify that.
* Pierce and Karlin 1957 measures a rate of about 40 to 50 bits per second.
* Other sources like Wu et al 2016 measure things like control or ability to make decisions, in which case it gets way lower, single digits.

So it seems that 40-50 bits is a reasonable figure, maybe?

## So what is the ratio?

OK, so we have all those puzzle pieces

Looking at the [Sign Language Processing](https://research.sign.mt/#sign-language-representations) site, we have the example where 107 frames of video is equivalent to 3 signs. Or to put it another way, 107 frames is about 4 seconds of video at 25 fps.

* 107 frames, let's call it 3 bytes per pixel (RGB), let's say 720p.  That'd be around (107) * (1280x720) * (3) = 295 million _bytes_, more like 2.3 BILLION bits. And when you load it into your ML framework it really might be this big, you kind of have to unpack the .mp4 file into frames and feed those into the neural networks.
* A generous estimate of Sign Language transmission rates (take the estimates I made above and double them?) would be something like 100 bits per second transmitted. Or double it again, call it 200. Nevermind the fact that consciously we can only take in something like 40 or 50, there's unconscious understanding as well. That'd give us a total message information for asking someone's name in ASL of about 200 * 4 seconds = 800 bits. Let's generously round that up to 1k bits.

So then out of 2.3 billion bits sent, only about 1k make it into the consciousness of the receiver, giving us a Signal to Noise Ratio of 1k/2.3 billion, or about 4.3e-7.

0.00000043, that's 6 zeroes.

Wow.

### See also

[Stanford explainer on entropy calculations](https://cs.stanford.edu/people/eroberts/courses/soco/projects/1999-00/information-theory/entropy_of_english_9.html)

[An experimental estimation of the entropy of English, in 50 lines of Python code](http://pit-claudel.fr/clement/blog/an-experimental-estimation-of-the-entropy-of-english-in-50-lines-of-python-code/), which explains the concept of entropy of a language quite beautifully.

[startASL](https://www.startasl.com/top-10-25-american-sign-language-signs-for-beginners-the-most-know-top-10-25-asl-signs-to-learn-first/) claims there are "About 10,000 different ASL signs exist that corresponds to English, which has about 200,000 words."

You can [calculate entropies of English text here](https://www.dcode.fr/shannon-index).

#### Google N-Gram

Google N-Gram viewer lets you look at word frequencies over time.

* [kolmogorov,gauss,the](https://books.google.com/ngrams/graph?content=kolmogorov%2Cgauss%2Cthe&year_start=1800&year_end=2019&case_insensitive=on&corpus=en-2019&smoothing=3) shows that "the" dwarfs the other two.
* [kolmogorov,gauss](https://books.google.com/ngrams/graph?content=kolmogorov%2Cgauss&year_start=1800&year_end=2019&corpus=en-2019&smoothing=3&case_insensitive=true) shows that Gauss dwarfs Kolmogorov. Looking at the number of zeroes, it seems Gauss is mentioned about 10x as much.

### Citations

```bibtex
@article{shannonPredictionEntropyPrinted1951,
  title = {Prediction and Entropy of Printed {{English}}},
  author = {Shannon, C. E.},
  year = {1951},
  month = jan,
  journal = {The Bell System Technical Journal},
  volume = {30},
  number = {1},
  pages = {50--64},
  issn = {0005-8580},
  doi = {10.1002/j.1538-7305.1951.tb01366.x},
  url = {https://ieeexplore.ieee.org/document/6773263},
  urldate = {2024-04-18}
}

@article{bellugiComparisonSignLanguage1972,
  title = {A Comparison of Sign Language and Spoken Language},
  author = {Bellugi, Ursula and Fischer, Susan},
  year = {1972},
  journal = {Cognition},
  volume = {1},
  number = {2-3},
  pages = {173--200},
  publisher = {Elsevier Science},
  address = {Netherlands},
  issn = {1873-7838},
  doi = {10.1016/0010-0277(72)90018-2},
  keywords = {Articulation (Speech),Deafness,Sign Language,Speech Rate,Verbal Communication}
}

@article{coupeDifferentLanguagesSimilar2019,
  title = {Different Languages, Similar Encoding Efficiency: {{Comparable}} Information Rates across the Human Communicative Niche},
  shorttitle = {Different Languages, Similar Encoding Efficiency},
  author = {Coup{\'e}, Christophe and Oh, Yoon Mi and Dediu, Dan and Pellegrino, Fran{\c c}ois},
  year = {2019},
  month = sep,
  journal = {Science Advances},
  volume = {5},
  number = {9},
  pages = {eaaw2594},
  publisher = {American Association for the Advancement of Science},
  doi = {10.1126/sciadv.aaw2594},
  url = {https://www.science.org/doi/10.1126/sciadv.aaw2594},
  urldate = {2024-04-18}
}
@misc{chongFrequencyOccurrenceInformation2009,
  title = {Frequency of {{Occurrence}} and {{Information Entropy}} of {{American Sign Language}}},
  author = {Chong, Andrew and Sankar, Lalitha and Poor, H. Vincent},
  year = {2009},
  month = dec,
  number = {arXiv:0912.1768},
  eprint = {0912.1768},
  primaryclass = {cs, math},
  publisher = {arXiv},
  doi = {10.48550/arXiv.0912.1768},
  url = {http://arxiv.org/abs/0912.1768},
  urldate = {2024-04-18},
  archiveprefix = {arxiv},
  keywords = {Computer Science - Information Theory}
}

@article{grignettiNoteEntropyWords1964a,
  title = {A Note on the Entropy of Words in Printed {{English}}},
  author = {Grignetti, Mario C.},
  year = {1964},
  month = sep,
  journal = {Information and Control},
  volume = {7},
  number = {3},
  pages = {304--306},
  issn = {0019-9958},
  doi = {10.1016/S0019-9958(64)90326-2},
  url = {https://www.sciencedirect.com/science/article/pii/S0019995864903262},
  urldate = {2024-04-18}
}


@article{pierceReadingRatesInformation1957,
  title = {Reading {{Rates}} and the {{Information Rate}} of a {{Human Channel}}},
  author = {Pierce, J. R. and Karlin, J. E.},
  year = {1957},
  month = mar,
  journal = {Bell System Technical Journal},
  volume = {36},
  number = {2},
  pages = {497--516},
  issn = {00058580},
  doi = {10.1002/j.1538-7305.1957.tb02409.x},
  url = {https://ieeexplore.ieee.org/document/6773165},
  urldate = {2024-03-14},
  langid = {english}
}

@article{fischerEffectsRatePresentation1999a,
  title = {Effects of Rate of Presentation on the Reception of {{American Sign Language}}},
  author = {Fischer, S. D. and Delhorne, L. A. and Reed, C. M.},
  year = {1999},
  month = jun,
  journal = {Journal of speech, language, and hearing research: JSLHR},
  volume = {42},
  number = {3},
  pages = {568--582},
  issn = {1092-4388},
  doi = {10.1044/jslhr.4203.568},
  langid = {english},
  pmid = {10391623},
  keywords = {Adult,Deafness,Female,Humans,Male,Middle Aged,Phonetics,Semantics,Sign Language,Time Factors,Videotape Recording,Visual Perception}
}

@inproceedings{segmentation:moryossef-etal-2023-linguistically,
  title = {Linguistically Motivated Sign Language Segmentation},
  booktitle = {Findings of the Association for Computational Linguistics: {{EMNLP}} 2023},
  author = {Moryossef, Amit and Jiang, Zifan and M{\"u}ller, Mathias and Ebling, Sarah and Goldberg, Yoav},
  editor = {Bouamor, Houda and Pino, Juan and Bali, Kalika},
  year = {2023},
  month = dec,
  pages = {12703--12724},
  publisher = {Association for Computational Linguistics},
  address = {Singapore},
  doi = {10.18653/v1/2023.findings-emnlp.846},
  url = {https://aclanthology.org/2023.findings-emnlp.846}
}
@article{millerMagicalNumberSeven1956,
  title = {The Magical Number Seven, plus or Minus Two: {{Some}} Limits on Our Capacity for Processing Information.},
  shorttitle = {The Magical Number Seven, plus or Minus Two},
  author = {Miller, George A.},
  year = {1956},
  month = mar,
  journal = {Psychological Review},
  volume = {63},
  number = {2},
  pages = {81--97},
  issn = {1939-1471, 0033-295X},
  doi = {10.1037/h0043158},
  url = {https://doi.apa.org/doi/10.1037/h0043158},
  urldate = {2024-03-14},
  langid = {english},
  keywords = {Humans,Mental Processes,Psychometrics,PSYCHOMETRICS,STATISTICS,Statistics as Topic}
}


@article{wuCapacityCognitiveControl2016,
  title = {The {{Capacity}} of {{Cognitive Control Estimated}} from a {{Perceptual Decision Making Task}}},
  author = {Wu, Tingting and Dufford, Alexander J. and Mackie, Melissa-Ann and Egan, Laura J. and Fan, Jin},
  year = {2016},
  month = sep,
  journal = {Scientific Reports},
  volume = {6},
  pages = {34025},
  issn = {2045-2322},
  doi = {10.1038/srep34025},
  url = {https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5034293/},
  urldate = {2024-03-14},
  pmcid = {PMC5034293},
  pmid = {27659950}
}


```

## Changelog

* 2024-04-18: article creation. 
