# NextJS Introduction

## 1.0 Library vs Framework

### 라이브러리 vs 프레임워크

- 라이브러리와 프레임워크의 주요 차이점은 Inversion of Control (IoC, 제어의 역전)
  |Library|Framework|
  |------|-----|
  |라이브러리는 개발자가 어떤 프로그램을 가져다 쓰는 것 (Ex.React : 렌더링 시 ReactDOM.render()를 불러와서 사용)|프레임워크는 정해진 틀 안에서 커스터마이징 (Ex. NextJS : 정해진 규칙에 따라 코드를 작성하면 렌더링 됨)|
  |라이브러리에서 메소드 호출 시 사용자가 제어|프레임워크에서는 제어가 역전되어 프레임워크가 사용자 코드를 호출|
  |사용자가 파일 이름이나 구조 등을 정하고 모든 결정을 내림|파일 이름이나 구조 등을 정해진 규칙에 따라 만들고 따름|

## 1.1 Pages

### NextJS의 페이지 구조

- pages 폴더 하위에 있는 파일명에 따라 route가 결정된다. 이 때 파일명이 그대로 url로 들어가게 되고 컴포넌트의 이름은 상관없다.

  - ex) pages/about.js 생성 -> localhost:3000/about (O)
  - 예외사항) index.js는 앱이 시작되는 파일로 localhost:3000/ 으로 접근한다. (/index 가 아님)

- 단, pages 하위에 작성되는 js파일은 export default로 선언되어야 라우팅 처리됨

- pages에 정의되어 있지 않은 js 외의 route는 NextJS의 자체 404 페이지를 노출함.

- 일반적인 리액트와 달리 jsx로 작성될 필요가 없으며 React를 import할 필요가 없음. (단, useState, useEffect 등은 사용할 경우 import)

## 1.2 Static Pre Rendering

### NextJS의 렌더링

- NextJS는 초기 상태를 렌더링(pre-rendering)해서 클라이언트에 전달 (HTML)
- 페이지가 처음에 로딩될 때 스크립트도 클라이언트에 전달되고, 클라이언트는 페이지가 로딩되었을 때 react.js 가 전송되고 클라이언트에서 스크립트를 실행
  - 일반적인 React 앱은 클라이언트에서 모든 것을 그려냄

### CSR(Client Side Rendering) 테스트해보기

- 개발자도구 -> Network -> Slow 3G로 변경 및 Disable cache 클릭 후 새로고침하면 테스트 가능
  - 테스트 사이트 : https://nomadcoders.github.io/react-masterclass/

## 1.3 Routing

### NextJS의 라우팅

- navigate 시 a 태그 대신 Link 사용
  - a 태그를 사용하는 경우 페이지 진입 시 앱이 reload됨 (느려지고 사용성 저하)
- Next 13버전 이전에는 Link의 스타일 지정 시 Link 태그 하위 a 태그를 사용해서 스타일 등을 지정해야 했으나 13버전 부터는 Link 태그에 직접 지정 가능
- NextJS app 생성 시 기본으로 useRouter 훅을 제공

  - router는 location 에 대한 정보를 가지고 있음

  `const router = useRouter();`

## 1.4 CSS Modules

### CSS Module 패턴

- 클래스 명을 텍스트가 아닌 js object의 property 형태로 사용
  - ex) `<nav className={styles.nav}> ... </nav>`
- 파일명 **.module.css** 로 생성 후 클래스 정의해서 사용
- 기존의 className, class를 사용한 방식과 달리 실제로 렌더링되면 클래스명이 파일명\_클래스명\_\_랜덤텍스트 형태로 나옴
  - ex) NavBar_nav\_\_OBiyO
  - 고유의 클래스명을 가지게 되어 스타일이 겹치지 않게 됨
- CSS Module 패턴으로 여러개의 className 사용하는 방법

```
1. className={`${styles.link} ${router.pathname === "/" ? styles.active : ""}`}
2. [styles.link, router.pathname === "/" ? styles.active : ""].join(" ")
```

## 1.5 Styles JSX

### Styles JSX

- NextJS에서 스타일을 추가하는 또다른 방식 (NextJS 고유의 방식)
- style 태그에 jsx props를 추가 후 사용 가능

  ` <style jsx>{`
  nav {
  background-color: tomato;
  }
  `}</style> `

