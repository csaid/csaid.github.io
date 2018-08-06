---
layout: post
title: Using your ears and head to escape the Cone Of Confusion
description: How the shape of your ears and head help you localize sound
image: /assets/2018_cone_of_confusion/cone_and_hrtf.png
---


One of coolest things I ever learned about sensory physiology is how the auditory system is able to locate sounds. To determine whether sound is coming from the right or left, the brain uses inter-ear differences in amplitude and timing. As shown in the figure below, if the sound is louder in the right ear compared to the left ear, it's probably coming from the right side. The smaller that difference is, the closer the sound is to the midline (i.e the vertical plane going from your front to your back). Similarly, if the sound arrives at your right ear before the left ear, it's probably coming from the right. The smaller the timing difference, the closer it is to the midline. There's a [fascinating](https://en.wikipedia.org/wiki/Coincidence_detection_in_neurobiology#Sound_localization) [literature](https://nba.uth.tmc.edu/homepage/cnjclub/2004spring/McAlpineGrothe2003.pdf) on the neural mechanisms behind this.
<div class="wrapper">
  <img src='/assets/2018_cone_of_confusion/right_and_front_right.png' class="inner" style="position:relative border: #222 2px solid; max-width:50%;" >
</div> 
Inter-ear loudness and timing differences are pretty useful, but unfortunately they still leave a lot of ambiguity. For example, a sound from your front right will have the exact same loudness differences and timing differences as a sound from your back right.
<div class="wrapper">
  <img src='/assets/2018_cone_of_confusion/front_right_and_back_right.png' class="inner" style="position:relative border: #222 2px solid; max-width:50%;" >
</div> 
Not only does this system leave ambiguities between front and back, it also leaves ambiguities between top and down. In fact, there is an entire _cone of confusion_ that cannot be disambiguated by this system. Sound from all points along the surface of the cone will have the same inter-ear loudness differences and timing differences.
<div class="wrapper">
  <img src='/assets/2018_cone_of_confusion/cone_of_confusion.png' class="inner" style="position:relative border: #222 2px solid; max-width:50%;" >
</div> 
While this system leaves a cone of confusion, humans are still able to determine the location of sounds from different points on the cone, at least to some extent. How are we able to do this?

Amazingly, we are able to do this because of the shape of our ears and heads. When sound passes through our ears and head, certain frequencies are attenuated more than others. Critically, the attenuation pattern is highly dependent on sound direction.

This location-dependent attenuation pattern is called a [Head-related transfer function](https://en.wikipedia.org/wiki/Head-related_transfer_function) (HRTF) and in theory this could be used to disambiguate locations along the cone of confusion. An example of someone's HRTF is shown below, with frequency on the horizontal axis and polar angle on the vertical axis. Hotter colors represent less attenuation (i.e. more power). If your head and ears gave you this HRTF, you might decide a sound is coming from the front if it has more high frequency power than you'd expect.
<div class="wrapper">
  <img src='/assets/2018_cone_of_confusion/cone_and_hrtf.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption">
    HRTF image from Simon Carlile's <a href = 'https://sonification.de/handbook/download/TheSonificationHandbook-chapter3.pdf'>Psychoacoustics chapter</a> in The Sonification Handbook.
  </div>
</div> <br>
This system sounds good in theory, but do we actually use these cues in practice? In 1988, Frederic Wightman and Doris Kistler performed an ingenious set of experiments ([1](http://public.vrac.iastate.edu/~charding/audio/Headphone%20simulation%20of%20free-field%20listening.%20I-%20Stimulus%20synthesis%20-%20J%20Acoust%20Soc%20Am%201989%20-%20Wightman.pdf), [2](http://public.vrac.iastate.edu/~charding/audio/Headphone%20simulation%20of%20free-field%20listening.%20II-%20Psychophysical%20validation%20-%20J%20Acoust%20Soc%20Am%201989%20-%20Wightman.pdf)) to show that that people really do use HRTFs to infer location. First, they measured the HRTF of each participant by putting a small microphone in their ears and playing sounds from different locations. Next they created a digital filter for each location and each participant. These filters replicated the pattern of filtering from each location. Finally, they placed headphones on the listeners and played sounds to them, each time passing the sound through one of the digital filters. Amazingly, participants were able to correctly guess the "location" of the sound, depending on which filter was used, even though the sound was coming from headphones. They were also much better at sound localization when using their own HRTF, rather than someone else's HRTF.

Further evidence for this hypothesis comes from [Hofman et al., 1998](https://doi.org/10.1038/1633), who showed that by using putty to reshape people's ears, they were able to change the HRTFs and thus disrupt sound localization. Interestingly, people were able to quickly relearn how to localize sound with their new HRTFs. 
<div class="wrapper">
  <img src='/assets/2018_cone_of_confusion/putty.png' class="inner" style="position:relative border: #222 2px solid; max-width:50%;" >
  <div class="caption">
    Image from <a href = 'https://doi.org/10.1038/1633'>Hofman et al., 1998</a>.
  </div>
</div><br>

A final fun fact: to improve the sound localization of humanoid robots, researchers in Japan attached [artificial ears to the robot heads](https://doi.org/10.1007/s10489-014-0544-y) and implemented some sophisticated algorithms to infer sound location. Here are some pictures of the robots.
<div class="wrapper">
  <img src='/assets/2018_cone_of_confusion/robots.png' class="inner" style="position:relative border: #222 2px solid; max-width:50%;" >
</div> 
Their paper is kind of ridiculous and has some questionable justifications for not just using microphones in multiple locations, but I thought it was fun to see these principles being applied.

