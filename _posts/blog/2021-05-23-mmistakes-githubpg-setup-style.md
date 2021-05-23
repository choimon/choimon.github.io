---
title: '깃헙 블로그 설정 - 스타일'
last_modified_at: 2021-05-23T10:33
categories:
  - Blog
tags:
  - minimal_mistakes
toc: true
toc_sticky: true
# show_date: false
---

## Summary 
제 깃헙 블로그에 추가한 스타일 관련 설정방법을 공유합니다. 계속 추가할 예정입니다.

이 글은 minimal mistakes 테마 기준으로 작성되어 있습니다.
{: .notice--warning}

## 파비콘 favicon 수정 

1. 로고 이미지를 준비해 주세요. 저는 canva.com에서 만들었습니다. 
2. [real favicon generator](https://realfavicongenerator.net/) 사이트에 가셔서 <a href="#" class="btn btn--info" style="pointer-events:none;">Select your Favicon image</a>를 클릭하시고 가진 이미지를 올려주세요. 
3. 간단한 설정(저는 기본 세팅으로 설정했습니다)을 하신 뒤 <a href="#" class="btn btn--info" style="pointer-events:none;">Generate your Favicons and HTML code</a>를 눌러주세요. 
4. 만들어진 Favicon package를 다운받아서 압축을 푼 폴더를 블로그 폴더로 옮겨주세요(전 assets 폴더에 넣어줬습니다). 
5. 만들어진 html code를 복사하셔서  `_includes/head/custom.html`에 넣어 주세요. 
  - 각 href에 알맞은 경로를 넣어주세요. 전 /assets/favicon 폴더 안에 있기 때문에 /assets/favicon을 앞에 넣어줬습니다.

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

html 파일 수정 없이 _config.yml에서 해결 할 수 있습니다!
`_config.yml`에 `show_date: true` 라인을 추가해 주세요.

`_config.yml`
{% highlight yml linenos %}
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
 `_config.yml`에 추가하지 마시고, 보여주고 싶은 각 포스트의 YAML front matter 에 추가해 주시면 됩니다. 
 
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
`_config.yml`에 `show_date: true`를 추가하고, 숨기고 싶은 포스트 YAML front matter에 `show_date: false`를 추가해주시면 되겠죠?


## 하이퍼링크 a 태그 밑줄 없애기 

블로그 글 리스트에 있는 글 제목링크나 포스트 안 링크를 보면 기본으로 밑줄이 쳐져 있습니다. 
심플심플을 위해 없애보겠습니다.

`_sass/minimal-mistakes/_base.scss`
```scss
/* links */

a {

  // remove underline 
  text-decoration: none;     ///// 이 부분을 넣어준다. 

  &:focus {
    @extend %tab-focus;
  }
  ...
}
```


## 블로그 색상 변경하기
mmistakes 는 기본 말고도 air, contrast 등 여러 스킨을 제공합니다. 
제공되는 스킨 말고 나만의 스킨을 만들어서 적용해볼 수 있습니다.

저는 기본 스킨에 꽤 만족하지만, 포스트 제목이 파란색인 건 참을 수 없었습니다. 
지금 제 블로그 테마는 그린그린이기 때문입니다. 스킨 파일을 `_sass/minimal-mistakes/skins` 폴더에 만들어 보겠습니다. 저는 `_choi.scss`라고 만들었지만, 원하시는 스킨 이름을 붙여주세요. choi라고 따라하셔도 됩니다.

`_sass/minimal-mistakes/skins/_choi.scss`
```scss
/* ==========================================================================
   Choi skin
   ========================================================================== */

/* Colors */
$whitish-green: #f1f7eb;
$light-green: #9bbd58; 
$green: #7bac74;
$olive-green: #94ac74;
$dark-green: #6f9e66;

$link-color: $light-green !default;  // 링크 색깔, 글 리스트 제목 링크 
$footer-background-color: $whitish-green !default; 


/* toc setting _navigation.scss*/
$toc-nav-title-color: $olive-green !default;
$toc-nav-active-color: mix(#fff, $olive-green, 80%) !default;

.page__title{
    color: $dark-green;
}
```

기존에 있는 스킨 scss파일들을 둘러보면서 설정해줬습니다. 

중간에 있는 `$toc-nav-title-color` 나 `$toc-nav-active-color`는 포스트에서 보여주는 오른쪽 목차(toc) 관련 설정입니다. 제가 만든 변수이기 때문에 이 variable을 사용하게 되는 `_navigation.scss`에도 따로 설정해줬습니다.(이 변수들을 만들지 않고 `_navigation.scss`에서 바로 색상을 넣어주셔도 됩니다) 

`_sass/minimal-mistakes/_navigation.scss`
```scss
/*
     Table of contents navigation
     ========================================================================== */

.toc {
  ...

  .nav__title {
    ...
    background: $toc-nav-title-color;
    ...
  }

  // Scrollspy marks toc items as .active when they are in focus
  .active a {
    @include yiq-contrasted($toc-nav-active-color);
  }
}

```

기존에는 `$primary-color` 와 `$active-color` 를 사용해서 넣어줬던 toc 색상을 변경시켜줬습니다.  `$primary color` 자체를 `_choi.scss`에 바꿀 수 있었지만, 이 변수는 다른 곳에서(우상단 메뉴, 인용문 등) 많이 쓰이기 때문에 따로 만들어 넣어줬습니다.


`$toc-nav-title-color`와 `$toc-nav-active-color` 제가 따로 추가한 변수들이기 때문에, `_choi.scss` 를 사용하지 않는 다른 테마 스킨일 경우 없는 변수라는 build error 가 일어납니다. 다른테마에서도 쓸 수 있도록 해당 변수들을 등록해 줍니다.  

`_sass/minimal-mistakes/_variable.scss`
```scss
/*
   Colors
   ========================================================================== */

....
/* brands */
...

/* choi colors */
$toc-nav-title-color: $primary-color !default;
$toc-nav-active-color: $active-color !default;
```


커스텀 scss 스킨을 만들고 설정을 까먹으면 안 되겠죠?
`_config.yml` 에 스킨 이름을 넣어주세요! 

`_config.yml`
```yml
minimal_mistakes_skin: 'choi'  
```





## References
1. [취미로 코딩하는 개발자 블로그](https://devinlife.com/howto%20github%20pages/github-pages-settings/)

2. [Danggai 블로그](https://danggai.github.io/github.io/Github.io-%EC%A0%9C%EB%AA%A9,-%EB%A7%81%ED%81%AC,-%EA%B0%95%EC%A1%B0%EC%83%89-%EB%B0%94%EA%BE%B8%EA%B8%B0/)
