---
title: '페이지를 다양한 방법으로 캡처하자 (feat. puppeteer)'
last_modified_at: 2023-10-31T23:30
categories:
  - frontend
tags:
  - puppeteer
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

## puppeteer
[puppeteer](https://github.com/puppeteer/puppeteer)는 Google Chrome팀에서 처음 만든 노드 라이브러리다. <br/>
테스트나 오토로 돌릴 때 쓰인다고 한다. 
GUI가 없는 headless chrome browser처럼 동작해서 캡처하고 싶은 영역도 지정할 수 있고, 실제 사용자가 클릭하면 뜨는 것처럼 인터랙션도 줄 수 있다고 한다.



```npm install puppeteer```

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

  const pdfPath = path.join(__dirname, 'webpage.pdf');
  // Capture the webpage as a PDF
  // await page.pdf({ path: pdfPath});
  // await page.pdf({ path: 'webpage.pdf', format: 'A4' });
  await page.screenshot({ path: path.join(__dirname, 'webpage.png') });

  await browser.close();
})();
```

# References
