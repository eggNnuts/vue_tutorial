# vue-Loader

## vue-Loader란?
> vue 컴포넌트를 자바스크립트 모듈로 변환할 수 있는 webpack에서 사용하는 로더<br>
> vue-Loader가 제공하는 유용한 기능 목록
> - ES2015(es6) 지원
> - 각 Vue 컴포넌트 마다 서로 다른 webpack 로더를 사용할 수 있다. 예를들면 `<style>`에 Sass, `<template>`에 Jade로 각각 설정 가능
> - `<style>`과 `<template>`에서 참조된 정적 Asset 파일을 모듈로 취급하고 webpack 로더로 처리
> - 각 컴포넌트마다 지정된 CSS를 시뮬레이트 가능
> - 개발 중에 컴포넌트 핫 리로딩
>     - <strong>핫 리로딩이란?</strong>  *.vue파일을 편집하고 저장하면 편집한 컴포넌트의 모든 인스턴스가 페이지를 리로딩하지않고 변경,즉 페이지를 새로고침하지않고 컴포넌트 수정후 저장하면 바로 적용된다

## webpack이란?
> 모듈의 묶음이다, .js, .jade, .css와 같이 각 파일을 모듈로 간주하고 , css면 css 끼리, js면 js끼리 파일간 종속성을 파악한 후 정적 Asset으로 변화하여 배포하는 모듈 번들러이다.<br>
>
> ![oop](./picture/web_pack.png)
> webpack을 사용하는 몇가지 예시들
> - ES2015 또는 CoffeeScript, TypeScript 모듈을 ES5 CommonJS 모듈로 변환.
> - 선택 사항으로 컴파일 전에 linter를 이용하여 소스 코드를 연결 가능.
> - Jade 템플릿을 일반 HTML로 변경하고 JavaScript 문자열로 반환
> - Sass 파일을 일반 CSS로 변환한 다음 CSS를 `<style>` 태그로 삽입하는 JavaScript 스니펫으로 변환.
>   - <strong>스니펫이란?</strong> 자주 사용하는 코드 패던을 저장해놨다가 단어를 통해 불러오게하는 코드 조각
> - HTML 또는 CSS에서 참조된 이미지 파일을 처리하고 경로 구성에 따라 이동한 후 md5 해시를 사용하여 이름을 지정.