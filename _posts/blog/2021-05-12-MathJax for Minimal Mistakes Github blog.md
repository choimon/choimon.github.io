---
title: 'How to use Latex(MathJax) on Minimal Mistakes Github blogs'
last_modified_at: 2021-05-21T20:41
categories:
  - blog
tags:
  - mathjax
  - minimal_mistakes
toc: true
use_math: true
---

## Summary 
We'll learn how to add MathJax and render math equations on our Minimal Mistakes Github pages. 

## What is MathJax?
> a cross-browser JS library that displays mathematical notation in web browsers, using MathML, LaTeX and ASCIIMathML markup. <cite><a href="https://en.wikipedia.org/wiki/MathJax">Wikipedia</a></cite>

It's a software that allows you to write LaTeXlike notation. 
$$\LaTeX$$


## How to add MathJax to Minimal Mistakes 

### 1. Set the markdown engine
Set the markdown engine to 'kramdown' in *config.xml*. 

*config.xml*
```xml
# Conversion
markdown: kramdown
```
If you're not using Minimal Mistakes, refer to [jekyll's official site](https://jekyllrb.com/docs/configuration/).
{: .notice}

### 2. Add a MathJax script
There are multiple ways to do this. You could add the MathJax script part directly in *_includes/scripts.html*, */_includes/head/custom.html*, or */_includes/footer/custom.html*, but I chose to create a separate html for this extra support.

*_includes/mathjax-custom.html*
```html

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: ["tex2jax.js"],
    jax: ["input/TeX", "output/HTML-CSS"],
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
      processEscapes: true
    },
    "HTML-CSS": { availableFonts: ["TeX"] }
  });


  MathJax.Hub.Register.MessageHook("Math Processing Error",function (message) {
    alert("Math Processing Error: "+message[1]);
    });
  MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message) {
    alert("Math Processing Error: "+message[1]);
    });
</script>


<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```
The first part contains a custom MathJax configuration.According to the MathJax website, the TeX input component doesn't enable single dollar signs as delimiters for in-line math. To enable single dollar signs, you need to explicitly configure it. 

(You might want to get the latest script src from this [site](http://docs.mathjax.org/en/latest/web/start.html))




### 3. Load the MathJax html
**Note** You can skip this step if you added the script in *_includes/script.html*,  */_includes/head/custom.html*, or */_includes/footer/custom.html*.
{: .notice--warning}


in *_layouts/default.html*, I added the following at the end: 

{% raw %}
```html
  {% if page.use_math %}
    {% include mathjax-custom.html %}
  {% endif %}
```
{% endraw %}

so my default.html would look like this: 
{% raw %}
```html
---
---

<!doctype html>
<!--
  Minimal Mistakes Jekyll Theme 4.22.0 by Michael Rose
  Copyright 2013-2020 Michael Rose - mademistakes.com | @mmistakes
  Free for personal and commercial use under the MIT license
  https://github.com/mmistakes/minimal-mistakes/blob/master/LICENSE
-->
<html lang="{{ site.locale | slice: 0,2 | default: "en" }}" class="no-js">
  <head>
    {% include head.html %}
    {% include head/custom.html %}

  </head>

  <body class="layout--{{ page.layout | default: layout.layout }}{% if page.classes or layout.classes %}{{ page.classes | default: layout.classes | join: ' ' | prepend: ' ' }}{% endif %}">
    {% include_cached skip-links.html %}
    {% include_cached browser-upgrade.html %}
    {% include_cached masthead.html %}

    <div class="initial-content">
      {{ content }}
    </div>

    {% if site.search == true %}
      <div class="search-content">
        {% include_cached search/search_form.html %}
      </div>
    {% endif %}

    <div id="footer" class="page__footer">
      <footer>
        {% include footer/custom.html %}
        {% include_cached footer.html %}
      </footer>
    </div>

    {% include scripts.html %}

    {% if page.use_math %}
      {% include mathjax-custom.html %}
    {% endif %}
  </body>
</html>


```

{% endraw %}


- Any page using this layout will be able to use MathJax to render math equations. 
- I added a conditional to avoid embedding the script on pages that don't need math equations.
- I also found some people adding this part in *_includes/head.html*



### 4. check!
Let's check if MathJax works!\
Don't forget to add the following in your post/page's YAML Front Matter: 

```md
---
title: 'some random post'
use_math: true
---

blahblahblah

```


- display mode
\\[p(\theta) = \mathbf{\prod}_{i,c}p(\mathbf{\theta}^i(c))\\]

$$
\begin{aligned} 
a^2 + b^2 &= c^2 \\ 
E &= M \cdot C^2 \\ 
&= xy + \mathbb{E} 
\end{aligned}
$$


$$ 
{X}_{0}  \\
  X_0 (working) \\
  \hat{a}_{b}   \\
  \hat{a}_b  \\
  \hat{a}_{b+c} \\
$$

$$
    f(n) =
      \begin{cases}
      n/2,  & \text{if $n$ is even} \\
      3n+1, & \text{if $n$ is odd}
      \end{cases}
$$
{% raw %}
```
\\[p(\theta) = \mathbf{\prod}_{i,c}p(\mathbf{\theta}^i(c))\\]

$$
\begin{aligned} 
a^2 + b^2 &= c^2 \\ 
E &= M \cdot C^2 \\ 
&= xy + \mathbb{E} 
\end{aligned}
$$


$$ 
{X}_{0}  \\
  X_0 (working) \\
  \hat{a}_{b}   \\
  \hat{a}_b  \\
  \hat{a}_{b+c} \\
$$

$$
    f(n) =
      \begin{cases}
      n/2,  & \text{if $n$ is even} \\
      3n+1, & \text{if $n$ is odd}
      \end{cases}
$$
```
{% endraw%}





- inline \
\\(p(\theta) = \mathbf{\prod}_{i,c}p(\mathbf{\theta}^i(c))\\) inline formula  yay
$ f(x) = x^2$


{% raw %}
```
\\(p(\theta) = \mathbf{\prod}_{i,c}p(\mathbf{\theta}^i(c))\\) inline formula  yay
$ f(x) = x^2$
```
{% endraw %}




## References
1. [MathJax docs](http://docs.mathjax.org/en/latest/web/start.html)
2. [mmistakes Github issue page](https://github.com/mmistakes/minimal-mistakes/issues/735)
3. [kevinfossez's blog](https://kevinfossez.github.io/posts/2020/04/blog-post-1/)
4. [Haoyu Ji's blog](https://sort-care.github.io/Latex-on-Blog/)
5. [Sanglee Park's blog](https://sanglee325.github.io/blog/mathjax-github-io/#)

