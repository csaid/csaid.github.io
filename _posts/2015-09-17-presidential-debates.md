---
layout: post
title: The debate CNN wanted and the debate the candidates wanted
description: Some interactive graphics showing who challenged who in the Republican debate.
---

In last night's Republican presidential debate, the CNN moderators tried very hard to pit individual candidates against each other. Pretty much every question was along the lines of: "Candidate A, what do you think about Candidate B's attacks on you?"

While CNN wanted to generate controversy, the disputes it tried to initiate were not necessarily the disputes that the candidates strategically wanted to have. To get a better sense for this, I tallied up two things:

1. **Moderator Prompts**. These were episodes where the moderators prompted one candidate to challenge another.
2. **Real Challenges**. These were episodes where either (a) the candidate took the bait from a Moderator Prompt and attacked the other candidate or (b) a candidate launched an unprompted attack on another candidate. I did not include episodes where a prompted candidate acknowledged a disagreement with another candidate in a nonconfrontational way.

The results are below. In the first graph, an arrow from one candidate to another indicates that the moderator asked the first candidate to challenge the second. The next graph shows real challenges. In both graphs, the boldness of the curve indicates how many times the event occurred.

<meta charset="utf-8">

<div class="center">
  <span id="debate_graph1"></span>
  <span id="debate_graph2"></span>
</div>


Some observations:

* The graph of Moderator Prompts is much denser than the graph of Real Challenges. As was clear to anyone watching the debate, the moderators wanted to generate more controversy than the candidates wanted.
* Most of the real action in the second graph was in the Trump-Bush-Paul Triangle of Controversy.
* The moderators tried to prompt several challenges to Ben Carson but nobody took the bait. Conversely, the moderators paid relatively little attention to Rand Paul, and yet he was part of several real disputes.
* The interests of Donald Trump were pretty well aligned with the interests of CNN. Donald Trump was prompted many times and engaged in many challenges.

It's also interesting to look at how many times each of the candidates challenged Hillary Clinton, who was not on the stage. Jeb Bush mentioned her on five separate occassions, whereas Donald Trump never mentioned her at all. It's just a few data points, but it's consistent with Jeb's larger focus on the general election.

{% include image.html url="/assets/hillary.png" %}

I doubt any of this data is very predicitive of election outcomes, but it definitely seems to be related to current polling trends. Either way, it's pretty interesting to look at. I might make these plots for some of the other debates to see how these relationships change over time.

