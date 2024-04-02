---
layout: post
title: A Coder's Guide to Sign Language Translation in 2024
---
![My Name is Colin]({{ site.baseurl }}/images/slt_for_coders/self_intro_asl.gif) ([`MY`](https://www.handspeak.com/word/1448/) [`NAME`](https://www.handspeak.com/word/1464/) [`C-O-L-I-N`](https://www.handspeak.com/learn/408/), aka "My name is Colin")
So, it's 2024, you want to try your hand (haha) at sign language translation! Not recognizing _finger-spelling_ in static images, you want to do full-on _translation_: videos come in with someone signing, and full sentences come out the other side in a spoken language. Where does one even start? Here's a few places to begin! An opinionated guide to Sign Language Translation for coders.

Prerequisites: you will be working with video data, so you will need:

* LOTS of hard drive space.
* LOTS of RAM.

I actually had some success getting things running in Colab Pro, but of course there's plenty of issues with that.

<!-- TODO: TOC here? https://heymichellemac.com/table-of-contents-jekyll https://stackoverflow.com/questions/9602936/how-to-create-a-table-of-contents-to-jekyll-blog-post https://stackoverflow.com/questions/18244417/how-do-i-create-some-kind-of-table-of-content-in-github-wiki-->

## A Coder's Guide to SLT (for me)

Truth be told, I am writing this to be the guide I wish I had - the guide that a coder needs, in order to Do Science within the realm of Sign Language to Text. Nevertheless I welcome feedback/suggestions!
<!-- TOC created with VSCode Markdown all in one extension -->
- [A Coder's Guide to SLT (for me)](#a-coders-guide-to-slt-for-me)
- [Codebases](#codebases)
  - [SLT Resources that seem replicable/usable](#slt-resources-that-seem-replicableusable)
    - [Sign Language Datasets](#sign-language-datasets)
    - [Two-Stream Network for Sign Language Recognition and Translation, NeurIPS2022](#two-stream-network-for-sign-language-recognition-and-translation-neurips2022)
    - [KnowComp submission for WMT SLT](#knowcomp-submission-for-wmt-slt)
    - [WMT SLT 2023 Sockeye Baseline](#wmt-slt-2023-sockeye-baseline)
    - [WMT-SLT23 I3D baseline, aka "Sign Language Translation from Instructional Videos"](#wmt-slt23-i3d-baseline-aka-sign-language-translation-from-instructional-videos)
    - [Pose-Format](#pose-format)
    - [MediaPipe Holistic Hand Landmarker](#mediapipe-holistic-hand-landmarker)
  - [Needs more effort to get running](#needs-more-effort-to-get-running)
    - [SLTUnet](#sltunet)
    - [Tackling Low-Resourced Sign Language Translation: UPC at WMT-SLT 22. One of the submissions to the WMT SLT '22 contest](#tackling-low-resourced-sign-language-translation-upc-at-wmt-slt-22-one-of-the-submissions-to-the-wmt-slt-22-contest)
  - [Not yet fully assessed](#not-yet-fully-assessed)
    - [Isolated Sign Recognition from RGB Video using Pose Flow and Self-Attention](#isolated-sign-recognition-from-rgb-video-using-pose-flow-and-self-attention)
    - [Temporal Lift Pooling for Continuous Sign Language Recognition](#temporal-lift-pooling-for-continuous-sign-language-recognition)
    - [Spatial-Temporal Graph Convolutional Networks for Sign Language Recognition](#spatial-temporal-graph-convolutional-networks-for-sign-language-recognition)
    - [Read and Attend: Temporal Localisation in Sign Language Videos](#read-and-attend-temporal-localisation-in-sign-language-videos)
    - [Better Sign Language Translation with STMC-Transformer](#better-sign-language-translation-with-stmc-transformer)
    - [Sign Language Transformers (CVPR'20)](#sign-language-transformers-cvpr20)
  - [Seems out of date](#seems-out-of-date)
    - [OpenHands (possibly a dead project?)](#openhands-possibly-a-dead-project)
  - [Tricks for getting old code working](#tricks-for-getting-old-code-working)
- [Good places to look for more](#good-places-to-look-for-more)
  - [research.sign.mt - best place for SLT background](#researchsignmt---best-place-for-slt-background)
  - [sign.mt and sign-language-processing github orgs](#signmt-and-sign-language-processing-github-orgs)
  - [👋 Sign Translate Wiki](#-sign-translate-wiki)
  - [WMT Shared Tasks](#wmt-shared-tasks)
    - [WMT-SLT22](#wmt-slt22)
    - [WMT-SLT23](#wmt-slt23)
  - [PapersWithCode](#paperswithcode)
- [Survey of Sign Language Processing Surveys](#survey-of-sign-language-processing-surveys)
  - [SLP in general](#slp-in-general)
    - [Sign Language Recognition, Generation, and Translation An Interdisciplinary Perspective (2019)](#sign-language-recognition-generation-and-translation-an-interdisciplinary-perspective-2019)
    - [Systemic Biases in Sign Language AI Research A Deaf-Led Call to Reevaluate Research Agendas (2024)](#systemic-biases-in-sign-language-ai-research-a-deaf-led-call-to-reevaluate-research-agendas-2024)
  - [Specifically SLT](#specifically-slt)
    - [Machine Translation from Signed to Spoken Languages State of the Art and Challenges (2022)](#machine-translation-from-signed-to-spoken-languages-state-of-the-art-and-challenges-2022)
    - [A survey on Sign Language machine translation (2023)](#a-survey-on-sign-language-machine-translation-2023)
    - [Sign Language Translation A Survey of Approaches and Techniques (2023)](#sign-language-translation-a-survey-of-approaches-and-techniques-2023)
    - [Sign Language Processing (research.sign.mt)](#sign-language-processing-researchsignmt)
  - [Other SLP tasks](#other-slp-tasks)
    - [Unraveling a Decade A Comprehensive Survey on Isolated Sign Language Recognition (2023)](#unraveling-a-decade-a-comprehensive-survey-on-isolated-sign-language-recognition-2023)
    - [A Extensive Survey on Sign Language Recognition Methods (2023)](#a-extensive-survey-on-sign-language-recognition-methods-2023)
    - [Sign Language Production: A Review (2021)](#sign-language-production-a-review-2021)
    - [Sign Language Recognition: Current State of Knowledge and Future Directions (2023)](#sign-language-recognition-current-state-of-knowledge-and-future-directions-2023)
  - [Miscellaneous](#miscellaneous)
    - [Comparative Analysis of Multiple ML Models and Real-Time Translation (2023)](#comparative-analysis-of-multiple-ml-models-and-real-time-translation-2023)
    - [Indian Sign Language Recognition Using Classical And Machine Learning Techniques A Review (2023)](#indian-sign-language-recognition-using-classical-and-machine-learning-techniques-a-review-2023)
    - [Quantitative Survey of the State of the Art in Sign Language Recognition (2020)](#quantitative-survey-of-the-state-of-the-art-in-sign-language-recognition-2020)

## Codebases

I personally like having some code to work with, instead of having to code things from scratch. Of course, Sign Language Processing is a developing field. Much of what exists out there is research code. So getting it running isn't always easy. I went ahead and took a crack at some of them, results below. Mainly what I was looking for was:

1. How recently maintained/updated is this? If it's been a long time, it will be harder to get running.
2. The ImportError test: How hard is it to just install the requirements and run? Can I boot up a Colab Instance and get the scripts running without ImportErrors?

### SLT Resources that seem replicable/usable

#### [Sign Language Datasets](https://github.com/sign-language-processing/datasets)

* This is a very helpful repo/library. Hard to get anything done without data! And this library gives you many datasets to choose from, and a standardized code interface to do it with!
* Actively maintained and updated, often responses/updates come within the same day!
* And they have a nice Colab Notebook linked from the README, always nice to see. Which runs quite nicely.

#### [Two-Stream Network for Sign Language Recognition and Translation, NeurIPS2022](https://github.com/FangyunWei/SLRT/tree/main/TwoStreamNetwork)

* One of the top papers on PapersWithCode for sign language translation.
* [My attempt to get this running](https://colab.research.google.com/drive/1Z1P__o3NTl0zAStigUSlbeqxOVAaqOKT) Successfully installed requirements in Colab. This one looks pretty cool.

#### [KnowComp submission for WMT SLT](https://aclanthology.org/2023.wmt-1.36/)

* [My attempt to get this running in Colab](https://colab.research.google.com/drive/1le0qsaMwCH0hE6IkuVK1ImRKjsBBMHy2?usp=sharing)
I was able to get the requirements installed! Just don't have the needed data yet.

#### [WMT SLT 2023 Sockeye Baseline](https://github.com/bricksdont/sign-sockeye-baselines)

* [My attempt to get it running in Colab](https://colab.research.google.com/drive/1P2KWBOktCakuAfJPxFoBlpk1KXOGS6JP?usp=sharing)
Ran into some trouble, haven't yet found a way to get them all installed.

#### [WMT-SLT23 I3D baseline, aka "Sign Language Translation from Instructional Videos"](https://ieeexplore.ieee.org/document/10208355)

* [repo](https://github.com/imatge-upc/slt_how2sign_wicv2023)
* [My attempt to install the requirements for the I3D baseline in Colab](https://colab.research.google.com/drive/1GKHID0WMbXwnWXHD1iXQfs95nhWz9NQt?usp=sharing)
... got them installed! Haven't yet managed to run anything though, need data for that.

#### [Pose-Format](https://github.com/sign-language-processing/pose)

* A whole pip-installable SLP library for dealing with poses, making it easier to deal with.

#### [MediaPipe Holistic Hand Landmarker](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)

Many papers use/cite this one. But it seems it's now a "legacy" item, no longer supported.

### Needs more effort to get running

#### [SLTUnet](https://github.com/bzhangGo/sltunet)

* Recent, cool-looking paper.
* [My attempt to get SLTUNet running in Colab](https://colab.research.google.com/drive/1OxBJkQI4eccChdEjeNyjHMPHyCghQaMd?usp=sharing)
* Alas, I wasn't able to install requirements in Colab. Requires an older Tensorflow version that would be difficult to install.

#### [Tackling Low-Resourced Sign Language Translation: UPC at WMT-SLT 22](https://github.com/mt-upc/fairseq/tree/wmt-slt22). One of the submissions to the WMT SLT '22 contest

* See also [the paper](https://arxiv.org/abs/2212.01140).
* [My attempt to install requirements](https://colab.research.google.com/drive/1wPdHzSTlgvJzTm0Tg_Z28FnYEH459-tr?usp=sharing) worked, but I got stuck on how to run things with "Task"

### Not yet fully assessed

Codebases/repos I have not yet assessed for viability.

#### [Isolated Sign Recognition from RGB Video using Pose Flow and Self-Attention](https://ieeexplore.ieee.org/document/9523108)

* [repo](https://github.com/m-decoster/ChaLearn-2021-LAP)

#### [Temporal Lift Pooling for Continuous Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-3-031-19833-5_30)

* [repo](https://github.com/hulianyuyy/Temporal-Lift-Pooling)

#### [Spatial-Temporal Graph Convolutional Networks for Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-3-030-30493-5_59)

* [repo](https://github.com/yysijie/st-gcn)

Other links from the paper:

* <http://csr.bu.edu/asl/asllvd/annotate/index.html>
* <http://www.cin.ufpe.br/~cca5/asllvd-skeleton>.
* <http://www.cin.ufpe.br/~cca5/st-gcn-sl>.
* <http://www.cin.ufpe.br/~cca5/asllvd-skeleton-20>.

#### [Read and Attend: Temporal Localisation in Sign Language Videos](https://arxiv.org/abs/2103.16481)

* [project website with weights](https://www.robots.ox.ac.uk/~vgg/research/bslattend/)
* [code from ECCV 20](https://github.com/gulvarol/bsl1k)
* The code/models from this one are used in other papers, including [Microsoft's submission to WMT-SLT 22](https://www.microsoft.com/en-us/research/publication/clean-text-and-full-body-transformer-microsofts-submission-to-the-wmt22-shared-task-on-sign-language-translation/)

#### [Better Sign Language Translation with STMC-Transformer](https://aclanthology.org/2020.coling-main.525/)

* [My attempt in Colab](https://colab.research.google.com/drive/1GG9GBpBIid2aiEj52cP0A1bnPRJM96HD?usp=sharing)
* Initial "can we install the requirements" is pretty encouraging. Installs ran without error.
* Docs seem well done and readable.
* Trying to run train.py, we get an ImportError, which I have not had a chance to debug.

#### [Sign Language Transformers (CVPR'20)](https://www.cihancamgoz.com/pub/camgoz2020cvpr.pdf)

* [code is here](https://github.com/neccam/slt)
* [My attempt to install requirements](https://colab.research.google.com/drive/17r9S9OOMapeKXwBTYkGpGbAqBrmgncDH?usp=sharing)
* Very brief attempt to get running on Colab runs into highly specific version-pinning error. Will probably need [some tricks](#tricks-for-getting-old-code-working).  Also it's from 4 years ago.

### Seems out of date

Resources that might be useful but seem to be out of date, at least enough that they'd be quite hard to use as a starting point.

#### [OpenHands (possibly a dead project?)](https://openhands.ai4bharat.org/en/latest/instructions/datasets.html)

Here's what they say...
> 👐OpenHands is an open source toolkit to democratize sign language research by making pose-based Sign Language Recognition (SLR) more accessible to everyone.

* [Here's their paper](https://arxiv.org/abs/2110.05877)
* it looks neat and ambitious ...But I see the documentation has not been updated in years. Likely dead.

### Tricks for getting old code working

Some useful tricks I've learned

* **Don't use Python 3.12!** If you try pip installing and it won't work, often it will work if you go back to older, more well-tested versions. Often libraries just cannot be installed on 3.12. Not an issue on Colab because it uses 3.10 by default, but if you are on a workstation and using conda, it'll try to install 3.12 by default at time of writing .
* pipreqs library can analyze code and generate a requirements.txt file
* deptry library can analyze a repo and requirements.txt for problems like unused imports, imports that aren't listed in the requirements, etc.
* pur will try to auto-update requirements.txt files to newer versions.
* condacolab lets you use conda commands in Google Colab, though not create new envs. This is really nice if a project uses environment.yml instead of requirements.txt

## Good places to look for more

### research.sign.mt - best place for SLT background

<https://research.sign.mt/> This is your best starting point for getting smart on SLT. It is a lovely open-source compendium of Sign Language Processing papers, data, and other resources.

### sign.mt and sign-language-processing github orgs

* <https://github.com/sign>
* <https://github.com/sign-language-processing>
These are very rich sources of relevant repos and links.

### 👋 Sign Translate Wiki

Still a work in progress, but links to quite a number of interesting repos, including a whole pipeline's worth that could use contributions. Nice jumping-on points for an aspiring researcher!

* [Signed to Spoken Languages](https://github.com/sign/translate/wiki/Signed-to-Spoken) Here is the pipeline, which links to all the sub-repos that need work.
* If you're looking for video-to-pose it seems to be under [the Pose-Format library now](https://github.com/sign-language-processing/pose)

### WMT Shared Tasks

The Workshop on Machine Translation had two Shared Tasks on Sign Language Translation. The "findings" papers from each of them is quite informative.

#### WMT-SLT22

2022 contest. A number of submissions and approaches detailed.

* Code: The 2022 Shared Task has a number of "tools" links at [this page](https://sites.google.com/view/wmt-slt-v2022/tools).

[Findings](https://aclanthology.org/2022.wmt-1.71/)

#### WMT-SLT23

Code: The 2023 Shared Task has a ["Tools" page linking to several repos for baseline systems](https://www.wmt-slt.com/tools)

[Findings](https://aclanthology.org/2023.wmt-1.4/)

### PapersWithCode

<https://paperswithcode.com/task/sign-language-translation> lists a slew of papers, but it's incomplete. Notably it seems to be missing anything on the DGS Corpus.

## Survey of Sign Language Processing Surveys

### SLP in general

#### [Sign Language Recognition, Generation, and Translation An Interdisciplinary Perspective (2019)](https://www.semanticscholar.org/paper/Sign-Language-Recognition%2C-Generation%2C-and-An-Bragg-Koller/c7fd5973a2cb3f7c6a5df0120400bc47b2f2bacf)

Quite an influential one, based on citation count maybe a "seminal work".

#### [Systemic Biases in Sign Language AI Research A Deaf-Led Call to Reevaluate Research Agendas (2024)](https://arxiv.org/abs/2403.02563)

Recent survey, looks at like 100+, including modeling decisions, and all of them are themselves Deaf. Interesting.

Things I learned:

* Basically the communication barriers may not be what one might think, lots of oversimplification/overstating going on
  * "deaf people use sign language" is oversimplified, it's way more complicated. Many times it's deliberately not given as an option/suppressed, and people often use a mix, and other complexities.
  * Lots of people know both kinds
  * Maybe people don't need the technosolutions, they've got their own strategies already, don't overinflate the importance of the tech.
* Dataset misalignment with, llike, real life
  * Interpreters only for example. They might not sign the same way.
  * Lots of details on who and how are glossed over in dataset creation
  * IRL, the "born deaf, only communicate with other deaf people" situation is super rare,  often you get "born to hearing families, signing specifically discouraged, learned it way later"
  * IRL there's so much variation, but datasets for ML simplify. Clean Data/annotations in conflict with IRL conditions
* labels lacking linguistic foundation
  * glosses without thinking about it
  * Present (gift) or Present (time) given the same gloss in one, different in another
  * glosses are not a translation!
  * What glossing system was even used?
  * Subtitles often have quality issues
  * It's NLP! Not just Computer Vision!
* Modeling decisions
  * various issues with pose estimation
  * various issues with pretraining on non-sign datasets SLP Open Problems and Questions
  * "Natural language-assisted sign language recognition" for example has issues which cascade from previous decisions, e.g. distinct glosses for the same sign

### Specifically SLT

#### [Machine Translation from Signed to Spoken Languages State of the Art and Challenges (2022)](https://arxiv.org/abs/2202.03086)

[Findings of the Second WMT Shared Task on Sign Language Translation (WMT-SLT23)](https://aclanthology.org/2023.wmt-1.4/) calls this a "comprehensive review" on the topic, so therefore it seems worth checking out.

This one influenced me quite a bit. For example to leave out papers with glove-based methods as impractical.

#### [A survey on Sign Language machine translation (2023)](https://www.sciencedirect.com/science/article/pii/S0957417422020115)

Has a nice section on CSLR with a description of architectures. Those papers are worth looking at me thinks. Interesting words: 3D-ResNet, iterative training (related to weak labelling/supervision), Temporal Convolution Pyramid (TCP).

Has a nice section talking about different SLT metrics. Mostly the same ones as NMT, e.g. BLEU. SLR has a few, e.g. sign error rate, gloss error rate. No mention of measuring sign language generation quality.

#### [Sign Language Translation A Survey of Approaches and Techniques (2023)](https://www.mdpi.com/2079-9292/12/12/2678)

This one could use some copyediting. I did appreciate their dive into visual feature extractors.

#### [Sign Language Processing (research.sign.mt)](https://research.sign.mt/)

Quite current, [open source.](https://github.com/sign-language-processing/sign-language-processing.github.io) Mentioned above under research.sign.mt

### Other SLP tasks

#### [Unraveling a Decade A Comprehensive Survey on Isolated Sign Language Recognition (2023)](https://ieeexplore.ieee.org/document/10350553)

If you wanted to do image2gloss, which I don't, this could be a good one... or so I thought. Actually contains quite a bit about video. Talks about input modalities, fusion methods, datasets, etc.

Interestingly it also does link a number of datasets with videos in them which might be relevant. This would be good to chase down.

TODO: have a look into these datasets, make sure they're included

* J. Wan, Y. Zhao, S. Zhou, I. Guyon, S. Escalera and S. Z. Li, "ChaLearn looking at people RGB-D isolated and continuous datasets for gesture recognition", CVPRW, pp. 56-64, 2016.
* Sergio Escalera, Xavier Baró, Jordi Gonzalez, Miguel A Bautista, Meysam Madadi, Miguel Reyes, et al., "Chalearn looking at people challenge 2014: Dataset and results", ECCVW, pp. 459-473, 2015.
* Hamid Reza Vaezi Joze and Oscar Koller, "MS-ASL: A large-scale data set and benchmark for understanding American sign language", 2018.
* O. M. Sincan and H. Y. Keles, "AUTSL: A large scale multi-modal Turkish sign language dataset and baseline methods", IEEE Access, vol. 8, pp. 181340-181355, 2020.
* Laura Docío-Fernández, José Luis Alba-Castro, Soledad Torres-Guijarro, Eduardo Rodríguez-Banga, Manuel Rey-Area, Ania Pérez-Pérez, et al., "LSE UVIGO: A multi-source database for Spanish sign language recognition", Proceedings of the LREC2020 9th Workshop on the Representation and Processing of Sign Languages: Sign Language Resources in the Service of the Language Community Technological Challenges and Application Perspectives, pp. 45-52, 2020.
* Oğulcan Özdemir, Ahmet Alp Kındıroğlu, Necati Cihan Camgöz and Lale Akarun, "BosphorusSign22k sign language recognition dataset", 2020.
* Gokul NC, Manideep Ladi, Sumit Negi, Prem Selvaraj, Pratyush Kumar and Mitesh Khapra, "Addressing resource scarcity across sign languages with multilingual pretraining and unified-vocabulary datasets", NIPS, vol. 35, pp. 36202-36215, 2022.

It seems there's actually a good amount of quality links in here at the very least. A good amount of this ISLR is video-based, and therefore is potentially useful for video-based sign2text, sign2gloss.

#### [A Extensive Survey on Sign Language Recognition Methods (2023)](https://ieeexplore.ieee.org/document/10084111)

Cites 51 references, but doesn't provide a lot of useful/practical info. Of the 51 many are quite dated, and only some are from the last few years. Also a number are based on something like a sensor or wearable device. Nevertheless I did find 10 whose titles seemed interesting in some way for video understanding of Sign Languages.

1. [SF-Net: Structured Feature Network for Continuous Sign Language Recognition](https://arxiv.org/abs/1908.01341) (2019) Continuous SLR and the abstract looks interesting. Skimming through I see a number of details about feature extraction methods. I don't think they release their code.
2. [Spatial-Temporal Graph Convolutional Networks for Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-3-030-30493-5_59) (2019) They provide source code and data for continuous SLR, and mention an interesting method as well.
3. [Multiple Proposals for Continuous Arabic Sign Language Recognition](https://link.springer.com/article/10.1007/s11220-019-0225-3) (2019) they do in fact process successive frames of video. They use pixel differences. No code I think.
4. [Sign Language Recognition from Digital Videos Using Deep Learning Methods](https://link.springer.com/chapter/10.1007/978-3-030-72073-5_9) (2021) Video, and capsule networks is at least interesting and diferent.
5. [An Integrated CNN-LSTM Model for Bangla Lexical Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-981-33-4673-4_57) (2020) CNN-LSTM implies video. It seems quite basic but I guess it's in scope.
6. [Temporal Lift Pooling for Continuous Sign Language Recognition](https://link.springer.com/chapter/10.1007/978-3-031-19833-5_30) (2022) code and continuous SLR, so it's relevant. [repo](https://github.com/hulianyuyy/Temporal-Lift-Pooling)
7. [One Model is Not Enough: Ensembles for Isolated Sign Language Recognition](https://www.mdpi.com/1424-8220/22/13/5043) (2022) Based on "We analyze two appearance-based approaches, I3D and TimeSformer, and one pose-based approach, SPOTER." it seems they're processing videos, so it's relevant!
8. [Two-Stream Mixed Convolutional Neural Network for American Sign Language Recognition](https://www.mdpi.com/1424-8220/22/16/5959) from 2022. It barely squeaks into scope by the fact they use two consecutive frames, which is interesting enough to include.
9. [Improved 3D-ResNet sign language recognition algorithm with enhanced hand features](https://www.nature.com/articles/s41598-022-21636-z) Processes video inputs, and it's in _Nature_. Skimming through I also see a number of diagrams and graphs that look interesting.
10. [Hand Landmark-Based Sign Language Recognition Using Deep Learning](https://link.springer.com/chapter/10.1007/978-981-16-7996-4_11#Sec5) Alas, it's based on static images, so not within my scope of interest.

Some odd stylistic/writing choices. "it must be suitable to fete and record homemade and non-manual movements". "Result Obtainment Using Continuous Sign Language Representation Data Sets"

They claim that "Review publications on hand gestures and SLR are numerous." but that in contrast their paper is very in-depth. Yet I feel it was pretty surface-level? Didn't seem to cover that many, and not in very much detail.

Table 1 has a list of: Approach, year, language. But then "accuracy"? On which datasets? How do you compare directly like that? Seems strange, not sure I understand.

Some of the terminology, e.g. "hand gestures" makes me think the paper might benefit from running it by some Deaf researchers.

In general this review is... not my favorite. I kind of dislike it when folks feel the need to use lots of big words to tell us how their paper is "extensive" and great and amazing. I didn't learn a lot of useful or state-of-the-art info from it, alas. But I got some interesting references, so that's nice.

#### [Sign Language Production: A Review (2021)](https://openaccess.thecvf.com/content/CVPR2021W/ChaLearn/html/Rastgoo_Sign_Language_Production_A_Review_CVPRW_2021_paper.html)

Potentially interesting, I'm just not focusing on text2sign at the moment.

#### [Sign Language Recognition: Current State of Knowledge and Future Directions (2023)](https://ieeexplore.ieee.org/document/10307879)

* Behind an IEEE paywall, but accessible through academic institutions.
* tl;dr CNNs are now the SOTA for static/image-based, as expected.

### Miscellaneous

#### [Comparative Analysis of Multiple ML Models and Real-Time Translation (2023)](https://ieeexplore.ieee.org/document/10346894)

Not my favorite. Their terminology is not always sensitive and kind. Also the writing quality is not amazing. And in the end the thing they actually do is pretty basic, not actually a survey paper but a few entry-level experiments on “alphabets”.

#### [Indian Sign Language Recognition Using Classical And Machine Learning Techniques A Review (2023)](https://ieeexplore.ieee.org/document/10136059)

Goes over various approaches to Indian Sign Language. We care about the vision-based ones. But then they're explaining what... Anaconda is...? And Precision/Recall are? This is very basic stuff. And they describe a system of their own but it seems to be just standard off-the-shelf items? I'm not sure this is worth including.

#### [Quantitative Survey of the State of the Art in Sign Language Recognition (2020)](https://arxiv.org/abs/2008.09918)

300 papers, 400 experimental results... very extensive work, and by one person! Also they basically collect everything on RWTH-PHOENIX-Weather at the time. They also have the line "Surprisingly, RWTH-PHOENIX-Weather with a vocabulary of 1080 signs represents the only resource for large vocabulary continuous sign language recognition benchmarking world wide." I wonder if that's still true 4 years later?
