---
title: Web에서 Rendering 기술 차이
description: Client Side Rendering과 Server Side Rendering의 차이
categories:
 - Web
tags: web 
---

😄 혹여나 잘못된 정보가 있다면, 알려주세요. 저에게 큰 힘이 됩니다! 😄

### 개요
시작하기 앞서, "Web Rendering" 기술 방식에는 `Client Side Rendering(CSR)` 방식과 `Server Side Rendering(SSR)` 방식이 존재합니다. <br>
<br>
우리가 흔히 알고 있는 프론트엔드 프레임워크/라이브러리는 Vue와 React가 있습니다. <br> 
이는 대표적인 **Single page application(SPA) framework**으로 `Client Side Rendering(CSR)` 방식으로 View를 만듭니다. <br>
반면, PHP는 **Multi page application(MPA)** 방식으로 `Server Side Rendering(SSR)` 방식으로 View를 만듭니다. <br>

그렇다면, "요즘 프론트엔드 채용공고에 Vue, React가 많으니, CSR이 좋은 거네~" 라고 생각하실 수 있습니다.<br>
본 글에서 각각의 방식에 대한 차이와 장단점에 대해 알아보고, 모던 웹페이지는 어떤 방식을 채택하는 지 알아 보겠습니다.<br>
(참고로 일반적으로 SPA = CSR, MPA = SSR 이라고 공식처럼 얘기되지만, 정확히는 MPA가 주로 SSR을 사용하는 것입니다.) 



### 들어가기 앞서...
* `SPA(Single Page Application)` : 한 개의 페이지로 구성된 어플리케이션 
* `MPA(Multiple Page Application)` : 여러개의 페이지로 구성된 어플리케이션
* `SSR(Server Side Rendering)` : 서버 측에서 렌더링하는 방식
* `CSR(Client Side Rendering)` : 클라이언트 측에서 렌더링하는 방식

* 렌더링(Rendering) : 내용을 브라우저 화면에 표시하는 것

---


결론부터 말하자면, 제가 이해한 바로 비유하자면 **SSR은 완제품, CSR은 조립식**이라고 설명드릴 수 있을 거 같습니다.<br>
이를 염두해두고 글을 읽으시면 조금 더 이해하기 쉬울겁니다.(제 생각입니다.)

### SSR(Server Side Rendering)
SSR(Server Side Rendering)은 이름에서 알 수 있듯이, **Web Server**에서 View(HTML)를 생성합니다. <br>
SSR은 페이지마다 하나의 HTML을 가집니다. (페이지마다 해당하는 HTML **완성품**을 이미 가지고 있습니다.)


#### SSR의 렌더링 방식 <br>
![img10](/img/ssrrender.png)
> 1.브라우저에서 URL 입력을 통해 서버에 HTTP 요청을 보낸다.<br> 2. 서버가 요청을 받아 해당 페이지를 만들기 시작한다. <br> 3. 필요한 data fetching(데이터 가져오기)을 미리 해서 빈 페이지가 아닌 초기 콘텐츠가 로딩된 페이지를 만들어준다. <br> 4. 브라우저는 서버에서 만들어 준 HTML 페이지를 받아 페이지를 DOM에 그린다. (페이지가 사용자에게 보이는 시점) <br> 5. 페이지를 그리며 태그를 통해 JS, CSS 파일 등을 로딩한다. <br> 6. 로딩된 JS를 실행한다.

```
[참고 사항] 5번 과정에서 JS, CSS 파일 등을 태그를 통해 로딩한다고 하였습니다.
그렇다면 사용되는 태그는 무엇일까요??

index.html을 보시면, <script>, <link> 태그들이 있는 걸 보실 수 있는데, 이를 통해 각각 JS, CSS 파일을 로딩합니다.
(개발자 도구(F12)를 눌러 확인하실 수 있습니다.)
```

#### SSR의 특징

페이지 전환이 발생될 때 마다 Client가 Server에 View를 요청하고, Server는 요청 사항을 생성 후 Client에게 보내줍니다. <br>
이러한 과정으로 인해 발생하는 장점과 단점은 아래와 같습니다.


