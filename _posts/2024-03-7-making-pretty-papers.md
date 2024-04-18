---
layout: post
title: Producing Pretty Papers, a Programmer's Guide
---
![Figure 1 from the paper Attention is All You Need, showing "The Transformer - model architecture". It is a complex diagram with a multitude of blocks connected by arrows]({{ site.baseurl }}/images/pretty_papers/AttentionIsAllYouNeedFig1.png) 
I've often wondered, when reading research papers, how exactly it is that people make such pretty figures like this. I decided to look into it, and start collecting resources here.

[Academic Project Page Template](https://github.com/eliahuhorwitz/Academic-project-page-template?tab=readme-ov-file)
Related, a nice github repo for creating slick research project web pages.

## Possible tools

### Tikz
Tikz: make graphs natively in LaTeX.

* https://tikz.dev/tutorial

### PlothNeuralNet

[github repo](https://github.com/HarisIqbal88/PlotNeuralNet).

> Latex code for drawing neural networks for reports and presentation. Have a look into examples to see how they are made. Additionally, lets consolidate any improvements that you make and fix any bugs to help more people with this code.

Essentially, a collection of functions that output tikz stuff you can copy into your LaTeX. 

## Sources

### Asked Researchers on Slack

Asked various devs on Slack, here are misc. graphing tools mentioned by various developers:

* https://mermaid.js.org/
* https://inkscape.org/
* https://affinity.serif.com/en-us/designer/
* https://tikz.dev/
* draw.io

### Searching variations on "Plot Neural Network Architecture"

[this Data Science stack exchange discussion](https://datascience.stackexchange.com/questions/12851/how-do-you-visualize-neural-network-architectures) mentions a few options.


[Appsilon tutorial](https://www.appsilon.com/post/visualize-pytorch-neural-networks)

[PyTorch Forum discussion](https://discuss.pytorch.org/t/how-to-visualize-draw-a-model/176263)

[This Neptune AI post](https://neptune.ai/blog/deep-learning-visualization) points out a few options.

[Machine Learning Mastery tutorial](https://machinelearningmastery.com/visualizing-a-pytorch-model/) explaining how to save out a PyTorch model and visualize using Netron.

[Reddit Thread on NNViz](https://www.reddit.com/r/pytorch/comments/11bwudq/nnviz_can_visualize_any_pytorch_model/)

[TowardsAI](https://towardsai.net/p/machine-learning/creating-stunning-neural-network-visualizations-with-chatgpt-and-plotneuralnet) says to use PlotNeuralNet with ChatGPT. The takeaway for me is to use PlotNeuralNet

[TowardsDataScience article](https://towardsdatascience.com/the-new-best-python-package-for-visualising-network-graphs-e220d59e054e) recommending `gravis`

[github repo "Tools to Design or Visualize Architecture of Neural Network "](https://github.com/ashishpatel26/Tools-to-Design-or-Visualize-Architecture-of-Neural-Network), last updated 3 years ago, shows lots of examples, seems to have lifted from the stack exchange discussions.

Options:
* [PyTorchViz](https://github.com/szagoruyko/pytorchviz)
* [TensorBoard](https://pytorch.org/tutorials/intermediate/tensorboard_tutorial.html)
* [TorchView](https://github.com/mert-kurttutan/torchview)
* [Netron](https://github.com/lutzroeder/netron)
* [Torchviz](https://github.com/szagoruyko/pytorchviz)
* [NNViz](https://github.com/LucaBonfiglioli/nnviz)
* [Net2Viz](https://github.com/viscom-ulm/Net2Vis) Needs Keras. Project looks dead. Online demo [here](https://viscom.net2vis.uni-ulm.de/ZbSatnwLnFCmkxSQpRCAdDDm7KUhCy7fpweSJ6ujOiv26kyRpr).
* [NeuralPlot](https://github.com/Rajsoni03/neuralplot). Again looks dead.
* [NN-SVG](https://github.com/alexlenail/NN-SVG) looks pretty slick, a few mentions above. 
  * [demo running in browser](http://alexlenail.me/NN-SVG/LeNet.html) looks quite cool. And lets you just define the arch right there.
* [gravis](https://robert-haas.github.io/gravis-docs/index.html) "Its name stands for graph visualization and its purpose is to create interactive 2D and 3D plots of graphs and networks". Neat!
* [PlotNeuralNet](https://github.com/HarisIqbal88/PlotNeuralNet) gets a lot of mentions, and has pretty results.
* [TensorSpace](https://github.com/tensorspace-team/tensorspace). Lets you make 3D JS visuals.
* [BertViz](https://github.com/jessevig/bertviz) is relevant here I guess!

I think the tl;dr is that PlotNeuralNet requires you to manually specify things, but looks the prettiest.

Second-prettiest might be the NN-SVG, which looks really easy to use as well.

## Changelog

* 2024-03-07: creation.
* 2024-04-18: added some new library links, and sections.
