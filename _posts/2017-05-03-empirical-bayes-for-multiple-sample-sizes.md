---
layout: post
title: Empirical Bayes for multiple sample sizes
description: The James-Stein Estimator with unequal variances
---

Here's a data problem I encounter *all the time*. Let's say I'm running a website where users can submit movie ratings on a continuous 1-10 scale. For the sake of argument, let's say that the users who rate each movie are an unbiased random sample from the population of users. I'd like to compute the average rating for each movie so that I can create a ranked list of the best movies.

Take a look at my data:

<img src="/assets/2017_shrinkage/fig_movies.png">
<div class="caption"><strong>Figure 1.</strong> Each circle represents a movie rating by a user. Diamonds represent sample means for each movie.</div>
<br>
I've got two big problems here. First, nobody is using my website. And second, I'm not sure if I can trust these averages. The movie at the top of the rankings was only rated by two users, and I've never even heard of it! Maybe the movie really is good. But maybe it just got lucky. Maybe just by chance, the two users who gave it ratings happened to be the users who liked it to an unusual extent. It would be great if there were a way to adjust for this.

In particular, I would like a method that will give me an estimate closer to the movie's *true mean* (i.e. the mean rating it would get if an infinite number of users rated it). Intuitively, movies with mean ratings at the extremes should be nudged, or "shrunk", towards the center. And intuitively, movies with low sample sizes should be shrunk more than the movies with large sample sizes.

<img src="/assets/2017_shrinkage/fig_movies_shrinkage.png">
<div class="caption"><strong>Figure 2.</strong> Each circle represents a movie rating by a user. Diamonds represent sample means for each movie. Arrows point towards shrunken estimates of each movie's mean rating. Shrunken estimates are obtained using the MSS James-Stein Estimator, described in more detail below.</div>
<br>

As common a problem as this is, there aren't a lot of accessible resources describing how to solve it. There are tons of [excellent blog posts](http://varianceexplained.org/r/empirical_bayes_baseball/) about the Beta-binomial distribution, which is useful if you wish to estimate the true fraction of an occurrence among some events. This works well in the case of Rotten Tomatoes, where one might want to know the true fraction of "thumbs up" judgments. But in my case, I'm dealing with a continuous 1-10 scale, not thumbs up / thumbs down judgments. The Beta-binomial distribution will be of little use.

Many resources mention the James-Stein Estimator, which provides a way to shrink the mean estimates only when the variances of those means can be assumed to be equal. That assumption usually only holds when the *sample sizes* of each group are equal. But in most real world examples, the sample sizes (and thus the variances of the means) are not equal. When that happens, it's a lot less clear what to do. 