#### SSR 정리 <br>
장점 <br>
> 1. 초기 로딩속도가 빠릅니다. (서버에서 한번에 완성된 페이지를 받아오기 때문입니다.)
> 2. 검색 엔진 최적화(SEO) 사용 가능합니다.

단점 <br> 
> 1. 페이지 이동 시 화면 깜빡임이 발생합니다..
> 2. 페이지 이동 시 변경이 불필요한 데이터도 같이 로드됩니다. (완제품이라 자그만한 문제라도 제품 전체를 서버에 반품처리해야 합니다.)
> 3. 페이지 전환 속도가 CSR에 비해 상대적으로 늦습니다.
> 4. 페이지 요청이 빈번해 질수록 CSR에 비해 Server에 부담이 커지게 됩니다.

---

### CSR(Client Side Rendering)

브라우저(Client)에서 JavaScript에 의해 View(HTML)을 동적으로 생성합니다. <br> 
SSR과는 다르게 하나의 HTML 파일로 모든 페이지를 구성합니다. (1개의 HTML에 JS를 통해 **조립**합니다.)


#### CSR의 렌더링 방식 <br>
![img11](/img/csrrender.png)
> 1.브라우저에서 URL 입력을 통해 서버에 HTTP 요청을 보낸다.<br> 2. 빈 HTML을 내려받습니다. (다만, 여러 JS, CSS파일을 불러올 수 있는 링크들이 있습니다.) <br> 3. 브라우저가 JS 코드를 실행합니다. (이때, JS 코드 안에서 Vue, React 등이 구동됩니다.) <br> 4. VirtualDOM 구성이 완료되면, 이를 브라우저의 DOM에 붙입니다. <br> 5. 브라우저가 페이지를 그립니다.


#### CSR의 특징

페이지 전환 시, 필요한 부분에 대해서만 재조립하여 페이지를 보여줍니다. <br>


장점 <br> 
> 1. 페이지 전환이 SSR보다 상대적으로 빠릅니다. (사용자 경험 향상: 필요한 부분만 후다닥 조립해서 보여줍니다.)
> 2. 서버와 클라인언트 간의 데이터 양과 트래픽이 SSR에 비해 현저히 감소합니다.

단점 <br>
> 1. 초기 구동 속도가 느립니다. (JS파일을 번들링해서 한 번에 받아오기 때문입니다. : 조립하기 위한 부품과 조립하는 시간이 걸리기 때문입니다.)
> 2. 검색 엔진 최적화(SEO)가 어렵습니다. (돠긴 합니다.)

---


### 검색 엔진 최적화(Search Engine Optimization)


각각의 방식의 장단점에서 빠지지 않고 등장하는 SEO란 녀석은 얼마나 중요하길래 매번 나오는 지 알아보겠습니다. <br>

검색 엔진 최적화(SEO)는 홈페이지 혹은 콘텐츠를 검색 결과의 상단에 위치시키는 작업을 말합니다. <br>
검색 엔진은 "크롤링"과 "인덱싱"을 통해 정보를 카테고리화 합니다. <br>


#### 동작 과정
검색 엔진은 각 페이지들에 노출된 `index.html`에 달린 `meta tag`와 `title`들을 파싱하며 어떤 정보를 노출시킬지 정하게 됩니다.

#### 왜 SEO가 중요한가?
SEO가 제대로 되지 않았다면, 검색해도 안 나올 수 있습니다. <br>
예를 들어, Apple 사이트 주소를 모르는 상황이라면, 대부분 검색창에 apple이라고 칩니다. <br>
이때, 만약 사이트가 나오지 않는다면? 다른 사람에게 물어보거나 다른 사이트를 거쳐 들어가는 방법밖에 없을 것입니다. <br>

이러한 문제는 BtoC 서비스를 진행하는 회사의 경우 치명적일 것입니다. <br>
뿐만 아니라, 내가 열심히 서비스를 만들었으나 아무도 모르는 그런 상황이 온다면? 눈물이 나긴 합니다.



