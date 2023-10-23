---
title: '차트 라이브러리 SVG vs Canvas 뭘 써야하나 (feat. Echart)'
last_modified_at: 2023-10-23T22:59
categories:
  - frontend
tags:
  - svg
  - canvas
toc: true
toc_sticky: true
---

기존에 작성한 [SVG vs Canvas 언제 써야하나](/frontend/svg-canvas/)에 이어지는 글이다.
전 글에는 SVG와 Canvas 기술 각각에 대해 설명했다면 이 글은 Chart를 사용할 때 SVG와 Canvas 렌더러 중 어떤 것을 선택해야하는지 다룬다.
{: .notice--info}

# 개요
차트 라이브러리[^fn1]를 고를 때 다양한 기준이 있을 수 있다. 지원하는 차트 타입, Vue와 같이 기존에 사용하던 프레임워크 컴포넌트 지원, 커스터마이징 가능 여부, 라이센스, 성능, 컬러, 애니메이션과 같이 비쥬얼, 활발한 커뮤니티 등이 있다.
이 밖에도 해당 차트 라이브러리가 SVG를 사용하는지 Canvas를 사용하는지 알아볼 수 있다. 보통 "난 SVG 차트 라이브러리를 쓸꺼야!"라고 시작하고 차트 라이브러리를 검토하진 않을 것이다.
하지만, 차트 라이브러리를 검토하다보면, 해당 라이브러리가 차트를 그릴 때 SVG인지 Canvas인지에 따라 분류할 수 있다. (Apache Echart, ZingChart, Dojo처럼 SVG와 Canvas 둘다 지원하는 라이브러리도 존재한다).
사용자가 차트를 사용하는 용도와 환경에 따라 SVG와 Canvas 중 어떤 렌더러를 사용하는게 적합한지 알아보자.

번외로 필자가 사용하는 Echart 라이브러리를 기준으로 SVG와 Canvas 렌더러를 비교한 내용도 포함한다.


# 데이터 양 
차트에 그려야할 데이터 양이 많으면 canvas를 사용하는 것이 좋다는 의견이 우세하다. <br/>
SVG는 보통 path를 만들지 않는 한 데이터 포인트당 DOM node를 생성한다고 한다. [^fn2]
데이터 양이 많을 때 SVG를 사용하면, DOM node가 많아지고, DOM tree 사이즈가 커지면서 성능 문제로 이어질 수 있다. 
이렇게 커지는 DOM 트리 문제를 우회할 수 있는 방법으로, path를 사용해 여러 데이터 포인트를 하나의 path로 만들 수 있다. Echart인 경우 line chart를 SVG렌더러로 그릴 때 한 line당 한 path를 그리는 것으로 확인했다.[^fn3]
하지만 차트 타입이 heatmap과 같은 복잡한 형태라면 path를 사용하기 어려워지는 경우가 많다. 또한, path를 사용하게 된다면 interaction을 잃을 수 있다.


여기서 말하는 데이터 양이 많다는 기준이 뭘까? Echart경우 1,000개 이상을 말한다.[^fn4]


# 차트 타입 
복잡한 차트를 그릴 때는 보통 Canvas 타입이 많은 것 같다. 
단순하게 그리는 line 차트인 경우 SVG path를 사용할 수 있다. 하지만 heatmap같은 경우, svg를 사용하게되면 각 박스별로 DOM node가 생성되기 때문에, 데이터 양이 많아지면 성능 문제가 발생할 수 있다.


# 결론
차트에 그릴 데이터 양, 차트 타입, 어떤 visual effect가 들어가는지, 어떤 디바이스에서 차트를 볼 건지 등 케이스에 따라 적합한 렌더러가 다른것으로 보인다.

