---
layout: post
title: Small correlations can drive large increases in teen depression
description: How a small correlation with social media can drive a 50% increase in teen depression
image: /assets/2022_teen_depression_and_social_media/fig_bars_preview.png
---
[[Code](https://github.com/csaid/teen-depression-and-social-media/blob/main/Teen%20mental%20health%20and%20social%20media%20%E2%80%93%20Back%20of%20the%20envelope%20calculations.ipynb)]

Major depressive episodes among teen girls have increased by 52% since 2005 ([Twenge, et al. 2020](https://prcp.psychiatryonline.org/doi/10.1176/appi.prcp.20190015)). Many reseachers believe social media is the primary cause ([Haidt & Twenge](https://docs.google.com/document/d/1w-HOfseF2wF9YIpXwUUtP65-olnkPyWcgF5BiAtBEy0/edit)).

Other researchers do not believe that social media is the cause, since the correlations between social media use and well-being are thought to be small. The correlation between overall screen time and well-being is -0.05. When [narrowing down](https://www.nature.com/articles/s41562-020-0839-4.epdf?author_access_token=AMli-v_NVizlRHfiHJUs2NRgN0jAjWel9jnR3ZoTv0NyO6WHXhaam3zaljiEGjfZWSw5xRcCYPYjudNb4RKEc1H5eAeNLyrwNMcZ3q6A3hZiGMwJNpRy1HGyUwXOLDn2TDAS79zv5Lgv80kc2gm_6A%3D%3D
) to social media and girls, the correlation is -0.17. Some researchers describe these correlations as "so minimal that they hold little practical value" ([Orben & Przybylski, 2019](https://files.de-1.osf.io/v1/resources/q7pr4/providers/osfstorage/5de76058e1e62f000c37365c?action=download&version=1&direct)). 

In this blog post, I use a simple linear+threshold model and back-of-the-envelope calculations to show that these correlations are not small and are in fact of significant practical importance. In particular, the -0.17 correlation for teen girls is enough to fully explain the 52% increase in depression since 2005. 

### Assumptions
My model makes the simplest possible assumptions. 
* Well-being is normally distributed. (The scale doesn't matter)
* Social media use averages 2 hours per day with a light rightward skew in the distribution. (Scale doesn't matter)
* Well-being is a linear function of social media use.
* Depression occurs when well-being falls below a threshold. The threshold was chosen to fit the baseline depression rate before social media.
* The correlation is causal.
<div class="wrapper">
  <img src='/assets/2022_teen_depression_and_social_media/fig_hours.png' class="inner" style="position:relative border: #222 2px solid; max-width:60%;" >
  <div class="caption">**Figure 1.** I assume social media use averages 2 hours per day with a light rightward skew.
  </div>
</div>

### Results
Using the simple linear+threshold model, a correlation of 0.17 would increase depression rates in girls by 50%, from the pre- social media baseline of 13% to a new value of 19%, which is almost the same value as observed in [Twenge, et al. (2020)](https://prcp.psychiatryonline.org/doi/10.1176/appi.prcp.20190015). 
<div class="wrapper">
  <img src='/assets/2022_teen_depression_and_social_media/fig_bars.png' class="inner" style="position:relative border: #222 2px solid; max-width:60%;" >
  <div class="caption">**Figure 2.** A model with a 0.17 correlation between social media and well-being in girls can successfuly predict a 50% increase in depression.
  </div>
</div>

In boys (see [notebook](https://github.com/csaid/teen-depression-and-social-media/blob/main/Teen%20mental%20health%20and%20social%20media%20%E2%80%93%20Back%20of%20the%20envelope%20calculations.ipynb)), the correlation between social media and well-being is much weaker, and the model predicts a much smaller increase in rates of depression, which is what we see in the data. 

### How can a "small" correlation drive a large increase in depression?
Since depression starts out somewhat rare (13%), a "small" downward shift in the well-being distribution can cause a large relative change in the percentage of teens with well-being below the depression threshold. And regardless of threshold effects, the drop in well-being will not seem "small" to the teens experiencing it.
<div class="wrapper">
  <img src='/assets/2022_teen_depression_and_social_media/animation.gif' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
  <div class="caption">**Figure 3.** Small changes in the slope between social media and well-being can drive a large relative change in the fraction of teens below the depression threshold (orange).
  </div>
</div>

### Assumption questioning zone
This is a back-of-the-envelope model. Its purpose is _not_ to precisely describe the relationship between social media and well-being. Rather, it is meant to respond to claims that small correlations cannot possibly explain large increases in depression. As this model shows, they can.

If one wishes, the model's assumptions could be further refined, which would variously strengthen or weaken the size of the effect.
* Allowing noise in depression diagnoses, rather than a hard threshold, would weaken the effect.
* Adding [nonlinearities](https://doi.org/10.1038/s41562-020-0839-4) would strengthen the effect.
* Adding [network effects](https://chris-said.io/2021/08/14/teens-loneliness-social-media/) would strengthen the effect.

For more discussion on social media and teen mental health, see this [open source literature review](https://docs.google.com/document/d/1w-HOfseF2wF9YIpXwUUtP65-olnkPyWcgF5BiAtBEy0/edit), or see my [previous blog post on network effects](https://chris-said.io/2021/08/14/teens-loneliness-social-media/).