# v-model

## v-model 이란?
> v-model 디렉티브는 양방향 데이터 바인딩이라고 하며, 데이터에 있는 값이 view(input과 textarea)에 나타나고 view의 값이 바뀌면 데이터의 값도 바뀌는 디렉티브이다.<br>
v-model속성은 v-bind와 v-on의 기능 조합으로 동작하며 사용자가 일일이 v-bind와 v-on속성을 다 지정하지 않아도 좀더 편하게 개발할 수 있게 고안된 문법
> 
> ```html
> <!-- 
>    v-model 디렉티브를 사용하지 않으면 아래 코드와 같이 v-bind:value와 v-on:input을
>    같이 사용하여야한다 
>  --> 
> <template>
>  <div id="app">
>	  <input type="text" v-bind:value="message" v-on:input="messageInputEvent">
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>		message :""
>    }
>  } ,
>  mounted(){
>	 
>  },
>  methods:{
>	 messageInputEvent : function(e){
>		 this.message =e.target.value;
>    }
>  }
> </script>
> ```
><br>
> 
> ```html
> <!-- 
>    v-model 사용시 아래와 같이 사용할수 있다.
>  --> 
> <template>
>  <div id="app">
>	  <input type="text" v-model="message">
>  </div>
> </template>
>
> <script>
>export default {
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
><br>


## v-model 수식어
> - v-model 디렉티브는 3가지 수식어를 제공한다
>
> | 수식어 | 설명 | 비고 |
> |---|:---:|---:|
> | `.lazy` | view에서 변동된 값이 바로 data에 반영되지 않고 엔터를 누르거나 포커스를 벗어나는 경우에 data에 반영 | `v-on:change와  동일한 기능` |
> | `.number` |  view에서 입력한 값을 숫자로 바꾸어주는 수식어 |  `숫자인 경우만 숫자타입으로 반환`  |
> | `.trim` | view에서 입력한 값의 앞과 뒤의 공백을 제거 | `trim function과 동일한 기능`|