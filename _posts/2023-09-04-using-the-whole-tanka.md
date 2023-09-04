---
layout: post
title: Using the Whole Tatanka
---
![Hani Storybook Sample]({{ site.baseurl }}/images/tatanka/whole-tatanka.png) 
In the 1990 film _Dances with Wolves_, Kevin Costner's character encounters a group of people whose language he does not speak. In order to start establishing a common vocabulary, he uses his body to mime the shape and behavior of a buffalo/bison. Recognizing this, they teach him their word for the animal ("Tatanka"). Our computational approaches to language learning miss out on this kind of thing, relying on massive quantities of mostly text, and leaving much of the data we do have unused. When I've spoken with actual linguists working on smaller languages, I've found that the they often _have_ data, it's just not in a form that computers can use easily, distributed across many formats and files. How can we "use the _whole Tatanka?_", not wasting the data that is available?

In the last post, I talked about the challenges to low-resource language modeling, which all fundamentally comes down to _cost_. If it's so expensive to gather data, that suggests we would want to creatively use all the data we _do_ have. How can we "unlock" data sources, using everything available ("the whole Buffalo") to learn languages? Let's look at some ideas for how we can do that!

Here let's start listing out nontraditional sources of language data, other than digital text. 
* Audio data! Easier to collect maybe, but harder to use? Recent approaches make more use of this.
* Videos! Very informationally rich, really difficult for computers to use. 
* Books! Like, paper books. Humans can read them, computers struggle. Ideas like PIXEL can help, maybe?
* Digital data, but it's not formatted in just the right way. Like, if someone took a bunch of notes in Microsoft Word...
* Just, like, visual information in general. You see an ad on the side of a bus with someone enjoying a nice beverage. Without even knowing the language, you can know that the words on the ad have something to do with the concept of, like, "you should buy this nice beverage". 

A lot of these data sources are still underutilized. 

#### Changelog
2023-09-04: Article begun, premise established. Next time, flesh out some of the ideas and previous work. In particular, what's up with the random files?