- 클래스명은 랜덤한 이름으로 생성됨 (jsx-OOOOOOOO...)
- 해당 스타일을 선언한 컴포넌트에 종속적임
  - 부모 컴포넌트에 선언된 스타일일지라도 자식 컴포넌트에 영향 미치지 않음
  - 전역으로 선언하는 방식은 다음 강의

## 1.6 Custom App

### Global로 style 설정

- 방법1. global props를 추가

  ` <style jsx global><{``}</style> `

  - 이 방법을 사용하더라도 해당 스타일이 선언됨 컴포넌트의 자식 컴포넌트들만 영향을 미침
  - 다른 페이지들에도전역으로 이를 적용하려면 App Component를 사용
    => pages 하위에 \_app.js 작성하여 커스터마이징

- 방법2. App Component 커스터마이징
  - 기본적으로 NextJS의 페이지 호출 시 아래와 같이 내장된 App Component를 호출
    ```
    export default function App({ Component, pageProps }) {
       return <Component {...pageProps} />;
    }
    ```
    - ex) about.js 호출 시 About 컴포넌트가 App 의 인자로 들어감
    - Typescript의 경우 : https://nextjs.org/docs/basic-features/typescript#custom-app 참조
  - App을 커스터마이징하려면 pages 아래에 \_app.js 를 작성해서 사용
  - 이 때 styles/globals.css 를 import 시 전역 스타일을 적용 가능하다.
    - globals.css는 App 컴포넌트 내에만 import 가능함을 유의
    - module.css 파일 형태를 제외한 나머지 css도 App 컴포넌트에서만 import 가능 (https://nextjs.org/docs/messages/css-global)

## 2.0 Patterns

### Layout Pattern

- Layout 영역을 별도로 구성하고 App을 Layout 의 children 으로 넘겨주는 패턴
- Navigation, Footer 등의 재사용

### Head

- NextJS에 내장된 next/head 라이브러리에서 import
- 페이지 head에 엘리먼트를 추가하기 위한 내장 컴포넌트를 노출

## 2.1 Fetching Data

### img 태그

- NextJS에서는 img 태그 대신 'next/image'의 Image를 사용하는 것을 권장
  - 더 나은 성능과 자동 이미지 최적화 제공

### 테스트 데이터

- TMDB의 영화 데이터 불러오기
  - tmdb 사이트에서 api key 획득해서 사용하기
  - https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}

## 2.2 Redirect and Rewrite

- NextJS에서 Redirect, Rewrite를 사용해 request로 들어오는 source url을 다른 destination url로 변경 가능하다.
- next.config.js 에 redirects(), rewrites()를 작성하여 사용 가능 (각각 async 함수)
  - next.config.js 는 수정 시 재시작 필요
- API Key를 사용자로부터 숨기는 스킬로 사용 가능하다.

### Redirect

- redirects()는 source, destination 및 permanent 속성이 있는 객체의 배열을 return하는 비동기 함수
- redirects()의 경우 URL이 명시적으로 변경된다.
- source : 들어오는 request 경로 패턴 (request 경로)
  - ex) `source: '/org/:path*`
    - /org/ 아래의 path를 모두 destination으로 변경
- destination: 라우팅하려는 경로 (redirect할 경로)
  - ex) `destination: '/new/:path*'`
    - `*` 옵션을 사용 시 위에 있는 org 하위 경로를 new 하위 경로로 치환 (패턴 매칭)
- permanent: true인 경우 클라이언트와 search 엔진에 redirect를 영구적으로 cache하도록 지시하는 308 status code를 사용하고, false인 경우 일시적이고 cache되지 않은 307 status code를 사용

### Rewrite

- rewrites() 사용 시 들어오는 request 경로를 다른 destination 경로에 매핑 가능하다.
  - ex) 외부 api인 https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY} 을 /api/movies 로 치환 가능
- rewrites()은 URL 프록시 역할을 하고 destination 경로를 masking하여 사용자가 사이트에서 위치를 변경하지 않은 것처럼 보이게 한다. (새 페이지로 reroute되고 url에 변경사항을 표시하는 redirect와는 다름)

### env 사용

- API Key와 같은 민감 정보를 공개하지 않기 위해 .env 파일에 정의하고, 해당 파일을 .gitignore에서 예외처리해서 관리하면 좋다.
