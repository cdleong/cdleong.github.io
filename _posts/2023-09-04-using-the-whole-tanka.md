---
layout: post
title: Using the Whole Tatanka (Or, I Don't Have A Massive Pile of Text, How Can We Use ALL the Data We've Got?)
---
![Hani Storybook Sample]({{ site.baseurl }}/images/tatanka/whole-tatanka.png) 
In the 1990 film _Dances with Wolves_, Kevin Costner's character encounters a group of people whose language he does not speak. In order to start establishing a common vocabulary, he uses his body to mime the shape and behavior of a buffalo/bison. Recognizing this, they teach him their word for the animal ("Tatanka"). Our computational approaches to language learning miss out on this kind of thing, relying on massive quantities of mostly text, and leaving much of the data we do have unused. When I've spoken with actual linguists working on smaller languages, I've found that the they often _have_ data, it's just not in a form that computers can use easily, distributed across many formats and files. How can we "use the _whole Tatanka?_", not wasting the data that is available?

In the last post, I talked about the challenges to low-resource language modeling, which all fundamentally comes down to _cost_. If it's so expensive to gather data, that suggests we would want to creatively use all the data we _do_ have. How can we "unlock" data sources, using everything available ("the whole Buffalo") to learn languages? Let's look at some ideas for how we can do that!

Here let's start listing out nontraditional sources of language knowledge, other than digital text. 
* Audio data! Easier to collect maybe, but harder to use? Recent approaches make more use of this. One way 
* Videos! Very informationally rich, really difficult for computers to use. 
* Books! Like, paper books. Humans can read them, computers struggle. Ideas like PIXEL can help, maybe?
* Digital data, but it's not formatted in just the right way. Like, if someone took a bunch of notes in Microsoft Word...
* Just, like, visual information in general! More on this later.
* Interactivity! Make a guess about how the language works, and try it. Or, you know, ask: "would it make sense to say it this way?"

## No really, visual information is badly underused
In fact, let's expand on that a bit! Here's a few examples of visual information humans can use in learning a language, but computers would currently have _no_ _clue_ what to do with:
* You see an ad on the side of a bus with someone enjoying a nice beverage. Without even knowing the language, you can know that the words on the ad have something to do with the concept of, like, "you should buy this nice beverage". If you know the script at all, you can probably guess which word is a proper noun, the name of the beverage. 
* Gestures and body language. Someone points at a tree and says "Shu".

One interesting paper that I often think of is [Unsupervised Multimodal Neural Machine Translation with Pseudo Visual Pivoting (Huang et al., ACL 2020)](https://aclanthology.org/2020.acl-main.731), where they use image captioners in both languages to generate "parallel data" by feeding the same image in to both captioners.

### The Buying Things At The Market Problem
One strategy for learning a language - you can make educated guesses based on conversational context. You go to the market and notice there's someone selling a fish. Someone else walks over and they take turns speaking. The newcomer points at a fish and says something with an intonation that sounds like a question. The other one says something in response. The newcomer nods, and pulls out some money. 

From all of this, someone who is paying attention could probably start taking a guess at words for, like, _fish_, and maybe that question response was a number? And so forth. You can start to learn, overall, what words, what sounds, are associated with "buying fish at the market". Then if you hear those again later, you can remember and cross-check. 

Can we make a system that could do this?

### The Tunic Problem
Oh man, have you ever played the video game _Tunic_? Can you imagine what it would take to learn that language?? Looking at the visual context of when/where pictures show up, noticing repeated patterns, deceiphering the script?

### Children's Picture Books Problem
I want to expand this whole section and talk about things like the [Bloom Library](https://bloomlibrary.org). But essentially: 
* Here is a book with pictures, and there are words underneath. You don't know what they say.
* Here is another book with _the_ _same_ _pictures_ in the _same_ _order_. But with _different_ words. You don't know what they say either.

Can you start building a dictionary from these?

One idea I want to try: just, like, run a bunch of object detection or scene description models to build a sort of "associated word cloud" for the text in the books, and test if that can be used for anything useful. 

(one idea to build a dataset for this that you KNOW is accurate - substitution-cipher "translation" of English into a transmuted version, Helihsgn)



## Audio Data is also hard
Here I'd like to expand on ideas like Allosaurus, Wav2Vec, Wav2Vec2Phoneme

<!-- ### Phone it in?  -->
<!-- 
### The Automatic Audio Language Learner Network
(TODO: idea - What if you could deploy a network of devices that would not save any data, not record any audio, but only learn a language?) -->

#### Changelog
2023-09-04: Article begun, premise established. Next time, flesh out some of the ideas and previous work. In particular, what's up with the random files?
2023-09-05: Adding more thoughts on using visual information, in particular. Would like to add in more on audio data.