- 데이터 양 적음 ➡️ SVG
- 데이터 양 많음, 인터렉션 없음 ➡️ SVG에 path 사용하거나 Canvas
- 데이터 양 많음, 인터렉션 많음 ➡️ Canvas하거나 SVG 좀 어렵게 사용. 이러나 저러나 복잡함
- 인터렉션 꼭 필요함 ➡️ SVG가 보통 우세함. 근데, 요즘 Canvas 라이브러리도 인터렉션 지원하는 경우가 많음[^fn5].
- 복잡한 차트 타입 ➡️ Canvas
- 차트를 보는 디바이스가 모바일이라면 ➡️ SVG 
- 차트를 보여주는 화면이 아주 큰 화면이라면 ➡️ SVG (확대해도 깨지지 않고, canvas를 큰 화면에 사용할 경우, 렌더해야할 픽셀이 많아지기 때문에 렌더 시간이 느려진다)

위 결론은 일반적으로 적용되는것이고 라이브러리에 따라 다를 것이다. <br/>
Echart인 경우 SVG를 사용할 때 성능 문제가 있었는지, Echart 5.3버전 이후로 [Virtual DOM을 통해 SVG 엘리먼트를 업데이트 하도록 변경해 성능을 대폭 향상](https://github.com/ecomfe/zrender/pull/836)했다고 한다.
무조건 Canvas를 써야하는 특이 경우가 아니라면, SVG/Canvas 상관없이 다른 기준으로(비쥬얼적인면, 지원하는 차트 타입) 차트 라이브러리를 우선 결정하는 것이 나을 것 같다. 
차트 라이브러리를 선택 후 테스트를 해봐서 성능 문제가 생겼을 때 렌더러를 고려해서 변경하면 될 것 같다.


# Echart 
Echart는 SVG와 Canvas를 모두 지원한다. 초기에는 Canvas 만 지원했으나 v4.0부터 SVG를 지원하기 시작했다.
Echart 공식 문서에[^fn4] 따르면, 엘리먼트가 많은 차트인 경우 Canvas렌더러가 적합하다고 한다. 여기서 말하는 엘리먼트가 많은 차트는, 
히트맵이나, 라인 차트의 데이터 포인트가 많은 경우, geo scatter plot, parallel coordinate plot과 같은 경우라고 한다. 

SVG는 Canvas보다 memory를 덜 사용한다. 차트를 보여주는 기계가 모바일 장치라면, SVG를 추천한다.
아래는 Echart공식 문서의 가이드다[^fn4]: 

Echart에서는 소프트웨어 뿐만 아니라 하드웨어 환경도 고려해야한다고 한다.
- sw, hw모두 좋은 환경 + 데이터 너무 많지 않음 ➡️ SVG, Canvas둘다 가능
- sw, hw환경 안 좋음 + 많은 Echart 인스턴스가 만들어져하고,브라우저가 crashing되는 경우 ➡️ SVG. (canvas사용했을 때 브라우저 crashing되는 경우는, canvas 개수가 메모리를 차지해 하드웨어 스펙을 초과해 생길 가능성이 높다고 한다)
- 많은 양의 데이터 (1000개 초과) ➡️ Canvas를 추천
- 특정 효과(trail effect, 히트맵에 blending 효과 등)는 Canvas에서만 가능하다 ➡️ Canvas만 지원하기 때문에 Canvas


# References
[^fn1]: [Flatlogic: Best 19+ JavaScript Chart Libraries to Use in 2023](https://flatlogic.com/blog/best-19-javascript-charts-libraries/){:target="_blank"}
[^fn2]: [stack overflow: SVG vs HTML5 Canvas Based Charts](https://stackoverflow.com/questions/28083421/svg-vs-html5-canvas-based-charts){:target="_blank"}
[^fn3]: [Echarts example: Stacked Line](https://echarts.apache.org/examples/en/editor.html?c=line-stack){:target="_blank"}
[^fn4]: [Echarts: Render with SVG or Canvas](https://apache.github.io/echarts-handbook/en/best-practices/canvas-vs-svg/){:target="_blank"}
[^fn5]: [chartjs-plugin-annotation](https://github.com/chartjs/chartjs-plugin-annotation){:target="_blank"}