#### CSR이 검색 엔진 최적화가 어려운 이유?

|SSR|CSR|
|------|---|
|![img15](/img/ssrseo.png)|![img16](/img/csrseo.png)|
|사진출처 : [SPA 에서 SEO 적용하기 :: 마이구미](https://mygumi.tistory.com/385)|


SSR은 각각의 페이지('/', '/a', '/b', '/c')를 가지므로 html이 여러개입니다. <br>
이로 인해 검색엔진 크롤러는 해당 페이지에 대한 정보들을 파싱할 수 있습니다. <br>

CSR은 각 페이지('/a', '/b', '/c')가 루트 페이지('/')를 바라보고 있습니다. <br>
이로 인해 검색엔진 크롤러는 HTML을 파싱해도 모두 똑같은 파일이 나오게 됩니다. <br>

결국, 검색서비스 노출 및 SNS 공유 기능 등이 원하는대로 동작할 수 없게 됩니다.


#### CSR에 SEO를 적용해도 SSR에 비해 불리한 이유

어쨋든, 어렵지만 되긴 하는데 그럼에도 CSR이 SSR에 비해 SEO를 적용해도 불리한 이유는 무엇일까요? <br>

저는 이를 `조립식의 한계`라고 표현하고 싶습니다.

CSR을 사용하면 View를 생성하기 위해서 JS가 필요하고(그 전까지는 빈 페이지이기 때문에 View가 완성되지 않아서), <br>
View를 생성하기 전까지는 크롤링을 할 수 없는 시점이 발생합니다. <br>
이는 상대적으로 검색 엔진 서비스에 노출되는 것이 줄어듦을 말합니다.

(옆집의 SSR은 벌써 만들어져서 검색 엔진에 노출되고 있는데, 우리 집 CSR은 아직 조립하고 있는 그런 상황입니다.)


#### 그래서 모던 웹페이지는 어떻게 극복할까?

두 가지 렌더링 방법을 적절하게 사용하여 첫 번째 페이지 로딩에서는 SSR을 사용하고, 그 후에 모든 페이지 로드에는 CSR을 활용하는 방법을 많이 사용한다고 합니다.

---


### Wireshark를 통한 네트워크 요청&응답 확인
Wireshark는 자유 및 오픈 소스 패킷 분석 프로그램입니다.
패킷을 분석하는 Tool이기 때문에 사용자에 따라 사용 목적이 상이하지만, <br>
여기선, 클라이언트와 서버 간 View를 요청/응답하는 패킷을 확인하는 정도로만 사용합니다.


* MPA에서의 렌더링
Server Side Rendering의 네트워크 요청/응답을 확인해봅시다.

1. 최초 접속 <br>
![img](/img/ssr_init.png)

2. page 전환 <br>
![img1](/img/ssr_request.png)

3. 패킷이 캡처된 모습 <br>
![img2](/img/ssr_capture.png)

Client Side Rendering에서 네트워크 요청/응답을 확인해봅시다.

1. 최초 접속 <br>
![img3](/img/csr_init.png)

2. 최초 접속 시, js 및 static file을 다운로드 받는다. <br>
![img4](/img/csr_init2.png)


#### 참고한 유용한 블로그 및 게시글들
[[React] 네이버 블로그의 Node.js기반 SSR 전환, CSR과 SSR방식을 자세하게 살펴보기](https://velog.io/@sunaaank/React-deep-dive#csr%EA%B3%BC-ssr%EC%9D%98-%EC%83%81%ED%98%B8%EB%B3%B4%EC%99%84
)

[SPA 에서 SEO 적용하기 :: 마이구미](https://mygumi.tistory.com/385)


[[JS]SEO, SSR, CSR](https://krpeppermint100.medium.com/web-seo-ssr-csr-82f9b7e42e21)

[MPA와 SPA, SSR과 CSR, 그리고 SEO](https://devowen.com/309)

[Virtual DOM 그리고 DOM](https://velog.io/@kja/Virtual-DOM)











