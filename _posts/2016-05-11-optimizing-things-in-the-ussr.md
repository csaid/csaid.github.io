---
layout: post
title: Optimizing things in the USSR
description: A review of Red Plenty, by Francis Spufford
---

<meta charset="utf-8">

<style>

.node rect {
  cursor: move;
  fill-opacity: .9;
  shape-rendering: crispEdges;
}

.node text {
  pointer-events: none;
  text-shadow: 0 1px 0 #fff;
}

.link {
  fill: none;
  stroke: #000;
  stroke-opacity: .2;
}

.link:hover {
  stroke-opacity: .5;
}

.caption {
  font-style: italic;
  font-size: 75%;
  text-align: left;
  line-height: 130%;
}

text {
  font: 10px sans-serif;
}


</style>

*[Content warning: Long and possibly very boring.]*

As a data scientist, a big part of my job involves picking metrics to optimize, and thinking about how to do things as efficiently as possible. With these types of questions on my mind, I recently discovered a totally fascinating book about about economic problems in the USSR and the team of data-driven economists and computer scientists who wanted to solve them. The book is called [*Red Plenty*](http://www.amazon.com/Red-Plenty-Francis-Spufford/dp/1555976042). It’s actually written as a novel, weirdly, but it nevertheless presents an accurate economic history of the USSR. It draws heavily on an earlier book from 1973 called [*Planning Problems in the USSR*](http://www.amazon.com/Planning-Problems-USSR-Contribution-Mathematical/dp/0521202493), which I also picked up. As I read these books, I couldn’t help but notice some parallels with planning in any modern organization. In what will be familiar to any data scientist today, the second book even includes a quote from a researcher who complained that 90% of his time was spent cleaning the data, and only 10% of his time was spent doing actual modeling!

Beyond all the interesting parallels to modern data science and operations research, these books helped me understand a lot of interesting things I previously knew very little about, such as linear programming, price equilibria, and Soviet history. This blog post is about I learned, and ends with some surprising-to-me speculation about whether the technical challenges that the brought down the Soviet Union would be as much of a problem in the future.

<h4 style="text-align: center;">Balance sheets and manual calculation: Kind of a trainwreck</h4>

The main task in the centrally planned Soviet economy was to allocate resources so that a desired assortment of goods and services was produced. Every year, certain target outputs for each good were established. Armed with estimates of the available input resources, central administrators used balance sheets to set plans for every factory, specifying exactly how much input commodities each factory would receive, and how much output it should produce. Up through the 1960s, this was always done by manual calculation. Since there were hundreds of thousands of commodities, and since the supply chains had many dependency steps, it was impossible to compute the full balance sheets for the economy. The administrators therefore decided to make some simplifying assumptions. As a result of these these simplifying assumptions, resource allocation became a bit of a trainwreck. Below are a few of the simplifications and their consequences.

* **Dimensionality reduction by removing variables.** Because there were too many commodities to track, administrators often limited their analysis to the 10,000 most important commodities in the economy. But when the production of those commodities were planned, there was often a hidden shortage of commodities whose output was not planned centrally but which were used as inputs to one of the 10,000 planned products. Factories that depended on those commodities often sat idle for months as they waited for the shortages to end.
* **Dimensionality reduction by aggregation.** Apparently, steel tubes can come in thousands of different types. They can come in different lengths, different shapes, and different compositions. To reduce the dimensionality of the problem, administrators would often track the total tonnage of a few broad classes of steel tubes in the models, rather than using a more detailed classification scheme. While their models successfully balanced the tonnage of tubes for the broad categories (the output in tons of tube-producing factories matched the input requirements in tons of tube-consuming factories), there were constant surpluses of some specific types of tubes, and shortages of other specific types of tubes. In particular, since tonnage was used as a metric, tube-producing factories were overly incentivized to make easy-to-produce thick tubes. As a result, thin tubes were always in short supply.
* **Propagating adjustments only a few degrees back.** Let’s say that during balance calculations, the administrators realized they needed to bump up the target output of one commodity. If they did that, it was also necessary to bump up the output targets of commodities that were input into the target commodity. But if they did *that*, they also needed to bump up the output targets of commodities that fed into those commodities, and so on! This involved a crazy amount of extra hand calculations every time they needed make an adjustment. To simplify things, the administrators typically made adjustments to the first-order suppliers, without making the necessary adjustments to the suppliers of the suppliers. This of course led to critical shortages of input commodities, which again led to idle factories.

<!-- The tooltip has absolute positioning, which means it is positioned
"relative" to any parent it has who has either absolute or relative positioning.
The #econ_scatter parent would by default be static, so I have to change it to
relative -->
<div class="wrapper">
  <div id="chart" class="inner" style="position:relative"></div>
  <!-- <div id="econ_scatter" class="inner" style="position:relative"></div> -->
  <div class="caption">
<strong>Figure 1.</strong> Some example inputs and outputs in the Soviet economy in 1951, described in units of weight. This summary shows an extreme dimensionality reduction, more extreme than was ever used in planning. In this diagram, most commodities are excluded and each displayed commodity collapses across multiple different product types. Multiple steps in the supply chain are collapsed into a single step. (Source: <a href = "http://www.foia.cia.gov/sites/default/files/document_conversions/89801/DOC_0000380738.pdf">CIA</a>)
  </div>
</div>

<br>Even if the administrators could get the accounting correct, which they couldn't, their attempts to allocate resources would still be far from optimal. In the steel industry, for example, some factories were better at producing some types of tubes whereas others were better at producing other types of tubes. Since there were thousands of different factories and tube types, it was non-trivial to decide how to best distribute resources and output requirements, and it was not immediately obvious which factories should be expanded and which should be closed down.

<h4 style="text-align: center;">Supply chain optimizations</h4>

In the late 1960’s, a group of economists and computer scientists known as the “optimal planners” began to push for a better way of doing things. The group argued that a technique called [linear programming](https://www.math.ucla.edu/~tom/LP.pdf), invented by [Leonid Kantorovich](https://en.wikipedia.org/wiki/Leonid_Kantorovich), could optimally solve the problems with the supply chain. At a minimum, since the process could be computerized, it would be possible to perform more detailed calculations than could be done by hand, with less dimensionality reduction. But more importantly, linear programming allowed you to optimize arbitrary objective functions given certain constraints. In the case of the supply chain, it showed you how to efficiently allocate resources, identifying efficient factories that should get more input commodities, and inefficient factories that should be shut down.


<div class="wrapper">
  <img src='/assets/kantorovich.jpg' height="300" class="inner" style="position:relative">
  <div class="caption">
    <strong>Figure 2.</strong> Leonid Kantorovich, inventor of linear programming and winner of the 1975 Nobel Prize in Economics.
  </div>
</div>

<br>The optimal planners had some success here. For example, in the steel industry, about 60,000 consumers requested 10,000 different types of products from 500 producers. The producers were not equally efficient in their production. Some producers were efficient for some types of steel products, but less efficient for other types of steel products. Given the total amount of each product requested, and given the constraints of how much each factory can produce, the goal was decide how much each factory should produce of each type of product. If we simplify the problem by just asking how much each factory should produce without considering how the products will be distributed to the consuming factories, this becomes a straightforward application of the [Optimal Assignment Problem](https://www.math.ucla.edu/~tom/LP.pdf), a well-studied example in linear programming. If we additionally want to optimize distribution, taking into account the distance-dependent costs of shipments from one factory to another, the problem becomes more complicated but is still doable. The problem becomes similar to the [Transportation Problem](https://www.math.ucla.edu/~tom/LP.pdf), another well-studied example in linear programming, but in this case generalized to multiple commodities instead of just one.

<div class="wrapper">
  <img src='/assets/steel_tubes.jpg' height="200" class="inner" style="position:relative">
</div>

By introducing linear programming, the optimal planners were modestly successful at improving the efficiency of some industries, but their effect was limited. First, political considerations prevented many of the recommendations surfaced by the model from being implemented. Cement factories that were known to be too inefficient or too far away from consumers were allowed to remain open even though the optimal solution recommended that they be closed. Second, since the planners were only allowed to work in certain narrow parts of the economy, they never had an opportunity to propagate their recommendations back in the supply chain, although one could imagine extending the models to do so. Third, and perhaps most importantly, the value of each commodity was set by old-school administrators in an unprincipled way, and so the optimal planners were forced to optimize objective functions that didn't even make sense.

<h4 style="text-align: center;">Ideas about optimizing the entire economy</h4>

While the optimal planners were able to improve the efficiency of a few industries, they had more ambitious plans. They believed they could use linear programming to optimize the entire economy and outperform capitalist societies. Doing so involved more than just scaling out the supply chain optimizations adopted by certain industries. It involved shadow prices and interest rates, and a few other things I’ll admit I don’t totally understand. But while I don’t really understand the implementation, I feel like the broader goal of the planners is easier to understand and explain:

Basically, in a completely free market, at least under [certain assumptions](https://en.wikipedia.org/wiki/Arrow%E2%80%93Debreu_model), prices are supposed to converge to what’s called a General Equilibrium. The equilibrium prices have a some nice properties. They balance aggregate supply and demand, so that no commodities are in shortage or surplus. They are also [Pareto efficient](https://en.wikipedia.org/wiki/Pareto_efficiency), which means that nobody in the economy can be made better off without making someone else worse off.

The optimal planners thought that they could do better. In particular, they pointed to two problems with capitalism: First, prices in a capitalist society were determined by individual agents using trial and error to guess the best price. Surely these agents, who had imperfect information, were not picking the exactly optimal prices. In contrast, a central planner using optimal computerized methods could pick prices that hit the equilibrium more exactly. Second, and more importantly, capitalism targeted an objective function that — while Pareto efficient — was not socially optimal. Because of huge differences in wealth, some people were able to obtain far more goods and services than other people. The optimal planners proposed using linear programming to optimize an objective function that would be more socially optimal. For example, it could aim to distribute goods more equitably. It could prioritize certain socially valuable goods (e.g. books) over socially destructive goods (e.g. alcohol). It could prioritize sectors that provide benefits over longer time horizons (e.g. heavy industry). And it could include constraints to ensure full employment.

<h4 style="text-align: center;">What happened</h4>
None of this ever really happened. The ambitious ideas of the optimal planners were never adopted, and by the 1970s it was clear that living standards in the USSR were falling further behind those of the West. Perhaps things would have been better if the optimal planners got their way, but it seems like the consensus is that their plans would have failed even if they were implemented. Below are some of the main problems that would have been encountered.

* **Computational complexity.** As described in a [wonderful blog post by Cosma Shalizi](http://crookedtimber.org/2012/05/30/in-soviet-union-optimization-problem-solves-you/), the number of calculations needed to solve a linear programming problem is: $$ (m+n)^{3/2} n^2 log(1/h) $$, where $$ n $$ is the number of products, $$ m $$ is the number of constraints, and $$ h $$ is how much error you are willing to tolerate. Since the number of products, $$ n $$, was in the millions, and since the complexity was proportional to $$ n^{3.5} $$, it would have been practically impossible for the Soviets to compute a solution to their planning problem with sufficient detail (although see below). Any attempt to reduce the dimensionality would lead to the same perverse incentives and shortages that bedeviled earlier systems driven by hand calculations.
* **Data quality.** The optimal planners thought that optimal computer methods could find prices that more exactly approximated equilibrium than could be done in a market economy, where fallible human actors guessed at prices by trial and error. The reality, however, would have been the exact opposite. Individual actors in a market economy understand their local needs and constraints pretty well, whereas central planners have basically no idea what’s going on. For example, central planners don’t have good information on when a factory fails to receive a shipment and they don’t have an accurate sense for how much more efficient some devices are than others. Even worse, in order to obtain more resources, factory managers in the USSR routinely *lied* to the central planners about their production capabilities. The situation became so bad that, [according to](https://books.google.com/books?id=Jlmm9GZqxkoC&printsec=frontcover#v=onepage&q&f=false) [one of the deep state secrets](http://crookedtimber.org/2012/05/30/in-soviet-union-optimization-problem-solves-you/#comment-416126) of the USSR, central planners preferred to use the CIA’s analyses of certain Russian commodities rather than reports from local Party bosses!   This is especially crazy if you consider that the CIA described its own data as being of [“debilitatingly”](http://www.foia.cia.gov/sites/default/files/document_conversions/89801/DOC_0000380738.pdf) poor quality.
* **Nonlinearities.** The optimal planners assumed linearity, such that the cost for a factory producing its 1000th widget was assumed to be the same as the cost for producing its first widget. In the real world, this is obviously false, as there are increasing returns to scale. It’s possible to model increasing returns to scale, but it becomes harder to solve computationally.
* **Choosing an objective function.** Choosing what the society should value is really a political problem, and Cosma Shalizi does a very [nice job](http://crookedtimber.org/2012/05/30/in-soviet-union-optimization-problem-solves-you/) describing why it would be so hard to come to agreement.
* **Incentives for innovation.** The central planners couldn’t determine resource allocation for products that didn’t exist yet, and more importantly neither they nor the factories had much incentive to invent new products. That's why the Soviet Union remained so focused on the steel/coal/cement economy while Western nations shifted their focus to plastics and microelectronics.
* **Political resistance.** As described in a previous example, the model-based recommendations to shut down certain factories were ignored for political reasons. It is likely that many recommendations for the broader economy would have been ignored as well. For example, if a computer recommended that the price of heating oil should be doubled in the winter, how many politicians would let that happen?

<h4 style="text-align: center;">Could this work in the future?</h4>

Had the optimal planners’ ideas been adopted at the time, they would have failed. But what about the future? In a hundred years, could we have the technical capability to pull off a totally planned economy? I did some poking around the internet and found, somewhat to my surprise, that the answer is actually… *maybe*. It turns out that two of the most serious problems with central planning could have technological solutions that may seem far-fetched but are perhaps not impossible:

Let’s start with **computational complexity**. As described above and in Cosma Shalizi's post, the number of steps required to solve a linear programming problem with $$ n $$ products and $$ m $$ constraints is proportional $$ (m+n)^{3/2} n^2 $$, which is prohibitively high for a large economy. I can’t believe I’m saying this, but I believe that [Cosma Shalizi](http://crookedtimber.org/2012/05/30/in-soviet-union-optimization-problem-solves-you/#comment-415949) [is](http://crookedtimber.org/2012/05/30/in-soviet-union-optimization-problem-solves-you/#comment-416096) [wrong](http://crookedtimber.org/2012/05/30/in-soviet-union-optimization-problem-solves-you/#comment-416201). This is because the actual input-output matrix of an economy is actually pretty sparse. Not every product depends on every other product as input. In 1993, Paul Cockshott and Allin Cottrell published a book called [*Towards a New Socialism*](http://ricardo.ecn.wfu.edu/~cottrell/socialism_book/new_socialism.pdf) in which they described a way to exploit the sparsity of the input-output matrix, bringing the computational complexity down to $$ m \times n $$. Unfortunately for Communism, the book was published in 1993.

<div class="wrapper">
  <img src='/assets/towards_a_new_socialism.gif' height="300" class="inner" style="position:relative">
</div>


As described earlier, the second serious issue with a centrally planned economy was **data quality**: Central planners’ knowledge about the input requirements and output capabilities of individual factories was simply not as good as the people actually working in the factory. While this was certainly the case in the Soviet Union, one can’t help but wonder about technological improvements in supply chain management. Imagine if every product had a GPS device to track its location, with other sensors and cameras to determine product quality. Already Amazon is moving in that direction for pretty much all consumer goods, and one could imagine a world where demand could be measured with the [Internet of Things](https://en.wikipedia.org/wiki/Internet_of_Things). Whether a government would be able to harness this data as competently as Amazon is doubtful, and it’s obviously worth asking whether we would ever want a government to be using that type of data. But from a technical point of view it’s interesting to think about how the data quality issues that destroyed the USSR could be much less serious in the future.

All that being said, it’s still unclear to me how an objective function could be chosen in way that would democratically satisfy people, how innovation could be incentivized, or how political freedoms could be preserved. In terms of freedom, things were obviously not-so-hot in the Soviet Union. If you’d like to read more about this, you should definitely check out [Red Plenty](http://www.amazon.com/Red-Plenty-Francis-Spufford/dp/1555976042). It was one of the weirdest and most interesting books I have read.

<div class="wrapper">
  <img src='/assets/red_plenty.jpeg' height="300" class="inner" style="position:relative">
</div>


<script src="/scripts/sankey.js"></script>
<script>

// Names to Index
var n2i = {
'petro': 0,
'steel': 1,
'copper': 2,
'aluminum': 3,
'ammonia': 4,
'food': 5,
'textiles': 6,
'logging': 7,
'automotive': 8
}

var commodities = {
  "nodes":[
    {'name': 'Petroleum residuals'},
    {'name': 'Crude Steel'},
    {'name': 'Copper'},
    {'name': 'Aluminum'},
    {'name': 'Ammonia'},
    {'name': 'Agriculture and Food'},
    {'name': 'Textiles and Apparel'},
    {'name': 'Logging and Paper Products'},
    {'name': 'Automotive Equipment'}
  ],
  'links':[
    {'source':n2i['petro'],'target':n2i['food'],'value':510},
    {'source':n2i['steel'],'target':n2i['food'],'value':80},
    {'source':n2i['ammonia'],'target':n2i['food'],'value':250},
    {'source':n2i['steel'],'target':n2i['textiles'],'value':10},
    {'source':n2i['ammonia'],'target':n2i['textiles'],'value':10},
    {'source':n2i['petro'],'target':n2i['logging'],'value':30},
    {'source':n2i['steel'],'target':n2i['logging'],'value':40},
    {'source':n2i['ammonia'],'target':n2i['logging'],'value':5},
    {'source':n2i['petro'],'target':n2i['automotive'],'value':60},
    {'source':n2i['steel'],'target':n2i['automotive'],'value':1800},
    {'source':n2i['copper'],'target':n2i['automotive'],'value':14},
    {'source':n2i['aluminum'],'target':n2i['automotive'],'value':3}
  ]
}

if( /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ) {
  var full_width = 400
  var full_height = 400;
} else {
  var full_width = 620
  var full_height = 500;
}

var margin = {top: 20, right: 40, bottom: 20, left: 40},
    width = full_width - margin.left - margin.right,
    height = full_height - margin.top - margin.bottom;

var formatNumber = d3.format(",.0f"),
    format = function(d) { return formatNumber(d) + " Th MT"; },
    color = d3.scale.category20();

var svg = d3.select("#chart").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");


var sankey = d3.sankey()
    .nodeWidth(15)
    .nodePadding(10)
    .size([width, height]);

var path = sankey.link();

  sankey
      .nodes(commodities.nodes)
      .links(commodities.links)
      .layout(32);

  var link = svg.append("g").selectAll(".link")
      .data(commodities.links)
    .enter().append("path")
      .attr("class", "link")
      .attr("d", path)
      .style("stroke-width", function(d) { return Math.max(1, d.dy); })
      .sort(function(a, b) { return b.dy - a.dy; });

  link.append("title")
      .text(function(d) { return d.source.name + " → " + d.target.name + "\n" + format(d.value); });

  var node = svg.append("g").selectAll(".node")
      .data(commodities.nodes)
    .enter().append("g")
      .attr("class", "node")
      .attr("transform", function(d) { return "translate(" + d.x + "," + d.y + ")"; })
    .call(d3.behavior.drag()
      .origin(function(d) { return d; })
      .on("dragstart", function() { this.parentNode.appendChild(this); })
      .on("drag", dragmove));

  node.append("rect")
      .attr("height", function(d) { return d.dy; })
      .attr("width", sankey.nodeWidth())
      .style("fill", function(d) { return d.color = color(d.name.replace(/ .*/, "")); })
      .style("stroke", function(d) { return d3.rgb(d.color).darker(2); })
    .append("title")
      .text(function(d) { return d.name + "\n" + format(d.value); });

  node.append("text")
      .attr("x", -6)
      .attr("y", function(d) { return d.dy / 2; })
      .attr("dy", ".35em")
      .attr("text-anchor", "end")
      .attr("transform", null)
      .text(function(d) { return d.name; })
    .filter(function(d) { return d.x < width / 2; })
      .attr("x", 6 + sankey.nodeWidth())
      .attr("text-anchor", "start");


  // Input label
  svg.append("text")
      .attr("transform", "rotate(-90)")
      .attr("x", -height/2)
      .attr("y", -20)
      .attr("dy", ".71em")
      .style("text-anchor", "middle")
      .style("font-size", "16px")
      .text("Inputs");

  // Output label
  svg.append("text")
      .attr("transform", "rotate(-90)")
      .attr("x", -height/2)
      .attr("y", width+10)
      .attr("dy", ".71em")
      .style("text-anchor", "middle")
      .style("font-size", "16px")
      .text("Outputs");



  function dragmove(d) {
    d3.select(this).attr("transform", "translate(" + d.x + "," + (d.y = Math.max(0, Math.min(height - d.dy, d3.event.y))) + ")");
    sankey.relayout();
    link.attr("d", path);
  }
// });
</script>
