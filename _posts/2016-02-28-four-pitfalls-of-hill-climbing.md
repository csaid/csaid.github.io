---
layout: post
title: Four pitfalls of hill climbing
description: An animated look at local maxima, novelty effects, differentiation, emergent maxima.
---

<meta charset="utf-8">

One of the great developments in product design has been the adoption of A/B testing. Instead of just guessing what is best for your customers, you can offer a product variant to a subset of customers and measure how well it works. While undeniably useful, A/B testing is sometimes said to encourage too much "hill climbing", an incremental and short-sighted style of product development that emphasizes easy and immediate wins.

Discussion around hill climbing can sometimes get a bit vague, so I thought I would make some animations that describe four distinct pitfalls that can emerge from an overreliance on hill climbing.

#### 1. Local maxima

If you climb hills incrementally, you may end up in a local maximum and miss out on an opportunity to land on a global maximum with much bigger reward. Concerns about local maxima are often wrapped up in critiques of *incrementalism*.

<div class="wrapper">
  <div class="inner" id="plot1"></div>
</div>

Local maxima and global maxima can be illustrated with hill diagrams like the one above. The horizontal axis represents product space collapsed into a single dimension. In reality, of course, there are many dimensions that a product could explore.

#### 2. Emergent maxima

If you run short A/B tests, or A/B tests that do not fully capture network effects, you might not realize that a change that initially seems bad may be good in the long run. This idea, which is distinct from concerns about incrementalism, can be described with a dynamic reward function animation. As before, the horizontal axis is product space. Each video frame represents a time step, and the vertical axis represents immediate, measurable reward.

<div class="wrapper">
  <div class="inner" id="plot2"></div>
</div>

When a product changes, the intial effect is negative. But eventually, customers begin to enjoy the new version, as shown by changes in the reward function. By waiting at a position that initially seemed negative, you are able to discover an emergent mountain, and receive greater reward than you would have from short-term optimization.

#### 3. Novelty effects

Short-term optimization can be bad, not only because it prevents discovery of emergent mountains, but also because some hills can be transient. One way a hill can disappear is through *novelty effects*, where a shiny new feature can be engaging to customers in the short term, but uninteresting or even negative in the long term.

<div class="wrapper">
  <div class="inner" id="plot3"></div>
</div>

#### 4. Loss of differentiation

Another way a hill can disappear is through loss of differentiation from more dominant competitors. Your product may occupy a market niche. If you try to copy your competitor, you may initially see some benefits. But at some point, your customers may leave because not much separates you from your more dominant competitor. Differentiation matters in some dimensions more than others.

<div class="wrapper">
  <span id="plot4"></span>
  <span id="plot5"></span>
</div>

You can think of an industry as a dynamic ecosystem where each company has its own reward function. When one company moves, it changes its own reward function as well as the reward functions of other companies. If this sounds like biology, you're not mistaken. The dynamics here are similar to evolutionary [fitness landscapes](http://www.randalolson.com/2014/04/17/visualizing-evolution-in-action-dynamic-fitness-landscapes/).

***

While all of the criticisms of hill climbing have obvious validity, I think it is easy for people to overreact to them. Here are some caveats in defense of hill climbing:

- The plots above probably exaggerate the magnitude and frequency with which reward functions change.
- There is huge uncertainty and disagreement about what future landscapes will look like. In most cases, it's better to explore regions that increase (rather than decrease) reward, making sure to run long term experiments when needed.
- The space is high dimensional. Even if your product is at a local maximum in one dimension, there are many other dimensions to explore and measure.
- We may overestimate the causal relationship between bold product moves and company success. Investors often observe that companies who don't make bold changes are doomed to fail. While I don't doubt that there is some causation here, I think there is also some reverse causation. Bold changes require lots of resources. Maybe it's mostly the success-bound companies who have enough resources to afford the bold changes.

Special thanks to Marika Inhoff, [John McDonnell](https://twitter.com/johnvmcdonnell) and [Alex Rosner](https://twitter.com/alexrosner) for comments on a draft of this post.
<style>

circle {
  stroke: #fff;
  stroke-width: 1.5px;
}

circle.you {
  fill: #ED2685;
}

circle.competitor {
  fill: #3AC3F2;
}

.background {
  fill: #e7e7e7;
}

.axis path,
.axis line {
  fill: none;
  stroke: #fff;
  shape-rendering: crispEdges;
}

.axis .tick:nth-child(2n) {
  stroke-opacity: 0.5;
}

.axis text {
  font: 12px sans-serif;
}

.line path {
  fill: none;
  stroke: #000;
  stroke-width: 2px;
  stroke-linecap: round;
  stroke-linejoin: round;
}

}

