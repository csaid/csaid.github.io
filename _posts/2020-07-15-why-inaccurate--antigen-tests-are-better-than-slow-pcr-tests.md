---
layout: post
title: Why low sensitivity antigen tests are better than slow PCR tests
description: For asymptomatic testing in schools and offices, speed matters more than sensitivity.
image: /assets/2020_antigen_testing/fig_test_every_1_day.png
---

_[Update: After I wrote this post, I came across a recently posted preprint from [Larremore et al.](https://www.medrxiv.org/content/10.1101/2020.06.22.20136309v2.full.pdf) that makes this point much better than I do. The paper emphasizes that antigen tests have high sensitivity when viral load is high, corresponding to when you are contagious. See [rapidtests.org](https://www.rapidtests.org/) for more info._

If we want to reopen schools and offices, we need to do so safely. According to Nobel Prize winning economist Paul Romer, the best way is with [frequent](https://schools.paulromer.net/) and [regular](https://paulromer.net/faqs-on-virus-tests-in-schools/) testing of asymptomatic students and employees. Catching infections early can stop the spread before it starts.

Much of the discussion about testing has centered on [PCR](https://www.npr.org/sections/health-shots/2020/05/01/847368012/how-reliable-are-covid-19-tests-depends-which-one-you-mean), a relatively expensive method that is vulnerable to [multiple](https://www.npr.org/2020/05/28/863558750/coronavirus-testing-machines-are-latest-bottleneck-in-troubled-supply-chain) [supply chain](https://www.latimes.com/california/story/2020-07-12/california-fail-coronavirus-testing-covid-start) bottlenecks and is generally [hard to scale up](https://www.technologyreview.com/2020/05/06/1001150/podcast-covid-19-testing-bottleneck/). Test samples must be sent to a central laboratory and then passed through expensive machines, a process that is currently taking 5-7 days, and in some places as high as [10-13 days](https://twitter.com/SFCovidTestWait/status/1283122098714693633). Efforts are being made to speed this up via pooling, but the operational complexity of PCR makes it likely that there will always be at least a few days of delay. 

A less talked about alternative to PCR is called _[antigen tests](https://www.sciencemag.org/news/2020/05/coronavirus-antigen-tests-quick-and-cheap-too-often-wrong)_. These tests are much cheaper than PCR, with estimates ranging between [$1](https://www.sciencemag.org/news/2020/05/coronavirus-antigen-tests-quick-and-cheap-too-often-wrong) and [$10](https://www.technologyreview.com/2020/04/24/1000486/antigen-testing-could-faster-cheaper-diagnose-covid-19-coronavirus/) per test, compared to [$50](https://www.technologyreview.com/2020/04/24/1000486/antigen-testing-could-faster-cheaper-diagnose-covid-19-coronavirus/) for PCR. More importantly, they can return results in just [15 minutes](https://www.cdc.gov/flu/professionals/diagnosis/clinician_guidance_ridt.htm), without any need to send samples to a central lab.

Since antigen tests are far cheaper and faster than PCR, why are we not massively investing in them? 

The usual answer is test sensitivity. SARS-CoV-2 antigen tests have a sensitivity of about [85-90%](https://www.nytimes.com/2020/07/06/health/fast-coronavirus-tests.html), meaning that only 85%-90% of true cases will be detected as such. And those are optimistic estimates reported by test manufacturers. For influenza, the CDC reports that antigen tests have a sensitivity of [50-70%](https://www.cdc.gov/flu/professionals/diagnosis/clinician_guidance_ridt.htm). This is much lower than PCR tests, which have a sensitivity as high at [98%](https://www.medrxiv.org/content/10.1101/2020.05.26.20112565v1.full.pdf).

Some doctors view the low sensitivity as an insurmountable hurdle. “It won’t work”, [said](https://www.technologyreview.com/2020/04/24/1000486/antigen-testing-could-faster-cheaper-diagnose-covid-19-coronavirus/) one doctor. “A misdiagnosis is worse than no diagnosis”, [said](https://www.sciencemag.org/news/2020/05/coronavirus-antigen-tests-quick-and-cheap-too-often-wrong) another.

I am not sure I agree, at least when it comes to mass testing of asymptomatic individuals. In this blog post I will show, via both simulations and analytical arguments, that the low sensitivity of antigen tests is a much better problem to have than the long delay of a PCR test. 

### Simulations

To test this hypothesis, I built an [agent-based SIR model](https://github.com/csaid/covid_model_with_testing/blob/master/SIR%20model%20with%20testing.ipynb) that includes testing and quarantining. I make the following key assumptions:

* Population of 200 students
* Using a rotation system, each student is tested once every K days. 
* Students go into a 14 day quarantine if they get positive test results.
* In addition to intra-school infections, each student has a chance of getting infected by someone outside the population.

I [simulated](https://github.com/csaid/covid_model_with_testing/blob/master/SIR%20model%20with%20testing.ipynb) two different scenarios.

* Regular PCR testing (sensitivity of 98%, testing delay of 5 days)
* Regular antigen testing (sensitivity of 75%, no testing delay)

For comparison, I also modeled a “best case scenario”, where no intra-school infections occur, and a “worst case scenario”, where there is no testing or quarantining at all.

Assuming that each student is tested once every 4 days, I find that the cumulative number of infections is far lower if we do regular antigen testing than if we do regular PCR testing. 

<div class="wrapper">
  <img src='/assets/2020_antigen_testing/fig_test_every_4_days.png' class="inner" style="position:relative border:#222 2px solid; max-width:95%;" >
</div><br>


If we change the frequency of testing to once every day, antigen testing is able to achieve close to best case results, with almost no intra-school infections. Meanwhile, daily PCR testing still results in a large number of infections.


<div class="wrapper">
  <img src='/assets/2020_antigen_testing/fig_test_every_1_day.png' class="inner" style="position:relative border:#222 2px solid; max-width:95%;" >
</div><br>


This seems very important. _Not only are antigen tests cheaper than PCR, they are also more effective in a mass testing regime._ Further simulations in my [notebook](https://github.com/csaid/covid_model_with_testing/blob/master/SIR%20model%20with%20testing.ipynb) show that the superiority of antigen tests is maintained even for much lower sensitivities.

### Why is low sensitivity better than slowness?

Maybe you don’t believe my simulations, so here’s an analytical argument instead. Imagine you test people every day using a 75% sensitive antigen test. If someone is infected, they will probably find out immediately. There is a smallish chance they will first find out after a day, and a very small chance it will take longer than that. On average they will find out after 0.33 days, which is much faster than a typical PCR test. In general, for a sensitivity of $$ s $$, daily antigen test participants will find out after an average of $$ d $$ days, where $$ d $$ is defined as:

$$ d = \sum_{t=0}^{\infty}{(1-s)^t s t} $$

which converges to 

$$ d = \frac{1-s}{s} $$

For any reasonable value of $$ s $$, $$ d $$ will be quite low. Here's a striking example. If you test people every day, a test with 50% sensitivity and no delay is equivalent to a test with 100% sensitivity and a 1-day delay!

This analysis assumes that $$ s $$ is fixed across time and individuals, an assumption I'll address in the FAQ, below.

### FAQ:

**Q:** _What is your main conclusion from this analysis?_
<br> **A:** For mass testing of asymptomatic individuals in schools and offices, antigen tests are cheaper and more effective than PCR tests.

**Q:** _Have other people come to this conclusion?_
<br> **A:** Yes. After writing this blog post, I came across two recent pre-prints. 
* [Larremore et al.](https://www.medrxiv.org/content/10.1101/2020.06.22.20136309v2.full.pdf) shows that sensitivity is less important than testing delays. It comes with a wonderful [interactive calculator](https://larremorelab.github.io/covid-calculator3).
* [Paltiel et al.](https://www.medrxiv.org/content/10.1101/2020.07.06.20147702v1.full.pdf) show that sensitivity is less important than test frequency.

**Q:** _Are you arguing that antigen tests should always replace PCR?_
<br> **A:** No, I’m only suggesting it for mass testing of asymptomatic individuals. If a symptomatic person comes into a doctor’s office a PCR test would make sense, since sensitivity becomes more important and cost becomes less important.

**Q:** _What if the antigen test has a much lower sensitivity for certain individuals, who will therefore remain undetected?_
<br> **A:** Low sensitivity for these individuals is [likely due to low viral load](https://www.sciencedirect.com/science/article/abs/pii/S138665321000106X), which most researchers believe means they will not be very infectious. That said, if would be really good if we could get more research on this, since it's an important question.

**Q:** _What if the sensitivity of the antigen test is especially low at the start of infection?_
<br> **A:** Another good question. As with the previous answer, this period of low sensitivity would likely be to due low viral load, when the individual is less infectious. Furthermore, the period where PCR tests can detect the virus while antigen tests cannot is [probably only 24 hours](https://www.medrxiv.org/content/10.1101/2020.06.22.20136309v2.full.pdf). But _even if_ we assume the individual is just as infectious during this early period, the antigen test still remains superior. To estimate this, I ran simulations where antigen tests were completely ineffective for the first two days of infection, a conservative assumption. Even with this conservative constraint, and even assuming the individuals are fully infectious during this period, antigen tests still outperformed PCR.

<div class="wrapper">
  <img src='/assets/2020_antigen_testing/fig_test_every_1_day_delayed_detection.png' class="inner" style="position:relative border:#222 2px solid; max-width:95%;" >
</div><br>

**Q:** _What if the false negatives cause riskier behavior?_
<br> **A:** It's possible that people might exhibit more risk after a false negative antigen test than after taking a PCR test and having to wait. However, this risk can be minimized by (a) educating people about sensitivity and (b) pairing the testing regime with other measures like masks and social distancing.

**Q:** _Do antigen tests require those invasive pharyngeal swabs that go deep up your nose?_
<br> **A:** Not necessarily. While [Quidel](https://www.nytimes.com/2020/05/09/health/antigen-testing-fda-coronavirus.html) and [Becton Dickinson](https://www.nytimes.com/reuters/2020/07/06/us/06reuters-health-coronavirus-becton-dickinson.html) are producing pharyngeal swab antigen tests, another company called [OraSure](https://www.inquirer.com/news/spit-test-covid-coronavirus-orasure-fda-hiv--20200706.html) is producing a saliva test. Will a saliva test be as accurate as a pharyngeal swab test? The evidence is mixed, with some studies suggesting it will be less accurate and others suggesting it will be more accurate ([1](https://www.medrxiv.org/content/10.1101/2020.04.16.20067835v1), [2](https://www.medrxiv.org/content/10.1101/2020.05.26.20112565v1.full.pdf), [3](https://jcm.asm.org/content/jcm/55/1/226.full.pdf)).

**Q:** _I heard that [SONA](https://m.canadianinsider.com/sona-nanotech-announces-validation-results-for-its-covid-19-antigen-test) has an antigen test with 96% sensitivity. This sounds super promising!_
<br> **A:** This statistic is a bit misleading. The positive samples were generated in a laboratory. When antigen tests are done in real world clinical settings, the sensitivity is [lower](https://www.technologyreview.com/2020/04/24/1000486/antigen-testing-could-faster-cheaper-diagnose-covid-19-coronavirus/).

**Q:** _Will we ever get a test that is both fast and sensitive?_
<br> **A:** The New York Times had a great [article](https://www.nytimes.com/2020/07/06/health/fast-coronavirus-tests.html) last week on new types of point-of-care tests that are being developed, including [HP-LAMP](https://www.medrxiv.org/content/10.1101/2020.06.13.20129841v1.full.pdf) and [SHINE](https://www.biorxiv.org/content/10.1101/2020.05.28.119131v1.full.pdf). These seem promising, although they are in an earlier stage of development than antigen tests, which already have several companies producing tests.

---
A special thanks to [Antonio Regalado](https://twitter.com/antonioregalado) and [Alan Wells](https://path.upmc.edu/personnel/faculty/Wells.htm) for help answering some questions during my research.