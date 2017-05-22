---
layout: post
title: Keyboard shortcuts I couldn't live without
---

Keyboard shortcuts are interesting. Even though I know they are almost always worth learning, I often find myself shying away from the uncomfortable task of actually learning them. But after years of clumsily reaching for the mouse while my colleagues looked at me with a kindly sense of pity, I have slowly accumulated enough keyboard tricks that I'd like to share them. This set is probably far from optimal, and different people have their own solutions, but it has worked well for me. Before jumping in, here's a reference table of key symbols and their common names:

<div class="wrapper">
<table style='font-size:.7rem; margin: auto; width: 250px'>
    <tr>
        <th>Key Symbol</th>
        <th>Key Name</th>
    </tr>
    <tr>
        <td align="center">⌘</td>
        <td>Command</td>
    </tr>
    <tr>
        <td align="center">⇧</td>
        <td>Shift</td>
    </tr>
    <tr>
        <td align="center">⌃</td>
        <td>Control</td>
    </tr>
    <tr>
        <td align="center">⌥</td>
        <td>Alt/Option</td>
    </tr>
     <tr>
        <td align="center">↵</td>
        <td>Enter</td>
    </tr>
</table>
</div>
<div class="caption"><strong>Table 1.</strong> Key symbol map.</div>

### Sublime Text
Sublime has [lots](http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_osx.html) of great shortcuts. My favorites is `⌘D`, which allows you to sequentially select exact matches of highlighted text. Once the matches are selected, you can simultaneously edit them with a multi-cursor. If you want to select all matches simultaneously, rather than sequentially, you can use `⌃⌘G`. 


<div class="wrapper">
  <img src='/assets/2017_keyboard_shortcuts/gif_sublime_shining_fast.gif' width="350" class="inner" style="position:relative">
  <div class="caption">
    <strong>Figure 1.</strong> Demonstration of <code>⌘D</code>, <code>⌘←</code>, <code>⌘→</code>, <code>⌘A</code> and <code>⌘KU</code> in Sublime Text.
  </div>
</div><br>

### Chrome
With the exception of scrolling and link clicking, everything you do in Chrome should be done with keyboard only. If you're new to this, I'd recommend starting with `⌘L`, `⌘T`, `⌘W` and then expanding from there. Special bonus:	 if you ever accidentally close a tab, you can reopen it with `⌘⇧T`.

{% include image.html url="/assets/2017_keyboard_shortcuts/no_touching.png" %}
<div class="caption"><strong>Figure 2.</strong> The "No Touching" Chrome Zone. Your mouse should never come anywhere near here.</div>

### Mac OS X
On Mac OS X, `⌘ Tab` switches between applications, and <code>⌘`</code> switches between windows of the same application. The `⌘+` and `⌘-` shortcuts change the display size of text and other items. You can jump to the beginning of a line with `⌘←` or `⌃A`, and to the end of a line with `⌘→` or `⌃E`. In Terminal, you can delete to the beginning of a line with `⌃U`. 

For easy window snapping, use the [Spectacle](https://www.spectacleapp.com/) app. Because Spectacle's default mappings conflict with Chrome's tab switching shortcuts, I'd recommend setting the four main screen position shortcuts to `⌘⌃←`, `⌘⌃→`, `⌘⌃↑`, `⌘⌃↓`, and eliminating all the other shortcuts, except the full screen shortcut, which should be set to `⌘⌃F`.

<div class="wrapper">
  <img src='/assets/2017_keyboard_shortcuts/gif_spectacle.gif' width="350" class="inner" style="position:relative; border: #666666 2px solid;" >
  <div class="caption">
    <strong>Figure 3.</strong> Arranging windows with custom shortcuts in Spectacle.
  </div>
</div>

### Gmail
If you're using a mouse on Gmail, you're doing it wrong. With the exception of a few word processing operations, literally everything you do in Gmail should be done with keyboard only. Compose, Reply, Reply All, Forward, Send, Search, Navigate, Open, Inbox, Sent, Drafts, Archive. All of these should be done with the keyboard. To enable these shortcuts, you must go into your Settings and navigate to the General tab. Once shortcuts have been enabled, you can see a list of all them by typing `?`.
{% include image.html url="/assets/2017_keyboard_shortcuts/fig_gmail_annotated.png" %}
<div class="caption"><strong>Figure 4.</strong> A small sample of things you can do on Gmail without ever touching your mouse.</div>

### Twitter 
With shortcuts similar to Gmail's, you can jump to different pages using only the keyboard: `gh` brings you to the Home Timeline and `gu` lets you jump to another user's profile. The most useful shortcut is probably `.`, which loads any new tweets that are waiting for you at the top of the Timeline. You can see a list of all shortcuts by typing `?`.

### JetBrains
JetBrains products like DataGrip, PyCharm, and IntelliJ offer plenty of keyboard shortcuts. My favorites are `⌃G`, for sequential highlighting, and `⌥⌥↓` and `⌥⌥↑` for [multi-line cursors](https://www.jetbrains.com/help/idea/2017.1/multicursor.html).

### Jupyter
Jupyter has tons of [essential](http://jupyter-notebook.readthedocs.io/en/latest/examples/Notebook/Notebook%20Basics.html?highlight=keyboard#Keyboard-Navigation) keyboard shortcuts that can be found by typing `?` while in command mode. In addition, it's possible to get Sublime-style text editing by following the instructions described [here](http://blog.rtwilson.com/how-to-get-sublime-text-style-editing-in-the-ipythonjupyter-notebook/).

<div class="wrapper">
  <img src='/assets/2017_keyboard_shortcuts/gif_jupyter_delayed.gif' width="350" class="inner" style="position:relative; border: #ccc 1px solid;">
  <div class="caption">
    <strong>Figure 5.</strong> Common Jupyter workflow done entirely with the keyboard, with help from some Sublime-style editing: <code>c</code> and <code>v</code> to copy and paste a cell, <code>⌘D</code> for multiple selection, <code>⌘→</code> to jump to the end of line, <code>dd</code> to delete a cell.
  </div>
</div>
<br>

