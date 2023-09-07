---
layout: post
title: Which Of These Words Means "Zebra"? (Or, Using Visual and Context Clues to Learn Words With Just A Few Examples)
---
![Zebra]({{ site.baseurl }}/images/tatanka/animals_zebra.png) ([image source cc-by](https://bloomlibrary.org/language:ru/book/G1ND6t4gq1?lang=ru)). Previously, we've discussed the fact that visual information is underutilised, and could help us in language modeling, today I want to look at an example! Now, I don't read Russian, and I don't know what any of these words mean. But, looking at this illustration from a children's storybook on the [Bloom Library](https://bloomlibrary.org), I can start to make some guesses. Most likely, the words underneath have something to do with what's in the image. So which of them, if any, means "Zebra"? This is something Humans can puzzle out, but is hard for NLP. If we can develop ways for machines to "unlock" this knowledge, that would help them learn new languages with limited examples.

"Зебра пасётся в поле." Are the words in question. First clue I notice is that "в" is way too short. I would be very surprised if any language would allocate a single character to a relatively rare creature like a Zebra, and even more surprised if Russian was one of those languages. Not many Zebras up there. 

So that leaves us "Зебра", "пасётся", and "поле". My gut already says that the first one just kinda looks the right length, and kinda looks like the English word "zebra"... but can we find any clues to help confirm this?

Flipping forward in the book we see other pages have different animals, and different text: 
![Zebra]({{ site.baseurl }}/images/tatanka/animals_4.png)

Comparing these 4 images, we can notice that... 
1. "в" is indeed not unique to any of them. Most likely some sort of article like "a" or "the".
2. "в воде" is likewise not unique, it shows up for the goose and the hippo. Perhaps "the pond" or "the lake" or something? But certanly it cannot mean "goose" or "hippo". 
3. ...but that would then imply that, in the caption under the Zebra, "в поле" does NOT contain the word Zebra. So we can narrow things down a bit, our candidates we are left with are "Зебра", and "пасётся".


Now lining up some more of the text in the book...
<pre><code>
* "Зебра   пасётся в  поле."    (under a Zebra.)
* "Утка    ныряет  в  воду."    (under a Goose.)
* "Краб    ползёт  по камню."   (under a Crab.)
* "Рыба    плавает в  воде."    (under a Fish.)
* "Ящерица ползёт  по камню."   (under a Lizard of some kind.)
</code></pre>

Ignorant as I am of Russian grammar and words, I think we can still conjecture that the second word in each sentence looks like some kind of verb? About the right length to be some sort of conjugated word like "swimming" or "climbing", which would make sense as well, as it would then connect the starting noun (Zebra, Crab, Fish), with the activity/location. 

At this point, my hypothesis is that these are all following some pattern along the lines of "animal activity a/the (some final noun, maybe the place?)".

Oh, and "Краб" kinda looks like "Krab", so that's a tiny bit of weak evidence for "first word is name of animal". 

So our final guess is that "Зебра" most likely means "Zebra". 

...And a quick search confirms that it is, in fact, the right word! 

...which then unlocks, for us, the ability to read the rest of the names. We can now confidently know the word for Crab, Goose/Duck, Lizard, and so on, because if they all follow the same pattern with the name of the animal first, then the first word in each of the other captions must be the name of that creature. Almost like a game of Sudoku, confirming one item can unlock knowledge of others elsewhere. 

In this exercise we used... 
* visual knowledge about what's in the pictures, 
* knowledge about how languages work in general (short words vs long, verbs exist, nouns exist), 
* knowledge about Russia and Russian geography (Not many Zebras there, and since they're both European languages it's reasonable to expect the scripts might be related to English)
* information across multiple pages, to notice patterns and find counterexamples,
* knowledge of English words and their visual similarity to Russian words, 
...and then leveraged that knowledge to guess some vocabulary. 

This sort of logical reasoning process, where we notice patterns, form and test theories, apply world knowledge, and so on, is currently very hard for AI/ML to do. But it lets us, as humans, learn new words and new languages with not very many datapoints at all. If we can get computers to do something like this, could they similarly learn new languages more efficiently?

Perhaps we could start with something like...

<pre><code>
for each image/caption pair: 
    # use object detectors and/or image captioners?
    generate a list of objects and activities that might be associated
Start to correlate what words tend to go with what pictures/objects/activities

</code></pre>
...perhaps this can be a loss function as well? As in, try to predict what objects/verbs (in English) go with which words/tokens in the other language. Over time, build up some vocabulary that way. Or perhaps use English captions as pseudo-parallel-data?

That's the tricky bit, of course. Once you have the words and concepts in the picture enumerated, then associating that with the words underneath is tricky. Perhaps you could literally just generate data like this: 

<pre><code>
"Зебра пасётся в поле." is the caption under a picture of a Zebra grazing. 
</code></pre>

And then feed that whole sentence, verbatim, as training data into some sort of multitask model? Or an instruct-style model? Give it the instruction to predict what sort of picture this is the caption of, say? 

This would have the advantage of turning image/caption pairs, effectively into text, which we can then feed into existing text-based pipelines! 

However we end up doing it, I think unlocking this image/language associational information could be very helpful, and advance our ability to "use the whole tatanka", more efficiently learning languages with fewer data points.

#### Further Reading
* [CLIP (Contrastive Language-Image Pretraining)](https://github.com/openai/CLIP), which is used to "Predict the most relevant text snippet given an image"
* [Unsupervised Multimodal Neural Machine Translation with Pseudo Visual Pivoting](https://aclanthology.org/2020.acl-main.731/), which uses image captioning models to generate artificial data to train machine translation algorithms. Could we leverage a similar process with existing captions?
* [Aligning language models to follow instructions](https://openai.com/research/instruction-following), OpenAI's article explaining the process of training an "instruct-style" model. 
* [Introducing Aya: An Open Science Initiative to Accelerate Multilingual AI Progress](https://txt.cohere.com/aya-multilingual/), a project to train a highly multilingual "instruct-style" model. 
* [NLLB-CLIP](https://twitter.com/visheratin/status/1699789363759231177): CLIP in 201 languages!

#### TODO

#### Changelog
* 2023-09-07: Article begun, got the basic idea down and explicated, along with a few related reading items. 