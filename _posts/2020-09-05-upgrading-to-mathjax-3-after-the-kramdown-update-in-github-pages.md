---
layout: post
title: Upgrading to MathJax 3.0 after the Kramdown update in Github Pages
description: A simple 3-step process to fixing your formulas
---

If you're like me, you run a Github Pages blog that uses MathJax for formulas. You pushed some changes and now your math isn't rendering correctly. The math looks fine locally, but it's all messed up remotely. What happened?

The cause of your problem is likely that in early August 2020 Github Pages [updated](https://github.com/github/pages-gem/pull/704) Kramdown to 2.3.0, which as far as I can tell does not support MathJax 2.x. 

Here's a simple 3-step process for getting your math back to normal. Everything here comes from officially sanctioned documentation. No weird hacks.

#### Step 1. Reproduce the issue locally.
You need to get the new version of Github Pages and Kramdown
```
bundle update
```
Now when you launch your blog locally, you'll be able to see that much of your math is no longer rendering properly.

#### Step 2. Upgrade to MathJax 3.0
Now, find wherever you included MathJax (probably an HTML file in your `_layouts` directory) and replace _all of it_ with the code below, which comes directly from MathJax's own [instructions](http://docs.mathjax.org/en/latest/web/configuration.html#loading-mathjax).

```
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js">
</script>
```

#### Step 3 [if applicable]. Change your $ signs to $$ signs.
I ran into some trouble because a lot of my in-line math was delimited with single `$` signs rather than double `$$` signs. I wasn't able to configure MathJax to accept the single `$` signs, so I just changed them all to `$$`. While it was a little tedious, it felt like the right thing to do. The single `$` was too ambiguous.
