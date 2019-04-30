---
layout: post
title: Sidequest - Deep Haiku. Or, "Can Deep Learning Learn 5-7-5 Just From Examples?"
---

*This Is A Sidequest / That Uses GPT2 / To Generate Haikus...*

Inspired by [Gwern's success in finetuning GPT2 for English Poetry](https://www.gwern.net/GPT-2), and by AI Weirdness' various text generation posts (such as ["Discount Strong Bad Emails"](https://aiweirdness.com/post/183868402192/discount-strong-bad-emails) I present GPT2Haiku, which I used to generate the following Haikus (Check them on [Haiku Syllable Counter](http://www.haikusyllablecounter.com/)): 

There's quite a variety. Many seem to be drawn from internet posts:

    A few years ago,
    i wasn't using it on
    anything at all.
    
    If you're not using
    the best builds that are listed
    online, then its fine.
    
    I see this has been
    posted a bunch but I still
    don't know where to find it.

    And we're still upset
    over this video too.
    I'm going to see.

    That being said, what
    value would trading be if
    money was less free?    
      
    I know you're probably
    annoyed with my ignorance.
    Why don't you like them?
    
    It's amazing what
    the screen can do for someone's
    day to day too Wow    

Some are unkind:

    I'm glad that you feel
    this way because I think you
    really suck. Come on.
    
    [deleted] Good luck!
    I can't tell if you're being
    silly or stupid.
    
And some are kinder: 
    
    That's insensitive.
    Just say what they've said and talk
    to them about it.

Some are helpful, if you're looking for a new local congregation:
    
    I also visit
    the church regularly, too.
    It's pretty awesome.

    Our prayer at Penang
    is pretty local compared
    to most cities around.

This one gives some info about the local power dynamics:

    Druids are forest
    guardians and they fight back
    against the warlords.    
    
There's some political (?) ones?

    I always forget
    all about Obama but
    he got injured once.

    It is more than will
    Hillary keep screwing him
    over the republic.
    
I'm not entirely sure what this one is about, but I think I agree that I'm glad about it: 

    Well at least now he's
    not wearing anabolic
    swamp or anything.        

These two gaming comments were generated right next to each other: 
    
    It was amazing.
    You can tell it just by how
    well they built that game.

    You can tell it just
    by how well they built that game.
    They didn't fake their deaths.    
    
    
I like this one about qualia of flora consumption: 

    That said, I haven't
    experienced the exact
    vegetable you offered.
    
There's a few giving sage life advice:
    
    Having a rich dad
    is mostly common sense, but
    maybe you should read.

    You are free to do
    as you wish, but the problem
    is that they don't care.

And this one is *Le Profound*:
    
    The reason people
    believe in France is because
    the country is French.      

# But... Why? And How? But Mainly, Why?    
A bit of background here: I've been wondering for a while whether deep learning is capable of learning rules such as one might find in poetry, *purely by example*. When I saw that Gwern's Gutenberg Poetry Corpus-trained model could generate Iambic Pentameter, I decided to give GPT2 a try with Haikus. 

It was surprisingly difficult to find a training corpus of sufficient size, with suitable content. Finally I discovered [Reddit Haikus](http://haiku.somebullshit.net/), which contains "every haiku that occurred on a Reddit comment between December 2005 and October 2016 (10.8 years). Some intentional, most unintentional."

The author mentions on that page, that "Details of how this was done are available in a blog post." However, the blog post was gone:
![Missing Blog Post]({{ site.baseurl }}/images/deep_haikus/blog_post.PNG)*Screenshot of missing blog post, with 403 error*  

All was not lost, though! I found [a backup on The Wayback Machine](https://web.archive.org/web/20170108210559/http://somebullshit.net:80/post/reddit-haikus/). 

Then, *that* gave me the link to the [entire saved archive of haikus](http://somebullshit.net/haikus.txt.xz). **Almost 250,000 haikus**!

Once I had that, I was able to follow Gwern's guide to fine-tune, and *et voila! Deep Haikus!*

So yes, I didn't tell GPT2 to generate me 5-7-5 haikus, it learned to do it *entirely by example.*
    
# A Few Notes
A couple fun items about this process to take note of: 

### There's some repetition?
This one showed up quite a few times for some reason, during training: 

        Forgetting these points
        are almost definitely
        why it was removed.

### Maybe GPT2 really IS memorizing The Internet?

I have heard some people theorize that GPT2 is really just memorizing large chunks of the Internet. 

I tried testing this by searching both the input text and Google on several occasions, and mostly came up with no results... with one exception. Readers of classic science fiction may recognize this snippet: 

        And when it has gone
        past I will turn the inner
        eye to see its path.

(Look familiar?)

For those of you who don't know, this is a *direct quotation* from the "Litany against Fear", a poem in the seminal SF novel [*Dune*](https://en.wikipedia.org/wiki/Dune_(novel)), by Frank Herbert. 

There is no way it just "came up" with that. 

### My training data wasn't perfect, so neither were the results
Not all of them are 5-7-5. It turns out that the original source corpus isn't completely 5-7-5 haikus. So, garbage in, garbage out. ¯\\_(ツ)_/¯

### Garbage in, potty humor out.
*Lots* of the haikus it generated are quite dirty. This is perhaps to be expected, given the source of the training data. -_-

Final thought, generated by GPT2: 

    I wish I was smart
    enough to have figured out
    myself that out first.
    
Wise words, GPT2, wise words.

# Credits:
* http://haiku.somebullshit.net/ for the corpus of about 250,000 haikus which I used to train GPT2. 
* Gwern, for detailing how to finetune GPT2 with a specific language corpus, such as the Gutenberg Poetry Corpus. 
    
