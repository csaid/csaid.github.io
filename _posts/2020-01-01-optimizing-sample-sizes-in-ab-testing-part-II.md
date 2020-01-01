---
layout: post
title: Optimizing sample sizes in A/B testing, Part II&#58; Aggregate time-discounted lift
description: A surprisingly simple formula for optimizing your experiment
image: /assets/2020_optimizing_sample_sizes/product_matrix.png
---

This is Part II of a two-part blog post on how to optimize your sample size in A/B testing. As in [Part I](/2020/01/01/optimizing-sample-sizes-in-ab-testing-part-I/), the focus will be on choosing a sample size at the beginning of the experiment and committing to it, not on dynamically updating the sample size as the experiment proceeds.

In [Part I](/2020/01/01/optimizing-sample-sizes-in-ab-testing-part-I/), we learned how before the experiment starts we can estimate $\hat{L}$, the expected post-experiment lift, a probability weighted average of outcomes. 

In Part II, we'll discuss how to estimate what is perhaps the most important per-unit cost of epxerimentation: the forfeited benefits that could come from shipping the winning bucket earlier. This leads to something I think is incredibly cool: A formula for $\hat{L}_a$, the aggregate time-discounted post-experiment lift, as a function of sample size. The formula allows you to pick optimal sample sizes specific to your business circumstances, and leads us to three lessons for practioners.

1. You should run "underpowered" experiments if you have a very high discount rate.
2. You should run "underpowered" experiments if you have a small user base.
3. That said, it's far better to run your experiment too long than too short.

## A quick modification from Part I

In [Part I](/2020/01/01/optimizing-sample-sizes-in-ab-testing-part-I/), we saw that if you run an experiment comparing the current version of your product (A) to a new alternative (B), and if you ship whichever version does best in the experiment, your business will on average experience a post-experiment per-user lift of 

$$ \hat{L} = \frac{\sigma_\Delta^2}{\sqrt{2\pi(\sigma_\Delta^2 + \frac{2\sigma_X^2}{n})}} $$

where $\sigma_\Delta^2$ is the variance on your normally distributed zero-mean prior for $\mu_B - \mu_A$, $\sigma_X^2$ is the within-group variance, and $n$ is the per-bucket sample size.

Because Part II is primarily concerned with the the duration of the experiment, we're going to modify the formula to be time-dependent. As a simplifying assumption we’re going to make *sessions*, rather then *users*, the unit of analysis. We’ll also assume that you have a constant number of sessions per day. This changes the definition of $\hat{L}$ to a _post-experiment per-session lift_, and the formula becomes 

$$ \hat{L} = \frac{\sigma_\Delta^2}{\sqrt{2\pi(\sigma_\Delta^2 + \frac{2\sigma_X^2}{mt})}} $$

where $m$ is the sessions per day for each bucket, and $t$ is duration of the experiment in days.

## Time Discounting

The formula above shows that larger sample sizes result in higher $ \hat{L} $, since larger samples make it more likely you will ship the better version. But as with all things in life, there are costs to increasing your sample size. In particular, the larger your sample size, the longer you have to wait to ship the winning bucket. In a fast moving startup, there's often good reason to accrue your wins as soon as possible. Lift today is much more valuable than the same lift a year from now. 

How much more valuable is lift today versus lift a year from now? A common way to quantify this is with exponential discounting, such that weights (or "discount factors") on future lift follow the form:

$$ w = e^{-rt} $$

where $ r $ is a discount rate. For teams shipping product at startups, the annual discount rate might be quite large, like 0.5 or even 1.0, which would correspond to a daily discount rate $r$ of 0.5/365 or 1.0/365, respectively.

<div class="wrapper">
  <img src='/assets/2020_optimizing_sample_sizes/discount_function.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption"><strong>Figure 1</strong>.
  </div>
</div><br>

## Aggregate time-discounted lift: Visual Intuition

Take a look at Figure 2, below. It shows an experiment that is planned to run for $\tau = 60$ days. The top panel shows $\hat{L}$. While the experiment is running, $\hat{L} = 0$, since our prior is that $\Delta$ is sampled from a normal distribution with mean zero. But once the experiment finishes and we launch the winning bucket, we should begin to reap our expected per-session lift. 

The middle panel shows our discount function.

The bottom panel shows our time-discounted lift, defined as the product of the lift in the top panel and the time discount in the middle panel. (We can also multiply it by $M$, the number of post-experiment sessions per day, which for simplicity we set to 1 here.) The aggregate time-discounted lift, $\hat{L}_a$, is the area under the curve.

<div class="wrapper">
  <img src='/assets/2020_optimizing_sample_sizes/discounted_lift_static.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption"><strong>Figure 2</strong>.
  </div>
</div><br>

