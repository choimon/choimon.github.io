---
title: '깃헙 블로그 설정'
last_modified_at: 2021-05-22T22:37
categories:
  - Blog
tags:
  - minimal_mistakes
toc: true
toc_sticky: true
# show_date: false
---

## Summary 
제 깃헙 블로그에 추가한 설정을 공유합니다. 계속 추가할 예정입니다.

이 글은 minimal mistakes 테마 기준으로 작성되어 있습니다.
{: .notice--warning}

## 파비콘 favicon 수정 

먼저, 로고 이미지를 준비해 주세요. 저는 canva.com에서 만들었습니다. 

[real favicon generator](https://realfavicongenerator.net/) 사이트에 가셔서 <a href="#" class="btn btn--info" style="pointer-events:none;">Select your Favicon image</a>를 클릭하시고 가진 이미지를 올려주세요. 그다음 간단한 설정(저는 기본 세팅으로 설정했습니다)을 하신 뒤 <a href="#" class="btn btn--info" style="pointer-events:none;">Generate your Favicons and HTML code</a>를 눌러주세요. 만들어진 Favicon package를 다운받아서 압축을 푼 폴더를 블로그 폴더로 옮겨주세요(전 assets 폴더에 넣어줬습니다). 그리고, 만들어진 html code를 복사하셔서  `_includes/head/custom.html`에 넣어 주세요. 




custom.html에 복붙하신 뒤 각 href에 알맞은 경로를 넣어주세요. 전 /assets/favicon 폴더 안에 있기 때문에 /assets/favicon을 앞에 넣어줬습니다.


`_includes/head/custom.html`
```html
<!-- start custom head snippets -->

<!-- insert favicons. use https://realfavicongenerator.net/ -->
<link
  rel="apple-touch-icon"
  sizes="180x180"
  href="/assets/favicon/apple-touch-icon.png"
/>
<link
  rel="icon"
  type="image/png"
  sizes="32x32"
  href="/assets/favicon/favicon-32x32.png"
/>
<link
  rel="icon"
  type="image/png"
  sizes="16x16"
  href="/assets/favicon/favicon-16x16.png"
/>
<link rel="manifest" href="/assets/favicon/site.webmanifest" />
<link rel="mask-icon" href="/assets/favicon/safari-pinned-tab.svg" color="#5bbad5" />
<meta name="msapplication-TileColor" content="#ffffff" />
<meta name="theme-color" content="#ffffff" />

<!-- end custom head snippets -->


```


## 포스트 제목 아래 날짜 표시 
기본 mmistakes 테마에선 포스트 제목 아래 읽는데 예상되는 시간(read time)만 보여줍니다. 작성된 날짜는 포스트 맨 아래로 내려야 보입니다. 제목 바로 아래 날짜를 보여주기 위한 세팅은 이미 몇몇 블로그 포스트에 설명되어있었습니다. 기존 글보다 간편한 방법을 찾았습니다. 

(제가 찾은 글에는 `_includes/archive-single.html` 이나 `_layouts/single.html`을 수정했는데, 제 생각에는 이 포스트들이 작성된 후 mmistakes 테마가 업데이트된 것 같습니다. 관련 코드가 page__meta.html로 옮겨간 것 같습니다). 

html 파일 수정 없이 config.xml에서 해결 할 수 있습니다!
`config.xml`에 `show_date: true` 라인을 추가해 주세요.

`config.xml`
{% highlight xml linenos %}
# Defaults: default settings for posts
defaults:
  # _posts
  - scope:
      path: ''
      type: posts
    values:
      layout: single
      ...
      show_date: true  #<-----
{% endhighlight %}

이렇게 설정하면 올리는 모든 포스트가 제목 아래 날짜를 가지게 됩니다. 
![show_date]({{"/assets/images/posts/20210522_blog_date.png"| relative_url}})
{: left-align}

혹시, 특정 몇 개의 포스트만 날짜를 보여주고 싶으시다면, 
 `config.xml`에 추가하지 마시고, 보여주고 싶은 각 포스트의 YAML front matter 에 추가해 주시면 됩니다. 
 
 ```md
 {% raw %}
---
title: "random.md"
show_date: true  #<-----
---

This post has post date enabled.
 {% endraw %}
```

반대로 기본으로 날짜를 보여주되, 몇 개 포스트만 숨기고 싶으시면, 
`config.xml`에 `show_date: true`를 추가하고, 숨기고 싶은 포스트 YAML front matter에 `show_date: false`를 추가해주시면 되겠죠?




## 수학 공식 (latex) 사용하기 
MathJax를 사용한 제 [블로그 글]({{ site.url }}{{ site.baseurl }}/blog/mathjax-for-minimalmistakes-githubpage/)을 참고해 주시기 바랍니다. 




## References
1. [취미로 코딩하는 개발자 블로그](https://devinlife.com/howto%20github%20pages/github-pages-settings/)
