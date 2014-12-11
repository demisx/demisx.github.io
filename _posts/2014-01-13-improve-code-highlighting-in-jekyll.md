---
layout: post
title:  "Improve Code Highlighting in a Jekyll-based Blog Site"
date:   2014-01-13 21:08:12
categories: jekyll
---

If you don't mind getting your hands on "HTML-like" type of code and looking around for a way to launch a
simple blog website paired with free hosting, then [Jekyll](http://jekyllrb.com)</a>
should definitely be on your evaluation list. It's open-source, text based and follows baking
CMS principle by generating a static site from pre-written markup files. Pair it with free hosting
from [GitHub Pages](http://pages.github.com), comments from [Disqus](http://disqus.com) and you have
yourself a very flexible blogging platform without required dependencies on a database and third party
module/plugins.

<!--more-->

The keyphrase here is "flexible", of course. And as we all know with
*Great flexibility comes great responsibility*. One such thing that I thought could be improved is
the built-in way of highlighting code snippets with included line numbers.

### Option 1: Via Addition of Custom CSS Rules

In order to have Jekyll highlight a
snippet of code in your posts you basically need 3 things:

1. Add [syntax highlighter CSS file](https://gist.github.com/demisx/025698a7b5e314a7a4b5) as
`css/syntax.css` to your existing or newly generated Jekyll site
2. Load CSS inside of a corresponding layout file (e.g. _layouts/default.html) {% highlight html %}
<head>
  ...
  <link href="/css/syntax.css" rel="stylesheet">
  ...
</head>{% endhighlight %}
3. Wrap your code snippets in posts with
`{% raw %} {% highlight ruby linenos %}...{% endhighlight %} {% endraw %}` Liquid tags and Jekyll
(via [Pygments highlighter](http://pygments.org)) outputs color highlighted code based on chosen
language scheme (e.g. ruby in my case). <span class="text-muted">_(Note, if you don't want to see
line numbers appearing next to each code line, simply remove `linenos`
directive from `{% raw %} {% highlight ruby %} {% endraw %}` and, probably, there is need to read the
rest of this post)_</span>

For example, this snippet of Ruby code:
{% raw %}
    {% highlight ruby linenos %}
    def show
      puts "Outputting a very lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong line"
      @widget = Widget(params[:id])
      respond_to do |format|
        format.html # show.html.erb
        format.json { render json: @widget }
      end
    end
    {% endhighlight %}
{% endraw %}

translates by Jekyll's built-in syntax highlighter to this:

<div class="preserve-original-format">
{% highlight ruby linenos %}
def show
  puts "Outputting a very lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong line"
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}
</div>

Not bad, but could be better. Notice two major issues with the built-in highlighter:

1. Line numbers have too much prominence and sort of blend in with the main code
2. Long lines similar to line 2 get wrapped making code less readable

I like when line numbers are grayed out and clearly separated from the main code. Also, I like when
long lines that don't fit into the snippet container get clipped. This is similar to how GitHub handles their
snippets. However, I also don't want to deviate from Jekyll's core code by introducing all sorts of
third-party plugins unless it's absolutely a must. I like my blog to be maintainable and upgradeable.

So, to improve the formatting of line numbers, just add these 2 lines to your `syntax.css` file
([full version of syntax.css](https://gist.github.com/demisx/8408522)):

{% highlight css linenos %}
/* Add to css/syntax.css */
.post > .highlight .lineno { color: #ccc; display:inline-block; padding: 0 5px; border-right:1px solid #ccc; }
.post > .highlight pre code { display: block; white-space: pre; overflow-x: auto; word-wrap: normal; }
{% endhighlight %}

And now your code snippets will be highlighted as this:

{% highlight ruby linenos %}
def show
  puts "Outputting a very lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong lo-o-o-o-o-o-o-o-o-o-o-o-o-o-o-o-ong line"
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}

### Option 2: Via Embedded GitHub Gists

Another alternative to using Jekyll's built-in highlighter is to use GitHub Gists. They are directly
embeddable into your posts with `{% raw %} {% gist [gist_number] %} {% endraw %}` tag.
Thus, if you are OK to host your code snippets on GitHub as gists, then inserting like this into your
post markup:

```
{% raw %}
{% gist 8387126 %}
{% endraw %}
```

will produce this GitHub-like formatted snippet loaded straight from GitHub and formatted like a GitHub
snippet:

{% gist 8387126  %}

I have mixed feelings about this particular approach. One one hand, it does make embedding properly
highlighted snippets in my posts a walk in a park. On the other hand, it introduces dependency on GitHub.
For now, I am sticking with built-in highlighter (Option 1) with GitHub gists being my fall-back option if
things go south.

___
##### Today my environment was:

Modestly tested on Jekyll v1.4.3 and may not cover all edge cases. Not been tested on some older
browsers that didn't bother to properly follow CSS standards at the time.
