---
layout: post
title: How to make polished Jupyter presentations with optional code visibility
---

One of the great things about Jupyter notebooks is that they allow you to present reports with figures and code in a single file, making it easier for others to reproduce your results. Sometimes though, you may want to present a cleaner looking report to an audience who may not care about the code. This post shows how to make code visibility optional, and how to remove various Jupyter elements to get a cleaner presentation.

On the left is a typical Jupyter presentation with code and some extra elements. On the right is a more polished version that removes some of the extra elements and makes code visibility optional with a button.



<style>
    div.image_container {
        position: relative;
        width: 510px;
        height: 420px;
    }

    div.in_container {
        position: absolute;
    }
</style>

<div class="image_container">
    <div class="in_container" style="position:absolute top:0px; left:0px">
      <a href = "http://nbviewer.jupyter.org/github/csaid/polished_notebooks/blob/master/notebook_original.ipynb" target="_blank">
        <img src="assets/fig_unpolished_small.png" border="2">
      </a>
    </div>
    <div class="in_container" style="position:absolute top:0px; left:260px">
      <a href = "http://nbviewer.jupyter.org/github/csaid/polished_notebooks/blob/master/notebook_polished.ipynb" target="_blank">
        <img src="assets/fig_polished_small.png" border="2">
      </a>
    </div>
</div>



To make the code optionally visible, available at the click of a button, include a raw cell at the beginning of your notebook containing the JavaScript and HTML below. This code sample is inspired by [this Stack Overflow post](http://stackoverflow.com/questions/27934885/how-to-hide-code-from-cells-in-ipython-notebook-visualized-with-nbviewer), but makes a few improvements such as using a raw cell so that the button position stays fixed, changing the button text depending on state, and displaying gradual transitions so the user understands what is happening.

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

To remove the extra elements, include a raw cell at the end of your notebook with some more JavaScript:

{% highlight html %}
<script>
$(document).ready(function(){
    $('div.prompt').hide();
    $('div.back-to-top').hide();
    $('nav#menubar').hide();
    $('.breadcrumb').hide();
    $('footer').hide();
});
</script>
{% endhighlight %}

One shortcoming with what we have so far is that users may still see some code or other unwanted elements while the page is loading. This can be an especially big problem if you have a long presentation with many plots. To avoid this, add a raw cell at the very top of you notebook containing a preloader. This example pre-loader includes an animation that signals to users that the page is still loading. It heavily inspired by [this preloader](http://codepen.io/mimoYmima/pen/fisgL) created by [@mimoYmima](https://twitter.com/@mimoYmima).

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
