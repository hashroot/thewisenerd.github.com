---
layout:     post
title:      what carte-noire has
date:       2015-12-11 20:00:00
categories: theme
thumbnail:  pencil-square-o
---

The theme carte-noire looks astounding (_momo: my opinions, my own_); and that was
one of the key features of it that made me want to use it with my own blog.

however, i also found in due course of time, that it also has a lot else to offer.

------------

the first amongst them is thumbnails.

The 'square-pencil-o' icon you see at the top of this page was just me typing

```
thumbnail:  pencil-square-o
```

at the beginning of markdown in this post. So, simply put, font-awesome icons are
used.

However, if you need a custom icon of yours, you're free to do that as well.
You just need to edit the ```_data/thumbnails.yml``` and add your thumbnail spec
in the following format:

```
jekyll: "http://i.imgur.com/aRQcGSi.png"
```

This has been more clearly elaborated [here][1].

-----------

The rest is just beauty of design. Jason has written a wonderful post [here][2] which
I shall shamelessly _kang_ (a part of) to illustrate this point.

-----------

### different font styles

_italic_

__bold__

<del>strikethrough</del> and <ins>insertions</ins>

-----------

### Code, with syntax highlighting

Code blocks use the [peppermint][3] theme.

```html
<!DOCTYPE html>
<title>Title</title>

<style>body {width: 500px;}</style>

<script type="application/javascript">
  function $init() {return true;}
</script>

<body>
  <p checked class="title" id='title'>Title</p>
  <!-- here goes the rest of the page -->
</body>
```

-----------

# Headings!

They're responsive, and well-proportioned (in `padding`, `line-height`, `margin`, and `font-size`).

##### They draw the perfect amount of attention

This allows your content to have the proper informational and contextual hierarchy. Yay.

##### well, ain't that true?

-----------

### There are lists, too

  * of
  * course
  * there
  * is

-----------

### and images

cute bunny picture

![Thumper](http://i.imgur.com/DMCHDqF.jpg)

-----------

### blockquotes look great

you'll find me frequently (or not so much) posting quotes here, so.

> Omnia dicta fortiora si dicta Latina

(Everything sounds more impressive in Latin)

-----------

### markdown options

Finally, getting back to markdown, here are a few options provided apart from
```layout```, ```title```, and ```date``` which are provided already by jekyll:

 * ```author```
 * ```summary```
 * ```categories```
 * ```thumbnail```
 * ```tags```

-----------

En total, awesome. 

[1]: http://carte-noire.jacobtomlinson.co.uk/jekyll/2014/06/08/using-thumbnails/
[2]: http://carte-noire.jacobtomlinson.co.uk/jekyll/2014/06/10/carte-noire-in-action/
[3]: https://noahfrederick.com/log/lion-terminal-theme-peppermint/