</style>

<script src="//d3js.org/d3.v3.min.js"></script>

<!-- I think Markdown doesn't like two script tags in a row -->
<div></div>

<script>





function makeObjectiveGraph(id, get_dot_x, objective, title, is_dynamic, company) {

  // Dot radius
  var r = 11;

  var margin = {top: 20, right: 20, bottom: 20, left: 20},
      width = 270 - margin.left - margin.right,
      height = 270 - margin.top - margin.bottom;

  var tickFormat = d3.format(".1f");

  var y = d3.scale.linear()
      .domain([0, 1])
      .range([height, 0]);

  var yAxis = d3.svg.axis()
      .scale(y)
      .orient("left")
      .tickSize(-width)
      .tickFormat(function(d, i) { return i & 1 ? null : tickFormat(d); })
      .tickPadding(8);

  var x = d3.scale.linear()
      .domain([0, 1])
      .range([0, width]);

  var xAxis = d3.svg.axis()
      .scale(x)
      .orient("bottom")
      .tickSize(-height)
      .tickFormat(function(d, i) { return i & 1 ? null : tickFormat(d); })
      .tickPadding(8);

  var path = d3.svg.line();

  var svg = d3.select("#" + id).append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
    .append("g")
      .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

  svg.append("rect")
      .attr("class", "background")
      .attr("width", width)
      .attr("height", height);

  var line = svg.append("g")
      .attr("class", "line")
    .append("path");

  var dot = svg.append("circle")
      .attr("r", r-1)
      .attr("class", company);

  // For X- and Y- labels.
  if (company == "competitor"){
    var prefix = "Competitor ";
  } else {
    var prefix = "";
  }

  // Title
  svg.append("text")
      .attr("class", "x label")
      .attr("text-anchor", "middle")
      .attr("x", width/2)
      .attr("y", -8)
      .style("font-size", "14px")
      .style("font-weight", "bold")
      .text(title);

  // X-Label
  svg.append("text")
      .attr("class", "x label")
      .attr("text-anchor", "middle")
      .attr("x", width/2)
      .attr("y", height + 14)
      .style("font-size", "12px")
      .text(prefix + "Product Space");

  // Y-Label
  svg.append("text")
      .attr("class", "y label")
      .attr("text-anchor", "middle")
      .attr("x", -width/2)
      .attr("y", -14)
      .attr("dy", ".75em")
      .attr("transform", "rotate(-90)")
      .style("font-size", "12px")
      .text(prefix + "Reward");


  if (!is_dynamic){
    line.attr("d", path(d3.range(0, 1, .002).concat(1).map(function(xo) {
      return [x(xo), y(objective(xo))];
    })));
  }


  // Provide pixel coordinates of small segment of curve.
  // Returns pixel coordinates just above segment.
  function get_adjusted_dot_coords(start_x, start_y, end_x, end_y) {
    var center_x = (start_x + end_x) / 2;
    var center_y = (start_y + end_y) / 2;
    var rise = (end_y - start_y);
    var run = (end_x - start_x);
    var k = Math.sqrt(Math.pow(r, 2)/(Math.pow(run, 2) + Math.pow(rise, 2)));
    var dx = k * rise;
    var dy = k * run;

    return [center_x + dx, center_y - dy]

  };

  // Functions x and y return pixel units.
  // Var xo is in graph coordinates from 0 to 1.
  // Var t is from 0 to 1.
  d3.timer(function(elapsed) {
    var t = (elapsed % 3000) / 3000; // will range from 0 to 1.
    var dot_xo = get_dot_x(t) // make this a function

    if (is_dynamic) {

      dot_coords = get_adjusted_dot_coords(
                                           x(dot_xo-.001),
                                           y(objective(t, dot_xo-.001)),
                                           x(dot_xo+.001),
                                           y(objective(t, dot_xo+.001))
                                           );

      // dot_coords = [x(dot_xo), y(objective(t, dot_xo))]

      dot.attr("cx", dot_coords[0]).attr("cy", dot_coords[1]);

      // dot.attr("cx", x(t)).attr("cy", y(objective(t, t))-12); // The 12 is in pixel world. Subtracting because pixel 0 is at top.

      line.attr("d", path(d3.range(0, 1, .002).concat(1).map(function(xo) {
        return [x(xo), y(objective(t, xo))];
      })));
    } else {

      dot_coords = get_adjusted_dot_coords(x(dot_xo-.001), y(objective(dot_xo-.001)), x(dot_xo+.001), y(objective(dot_xo+.001)))
      dot.attr("cx", dot_coords[0]).attr("cy", dot_coords[1]);
    }


  });

  d3.select(self.frameElement).style("height", height + margin.top + margin.bottom + "px");
}

