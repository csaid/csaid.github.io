---
author: csaid81
comments: true
date: 2013-10-12 20:59:14+00:00
layout: post
slug: my-insight-data-science-project
title: My Insight Data Science Project
wordpress_id: 206
disqus_identifier: 206 http://filedrawer.wordpress.com/?p=206
---

I just finished an excellent fellowship at [Insight Data Science](http://insightdatascience.com/). During our first few weeks there, each of us designed a website to demo at Insight’s sponsor companies. My website is called [DealSpotter](http://www.dealspotter.info).

This all started earlier this summer when I went to Craigslist to find a used car. There were lots of good deals on Craigslist, but it took way too long to find them. When I searched for a particular model, I got hundreds of hits, but only a few of the hits included the mileage in the posting title. Since I needed the mileage to know whether I was getting a good deal, I had to click on each of the hundreds of listings. Pretty time consuming.


[{% include image_with_border.html url="/assets/listings.png" %}](http://www.dealspotter.info)

A larger problem was that even if I clicked on every post, I didn’t always have a sense for what was the best deal. For example, if I had $3,000, was it better to spend it on a 2001 model with 100K miles, or a 2003 model with 140K miles?

[_DealSpotter_](http://www.dealspotter.info) is a proof-of-concept website that shows how these problems could be solved. DealSpotter grabs all the Craigslist car postings in the San Francisco Bay Area and automatically shows you the best deals. It knows how much each car should be priced, based on the model, year, and mileage. Cars that are priced lower than DealSpotter expects them to be are shown at the top of the list. DealSpotter also presents the same information in a visual format called “Graph” mode, where the best deals are highlighted in blue.


[{% include image_with_border.html url="/assets/full.png" %}](http://www.dealspotter.info)


To determine how much each car should be priced, DealSpotter doesn’t use Kelley Blue Book, which tends to overprice cars, especially newer models. Instead, DealSpotter builds its own pricing model based on the actual Craigslist market. In particular, it uses a [Random Forest](http://en.wikipedia.org/wiki/Random_forest) pricing model because, unlike smooth parametric models, Random Forests are able to detect sharp discontinuities in prices that may be caused by factors such as manufacturer design overhauls.

By selecting cars that are priced much lower than would be expected based on year and mileage, DealSpotter picks out some incredible deals, as well as the occasional clunker with an accident history. A more elaborate service might find a way to filter for accident history, but for now DealSpotter remains useful because it greatly narrows down the scope of the search for users. Once users are dealing with a handful of posts, they can easily inspect the text of the ad to determine which cars are good deals, and which have a history of accidents.

If you are in the San Francisco Bay Area and are looking for a used car, you should definitely check out my website _right now_. Many cars are underpriced by thousands of dollars. In the future though, I won’t be updating the listings, which will soon become outdated. Craigslist has a [history of suing](http://news.cnet.com/8301-1023_3-57479344-93/craigslist-sues-padmapper-for-mass-harvesting-listings/) other services that try to improve on how their data is presented. Craigslist’s litigiousness is understandable -- they curated the data after all. But it apparently has also [stifled innovation](http://bits.blogs.nytimes.com/2012/07/29/when-craigslist-blocks-innovations-disruptions/?_r=0). Craigslist users spend many hours of their time clicking on blue links because the website’s search and UI tools are still stuck in the 90’s. Users are also at higher risk of scams because there is no reputation system. Normally, issues like this would put a company out of business, but a combination of lawsuits and network lock-in effects have kept Craigslist at the top of classifieds services. Hopefully, we will one day get a better Craigslist. In the meantime, if you want to find an incredible deal on a car while the postings are still fresh, you should do so [now](http://www.dealspotter.info).
