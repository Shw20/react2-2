# 202230313 신희원
## 8월 28일
#### **Pages Router vs. App Router**
* React로 개발하다 처음 Next를 사용하면 Router를 볼 수 있음
* `Next.js`는 App Router가 Stable하게 지원하기 시작
* 강의는 App Router로 진행

**[Page Router]**
* pages 디렉토리가 root이고, index.js가 index page가 됨
* about.js는 /about, team.js는 /team 경로로 **클라이언트 중심의 라우팅** 됨

**[App Router]**
* App 디렉토리가 root이고 page.js가 index page가 됨
* /about/page.js는 /about, /login/page.js는 /page 경로로 **서버 중심의 라우팅** 됨
* 번들 사이즈가 작음

#### **Next.js 13 vs 14**
* Pages Router -> App Router
* Data Fetching: 13까지는 getServerProps, getStaticProps 메소드를 이용해 구현했으나, 14부터 SSG(정적 사이트 생성), SSR(서버측 랜더링) 및  ISR(증분적 정적 재생성)에서 하나의 API만을 사용해 구현할 수 있게 됨
```
// This request should be cached until manually invalidate.
// Similar to `getStaticProps`
// `force-cache` is the default and can be omitted.
fatch(URL, {cache:`force-cache`});

// This request should be refetched on every request.
// Similar to `getServerSideProps`
fetch(URL, {cache:`no-store`});

// This request should be cached with
```
* Tubopack: Rust 기반으로 개발된 새로운 번들러 사용으로 webpack보다 700배 빠르다고 발표   
Turbopack 은 3,000개의 모듈이 있는 애플리케이션에서 1.8초만에 부팅되고, Webpack은 16.5초, Vite는 11.4초가 걸림   

* 이미지 최적화: 13까지는 **도구**를 사용했으나, 14부터는 **자체적**으로 지원하기 시작
* 보안 강화: XXS 공격에 대한 보호 기능이 강화되고, 보안 관련 헤더 설정을 더욱 쉽게 만듬

### Chapter 1. `Next.js` 알아보기
* `Next.js` 는 리액트를 위해 만든 오픈소스 자바스크립트 웹 프레임워크
* 리액트에 없는 다양한 기능 제공
    - 서버 사이드 랜더링(SSR: Server Side Rendering)
    - 정적 사이트 생성(SSG: Static Site Generation)
    - 증분 정적 재생성(ISR: Incremental Static Regeneration)   
```
SSG(정적 페이지 생성)는 미리 만들어 놓은 페이지를 서비스 하기 때문에 
속도는 빠르나 한번 생성시 수정 불가능
이러한 단점 보완을 위해 나온 것이 ISR(증분 정적 재생성)
이미 생성된 페이지를 일정 시간이 지난 후에 다시 생성(최신 데이터로 업데이트)
```

[ Chapter 1 주요 내용]
* Next.js 소개 / 프레임워크 비교 / 리액트와 차이점
* 기본 구조 타입스크립트 사용법
* 바벨과 웹팩 설정 커스터마이징(Next.js14는 Webpack->Tubopack 바뀜)
