---
title: 'SVG vs Canvas 언제 써야하나'
last_modified_at: 2023-10-23T16:25
categories:
  - frontend
tags:
  - svg
  - canvas
toc: true
toc_sticky: true
---


# 개요
SVG와 Canvas는 둘다 HTML5에서 추가된 기술이다. 
둘다 그래픽을 그리는 기술이지만, 그려지는 방식이 다르다.

차트 라이브러리같은 경우 Canvas 기반과 SVG 기반으로 나뉘어져있다 (Echart처럼 둘다 지원하는 라이브러리도 존재한다).
언제 어떤 기술을 사용해야할지 알아보기 전 각 기술의 특징을 알아보자.

# SVG 
SVG(Scalable Vector Graphics)는 벡터(vector)이며 선언적(declarative)이다.[^fn1]

벡터 아트가 필요할땐 SVG를 사용한다.<br/>
벡터 아트 특징 
- 보기에 깔끔하게 떨어지고(crisp, 테두리가 지글지글거리지않음), 확대해도 깨지지 않음
- 벡터아트는 JPG와 같은 래스터 그래픽보다 보통 파일 크기가 작음

페이지 로고를 쓸 때 SVG를 흔하게 사용한다. SVG 코드는 HTML파일에 바로 삽입할 수 있고, 선언적으로 어떻게 그릴지 명시할 수 있다. 
```html 
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>
```
위 코드는 가로, 세로 100 사이즈의 SVG를 그리고, 그 안에 중앙 포인트가 (50,50)에 위치하며, 반지름이 40인 원을 그린다.
이 원은 녹색 테두리와 노란색으로 채워진다.


그려야할 그래픽이 유연하고(flexible) 반응형이여야한다면(responsive) SVG를 사용한다.

