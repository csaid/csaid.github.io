---
layout: post
title: 2015 Republican Presidential Debates
---

Cum sociis natoque penatibus et magnis <a href="#">dis parturient montes</a>, nascetur ridiculus mus. *Aenean eu leo quam.* Pellentesque ornare sem lacinia quam venenatis vestibulum. Sed posuere consectetur est at lobortis. Cras mattis consectetur purus sit amet fermentum.

Cum sociis natoque penatibus et magnis <a href="#">dis parturient montes</a>, nascetur ridiculus mus. *Aenean eu leo quam.* Pellentesque ornare sem lacinia quam venenatis vestibulum. Sed posuere consectetur est at lobortis. Cras mattis consectetur purus sit amet fermentum.

Cum sociis natoque penatibus et magnis <a href="#">dis parturient montes</a>, nascetur ridiculus mus. *Aenean eu leo quam.* Pellentesque ornare sem lacinia quam venenatis vestibulum. Sed posuere consectetur est at lobortis. Cras mattis consectetur purus sit amet fermentum.

<meta charset="utf-8">

<div id="debate_graph"></div>

<style>
/*
#3AC3F2:
#ED2685:*/

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

text {
  font: 10px sans-serif;
  pointer-events: none;
  text-shadow: 0 1px 0 #fff, 1px 0 0 #fff, 0 -1px 0 #fff, -1px 0 0 #fff;
}

</style>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script>
<!-- <script type='text/javascript' src='/javascripts/jquery-2.1.4.min.js'></script>
<script type='text/javascript' src='/javascripts/jquery.tipsy.js'></script> -->
<!-- <link rel="stylesheet" href="/stylesheets/tipsy.css" type="text/css" /> -->
<script>

var links = [
  {source: 0, target: 10, mentions: 2},
  {source: 1, target: 0, mentions: 2},
  {source: 1, target: 7, mentions: 2},
  {source: 2, target: 10, mentions: 8},
  {source: 3, target: 10, mentions: 1},
  {source: 4, target: 10, mentions: 2},
  {source: 5, target: 10, mentions: 1},
  {source: 6, target: 10, mentions: 3},
  {source: 7, target: 1, mentions: 4},
  {source: 7, target: 10, mentions: 5},
  {source: 7, target: 8, mentions: 2},
  {source: 8, target: 3, mentions: 1},
  {source: 8, target: 7, mentions: 2},
  {source: 9, target: 1, mentions: 1}
];

var nodes = [
{idx: 0, name: 'Bush', fixed: true, x: 280.0, y: 150.0},
{idx: 1, name: 'Trump', fixed: true, x: 255.172209269, y: 226.412082798},
{idx: 2, name: 'Walker', fixed: true, x: 190.172209269, y: 273.637347118},
{idx: 3, name: 'Huckabee', fixed: true, x: 109.827790731, y: 273.637347118},
{idx: 4, name: 'Carson', fixed: true, x: 44.8277907313, y: 226.412082798},
{idx: 5, name: 'Cruz', fixed: true, x: 20.0, y: 150.0},
{idx: 6, name: 'Rubio', fixed: true, x: 44.8277907313, y: 73.587917202},
{idx: 7, name: 'Paul', fixed: true, x: 109.827790731, y: 26.3626528816},
{idx: 8, name: 'Christie', fixed: true, x: 190.172209269, y: 26.3626528816},
{idx: 9, name: 'Kasich', fixed: true, x: 255.172209269, y: 73.587917202},
{idx: 10, name: 'Clinton', fixed: true, x: 410, y: 100}
]

// Compute the distinct nodes from the links.
links.forEach(function(link) {
  link.source = nodes[link.source] || (nodes[link.source] = {name: link.source});
  link.target = nodes[link.target] || (nodes[link.target] = {name: link.target});
});

var width = 600,
    height = 300;

var force = d3.layout.force()
    .nodes(d3.values(nodes))
    .links(links)
    .size([width, height])
    .on("tick", tick)
    .start();

var svg = d3.select("div#debate_graph").append("svg")
    .attr("width", width)
    .attr("height", height);

// Per-type markers, as they don't inherit styles.
svg.append("defs")
    .append("marker")
    .attr("id", "marker")
    .attr("viewBox", "0 -5 10 10") // min-x, min-y, width, height
    .attr("refX", 12) // The reference point. Even though arrow is length 10, using 12 because otherwise would go to center of circle.
    .attr("refY", 0)
    .attr("markerWidth", 14)
    .attr("markerHeight", 14)
    .attr("markerUnits", "userSpaceOnUse") // makes marker size independent of stroke-width
    .attr("orient", "auto")
    .attr("fill", "#3AC3F2")
  .append("path")
    .attr("d", "M0,-5L10,0L0,5"); // Arrow definition. Start at 0,-5. Then draw line to 10, 0. Then draw line to 0, 5

// http://stackoverflow.com/questions/10805184/d3-show-data-on-mouseover-of-circle
// http://bl.ocks.org/biovisualize/1016860
var tooltip = d3.select("div#debate_graph")
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
    .on("mouseover", function(d){return tooltip.style("visibility", "visible").text("Mentions: " + d.mentions)})
    .on("mousemove", function(){return tooltip.style("top",
        (d3.event.pageY-10)+"px").style("left",(d3.event.pageX+10)+"px");})
    .on("mouseout", function(){return tooltip.style("visibility", "hidden");});


var circle = svg.append("g").selectAll("circle")
    .data(force.nodes())
  .enter().append("circle")
    .attr("r", 6);

var text = svg.append("g").selectAll("text")
    .data(force.nodes())
  .enter().append("text")
    .attr("x", 8)
    .attr("y", ".31em")
    .text(function(d) { return d.name; });


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

</script>
As you can see...
