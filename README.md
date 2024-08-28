# 202230313 신희원
## 9월 2일 | 2주차
## 8월 28일 | 1주차
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
* Data Fetching: 13까지는 getServerProps, getStaticProps 메소드를 이용해 구현했으나, 14부터 **SSG**(정적 사이트 생성), **SSR**(서버측 랜더링) 및 **ISR**(증분적 정적 재생성)에서 하나의 API만을 사용해 구현할 수 있게 됨
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


#### 1.1 준비
* Node.js와 npm을 설치하거나, codesandbox.io 혹은 repl.it 등의 사이트 이용
* 이후 프로젝트별로 필요한 의존성 패키지 npm으로 설치   
`가볍게 이용할 경우 사이트도 좋지만 그렇지 않으면 local에서 환경 설정하는 것이 좋음`

#### 1.2 `Next.js`란?
Angular, React, Vue 와 같은 프레임워크가 등장하면서 웹 개발 분야 급속도로 변화
1. 리액트는 메타(페이스북)의 **조던 발케**가 제작, 2013년 오픈소스 발표
2. 리액트는 클라이언트 사이드(CSR)에서만 작동하는 문제점이 있음   
    * 검색 엔진 최적화(SEO)의 효과를 거의 볼 수 없음
    * 애플리케이션 실행 초기 성능 부담   
    현재 리액트에서도 SSR을 지원하지만 구현은 `Next.js`보다 복잡
3. 이 문제 해결을 위해 서버에서 미리 렌더링 해두는 방법 연구
4. 이 연구 결과가 `Next.js` (Vercel에서 제공)
5. 이 밖에 리액트가 제공하지 않는 여러 기능을 제공함

[ `Next.js`가 제공하는 새로운 기능들 ]
* 코드 분할(Splitting): 페이지를 로딩 할 때 번들을 여러 조각으로 나눠 필요한 부분만 전송
* 파일 기반 라우팅(React 에서는 React-Router-Dom 사용)
* 경로 기반 프리페칭(Prefetching): 사용자가 다음에 이동할 수 있는 페이지를 미리 가져오는 기술
* 서버 사이드 렌더링(SSR: 페이지 요청이 올 때마다 사전 렌더링)   
(페이지 요청 시 Fetching 요소 적용해 렌더링)
* 정적 사이트 생성(SSG): 빌드하는 동안 페이지 사전 생성   
(Fetching 요소 적용 위해 재빌드)
* 증분 정적 콘텐츠 생성(ISR): 배포 후에도 재배포 없이 계속 업데이트(일정 시간마다 SSG 재렌더링)
* 타입스크립트 기본 지원
* 자동 폴리필(Polyfill) 적용: 이전 브라우저에서 최신 기능을 제공하는 데 필요한 코드 제공
* 이미지 최적화: Next/image 컴포넌트로 제공하는 이미지 최적화 기술   
( lazy loading-지연, 이미지 사이즈 최적화-webp 변환, placeholder-영역 확보)
* 웹 애플리케이션의 국제화 지원: 다국어 지원(local에 맞는 URL로 라우팅)
* `Next.js`는 현재 넷플릭스, 트위치, 틱톡, 나이키, 우버, 엘라스틱 등 여러 사이트에서 사용중

#### 1.3 `Next.js` 와 비슷한 프레임워크
[ Gatsby ]
* 정적 웹 사이트를 만들 수 있는 프레임워크
* 정적 사이트 생성, 클라이언트 사이드 렌더링만 지원
* 동적 웹사이트 제작 불가능

[ Razzle ] <https://github.com/jaredpalmer/razzle> / <https://razzlejs.org/>
* 서버 사이드 렌더링이 가능한 자바스크립트 애플리케이션 개발 가능
* CRA와 유사하게 프로젝트를 구성할 수 있는 장점(create-razzle-app)
* React, Preact, Reacon-React, Angular 및 Vue 와 함께 사용 가능

[ Nuxt.js ]
* Vue를 사용한 웹 애플리케이션 개발에서 리액트의 `Next.js`에 해당하는 프레임워크
* Nuxt.js 나 `Next.js` 모두 같은 목표를 갖는 프레임워크지만 Nust.js 는 더 많은 설정 필요
* 만약 Vue 개발자가 서버 사이드 렌더링이 필요하다면 Nuxt.js 추천

[ Angular Universal ]
* 정적 사이트 생성과 서버 사이드 렌더링 지원
* Nuxt나 Next와는 달리 **구글**에서 만듬
* Angular 로 개발하는 경우 대부분 Angular Univeral 사용   

2016년 첫 발표

#### 1.4 왜 `Next.js` 일까
* React에서 제공하지 않는 여러 기능 지원
* 설정이나 개발 옵션 등 다양한 부분에서도 유용한 기능 제공
* 활동적인 커뮤니티가 있어 개발 단계별로 많은 지원을 받을 수 있음
> <https://react.dev/learn/start-a-new-react-project>

#### 1.5 리액트에서 `Next.js` 로
* `Next.js`의 기본 철학은 React와 유사
* **"설정보다는 관습"** 리액트 철학 계승

"CoC: Convention over Configuration"은 개발자가 해야 할 결정의 수를 줄여주면서도, 유연성은 잃지 않도록 하는 소프트웨어 설계 패러다임

* 예로 설정 파일을 만들지 않고도 어떤 페이지에서 서버 사이드 렌더링을 적용하고, 어떤 페이지에 정적 페이지 생성을 적용할지 지정 가능
* `Next.js` 는 fetch, window, document 와 같은 웹 브라우저에서 제공하는 전역 체계나 canvas 같은 **HTML 요소 접근 불가**
* 전역 객체나 HTML 요소를 반드시 사용해야 하는 컴포넌트를 다루는 방법 별도 제공   

그밖에 . .

* 클라이언트 사이드 앱, 프로그레시브 웹앱, 오프라인 웹앱도 쉽게 만들 수 있음
* 많은 내장 컴포넌트와 최적화 기능 사용이 가능하다는 장점   
Progressive Web Apps(PWA)은 웹앱과 네이티브 앱의 장점 모두 제공   
Java, Kotiln / Object-C, Swift

#### 1.6 `Next.js` 시작하기

* 개발환경 Node.js 와 npm 설치 (Node.js 설치시 npm 함께 설치됨)
* 버전 확인 (v22.7.0 버전 사용, 다른 버전도 ok, 의존성 문제시 변경)
```
$ node -v
v22.7.0
$ npm -v
10.8.2
```