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



# A Coder's Guide to SLT
Truth be told, I am writing this to be the guide I wish I had - the guide that a coder needs, in order to Do Science within the realm of Sign Language to Text.

- [A Coder's Guide to SLT](#a-coders-guide-to-slt)
  - [Codebases](#codebases)
    - [Resources that seem replicable/usable](#resources-that-seem-replicableusable)
    - [Needs more effort to get running.](#needs-more-effort-to-get-running)
    - [Not yet fully assessed.](#not-yet-fully-assessed)
    - [Out of date?](#out-of-date)
    - [Tricks for getting old code working](#tricks-for-getting-old-code-working)
  - [Good places to look](#good-places-to-look)
    - [research.sign.mt - best place for SLT background](#researchsignmt---best-place-for-slt-background)
    - [sign.mt and sign-language-processing github orgs](#signmt-and-sign-language-processing-github-orgs)
    - [👋 Sign Translate Wiki](#-sign-translate-wiki)
    - [WMT Shared Tasks](#wmt-shared-tasks)
      - [WMT-SLT22](#wmt-slt22)
      - [WMT-SLT23](#wmt-slt23)
    - [PapersWithCode](#paperswithcode)


## Codebases
I personally like having some code to work with, instead of having to code things from scratch. Of course, Sign Language Processing is a developing field. Much of what exists out there is research code. So getting it running isn't always easy. I went ahead and took a crack at some of them, results below. Mainly what I was looking for was:
1. How recently maintained/updated is this? If it's been a long time, it will be harder to get running. 
2. The ImportError test: How hard is it to just install the requirements and run? Can I boot up a Colab Instance and get the scripts running without ImportErrors?



### Resources that seem replicable/usable
[Sign Language Datasets](https://github.com/sign-language-processing/datasets)
- This is a very helpful repo/library. Hard to get anything done without data! And this library gives you many datasets to choose from, and a standardized code interface to do it with! 
- Actively maintained and updated, often responses/updates come within the same day! 
- And they have a nice Colab Notebook linked from the README, always nice to see. Which runs quite nicely.

[Two-Stream Network for Sign Language Recognition and Translation, NeurIPS2022](https://github.com/FangyunWei/SLRT/tree/main/TwoStreamNetwork)
- One of the top papers on PapersWithCode for sign language translation. 
- [My attempt to get this running](https://colab.research.google.com/drive/1Z1P__o3NTl0zAStigUSlbeqxOVAaqOKT) Successfully installed requirements in Colab. This one looks pretty cool. 

[KnowComp submission for WMT SLT]()
- [My attempt to get this running in Colab](https://colab.research.google.com/drive/1le0qsaMwCH0hE6IkuVK1ImRKjsBBMHy2?usp=sharing)
I was able to get the requirements installed! Just don't have the needed data yet. 


[WMT SLT 2023 Sockeye Baseline](https://github.com/bricksdont/sign-sockeye-baselines)
- [My attempt to get it running in Colab](https://colab.research.google.com/drive/1P2KWBOktCakuAfJPxFoBlpk1KXOGS6JP?usp=sharing)
Ran into some trouble, haven't yet found a way to get them all installed. 

[WMT-SLT23 I3D baseline, aka "Sign Language Translation from Instructional Videos"](https://ieeexplore.ieee.org/document/10208355)
- [repo](https://github.com/imatge-upc/slt_how2sign_wicv2023)
- [My attempt to install the requirements for the I3D baseline in Colab](https://colab.research.google.com/drive/1GKHID0WMbXwnWXHD1iXQfs95nhWz9NQt?usp=sharing)
... got them installed! Haven't yet managed to run anything though, need data for that. 


### Needs more effort to get running. 
[SLTUnet](https://github.com/bzhangGo/sltunet)
- Recent, cool-looking paper.
- [My attempt to get SLTUNet running in Colab](https://colab.research.google.com/drive/1OxBJkQI4eccChdEjeNyjHMPHyCghQaMd?usp=sharing)
- Alas, I wasn't able to install requirements in Colab. Requires an older Tensorflow version that would be difficult to install.


[Tackling Low-Resourced Sign Language Translation: UPC at WMT-SLT 22](https://github.com/mt-upc/fairseq/tree/wmt-slt22). One of the submissions to the WMT SLT '22 contest. 
- See also [the paper](https://arxiv.org/abs/2212.01140). 
- [My attempt to install requirements](https://colab.research.google.com/drive/1wPdHzSTlgvJzTm0Tg_Z28FnYEH459-tr?usp=sharing) worked, but I got stuck on how to run things with "Task" 

### Not yet fully assessed. 
Others can go here

### Out of date?
Resources that might be useful but seem to be out of date. 


[OpenHands (possibly a dead project?)](https://openhands.ai4bharat.org/en/latest/instructions/datasets.html)
Here's what they say...
> 👐OpenHands is an open source toolkit to democratize sign language research by making pose-based Sign Language Recognition (SLR) more accessible to everyone.

- [here's their paper](https://arxiv.org/abs/2110.05877)
- it looks neat and ambitious ...But I see the documentation has not been updated in years. Likely dead. 

### Tricks for getting old code working

Some useful tricks I've learned
* **Don't use Python 3.12!** If you try pip installing and it won't work, often it will work if you go back to older, more well-tested versions. Often libraries just cannot be installed on 3.12. Not an issue on Colab because it uses 3.10 by default, but if you are on a workstation and using conda, it'll try to install 3.12 by default at time of writing .
* pipreqs library can analyze code and generate a requirements.txt file
* deptry library can analyze a repo and requirements.txt for problems like unused imports, imports that aren't listed in the requirements, etc.
* pur will try to auto-update requirements.txt files to newer versions.
* condacolab lets you use conda commands in Google Colab, though not create new envs. This is really nice if a project uses environment.yml instead of requirements.txt

## Good places to look 

### research.sign.mt - best place for SLT background
https://research.sign.mt/ This is your best starting point for getting smart on SLT. It is a lovely open-source compendium of Sign Language Processing papers, data, and other resources. 

### sign.mt and sign-language-processing github orgs
* https://github.com/sign 
* https://github.com/sign-language-processing
These are very rich sources of relevant repos and links. Here are a few 


### 👋 Sign Translate Wiki
Still a work in progress, but links to quite a number of interesting repos, including a whole pipeline's worth that could use contributions. Nice jumping-on points for an aspiring researcher! 

- [Signed to Spoken Languages](https://github.com/sign/translate/wiki/Signed-to-Spoken) Here is the pipeline, which links to all the sub-repos that need work. 
- If you're looking for video-to-pose it seems to be under [the Pose-Format library now](https://github.com/sign-language-processing/pose)



### WMT Shared Tasks
The Workshop on Machine Translation had two Shared Tasks on Sign Language Translation. The "findings" papers from each of them is quite informative. 

#### WMT-SLT22
2022 contest. A number of submissions and approaches detailed. 
- Code: The 2022 Shared Task has a number of "tools" links at [this page](https://sites.google.com/view/wmt-slt-v2022/tools). 

[Findings](https://aclanthology.org/2022.wmt-1.71/)


#### WMT-SLT23
Code: The 2023 Shared Task has a ["Tools" page linking to several repos for baseline systems](https://www.wmt-slt.com/tools)

[Findings](https://aclanthology.org/2023.wmt-1.4/)


 

### PapersWithCode

https://paperswithcode.com/task/sign-language-translation lists a slew of papers, but it's incomplete. Notably it seems to be missing anything on the DGS Corpus. 




