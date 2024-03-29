---
layout: post
title: The shower problem
description: How do you most quickly get hot water from an unlabeled shower knob?
image: /assets/2020_shower_problem/shower_knob.png
---

_Attention mathematicians and computer scientists:_ I've got a problem for you, and I don’t know the solution.

Here’s the setup: You're at your friend's place and you need to take a shower. The shower knob is unlabeled. One direction is hot and the other direction is cold, and you don’t know which is which.

<div class="wrapper">
  <img src='/assets/2020_shower_problem/shower_knob.png' class="inner" style="position:relative border: #222 2px solid; max-width:40%;" >
</div>

You turn it to the left. It's cold. You wait. 

At what point do you switch over to the right?

### The baseline shower problem
Let’s make this more explicit. 

- Your goal is to find a policy that minimizes the expected amount of time it takes to get hot water flowing out of the shower head. To simplify things, assume that the water coming out of the head is either hot or cold, and that the lukewarm transition time is effectively zero. 
- You know that the shower has a Time-To-Hot constant called $$\tau$$. This value is defined as the time it takes for hot water to arrive, assuming you have turned the knob to the hot direction and keep it there.
- The $$\tau$$ constant is a fixed property of the shower and is sampled once from a known distribution. You have certain knowledge of the distribution, but you don’t know $$\tau$$.
- The shower is memoryless, such that every time you turn the knob to the hot direction, it will take $$\tau$$ seconds until the hot water arrives, regardless of your prior actions. Every time you turn it to the cold direction, only cold water will come out.

<img src='/assets/2020_shower_problem/distributions.gif'>
<div class="caption">
**Figure 1.** Unbeknownst to the user, the hot water direction is to the left, with a time constant τ of 100 seconds. The user knows the probability distribution over τ, and follows a strategy of eliminating segments of that distribution. They initially guess the correct direction, but give up too soon at 75 seconds. They then spend another 75 seconds on the rightwards direction. Finally, they return to the left direction, passing through the 75 seconds they had already eliminated, and then finally getting the hot water after 100 seconds on the left direction. In all, it takes the user 250 seconds to find the hot water.
</div>

I don’t know how to solve this problem. But as a starting point I realize it’s possible to keep track of the probability that the hot direction is to the left or to the right. In the animation above, the probability that the hot direction is to the right is just the unexplored white area under the right curve, divided by the total unexplored white area of both curves.

But how do you turn that into a policy for exploring the space? Does anybody know?

### Submissions
If you would like to submit a proposal, please report your average duration for the sample of 20,000 $$ \tau $$'s provided [here](https://gist.github.com/csaid/a57c4ebaa1c7b0671cdc9692638ea4c4). 

On February 9, 2020, [Cameron Davidson-Pilon](https://twitter.com/Cmrn_DP) took an early lead, with an average duration of [111.4 seconds](https://gist.github.com/CamDavidsonPilon/be1333d348865fbf1ab13c409e849ee2).

On June 22, 2021, [Allen Downey](https://twitter.com/AllenDowney) took the lead with an average time of [102 seconds](https://github.com/AllenDowney/ThinkBayes2/blob/master/examples/shower.ipynb), pending verification.

On [July 11, 2021](https://twitter.com/CookieSci/status/1414331761400717314), [Jake Westfall](https://twitter.com/CookieSci) took the lead with an average time of [101.5 seconds](https://colab.research.google.com/drive/1SL664cbudJnjV0Qo8PmP0klng9vhVopr?usp=sharing).

### Bonus problem: Plumbing realities and the elusive “Middle Solution”
The baseline shower problem assumes a simplified version of reality, where the shower is memoryless and there is only a single pipe. If you want a harder problem, I have written a comment below that describes some of the plumbing realities, including lag and the existence of separate hot and cold pipes. The comment explores the tantalizing possibility that we’ve all been fiddling with our showers wrong this whole time. Instead of swinging the knob between one extreme and the other, what if the optimal solution is to start by putting the knob in the middle? To read more, see the comment below.