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