Now let's see what happens with different experiment durations. Figure 3 shows that the longer you plan to run your experiment, the higher $\hat{L}$ will be (top panel). But due to time discounting, (middle panel), the area under the time-discounted lift curve (bottom panel) is low for overly large sample sizes. There is an optimal duration of the experiment (in this case, $\tau = 24$ days), that maximizes $\hat{L}_a$, the area under the curve.

<div class="wrapper">
  <img src='/assets/2020_optimizing_sample_sizes/discounted_lift_dynamic.gif' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption"><strong>Figure 3</strong>.
  </div>
</div><br>


## Aggregate time-discounted lift: Formula
The aggregate time-discounted lift $\hat{L}_a$, i.e. the area under the curve in the bottom panel of Figure 3, is: 

$$ \hat{L}_a = \frac{\sigma_\Delta^2 M e^{-r\tau}}{r \sqrt{2\pi(\sigma_\Delta^2 + \frac{2\sigma_X^2}{m\tau})}} $$

where $ \tau $ is the duration of the experiment and $M$ is the number of post-experiment sessions per day. See the Appendix for a derivation.

There's two things to note about this formula.
1. Increasing the number of bucketed sessions per day, $m$, always increases $\hat{L}_a$.
2. Increasing the duration of the experiment, $\tau$, may or may not help and is the result of competing forces in the numerator and denominator. In the numerator, higher $\tau$ decreases $\hat{L}_a$ by delaying shipment. In the denominator, higher $\tau$ increases $\hat{L}_a$ by making it more likely you will ship the superior version. 


## Optimizing sample size

At long last, we can answer the question, "How long should we run this experiment?". A nice way to do it is to plot $\hat{L}_a$ as a function of $\tau$. Below we see what this looks like for one set of parameters. Here the optimal duration is 18 days.
<div class="wrapper">
  <img src='/assets/2020_optimizing_sample_sizes/time_aggregated_lift_by_tau.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption"><strong>Figure 4</strong>.
  </div>
