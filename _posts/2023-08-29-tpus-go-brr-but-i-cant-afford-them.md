---
layout: post
title: TPUS Go BRRR... But I Don't Have Data! (Or, Can We Train Language Models Without Billions of Tokens?)
---

(Insert "bitter lesson" meme here) So, you want to build language technology to help people. Say a machine translator, so you can help folks to communicate, to read, to share knowledge... ideally you would like your computer to learn new languages! In fact, maybe you want it to learn _all_ the languages! It turns out this is very hard and _expensive_, let's look at why that is, and how to attack it. 


## What if we just write out all the rules? (Rule-Based)
This is the first approach you might think of. In a nutshell, you and a team of really smart people start writing some rules. This word goes to that word, except when such and such word is in front...

* Pros: Doesn't require any data, and doesn't require machine learning! You can start now! 
* Cons: Writing good rules very very hard and takes a long time. Languages are _complicated_!  

For example: The French translation for "it" in "The dog couldn't cross the road because it was too _tired_" is _different_ from the translation of "it" in "The dog couldn't cross the road because it was too _wide_" [See "The Illustrated Transformer" for more on this example](https://jalammar.github.io/illustrated-transformer/). You have to look at other words in the sentence just to translate "it". 

Catching all these little rules for English/French is hard! You often miss little sneaky details. It could take you a lifetime to get just one pair of languages to a good state... and then you have to start all over again with every new pair!

### Examples:
* TBTA: "The Bible Translator's Assistant"

**Cost**: `$$$$$` Doesn't require any machine learning or specialized computers... but you need experts to write thousands of rules for thousands of pairs languages and all those experts need to eat something. 



## OK, let's just line up a bunch of translation pairs! Machine Learning will save us! (Machine Translation)

Yes! This works pretty well! [citations needed]. You get thousands or millions of pairs of "parallel data". (see my previous article about Hani Machine Translation)

This has (had?) some problems though. Originally you needed a model for every pair of languages, each of which required paired data.

Problems: 
* Needs _lots_ of data for good results! (Though it's getting more efficient)
* Accuracy isn't the best (Though algorithms have gotten better, especially with more data)
* You still need people to translate sentences, lots of sentences, manually. 

This isn't as insurmountable as it might seem, for several reasons. 
* For one thing, there are a lot of people already working to translate things: the Bible, JW books, government meetings, [Children's books...](Bloomlibrary.org). 
* There's also been some success with methods that try and find such pairs automatically on the web, but with a [few quality issues...](https://arxiv.org/abs/2103.12028)

**Cost**: `$$$$`, You don't need so much expert knowledge any more, but gathering all the translations can be hard! 

### Examples
* [JoeyNMT](https://github.com/joeynmt/joeynmt), on which my previous blog posts were based, is a good example. It expects every sentence in language A (the "source") to be properly lined up with a corresponding translated sentence in language B (the "target"). Then once you have that data, you can train a model that goes from language A to language B, and only that direction.

## Can We Just Get A Big Pile of Text In Each Language? (Masked Language Modeling and Encoder/Decoder models)

This has been a major breakthrough in recent years! No longer do you have to line up pairs of translated sentences. Instead, you can "learn" a language by simply gathering piles of monolingual data and using methods that automatically learn patterns about the language. [The Illustrated Word2Vec](https://jalammar.github.io/illustrated-word2vec/) Talks about this somewhat. 

Then we've also learned that you can train big multilingual models on data from many languages, and learn about all those languages at once. Then you can take that knowledge, that representation, and use it for translation, grammar checking and so on! [citation needed]

### Examples: 
* Most of them nowadays. mBERT for example. 
* https://huggingface.co/training-cluster lets you estimate costs for training LLMs. Costs _start_ at $57k for the smallest, on the least data

## Oh... but I don't have piles of monolingual text in my language.

Oh. What _do_ you have? (To be continued!)

<!-- ### Can we use data from other languages?

### Can we scan in some old documents?

### Can we use audio data? -->



## Changelog
2023-08-29: Article begun, many sections still TODO. Hoping to add in historical things like transfer learning, but also move some of the experimental approach sections to "Tatanka" article. Also focus the article on a cost estimate for all the languages.
2023-09-05: Changed the title, fleshed out a bit more, adding some examples and such. 