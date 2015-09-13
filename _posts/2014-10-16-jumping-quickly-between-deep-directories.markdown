---
author: csaid81
comments: true
date: 2014-10-16 21:11:00+00:00
layout: post
slug: jumping-quickly-between-deep-directories
title: Jumping quickly between deep directories
wordpress_id: 315
---

I often need to jump between different directories with very deep paths, like this:

{% highlight bash %}

$ cd some/very/deep/directory/project1
$ # do stuff in Project 1
$ cd different/very/deep/directory/project2
$ # do stuff in Project 2

{% endhighlight %}


While it only takes a handful of seconds to switch directories, the extra mental effort often derails my train of thought. Some solutions exist, but they all have their limitations. For example, `pushd` and `popd` don’t work well for directories you haven’t visited in a while. Aliases require you to manually add a new alias to your .bashrc every time you want to save a new directory.




I recently found a solution, inspired by [this post](http://jeroenjanssens.com/2013/08/16/quickly-navigate-your-filesystem-from-the-command-line.html) from [Jeroen Janssens](https://twitter.com/jeroenhjanssens), that works great and feels totally natural. All it takes is a one-time change to your .bashrc that will allow you to easily save directories and switch between them. To save a directory, just use the `mark` function:




{% highlight bash %}

$ pwd
some/very/deep/directory/project1
$ mark project1

{% endhighlight %}




To navigate to a saved directory, just use the `cdd` function:




{% highlight bash %}

$ cdd project1
# do stuff in Project 1
$ cdd project2
# do stuff in Project 2

{% endhighlight %}


You can display a list of your saved directories with the `marks` function, and you can remove a directory from the list with the `unmark` function:




{% highlight bash %}
$ unmark project1
{% endhighlight %}




For any of this to work, you'll need to add this to your .bashrc, assuming you have a Mac and use the bash shell.




{% highlight bash %}

    function cdd {
        cd -P "$MARKPATH/$1" 2>/dev/null || echo "No such mark: $1"
    }
    function mark {
        mkdir -p "$MARKPATH"; ln -s "$(pwd)" "$MARKPATH/$1"
    }
    function unmark {
        rm -i "$MARKPATH/$1"
    }
    function marks {
        \ls -l "$MARKPATH" | tail -n +2 | sed 's/  / /g' | cut -d' ' -f9- | awk -F ' -> ' '{printf "%-10s -> %s\n", $1, $2}'
    }

    _cdd()
    {
        local cur=${COMP_WORDS[COMP_CWORD]}
        COMPREPLY=( $(compgen -W "$( ls $MARKPATH )" -- $cur) )
    }
    complete -F _cdd cdd

{% endhighlight %}




This differs from Jeroen’s [original](http://jeroenjanssens.com/2013/08/16/quickly-navigate-your-filesystem-from-the-command-line.html) code in a couple of ways. First, to be more brain-friendly, it names the function “cdd” instead of “jump”. Second, the tab completion works [better](https://news.ycombinator.com/item?id=6229291).




Update: [John McDonnell](https://twitter.com/johnvmcdonnell) points me to [autojump](https://github.com/joelthelion/autojump).