var left_pos  = 0.20;
var mid_pos   = 0.50;
var right_pos = 0.80;
var mu = 0.25


// Provides soft trajectory, given starting and stopping times and locations.
// Generally the first agrument will be time t, but using x
// internally here so I can think about the functions visually.
var soft_motion = function(x, start_x, start_y, end_x, end_y) {
  var period = 2 * (end_x - start_x);
  var scale = (end_y - start_y)/2;
  var intercept = start_y + scale;

  if (x < start_x) { return start_y } else
  if (x < end_x) { return -scale * Math.cos(2*Math.PI/period * (x-start_x)) + intercept} else
  return end_y
};

// The horizontal position of your dot
var get_dot_x = function(t) {
  return soft_motion(t, .2, .2, .8, .8)
};

var get_dot_x_local_max = function(t) {
  return soft_motion(t, .2, .35, .8, .2)
};

// The horizontal position of your competitor's dot
var get_competitor_dot_x = function(t) {
  return 0.8;
};

// Gaussian function. For dynamics, call repeatedly with different scale values.
function gauss(x, scale, mu, sig) {
  var coef = scale / (sig* Math.sqrt(2 * Math.PI));
  var numer = Math.pow((x-mu), 2)
  var denom = 2*Math.pow(sig, 2)
  return coef * Math.exp(-numer/denom);
};


var objective1 = function(x){
  var left_scale = 0.2;
  var right_scale = 0.3;
  var sd = 0.2
  return gauss(x, left_scale, left_pos, sd) + gauss(x, right_scale, right_pos, sd)
}

makeObjectiveGraph("plot1", get_dot_x_local_max, objective1, "", false, "you")


var objective2 = function(t, x){
  var left_scale  = soft_motion(t, 0.6, 0.2, 0.95, 0.10);
  var right_scale = soft_motion(t, 0.6, 0.0, 0.95, 0.3);
  var sd = 0.2

  return gauss(x, left_scale, left_pos, sd) + gauss(x, right_scale, right_pos, sd)
};

makeObjectiveGraph("plot2", get_dot_x, objective2, "", true, "you")


var objective3 = function(t, x){
  var left_scale  = soft_motion(t, 0.6, 0.15, 0.95, 0.15);
  var right_scale = soft_motion(t, 0.6, 0.30, 0.95, 0.15);

  return gauss(x, left_scale, left_pos, .25) + gauss(x, right_scale, right_pos, .3)
};

makeObjectiveGraph("plot3", get_dot_x, objective3, "", true, "you")

var objective4 = function(t, x){
  var left_scale  = soft_motion(t, 0.6, 0.15, 0.95, 0.15);
  var right_scale = soft_motion(t, 0.6, 0.30, 0.95, 0.15);

  return gauss(x, left_scale, left_pos, .25) + gauss(x, right_scale, right_pos, .3)
};

makeObjectiveGraph("plot4", get_dot_x, objective4, "Losing differentiation", true, "you")

// Competitor
var objective5 = function(t, x){
  var left_scale  = soft_motion(t, 0.6, 0.05, 0.95, 0.07);
  var right_scale = soft_motion(t, 0.6, 0.4,  0.95, 0.50);

  return gauss(x, left_scale, left_pos, .15) + gauss(x, right_scale, right_pos, .3)
};

makeObjectiveGraph("plot5", get_competitor_dot_x, objective5, "Dominant competitor wins", true, "competitor")


</script>
