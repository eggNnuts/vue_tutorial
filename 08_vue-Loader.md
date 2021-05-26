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
>
> webpack을 vue에서 사용할려면 `webpack`,`webpack-cli`등 webpack관련 모듈을 설치하고
> 프로젝트 안에 `webpack.config.js`파일을 설치해야한다 <br>
> 
> webpack.config.js의 기본 구조는 아래와 같다
> ```java
>  module.exports={
>    entry:'',
>    output:'',
>    module:'',
>    plugins:''
> }
>```
> ## entry
> *웹팩이 파일을 읽어들이기 시작하는 부분 즉,프로젝트에 존재하는 `모든 모듈(자바스크립트,스타일시트,이미지 등)`중 `Entry 속성에 명시된 파일`을 기준으로 의존성 트리를 만들어 하나의 번들 파일을 만들어 냄*
> - 자바스크립트가 로딩하는 모듈이 많아질수록 모듈간의 `의존성은 증가` 
> - 웹펙이 프로젝트 구조를 이해하기 위해서 어디로 진입해야 webpack이 프로젝트 구조를 이해 
할수있는지를 명시한 진입점이 entry설정임
> - 즉 entry는 vue프로젝트의 최상위 컴포넌트를 지정하면된다(보통 App.vue, java로 치면 클래스의 main함수, 프로젝트의 시작점)
> - 번들이란?
>   - 소프트웨어 및 일부 하드웨어와 함께 작동하는 데 필요한 모든 것을 포함하는 Package
>   - 각각의 모듈들에 대해 의존성 관계를 파악하여 하나 또는 여러개의 그룹으로 볼 수 있음
>
> |  설정  |  결과   |    
> |---|:---|                        
> |  entry : 'main.js' | `main.js 파일 한개로 webpack이 번들파일 생성` | 
> | entry: {  app: 'app2.js', main: 'main2.js' }  | `app2.js와 main2.js를   webpack이 각각app.js,main.js로 번들 파일 생성, 멀티페이지 사이트에서 주로 사용 ` |  
> | entry: {app:['main.js',app2.js]} | `main.js와 app2.js를 webpack이 app.js이라는 하나의 파일로 엮어서 번들파일 생성` | 
><br>
>
> ## Ouput
> *웹펙이 어디에 `번들파일`을 만들어 낼것인지, 어떤이름으로 파일을 어떻게 만들것인지 대한 설정, 위의 `Entry` 설정은 항상 프로젝트 디렉토리 내부임으로 상대 경로로 설정하며 `output`설정은  항상 프로젝트 내부라는 보장이 없음으로 절대 경로로 설정*
> - default value : `/dist/main.js`
>
> |  설정  |  비고   |    
> |---|:---|                        
> |  path| ` output으로 나올 파일이 저장될 경로` | 
> | publicPath  | `파일들이 위치할 서버 상의 경로` |  
> | filename | `파일의 이름(경로를 작성하면안됨)` |
> 
 
>```java
> module.exports={
>   entry : './main.js',
>   output :{
>     filename: 'bundle.js',
>     path: __dirname + '/build'
>   }    
> }
>```  
> - filename 아래의 약어로도 사용할 수 있다
>   - `[name]`: 청크의 이름(entry의 키값)
>   - `[hash]`: 컴파일할 때마다 랜덤 문자열
>   - `[chunkhash]`: 파일이 달라질 때에만 랜덤 값
>
> output의 filename은 위의 `[name]`이라는 약어를 통해 entry에 명시된 키값으로 export(명시된 키값으로 각각 파일이 생김)하거나
> `나머지 2가지 약어` 및 `사용자 정의` 파일이름으로 export할수 있다
> ## Module(Loader)
> *웹팩이 웹 애플리케이션을 해석할 때 자바스크립트 파일이 아닌 웹 자원(HTML, CSS, Images, 폰트 등)들을 변환할 수 있도록 도와주는 속성 Loader를 설정하지 않으면 `자바스크립트를 제외한 웹자원`을 webpack으로 `빌드`할때 `Unexpected toke`n 에러발생*
>
>```java
>module.exports = {
>  entry : './main.js',
>  output: {
>    path: __dirname + '/build'
>    filename: "build.js",
>  },
>  module: {
>    rules: [
>      {
>        test: /\.css$/,
>        use: ["style-loader", "css-loader"],
>      },
>    ],
>  },
>};
>``` 
> - 설정 방법은 `module.exports` 객체에 `module` 속성과 `module`내부에 `rules` 속성을 추가후
>   웹자원을 나열함
>   - `test` : 웹자원의 `suffix` 정규식
>   - `use`  : 사용할 로더 ex) css일 경우 `style-loader`,`css-loader` (물론 npm을 통해 각 loader의 의존성 모듈을 설치해야함) 
> <br>
> 
> <strong>Loader의 특징</strong>
>  - 체이닝 가능. `체인의 각 로더들은 처리된 리소스로 변환함`(체인은 역순으로 실행), 예를들어 첫번째 로더가(변환 된) 결과를 다음 체인으로 전달하고 이러한 과정을 거쳐 최종적으로 webpack은 체인의 마지막 로더에 의해 Javascript로 반환된다
> - `동기/비동기` 모두 가능
> - Node.js 에서 실행되며 가능한 모든 작업을 수행할 수 있다.
> - `options` 객체를 통해 구성할 수 있다. (쿼리 파라미터는 여전히 지원되지만 더 이상 사용되지 않음)
package.json 의 loader 필드를 이용하여 일반 모듈들은 추가적으로 일반 main으로 로더를 내보낼(export) 수 있다.
> - 플러그인은 로더에 더 많은 기능을 제공한다.
> - 로더는 부가적인 임의의 파일들을 생성할 수 있다.
> 
> ## Plugin
> *webpack의 로더가 할 수 없는 일들을 수행할 수 있다.<br>로더는 파일(웹자원)을 해석하고 변환하는 과정을 관여하지만, 플러그인은 해당 결과물의 형태를 바꾸는 역활*<br>
> - 예를들어 웹팩을 실행할때마다 기존에 있던 번들파일을 먼저 지우고 빌드된 번들파일을 생성할수 있다
> - plugin은 module(Loader)부분이 처리되고나서 후 처리되는 역활을 명시해논 설정임 
>```java
>module.exports = {
>  entry : './main.js',
>  output: {
>    path: __dirname + '/build'
>    filename: "build.js",
>  },
>  module: {
>    rules: [
>      {
>        test: /\.css$/,
>        use: ["style-loader", "css-loader"],
>      },
>    ],
>  },
> /*
> 플러그인은 인자와 옵션을 사용할 수 있으므로, plugins 속성에 new 인스턴스를 전달해야 한다.
> */
>  plugins: [new CleanWebpackPlugin("main.js")],
>};
> ```
><br>

 