[//]: # (- retained mode graphics model )

## DOM 
SVG는 HTML문서 DOM에 존재한다. 
`<svg>` 엘리먼트가 문서에 들어가면, `<div>`와 비슷하게 동작한다. SVG는 엘리먼트, 속성(attributes), styles, 그리고 HTML DOM이벤트를 가질 수 있다.
(`<svg>`는 `<div>`와 다르게 추가적으로 `SVGDocument` 인터페이스를 가진다.[^fn6] `SVGDocument`는 SVG안 벡터 그래픽과 다양한 인터렉션을 제공한다고 한다.)

```html
<svg viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="50" />
  <script>
    document.querySelector('circle').addEventListener('click', e => {
      e.target.style.fill = "red";
    });
  </script>
</svg>
```
SVG `<circle>`엘리먼트는 `<div>`처럼 `click`, `mouseDown`과 같은 DOM 이벤트를 가질 수 있다.
위 코드는 `circle`을 클릭시 `circle` 색이 빨간색으로 바뀌는 이벤트리스너를 등록한다. 

## 접근성(accessibility)
접근성을 위해 SVG를 사용할 수 있다. 

canvas도 아래처럼 대체 텍스트(alternative text)를 가질 수 있다.
```html
<canvas aria-label="Hello ARIA World" role="img"></canvas>
```

위처럼 SVG 엘리먼트에도 접근성 속성 `aria-label`을 추가할 수 있다. 이것말고도, SVG는 바로 DOM에 존재하기 때문에,
접근성을 개발할 때 더 다양하게 활용할 수 있다. 
예를 들어, 보조 기술(assistive technology)을 개발할 때 SVG안 링크나 하위 엘리먼트를 접근하여 소리내어 읽어주는 기술에 활용할 수 있다.

특히 텍스트에 대해 Canvas보다 SVG가 더 접근적이다. canvas에서는 text가 뿌옇게 보이는데, SVG는 `<text>`엘리먼트를 가지고 있고, 뿌옇게 보이지 않는다.
구글 서치 엔진에서도 SVG내용을 인식할 수 있다.[^fn4]

접근성 부분은 실무에서 직접 사용해보지 않아서 더 자세히 찾아봐야겠다. 

## CSS 적용 
SVG는 CSS를 적용할 수 있다. 
```html
<svg viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="50" />
  
  <style>
    circle { fill: blue; }
  </style>
</svg>
```

SVG에 `:hover`과 같은 상태나 CSS 애니메이션도 적용가능하다.

```html
<svg viewBox="0 0 100 100">
  
     <circle cx="50" cy="50" r="50" />
  
  <style>
    circle { fill: blue; animation: pulse 2s alternate infinite; }
    @keyframes pulse {
      100% {
        r: 30;
      }
    }
  </style>
</svg>

```


# Canvas
Canvas는 자바스크립트의 drawing API이다.[^fn3]
HTML에 `<canvas>`엘리먼트를 넣고, 자바스크립트로 그림을 그린다.
JS로 어떻게 그릴지 명령한다. 선언적(declarative)인것보다 명령적(imperative)이다.[^fn1]
Canvas는 래스터(raster)이며, 프로그래밍적(programmatic)이다.[^fn1]<br/>

"retained mode graphic"인 SVG와는 다르게, Canvas는 "immediate mode"그래픽이다.[^fn6] Immediate mode 그래픽 렌더링이란, "fire and forget"모델이라고도 하는데, 
그래픽을 스크린에 다이렉트로 그리고, 그래픽을 그린 후에는 그래픽에 대한 정보를 유지하지 않는다. 어떤 컨텍스트를 가지고 그렸는지, 렌더된 그래픽에 대한 정보를 들고 있지 않는다고 한다. 
새로운 프레임이 필요할 때, 개발자가 그전에 그렸던 모든 drawing 코맨드를 다시 불러서 그려야 한다.

[//]: # (immediate mode graphics)

```html
<canvas id="myCanvas" width="578" height="200"></canvas>
<script>
  var canvas = document.getElementById('myCanvas');
  var context = canvas.getContext('2d');
  var centerX = canvas.width / 2;
  var centerY = canvas.height / 2;
  var radius = 70;

  context.beginPath();
  context.arc(centerX, centerY, radius, 0, 2 * Math.PI, false);
  context.fillStyle = 'green';
  context.fill();
</script>
```
위 코드는 가로, 세로 578, 200 사이즈의 Canvas를 그리고, 
canvas 안에 arc path를 그리도록 지정한다. arc path는 중앙 포인트가 (289,100)에 위치하며, 반지름이 70이며, 각도 0부터 360도까지 그린다. 
즉, 반지름 70인 원을 그린다. 이 원은 녹색으로 채워진다.


## pixels 
Canvas는 픽셀을 직접 다룬다.
인터렉티브한 동작이나 복잡한 디테일/gradient 은 Canvas 영역이라고 한다. 
이 이유 때문에 SVG 보다 Canvas로 만든 게임이 많다. 

## 이벤트 
렌더된 이미지에 대한 이벤트 처리가 SVG보다 복잡하다. 
사용자가 마우스 포인터를 이미지 특정 영역에 올려놨다고 가정한다. Canvas는 렌더된 그래픽에 대해 정보를 들고 있지 않기 때문에, 개발자가 직접 `<canvas>`태그 안 
`mouseX`, `mouseY` 좌표를 계산해야 한다. 해당 좌표를 기준으로, 어떤 그래픽에 대한 이벤트를 처리할지 개발자가 직접 계산해야 한다. 
[`isPointInPath` API](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/isPointInPath)를 제공해서
요즘은 좀 더 쉽게 이벤트를 처리할 수 있다고 하는데, 이 API도 마지막 path에 대한 처리만 가능하다고 한다.



# Canvas + SVG 
Canvas와 SVG는 상호 배타적인 관계가 아니다. 
`<svg>`는 `<canvas>` 안에 그려질 수 있다. 

# 결론 w. 비교표

| SVG                               | Canvas                                    |
|-----------------------------------|-------------------------------------------|
| Shape 베이스                         | Pixel베이스                                  |
| 여러개의 그래픽 엘리먼트가 DOM 안에 들어감         | 1개의 HTML 엘리먼트                             |
| script, css 를 사용하여 변경 가능          | script 만 사용 가능                            |
| 이벤트 모델/사용자 인터렉션이 추상화됨 (rect,path) | 이벤트 모델/사용자 인터렉션이 아주 세부적으로 쪼개짐 (x,y 계산해야함) |
| 적은 오브젝트 개수(<10k)/큰 화면일 때 성능적으로 나음 | 많은 오브젝트 개수(>10k)/작은 화면일 때 성능적으로 나음        |

장단점을 비교한 표다:


|    | SVG                                     | Canvas                       |
|----|-----------------------------------------|------------------------------|
| 장점 | UI/UX 애니메이션 하기 쉬움                       | 3D와 같이 딥하게 보여줄때 사용하기 좋음      |
| ^^ | 디버깅하기 더 쉬움                              | object 개수가 많을 때(>10k) 성능이 좋음 |
| ^^ | 접근성이 더 좋음                               |                              |
| ^^ | 큰 화면에 보여줄 때 깨지지 않고 렌더 타임이 canvas보다 적게들음 |                              |
| 단점 | 오브젝트 개수가 많아질때 DOM 노드개수도 많아짐             | 접근성 좋게 만들기 어려움               |
| ^^ | 복잡한 DOM                                 | 메모리를 많이 쓴다는 의견 존재            |
| ^^ | 오브젝트 개수가 많아지면(>10k)성능 떨어짐               |                              |

- obj가 많으면 (1,000개 이상) Canvas가 더 빠르다는 의견이 많다.
- canvas를 사용할 때 화면이 커질수록 그려야할 pixel 개수가 증가하게되어서, 렌더 시간이 늘어나 성능적으로 떨어진다고 한다.

- 디폴트로 SVG를 사용하고, [SVG를 못 쓸 때 Canvas를 쓰라는 의견](https://css-tricks.com/when-to-use-svg-vs-when-to-use-canvas/#aa-svg-is-the-default-choice-canvas-is-the-backup)도 존재함. 
- 작은 단순한 아이콘, 정적인 이미지 ➡️  SVG 
- 복잡한 애니메이션과 많은 인터렉션, 엘리먼트 이동이 많은 게임 ➡️  Canvas.
- 날씨화면과 같이 실시간 데이터를 보여주는 화면 ➡️  [Canvas](https://learn.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/samples/gg193983(v=vs.85)#real-time-data)
  <br/>(Canvas가 DOM에 변경을 일으키지 않고 빠르게 그리고 지울 수 있다고 함. 물론, CPU 속도와 JS 엔진에 따라 다르다고 함)


# References
[^fn1]: [CSS-TRICKS: When to Use SVG vs. When to Use Canvas](https://css-tricks.com/when-to-use-svg-vs-when-to-use-canvas/){:target="_blank"}
[^fn2]: [LogRocket: Using SVG vs. Canvas: A short guide](https://blog.logrocket.com/svg-vs-canvas/){:target="_blank"}
[^fn3]: [mdn: Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API){:target="_blank"}
[^fn4]: [Tweaking your SVG file for a better Google presence](http://www.joningram.co.uk/article/svg-search-engine-friendly-accessible/){:target="_blank"}
[^fn5]: [SVG vs Canvas Scaling](https://codepen.io/osublake/pen/WzbKLQ){:target="_blank"}
[^fn6]: [Microsoft: SVG vs canvas: how to choose](https://learn.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/samples/gg193983(v=vs.85)#svg){:target="_blank"}
