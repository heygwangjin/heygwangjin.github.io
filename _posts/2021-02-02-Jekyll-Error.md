---
layout: post
title: "[Jekyll] 지킬 포스팅 HTTP 404 page not found 오류 해결법"
tags: Jekyll
---

![404](/images/404error.png)
<p>깃허브 페이지를 지킬 테마를 적용하여 사용 중이다.<br>포스팅을 태그와 카테고리를 통해서 웹 페이지를 열면 위와 같은 오류가 발생했다.<br>
수 시간의 삽질을 통해 알아낸 해결법을 공유하고자 한다.</p>

## 깃허브 리모트 레포지토리명과 프론트매터 categories가 동일한지 확인하기 
* * *
<p>문제는 바로 깃허브 <strong>리모트 레포지토리명과 categories 이름이 동일</strong>해서 발생했다.<br>
Github에 javascript30이라는 리모트 레포지토리를 생성한 후, 웹사이트 배포를 했다.<br>
그래서, username.github.io/javascript30/ 이라는 url을 통해서 폴더안의 html 파일을 띄워 줬었는데, categories에 javascript30이라는 카테고리를 만들어서 같은 url을 사용하게 됐다.</p>
![remoteRepo](/images/githubremote.png) 
```markdown
---
layout: post
title: "[JavaScript30] 자바스크립트와 CSS로 애플 시계 만들기"
tags: Web JavaScript30
categories: JavaScript30
---
```

위와 같은 설정에서 아래의 이미지처럼**categories를 리모트 레포지토리명과 겹치지 않게 설정해 줌으로써, 이슈가 해결** 됐다.<br>
tag는 동일해도 상관이 없다. 아마, 지킬 테마를 만든 사람이 categories 이름을 통해서 url을 생성하게 코드를 작성해 놔서 그런 것 같다.
```markdown
---
layout: post
title: "[JavaScript30] 자바스크립트와 CSS로 애플 시계 만들기"
tags: Web JavaScript30
categories: Challenge
---
``` 

![url](/images/url.png)
