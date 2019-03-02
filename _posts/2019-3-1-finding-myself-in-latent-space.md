---
layout: post
title: The Quest for the Anti-Me - Finding Myself in Latent Space
---

<figure>
  <video src="{{ site.baseurl }}/images/finding_myself/11k_iterations_256x256_200ms_MG_4136_spedup.mp4" controls autoplay/>
</figure>

(Here's a video of the StyleGAN encoder in action. In this case, within 11,000 iterations, StyleGAN has pretty much nailed my face.)

### StyleGANs and Anti-Faces
Ever since I first came across [StyleGans](https://arxiv.org/abs/1812.04948), Nvidia's hot new image generator tech, I've been overcome with the desire to play around with it. 

It's responsible for some pretty impressive results. To see a few yourself, go to [https://thispersondoesnotexist.com](https://thispersondoesnotexist.com) and refresh your browser a few times. 

Pretty cool, right? 

Well, if you actually go and read the [Research Paper](https://arxiv.org/pdf/1812.04948.pdf), it turns out it is full of fascinating little tidbits, that get the mind going. In particular, I was fascinated by this figure: 

> ![A figure showing an interpolation between faces and corresponding "anti-faces", taken from Nvidia's paper. The figure is labelled "figure 8", and has a lengthy text description beneath]({{ site.baseurl }}/images/finding_myself/anti-face.png  "anti-faces")

I became deeply curious to try it myself. And, I found myself wondering... 

> Could I generate... an **anti-me**? A version of myself, with all attributes flipped? And what would she look like?

### Finding Myself In Latent Space.

Before I could do such a thing, I needed to "find myself". That is to say, I needed to find the set of inputs that, when fed into the StyleGAN model, would generate my original face. These inputs are called "latents", and a particular set of them together is called a "latent vector". Once you've found the particular latent vector that generates your face, you can begin playing around. More on that in the next post. First, we need to "encode" a picture, to find its latent vector.

This is done by feeding in a target picture for StyleGAN to try and match, then doing gradient descent in latent space, using [Frechet Inception Distance](https://nealjean.com/ml/frechet-inception-distance/) for the loss. 

If you don't know what those are, well, basically you start by feeding picking some arbitrary starting latent vector, and doing the following repeatedly:

```
1. Nudge the latent vector just a bit in some direction. Maybe add some random small number to each latent.
2. Feed the new latent vector into the StyleGAN, and use that to generate a new picture. 
3. Check if that new picture looks more similar to the target picture (this is what Frechet.
4. If the new one looks more similar, that was a good nudge, and we're going the right direction. Next time, nudge it more like that!
```

Do that until the new image looks really similar to the target image, and voila!

If you'd like a much better explanation of gradient descent, you could try this [Towards Data Science article](https://towardsdatascience.com/gradient-descent-in-a-nutshell-eaf8c18212f0)

And so, I gave it a try. 

Result? **Success!**


Real Image                 |  Generated Image
:-------------------------:|:-------------------------:
![a]({{ site.baseurl }}/images/finding_myself/MG_4136_01.png)  |  ![a]({{ site.baseurl }}/images/finding_myself/00057000_iterations__MG_4136_01.png)
![a]({{ site.baseurl }}/images/finding_myself/MG_4141_01.png)  |  ![a]({{ site.baseurl }}/images/finding_myself/00013000_iterations__MG_4141_01.png)


### Give it a Try Yourself!

If you'd like to give it a whirl, all the code, including instructions on how to use it, can be found at [this Colab Notebook](https://github.com/cdleong/stylegan-encoder/blob/master/Latent_Me.ipynb). 

If you don't know how to use a Colab Notebook, go to [Google's tutorial](https://colab.research.google.com). Essentially, a Colab Notebook is a visual way of running code right in your browser. The really interesting thing is, Google does all the hard computing work with GPUs behind the scenes, _gratis_. Yes, it's free GPU access. Yes, that does sound fishy and too good to be true. 

Don't worry, the first taste is free. Google will make their money, somehow...

(I adapted that Colab notebook from the Colab Notebook "[Latent Me](https://colab.research.google.com/drive/139OhnW0O_3-4IrnUCXRkO9nJn38qcdsi)", which somebody made based on the [stylegan-encoder repo on Github](https://github.com/Puzer/stylegan-encoder) by "Puzer", which itself extends the [original code from Nvidia](https://github.com/NVlabs/stylegan).)

### Things I Learned, While Finding Myself

#### 1. It has trouble with my glasses. 

Several times, I fed it photos with my glasses on, and it struggled to recreate them. Sometimes catastrophically.



When I fed it this old photo of me...
![Old photo of rounder Colin, with glasses]({{ site.baseurl }}/images/finding_myself/20160225_124223_01.png  "Old photo of rounder Colin, with glasses")

It really struggled with the glasses. After 1000 iterations, it formed a weird sunken-eyed version, with strange depressions where the rims should be:
![generated picture of weird sunken-eyed Colin]({{ site.baseurl }}/images/finding_myself/1000_iters_generated_20160225_124223_01.png  "generated picture of weird sunken-eyed Colin")

After 5000 iterations, it was still trying to figure out the eyebrow/rim region:
![generated picture of Colin, with blurred areas around glasses rims]({{ site.baseurl }}/images/finding_myself/5000_iters_generated_20160225_124223_01.png  "generated picture of Colin, with blurred areas around glasses rims")

And even after 20,000 iterations (which took _hours_ to run), this was the best it could do:
![generated picture of Colin, with blurred areas around glasses rims still]({{ site.baseurl }}/images/finding_myself/20000_iters_generated_20160225_124223_01.png  "generated picture of Colin, with blurred areas around glasses rims still")

#### 2. You really should give it well-lit photos. For your sanity.

This photo was taken at night, halfway through a haircut.
![A fuzzy picture of Colin's face, in poor lighting]({{ site.baseurl }}/images/finding_myself/Webp.net-resizeimage_MG_4008.png  "A fuzzy picture of Colin's face, in poor lighting")

This was the output. It looks like a melted corpse.
![A horrific melted generated facsimile of Colin's face]({{ site.baseurl }}/images/finding_myself/4000_iters_Webp.net-resizeimage_MG_4008.png  "A horrific melted generated facsimile of Colin's face")


...and here's a poorly lit photo of me, also wearing glasses.
![A poorly lit photo of Colin's smiling face]({{ site.baseurl }}/images/finding_myself/2_MG_4019.png  "A poorly lit photo of Colin's smiling face")

Which gave me this:
![A weird, impressionist-painting like image, similar to the photo of Colin]({{ site.baseurl }}/images/finding_myself/2000_iters_Webp-net-resizeimage_2_MG_4019.png  "A weird, impressionist-painting like image, similar to the photo of Colin")

It certainly leaves an _impressionist_ sort of impression, but I'm not sure it quite qualifies as art.



#### 3. It really seems to like PNG files better than jpgs. Also, make sure you check the "alignment".

Part of the process of finding myself involved running the "align" code, which finds and crops out a square portion of the picture you give it. It then tries to generate pictures to match that square, cropped image. Unfortunately, that alignment process can... glitch out. 

I started with a .jpg version of this picture. 
![Decently lit picture of Colin's face, wearing a black-and-red Redhat hat]({{ site.baseurl }}/images/finding_myself/MG_3927.png  "Decently lit picture of Colin's face, wearing a black-and-red Redhat hat")
(I like this one, because I'm wearing my black-and-red Redhat hat.)

Unfortunately, somehow in the alignment, process, something went wrong. The encoder generated a sort of weird, ghostly image. But I didn't download the aligned version to check. I just told it to start trying to match. So it ran for hours, and hours, and after 30,000 iterations I got this: 
![Weird greyscale image with overlapping ghostly Colin faces]({{ site.baseurl }}/images/finding_myself/30000_iters_MG_3927_01.png  "Weird greyscale image with overlapping ghostly Colin faces")

Which is kind of interesting, really. Does this mean it can match literally anything you give it, not just faces?

### And that's all for now.

Tune in next time, as we go _way_ too deep the age, gender, and smileyness... and then _keep going_!

Preview:
<figure>
  <video src="{{ site.baseurl }}/images/finding_myself/MG_4141_old.mp4" controls autoplay/>
</figure>
