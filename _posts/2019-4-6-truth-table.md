---
layout: post
title: The Quest for the Anti-Me - Truth in Tables
---
Hi, and welcome back to **_Deeply Goofy_!**
We're still goofing around in Latent Space. Last time, I promised we'd "go _way_ too deep into age, gender, and smileyness... and then _keep going_!" That's still coming, but first, we must find fill a table, with truth!

### What's a Latent Direction?
As you may recall, last time I found myself in latent space. That is to say, I found the big list (the "vector") of numbers (or "latents"), that, when fed into the program, generates my face:
![my face, generated]({{ site.baseurl }}/images/finding_myself/00057000_iterations__MG_4136_01.png)

The really _cool_ thing about these "latent vectors", is that there are... directions you can go. And often, those directions are understandable by people.

So, for example, you can take a whole bunch of latent vectors of people smiling, and another group of people frowning, and do something like:

`(smiling people) - (not smiling people) = [some new big list of numbers]`

That new list of numbers describes (approximately) the "smiling/not-smiling" direction. So then you take that, and add it to a latent vector that describes someone's face, and get something like this:

![a]({{ site.baseurl }}/images/goofing_around/truth_table/teaser.png)
(In this picture, from the original [Stylgan-Encoder repo by Puzer](https://github.com/Puzer/stylegan-encoder), Puzer has managed to find a latent direction that describes frowning/smiling. They then added and subtracted that latent direction to pictures of famous politicians. The original photos are down the center.)

I was wondering whether I'd be able to do anything like this myself, given the right pairs of pictures. I went out into a local park with my friend, and we took a set of 8 pictures...

### The Truth Table
So, I was wondered what the difference would be, 
* between hatted me, 
* and smiling me, 
* and with my glasses on so I can see. 

So I made the combinations of all three! 

(This sort of layout, where you write down every combination in yes/no format, is called a "Truth Table")

| Hat 	| Glasses 	| Smile 	| Original File 	| Generated after a While 	|
|-----	|---------	|-------	|------	|-------	|
|no  	|no      	|no    	| ![4138]({{ site.baseurl }}/images/goofing_around/truth_table/MG_4138_01.png)*Aligned original: MG_4138* 	| ![4138]({{ site.baseurl }}/images/goofing_around/truth_table/00004500_iterations__MG_4138_01.png)*4500 iterations, FID=0.12*  	|
|no  	|no      	|ya    	| ![4142]({{ site.baseurl }}/images/goofing_around/truth_table/MG_4142_01.png)*Aligned original: MG_4142*    | ![4142]({{ site.baseurl }}/images/goofing_around/truth_table/00003600_iterations__MG_4142_01.png)*3600 iterations, FID=0.14*  	|
|no  	|ya      	|no    	| ![4139]({{ site.baseurl }}/images/goofing_around/truth_table/MG_4139_01.png)*Aligned original: MG_4139*  	| ![4139]({{ site.baseurl }}/images/goofing_around/truth_table/00009868_iterations_best_loss_thus_far_0.0915113165974617__MG_4139_01.png)*9868 iterations, FID=0.09*  	|
|no  	|ya      	|ya    	| ![4143]({{ site.baseurl }}/images/goofing_around/truth_table/MG_4143_01.png)*Aligned original: MG_4143*  	| ![4143]({{ site.baseurl }}/images/goofing_around/truth_table/00011400_iterations__MG_4143_01.png)*11400 iterations, FID=0.13*  	|
| Hat 	| Glasses 	| Smile 	| Original File 	| Generated after a While 	|
|ya  	|no      	|no    	| ![4137]({{ site.baseurl }}/images/goofing_around/truth_table/MG_4137_01.png)*Aligned original: MG_4137*  	| ![4137]({{ site.baseurl }}/images/goofing_around/truth_table/00001500_iterations__MG_4137_01.png)*1500 iterations, FID=0.19*  	|
|ya  	|no      	|ya    	| ![4141]({{ site.baseurl }}/images/goofing_around/truth_table/MG_4141_01.png)*Aligned original: MG_4141*  	| ![4141]({{ site.baseurl }}/images/goofing_around/truth_table/00013000_iterations__MG_4141_01.png)*13000 iterations, FID=0.08*  	|
|ya  	|ya      	|no    	| ![4136]({{ site.baseurl }}/images/goofing_around/truth_table/MG_4136_01.png)*Aligned original: MG_4136*  	| ![4136]({{ site.baseurl }}/images/goofing_around/truth_table/00057000_iterations__MG_4136_01.png)*57000 iterations, FID<0.05*  	|
|ya  	|ya      	|ya    	| ![4140]({{ site.baseurl }}/images/goofing_around/truth_table/MG_4140_01.png)*Aligned original: MG_4140*  	| ![4140]({{ site.baseurl }}/images/goofing_around/truth_table/00012000_iterations__MG_4140_01.png)*12000 iterations, FID<0.16*  	|


### Bonus Image
![Me, with Cowboy Hat on.]({{ site.baseurl }}/images/goofing_around/truth_table/best_loss_thus_far_0.12451311200857162_00019688_iterations__MG_4148_01.png)*Me, with Cowboy Hat on. 19688 iterations, FID=0.12* 


Note: this took _forever_ to do. I had to roll balls down hills for _hours_.

Next time: Can we roll a ball down a hill more efficiently?

![Gradient descent animation from "An overview of gradient descent optimization algorithms" by Ruder](http://ruder.io/content/images/2016/09/saddle_point_evaluation_optimizers.gif)


(Image from ["An overview of gradient descent optimization algorithms"](http://ruder.io/optimizing-gradient-descent/index.html) by Sebastian Ruder)

<!-- No idea why this works, and nothing else does, but now I have a table so yay? -->
<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>