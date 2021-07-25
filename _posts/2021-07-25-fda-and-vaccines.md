---
layout: post
title: The FDA is 5 times too worried about long term credibility
description: The FDA should fully approve the Covid vaccines now.
image: /assets/2021_fda_and_vaccines/fig_moderate.png
---


Covid-19 vaccinations are a triumph of science. Every major public health official in America recommends them. 

Despite widespread expert support for vaccination, the FDA is taking its time with vaccine approvals. While the FDA granted its first Emergency Use Authorization (EUA) in [December 2020](https://www.fda.gov/emergency-preparedness-and-response/coronavirus-disease-2019-covid-19/pfizer-biontech-covid-19-vaccine), full approval could come as late as [January or February 2022](https://www.healthline.com/health-news/when-will-the-fda-give-full-approval-for-covid-19-vaccines#Employer-COVID-19-vaccine-mandates).

[Matthew Yglesias](https://www.slowboring.com/p/vaccine-fda-approve) and [Eric Topol](https://www.nytimes.com/2021/07/01/opinion/fda-vaccines-full-approval.html) argue that the difference between full approval and an EUA matters a lot. Full FDA approval will make many organizations more comfortable with vaccine mandates, which can drive [massive increases in vaccination](https://www.economist.com/graphic-detail/2021/07/14/why-vaccine-shy-french-are-suddenly-rushing-to-get-jabbed), as seen in France. Full approval also allows vaccine providers to advertise, which will help persuade many people on the fence. Even without the mandates and advertising, [30% of unvaccinated people](https://www.kff.org/coronavirus-covid-19/poll-finding/kff-covid-19-vaccine-monitor-june-2021/) say they would be more likely to get vaccinated once full approval happens. The FDA’s sluggishness to approve the vaccine is therefore driving a large number of unnecessary infections and deaths.

Defenders of the FDA say that critics aren’t thinking about the long term. Sure, full approval might drive more vaccinations in the short term. But if safety issues emerge after full approval, the credibility of the FDA would be shot for years, driving fewer vaccinations in the long term.

---

I understand the point about long term credibility, but it feels hand-wavy and non-quantitative. Let’s do some back-of-the-envelope calculations to see how much it matters.

As a simplifying assumption, let’s suppose we’ll need a new Covid vaccine every year for the next 10 years, similar to the flu shot. Let’s generously assume that the FDA fully approves each vaccine from 2022 onwards in a timely manner. The only decision it faces is whether to do “fast” approval or “slow” approval for the 2021 vaccine, where fast approval means that the vaccine would be fully approved 1 year earlier than under slow approval. The figure below shows these two policies.

<div class="wrapper">
  <img src='/assets/2021_fda_and_vaccines/fig_policies.png' class="inner" style="position:relative border: #222 2px solid; max-width:90%;" >
  <div class="caption">**Figure 1**. Two policies the FDA could take.
  </div>
</div>


If the FDA does fast approval in 2021, more people will vaccinate in 2021. But if a safety issue emerges after the approval, the FDA might lose some credibility, leading to lower vaccination rates in subsequent years. Let’s now consider three possible scenarios.

### 1. Safe vaccine scenario
In the safe vaccine scenario, no new significant safety issues emerge after full approval. Perhaps some minor issues may emerge, but none will be large enough to drive large and sustained drops in vaccination rates. Given that historically all vaccine side effects have emerged [well within the first year](https://www.sandiegouniontribune.com/news/health/story/2021-05-31/misinformation-remains-the-biggest-hurdle-as-vaccination-effort-turns-to-cash-incentives), I think this scenario is fairly likely, perhaps around 85%.

Based on [current vaccination rates](https://www.mayoclinic.org/coronavirus-covid-19/vaccine-tracker), we should assume 50% vaccination in 2021 under slow approval. Based on [self-reports](https://www.kff.org/coronavirus-covid-19/poll-finding/kff-covid-19-vaccine-monitor-june-2021/), the promise of advertising, and the [powerful impact of vaccine mandates](https://www.economist.com/graphic-detail/2021/07/14/why-vaccine-shy-french-are-suddenly-rushing-to-get-jabbed), we can assume a 15 percentage point increase in 2021 vaccination with full approval, bringing the rate to 65%. Since we assume fast approval in all subsequent years, the vaccination rate will be 65% in both policies from 2022 onwards.

<div class="wrapper">
  <img src='/assets/2021_fda_and_vaccines/fig_baseline.png' class="inner" style="position:relative border: #222 2px solid; max-width:90%;" >
  <div class="caption">**Figure 2.** The two policies under a safe vaccine scenario, where no significant new safety issues emerge for the 2021 vaccine after full approval.
  </div>
</div>


With a total population of 300 million, and assuming all approvals are at the start of the year, fast approval in 2021 would drive a total of 0.15 * 300M = 45 million vaccinated person-years, compared to slow approval. 

### 2. Moderate issue scenario
In the moderate issue scenario, important new safety issues in the 2021 vaccine emerge after full approval, but their overall safety impact is “moderate”. By “moderate” I mean either a serious issue that affects a very small percentage of people, or a minor issue that affects a large number of people. Given that historically all safety issues with vaccines have emerged well within the first year, I consider this outcome unlikely. But since mRNA vaccines are a new and intensely scrutinized technology, let’s give this scenario a 19% probability.

If safety issues emerge, vaccination rates will drop. But critically, they will drop regardless of whether the 2021 vaccine had full approval or just an EUA. In the wake of bad news about the 2021 vaccine, the people hesitating about the 2022 vaccine aren’t going to really care whether the 2021 vaccine was fast approved or just EUA’d. Maybe they’ll care a little bit, but for the most part their hesitancy will be there regardless.

I therefore model this scenario with a dip in vaccination rates that lasts a few years, with a slightly larger dip if the 2021 vaccine had been fast approved.

<div class="wrapper">
  <img src='/assets/2021_fda_and_vaccines/fig_moderate.png' class="inner" style="position:relative border: #222 2px solid; max-width:90%;" >
  <div class="caption">**Figure 3.** The two policies under a moderate issue scenario.
  </div>
</div>


With a total population of 300 million, fast approval in 2021 would still provide the extra 45 million vaccinated person-years in 2021, but we’d lose 42 million vaccinated person-years in 2022-2030, netting out at +3 million vaccinated person-years.

### 3. Major issue scenario
In the major issue scenario, serious safety issues in the 2021 vaccine emerge in a large fraction of the population. Nothing remotely like this has never happened before, especially for a widely taken vaccine that already has almost a year’s worth of data, so I think this is extremely unlikely. But let’s give it a 1% chance anyway.

We’ll model this as a longer and deeper dip in vaccination rates. In this scenario, fast approval nets out to a loss of 20 million vaccinated person-years.

<div class="wrapper">
  <img src='/assets/2021_fda_and_vaccines/fig_major.png' class="inner" style="position:relative border: #222 2px solid; max-width:90%;" >
  <div class="caption">Figure 4. The two policies under a major issue scenario.
  </div>
</div>

### Putting all three scenarios together

Now let’s add up these outcomes weighted by their probability:

| |**Probability**|**Impact of 2021 fast approval on ten-year vaccinated person-years**|
|Safe vaccine scenario|85%|+45M|
|Moderate issue scenario|19%|+3M|
|Major issue scenario|1%|-20M|
|Weighted sum|(100%)|+39M|


Based on this back-of-the-envelope analysis, FDA could gain 39 million vaccinated person-years if they had fully approved the 2021 vaccine, even when accounting for long term risks. Assuming that [0.3% of unvaccinated people will die each year](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7721859/) because they are not vaccinated, this decision would save 115,000 lives in expectation. **To put it more starkly, an FDA decision to not fully approve in 2021 will cost 115,000 lives.** I have no doubt that the FDA is well-intentioned, but I really think they should reconsider their extreme cautiousness on this issue.

Under the model outlined above, the decision to delay full approval would only make sense if you thought the probability-weighted long-term risks were [5 times higher](https://github.com/csaid/fda_and_vaccines/blob/main/FDA%20and%20Vaccines.ipynb) than I think are reasonable.

### But what about…
Readers should absolutely question my assumptions. This post is a simplified back-of-the-envelope exercise because I believe all of the discussion so far has only been qualitative. If you think any of my assumptions are substantively wrong (and I’m sure some are!), it’s best to propose a _quantitative_ alternative. And be sure to explicitly consider the difference between a 2021 EUA and full approval. For example, perhaps you might think that hesitancy could persist for decades in a catastrophic safety scenario. Maybe so. But if that happens, will anybody decades from now care whether the 2021 vaccine had an EUA versus full approval? I don’t think so.

Finally, I should say that two of my most important assumptions were generous to the FDA. First, I assumed that we will continue to require annual Covid vaccines for the next 10 years. In this world, the “long term” (on the order of 10 years) is important, making the FDA’s concern about long term hesitancy very relevant. But this assumption might turn out to be false. Much of the next 10 years might be a return to normalcy, where Covid becomes a manageable infection similar to the flu, and most of the vaccines we take are less likely to be impacted by new vaccine hesitancy. In the same vein, we might only need a booster once every 5 years rather than every 1 year, which would blunt the impact of a medium-term loss of confidence.

Second, I assumed that all of the damage from slow approval would be confined to 2021 and that by 2022 the FDA would adopt fast approval. If that is not the case, and the FDA continues to take well over a year to approve each vaccine, the damage could be much greater.

