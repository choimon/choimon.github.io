---
title: 'SVG vs Canvas 언제 써야하나 (feat. Chart)'
last_modified_at: 2023-10-21T20:00
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
SVG는 벡터(vector)이며 선언적(declarative)이다.[^fn1]

벡터 아트가 필요할땐 SVG를 사용한다.
벡터 아트는 보기에 깔끔하게 떨어지고(crisp, 테두리가 지글지글거리지않음), 확대해도 깨지지 않는다. 그리고, 벡터아트는 JPG와 같은 래스터 그래픽보다 보통 파일 크기가 작다. 

페이지 로고를 쓸 때 SVG를 흔하게 사용한다. SVG 코드는 HTML파일에 바로 삽입할 수 있고, 선언적으로 어떻게 그릴지 명시할 수 있다. 
```html 
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>
```
위 코드는 가로, 세로 100 사이즈의 SVG를 그리고, 그 안에 중앙 포인트가 (50,50)에 위치하며, 반지름이 40인 원을 그린다.
이 원은 녹색 테두리와 노란색으로 채워진다.


그려야할 그래픽이 유연하고(flexible) 반응형이여야한다면(responsive) SVG를 사용한다.

## DOM 
SVG는 HTML DOM에 존재한다. 따라서, SVG는 HTML DOM이벤트를 가질 수 있다.
SVG `<circle>`은 `<div>`처럼 `click`, `mouseDown`과 같은 DOM 이벤트를 가질 수 있다.

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
위 코드는 `circle`을 클릭시 색이 빨간색으로 바뀌는 이벤트리스너를 등록한다. 

# Canvas
Canvas는 자바스크립트의 drawing API이다.[^fn3]
HTML에 `<canvas>`엘리먼트를 넣고, 자바스크립트로 그림을 그린다.
JS로 어떻게 그릴지 명령한다. 선언적(declarative)인것보다 명령적(imperative)이다.[^fn1]
Canvas는 래스터(raster)이며, 프로그래밍적(programmatic)이다.[^fn1]

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

# References
[^fn1]: [CSS-TRICKS: When to Use SVG vs. When to Use Canvas](https://css-tricks.com/when-to-use-svg-vs-when-to-use-canvas/){:target="_blank"}
[^fn2]: [LogRocket: Using SVG vs. Canvas: A short guide](https://blog.logrocket.com/svg-vs-canvas/){:target="_blank"}
[^fn3]: [mdn: Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API){:target="_blank"}