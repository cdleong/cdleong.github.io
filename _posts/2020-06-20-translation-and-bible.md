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

# Bible Translation Software
Currently, there is almost no deployed usage of Machine Translation in the Bible Translation field. However, there has been increased interest in looking into this, and I've found several people who may be interested in collaboration. 

Current projects do make use of various computer software tools. In order to make a practical, usable solution we will need to understand what is actually in use and how it is being used.

## [Paratext ](https://paratext.org/)
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


# Deployable AI
How can we setup our system so that our users can actually make use of it? Techniques that minimize the need for expensive dedicated hardware, more efficient chips, smaller models, less GPU-intensive training, and so on. 

For example, the startup Neural Magic [has developed algorithms to run trained models efficiently on old hardware](https://www.technologyreview.com/2020/06/18/1003989/ai-deep-learning-startup-neural-magic-uses-cpu-not-gpu/). This can apparently run well on CPUs, not GPUs.

# Polyglots: Efficient Language Learners?
Stealing ideas from nature is a time-honored tradition. Engineers have for many centuries, looked at how the natural world solved a problem, then copied its homework shamelessly. 

We're trying to understand how to efficiently learn a language so that we can translate text into it, right? And who is most efficient at language learning? Polyglots who have demonstratively done it. 

People such as the Youtuber going by "Matt vs Japan" have been known to openly describe their methods for language learning, e.g. ["Consciousness and Language Acquisition"](https://youtu.be/2i8AzjxwhSU).

Or, for example, see ["Polyglots’ Brains – Neuroscience and Language Learning - Eryk Walczak"](https://youtu.be/MkWCjDmPaHQ).

# Resources

## Data
### [OPUS parallel Bible dataset](http://opus.nlpl.eu/)
One of the many [Open Parallel Corpus](http://opus.nlpl.eu/) datasets, contains the Bible, in parallel, over 100 languages, in various formats suitable for machine learning. What's more, their [github link](https://github.com/christos-c/bible-corpus) provides resources for processing and training with it, and has other updates as well. 

### [eBible.org](https://ebible.org/download.php)
This website contains download links to hundreds of Bible translations, for many, many countries. 

Just one example, [The New Testament in the North Tanna Language of Vanuatu](https://ebible.org/details.php?id=tnn) is offered in a variety of formats including plaintext. But they also have "formats for developers" which include SIL's [XeTeX typesetting system](http://scripts.sil.org/xetex), OSIS [an XML standard for "The Bible Tool"](http://www.crosswire.org/study/index.jsp?section=aboutthetool), and [Unified Scripture Format XML (USFX)](https://ebible.org/usfx/)

## Code

### [OpenNMT](https://opennmt.net/)
A very mature and well-developed codebase/ecosystem for neural machine translation. Probably the place to start.

### [MBART](https://github.com/pytorch/fairseq/tree/master/examples/mbart)
Working code from the [Facebook AI Research Sequence-to-Sequence (fairseq) Toolkit](https://github.com/pytorch/fairseq). Full sequence-to-sequence model, and therefore easy to use for pretraining models. 

### [LASER (Language-Agnostic SEntence Representations) toolkit](https://github.com/facebookresearch/LASER)
A "library to calculate and use multilingual sentence embeddings." I previously discussed this in [my post on the PHD]({{ site.baseurl }}/phd-journey/). But the code and many models are freely available for use. 

# People
Who is working in this, or related fields? Would they like to join forces?


## Bible Translation specifically
Who is working with the Bible, or Bible translation? 

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
