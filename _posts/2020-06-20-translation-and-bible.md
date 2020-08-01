---
layout: post
title: Translation and the Bible
permalink: /translation-and-bible/
---

[Previously, I discussed why I'm curious about machine learning and Bible translation.]({{ site.baseurl }}/phd-journey/) Here, I will be collecting resources I find interesting or relevant. I hope to grow and improve this page over time.

# The Mission
The mission is simple: Translate the Bible into every human language. 
Stretch goal: Using the Bible as a seed for translation algorithms, translate all human knowledge into every language.

# Problems to solve: 
The two biggest problems are:
* How can we translate into the language of a tribe with low or zero amounts of data?
* How can we field-deploy our solutions in a way that's actually practical to use?

# Questions to Answer

Currently, the main questions I'd like to answer are:
* [Bible Translation Software](#bible-translation-software): What's the current state of Bible Translation software, and how can it be improved most practically? 
* [Low-Resource MT](#low-resource-machine-translation): How can we get better quality translations with very little starting data? 
* [Deployable AI](#deployable-ai): Our likely users might be on an old laptop in a jungle. How can we make this tech usable for them?
* [Polyglots: Efficient Language Learners](#polyglots-efficient-language-learners): Some people can learn many languages in a short amount of time with little data. Can we use their techniques to improve ours?
* [Resources](#resources): What code, data, etc. are currently available that we can start playing with?
* [People](#people): Who else is working in this field? Would they like to join forces? 

## Subproblems that have showed up in the literature: 

### The Hubness Problem
To put it simply, sometimes in really high-dimensional space, everything is nearest neighbors to certain "hubs". Because of this "hubness problem", you cannot so easily just calculate a [word embedding](https://jalammar.github.io/illustrated-word2vec/) in one language and simply find the nearest neighbor in another language. 

For example, this comes up in [Bilingual Lexicon Induction through Unsupervised Machine Translation](https://arxiv.org/abs/1907.10761) by Artetxe et al, where they attempt to tackle it. 

# Bible Translation Software
Currently, there is almost no deployed usage of Machine Translation in the Bible Translation field. However, there has been increased interest in looking into this, and I've found several people who may be interested in collaboration. 

Current projects do make use of various computer software tools. In order to make a practical, usable solution we will need to understand what is actually in use and how it is being used.

## [Paratext](https://paratext.org/)
Apparently one of the primary tools used by Bible translators in our current era, Paratext [provides features](https://paratext.org/features/) for "development and checking of new Bible translation texts, or revisions to existing texts." Including wordlists, a tool to ensure consistent use of Biblical terms, and the ability to view multiple translations in parallel. 

# Low-Resource Machine Translation
Techniques that make more efficient use of the data we have.

## Snowballing
Once we have some existing data in a language, can we incrementally start pulling in more and more? Can we start with a small amount of manual translation by a human, then have the model learn from this and gradually get better? Ideally we'd "snowball" before long, with the model eventually picking up most of the slack. Even better, we could perhaps continue that snowball onwards beyond the Bible itself to include more and more knowledge. Perhaps Wikipedia next?

Here we are looking for techniques that allow a machine learning model to do "incremental learning", slowly improving over time as more data or translations are generated.

For one example of the potential of building on a Bible translation, see [The Lowlands Project](#the-lowlands-project).

## Transfer Learning
Often, you have data or models for other languages than the one you wish to translate to. Rather than starting our machine learning from scratch each time, can we preserve and transfer existing knowledge? For example, if we have data in related languages, our system might be able to start there and learn things that help with our target language. 

## Unsupervised Learning: Making Datasets Easier to Collect
Often, it's actually pretty easy to gather data, the hard part is labelling it into the form a computer machine learning system can learn from. Are there ways we can reduce the burden of creating training data? Are there ways we can get our algorithms to learn from the "raw" data?

For example, could we simply record people speaking, and get the machine learning system to start noticing patterns in the audio?

### [Unsupervised Multimodal Neural Machine Translation with Pseudo Visual Pivoting](https://arxiv.org/abs/2005.03119)
Very novel and clever method based on the fact that people speak different languages but are looking at a similar visual world. 

What's interesting here is that all you need is image captions to provide some weak supervision. Once you've got image captions in different languages, you can use those images to guide the embedding and alignment process. And it's much easier to get people to caption some pictures than to get them to translate text.


# Deployable AI
How can we setup our system so that our users can actually make use of it? Techniques that minimize the need for expensive dedicated hardware, more efficient chips, smaller models, less GPU-intensive training, and so on. 

For example, the startup Neural Magic [has developed algorithms to run trained models efficiently on old hardware](https://www.technologyreview.com/2020/06/18/1003989/ai-deep-learning-startup-neural-magic-uses-cpu-not-gpu/). This can apparently run well on CPUs, not GPUs.

* [XtremeDistil: Multi-stage Distillation for Massive Multilingual Models](https://www.microsoft.com/en-us/research/publication/xtremedistil/), provides another option, namely "distilling" a large and complex model like mBERT down into something lightweight, while still giving you most of the same performance. [github repo here](https://github.com/MSR-LIT/XtremeDistil)

# Polyglots: Efficient Language Learners?
Stealing ideas from nature is a time-honored tradition. Engineers have for many centuries, looked at how the natural world solved a problem, then copied its homework shamelessly. 

We're trying to understand how to efficiently learn a language so that we can translate text into it, right? And who is most efficient at language learning? Polyglots who have demonstratively done it. 

People such as the Youtuber going by "Matt vs Japan" have been known to openly describe their methods for language learning, e.g. ["Consciousness and Language Acquisition"](https://youtu.be/2i8AzjxwhSU).

Or, for example, see ["Polyglots’ Brains – Neuroscience and Language Learning - Eryk Walczak"](https://youtu.be/MkWCjDmPaHQ).


# Resources

## Other Resource Collections

### [CMU LTI Low Resource NLP Bootcamp 2020](https://github.com/neubig/lowresource-nlp-bootcamp-2020)
"a page for a low-resource natural language and speech processing bootcamp held by the Carnegie Mellon University Language Technologies Institute in May 2020."
* [spaCy tutorial](https://spacy.io/usage/spacy-101)
* tutorial using [epitran](https://github.com/dmort27/epitran): "A library and tool for transliterating orthographic text as IPA (International Phonetic Alphabet)."
* [Universal Dependencies (UD) Treebank](https://universaldependencies.org/): "a framework for consistent annotation of grammar (parts of speech, morphological features, and syntactic dependencies) across different human languages."
* [fast_align](https://github.com/clab/fast_align) tutorial. "a simple, fast, unsupervised word aligner."
* [JoeyNMT](https://joeynmt.readthedocs.io/en/latest/tutorial.html) tutorial with [Latvian-English data](http://www.statmt.org/wmt17/translation-task.html)
* [tutorial on creating an interlinear gloss using Leipzig Glossing Rules](https://www.eva.mpg.de/lingua/pdf/Glossing-Rules.pdf)
* [learning of word representations using fastText](https://fasttext.cc/docs/en/unsupervised-tutorial.html) tutorial, as well as "using them for simple text classification, and finding similar words".
* Multilingual BERT usage. 
* [Kaldi Speech Recognition Toolkit](https://github.com/kaldi-asr/kaldi) tutorial



## Data
### [OPUS parallel Bible dataset](http://opus.nlpl.eu/)
One of the many [Open Parallel Corpus](http://opus.nlpl.eu/) datasets, contains the Bible, in parallel, over 100 languages, in various formats suitable for machine learning. What's more, their [github link](https://github.com/christos-c/bible-corpus) provides resources for processing and training with it, and has other updates as well. 

### [eBible.org](https://ebible.org/download.php)
This website contains download links to hundreds of Bible translations, for many, many countries. 

Just one example, [The New Testament in the North Tanna Language of Vanuatu](https://ebible.org/details.php?id=tnn) is offered in a variety of formats including plaintext. But they also have "formats for developers" which include SIL's [XeTeX typesetting system](http://scripts.sil.org/xetex), OSIS [an XML standard for "The Bible Tool"](http://www.crosswire.org/study/index.jsp?section=aboutthetool), and [Unified Scripture Format XML (USFX)](https://ebible.org/usfx/)

## Code

### [OpenNMT](https://opennmt.net/)
A very mature and well-developed codebase/ecosystem for neural machine translation. Probably the place to start.

## [JoeyNMT](https://joeynmt.readthedocs.io/en/latest/index.html)
* "a minimalist neural machine translation toolkit for educational purposes."
* [nice starter notebook here](https://github.com/masakhane-io/masakhane-mt/blob/master/starter_notebook.ipynb), using opus data.
* [github](https://github.com/joeynmt/joeynmt)

### [MBART](https://github.com/pytorch/fairseq/tree/master/examples/mbart)
Working code from the [Facebook AI Research Sequence-to-Sequence (fairseq) Toolkit](https://github.com/pytorch/fairseq). Full sequence-to-sequence model, and therefore easy to use for pretraining models. 

### [LASER (Language-Agnostic SEntence Representations) toolkit](https://github.com/facebookresearch/LASER)
A "library to calculate and use multilingual sentence embeddings." I previously discussed this in [my post on the PHD]({{ site.baseurl }}/phd-journey/). But the code and many models are freely available for use. 

### [Kaldi](https://kaldi-asr.org/)
[Github link](https://github.com/kaldi-asr/kaldi)
"Kaldi is a toolkit for speech recognition written in C++ and licensed under the Apache License v2.0. Kaldi is intended for use by speech recognition researchers."

### the Artetxe libraries
Various libraries created by Mikel Artetxe, including...
* [Monoses](https://github.com/artetxem/monoses), "an open source implementation of our unsupervised statistical machine translation system"
* [VecMap](https://github.com/artetxem/vecmap), "an open source implementation of our framework to learn cross-lingual word embedding mappings"

# People
Who is working in this, or related fields? Would they like to join forces?


## Bible Translation specifically
Who is working with the Bible, or Bible translation? 

### [All the Word](https://alltheword.org/about/)
This is one of the organizations involved in developing modern [Bible Translation Software](#bible-translation-software). They do not use neural or statistical machine translation, instead preferring to develop a "a linguistically based rule system." 

Their [QA Section](https://alltheword.org/qa/) describes their reasons for doing this. 
* Higher quality translations. They measure this in terms of percentage increases in translator productivity. However, given the [enormous](https://mse238blog.stanford.edu/2017/08/jchoi8/machine-learning-transforms-google-translate-overnight/) [advances](https://kottke.org/16/12/ai-hemingways-the-snows-of-kilimanjaro) in perceived translation quality by systems such as Google Translate, I wonder whether there might still be room for improvement? After all, "Whether the leopard had what the demand at that altitude, there is no that nobody explained." was made by a rule-based system.
* No need for training corpora. This is a good point, as the need for training data is one of the biggest problems with NMT.
* Ease of adaptation to related languages. Tweaking rules for related dialects is apparently quite easy. In contrast, they say that "the statistics produced by a statistical system can't be modified for a related language; each new language requires its own training corpora.". I wonder whether that's still true with modern transfer learning techniques?
* "Rule based systems give linguists a very good starting point for a publishable grammar". This is an interesting point I had not considered. However, I wonder whether it wouldn't be possible to back that sort of information out of a good enough trained model somehow? (NOTE: potentially yes, according to ["Finding Universal Grammatical Relations in Multilingual BERT"](https://arxiv.org/abs/2005.04511))

There are many other interesting facts in this QA, providing a lot of food for thought on the practical realities of translation. 

For example, they have apparently manually created a heavily-annotated "semantic representation" of every word, phrase, and clause in the Bible. This seems like it would be a very interesting dataset to play with. 

Also, they have an interesting breakdown of the current process for translating to a new language. 
* 40-50 hours to learn the software itself. 
* 150 more hours to do a Grammar Introduction, for an experienced software user. In one example, it took 30 two-hour meetings.  
* 30-50 more hours to work through a non-biblical story about preventing eye infections. Now, you're ready to begin working on a short book of the Bible, say, Ruth. 
* 200 or more hours to do Ruth, counting in meetins and work outside of meetings. 

So this would be the thing to try and improve. For example, could we, after the Grammar Introduction phase, begin seeding a NMT system, and use it to start generating candidate translations? Or could we begin suggesting candidate grammar rules for the software to present to the user? 

Overally they seem quite experienced in the field. I've been in contact with some of them, and begun some interesting discussions!

### Sami Liedes
As of 2018, this person wrote ["Recent developments and ideas on the Bible Neural Machine Translation problem"](https://samiliedes.wordpress.com/2018/08/28/recent-developments-and-ideas-on-the-bible-neural-machine-translation-problem/), which contains some interesting thoughts on the problem setting, and potential ideas for tackling it. However, his blog has not been updated in the two years since then. 

### The Lowlands Project
Professor Anders Søgaard of the University of Copenhagen wrote ["Linguists use the Bible to develop language technology for small languages"](https://humanities.ku.dk/news/2015/linguists_use_the_bible_to_develop_language_technology_for_small_languages/) in 2015, describing their work on low-resource languages using the Bible as a starting point. 

One notable work of theirs was ["If all you have is a bit of the Bible: Learning POS taggers for truly low-resource languages"](https://www.aclweb.org/anthology/P15-2044), which provides a fine example of the potential of using a bit of translated data to [snowball](#snowballing) further.

## Low-Resource Machine Translation

## Daniel Whitenack
Creator of ["Wash your hands" in 500+ languages"](https://datadan.io/blog/wash-your-hands), a very inspiring usage of Bible data to translate helpful knowledge into many languages. 
He also created a very intriguing page entitle ["Resources for Low Resource Machine Translation"](https://datadan.io/blog/resources-for-low-resource-machine-translation). TODO: write more about him!

### [Sebastian Ruder](https://ruder.io/)
A very well-known researcher, especially for his many interesting and popular blog articles on Natural Language Processing. He maintains [NLP Progess](https://nlpprogress.com/) a continually-updated list of datasets/benchmarks and results. TODO: write much more about this man, and his most interesting papers and articles, such as ["NLP's ImageNet Moment has Arrived"](https://ruder.io/nlp-imagenet/).


### [Mikel Artetxe](http://www.mikelartetxe.com/)
One of the foremost publishers of relevant research to this task, he has written extensively on unsupervised and low-resource machine translation. 
* [Google Scholar page](https://scholar.google.com/citations?user=N5InzP8AAAAJ&hl=en)
* [Semantic Scholar page](https://www.semanticscholar.org/author/Mikel-Artetxe/2347956)

### [Masakhane project](https://www.masakhane.io/)
Machine translation for African Languages. They're done a lot of research in the realm of low-resource translation. 
* [masakhane-mt](https://github.com/masakhane-io/masakhane-mt) is their github repo for machine translation. 


# Experiments I'd like to try

## Jargon/Dialect translator with Simple Wikipedia?
* Take a word-aligner such as fast_align or Monoses, and translate from, say the KJV to the ESV. 

### Datasets 
* Wikipedia vs. Simple English Wikipedia?
* Bible in Basic English

### Code: 
* We could use [wikiextractor](https://github.com/attardi/wikiextractor) with the Cirrus Data dumps: https://dumps.wikimedia.org/other/cirrussearch/20200420/ simplewiki is an option.
* Then we calculate some word embeddings with fasttext? 


## The Holy Grail of Low-Resource Translation: Unsupervised Universal Translator.
While we're aiming high, can we get a machine to learn languages with minimal help?

Perhaps something like...
* Drop a microphone down.
* Collect lots of data. 
* Have it start noticing patterns in the data on its own, while also easily accepting input from the humans involved?


# Leads, TODOs, and unanswered questions
Collecting quick thoughts and questions for later review
* What is VecMap? https://www.aclweb.org/anthology/P18-1073/, and is it useful? Is there code? (edit: yes, https://github.com/artetxem/vecmap)
* Wikiextractor, eh? https://github.com/attardi/wikiextractor [used by Artetxe](https://arxiv.org/pdf/1907.10761.pdf). Can we use this to scrape, say, Simple English Wikipedia? 
* FastAlign https://www.aclweb.org/anthology/N13-1073/ [used by Artetxe for word alignment](https://arxiv.org/pdf/1907.10761.pdf). Worth looking into? Code: https://github.com/clab/fast_align
* Monoses, "an open source implementation of our unsupervised statistical machine translation system" by Artetxe et al. https://github.com/artetxem/monoses
* TODO: look into whether one could one back out a "publishable grammar" from a language model, e.g. using techniques based on [this paper](https://arxiv.org/abs/1905.05950). See also https://arxiv.org/abs/2005.04511
* AllTheWord.org has created Semantic Representations of every word, phrase and clause in the Bible. And they have existing translations from this. Hmmmmmm... will they let us play with those? 
* [epitran](https://github.com/dmort27/epitran) is a library for G2P, or grapheme to phoneme. Could be a building block in an unsupervised data collection pipeline system of some sort. 
* Generate my own artificial language? e.g. Pig Latin translator? Take a massive English corpus. Generate the Pig Latin version. Translate. 
* The videogame Heaven's Gate involves learning a language entirely from written text. 
* Can a I frame my audio problem as a computer vision problem? 
* Can I frame my language model learning as a reinforcement learning problem? "ask smart questions to give me a correct grammar". 
* TODO: https://github.com/masakhane-io/masakhane-mt/blob/master/MT4LRL.md has a lot of interesting items, broken down into different scenarios based on what data is available. 
* TODO: look more into Apertium, which is a good way to start a RBMT project up. https://apertium.org/index.eng.html