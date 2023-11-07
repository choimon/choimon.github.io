---
title: '페이지를 다양한 방법으로 캡처하자 (feat. Puppeteer)'
last_modified_at: 2023-11-08T01:14
categories:
  - frontend
tags:
  - puppeteer
  - playwright
  - nodejs
toc: true
toc_sticky: true
---


# 개요
웹 페이지를 캡처하는 방법을 설명한다. 

물론, 사용자가 웹 브라우저를 통해 페이지를 접속해서 직접 개발자도구 캡처 기능을 사용할 수도 있다. 
이 글은 프로그램으로 어떻게 캡처하는지 설명한다. 

클라이언트 코드 단과 서버 단 모두 가능하다.


# 서버
백엔드에서 캡처를 실행해야하는 경우를 설명한다. 

## Puppeteer
[Puppeteer](https://github.com/puppeteer/puppeteer)는 Google Chrome팀에서 2018년에 처음 만든 노드 라이브러리다. <br/>
테스트나 오토로 돌릴 때 쓰인다고 한다. 

GUI가 없는 headless chrome browser처럼 동작해서 캡처하고 싶은 영역도 지정할 수 있고, 실제 사용자가 클릭하면 뜨는 것처럼 인터랙션도 줄 수 있다.


설치 명령어:
```npm install puppeteer```

전달한 URL페이지를 1920x1080 viewport 사이즈로, png 파일로 캡처하는 예제:
```javascript

const puppeteer = require('puppeteer');
const path = require('path');


(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  // Navigate to the URL you want to capture
  await page.goto('https://echarts.apache.org/examples/en/editor.html?c=area-basic');

  // Optionally, you can wait for some time or events to ensure the page is fully loaded


  // Set the viewport to match the entire page's dimensions
  await page.setViewport({ width: 1920, height: 1080 }); // Adjust these dimensions as needed

  //const pdfPath = path.join(__dirname, 'webpage.pdf');
  // Capture the webpage as a PDF
  // await page.pdf({ path: pdfPath});
  // await page.pdf({ path: 'webpage.pdf', format: 'A4' });
  await page.screenshot({ path: path.join(__dirname, 'webpage.png') });

  await browser.close();
})();
```


특정 크롬 또는 Chromium 버전을 지정해서 테스트할 수 있다.
```javascript
const browser = await puppeteer.launch({executablePath: '/path/to/Chrome'});
```
크롬 브라우저 말고도 Firefox도 지원한다고 하는데 아직 실험단계인 것 같다..[^fn1]

## Playwright

Microsoft에서 만든 웹 테스트 및 automation 프레임워크다. Puppeteer보다 비교적 최근인 2020년에 나왔다고 한다.
Puppeteer처럼 headless browser로 동작해서 캡처하고 싶은 영역도 지정할 수 있고, 실제 사용자가 클릭하면 뜨는 것처럼 인터랙션도 줄 수 있다.

크롬 위주인 Puppeteer와 다르게 크로스 브라우저를 지원한다고 대문짝만하게 광고한다.
Chromium, Firefox, Webkit 모두 테스트 대상으로 지원한다. 크로스 브라우저 지원이 필요할때는 Puppeteer보다 Playwright를 사용하는게 좋을 것 같다.

JS만 지원하는 Puppeteer와 다르게 Playwright은 JS, Java, Python, .NET C#도 지원한다고 한다.

```shell
npm install playwright # playwright 라이브러리 설치
npx playwright install   # 브라우저 설치
```
playwright은 버전별로 자신들이 만드는 특정 버전의 browser binary파일을 설치해줘야한다.[^fn3]
`npx playwright install` 명령어를 통해 설치할 수 있다. 명령어를 실행해보니 종류별로 다 설치하는 것 같아서 chromium만 설치했다.


전달한 URL페이지를 1920x1080 viewport 사이즈로, png 파일로 캡처하는 예제:
```javascript
const { chromium } = require('playwright');

(async () => {
  const browser = await chromium.launch();
  const page = await browser.newPage();

  await page.setViewportSize({ width: 1920, height: 1080 });

  await page.goto('https://echarts.apache.org/examples/en/editor.html?c=area-basic');

  // Wait for the page to load
  await page.waitForTimeout(5000);

  // await page.pdf({ path: 'webpage.pdf', format: 'A4' });
  await page.screenshot({ path: 'webpage.png' });

  // Close the browser
  await browser.close();
})();
```

[//]: # (browser는 Chromium/Firefox/Webkit 인스턴스를 뜻한다. 브라우저 인스턴스를 띄우고&#40;launch&#41; 원하는 업무를 수행하고 끝낼때는 browser는 닫아줘야한다 &#40;`browser.close&#40;&#41;`&#41;)
[//]: # (https://www.cuketest.com/playwright/docs/core-concepts)


# client
브라우저에서 돌아가는 클라이언트 코드안에서 캡처하는 방식을 설명한다. 

## html2canvas
html2canvas는 웹 페이지를 캡처해주는 JS 라이브러리다. <br/>
html2canvas는 사실 페이지를 "스크린샷"하지는 않고, 페이지의 DOM을 돌면서, 엘리먼트정보를 습득해서,html과 css를 렌더링해서 해당 페이지의 "카피본(representation)"을 canvas에 그린다. <br/>
이렇게 그려진 canvas를 이미지로 변환해서 다운로드 받을 수 있다. 

이렇게 때문에 html2canvas 스크립트가 이해하는 property들만 제대로 렌더해서 캡처할 수 있다.
html2canvas가 제대로 이해하지 못한 CSS 속성들은 캡처되지 않는다. html2canvas가 이해하는 CSS 속성은 [여기](https://html2canvas.hertzen.com/features)에서 확인할 수 있다.

html2canvas 스크립트가 이해하는 엘리먼트/이미지는 same origin에 존재해야한다.cross-origin 콘텐츠는 html2canvas가 읽을 수 없어진다. 

```npm install html2canvas```


```javascript
import html2canvas from 'html2canvas';

const screenshotTarget = document.body;
html2canvas(screenshotTarget).then(function(canvas) {
    // document.body.appendChild(canvas);
    const base64image = canvas.toDataURL("image/png");
    downloadURI(base64image, "capture-page.png");
});
  


function downloadURI(uri, name) {
  var link = document.createElement("a");
  link.download = name;
  link.href = uri;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
}

```


# References
[^fn1]: [puppeteer: cross-browser support](https://pptr.dev/faq#q-what-is-the-status-of-cross-browser-support){:target="_blank"}
[^fn2]: [Playwright vs Puppeteer: Which to choose in 2023?](https://www.browserstack.com/guide/playwright-vs-puppeteer){:target="_blank"}
[^fn3]: [Playwright browsers](https://playwright.dev/docs/browsers){:target="_blank"}
[^fn4]: [Playwright screenshots](https://playwright.dev/docs/screenshots){:target="_blank"}

