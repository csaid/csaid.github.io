---
layout: home
title: Chris Said
---

Below are links to blog posts I've written, as well as some other projects. I can also be found on [Twitter](https://twitter.com/Chris_Said), [Threads](https://www.threads.net/@chrissaid82), [Bluesky](https://bsky.app/profile/csaid.bsky.social), and [LinkedIn](https://www.linkedin.com/in/chris-said-97986b6b/).

#### Sample Projects
* [Apollo Academic Surveys](https://www.apollosurveys.org/) (2022-present). A nonprofit that aggregates the views of academic experts, making them freely available to everyone.
* [RapidTests.org](https://www.rapidtests.org/) (2020). All-volunteer group advocating for rapid tests for Covid.
* [Which famous economist are you most similar to?](http://whichfamouseconomistareyoumostsimilarto.com/) (2013-2015). Interactive economics quiz featured in the Washington Post, Wall Street Journal, and NPR.

#### Selected Blog Posts
* [Optimizing sample sizes in A/B testing, Part I: General summary](/2020/01/10/optimizing-sample-sizes-in-ab-testing-part-I/) ([Part II](/2020/01/10/optimizing-sample-sizes-in-ab-testing-part-II/), [Part III](/2020/01/10/optimizing-sample-sizes-in-ab-testing-part-III/)) (10 Jan 2020)
* [Variance after scaling and summing: One of the most useful facts from statistics](/2019/05/18/variance_after_scaling_and_summing/) (18 May 2019)
* [Empirical Bayes for multiple sample sizes](/2017/05/03/empirical-bayes-for-multiple-sample-sizes/) (03 May 2017)
* [Optimizing things in the USSR](/2016/05/11/optimizing-things-in-the-ussr/) (11 May 2016)
* [Small correlations can drive large increases in teen depression](2022/05/10/social-media-and-teen-depression/) (10 May 2022)

#### Full Blog Archive

{% for post in site.posts %}
  * [ {{ post.title }} ]({{ post.url }}) ({{ post.date | date_to_string }})
{% endfor %}