After doing a lot of digging and [asking](https://twitter.com/Chris_Said/status/851211601445240832) some very helpful folks on Twitter, I found several solutions. For many of the solutions, I ran simulations to determine out which worked best. This blog post is my attempt to summarize what I learned. Along the way, we'll cover the original James-Stein Estimator, two extensions to the James-Stein Estimator, Markov Chain Monte Carlo (MCMC) methods, and several other strategies.

Before diving in, I want to include a list of symbol definitions I'll be using because -- side rant -- it sure would be great if all stats papers did this, given that literally every paper I read used its own idiosyncratic notations! I'll define everything again in the text, but this is just here for reference:

<div style='margin:40px'>
<table style='font-size:.7rem'>
    <tr>
        <th>Symbol</th>
        <th>Definition</th>
    </tr>
    <tr>
        <td>$ k $</td>
        <td>The number of groups.</td>
    </tr>
    <tr>
        <td>$ \theta_i $</td>
        <td>The true mean of a group.</td>
    </tr>
    <tr>
        <td>$ x_i $</td>
        <td>The sample mean of a group. The MLE estimate of $ \theta_i $.</td>
    </tr>
    <tr>
        <td>$ \epsilon^{2}_i $</td>
        <td>The true variance of observations within a group.</td>
    </tr>
    <tr>
        <td>$ \epsilon^2 $</td>
        <td>The true variance of observations within a group if we assume all groups have the same variance.</td>
    </tr>
    <tr>
        <td>$ s^{2}_i $</td>
        <td>The sample variance of a group. The MLE estimate of $ \epsilon^{2}_i $.</td>
    </tr>
    <tr>
        <td>$ n_i $</td>
        <td>The number of observations in a group.</td>
    </tr>
    <tr>
        <td>$ n $</td>
        <td>The number of observations in a group, if we assume all groups have the same size.</td>
    </tr>
    <tr>
        <td>$ \sigma^{2}_i $</td>
        <td>The true variance of a group's mean. If each group has the same variance of observations, then $ \sigma^{2}_i  = \epsilon^{2} / n_i $. If each group has different variances of observations, then $ \sigma^{2}_i = \epsilon^{2}_i / n_i $.</td>
    </tr>
    <tr>
        <td>$ \sigma^{2} $</td>
        <td>Like $ \sigma^{2}_i $, but if we assume all groups had the same variance of the mean. Equal to $ \epsilon^{2} / n $.</td>
    </tr>
    <tr>
        <td>$ \hat{\sigma^{2}_i} $</td>
        <td>Estimate of $ \sigma^{2}_i $.</td>
    </tr>
    <tr>
        <td>$ \hat{\sigma^{2}} $</td>
        <td>Estimate of $ \sigma^{2} $. 
        </td>
    </tr>
    <tr>
        <td>$ \mu $</td>
        <td>The true mean of the $ \theta_i $'s (the true group means). The mean of the distribution from which the $ \theta_i $'s are drawn.</td>
    </tr>
    <tr>
        <td>$ \overline{X} $</td>
        <td>The sample mean of the sample means.</td>
    </tr>
    <tr>
        <td>$ \tau^{2} $</td>
        <td>The true variance of the $ \theta_i $'s (the true group means). The variance of the distribution from which the $ \theta_i $'s are drawn.</td>
    </tr>
    <tr>
        <td>$ \hat{\tau^{2}} $</td>
        <td>Estimate of $ \tau^{2} $.</td>
    </tr>
    <tr>
        <td>$ \hat{B} $</td>
        <td>Estimate of the best term for weighting $ x_i $ and $ \overline{X} $ when calculating $ \hat{\theta_i} $. Assumes each group has the same $ \sigma^2 $.</td>
    </tr>
    <tr>
        <td>$ \hat{B_i} $</td>
        <td>Estimate of the best term for weighting $ x_i $ and $ \overline{X} $ when calculating $ \hat{\theta_i} $. Does not assume that all group's have the same $ \sigma^{2}_i $.</td>
    </tr>
    <tr>
        <td>$ \hat{\theta_i} $</td>
        <td>Estimate of a true group means. Its value depends on the method we use.</td>
    </tr>
    <tr>
        <td>$ k_{\Gamma} $</td>
        <td>Shape parameter for the Gamma distribution from which sample sizes are drawn.</td>
    </tr>
    <tr>
        <td>$ \theta_{\Gamma} $</td>
        <td>Scale parameter for the Gamma distribution from which sample sizes are drawn.</td>
    </tr>
    <tr>
        <td>$ \mu_v $</td>
        <td>In simulations in which group observation variances $ \epsilon^{2}_i $ are allowed to vary, this is the mean parameter of the log-normal distribution from which the $ \epsilon^{2}_i $'s are drawn.</td>
    </tr>
    <tr>
        <td>$ \tau^{2}_v $</td>
        <td>In simulations in which group observation variances $ \epsilon^{2}_i $ are allowed to vary, this is the variance parameter of the log-normal distribution from which the $ \epsilon^{2}_i $'s are drawn.</td>
    </tr>
</table>
</div>

### Quick Analytic Solutions

Our goal is to find a better way of estimating $ \theta_i $, the true mean of a group. A [common](http://projecteuclid.org/download/pdf_1/euclid.cbms/1462106062) [theme](https://mathmodelsblog.wordpress.com/2010/02/02/introduction-to-buhlmann-credibility/) [in](https://www.soa.org/files/pdf/c-24-05.pdf) [many](http://www.stat.cmu.edu/~acthomas/724/Efron-Morris.pdf) [of](http://www.tandfonline.com/doi/abs/10.1080/09332480.2007.10722861) [the](http://intlpress.com/site/pub/files/_fulltext/journals/sii/2010/0003/0004/SII-2010-0003-0004-a011.pdf) [papers](http://conservancy.umn.edu/bitstream/handle/11299/5852/Staffpaper10.pdf;jsessionid=433FD5983CCE7EE49AE0319D9B9FFA02?sequence=1) I read is that good estimates of $ \theta_i $ are usually weighted averages of the group's sample mean $ x_i $ and the global mean of all group means $ \overline{X} $. Let's call this weighting factor $ \hat{B_i} $.

$$ \hat{\theta_i} = \left(1-\hat{B_i}\right) x_i + \hat{B_i} \overline{X} $$

This seems very sensible. We want something that is in between the sample mean (which is probably too extreme) and the mean of means. But how do we know what value to use for $ \hat{B_i} $? Different methods exist, and each leads to different results.

Let's start by defining $ \sigma^{2}_i $, the true variance of a group's mean. This is equivalent to $ \epsilon^{2}_i / n_i $, where $ \epsilon^{2}_i $ is the true variance of the observations within that group, and $ n_i $ is the sample size of the group. According to the original James-Stein approach, if we assume that all the group means have the same known variance $ \hat{\sigma^2} $, which would usually only happen if the groups all had the same sample size, then we can define a common $ \hat{B} $ for all groups as:

$$ \hat{B} = \frac{\left(k-3\right)\hat{\sigma^2}}{\sum{\left(x_i - \overline{X}\right)}} $$

This formula seems really weird and arbitrary, but it begins to make more sense if we rearrange it a bit and sweep that pesky $ \left(k-3\right) $ under the rug and replace it with a $ (k-1) $. Sorry hardliners!

$$
\begin{eqnarray} 
\hat{B} &\approx& \frac{\left(k-1\right)\hat{\sigma^2}}{\sum{\left(x_i - \overline{X}\right)}}\\
  \\
  &=& \frac{\hat{\sigma^2}}{\sum{\left(x_i - \overline{X}\right)}/\left(k-1\right)}  \\
  \\
  &=& \frac{\hat{\sigma^2}}{\hat{\tau^{2}} + \hat{\sigma^2}}

\end{eqnarray} 
$$

Before getting to why this makes sense, I should explain the last step above. The denominator $ \sum{\left(x_i - \overline{X}\right)}/\left(k-1\right) $ is the observed variance of the observed sample means. This variance comes from two sources: $ \tau^2 $ is true variance in the true means and $ \sigma^2 $ is the true variance caused by the fact that each $ x_i $ is computed from a sample. Since variances add, the total variance of the observed means is $ \tau^{2} + \sigma^{2} $.

Anyway, back to the result. This result is actually pretty neat. When we estimate a $ \theta_i $, the weight that we place on the global mean $ \overline{X} $ is the fraction of total variance in the means that is caused by within-group sampling variance. In other words, when the sample mean comes with high uncertainty, we should weight the global mean more. When the sample mean comes with low uncertainty, we should weight the global mean less. At least directionally, this makes sense.

This idea is so widely applicable that many other fields have discovered it independently. In the image processing literature, it is a special case of the [Wiener Filter](http://www.dfmf.uned.es/~daniel/www-imagen-dhp/biblio/adaptive-wiener-noisy.pdf), assuming that both the signal and the additive noise are Gaussian. In the insurance world, actuaries call it the [Bühlmann model](https://en.wikipedia.org/wiki/B%C3%BChlmann_model). And in [animal breeding](https://en.wikipedia.org/wiki/Charles_Roy_Henderson), early researchers called it the Best Unbiased Linear Prediction or [BLUP](http://www.public.iastate.edu/~dnett/S511/27BLUP.pdf) (technically the Empirical BLUP). The BLUP approach is so useful, in fact, that it has received the highly coveted endorsement of the [National Swine Improvement Federation](http://www.nsif.com/guidel/guidelines.htm).

While the original James-Stein formula is useful, the big limitation is that it only works when we believe that all groups have the same $ \sigma^2 $. In cases where we have different sample sizes, each group will have it's own $ \sigma^{2}_i $. (Recall that $ \sigma^{2}_i = \epsilon^{2}_i / n_i $.) We're going to want to shrink some groups more than others, and the original James-Stein estimator does not allow this. In the following sections, we'll look at a couple of extensions to the James-Stein estimator. These extensions have analogues in the Bühlmann model and BLUP literature. 

#### The Multi Sample Size James-Stein Estimator

The most natural extension of James-Stein is to define each group's $ \hat{\sigma^{2}_i} $ as the squared standard error of the group's mean. This allows us to estimate a weighting factor $ \hat{B_i} $ tailored to each group. Let's call this the Multi Sample Size James-Stein Estimator, or MSS James-Stein Estimator. 

$$ \hat{B_i} = \frac{\hat{\sigma^{2}_i}}{\hat{\tau^{2}} + \hat{\sigma^{2}_i}} $$

The denominator can just be estimated as the variance across group sample means.

As reasonable as this approach sounds, it somehow didn't feel totally kosher to me. But when I looked into the literature, it seems like most researchers basically said "[yup](http://projecteuclid.org/download/pdf_1/euclid.cbms/1462106062), [that sounds](https://mathmodelsblog.wordpress.com/2010/02/02/introduction-to-buhlmann-credibility/) [pretty reasonable](https://www.soa.org/files/pdf/c-24-05.pdf)".

To test this approach, I ran some [simulations](https://github.com/csaid/empirical_bayes/blob/master/simulations.ipynb) on 1000 artificial datasets. Each dataset involved 25 groups with sample sizes drawn from a Gamma distribution $ \Gamma(k_{\Gamma}=1.5,\theta_{\Gamma}=10) $. True group means ($ \theta_i $'s) were sampled from a Normal distribution $ \mathcal{N}(\mu, \tau^2) $. Observations within each group were sampled from $ \mathcal{N}(\theta_i, \epsilon^2) $, where $ \epsilon $ was shared between groups. 

For each dataset I computed the Mean Squared Error (MSE) between the vector of true group means and the vector of estimated group means. I then averaged the MSEs across datasets. This process was repeated for a variety of different values of $ \epsilon $ and for two different estimators: The MSS James-Stein Estimator and the Maximum Likelihood Estimator (MLE). To compute the MLE, I just used $ x_i $ as my $ \hat{\theta_i} $ estimate.

{% include image.html url="/assets/2017_shrinkage/fig_shared_2.png" %}
<div class="caption"><strong>Figure 3.</strong> Mean Squared Error between true group means and estimated group means. In this simulation, the $ \epsilon $ parameter for within-group variance of observations is shared by all groups.</div>
<br>
As expected, the MSS James-Stein Estimator outperformed the MLE, with lower MSEs particularly for high values of $ \epsilon $. This make sense. When the raw sample means are noisy, the MLE should be especially untrustworthy and it makes sense to pull extreme estimates back towards the global mean.

#### The Multi Sample Size Pooled James-Stein Estimator

One thing that's a little weird about the MSS James-Stein Estimator is that even though we know all the groups should have the same within-group variance $ \epsilon^2 $, we still estimate each group's standard error separately. Given what we know, it might make more sense to [pool](https://en.wikipedia.org/wiki/Pooled_variance) the data from all groups to estimate a common $ \epsilon^2 $. Then we can estimate each group's $ \sigma^{2}_i $ as $ \epsilon^2 / n_i $. Let's call this approach the MSS Pooled James-Stein Estimator.

{% include image.html url="/assets/2017_shrinkage/fig_shared_3.png" %}
<div class="caption"><strong>Figure 4.</strong> Mean Squared Error between true group means and estimated group means. In this simulation, the $ \epsilon $ parameter for within-group variance of observations is shared by all groups.</div>
<br>
This works a bit better. By obtaining more accurate estimates of each group's $ \sigma^{2}_i $, we are able to find a more appropriate shrinking factor $ B_i $ for each group. 

Of course, this only works better because we created the simulation data in such a way that all groups have the same $ \epsilon^2 $. But if we run a different set of simulations, in which each group's $ \epsilon_i $ is drawn from a log-normal distribution $ ln\mathcal{N}\left(\mu_v, \tau^{2}_v\right) $, we obtain the reverse results. The MSS James-Stein Estimator, which estimates a separate $ \hat{\epsilon^{2}_i} $ for each group, does a better job than the  MSS Pooled James-Stein Estimator. This makes sense.

{% include image.html url="/assets/2017_shrinkage/fig_unshared_3.png" %}
<div class="caption"><strong>Figure 5.</strong> Mean Squared Error between true group means and estimated group means. In this simulation, each group has its own variance parameter $ \epsilon^2 $ for the observations within the group. These parameters are sampled from a log-normal distribution $ ln\mathcal{N}\left(\mu_v, \tau^{2}_v\right) $. For simplicity, the two parameters of this distribution are always set to be identical, and are shown on the horizontal axis. </div>
<br>

Which method you choose should depend on whether you think your groups have similar or different variances of their observations. Here's an interim summary of the methods covered so far.

<div class='box'>
<h4 style="margin-top:0rem">Summary of analytic solutions</h4>
All of these estimators define $ \hat{\theta_i} $ as a weighted average of the group sample mean $ x_i $ and the mean of group sample means $ \overline{X} $.
$$ \hat{\theta_i} = \left(1-\hat{B}\right) x_i + \hat{B_i} \overline{X} $$

Make sure to clip $ \hat{B_i} $ to the range [0, 1].<br><br>

<ol>
    <li style='margin-bottom: 10px'>
        <div><strong>Maximum Likelihood Estimation (MLE)</strong></div>
        <div style='margin-top: 0px; line-height: 5.7em'> $ \hat{B_i} = 0 $</div>
    </li>

    <li style='margin-bottom: 20px'>
        <div><strong>MSS James-Stein Estimator</strong></div>
        <div style='margin-top: 0px; line-height: 5.7em'> $ \hat{B_i} = \frac{s^{2}_i/n_i}{\sum\frac{(x_i - \overline{X})^2}{k-1}} $</div>
        where $ s_i $ is the standard deviation of observations with a group.<br>
    </li>

    <li>
        <div><strong>MSS Pooled James-Stein Estimator</strong></div>
        <div style='margin-top: 0px; line-height: 5.7em'> $ \hat{B_i} = \frac{s^{2}_p/n_i}{\sum\frac{(x_i - \overline{X})^2}{k-1}} $</div>
        where $ s^{2}_p $ is the <a href="https://en.wikipedia.org/wiki/Pooled_variance">pooled</a> estimate of variance.
    </li>
</ol>
Implementations in Python and R are available <a href = "https://github.com/csaid/empirical_bayes">here</a>.
</div>

#### A Bayesian interpretation of the analytic solutions

So far, the analytic approaches make sense directionally. As described above, our estimate of $ \theta_i $ should be a weighted average of $ x_i $ and $ \overline{X} $, where the weight depends on the ratio of sample mean variance to total variance of the means.

$$ \theta_i = \left(1 - \frac{\hat{\sigma^{2}_i}}{\hat{\tau^{2}} + \hat{\sigma^{2}_i}}\right) x_i + \left(\frac{\hat{\sigma^{2}_i}}{\hat{\tau^{2}} + \hat{\sigma^{2}_i}}\right) \overline{X} $$

But is this really the best weighting? Why use a ratio of variances instead of, say, a ratio of standard deviations? Why not use something else entirely?

It turns out this formula falls out naturally from Bayes Law. Imagine for a moment that we already know the prior distribution $ \mathcal{N}\left(\mu, \tau^2\right) $ over the $ \theta_i $'s. And imagine we know the likelihood function for a group mean is $ \mathcal{N}\left(x_i, \epsilon^{2}_i/n_i\right) $. According to the Wikipedia page on [conjugate priors](https://en.wikipedia.org/wiki/Conjugate_prior#Continuous_distributions), the posterior distribution for the group mean is itself a Gaussian distribution with mean:

$$ \hat{\theta_i} = \frac{\frac{x_i n_i}{\epsilon^{2}_i} + \frac{\mu}{\tau^{2}}}{\frac{1}{\tau^2} + \frac{n_i}{\epsilon^{2}_i}} $$

(Note that the Wikipedia [page](https://en.wikipedia.org/wiki/Conjugate_prior#Continuous_distributions) uses the symbol '$ x_i $' to refer to observations, whereas this blog post will always use the term to refer to the sample mean, including in the equation above. Also note that Wikipedia refers to the variance of observations within a group as '$ \sigma^2 $' whereas this blog post uses $ \epsilon^{2}_i $.) 

If we multiply all terms in the numerator and denominator by $ \frac{\tau^2 \epsilon^{2}_i}{n_i} $, we get:

$$ \hat{\theta_i} = \frac{\tau^2 x_i + \mu \sigma^{2}_i}{\sigma^{2}_i + \tau^2} $$

Or equivalently,

$$ \hat{\theta_i} = \left(1-B\right) x_i + B \mu $$

where

$$ B = \frac{\sigma^{2}_i}{\sigma^{2}_i + \tau^2} $$

This looks familiar! It is basically the MSS James-Stein estimator. The only difference is that in the pure Bayesian approach you must somehow know $ \mu $, $ \tau^2 $, and $ \sigma^{2}_i $ in advance. In the MSS James-Stein approach, you estimate those parameters from the data itself. This is the key insight in Empirical Bayes: Use priors to keep your estimates under control, but obtain the priors *empirically* from the data itself.

### Hierarchical Modeling with MCMC

In previous sections we looked at some analytic solutions. While these solutions have the advantage of being quick to calculate, they have the disadvantage of being less accurate than they could be. For more accuracy, we can turn to Hierarchical Model estimation using Markov Chain Monte Carlo (MCMC) methods. MCMC is an iterative process for approximate Bayesian inference. While it is slower than analytical approximations, it tends to be more accurate and has the added benefit of giving you the full posterior distribution. I'm not an expert in how it works internally, but [this post](http://twiecki.github.io/blog/2015/11/10/mcmc-sampling/) looks like a good place to start.

To implement this, I first defined a [Hierarchical Model](http://mc-stan.org/documentation/case-studies/radon.html) of my data. The model is a description of how I think the data is generated: True means are sampled from a normal distribution, and observations are sampled from a normal distribution centered around the true mean of each group. Of course, I know exactly how my data was generated, because I was the one who generated it! The key thing to understand though is that the Hierarchical Model does not contain any information about the value of the parameters. It's the MCMC's job to figure that out. In particular, I used [PyStan's](https://pystan.readthedocs.io/en/latest/) MCMC implementation to fit the parameters of the model based on my data, although I later [learned](https://twitter.com/talyarkoni/status/859837371034062848) that it would be even easier to use [bambi](https://github.com/bambinos/bambi).

{% include image.html url="/assets/2017_shrinkage/fig_shared_4.png" %}
<div class="caption"><strong>Figure 6.</strong> Mean Squared Error between true group means and estimated group means. In this simulation, the $ \epsilon $ parameter for within-group variance of observations is shared by all groups.</div>
<br>

For simulated data with shared $ \epsilon^2 $, MCMC did well, outperforming both the MSS James-Stein estimator and the MSS Pooled James-Stein estimator.

If you don't care about speed and are willing to write the Stan code, then this is probably your best option. It's also good to learn about MCMC methods, since they can be applied to more complicated models with multiple variables. But if you just want a quick estimate of group means, then one of the analytic solutions above makes more sense.

### Other solutions

There are several other solutions that I did not include in my simulations.

1. [Regularization](https://twitter.com/seanjtaylor/status/851227324796157953). Pick the set of $ \hat{\theta_i} $'s that minimize $$ \sum{(\hat{\theta_i} - x_i)^2} + \lambda \sum{(\hat{\theta_i} - \overline{X})^2} $$. Use cross-validation to choose the best $ \lambda $. This will probably work pretty well, although it takes a bit more work and time than the analytic solutions described above.

2. [Mixed Models](http://www.bodowinter.com/tutorial/bw_LME_tutorial2.pdf). Over in Mixed Models World, there's a whole &rsquo;nother literature on how to shrink estimates depending on sample size. I don't really understand the math behind Mixed Models, but I was able to use [lme4](https://cran.r-project.org/web/packages/lme4/lme4.pdf) to estimate group means in simulated data under the assumption that the group means are a random effect. This gave me slightly different results compared to the James-Stein / Empirical Bayes approach. I would love if some expert who understood this could write an accessible and authoritative blog post on the differences between Mixed Models and Empirical Bayes. The closest I could find was [this comment](http://projecteuclid.org/download/pdf_1/euclid.ss/1177011928) by David Harville. 

3. [Efron and Morris](http://www.stat.cmu.edu/~acthomas/724/Efron-Morris.pdf)' generalization of James-Stein to unequal sample sizes (Section 3 of their paper). I thought this paper was difficult to read. A more accessible presentation can be found at the end of [this column](http://www.tandfonline.com/doi/abs/10.1080/09332480.2007.10722861). The Efron and Morris approach is a numerical solution that seemed to work reasonably well when I played around with it, but I didn't take it very far. If you want to implement it, be sure to prevent any variance estimates from falling below zero. If one of them does, just set it to zero and then compute your estimates of the means. That being said, I feel like if you're going to go with a numerical solution, you may as well just go with MCMC.

4. [Double Shrinkage](http://intlpress.com/site/pub/files/_fulltext/journals/sii/2010/0003/0004/SII-2010-0003-0004-a011.pdf). When we think that different groups not only have different sample sizes, but also different $ \epsilon_i $'s, we are faced with an interesting conundrum. As shown above, the MSS James-Stein Estimator outperforms the MSS Pooled James-Stein Estimator, because it computes $ \hat{\epsilon_i} $'s specific to each group. However, these estimates of group variances are probably noisy! Just like we don't trust the raw estimates of group sample means, why should we trust the raw estimates of group sample variances? One way to address this is to use Zhao's Double Shinkage Estimator, which not only shrinks the means, but also shrinks the variances.

5. [Kleinman's weighted moment estimator](https://www.jstor.org/stable/2284137?seq=1#fndtn-page_scan_tab_contents). Apparently this was motivated by groups of proportions (i.e. the Rotten Tomatoes case), but the estimator can be applied generally.

### Conclusion
Which method you choose depends on your situation. If you want a simple and computationally fast estimate, and if you don't want to assume that the group variances $ \epsilon^{2}_i $ are identical, I would recommend either the MSS James-Stein Estimator or the Double Shrinkage Estimator if you can get it to work. If you want a fast estimate and can assume all groups share the same $ \epsilon^{2} $, I'd recommend the MSS Pooled James-Stein Estimator. If you don't care about speed or code complexity, I'd recommend MCMC, Mixed Models, or regularization with a cross-validated penalty term.


### Acknowledgements
Special thanks to the many people who responded to my [original question on Twitter](https://twitter.com/Chris_Said/status/851211601445240832), including: [Sean Taylor](https://twitter.com/seanjtaylor), [John Myles White](https://twitter.com/johnmyleswhite), [David Robinson](https://twitter.com/drob), [Joe Blitzstein](https://twitter.com/stat110), [Manjari Narayan](https://twitter.com/NeuroStats), [Otis Anderson](https://twitter.com/oldjacket), [Alex Peysakhovich](https://twitter.com/alex_peys), [Nathaniel Bechhofer](https://twitter.com/bechhof), [James Neufeld](https://twitter.com/jamneuf), [Andrew Mercer](https://twitter.com/awmercer), [Patrick Perry](https://twitter.com/ptrckprry), [Ronald Richman](https://twitter.com/RichmanRonald), [Timothy Sweetster](https://twitter.com/Hacktuarial), and [Tal Yarkoni](https://twitter.com/talyarkoni) and [Alex Coventry](https://twitter.com/AlxCoventry). Special thanks also to Marika Inhoff and Leo Pekelis for many discussions.

All code used in this blog post is available on [GitHub](https://github.com/csaid/empirical_bayes).

