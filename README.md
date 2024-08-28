# 202230313 신희원
### 8월 28일
---
#### Pages Router vs. App Router
* React로 개발하다 처음 Next를 사용하면 Router를 볼 수 있음
* Next.js는 App Router가 Stable하게 지원하기 시작
* 강의는 App Router로 진행

[Page Router]
* pages 디렉토리가 root이고, index.js가 index page가 됨
* about.js는 /about, team.js는 /team 경로로 **클라이언트 중심의 라우팅** 됨

[App Router]
* App 디렉토리가 root이고 page.js가 index page가 됨
* /about/page.js는 /about, /login/page.js는 /page 경로로 **서버 중심의 라우팅** 됨
* 번들 사이즈가 작음