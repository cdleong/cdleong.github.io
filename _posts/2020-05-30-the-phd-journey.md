---
layout: post
title: The PhD Journey
permalink: /phd-journey/
---

So, I've now begun a PhD in machine translation at the University of Dayton. I have it on [good authority](https://ruder.io/10-tips-for-research-and-a-phd/) that blogging about the process can be of great help. This is the start of that! As usual, the idea is for it to be a stable, long-term essay that gets better over time.

So... what am I working on? Well, I'm hoping to apply Machine Translation techniques to assist with low-resource machine translation projects, focusing on assisting Bible translation. I'm quite excited about this for several reasons!

## First, it is _important_. 

I want to work on this because I think it is important. Accelerating translation into low-resource languages is of great importance for several reasons. 

### It may have eternal consequences for speakers of low-resource languages.
I'm a Christian. That means that I believe the Bible to be very very important. I want \<small jungle tribe language> speakers to have the same access to it that I do, which means they need it in their native tongue. Getting the Bible translated even one year earlier might change the lives of many people for the better. 

### It unlocks the world's knowledge for speakers of low-resource languages... and vice versa!
In the article [""Wash your hands" in 500+ languages"](https://datadan.io/blog/wash-your-hands) by Daniel Whitenack, he builds on Bible translation data and projects to translate a public health message into hundreds of very obscure dialects. The speakers of these languages often lack access to even very basic information that is trivial for English speakers to access. 

So the implication is: by assisting Bible translators, we not only translate a religious text, but that translation can actually create the "seed" for translations of other texts as well. This could create a snowball effect, accelerating the translation of knowledge, and dramatically improving the lives of some of the world's most marginalized and forgotten people groups. 

A few examples of low-resource languages (mostly from [List of Wikipedias](https://en.wikipedia.org/wiki/List_of_Wikipedias)): 
* [Abkhaz](https://en.wikipedia.org/wiki/Abkhaz_language). This language has over 100,000 speakers, most of whom live, well... strategically located near Russia. Their access to non-Russian sources of information may be very limited... and the West's understanding of their circumstances is hobbled by the communication divide as well. This could have geopolitical implications going forward. Wikipedia size: about [6k articles](https://ab.wikipedia.org/wiki/%D0%A6%D0%B0%D1%81%D1%82%D3%99%D0%B8:%D0%A1%D1%82%D0%B0%D1%82%D0%B8%D1%81%D1%82%D0%B8%D0%BA%D0%B0)
* [Hani](https://en.wikipedia.org/wiki/Hani_people). Almost 2 million people scattered across southeast Asia. They are largely cut off from English-language information. Having lived among them, I know they have a rich history of legends and folk tales... which we English speakers have almost no access to. Translation tools would enrich both sides. Wikipedia size: 0 pages. It does not exist.
* [Hausa](https://en.wikipedia.org/wiki/Hausa_language). Almost 80 million speakers, mostly in northern Nigeria.  If they had more access to information and resources, perhaps the influence of groups like [Boko Haram](https://en.wikipedia.org/wiki/Boko_Haram) could be countered? Wikipedia size, only about [5k pages](https://ha.wikipedia.org/wiki/Special:Statistics).
* [Zulu](https://en.wikipedia.org/wiki/Zulu_language) The [Zulu people](https://en.wikipedia.org/wiki/Zulu_people), 12 million strong, have a proud history and a rich and unique culture. Wikipedia size: [barely over 2k](https://en.wikipedia.org/wiki/Zulu_Wikipedia)


## Second, it might actually _work_
So, it's valuable work, if it works. But it's not enough for a project to be important, and interesting, it must also be practical. 

### Getting around the low-resource problem.
See, the big problem with using machine translation for this task is the lack of data in target languages. 

It's a sort of catch-22. For Machine Translation techniques to work in the traditional way, you need parallel data. "Computer, here's a sentence in English, here's the same sentence in French, now learn". But when you're working with \<small jungle tribe language>, you don't have any parallel sentences to work with. 

But with _modern_ techniques, people are starting to find ways around this limitation.

#### [LASER](https://engineering.fb.com/ai-research/laser-multilingual-sentence-embeddings/) 
There are many examples of exciting research, but LASER is one that was particularly inspiring to me. Their "Universal, language-agnostic sentence embeddings" essentially allow you to exploit the knowledge gained in one language, and easily adapt that to other languages. 

> "The tool maps a sentence in any language to a point in a high-dimensional space with the goal that the same statement in any language will end up in the same neighborhood."

> "low-resource languages benefit from the resources of high-resource languages of the same family"

I'm also very fascinated by the fact that they managed to recreate language families. As they put it, their automatically discovered relationships "showed an almost perfect correlation with the linguistically defined language families".

Perhaps this can be extended further, to \<small jungle tribe language>?

Going forward, I plan to collect resources [here]({{ site.baseurl }}/translation-and-bible/)

### Getting out of the translator's way
The other big problem with using machine translation for assisting Bible translators is that these techniques can be very clunky. If a translator sitting in a cafe with a crappy laptop and terrible wi-fi can't get it to run, they _will not use it_, no matter how cool it is. Furthermore, if the amount of effort required to get it working _well_ is too high, it won't be worth their time. 

However, recent years have seen significant advances in: 
#### shrinking the machine learning models. 
For example, ["A highly efficient, real-time text-to-speech system deployed on CPUs"](https://ai.facebook.com/blog/a-highly-efficient-real-time-text-to-speech-system-deployed-on-cpus/), where Facebook gets a text-to-speech neural net to run fast on a CPU. That sounds much more realistic. And maybe we could use it to collect some data to seed our model with...?

#### models that can keep learning as you go. 
The dream would be: as you translate more and more of the Bible, the system gets better and better at helping you. And with techniques like ["Transfer Learning in Multilingual Neural Machine Translation with Dynamic Vocabulary"](https://arxiv.org/abs/1811.01137), we're getting closer to that ideal.

## Resources on doing a PhD Successfully:
I'm going to be collecting here any links that seem helpful for the _process_ of doing the PhD. Meta-research, so to speak. 

* [10 Tips for Research and a PhD](https://ruder.io/10-tips-for-research-and-a-phd/) by Sebastian Ruder.
* [Productivity](https://blog.samaltman.com/productivity) by Sam Altman. "It doesn’t matter how fast you move if it’s in a worthless direction.  Picking the right thing to work on is the most important element of productivity and usually almost ignored."