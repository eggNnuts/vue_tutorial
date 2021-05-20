# Component

## component란?
> 레고 처럼 부품을 조합하여 화면을 조합할 수 있는 블록을 의미한다, 자주 사용하는 화면상 기능을 범용성있는 컴포넌트로 개발하면 다음 개발시에도 이미 개발된 컴포넌트를 재사용하면 됨으로
자원(시간 등을)을 줄일 수 있어 생산성이 증대된다.<br>
> 
> <Strong>컴포넌트롤 구분 하는 기준은 아래 3가지 기준으로 나눌 수 있다</strong>
> - 재사용을 할 것인가
> - 재사용은 하지 않지만 로직이 길어져 유지보수를 용이하게 할 것인가
> - 개념적으로 분류할 필요성이 있을때
>
> <Strong>컴포넌트의 실행 과정</strong>
>
> 뷰 라이브러리 파일 로딩 > 인스턴스 객체 생성 > 특정 화면 요소에 인스턴스를 붙임 > 인스턴스 내용이 화면 요소로 변환(등록된 컴포넌트 내용도 변환) > 변환된 화면 요소를 사용자가 최종 확인
 


## 지역컴포넌트와 전역 컴포넌트
> vue에서의 component는 지역 컴포넌트와 전역 컴포넌트로 나눌 수 있다
>
## 지역(Local) 컴포넌트 
> 지역 컴포넌트는 해당 컴포넌트를 등록한 인스턴트 안에서만 사용 가능(유효) 하다
> 
> <Strong>지역 컴포넌트 등록</Strong>
> - 지역 컴포넌트는 인스턴스에 component 속성을 추가하고 등록할 컴포넌트 이름과 내용을 정의하면된다
> ```html
> <template>
>  <div id="app">
>	  <local_component></local_component>
>  </div>
> </template>
>
> <script>
> var localComponent = {   template: '<div>local_component</div>' }
> export default {
>  name: 'test',
>  components:{
>	  local_component
>  }
>  data(){
>    return {
>		message :""
>    }
>  } ,
>  mounted(){
>	 
>  },
> </script>
> ```
><br>

## 전역(Golbal) 컴포넌트 
> 전역 컴포넌트는 지역 컴포넌트와 다르게 vue로 접근 가능한 모든 범위에서 사용할 수 
있다.<br>
>
> <Strong>전역 컴포넌트 등록</Strong> 
> - 전역 컴포넌트는 뷰 라이브러리를 로딩하고 나면 접근 가능한 Vue 변수를 이용하여 등록한다.
> - 전역 컴포넌트는 지역 컴포넌트와 등록하는 형식이 다른대  Vue 생성자에서 .component()를 호출
> ```javascript
> Vue.component('컴포넌트 이름',{
>    //컴포넌트 내용
>});
>```
> ```html
> <template>
>  <div id="app">
>	  <global-component></global-component>
>  </div>
> </template>
>
> <script>
> // 컴포넌트 등록
> Vue.component('global-component', { template : 'global-component</div>' });
>
> 
> export default {
>  name: 'test',
>  data(){
>    return {
>		message :""
>    }
>  } ,
>  mounted(){
>	 
>  },
> </script>
> ```

## component의 data속성이 함수인 이유

> 컴포넌트를 구성하는 vue의 속성중 data속성은 함수로 선언해서 사용한다.<br>
> data속성을 함수로 사용하는 이유는 불변성 때문이다, component를 사용하는 가장 큰 이유는 재사용성 때문인데<br> 만약 data 속성이 함수가 아닌 객체라면 A라는 컴포넌트를 사용하는 B,C의 컴포넌트가 있는대 만약 B에서 A컴포넌트의 data 속성의 내용을 바꾸면  C또한  바뀐 A컴포넌트의 data 속성의 내용을 참조하여 예상치 못한 기능 장애가 발생될 수 있기때문에 data속성은 함수를 사용한다

 ## props 속성
