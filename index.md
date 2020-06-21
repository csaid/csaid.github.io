---
layout: home
title: Chris Said
---

Below are links to blog posts I've written, as well as some other projects. I can also be found on [Twitter](https://twitter.com/Chris_Said), [LinkedIn](https://www.linkedin.com/in/chris-said-97986b6b/), and [Github](https://github.com/csaid).

#### Selected Blog Posts (Statistics)
* [Optimizing sample sizes in A/B testing, Part I: General summary](/2020/01/10/optimizing-sample-sizes-in-ab-testing-part-I/) ([Part II](/2020/01/10/optimizing-sample-sizes-in-ab-testing-part-II/), [Part III](/2020/01/10/optimizing-sample-sizes-in-ab-testing-part-III/)) (10 Jan 2020)
* [Variance after scaling and summing: One of the most useful facts from statistics](/2019/05/18/variance_after_scaling_and_summing/) (18 May 2019)
* [Empirical Bayes for multiple sample sizes](/2017/05/03/empirical-bayes-for-multiple-sample-sizes/) (03 May 2017)

#### Selected Blog Posts (General interest)
* [Optimizing things in the USSR](/2016/05/11/optimizing-things-in-the-ussr/) (11 May 2016)
* [Four pitfalls of hill climbing](/2016/02/28/four-pitfalls-of-hill-climbing/) (28 Feb 2016)

#### Other Projects
* [Which famous economist are you most similar to?](http://whichfamouseconomistareyoumostsimilarto.com/) (2013-2015)
* [Is my job in another state?](http://ismyjobinanotherstate.com/) (2014)

#### Full Blog Archive

{% for post in site.posts %}
  * [ {{ post.title }} ]({{ post.url }}) ({{ post.date | date_to_string }})
{% endfor %}
