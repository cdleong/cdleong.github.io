---
layout: post
title: A Coder's Guide to Sign Language Translation Research in 2024
permalink: /slt-guide/
---
![My Name is Colin]({{ site.baseurl }}/images/slt_for_coders/self_intro_asl.gif) ([`MY`](https://www.handspeak.com/word/1448/) [`NAME`](https://www.handspeak.com/word/1464/) [`C-O-L-I-N`](https://www.handspeak.com/learn/408/), aka "My name is Colin" in [ASL](https://www.ethnologue.com/language/ase/), though I flubbed the "N" a bit)
So, it's 2024, and AI is cool, and you want to try your hand (haha) at sign language translation! Not recognizing _finger-spelling_ in static images, you want to do full-on _translation_: videos come in with someone signing in one of the [many sign languages that are out there](https://www.ethnologue.com/subgroup/2/), and full sentences come out the other side in some spoken language. Where does one even start? Here's some notes and starting from a coder's perspective, focusing on what seems the most viable and pragmatic from a coding standpoint.

About me: I am from a primarily computer-vision and low-resource NLP background but am not a member of any Deaf community and my attempts to learn a sign language are thus far quite limited. So my perspective quite likely has many gaps, limitations and biases. Read accordingly! But I want to learn humbly, and share what I've learned, and maybe be of service to others. Thus this post!

**NOTE: These are my own informal (and often incomplete) notes/thoughts on this subject, from a coder's perspective primarily. See ["Sign Language Processing"](https://research.sign.mt/) by Moryossef, Amit and Goldberg, Yoav for a much more well-researched and professional guide, and [Systemic Biases in Sign Language AI Research: A Deaf-Led Call to Reevaluate Research Agendas](https://arxiv.org/abs/2403.02563) for a review from the perspective of researchers who know much better than me. Mistakes are my own, if I've messed something up, or you have any suggestions, please let me know [here](https://forms.gle/E3U6zibEzJVRMfr86)!**

<!-- TODO: TOC here? https://heymichellemac.com/table-of-contents-jekyll https://stackoverflow.com/questions/9602936/how-to-create-a-table-of-contents-to-jekyll-blog-post https://stackoverflow.com/questions/18244417/how-do-i-create-some-kind-of-table-of-content-in-github-wiki-->

## A Coder's Guide to SLT (for me)

Truth be told, I am writing this to be the guide I wish I had - the guide that a coder needs, in order to Do Science within the realm of Sign Language to Text. Nevertheless, [I welcome feedback/suggestions]((https://forms.gle/E3U6zibEzJVRMfr86))!

Hardware Prerequisites: you will likely be working with video data, so you will need:

* LOTS of hard drive space.
* LOTS of RAM.

Don't panic, though! Even ithout these, your SL research may require some creativity in order to workaround these issues, but you need not let that discourage you. See [Sebastian Ruder's blog post "NLP Research in the Era of LLMs"](https://newsletter.ruder.io/p/nlp-research-in-the-era-of-llms?utm_campaign=post&utm_medium=web) for a few low-compute inspirations, for example.

I actually had _some_ success getting things running in Colab Pro, but of course there's plenty of issues with that too. Anyway...
<!-- TOC created with VSCode Markdown all in one extension -->
- [A Coder's Guide to SLT (for me)](#a-coders-guide-to-slt-for-me)
- [tl;dr](#tldr)
- [The Problem: Challenges for Sign Language Translation](#the-problem-challenges-for-sign-language-translation)
  - [Sign Languages are Natural Languages, Natural Language Processing is Hard](#sign-languages-are-natural-languages-natural-language-processing-is-hard)
    - [A selection of interesting visual/linguistic challenges](#a-selection-of-interesting-visuallinguistic-challenges)
  - [Sign Languages are Visual, Spatial, Simultaneous, and Nonlinear](#sign-languages-are-visual-spatial-simultaneous-and-nonlinear)
  - [Sign Language Data is hard to work with, computationally](#sign-language-data-is-hard-to-work-with-computationally)
    - [Dimensionality of Sign Language Data is High](#dimensionality-of-sign-language-data-is-high)
  - [Sign Language Datasets are Hard To Make, there are not so many as text](#sign-language-datasets-are-hard-to-make-there-are-not-so-many-as-text)
  - [Sign Language Translation Research in 2024 is the Wild West... in 1861](#sign-language-translation-research-in-2024-is-the-wild-west-in-1861)
  - [Sign Languages are real languages used by real people and real communities](#sign-languages-are-real-languages-used-by-real-people-and-real-communities)
- [What Codebases are out there?](#what-codebases-are-out-there)
  - [Useful tools/libraries](#useful-toolslibraries)
    - [Pose-Format](#pose-format)
    - [MediaPipe Holistic Hand Landmarker](#mediapipe-holistic-hand-landmarker)
    - [Video Feature libraries/code](#video-feature-librariescode)
    - [`video_features`](#video_features)
      - [i3dFeatureExtraction](#i3dfeatureextraction)
  - [SLT Resources that seem replicable/usable](#slt-resources-that-seem-replicableusable)
    - [Sign Language Datasets](#sign-language-datasets)
    - [Two-Stream Network for Sign Language Recognition and Translation, NeurIPS2022](#two-stream-network-for-sign-language-recognition-and-translation-neurips2022)
    - [KnowComp submission for WMT SLT](#knowcomp-submission-for-wmt-slt)
    - [WMT SLT 2023 Sockeye Baseline](#wmt-slt-2023-sockeye-baseline)
    - [WMT-SLT23 I3D baseline, aka "Sign Language Translation from Instructional Videos"](#wmt-slt23-i3d-baseline-aka-sign-language-translation-from-instructional-videos)
    - [Conditional Variational Autoencoder for Sign Language Translation with Cross-Modal Alignment](#conditional-variational-autoencoder-for-sign-language-translation-with-cross-modal-alignment)
  - [Needs more effort to get running](#needs-more-effort-to-get-running)
    - [SLTUnet](#sltunet)
    - [Tackling Low-Resourced Sign Language Translation: UPC at WMT-SLT 22. One of the submissions to the WMT SLT '22 contest](#tackling-low-resourced-sign-language-translation-upc-at-wmt-slt-22-one-of-the-submissions-to-the-wmt-slt-22-contest)
  - [Not yet fully assessed](#not-yet-fully-assessed)
    - [Isolated Sign Recognition from RGB Video using Pose Flow and Self-Attention](#isolated-sign-recognition-from-rgb-video-using-pose-flow-and-self-attention)
    - [Temporal Lift Pooling for Continuous Sign Language Recognition](#temporal-lift-pooling-for-continuous-sign-language-recognition)
    - [Spatial-Temporal Graph Convolutional Networks for Sign Language Recognition](#spatial-temporal-graph-convolutional-networks-for-sign-language-recognition)
    - [Read and Attend: Temporal Localisation in Sign Language Videos](#read-and-attend-temporal-localisation-in-sign-language-videos)
    - [Better Sign Language Translation with STMC-Transformer](#better-sign-language-translation-with-stmc-transformer)
    - [Sign Language Transformers (CVPR'20)](#sign-language-transformers-cvpr20)
  - [Seems out of date](#seems-out-of-date)
    - [OpenHands (possibly a dead project?)](#openhands-possibly-a-dead-project)
  - [Tricks for getting old code working](#tricks-for-getting-old-code-working)
- [Good places to look for more](#good-places-to-look-for-more)
  - [research.sign.mt - best place for SLT background](#researchsignmt---best-place-for-slt-background)
  - [sign.mt and sign-language-processing github orgs](#signmt-and-sign-language-processing-github-orgs)
  - [👋 Sign Translate Wiki](#-sign-translate-wiki)
  - [WMT Shared Tasks](#wmt-shared-tasks)
    - [WMT-SLT22](#wmt-slt22)
    - [WMT-SLT23](#wmt-slt23)
  - [PapersWithCode](#paperswithcode)
- [Theories and Architectures](#theories-and-architectures)
  - [What's the general trend for sign language translation?](#whats-the-general-trend-for-sign-language-translation)
  - [Pose: everyone uses one of two options, but mostly MediaPipe](#pose-everyone-uses-one-of-two-options-but-mostly-mediapipe)
  - [Interesting/Novel, I want to learn more](#interestingnovel-i-want-to-learn-more)
- [Papers that do CNN+LSTM of some kind](#papers-that-do-cnnlstm-of-some-kind)
- [Survey of Sign Language Processing Surveys](#survey-of-sign-language-processing-surveys)
  - [SLP in general](#slp-in-general)
    - [Sign Language Recognition, Generation, and Translation An Interdisciplinary Perspective (2019)](#sign-language-recognition-generation-and-translation-an-interdisciplinary-perspective-2019)
    - [Systemic Biases in Sign Language AI Research A Deaf-Led Call to Reevaluate Research Agendas (2024)](#systemic-biases-in-sign-language-ai-research-a-deaf-led-call-to-reevaluate-research-agendas-2024)
  - [Specifically SLT](#specifically-slt)
    - [Machine Translation from Signed to Spoken Languages State of the Art and Challenges (2022)](#machine-translation-from-signed-to-spoken-languages-state-of-the-art-and-challenges-2022)
    - [A survey on Sign Language machine translation (2023)](#a-survey-on-sign-language-machine-translation-2023)
    - [Sign Language Translation A Survey of Approaches and Techniques (2023)](#sign-language-translation-a-survey-of-approaches-and-techniques-2023)
    - [Sign Language Processing (research.sign.mt)](#sign-language-processing-researchsignmt)
  - [Other SLP tasks](#other-slp-tasks)
    - [Unraveling a Decade A Comprehensive Survey on Isolated Sign Language Recognition (2023)](#unraveling-a-decade-a-comprehensive-survey-on-isolated-sign-language-recognition-2023)
    - [A Extensive Survey on Sign Language Recognition Methods (2023)](#a-extensive-survey-on-sign-language-recognition-methods-2023)
    - [Sign Language Production: A Review (2021)](#sign-language-production-a-review-2021)
    - [Sign Language Recognition: Current State of Knowledge and Future Directions (2023)](#sign-language-recognition-current-state-of-knowledge-and-future-directions-2023)
  - [Miscellaneous](#miscellaneous)
    - [Metrics](#metrics)
    - [Comparative Analysis of Multiple ML Models and Real-Time Translation (2023)](#comparative-analysis-of-multiple-ml-models-and-real-time-translation-2023)
    - [Indian Sign Language Recognition Using Classical And Machine Learning Techniques A Review (2023)](#indian-sign-language-recognition-using-classical-and-machine-learning-techniques-a-review-2023)
    - [Quantitative Survey of the State of the Art in Sign Language Recognition (2020)](#quantitative-survey-of-the-state-of-the-art-in-sign-language-recognition-2020)
- [Ideas and Potential Starting Points](#ideas-and-potential-starting-points)
  - [Ctrl-F for Signs?](#ctrl-f-for-signs)
  - [Compress and Simplify?](#compress-and-simplify)
- [Changelog](#changelog)

## tl;dr

Currently, my advice to past-me would be:

1. Read [this](https://research.sign.mt/) to learn about the state of the field, and [this](https://arxiv.org/abs/2403.02563) and [this](https://signon-project.eu/wp-content/uploads/2023/12/White-Paper-Sign-Language-Technology-FINAL.pdf) to get some important perspective on what research to even do, and how to do it right.
2. Join [this slack channel](https://join.slack.com/t/signlanguager-kgn3800/shared_invite/zt-2fywilz3k-h_nctcijICnqoAPAfnNd~A) to ask questions.
3. _Only then_ go to the [codebases](#what-codebases-are-out-there) section below and have a go at whatever seems most viable.
4. Use [this library for datasets](https://github.com/sign-language-processing/datasets).

Read on for more details.

## The Problem: Challenges for Sign Language Translation

There's a few things that make Sign Language Translation a difficult problem, especially from my perspective coming from the machine translation and computer vision background.

### Sign Languages are Natural Languages, Natural Language Processing is Hard

It may seem like a redundant statement, a Profound Insight Into the Obvious. But the fact that Sign Languages _are languages_ makes it really difficult to create a good translation system. With something like hand-gesture recognition or pose estimation or object recognition, you can get good results without a lot of context.

But these are _natural languages_. Which means they have all the incredible complexities that the field Linguistics revels and delights in. Phonetics, Phonology, Morphology, Lexicology, Syntax, Semantics, Pragmatics... languages are complex and beautiful and so, so hard to fully describe.

With _languages_ you have _grammar_. One part of the sentence affects the other part of the sentence. The word "it" in "the dog couldn't cross the street because it was too wide" refers to something different than if you said "...because it was too tired". That matters, especially when translating into some other language with yet another grammar and syntax.

With languages you have _morphology_. Words and signs can change their form, or be modified by other ones. You might have combinations of signs that change the meaning or form of others. Like how in English you have suffixes or affixes like "-ing" or "-er", e.g. "teach" plus "-er" is "teacher". Well, in American Sign Language you can [sign "TEACH" and then a version of "PERSON" to mean "teacher"](https://www.lifeprint.com/asl101/pages-signs/t/teacher.htm). Translating that as "teach person" would not really capture the meaning right!

And then there's things like idioms, and proper nouns or named entities, and sometimes little one-off things which don't fit the typical pattern of a language, which you just need to sometimes memorize and use. Languages are fascinating, beautiful, and deeply complicated.

If you want a really deep dive, have a read of [this textbook](https://www.cambridge.org/core/books/sign-language-and-linguistic-universals/3DCD779D499F59838FBC2FC01C0092BC), pretty much the seminal work on Sign Languages and their linguistics.

#### A selection of interesting visual/linguistic challenges

* [Classifier predicates](https://www.handspeak.com/learn/17/). Morphology in action here, you have a predicate sign which modifies the meaning of a subject sign.
* [Indexing](https://www.lifeprint.com/asl101/pages-layout/indexing.htm), where you point at a person or thing. Which can be absent actually, you can for example point off to the side to indicate an abstract "they". Or you point at someone who is actually there.
* [Role Shifting](https://www.handspeak.com/learn/16/), where one assumes a role by for example shifting your head, stepping to one side or the other. I saw a Sign Language Interpreter face up as if to Heaven when translating prayers to God, and face down as if speaking from above when interpreting a divine statement. In [A Unified Contrastive Analysis of Lateral Shift in American Sign Language](https://projects.iq.harvard.edu/files/meaningandmodality/files/joyce_poster_final.pdf), Taylor Joyce does linguistic analysis on the phenomenon, looking at different ways body shifts can convey meaning.
* [Lexicalization](https://www.lifeprint.com/asl101/topics/lexicalization.htm). Lifeprint describes lexicalization as "the process of making words by squishing a string of words, or words parts into one word (or group of words that you treat as one concept) and/or borrowing a word from another language." Which reminds me very much of how in spoken languages you get words smushed together like "you all" becoming "y'all", or "you all must have" -> "y'allmst've". Yes I believe that's a real one, I've heard reports of it being used. I'm also a fan of "imma".
* [Loan Signs](https://www.lifeprint.com/asl101/topics/loansigns.htm). Yup, just like spoken languages, sign languages can borrow words from each other. Sign languages are natural languages, natural languages grow and change and morph over time!

**Implications:**

To really translate any language properly, you would need to be able to take all these things into account! We struggle to do this, even for spoken languages, recorded with text!

Of course, even an imperfect solution can still be useful! If I'm using Google Translate with some language and I realize it's not super accurate, I may still use it to translate simple things like "bathroom" and "where"!

* Like any natural language, with a sign language we would like to have some way to capture the context of the whole phrase or sentence, and ideally the whole conversation. Single-frame methods just won't do.
* We would also like to have some way to understand and take into account the wider patterns of the language. How words combine, how certain words are used typically... grammar and idioms and expressions and morphology and even more. Which means for any particular language, we need to have a representative sample of language as a whole, and somehow encode that knowledge into our translation method.

### Sign Languages are Visual, Spatial, Simultaneous, and Nonlinear

To say that Sign Languages are Visual is another Profoundly Obvious Statement. And at first that might not seem such a difficult thing. After all, computer vision is getting very, very good these days! We can, sometimes at hundreds of frames per second:

* Classify objects into thousands of categories.
* Segment videos for self-driving cars, identifying where the road is, where the people are, and so on.
* Identify the position of someone's hands.
* Track and recognize faces.
* ...[and many more tasks!](https://paperswithcode.com/methods/area/computer-vision)

But here's just a few of the complications that make this a challenging problem from a computer vision perspective:

* "Spatial Referencing". For example, in ASL, you can point at a location in space and articulate a sign, maybe something like "mother". And then later in the conversation, you point there, and people know you're referring to "mother". And you literally can just do something like: make the sign for ASK, and then physically move it from that location towards yourself to communicate "mother asked me". So now your method needs to be able to track abstract locations in physical space.
* Pointing at things: "pointing" isn't the precise linguistic term or anything, but it's perfectly allowed in ASL to, say, sign ["CUP"](https://www.lifeprint.com/asl101/pages-signs/c/cup.htm) with one hand, and _point_ at a cup with another hand, and communicate thusly "_that_ cup". So now your method needs to be able to recognize (a) pointing at things, and (b) classify and localize _arbitrary objects_.
  * Yes, CUP is listed in that link as a two-handed sign. Yes, it's still possible, you modify the sign to do this. No, that's not in most of the dictionaries or datasets, though you might find things like this in corpora that show real conversations in a culturally Deaf community. 
    * But even then it might be hard, because they're recorded in a studio or a lab, etc. For example the [DGS Corpus](https://www.sign-lang.uni-hamburg.de/meinedgs/ling/start-name_de.html), where they're sitting in a mostly empty room.
    * [Here's one from SpreadTheSign](https://media.spreadthesign.com/video/mp4/13/93806.mp4) translated as "that is a pretty mousepad", where they change the dominant handshape to refer to an object, but since there's not actually a mousepad in the room they have to introduce the referent in space first. Presumably if there was one actually sitting there they could just point at it. (thanks to Amit Moryossef for referring me to this).
      * Oh, there's versions in other SLs [here](https://www.spreadthesign.com/en.us-to-es.es/sentence/9764/esa-almohadilla-para-raton-es-muy-bonita/?q=mouse+pad).
* Simultaneity: just considering the hands and head and face, there's about 4 things to pay attention to at the same time. Meanings of signs can be modified by facial expressions, or head nods, or where someone is looking. This is one of the ways that sign languages are able to commmunicate the same amount of information per unit of time as spoken languages do.
* Nonlinearity: Because of all the simultaneity, the multiple different things changing/moving at the same time, it is... challenging to try and represent SL as a linear sequence of writing or symbols, meaning that if your idea is to just transcribe it to some sequence and dump _that_ into an existing NLP method, well... it might not work as well as you hoped. I mean, how would you transcribe the "that cup" example, even? Write some symbol for "that" as a superscript on top of "CUP" to show they're at the same time? Essentially you have to have multiple "tracks" of symbols for all the different body parts involved.
  * [Sutton SignWriting](https://www.sutton-signwriting.io/) might be the closest? It encodes positions and movements of various body parts. It's not really for writing down any specific sign language, it seems you can think of this as the sign language equivalent of writing down phonetic symbols in IPA.

**Implications:**

* Definitely remember that Sign Languages are not text. Any method that works great for text will likely need modifications.
* Spend some time thinking about methods, and especially representations, that can deal multiple things going on at once simultaneously. Multi-object tracking algorithms maybe?
* Think about ways for your method to consider the spatial context of the whole scene. The banana on the table, the position in the air where someone fingerspelled a name, the places people stand, and then shift their body... all of that.

### Sign Language Data is hard to work with, computationally

When you are trying to do sign language processing you have to deal with some fundamental trade-offs which are just difficult to handle.

* If you use raw videos, they are _huge_. Often tens or hundreds of Gigabytes. They take hundreds of times as much RAM and hard drive space as a text dataset with the same number of sentences.
  * They take hours to download and fill your hard drive up to bursting.
  * Your standard Google Colab notebook will crash just from trying to load the data into memory if you're not careful.
  * Your standard Transformer model's memory usage goes up quadratically with context length. If you try to load in hundreds of video frames, it will choke and your GPU may spontaneously combust. [Though many folks are working on that](https://arxiv.org/abs/2402.02244).
* If you strip out information, you, well, lose subtleties. Having a wireframe skeleton showing the pose/positions you might miss things like: where are they looking? Are they making a happy face or a sad face?

#### Dimensionality of Sign Language Data is High

Sign Language Data is highly dimensional, and the signal to noise ratio is really high. See [my musings on this]({{ site.baseurl }}/sign-bits/).

**Implications:**

* It would be nice to make the representation you pick [as simple as possible, but not simpler!](https://quoteinvestigator.com/2011/05/13/einstein-simple/). Smaller formats are easier to work wish, easier to process, easier to share. But lose information. Think about what you are losing!
* Here is a place we could draw inspiration from other fields with high-dimensional data. Perhaps things like Hyperspectral Imagery work?

### Sign Language Datasets are Hard To Make, there are not so many as text

To make a spoken-language (e.g. Swahili, English, Thai) dataset you can do things like...

* Scan a printed book or document.
* Scrape a website or social media.
* Re-format a Bible translation and/or supplementary materials.
* Get some volunteers who know the language and how to write it, to just type some things up.
* Record some volunteers who speak the language and write down what they said.

There's tools for all of these things. Segmenters, dedupers, tokenizers, you name it. Text is easy to work with, easy to store. You can have thousands of sentences in one compressed file, small enough to email it. And the text is anonymous. Once you've typed the words down you can pull out any private or identifying info then you're pretty much set. Not that there's not a million and one ways to do this unethically. Just ask the [African Language NLP community at Masakhane](https://www.masakhane.io/community) about all the... how to put this nicely, the methods they felt did not really help their communities?

(If you're looking for a positive and inspiring example, Masakhane's participatory research practices might make for an encouraging read, by the way! For example [Participatory Research for Low-resourced Machine Translation: A Case Study in African Languages](https://aclanthology.org/2020.findings-emnlp.195/). They have certainly been kind and encouraging to me!)

With Sign Language data on the other hand, you basically have to _record a video of someone_. A real, specific person, and their face and hands. This presents a number of issues right off the bat.

* Very reasonably, these people may not wish you to spread videos of them around without asking.
* It is expensive to set up! You need to record a video with some sort of camera. You might need a studio.
* If you pulled it off a website, do you have access legally? I assisted with a dataset like this, and we wrestled with this question a lot. [See discussion here.](https://openreview.net/forum?id=O36QcmUEDM&referrer=%5BAuthor%20Console%5D(%2Fgroup%3Fid%3DEMNLP%2F2023%2FConference%2FAuthors%23your-submissions))

And then you run into issues once you've collected it. How, precisely does one annotate this? Is it truly representative of the language, do people really sign that way in casual conversations, or only when placed in front of a camera in a studio setting? And so on. There's a lot to think about.

**Implications:**

* You might not have a lot of data to work with.
* One cannot just use whatever one finds, without potentially causing more harm than good.
* Potentially you might need to get creative with things like transfer learning, hybrid systems, weak supervision and the like.
* Building high quality benchmarks to actually measure and compare methods is likely to have massive impact downstream.
* Same deal with building the tools to build the datasets. If you can lower the difficulty on _that_, the whole field benefits.
* Maybe just spending a week annotating some data is going to have more impact than fiddling with a fancy model.

### Sign Language Translation Research in 2024 is the Wild West... in 1861

In 1861, the Pony Express began operating. That same year, the first transcontinental telegraph began operating. A few years later the first transcontinental railroad connected East to West. The frontier rapidly become more and more accessible and organized from there.

What I mean by this is, sign language research is a frontier field, but developing. There's not much data, tools are new and untested, datasets aren't all standardized, and there's a lot of kinks to iron out. But that is changing! Tech is improving, code is being released and updated, tools are being improve, standards are being created, datasets are being released. We're still in the Wild West, but it's becoming more orderly and organized all the time.

All of this very much reminds me of the low-resource machine translation field just a few years back, in 2020 or some. Back then, many of the same issues existed there, especially for lower resource languages, African languages, etc.

Like with sign languages, many of these languages lacked (and frankly, usually _still_ lack) the sort of [digital support](https://www.semanticscholar.org/paper/Assessing-Digital-Language-Support-on-a-Global-Simons-Thomas/02f0b82987e5a27f7e0101d3bb9353b70f266591) present for the more, how shall we say, well-funded languages. There were [Systematic inequalities](https://arxiv.org/abs/2110.06733), [poor quality datasets](https://arxiv.org/abs/2103.12028), issues with [dataset-building tools](https://arxiv.org/abs/2010.14571), etc.

Or, like with sign languages, you had many people who just... didn't really understand the issues involved. English was just automatically assumed to be the default. It was blithely assumed that what worked for English, or German, or other European languages would work fine for others. There were papers or techniques that claimed to have "solved" problems that were very much not yet solved. Which was very, very frustrating for members of communities that ended up badly marginalized, sometimes worse off forever. Why hire a translator for African languages like Fon, or Igbo, or Twi to help people at a hospital? Somebody made an auto-translator online, let's just use that! But no one asked the Africans. If they'd done so, they might have learned the model was not capable of doing that job!

And, well, over time people got together and worked together, and built things, and the situation improved. Slowly, and often frustratingly. [AfricaNLP](https://sites.google.com/view/africanlp2023/home), and [No Language Left Behind](https://arxiv.org/abs/2207.04672) and the [Bender Rule](https://thegradient.pub/the-benderrule-on-naming-the-languages-we-study-and-why-it-matters/)

**Implications:**

* Expect bugs. File bug reports and pull requests if you can!
* Different datasets might not be compatible or comparable. Maybe you can help?
* Things are changing rapidly. Be flexible, and keep learning.

### Sign Languages are real languages used by real people and real communities

> If I speak in the tongues of men and of angels, but have not love, I am a noisy gong or a clanging cymbal.
[_1 Corinthians 13:1, ESV_](https://www.biblegateway.com/passage/?search=1%20Corinthians%2013&version=ESV)

In all of this discussion, I think it's really important to never lose sight of the fact that the whole point of any of this, of _all_ of this, is to _make the world better_. To _help_ people. What, precisely, is the point of building a fancy technology, if it does not do that?

The history of Deaf communities has a long and often painful and sensitive history. And it's pretty common for people to just come in, and without actually communicating with any Deaf communities, decide arbitrarily that, for example, they should wear some sort of special glove and that'll fix it! But it's not that easy, [turns out that doesn't really help anyone.](https://www.theatlantic.com/technology/archive/2017/11/why-sign-language-gloves-dont-help-deaf-people/545441/). Or see [this blog post](https://audio-accessibility.com/news/2016/05/signing-gloves-hype-needs-stop/).

Certainly I've seen many, many papers that will make bold claims that they've completely solved everything, Sign Language Translation Is Taken Care Of. And then it turns out that the actual technology just recognizes individual letters, from single images. Which is... not really how people usually sign? It's kind of like asking people to speak English one letter at a time... "H" "e" "l" "l" "o".

As for me, well. I'm not a member of any Deaf community myself, and don't know anyone who is, a matter I am trying to remedy. So it would be very easy for me to fall into mistakes along those lines, and that's something I've tried to be aware of. I quite frankly don't know all the answers. All I can do personally do is try to [do justice, love kindness, and walk as humbly as I can](https://www.esv.org/Micah+6/).

**Takeaways:**

* Don't come into all this and think you're going to be the Great Savior Who Will Solve Everything. Be humble, ask questions. On that note, if I've messed anything in these notes up, [let me know](https://forms.gle/E3U6zibEzJVRMfr86)!
* Don't overclaim. If you say that your neat tech has fixed everything, and then the hospital uses your tool instead of an actual interpreter... that would be pretty bad.
* On a hopeful note, meaningful and useful technology might be more doable than you may think! You don't need to solve every problem for every language perfectly, in order to build something that people can actually get some decent use out of. Especially if you actually work with them and understand their needs.
* Go read up a bit first!
  * [Systemic Biases in Sign Language AI Research: A Deaf-Led Call to Reevaluate Research Agendas](https://arxiv.org/abs/2403.02563), by actual Deaf researchers. One of their calls to action is to simply be transparent about one's own position and situation.
  * The [SignOn White Paper "Sign Language Technology: Dos and Don'ts"](https://signon-project.eu/wp-content/uploads/2023/12/White-Paper-Sign-Language-Technology-FINAL.pdf) has a number of recommendations, for example the ought-to-be-obvious recommendation to work on the problems and use-cases that people in the Deaf communities actually want solved. Like maybe they might want to use your tech for low-stakes daily things instead of high-stakes medical things?
* Finally, don't take my word for any of this. Go join one of the online SL research communities, talk to people! They seem really nice, actually!
  * The CREST network, for example. [signup here](https://groups.io/g/crest)
  * The Sign Language Research slack channel is an excellent place to ask questions! [Invite link here, open to anyone interested in SL research](https://join.slack.com/t/signlanguager-kgn3800/shared_invite/zt-2fywilz3k-h_nctcijICnqoAPAfnNd~A).

## What Codebases are out there?

Like most coders/engineers, I ~~am a bit lazy~~ like to be economical with time and effort. I personally like having some code to work with, instead of having to create things totally from scratch. Of course, Sign Language Processing is a developing field. Much of what exists out there is research code. So getting it running isn't always easy. I went ahead and took a crack at some of them, results below. Mainly what I was looking for was:

1. How recently maintained/updated is this? If it's been a long time, it will be harder to get running.
2. The ImportError test: How hard is it to just install the requirements and run? Can I boot up a Colab Instance and get the scripts running without ImportErrors?

### Useful tools/libraries

#### [Pose-Format](https://github.com/sign-language-processing/pose)

* A whole pip-installable SLP library for dealing with poses, making it easier to deal with.

#### [MediaPipe Holistic Hand Landmarker](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)

Many papers use/cite this one. But it seems it's now a "legacy" item, no longer supported.

#### Video Feature libraries/code

#### [`video_features`](https://v-iashin.github.io/video_features/)

A number of SLP approaches use visual features such as I3D or S3D. This repo purports to let you extract features such as:

* I3D
* [S3D](https://arxiv.org/abs/1712.04851) (the original/official implementation of which is [here](https://github.com/kylemin/S3D))
* Optical Flow

##### i3dFeatureExtraction

`i3dFeatureExtraction` [here](https://pypi.org/project/i3dFeatureExtraction/#history), a pip library that "helps extract i3D features with ResNet-50 backbone given a folder of videos".

### SLT Resources that seem replicable/usable

#### [Sign Language Datasets](https://github.com/sign-language-processing/datasets)

* This is a very helpful repo/library. Hard to get anything done without data! And this library gives you many datasets to choose from, and a standardized code interface to do it with!
* Actively maintained and updated, often responses/updates come within the same day!
* And they have a nice Colab Notebook linked from the README, always nice to see. Which runs quite nicely.

#### [Two-Stream Network for Sign Language Recognition and Translation, NeurIPS2022](https://github.com/FangyunWei/SLRT/tree/main/TwoStreamNetwork)

* One of the top papers on PapersWithCode for sign language translation.
* [My attempt to get this running](https://colab.research.google.com/drive/1Z1P__o3NTl0zAStigUSlbeqxOVAaqOKT) Successfully installed requirements in Colab. This one looks pretty cool.

#### [KnowComp submission for WMT SLT](https://aclanthology.org/2023.wmt-1.36/)

* [My attempt to get this running in Colab](https://colab.research.google.com/drive/1le0qsaMwCH0hE6IkuVK1ImRKjsBBMHy2?usp=sharing)
I was able to get the requirements installed! Just don't have the needed data yet.

#### [WMT SLT 2023 Sockeye Baseline](https://github.com/bricksdont/sign-sockeye-baselines)

* [My attempt to get it running in Colab](https://colab.research.google.com/drive/1P2KWBOktCakuAfJPxFoBlpk1KXOGS6JP?usp=sharing)
Ran into some trouble, haven't yet found a way to get them all installed.

#### [WMT-SLT23 I3D baseline, aka "Sign Language Translation from Instructional Videos"](https://ieeexplore.ieee.org/document/10208355)

* [repo](https://github.com/imatge-upc/slt_how2sign_wicv2023)
* [My attempt to install the requirements for the I3D baseline in Colab](https://colab.research.google.com/drive/1GKHID0WMbXwnWXHD1iXQfs95nhWz9NQt?usp=sharing)
... got them installed! Haven't yet managed to run anything though, need data for that.

#### [Conditional Variational Autoencoder for Sign Language Translation with Cross-Modal Alignment](https://openaccess.thecvf.com/content/CVPR2022/html/Chen_A_Simple_Multi-Modality_Transfer_Learning_Baseline_for_Sign_Language_Translation_CVPR_2022_paper.html)

* [repo](https://github.com/rzhao-zhsq/CV-SLT), apparently based on [SLRT](https://github.com/FangyunWei/SLRT/blob/main/TwoStreamNetwork/docs/SingleStream-SLT.md)
* [Colab attempt at installing reqs](https://colab.research.google.com/drive/1G314SDvCDpyY2shMVouOusQ9fplP9t1z?usp=sharing), got the reqs installed successfully.

The tl;dr on this paper: Make the model guess from visual info only, then go again with text. Use statistics - first time through was the "prior", second time through is the "posterior", and it learns to correct itself using, like, a distribution. And there's CVAEs ([explained here](https://theaiacademy.blogspot.com/2020/05/understanding-conditional-variational.html), given Y please generate X etc) for the visuals. 

The code looks pretty nice! Conda env file, data prep, not too old, paths in .yaml files, checkpoints provided (including the ones for replication), commands to train and evaluate, very nice.

Apparently it requires "pre-extracted features", apparently from S3D, [see here](https://github.com/FangyunWei/SLRT/blob/main/TwoStreamNetwork/docs/SingleStream-SLT.md#multi-modal-joint-training) for how to do that.



### Needs more effort to get running

#### [SLTUnet](https://github.com/bzhangGo/sltunet)

* Recent, cool-looking paper.
* [My attempt to get SLTUNet running in Colab](https://colab.research.google.com/drive/1OxBJkQI4eccChdEjeNyjHMPHyCghQaMd?usp=sharing)
* Alas, I wasn't able to install requirements in Colab. Requires an older Tensorflow version that would be difficult to install.

#### [Tackling Low-Resourced Sign Language Translation: UPC at WMT-SLT 22](https://github.com/mt-upc/fairseq/tree/wmt-slt22). One of the submissions to the WMT SLT '22 contest

* See also [the paper](https://arxiv.org/abs/2212.01140).
* [My attempt to install requirements](https://colab.research.google.com/drive/1wPdHzSTlgvJzTm0Tg_Z28FnYEH459-tr?usp=sharing) worked, but I got stuck on how to run things with "Task"

### Not yet fully assessed

Codebases/repos I have not yet assessed for viability.

#### [Isolated Sign Recognition from RGB Video using Pose Flow and Self-Attention](https://ieeexplore.ieee.org/document/9523108)

* [repo](https://github.com/m-decoster/ChaLearn-2021-LAP)

#### [Temporal Lift Pooling for Continuous Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-3-031-19833-5_30)

* [repo](https://github.com/hulianyuyy/Temporal-Lift-Pooling)

#### [Spatial-Temporal Graph Convolutional Networks for Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-3-030-30493-5_59)

* [repo](https://github.com/yysijie/st-gcn)

Other links from the paper:

* <http://csr.bu.edu/asl/asllvd/annotate/index.html>
* <http://www.cin.ufpe.br/~cca5/asllvd-skeleton>.
* <http://www.cin.ufpe.br/~cca5/st-gcn-sl>.
* <http://www.cin.ufpe.br/~cca5/asllvd-skeleton-20>.

#### [Read and Attend: Temporal Localisation in Sign Language Videos](https://arxiv.org/abs/2103.16481)

* [project website with weights](https://www.robots.ox.ac.uk/~vgg/research/bslattend/)
* [code from ECCV 20](https://github.com/gulvarol/bsl1k)
* The code/models from this one are used in other papers, including [Microsoft's submission to WMT-SLT 22](https://www.microsoft.com/en-us/research/publication/clean-text-and-full-body-transformer-microsofts-submission-to-the-wmt22-shared-task-on-sign-language-translation/)

#### [Better Sign Language Translation with STMC-Transformer](https://aclanthology.org/2020.coling-main.525/)

* [My attempt in Colab](https://colab.research.google.com/drive/1GG9GBpBIid2aiEj52cP0A1bnPRJM96HD?usp=sharing)
* Initial "can we install the requirements" is pretty encouraging. Installs ran without error.
* Docs seem well done and readable.
* Trying to run train.py, we get an ImportError, which I have not had a chance to debug.

#### [Sign Language Transformers (CVPR'20)](https://www.cihancamgoz.com/pub/camgoz2020cvpr.pdf)

* [code is here](https://github.com/neccam/slt)
* [My attempt to install requirements](https://colab.research.google.com/drive/17r9S9OOMapeKXwBTYkGpGbAqBrmgncDH?usp=sharing)
* Very brief attempt to get running on Colab runs into highly specific version-pinning error. Will probably need [some tricks](#tricks-for-getting-old-code-working).  Also it's from 4 years ago.

### Seems out of date

Resources that might be useful but seem to be out of date, at least enough that they'd be quite hard to use as a starting point.

#### [OpenHands (possibly a dead project?)](https://openhands.ai4bharat.org/en/latest/instructions/datasets.html)

Here's what they say...
> 👐OpenHands is an open source toolkit to democratize sign language research by making pose-based Sign Language Recognition (SLR) more accessible to everyone.

* [Here's their paper](https://arxiv.org/abs/2110.05877)
* it looks neat and ambitious ...But I see the documentation has not been updated in years. Likely dead.

### Tricks for getting old code working

Some useful tricks I've learned

* **Don't use Python 3.12!** If you try pip installing and it won't work, often it will work if you go back to older, more well-tested versions. Often libraries just cannot be installed on 3.12. Not an issue on Colab because it uses 3.10 by default, but if you are on a workstation and using conda, it'll try to install 3.12 by default at time of writing.
  * Check the [Status of Python Versions](https://devguide.python.org/versions/). When they wrote their Readme, what was the default?
* pipreqs library can analyze code and generate a requirements.txt file
* deptry library can analyze a repo and requirements.txt for problems like unused imports, imports that aren't listed in the requirements, etc.
* pur will try to auto-update requirements.txt files to newer versions.
* condacolab lets you use conda commands in Google Colab, though not create new envs. This is really nice if a project uses environment.yml instead of requirements.txt


## Good places to look for more

If you're interested to get smart about SLT methods, here are some sources for more info.

### research.sign.mt - best place for SLT background

<https://research.sign.mt/> This is definitely your best starting point for getting smart on SLT. It is a lovely open-source compendium of Sign Language Processing papers, data, and other resources.

It's readable, and kept generally up to date. Check it out.

### sign.mt and sign-language-processing github orgs

* <https://github.com/sign>
* <https://github.com/sign-language-processing>
These are very rich sources of relevant repos and links.

### 👋 Sign Translate Wiki

Still a work in progress, but links to quite a number of interesting repos, including a whole pipeline's worth that could use contributions. Nice jumping-on points for an aspiring researcher!

* [Signed to Spoken Languages](https://github.com/sign/translate/wiki/Signed-to-Spoken) Here is the pipeline, which links to all the sub-repos that need work.
* If you're looking for video-to-pose it seems to be under [the Pose-Format library now](https://github.com/sign-language-processing/pose)

### WMT Shared Tasks

The Workshop on Machine Translation had two Shared Tasks on Sign Language Translation. The "findings" papers from each of them is quite informative.

#### WMT-SLT22

2022 contest. A number of submissions and approaches detailed.

* Code: The 2022 Shared Task has a number of "tools" links at [this page](https://sites.google.com/view/wmt-slt-v2022/tools).

[Findings](https://aclanthology.org/2022.wmt-1.71/)

#### WMT-SLT23

Code: The 2023 Shared Task has a ["Tools" page linking to several repos for baseline systems](https://www.wmt-slt.com/tools)

[Findings](https://aclanthology.org/2023.wmt-1.4/)

### PapersWithCode

<https://paperswithcode.com/task/sign-language-translation> lists a slew of papers, but it's incomplete. Notably it seems to be missing anything on the DGS Corpus?

## Theories and Architectures

### What's the general trend for sign language translation?

Sign Languages are inherently 4-dimensional. You've got the 3D position of your hands, face, etc., but also movement trajectories of those body parts.

But then on top of that you've got the linguistic knowledge you need to capture. You need to somehow guide your model to encode all that 4D information into some sort of representation/embedding that describes the _meaning_ of the signs and sentences.

Everyone has thought about this a lot, and borrowed from related fields. For example, taking in some continuous signal and mapping it to words and tokens is something the speech recognition folks do, let's see what they do! Or: take an action recognition model, or pose recognition model, and apply it.

After reading many papers, I've noticed a few themes:

* Many papers mention CNN+LSTM, where you extract visual information from each time-step/frame with a CNN, and then run an LSTM over the time dimension.
* Many do pose estimation first, then plug the sequence of poses into an LSTM or transformer.
* Many mention some sort of 3D Transformer over video frames. That is, 2D image plus time.
* If they mention CNN only, or the word "accuracy", they are probably doing static classification from single frames.

Essentially you've generally got some sort of "frame->feature" step, and then some sort of temporal architecture. CNN or vision transformer extract features from the 2D frames, or you do pose estimation.

Then to get the time aspect you have things like I3D, Optical Flow, or just have a video transformer that operates 3D. 

### Pose: everyone uses one of two options, but mostly MediaPipe

Many papers I've seen use one of these two:

* [OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose)
* [MediaPipe Holistic](https://github.com/google/mediapipe/blob/master/docs/solutions/holistic.md)

But more and more I see MediaPipe. Mostly, I think, because:

* Can give you 3D poses, not just 2D.
* It just kind of seems easier to use?

But as of April, it seems that the MediaPipe Holistic option is being... "upgraded"? And is now a ["legacy" solution](https://developers.google.com/mediapipe/solutions/guide#legacy). So watch that space I suppose.

Another one worth mentioning might be [AlphaPose](https://github.com/MVIG-SJTU/AlphaPose). [LSA-T dataset](https://link.springer.com/chapter/10.1007/978-3-031-22419-5_25#Fn2) uses it for example, talking about how it lets them capture full-body keypoints for multiple signers in a scene, which then also allowed them to identify which person was actively signing.

### Interesting/Novel, I want to learn more

Here I collect things that seem worth further investigation from a theory and architecture perspective, and why I think they're interesting

* [Towards Privacy-Aware Sign Language Translation at Scale](http://arxiv.org/abs/2402.09611)
  * Interest: high. They’ve got a semi-supervised method that works on large datasets, and a discussion of their architecture choices.
  * Architecture:
    * Hiera and MAE for the visuals.
    * Projection of the visual information into T5 for language.
  * Code: "will be released" as of 4/5/2024.
* [KnowComp Submission for WMT23 Sign Language Translation Task](https://aclanthology.org/2023.wmt-1.36)
  * Interest: definitely. They’re doing SLT and talk about how, and they submitted to WMT, and there's code!
  * Architecture: 
    * Pretrained I3D for visuals
    * Project that into German-GPT2
  * Code: [https://github.com/HKUST-KnowComp/SLT](https://github.com/HKUST-KnowComp/SLT)
* [Linguistically motivated sign language segmentation](https://aclanthology.org/2023.findings-emnlp.846)
  * Interest: They leverage knowledge of SL Linguistics to build a segmentation method that can operate on videos of languages not in the training, pretty successfully. Segmentation of video is useful for downstream tasks like translation, and there’s important information about SL linguistics, and how to leverage that intelligently.
  * Architecture:
    * Visuals go into MediaPipe pose estimator, optical flow
    * They do various preprocessing, pose normalization, etc.
    * LSTM encoder for the task, plus a classifier on top. And they cite the original 1997 LSTM paper, nice.
  * Code: [https://github.com/sign-language-processing/segmentation](https://github.com/sign-language-processing/segmentation)
* [SignNet II: A Transformer-Based Two-Way Sign Language Translation Model](https://ieeexplore.ieee.org/abstract/document/9999492)
  * Interest: High. Recent, SLT on German Sign Language, “Cross-Feature Fusion Based Transformer Model”, Dual Learning, Pose2Text, Text2Pose, Attention Visualization does both sign2text and tex2sign, goes into detail with pictures of architectures, does list/comparison with other archs. They evaluate on RWTH-PHOENIX-Weather2014T only, perhaps might be interesting to try on others?
  * "Neural Machines for Continuous Sign Language Translation" section has a nice roundup of the SOTA, worth a read.
  * Architecture/Theory:
    * "Cross-Feature Fusion Based Transformer Model".
    * Basically they go from video to three different output features at once: CNN, Optical Flow, and OpenPose. Those are your embeddings for the vision part.
    * That then goes into a Transformer for the time domain.
    * Then they analyze THAT and conclude that the keypoint/pose is the best.
    * Finally, they do a "dual-learning" setup, where they train on both pose-to-text and text-to-pose, I believe the idea being that this helps regularize the learning. Reminds me of back-translation.
  * Code: no.

## Papers that do CNN+LSTM of some kind

Quite a few papers do something of this nature. Examples include:

[A Deep Learning Solution for Arabic Words Sign Language Recognition in the Context of Sentences](https://ieeexplore.ieee.org/document/10361379)

* preprocess video to get "Sum of Displaced Differences Images"
* uses CNN on those.
* Put that into a BiLSTM.

## Survey of Sign Language Processing Surveys

Let's have a look at some survey papers, shall we?

### SLP in general

#### [Sign Language Recognition, Generation, and Translation An Interdisciplinary Perspective (2019)](https://www.semanticscholar.org/paper/Sign-Language-Recognition%2C-Generation%2C-and-An-Bragg-Koller/c7fd5973a2cb3f7c6a5df0120400bc47b2f2bacf)

Quite an influential one, based on citation count maybe a "seminal work".

#### [Systemic Biases in Sign Language AI Research A Deaf-Led Call to Reevaluate Research Agendas (2024)](https://arxiv.org/abs/2403.02563)

Recent survey, looks at like 100+, including modeling decisions, and all of the writers are themselves Deaf. Interesting. I've seen it described by a quite respected SL researcher as a "must-read", and I think they're right.

Things I learned:

* Basically the communication barriers may not be what one might think, lots of oversimplification/overstating going on
  * "deaf people use sign language" is oversimplified, it's way more complicated. Many times it's deliberately not given as an option/suppressed, and people often use a mix, and other complexities.
  * Lots of people know both a sign language and a spoken language. Basically it's not homogenous.
  * Maybe people don't need the technosolutions, they've got their own strategies already, don't overinflate the importance of the tech.
* Dataset misalignment with, like, real life
  * Interpreters only for example. They might not sign the same way.
  * Lots of details on who and how are glossed over in dataset creation
  * IRL, the "born deaf, only communicate with other deaf people" situation is super rare, often you get "born to hearing families, signing specifically discouraged, learned it way later".
  * IRL there's so much variation, but datasets for ML simplify. Clean Data/annotations in conflict with IRL conditions
* labels lacking linguistic foundation
  * glosses without thinking about it
  * Present (gift) or Present (time) given the same gloss in one paper, different in another paper. Inconsistencies.
  * Glosses are not a translation! Some people treat it that way.
  * What glossing system was even used? Not always explained.
  * Subtitles often have quality issues, and that's not addressed.
  * It's NLP! Not just Computer Vision! So techniques that act like it's purely CV run into issues there.
* Modeling decisions
  * various issues with pose estimation
  * various issues with pretraining on non-sign datasets SLP Open Problems and Questions
  * "Natural language-assisted sign language recognition" for example has issues which cascade from previous decisions, e.g. distinct glosses for the same sign

### Specifically SLT

#### [Machine Translation from Signed to Spoken Languages State of the Art and Challenges (2022)](https://arxiv.org/abs/2202.03086)

[Findings of the Second WMT Shared Task on Sign Language Translation (WMT-SLT23)](https://aclanthology.org/2023.wmt-1.4/) calls this a "comprehensive review" on the topic, so therefore it seems worth checking out.

This one influenced me quite a bit. For example to leave out papers with glove-based methods as impractical.

#### [A survey on Sign Language machine translation (2023)](https://www.sciencedirect.com/science/article/pii/S0957417422020115)

Has a nice section on CSLR with a description of architectures. Those papers are worth looking at me thinks. Interesting words: 3D-ResNet, iterative training (related to weak labelling/supervision), Temporal Convolution Pyramid (TCP).

Has a nice section talking about different SLT metrics. Mostly the same ones as NMT, e.g. BLEU. SLR has a few, e.g. sign error rate, gloss error rate. No mention of measuring sign language generation quality.

TODO: have another look-see at this one, see if there's any methods that look interesting to investigate a bit more. 

#### [Sign Language Translation A Survey of Approaches and Techniques (2023)](https://www.mdpi.com/2079-9292/12/12/2678)

This one could use some copyediting. I did appreciate their dive into visual feature extractors.

#### [Sign Language Processing (research.sign.mt)](https://research.sign.mt/)

Quite current, [open source.](https://github.com/sign-language-processing/sign-language-processing.github.io) Mentioned above under research.sign.mt

### Other SLP tasks

#### [Unraveling a Decade A Comprehensive Survey on Isolated Sign Language Recognition (2023)](https://ieeexplore.ieee.org/document/10350553)

This survey contains quite a bit about video methods actually, I misinterpreted the name at first. Talks about input modalities, fusion methods, datasets, etc.

Interestingly it also does link a number of datasets with videos in them which might be relevant. This would be good to chase down.

* J. Wan, Y. Zhao, S. Zhou, I. Guyon, S. Escalera and S. Z. Li, "ChaLearn looking at people RGB-D isolated and continuous datasets for gesture recognition", CVPRW, pp. 56-64, 2016.
* Sergio Escalera, Xavier Baró, Jordi Gonzalez, Miguel A Bautista, Meysam Madadi, Miguel Reyes, et al., "Chalearn looking at people challenge 2014: Dataset and results", ECCVW, pp. 459-473, 2015.
* Hamid Reza Vaezi Joze and Oscar Koller, "MS-ASL: A large-scale data set and benchmark for understanding American sign language", 2018.
* O. M. Sincan and H. Y. Keles, "AUTSL: A large scale multi-modal Turkish sign language dataset and baseline methods", IEEE Access, vol. 8, pp. 181340-181355, 2020.
* Laura Docío-Fernández, José Luis Alba-Castro, Soledad Torres-Guijarro, Eduardo Rodríguez-Banga, Manuel Rey-Area, Ania Pérez-Pérez, et al., "LSE UVIGO: A multi-source database for Spanish sign language recognition", Proceedings of the LREC2020 9th Workshop on the Representation and Processing of Sign Languages: Sign Language Resources in the Service of the Language Community Technological Challenges and Application Perspectives, pp. 45-52, 2020.
* Oğulcan Özdemir, Ahmet Alp Kındıroğlu, Necati Cihan Camgöz and Lale Akarun, "BosphorusSign22k sign language recognition dataset", 2020.
* Gokul NC, Manideep Ladi, Sumit Negi, Prem Selvaraj, Pratyush Kumar and Mitesh Khapra, "Addressing resource scarcity across sign languages with multilingual pretraining and unified-vocabulary datasets", NIPS, vol. 35, pp. 36202-36215, 2022.

It seems there's actually a good amount of quality links in here at the very least. A good amount of this ISLR is video-based, and therefore is potentially useful for video-based sign2text, sign2gloss.

#### [A Extensive Survey on Sign Language Recognition Methods (2023)](https://ieeexplore.ieee.org/document/10084111)

Cites 51 references, but doesn't provide a lot of useful/practical info. Of the 51 many are quite dated, and only some are from the last few years. Also a number are based on something like a sensor or wearable device. Nevertheless I did find 10 whose titles seemed interesting in some way for video understanding of Sign Languages.

1. [SF-Net: Structured Feature Network for Continuous Sign Language Recognition](https://arxiv.org/abs/1908.01341) (2019) Continuous SLR and the abstract looks interesting. Skimming through I see a number of details about feature extraction methods. I don't think they release their code.
2. [Spatial-Temporal Graph Convolutional Networks for Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-3-030-30493-5_59) (2019) They provide source code and data for continuous SLR, and mention an interesting method as well.
3. [Multiple Proposals for Continuous Arabic Sign Language Recognition](https://link.springer.com/article/10.1007/s11220-019-0225-3) (2019) they do in fact process successive frames of video. They use pixel differences. No code I think?
4. [Sign Language Recognition from Digital Videos Using Deep Learning Methods](https://link.springer.com/chapter/10.1007/978-3-030-72073-5_9) (2021) Video, and capsule networks is at least interesting and diferent.
5. [An Integrated CNN-LSTM Model for Bangla Lexical Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-981-33-4673-4_57) (2020) CNN-LSTM implies video. It seems quite basic but I guess it's in scope.
6. [Temporal Lift Pooling for Continuous Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-3-031-19833-5_30) (2022) code and continuous SLR, so it's relevant. [repo](https://github.com/hulianyuyy/Temporal-Lift-Pooling)
7. [One Model is Not Enough: Ensembles for Isolated Sign Language Recognition](https://www.mdpi.com/1424-8220/22/13/5043) (2022) Based on "We analyze two appearance-based approaches, I3D and TimeSformer, and one pose-based approach, SPOTER." it seems they're processing videos, so it's relevant!
8. [Two-Stream Mixed Convolutional Neural Network for American Sign Language Recognition](https://www.mdpi.com/1424-8220/22/16/5959) from 2022. It barely squeaks into scope by the fact they use two consecutive frames, which is interesting enough to include.
9. [Improved 3D-ResNet sign language recognition algorithm with enhanced hand features](https://www.nature.com/articles/s41598-022-21636-z) Processes video inputs, and it's in _Nature_. Skimming through I also see a number of diagrams and graphs that look interesting.
10. [Hand Landmark-Based Sign Language Recognition Using Deep Learning](https://link.springer.com/chapter/10.1007/978-981-16-7996-4_11#Sec5) Alas, it's based on static images, so not within my scope of interest.

Some odd stylistic/writing choices. "it must be suitable to fete and record homemade and non-manual movements". "Result Obtainment Using Continuous Sign Language Representation Data Sets"

They claim that "Review publications on hand gestures and SLR are numerous." but that in contrast their paper is very in-depth. Yet I feel it was pretty surface-level? Didn't seem to cover that many, and not in very much detail.

Table 1 has a list of: Approach, year, language. But then "accuracy"? On which datasets? How do you compare directly like that? Seems strange, not sure I understand.

Some of the terminology, e.g. "hand gestures" makes me think the paper might benefit from running it by some Deaf researchers.

In general this review is... not my favorite. I kind of dislike it when folks feel the need to use lots of big words to tell us how their paper is "extensive" and great and amazing. I didn't learn a lot of useful or state-of-the-art info from it, alas. But I got some interesting references, so that's nice.

#### [Sign Language Production: A Review (2021)](https://openaccess.thecvf.com/content/CVPR2021W/ChaLearn/html/Rastgoo_Sign_Language_Production_A_Review_CVPRW_2021_paper.html)

Potentially interesting, I'm just not focusing on text2sign at the moment.

#### [Sign Language Recognition: Current State of Knowledge and Future Directions (2023)](https://ieeexplore.ieee.org/document/10307879)

* Behind an IEEE paywall, but accessible through academic institutions.
* tl;dr CNNs are now the SOTA for static/image-based, as expected.

### Miscellaneous


#### Metrics

Often people use standard MT Metrics, e.g. BLEU. 

[Helpful guide to those.](http://www2.statmt.org/book/slides/08-evaluation.pdf) which explains a bit about BLEU-1, BLEU-4, brevity, etc. Of course it can be difficult, so in NMT people often use [SacreBLEU](https://github.com/mjpost/sacrebleu)

#### [Comparative Analysis of Multiple ML Models and Real-Time Translation (2023)](https://ieeexplore.ieee.org/document/10346894)

Not my favorite. I'm a bit unsure about the terminology choices, and I struggle to understand the writing style. And in the end the thing they actually do is not actually a survey paper but a few entry-level experiments on “alphabets”.

#### [Indian Sign Language Recognition Using Classical And Machine Learning Techniques A Review (2023)](https://ieeexplore.ieee.org/document/10136059)

Goes over various approaches to Indian Sign Language. We care about the vision-based ones. But then they're explaining what... Anaconda is...? And Precision/Recall are? This is very basic stuff. And they describe a system of their own but it seems to be just standard off-the-shelf items? 

#### [Quantitative Survey of the State of the Art in Sign Language Recognition (2020)](https://arxiv.org/abs/2008.09918)

300 papers, 400 experimental results... very extensive work, and by one person! Also they basically collect everything on RWTH-PHOENIX-Weather at the time. They also have the line "Surprisingly, RWTH-PHOENIX-Weather with a vocabulary of 1080 signs represents the only resource for large vocabulary continuous sign language recognition benchmarking world wide." I wonder if that's still true 4 years later? (I don't think so, there's a lot more listed on research.sign.met)

## Ideas and Potential Starting Points

### Ctrl-F for Signs?

One task which would be very helpful would be the ability to perform a sign and then scrub through videos in order to find similar signs. Useful for:

* Learning a sign language. Someone does a sign and you want to find it in a database or a video.
* For people creating (or watching) SL videos, this would make it much easier to do things find/replace operations, or even know which videos contain it and where. Based on conversations with people involved with ASLV, this would make their lives easier.

### Compress and Simplify?

* If the data is highly dimensional, can we compress it for storage and processing?
  * Naive ideas like masking out the non-people-parts so the .mp4 can reduce file size.
    * But then the NN takes in the full grid of pixels for every single frame, even though huge swaths of the input are just not relevant.
    * Can we use an attention model to estimate the relevant parts of the data and lossfully compress the rest?
    * Compress, THEN learn, like in [Training LLMs over Neurally Compressed Text](https://arxiv.org/abs/2404.03626)
  * can we do a dimensionality reduction? Get it down closer to 40-50 bits/second.
  * What about an autoencoder? [Sign-VQ](https://github.com/sign-language-processing/sign-vq) repo looks at that.

## Changelog

* 2024-04-01: started the article. Fleshed out the main outline and over the course of that week.
* 2024-04-15: added "a selection of visual/linguistic challenges".
* 2024-04-18: cross link to new article
* 2024-04-19: add some ideas/starting points.
