---
layout: post
title: "[JavaScript] script태그 async와 defer 속성의 차이점"
tags: JavaScript
categories: JavaScript 
---
# Difference of async and defer
* * *
이번 포스팅에서는 HTML 태그 중 script라는 태그의 속성(Attribute)인 **async 와 defer의 차이점**에 대해 알아본다.  
HTML에서 자바스크립트를 포함할 때 어떻게 포함하는 것이 효율적인지에 대해서 알아본다.  
정리하는데 참고한 사이트는 포스팅 최하단의 <a style="color: black" href="#ref">Reference</a> 부분에 작성했다. 감사의 인사를 전합니다.

## No Use Any Attributes in head tag
* * *
![head](/images/js-head.png)
단점: main.js파일의 용량이 크거나 네트워크가 느린 경우, 사용자가 웹사이트를 보는데 많은 시간이 걸린다.

## No Use Any Attributes in body tag
* * *
![body](/images/js-body.png)
장점: 사용자가 웹 사이트를 빨리 볼 수 있다.  
단점: 웹 사이트가 자바스크립트에 의존적이라면, 사용자가 의미 있는 웹사이트를 보느데까지 많은 시간이 걸릴 수 있다.

## Use async Attribute in head tag 
* * *
![async](/images/js-async.png)
boolean type의 속성 값으로 선언하는 것만으로 true를 의미한다.  
브라우저가 **script태그를 읽는 순간 js 파일을 fetching하고 fetch가 완료되면, HTML 파싱을 멈추고 js파일을 실행**한다.

**장점**
- js파일을 다운로드 받는 시간을 절약할 수 있다.

**단점**
- HTML 파싱이 끝나기 전에 js 파일을 실행하기 떄문에, 만약 자바스크립트 파일에서 돔 요소를 조작하면 그 시점에 html에 요소가 정의되어 있지 않을 수도 있다.
- 사용자가 웹 페이지를 보는데까지 많은 시간이 걸릴 수 있다.


## Use defer Attribute in head tag 
* * * 
![defer](/images/js-defer.png) 
브라우저가 script 태그를 읽는 순간 js 파일을 fetching하면서 HTML 파일도 계속 파싱한다.  
그리고 HTML이 다 파싱되면 그 떄 js파일이 실행된다.  
**가장 효율적이고 안전한 방법**

## async 와 defer 속성을 가진 태그를 여러 줄 사용하는 경우
* * *
**async**  
![multiple-async](/images/js-async2.png)

코드 상의 순서와 상관없이 가장 먼저 fetch되는 js파일을 실행하게 된다. 그래서 **개발자의 의도대로 할 수 없을 수**도 있다.
* * *
**defer**
![multiple-defer](/images/js-defer2.png)

js 파일들을 먼저 fetch해 두고, HTML 파싱이 끝나면 그 때 코드 상 순서대로 js 파일들을 실행한다. 그래서 **개발자의 의도대로 js파일을 실행할 수** 있다.

<h2 id="ref">Reference</h2>
* * *
<a href="https://www.youtube.com/watch?v=tJieVCgGzhs&list=PLv2d7VI9OotTVOL4QmPfvJWPJvkmv6h-2&index=2">자바스크립트 2. 콘솔에 출력, script async 와 defer의 차이점 및 앞으로 자바스크립트 공부 방향 | 프론트엔드 개발자 입문편 (JavaScript ES5+)</a>  
