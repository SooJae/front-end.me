---
title: '웹 렌더링'
date: 2020-11-05 20:40:00
category: Web
thumbnail: './images/page_optimization1.jpg'
draft: true
---

## 웹 렌더링의 종류(CSR, SSR, SSG)

## 렌더링과 렌더링 과정
렌더링과 렌더링 과정은 박스여우님이 포스팅 하신 [브라우저 렌더링 과정 - Reflow Repaint, 그리고 성능 최적화](https://boxfoxs.tistory.com/408) 에 잘 정리되어있습니다. 

## SEO란?
SEO란 Search Engine Optimization의 약자이며, 사용자의 의도에 맞게 웹 페이지를 제작하고, 이 페이지가 페이지에 잘 노출 되도록 최적화 하는 것을 뜻합니다.
검색 결과를 제공하는 주요 프로세스에는  크롤링(Crawling), 인덱싱(Indexing), 랭킹(Ranking)이 있습니다.

[검색엔진최적화는 무엇인가?](https://www.ascentkorea.com/what-is-seo/) 에는 SEO를 다음과 같이 정의했습니다.

>웹사이트를 웹 검색 크롤러가 잘 읽어갈 수 있도록 만들고, 각 페이지가 트래픽 유입을 일으킬 수 있는 주제로 색인될 수 있도록 하며, 검색 결과에서 높은 랭킹을 얻을 수 있도록 페이지별로 태깅과 콘텐츠를 최적화하는 것

웹 페이지 레벨에서 아무리 열심히 검색엔진최적화를 추진하더라도 웹사이트 레벨에서 구조가 복작하여 크롤러가 웹페이지를 제대로 읽어가지 못하거나 중복된 콘텐츠가 많아서 같은 도메인 안의 웹 페이지들이 검색 결과에서 서로 경쟁한다면 SEO의 성과를 기대할 수 없는데, 바로 이런 부분들이 크롤링과 인덱싱  깊게 관련이 되어있기 때문이다. 

 
대부분의 웹 크롤러, 봇들은 JS를 실행시키지 못하고 HTML에서만 컨텐츠를 수집하기 때문에
CSR 방식으로 개발된 페이지를 빈 페이지로 인식하게 됩니다.
SSR 방식은 View를 서버에서 전부 렌더링하기 때문에 HTML에 모든 컨텐츠가 저장되어 있어 SEO를 사용하는데 문제가 없습니다.

FCP TTI등 용어에 대해 모르시는 분들께서는 이 게시물을 보고 와주세요

### Hydration 이란?
SSR / SSG에서 사용하는 아키텍쳐로 빠른 FCP를 위해 어느 정도 만들어진 HTML을 내려주고, 그 이후에 HTML의 나머지 부분과, 이벤트 리스너(화면 스크롤, 클릭 등)를 추가하는 방식입니다.
즉 FCP와 TTI까지의 시간에 동작하는 것이라고 보시면 됩니다. 


https://blog.julien-maury.dev/en/what-the-hell-are-hydration-and-rehydration

### CSR

Client Side Rendering은 뼈대만 있는 HTML파일(\<html>\<style>\<meta>태그만 있는)에, 서버에서 받은 자바스크립트 파일을 사용하여 브라우저에서 페이지를 직접 렌더링 합니다.
\<div id="root">\</div>

![client-side-rendering](client-rendering-tti.png)

장점: 자바스크립트 번들 파일을 다운받고 실행한 후에는 페이지 전환 등이 빠릅니다. 

단점: 자바스크립트 번들 파일이 커질수록 FCP 까지의 시간이 길어집니다. (사용자 이탈률에 중요한 시간) 
빈 페이지에 자바스크립트를 호출해서 받는 방식이므로, 대부분의 크롤러가 HTML이 비어있다고 판단합니다. 검색 엔진 최적화(SEO)가 잘 안된다.(하지만 Google Bot은 잘 된다. 크롤링하는 요소중에 자바스크립트를 지원하기 때문에)
FCP 시간 동안 빈 페이지가 보이므로 대부분의 사이트에서 이 부분을 로딩 아이콘으로 대체합니다. 

자바스크립트 번들 파일이 클수록 느려지기 때문에 코드 분할(Code Spliting)을 사용하는 것이 좋습니다. 

### Server Side Rendering
요청한 페이지의 전체 HTML을 서버에서 생성해서 전송합니다.

![server-side-rendering](server-rendering-tti.png)

장점: 
FCP 까지의 시간이 빠르다. 
페이지 로직 및,페이지 렌더링을 서버에서 실행하므로 많은 자바스크립트를 클라이언트에 보내지 않아도 되므로 TTI도 빠릅니다.
빈 페이지에 자바스크립트를 호출하는 방법이 아닌, 모든 정보가 포함된 HTML을 보여주므로 SEO가 잘된다.

단점 : 
// 페이지를 요청할 때 마다 새고로침(깜빡거림)이 발생하여 사용자 경험이 안 좋아집니다.
CSR은 서버에서 상대적으로 작은 응답을 받는 것에 비해, SSR은 페이지의 HTML을 작성하는 시간 때문에 TTFB가 상대적으로 느립니다.
간단한 요청도 서버를 통해야 합니다. 즉 요청이 많아지면 서버에 무리가 갑니다.
    
## Static Site Generator
SSG의 순서는 다음과 같습니다. 
1. SSG는 사용하면 빌드 시에 HTML파일을 생성합니다.
2. 각각의 HTML에 페이지 구성을 위한 최소한의 자바스크립트만 연결시킵니다. 
3. 이후 페이지가 브라우저에 로드되면 추가적인 자바스크립트를 실행시킵니다. 

Nextjs의 경우 getStaticProps함수에 SQL문을 작성하여 빌드시에 서버에 접근하여 SQL문을 실행시킬 수 있습니다.
(예 : https://stackoverflow.com/questions/63802019/calling-the-database-from-getstaticprops-in-nextjs)

장점 : 
빌드 시에 HTML이 생성되고, 이후 요청할때마다 생성한 HTML을 **재 사용** 하여 웹사이트가 빨라집니다.
정적 웹 페이지이므로 CDN에 배포가 가능합니다.
보안에도 좋습니다.
API 호출이 줄어듭니다.
 
단점: 
콘텐츠가 자주 변경되고, 그 콘텐츠가 최신 상태여야 한다면 사용하기 힘듭니다.
웹사이트가 큰 경우 빌드시간이 매우 길어질 수 있습니다.

다음은 CSR, SSR, SSG 비교 그림입니다.
![csr_ssr_ssg](./csr_ssr_ssg.png)


## 오늘은 여기까지
이번 포스팅에서는 CSR, SSR, SSG에 대해 알아보았습니다. 
CSR과 SSR를 비교해 봤을 때, 본인의 프로젝트에 따라 유연하게 사용할 수 있을 것 같습니다.
SSG의 경우에는 개념만 본다면 빌드시에만 변경된 데이터가 적용되니 게시글이 가끔 올라오는 블로그나, 회사 소개 페이지에만 사용할 수 있을것 같다고 생각할 수도 있습니다.
하지만 SSG도 많은 발전을 해서 Nextjs의 경우 [Incremental Static Regeneration](https://nextjs.org/docs/basic-features/data-fetching#incremental-static-regeneration) 을 사용하면 **빌드 없이** 변경된 데이터를 적용할 수 있습니다.
변경 적용이 2초정도 걸리는 것으로 보아 [Static Reactions Demo](https://reactions-demo.now.sh/) 기술이 발전 함에 따라 SSG가 SSR을 대체할 수 있을 것 같습니다.

다음 포스팅은 Nextjs에 대해 알아보겠습니다.

회사들의 SSR 도입기를 아래의 링크에서 확인 하실 수 있습니다.
네이버: https://d2.naver.com/helloworld/7804182
줌 모바일: https://zuminternet.github.io/ZUM-Mobile-NodeJS/

## Client Side Rendering


Rehydration : https://stackoverflow.com/a/33790716

Rehydration: 클라이언트가 서버에서 렌더링 한 HTML의 DOM 트리와 데이터를 재사용하도록 자바 스크립트 뷰를 "부팅"합니다.
Prerendering: 빌드 타임에 클라이언트 측 애플리케이션을 실행하여 초기 상태를 정적 HTML로 캡처합니다.

SSR vs CSR
SSR: Server-Side Rendering (서버 측 렌더링) - 클라이언트 측 또는 유니버설 앱을 서버의 HTML로 렌더링합니다.
CSR: Client-Side Rendering (클라이언트 측 렌더링) - 브라우저에서 애플리케이션을 렌더링합니다. 일반적으로 DOM을 사용합니다.


## Static Rendering 

![static-rendering-tti](static-rendering-tti.png)

아래의 그림은 서버 - 클라이언트 방식의 스펙트럼을 보여줍니다.



![infographic](./infographic.png) 

https://addyosmani.com/blog/rehydration/
[검색엔진최적화는 무엇인가?](https://www.ascentkorea.com/what-is-seo/)
[The Benefits of Server Side Rendering Over Client Side Rendering](https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8) 
[Web Rendering](https://developers.google.com/web/updates/2019/02/rendering-on-the-web)
https://lo-victoria.com/client-side-rendering-vs-server-side-rendering-vs-static-site-generation
