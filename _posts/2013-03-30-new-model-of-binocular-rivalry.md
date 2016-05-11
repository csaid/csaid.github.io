---
author: csaid81
comments: true
date: 2013-03-30 15:23:03+00:00
layout: post
slug: new-model-of-binocular-rivalry
title: New model of binocular rivalry
wordpress_id: 175
---

Binocular rivalry is a visual illusion that occurs when the two eyes are presented with incompatible images. Instead of perceiving a mixture of the two images, most people experience alternations in which only one image is visible at a time. Binocular rivalry works best under controlled laboratory conditions with prisms or mirrors, but if you are lucky you might be able to experience it in the figure below. Try crossing your eyes to align the left boxes and right boxes, so that three boxes are observed rather than two. If you can keep your eyes stable, you might perceive alternations between the two different gratings in the middle box. It helps if you first try to merge the "Merge me!" phrase and then, once that it is stable, focus on the middle box. If you can't stabilize your eyes enough, don't worry. You are not alone.

.

{% include image.html url="/assets/fig_dichop1.png" %}

.

Binocular rivalry is more than just an interesting illusion: it reflects actual inhibitory competition between neurons in the brain, and therefore provides a rare window into neural dynamics. To help us understand these mechanisms, researchers have developed several models of the phenomenon. Yet surprisingly, all of these models make a big incorrect prediction about a type of stimulus known as “binocular plaids”. You can view some binocular plaids by crossing your eyes on the boxes below, or simply by looking at one of the boxes normally.

.

{% include image.html url="/assets/fig_binoc.png" %}


.

As you can see, a plaid is composed of two gratings, a rightward pointing grating and a leftward pointing grating. The big, incorrect prediction made by previous models of rivalry is that the leftward pointing grating should alternate with the rightward pointing grating, just as it would in the traditional rivalry stimuli shown above. This prediction -- which follows because the same neural inhibition that creates competition in the first figure must necessarily also create competition in the second figure -- is clearly wrong: When viewing the binocular plaid, you probably perceive that the rightward grating remains just as strong as the leftward grating, without any alternations. This failed prediction extends far beyond these toy stimuli. Plaid perception is typically explained by the broad theory of [divisive normalization](http://www.nature.com/nrn/journal/v13/n1/full/nrn3136.html), which also covers a whole host of other inhibitory interactions in cortex. Models of the inhibitory processes in rivalry are thus in tension with models of inhibitory processes that use divisive normalization.

Together with my advisor [David Heeger](http://www.cns.nyu.edu/~david/), I developed a [new model](http://www.ploscompbiol.org/article/info%3Adoi%2F10.1371%2Fjournal.pcbi.1002991) of rivalry that is able to accommodate plaids, and which I hope reconciles models of rivalry with models of normalization. Finding a solution was not as easy as you might think. When we presented the problem to colleagues, everyone immediately had intuitions for how to solve it, but amazingly none of them worked. We found only one solution that worked, and it is one that I later discovered was once proposed by Randolph Blake. The model makes novel predictions that we confirmed with psychophysical tests. If you want to read more about it, you can find the paper below. The Matlab code is available [here](http://www.cns.nyu.edu/~csaid/code/SaidAndHeeger_model_code.zip).

[Said CP & Heeger DJ (2013). A model of binocular rivalry and cross-orientation suppression. _PLOS Computational Biology._](http://www.ploscompbiol.org/article/info%3Adoi%2F10.1371%2Fjournal.pcbi.1002991)