**Update:** Coincidentally, the New York Times just released a [similar analysis with similar graphics](http://www.nytimes.com/interactive/2015/09/17/us/politics/gop-debate-trump-attacks-speaking-time.html). Also, special thanks to [@trebor](http://www.twitter.com/trebor) for helping me set up mouseover-triggered transparency.

<style>

.debate_link {
  fill: none;
  stroke: #3AC3F2;
}

.debate_tooltip {
    border-radius: 5px;
    background: #ccc;
    border-color: #555;
    padding: 5px;
    font-size: 10px;
    /*width: 200px;*/
    /*height: 150px;*/
}

.debate_link.resolved {
  stroke-dasharray: 0,2 1;
}

circle {
  fill: #ED2685;
  stroke: #fff;
  stroke-width: 1.5px;
}

path.unselected {
  opacity: 0.1;
}

circle.unselected {
  fill: #ED86C5;
}


text {
  font: 10px sans-serif;
}

</style>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script>
<!-- <script type='text/javascript' src='/javascripts/jquery-2.1.4.min.js'></script>
<script type='text/javascript' src='/javascripts/jquery.tipsy.js'></script> -->
<!-- <link rel="stylesheet" href="/stylesheets/tipsy.css" type="text/css" /> -->
<script>

var mod_links = [
   {source: 0, target: 1, mentions: 3},
   {source: 0, target: 2, mentions: 1},
   {source: 0, target: 3, mentions: 1},
   {source: 0, target: 5, mentions: 1},
   {source: 0, target: 7, mentions: 1},
   {source: 1, target: 0, mentions: 3},
   {source: 1, target: 6, mentions: 2},
   {source: 1, target: 8, mentions: 2},
   {source: 1, target: 10, mentions: 2},
   {source: 2, target: 4, mentions: 1},
   {source: 2, target: 10, mentions: 1},
   {source: 3, target: 0, mentions: 2},
   {source: 3, target: 1, mentions: 1},
   {source: 4, target: 1, mentions: 3},
   {source: 4, target: 2, mentions: 1},
   {source: 4, target: 5, mentions: 1},
   {source: 5, target: 0, mentions: 1},
   {source: 5, target: 4, mentions: 1},
   {source: 5, target: 9, mentions: 1},
   {source: 6, target: 1, mentions: 1},
   {source: 7, target: 2, mentions: 1},
   {source: 8, target: 1, mentions: 1},
   {source: 8, target: 4, mentions: 2},
   {source: 8, target: 6, mentions: 1},
   {source: 9, target: 1, mentions: 1},
   {source: 9, target: 5, mentions: 3},
   {source: 9, target: 10, mentions: 1},
   {source: 10,target: 1, mentions: 4},
];

var mod_nodes = [
{idx: 0, name: 'Bush', fixed: true, x: 235.0, y: 135.0},
{idx: 1, name: 'Trump', fixed: true, x: 219.125353283, y: 189.064081746},
{idx: 2, name: 'Walker', fixed: true, x: 176.5415013, y: 225.963199535},
{idx: 3, name: 'Huckabee', fixed: true, x: 120.768516173, y: 233.982144188},
{idx: 4, name: 'Carson', fixed: true, x: 69.5139266055, y: 210.574957435},
{idx: 5, name: 'Cruz', fixed: true, x: 39.0507026386, y: 163.173255684},
{idx: 6, name: 'Rubio', fixed: true, x: 39.0507026386, y: 106.826744316},
{idx: 7, name: 'Paul', fixed: true, x: 69.5139266055, y: 59.4250425646},
{idx: 8, name: 'Christie', fixed: true, x: 120.768516173, y: 36.0178558119},
{idx: 9, name: 'Kasich', fixed: true, x: 176.5415013, y: 44.0368004645},
{idx: 10, name: 'Fiorina', fixed: true, x: 219.125353283, y: 80.9359182544},
]



var challenge_links = [
   {source: 0, target: 1, mentions: 4},
   {source: 0, target: 5, mentions: 1},
   {source: 1, target: 0, mentions: 4},
   {source: 1, target: 2, mentions: 1},
   {source: 1, target: 6, mentions: 1},
   {source: 1, target: 7, mentions: 3},
   {source: 1, target: 10, mentions: 1},
   {source: 2, target: 1, mentions: 1},
   {source: 7, target: 0, mentions: 3},
   {source: 7, target: 1, mentions: 1},
   {source: 7, target: 8, mentions: 1},
   {source: 8, target: 7, mentions: 1},
   {source: 8, target: 10, mentions: 1},
   {source: 10,target: 1, mentions: 2},
   {source: 10,target: 8, mentions: 1},
];





var challenge_nodes = [
{idx: 0, name: 'Bush', fixed: true, x: 235.0, y: 135.0},
{idx: 1, name: 'Trump', fixed: true, x: 219.125353283, y: 189.064081746},
{idx: 2, name: 'Walker', fixed: true, x: 176.5415013, y: 225.963199535},
{idx: 3, name: 'Huckabee', fixed: true, x: 120.768516173, y: 233.982144188},
{idx: 4, name: 'Carson', fixed: true, x: 69.5139266055, y: 210.574957435},
{idx: 5, name: 'Cruz', fixed: true, x: 39.0507026386, y: 163.173255684},
{idx: 6, name: 'Rubio', fixed: true, x: 39.0507026386, y: 106.826744316},
{idx: 7, name: 'Paul', fixed: true, x: 69.5139266055, y: 59.4250425646},
{idx: 8, name: 'Christie', fixed: true, x: 120.768516173, y: 36.0178558119},
{idx: 9, name: 'Kasich', fixed: true, x: 176.5415013, y: 44.0368004645},
{idx: 10, name: 'Fiorina', fixed: true, x: 219.125353283, y: 80.9359182544},
]



function makeGraph(links, nodes, id, interactionType) {

  // Use elliptical arc path segments to doubly-encode directionality.
  function tick() {
    path.attr("d", linkArc);
    circle.attr("transform", transform);
    text.attr("transform", transform);
  }

  function linkArc(d) {
    var dx = d.target.x - d.source.x,
        dy = d.target.y - d.source.y,
        dr = Math.sqrt(dx * dx + dy * dy);
    return "M" + d.source.x + "," + d.source.y + "A" + dr + "," + dr + " 0 0,1 " + d.target.x + "," + d.target.y;
  }

  function transform(d) {
    return "translate(" + d.x + "," + d.y + ")";
  }

  // Mark each node with chart id
  nodes.forEach(function(d) {d.id=id});

  // Compute the distinct nodes from the links.
  links.forEach(function(link) {
    link.source = nodes[link.source] || (nodes[link.source] = {name: link.source});
    link.target = nodes[link.target] || (nodes[link.target] = {name: link.target});
  });

  var width = 270,
      height = 280;

  var force = d3.layout.force()
      .nodes(d3.values(nodes))
      .links(links)
      .size([width, height])
      .on("tick", tick)
      .start();

  var svg = d3.select("span#" + id).append("svg")
      .attr("width", width)
      .attr("height", height);
      // .attr("class", "center");

  // Per-type markers, as they don't inherit styles.
  svg.append("defs")
      .append("marker")
      .attr("id", "marker")
      .attr("viewBox", "0 -5 10 10") // min-x, min-y, width, height
      .attr("refX", 17) // The reference point. Even though arrow is length 10, using 12 because otherwise would go to center of circle.
      .attr("refY", 0)
      .attr("markerWidth", 10)
      .attr("markerHeight", 10)
      .attr("markerUnits", "userSpaceOnUse") // makes marker size independent of stroke-width
      .attr("orient", "auto")
      .attr("fill", "#3AC3F2")
    .append("path")
      .attr("d", "M0,-5L10,0L0,5"); // Arrow definition. Start at 0,-5. Then draw line to 10, 0. Then draw line to 0, 5

  // http://stackoverflow.com/questions/10805184/d3-show-data-on-mouseover-of-circle
  // http://bl.ocks.org/biovisualize/1016860
  var tooltip = d3.select("span#" + id)
      .append("div")
      .attr("class", "debate_tooltip")
      .style("position", "absolute")
      .style("z-index", "10")
      .style("visibility", "hidden")
  ;


  var path = svg.append("g").selectAll("path")
      .data(force.links())
    .enter().append("path")
      .attr("class", "debate_link")
      .attr("stroke-width", function(d) { return d.mentions })
      .attr("marker-end", "url(#marker)") // This just say that the arrow should go at the end of the link, rather than the beginning.
      .on("mouseover", function(d){return tooltip.style("visibility", "visible").text(interactionType + ": " + d.mentions)})
      .on("mousemove", function(){return tooltip.style("top",
          (d3.event.pageY-10)+"px").style("left",(d3.event.pageX+10)+"px");})
      .on("mouseout", function(){return tooltip.style("visibility", "hidden");});

  function nodeMouseOver(node) {
    d3.selectAll("#" + node.id + " circle")
      .classed("unselected", function(d) { return d != node;});
    d3.selectAll("#" + node.id + " .debate_link")
      .classed("unselected", function(d) {return d.source != node;});
  }

  function nodeMouseOut(node) {
    d3.selectAll("#" + node.id + " circle")
      .classed("unselected", false);
    d3.selectAll("#" + node.id + " .debate_link")
      .classed("unselected", false);
  }
  // "g" is to SVG as "div" is to HTML
  var circle = svg.append("g").selectAll("circle")
      .data(force.nodes())
    .enter().append("circle")
      .attr("r", 8)
      .call(force.drag)
      .on("mouseover", nodeMouseOver)
      .on("mouseout", nodeMouseOut);


  var text = svg.append("g").selectAll("text")
      .data(force.nodes())
    .enter().append("text")
      .attr("x", 8)
      .attr("y", ".31em")
      .text(function(d) { return d.name; });


  svg.append("text")
          .attr("x", (width / 2))
          .attr("y", 15)
          .attr("text-anchor", "middle")
          .style("font-size", "16px")
          .text(interactionType);

}



makeGraph(mod_links, mod_nodes, "debate_graph1", "Moderator Prompts");
makeGraph(challenge_links, challenge_nodes, "debate_graph2", "Real Challenges");

</script>


