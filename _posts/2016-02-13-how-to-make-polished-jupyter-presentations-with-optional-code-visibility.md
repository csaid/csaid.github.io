---
layout: post
title: How to make polished Jupyter presentations with optional code visibility
---

Jupyter notebooks are great because they allow you to easily present interactive figures. In addition, these notebooks include the figures and code in a single file, making it easy for others to reproduce your results. Sometimes though, you may want to present a cleaner looking report to an audience who may not care about the code. This blog post shows how to make code visibility optional, and how to remove various Jupyter elements to get a cleaner presentation.

On the top is a typical Jupyter presentation with code and some extra elements. Below that is a more polished version that removes some of the extra elements and makes code visibility optional with a button.

<div class="wrapper">
    <a class="inner" href = "http://nbviewer.jupyter.org/github/csaid/polished_notebooks/blob/master/notebook_original.ipynb" target="_blank">
        {% include image_with_border.html url="/assets/fig_unpolished_small.png" %}
    </a>
</div>
<div class="wrapper">
    <a class="inner" href = "http://nbviewer.jupyter.org/github/csaid/polished_notebooks/blob/master/notebook_polished.ipynb" target="_blank">
        {% include image_with_border.html url="/assets/fig_polished_small.png" %}
    </a>
</div>


To make the code optionally visible, available at the click of a button, include a raw cell at the beginning of your notebook containing the JavaScript and HTML below. This code sample is inspired by a [Stack Overflow post](http://stackoverflow.com/questions/27934885/how-to-hide-code-from-cells-in-ipython-notebook-visualized-with-nbviewer), but makes a few improvements such as using a raw cell so that the button position stays fixed, changing the button text depending on state, and displaying gradual transitions so the user understands what is happening.

{% highlight html %}
<script>
  function code_toggle() {
    if (code_shown){
      $('div.input').hide('500');
      $('#toggleButton').val('Show Code')
    } else {
      $('div.input').show('500');
      $('#toggleButton').val('Hide Code')
    }
    code_shown = !code_shown
  }

  $( document ).ready(function(){
    code_shown=false;
    $('div.input').hide()
  });
</script>
<form action="javascript:code_toggle()"><input type="submit" id="toggleButton" value="Show Code"></form>
{% endhighlight %}

It's pretty straightforward to remove the extra elements like the header, footer, and prompt numbers. That being said, you may want to still include some attribution to the Jupyter project and to your free hosting service. To do all of this, just include a raw cell at the end of your notebook with some more JavaScript.

{% highlight html %}
<!-- If you find a way to simplify the footer styling that actually works, I'd love to hear about it. -->
<footer id="attribution" style="float:right; color:#999; background:#fff;">
Created with <a style="color:#337ab7" href="http://jupyter.org/">Jupyter</a>, delivered by <a style="color:#337ab7" href="http://www.fastly.com">Fastly</a>, rendered by <a style="color:#337ab7" href="https://developer.rackspace.com/?nbviewer=awesome">Rackspace</a>.
</footer>

<script>
  $(document).ready(function(){
    $('div.prompt').hide();
    $('div.back-to-top').hide();
    $('nav#menubar').hide();
    $('.breadcrumb').hide();
    $('.hidden-print').hide();
  });
</script>
{% endhighlight %}

One shortcoming with what we have so far is that users may still see some code or other unwanted elements while the page is loading. This can be especially problematic if you have a long presentation with many plots. To avoid this problem, add a raw cell at the very top of your notebook containing a preloader. This example preloader includes an animation that signals to users that the page is still loading. It heavily inspired by [this preloader](http://codepen.io/mimoYmima/pen/fisgL) created by [@mimoYmima](https://twitter.com/@mimoYmima).

{% highlight html %}
<script>
  jQuery(document).ready(function($) {

  $(window).load(function(){
    $('#preloader').fadeOut('slow',function(){$(this).remove();});
  });

  });
</script>

<style type="text/css">
  div#preloader { position: fixed;
      left: 0;
      top: 0;
      z-index: 999;
      width: 100%;
      height: 100%;
      overflow: visible;
      background: #fff url('http://preloaders.net/preloaders/720/Moving%20line.gif') no-repeat center center;
  }

  div#preloader_text {
      font-size: 250%;
      color: gray;
      text-align: center;
      line-height: 250px;
   }

</style>

<div id="preloader">
  <div id="preloader_text">
    Please allow a few seconds for page to load...
  </div>
</div>
{% endhighlight %}

<meta charset="utf-8">

To work with these notebooks, you can clone my [GitHub repository](https://github.com/csaid/polished_notebooks). While the notebooks render correctly on nbviewer ([unpolished](http://nbviewer.jupyter.org/github/csaid/polished_notebooks/blob/master/notebook_original.ipynb), [polished](http://nbviewer.jupyter.org/github/csaid/polished_notebooks/blob/master/notebook_polished.ipynb)), they do not render correctly on the GitHub viewer.