> props 속성은 부모에서 자식으로 데이터를 포워딩할때 사용하는 속성이다
> - 부모컴포넌트에서 자식 컴포넌트로 데이터를 포워딩할때는 html 인라인 속성상 카멜케이스(camelCase)가 아닌 케밥 케이스(kebab-case)로 사용해야함 
>   - ex) myMessage -> my-message 
>
> - props 사용 예시 
> ```html
> // 부모 컴포넌트 
> <template>
>  <div id="app">
>	  <child-component v-bind:message='msg'></child-component>
>  </div>
> </template>
>
> <script>
>  
> export default {
>  name: 'test',
>  data(){
>    return {
>		msg :"hello"
>    }
>  } ,
>  mounted(){
>	 
>  },
> </script>
> ```
> ```java
> // 자식 컴포넌트 
> Vue.component('child-component',{
>			 // hello 출력 
>      template:'<div>{{message}}</div>',
>      props:['message']
> })
> ``` 
> - 위 자식 컴포넌트 예시에서 props 속성 사용법은 기본적인 옵션,props 요소의 데이터 자료형, 기본 값을 사용하지 않는 방식이다<br>
> props 요소의 기본적인 옵션을 사용하는 예시는 아래와 같다 
> ```html
> // 부모 컴포넌트 
> <template>
>  <div id="app">
>	  <child-component v-bind:message='msg'></child-component>
>  </div>
> </template>
>
> <script>
>  
> export default {
>  name: 'test',
>  data(){
>    return {
>		msg :"hello"
>    }
>  } ,
>  mounted(){
>	 
>  },
> </script>
> ```
>
> ```java
> // props의 message property의 자료형이 string, 기본 값(데이터가 넘어오지 않을시)는 Default! 
> Vue.component('child-component',{
>			 // hello 출력 
>    template:'<div>{{message}}</div>',
>    props:{
>		message : {
>		 type:String,
>		 default : 'Default!'
>    }
> })
> // message property의의 자료형이 string이거나 number일때
> Vue.component('child-component',{
>			 // hello 출력 
>    template:'<div>{{message}}</div>',
>    props:{
>		message : {
>		 type:[String,Number],
>		 default : 'Default!'
>    }
> })
>
> // message property가 필요값일때
> Vue.component('child-component',{
>			 // hello 출력 
>    template:'<div>{{message}}</div>',
>    props:{
>		message : {
>		 type:[String,Number],
>		 default : 'Default!',
>          required: true
>    }
> })
>
> // message property의 유효성 검즘
> Vue.component('child-component',{
>			 // hello 출력 
>    template:'<div>{{message}}</div>',
>    props:{
>		message : {
>		 type:[String,Number],
>		 validator:function(value){
>           // return값일 true일때만 유효성 검증 통과됨
>           // return value==='Hello'    
>           return true
>        }
>    }
> })
> ``` 

## 사용자 지정 이벤트 $emit
> props속성이 부모에서 자식으로 데이터를 넘겨주는 속성이라고 한다면 $emit는 자식이 부모에게 데이터를 전달하기 위해서 이벤트를 발생하는 속성이다
> - vue는 기본적으로  자식 컴포넌트 데이터를 정제하고 정제된 데이터가 부모의 데이터에 영향(전달하는)을 끼치는게 허용되지 않은 단방향 데이터 바인딩을 지향한다. 
> - 그러므로 자식 컴포넌트에서 부모 컴포넌트로 직접적으로 데이터를 전달하는 prop속성을 사용할 수 없다
> - $emit 속성은 어쩔수 없이 자식 컴포넌트에서 정제된 데이터를 부모 컴포넌트로 전달할 필요가 있을때 $emit 속성을 사용함으로써 간접적으로 데이터를 전달할수 있다 
> ```html
> // 부모 컴포넌트 
> <template>
>  <div id="app">
>	  <child-component v-bind:message='msg' v-on:update-message:"childUpdateEvent"></child-component>
>  </div>
> </template>
>
> <script>
>  
> export default {
>  name: 'test',
>  data(){
>    return {
>		msg :"hello"
>    }
>  } ,
>  mounted(){
>	 
>  },
>  methods:{
>	  childUpdateEvent : function(message){
>		 console.log(message);
>	  }
>  }
> </script>
> ```
>
> ```java
> // 자식 컴포넌트 
> Vue.component('child-component',{
>			 // hello 출력 
>	 template:'<div v-on:click="updateMessage">{{childMessage}}</div>',
>    props:{
>		message : {
>		 	type:String,
>			default : 'Default!'
>    },
>	 data(){
>		return {childMessage :''}
>	 },
>	 mounted(){
>		// 부모로 부터 전달받은 데이터는 가능하나,  prop 요소는 변경하지 말라는  waring발생해서 data속성에 childMessage요소를 추가했음
>		this.childMessage = message;
>	 },
>	 methods:{
>		updateMessage : function(){
>		  	this.childMessage='good';
>			// updateMessage이벤트 이름으로 ,this.childMessage를 전달함 
>			this.$emit('updateMessage',this.childMessage);
>		}
>	 }
> })
> ``` 