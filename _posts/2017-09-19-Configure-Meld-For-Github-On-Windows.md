---
published: true
---

## Github Meld Windows
I recently made the move to windows machine being my primary development enviornment. The first biggest difference, that you realize is regarding the toolset that is available on the platform and second is the toolset available to get/install the tools that one needs. I missed yum/dnf/apt-get like how I could never imagine. 

I know about the in built version of linux that I can use on top of windows now. For some reason, I am unable to update my windows. 

Anyways, After having most of the tools set up, I starting doing development and ran into a merge conflict while doing some updates. And thus, i missed the toolset again. I have always used and loved [Meld](http://meldmerge.org/) as my primary merge/diff tool. Thus, a couple of commands needed to set this up with git are as follows:


    git config --global merge.tool meld
    git config --global diff.tool meld
    git config --global mergetool.meld.path “C:\Program Files (x86)\Meld\meld\meld.exe”

You may need to replace the meld location to be your own custom installation location if you modified the default installation location.
