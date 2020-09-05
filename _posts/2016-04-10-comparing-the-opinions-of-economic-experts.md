---
layout: post
title: Comparing the opinions of economic experts and the general public
description: Visualizationing the data in Sapienza and Zingales (2013)
---

<meta charset="utf-8">

<style>

.axis path,
.axis line {
  fill: none; /* chance go none */
  stroke: #000;
  shape-rendering: crispEdges;
}

.axis text {
  font: 12px sans-serif;
}

.survey_dot {
  stroke: #fff;
  fill: #f58032;
}

.tooltip {
  position: absolute;
  text-align: left;
  width: 150px;
  padding: 8px;
  margin-top: -20px;
  font: 10px sans-serif;
  background: rgba(200, 200, 200, 0.9);
  pointer-events: none;
}

</style>


Last week on Marginal Revolution, there was a link to a wonderful paper comparing the policy opinions of economic experts to those of the general public. The [paper](http://faculty.chicagobooth.edu/luigi.zingales/papers/research/economic-experts-vs-average-americans.pdf), by [Paola Sapienza](http://www.kellogg.northwestern.edu/faculty/directory/sapienza_paola.aspx) and [Luigi Zingales](http://www.chicagobooth.edu/faculty/directory/z/luigi-zingales), found some pretty significant discrepancies between the two groups. The authors attributed this difference to the degree of trust each group put in the implicit assumptions embedded into the economists’ answers. It's an excellent and fascinating read. However, like pretty much all academic economics papers, it displays its data in cumbersome text tables rather than figures. In this blog post, I've created some figures from the data that I think are easier to read than tables.

For each policy question, the paper provides two numbers. One is the percentage of economic experts who agree with the policy position. The second is the percentage of general public respondents who agree with the policy position. This sounds like a good opportunity for a scatter plot:

<!-- The tooltip has absolute positioning, which means it is positioned
"relative" to any parent it has who has either absolute or relative positioning.
The #econ_scatter parent would by default be static, so I have to change it to
relative -->
<div class="wrapper">
  <div id="econ_scatter" class="inner" style="position:relative"></div>
</div>

There's a light negative correlation (*r* = -0.47) between how much economic experts and the general public agree with these positions. As shown in the bottom right, the vast majority of economists prefer a carbon tax to car emissions standards, whereas the vast majority of the general public prefers the reverse. As shown in the top left, the vast majority of the general public believes that requiring the government to "Buy American" is an effective way to improve manufacturing employment, whereas the majority of economists do not. For the exact wording of the questions, see the [Appendix](http://docplayer.net/9302120-Economic-experts-vs-average-americans-online-appendix.html) to the original paper.

Another way to visualize the same data is with a [slopegraph](http://charliepark.org/slopegraphs/). Below, the left column shows all the policy positions ranked by agreement from economic experts. The right column shows the same policy positions ranked by agreement from the general public. This type of plot vividly shows how unanimous the experts are on a few beliefs: It's very hard to predict the stock market, we're on the left side of the Laffer Curve, and the US economy is fiscally unsustainable without healthcare cuts or taxes hikes.

<a href="http://chris-said.io/assets/fig_econ_poll.png"><img src="/assets/fig_econ_poll.png"></a>


Slopegraphs are useful in this context because they create less text overlap than scatter plots. With the scatter plot above, I had to use interactive mouseover events to selectively show the text for individual data points. This wouldn't be possible in a publication, since most economics journals still require static PDFs.

Even with a slopegraph, there is still some risk of overlapping text. To keep this from happening, I added some light repulsion to the data points.

For those interested, the experts in this dataset come from the [Economic Expert Panel](http://www.igmchicago.org/igm-economic-experts-panel), a diverse set of economists comprising Democrats, Republicans, and Independents. This panel is the same panel that generates the data used in my [Which Famous Economist](http://whichfamouseconomistareyoumostsimilarto.com/) website.

\[Python [code](https://gist.github.com/csaid/21677bb64c1579f9e9d4852529331ac2) for the slope graph. Scatter plot is in page source.\]


<script>
<!-- Example based on http://bl.ocks.org/weiglemc/6185069 -->


var survey_results = [
  {
    "issue": "Moving education funding to school vouchers would benefit students",
    "public_agreement": 56.2,
    "expert_agreement": 51.4
  },
  {
    "issue": "Benefits of automakers bailouts will exceed their cost",
    "public_agreement": 51.9,
    "expert_agreement": 57.5
  },
  {
    "issue": "To reduce student loan risk, link college eligibility to performance",
    "public_agreement": 61.0,
    "expert_agreement": 69.7
  },
  {
    "issue": "2009 Stimulus: benefits will exceed its costs",
    "public_agreement": 43.4,
    "expert_agreement": 52.7
  },
  {
    "issue": "Large banks are big mostly for efficiency gains, not for political power",
    "public_agreement": 39.4,
    "expert_agreement": 17.9
  },
  {
    "issue": "CEOs are paid more than the value they add to firms",
    "public_agreement": 66.8,
    "expert_agreement": 39.3
  },
  {
    "issue": "2010 unemployment rate was lower thanks to automaker bailout",
    "public_agreement": 54.8,
    "expert_agreement": 84.8
  },
  {
    "issue": "2008 bank bailouts: benefits outweighed costs",
    "public_agreement": 38.7,
    "expert_agreement": 69.7
  },
  {
    "issue": "Raising taxes on the rich would increase tax revenue",
    "public_agreement": 66.3,
    "expert_agreement": 97.4
  },
  {
    "issue": "Large banks would be smaller without government support",
    "public_agreement": 65.2,
    "expert_agreement": 33.3
  },
  {
    "issue": "Fannie & Freddie do not rebate subsidies through lower interest rates",
    "public_agreement": 66.7,
    "expert_agreement": 31.4
  },
  {
    "issue": "Changes in US gasoline prices are mainly due to market factors",
    "public_agreement": 54.3,
    "expert_agreement": 92.3
  },
  {
    "issue": "It is hard to predict stock prices",
    "public_agreement": 55.2,
    "expert_agreement": 100.0
  },
  {
    "issue": "2009 ARRA lowered unemployment rate",
    "public_agreement": 45.6,
    "expert_agreement": 91.6
  },
  {
    "issue": "NAFTA increased welfare",
    "public_agreement": 46.1,
    "expert_agreement": 94.5
  },
  {
    "issue": "Eliminating mortgage deduction improves individual finance efficiency",
    "public_agreement": 35.6,
    "expert_agreement": 89.4
  },
  {
    "issue": "\"Buy American\" has significant impact on manufacturing employment",
    "public_agreement": 75.6,
    "expert_agreement": 11.4
  },
  {
    "issue": "US economy is sustainable w/o healthcare cuts or tax hikes",
    "public_agreement": 67.6,
    "expert_agreement": 0.00
  },
  {
    "issue": "A carbon tax is more efficient than car emission standards",
    "public_agreement": 22.5,
    "expert_agreement": 92.5
  }
  ]

if( /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ) {
  var full_width = 400
  var full_height = 300;
  var r = 10
} else {
  var full_width = 550
  var full_height = 400;
  var r = 12
}

var margin = {top: 30, right: 20, bottom: 50, left: 50},
    width = full_width - margin.left - margin.right,
    econ_survey_height = full_height - margin.top - margin.bottom;

/*
 * value accessor - returns the value to encode for a given data object.
 * scale - maps value to a visual display encoding, such as a pixel position.
 * map function - maps from data value to display value
 * axis - sets up axis
 */

// setup x
var xValue = function(d) { return d.expert_agreement;}, // data -> value
    xScale = d3.scaleLinear().range([0, width]), // value -> display
    xMap = function(d) { return xScale(xValue(d));}, // data -> display
    xAxisEconSurvey = d3.axisBottom(xScale);

// setup y
var yValue = function(d) { return d.public_agreement;}, // data -> value
    yScale = d3.scaleLinear().range([econ_survey_height, 0]), // value -> display
    yMap = function(d) { return yScale(yValue(d));}, // data -> display
    yAxisEconSurvey = d3.axisLeft(yScale);


// add the graph canvas to the #econ_scatter of the webpage
var svg = d3.select("#econ_scatter").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", econ_survey_height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

// add the tooltip area to the webpage
var tooltip = d3.select("#econ_scatter").append("div")
    .attr("class", "tooltip")
    .style("display", "none")
    // .style("opacity", 0);


  // don't want dots overlapping axis, so add in buffer to data domain
  xScale.domain([-5, 105]);
  yScale.domain([15, 85]);

  // x-axis
  svg.append("g")
      .attr("class", "x axis")
      .attr("transform", "translate(0," + econ_survey_height + ")")
      .call(xAxisEconSurvey);

  // y-axis
  svg.append("g")
      .attr("class", "y axis")
      .call(yAxisEconSurvey);

  // x-label
  svg.append("text")
      .attr("x", width/2)
      .attr("y", econ_survey_height + 40)
      .style("text-anchor", "middle")
      .style("font-size", "14px")
      .text("Expert Agreement (%)");

  // y-label
  svg.append("text")
      .attr("transform", "rotate(-90)")
      .attr("x", -econ_survey_height/2)
      .attr("y", -45)
      .attr("dy", ".71em")
      .style("text-anchor", "middle")
      .style("font-size", "14px")
      .text("Public Agreement (%)");

  // draw dots
  svg.selectAll(".survey_dot")
      .data(survey_results)
    .enter().append("circle")
      .attr("class", "survey_dot")
      .attr("r", r)
      .attr("cx", xMap)
      .attr("cy", yMap)
      .on("mouseover", mouseover)
      .on("mousemove", mousemove)
      .on("mouseout", mouseout);


  function mouseover(d) {
    tooltip.style("display", "inline")
      .style("position", "absolute");
  }

  function mousemove(d) {
    tooltip
        .text(d.issue)
        .style("position", "absolute")
        .style("left", (xMap(d)) + "px")
        .style("top", (yMap(d)) + "px");
  }

  function mouseout(d) {
    tooltip.style("display", "none")
    .style("position", "absolute");
  }

</script>