</div><br>
Note also that a set of simulated experiment and post-experiment periods (in blue) confirm the predictions of the closed form solution (in gray). See the [notebook](https://github.com/csaid/optimizing-sample-sizes-in-AB-testing/blob/master/Optimizing%20sample%20sizes%20in%20AB%20testing.ipynb) for details.

## Three lessons for practioners
I played around with the formula for $\hat{L}_a$ and came across three lessons I think will be of interest to practitioners.

#### 1. You should run "underpowered" experiments if you have a very high discount rate
Take a look at Figure 5, which shows some recommendations for conversion rate experiments where $p=0.10$ and $\sigma_\Delta=0.01$. On the left panel we plot the optimal duration as a function of the annual discount rate. If you have a high discount rate, you care a lot more about the near future than the distant future and therefore it is critical that you ship any potentially winning version as soon as possible. In this scenario, the optimal duration is low (left panel), and the probability you will find a statistically significant result is low (right panel). For many of these cases, the optimal duration would traditionally be considered “underpowered”.
<div class="wrapper">
  <img src='/assets/2020_optimizing_sample_sizes/optimal_tau_and_sig_rate_by_r.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption"><strong>Figure 5</strong>.
  </div>
</div><br>

#### 2. You should run 'underpowered' experiments if you have a small user base
Now let's plot these curves as a function of $m$, our daily sessions per bucket. If we only have a small number of sessions to work with, we'll need to run the experiment for longer (left panel). What's especially interesting is that the optimal duration for low $m$ scenarios still results in a low rate of statistical significance. That is, if you don't have a lot of users to work with, it's optimal to run an "underpowered" experiment. Waiting to get a large number of sessions just causes too much time-discounting loss. 
<div class="wrapper">
  <img src='/assets/2020_optimizing_sample_sizes/optimal_tau_and_sig_rate_by_m.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption"><strong>Figure 6</strong>.
  </div>
</div><br>

#### 3. That said, it's far better to run your experiment too long than too short
Below we plot the aggregate time-discounted lift, $\hat{L}_a$ as a function of duration, for various combinations of $m$ and $r$. In all plots the left shoulder is steeper than the right shoulder. This means that it's really bad if your experiment is shorter than optimal, but it's kind of ok if your experiment is longer than optimal. This is true for basically all parameter regimes, unless you have an insanely high discount rate (not shown). 

<div class="wrapper">
  <img src='/assets/2020_optimizing_sample_sizes/L_a_by_tau_for_m_and_r.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption"><strong>Figure 7</strong>.
  </div>
</div><br>

Side note: It's instructive to look at the bottom right panel of Figure 7, where there is a very high number of sessions per day and a relatively steep discount curve. Because of the high number of sessions, all of the benefits of the experiment are immediately obtained on the first day, and the $\hat{L}_a$ curve smashes up at its ceiling. After that, nothing else can be gained by extending the experiment, and any additional time spent is lost to discounting.


## Examples in Python
### Example 1: Continuous variable metric
Let's say you want to run an experiment comparing two different versions of a website, and your main metric is revenue per session. You know in advance that the within-group variance of this metric is $\sigma_X^2 = 100$. You don't know which version is better but you have a prior that the true difference in means is normally distributed with variance $\sigma_\Delta^2 = 1$. You have 200 sessions per day and plan to bucket 100 sessions into Version A and 100 sessions into Version B, running the experiment for $\tau=20$ days. Your discount rate is fairly aggressive at 1.0 annually, or $r = 1/365$ per day. Using the function in the [notebook](https://github.com/csaid/optimizing-sample-sizes-in-AB-testing/blob/master/Optimizing%20sample%20sizes%20in%20AB%20testing.ipynb), you can find $\hat{L}_a$ with ths command:

```python
get_agg_lift_via_closed_form(var_D=1, var_X=100, m=100, tau=25, r=1/365, M=200)
# returns 26298
```

You can also use the `find_optimal_tau` function to determine the optimal duration, which in this case is $\tau=18$.

### Example 2: Conversion rates
Let's say your main metric is conversion rate. You think that on average conversion rates will be about 10%, and that the difference in conversion rates between buckets will be normally distributed with variance 1%. Using the normal approximation of the binomial distribution, you can use `p*(1-p)` for `var_X`.

```python
p = 0.1
get_agg_lift_via_closed_form(var_D=0.01**2, var_X=p*(1-p), m=100, tau=25, r=1/365, M=200)
# returns 207
```

You can also use the `find_optimal_tau` function to determine the optimal duration, which in this case is $\tau=49$.

## FAQ
**Q:** Has there been any similar work on this?

**A:** As I was writing this, I came across a [fantastic in-press paper](https://arxiv.org/pdf/1811.00457.pdf) by [Elea Feit](https://drexel.edu/now/experts/Overview/Feit-Elea/) and [Ron Berman](https://www.ron-berman.com/). The paper is exceptionally clear and I would recommend reading it. Like this blog post, Feit and Berman argue that it doesn't make any sense to pick sample sizes based on statistical significance and power thresholds. Instead they recommend profit-maximizing sample sizes. They independently come to the same formula for $ \hat{L} $ as I do (see right addend in their Equation 9, making sure to substitute my $\frac{\sigma_\Delta^2}{2}$ for their $\sigma^2)$. Where they differ is that they assume there is a fixed pool of $N$ users that can only experience the product once. In their setup, you can allocate $n_1$ users to Bucket A and $n_2$ users to Bucket B. Once you have identified the winning bucket, you ship that version to the remaining $N-n_1-n_2$ users. Your expected profit is determined by the expected lift from those users. My experience in industry differs from this setup. In my experience there is no constraint that you can only show the product once to a fixed set of users. Instead, there is often an indefinitely increasing pool of new users, and once you ship the winning bucket you can ship it to everyone, including users who already participated in the experiment. To me, the main constraint in industry is therefore time discounting, rather than a finite pool of users.

**Q:** In addition to the lift from shipping a winning bucket, doesn't experimentation also help inform you about the types of products that might work in the future? And if so, doesn't this mean we should run experiments longer than recommended by your formula for $\hat{L}_a$?

**A:** Yes, experimentation can teach lessons that are generalizable beyond the particular product being tested. This is an advantage of high powered experimentation not included in my framework.

**Q:** If some users can show up in multiple sessions, doesn't bucketing by session violate independence assumptions?

**A:** Yeah, so this is tricky. For many companies, there is a distribution of user activity, where some users come for many sessions per week and other users come for only one session at most. Modeling this would make the framework significantly more complicated, so I tried to simplify things by making sessions the unit of analysis.

**Q:** Is there anything else on your blog related to this topic?

**A:** I'm glad you asked! 
* [Four pitfalls of hill climbing](/2016/02/28/four-pitfalls-of-hill-climbing/) discusses some product-focused issues in A/B testing 
* [Hyperbolic discounting — The irrational behavior that might be rational after all](/2018/02/04/hyperbolic-discounting/) is about time discounting, although not in the constext of experimentation.

## Appendix
The aggregate time-discounted lift $\hat{L}_a$ is 

$$ \hat{L}_a = \int_{\tau}^{\infty} \hat{L} M e^{-rt} \,dt $$

where $\hat{L}$ is the expected per-session lift, $M$ is the number of post-experiment sessions per day, $r$ is the discount rate, and $ \tau $ is the duration of the experiment. Solving the integral gives:

$$ \hat{L}_a = \frac{\hat{L} M e^{-r\tau}}{r} $$

Plugging in our previously solved value of $\hat{L}$ gives

$$ \hat{L}_a = \frac{\sigma_\Delta^2 M e^{-r\tau}}{r \sqrt{2\pi(\sigma_\Delta^2 + \frac{2\sigma_X^2}{m\tau})}} $$