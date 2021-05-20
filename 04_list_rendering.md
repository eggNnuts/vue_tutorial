# 리스트 렌더링
> vue.js에서는 객체,배열데이터에 따라서 화면에 반복적으로 표현(렌더링)하는 v-for 디렉티브를 제공한다.<br>
pure javascript나 jquery를 활용해서 객체 및 배열 데이터를 화면에 반복적으로 표현 할려면 append,appendChild등을 활용하여 표현 하기때문에<br>데이터를 가공하는 등 복잡한 연산 혹은 이벤트 바인딩이 들어갈 경우에는 생각보다 골치가 아프며, 또한 코드를 볼때도 직관적이지 않은 문제가 발생한다.<br> 그러나 vue.js에서 v-for 디렉티브를 사용하면 코드가 직관적이며 이벤트 바인딩 및 computed,v-if 디렉티브등 vue에서 제공하는 속성을 사용하여 가공된 데이터를 출력하기 때문에<br> pure javascript 및 jquery를 활용하는것보다 표현하는 방법이 수월하다 
>
> - v-for를 사용할때 각 노드에 대한 고유한 값(아이디)를 지정해야 vue에서 각 노드를 고유한 값을 통해 구분할 수 있다. 고유한 값은 v-bind:key를 사용한다
> - v-for 디렉티브와 v-if 디렉티브 를 한태그에 같이 사용 할경우에는 vue는  <strong>You should not mix 'v-for' with 'v-if'</strong> waring를 발생 시킨다.
> - v-for디렉티브와 v-if 디렉티브를 한 태그에서 사용할 경우 v-for가 더 우선순위가 높다

## 배열(Array) 렌더링
>  배열을 v-for 디렉티브를 사용 할 경우 아래와 같이 사용하면 된다
> - v-for 디렉티브를 사용할때 아래와 같이 2번째 인자에 index를 사용할 수 있다
>   인덱스의 시작값은 0이다, 
> - 아래와 같이 todos의 값이 변경될 일이 없으면 computed 혹은 methods등을 활용하여 데이터를 가공하여 id를 넣어주지 않고 index으로도 key를 지정하는데 문제는 없다
> - 2번째 코드와 같이 배열에 할당하는 방식으로 데이터를 추가하면 반응성이 주입되지 않는 방식이기 때문에  렌더링이 다 끝난 후 정의되지 않은 데이터(야식먹기)가 추가가 되도 화면상에 업데이트가 되지 않는다, 그러나 todos에는 야식먹기 데이터가 추가되었다
> 이때 vue내장 메스드인 $forceUpdate 메소드를 사용하여 강제적으로 재렌더링 시키거나 push 메서드를 사용하여 배열에 추가시키는 방법을 사용한다
> -  $forceUpdate는 하위 컴포넌트나 인스턴스에 영향을 끼치지 않고 $forceUpdate 메소드가 실행된 인스턴스만 다시 렌더링 한다
> -   렌더링이라는 작업 자체가 비용이 많이 드는 작업이므로 $forceUpdate를 과도하게 사용한다면 애플리케이션의 성능이 하락할 수 있는 주원인이 될 수 있다
> ```html
> <template>
>  <div id="app">
>   <ul class="todos">
>	  <li v-for="(todo,index) in todos" v-bind:key="index"> 
>		  {{title}}
>	 </li>
>	</ul>
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>       todos:[
>			{title: "아침 먹기"},
>			{title: "점심 먹기"},
>			{title: "저녁 먹기"}
>		]
>    }
>  },
>}
> </script>
> ```
><br>
>
> 
>```html
> <template>
>  <div id="app">
>	<button v-on:click="push()">Push</button>
>   <ul class="todos">
>	  <li v-for="(todo,index) in todos" v-bind:key="index"> 
>		  {{title}}
>	 </li>
>	</ul>
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>       todos:[
>			{title: "아침 먹기"},
>			{title: "점심 먹기"},
>			{title: "저녁 먹기"}
>		]
>    }
>  },
>  methods:{
>	 push : function(){
>       // 이렇게 사용하면 반응성이 주입되지 않아 화면에 추가되지 않음
>		this.todos[3]={title:"야식 먹기"}
>       // 굳이 할당하는 방식으로 데이터를 추가할때는 
>       this.$forceupdate()
>       // 혹은 push 사용
>       this.todos.push({title:"야식 먹기"})   
>
>	 }
>  }
>}
> </script>
>```
><br>
<br>

## 객체(Object) 렌더링
> - 객체를 렌더링할때는 키를 index를 사용할려면 배열 렌더링할때와 index 매개변수의 위치가 조금 다르다 
> - 아래 v-for디렉티비의 괄호안 매개변수는 값,키 인덱스 순이다
> - 객체 렌더링때도 마찬가지로 렌더링이 끝난 후에 heropy에 새로운 키와 값을 추가시키면
>   반응성이 주입되지 데이터를 사용하기 때문에 화면에 적용되지 않는다
>   (반응성은 beforeCreate Hook이 실행된후 주입된다 )
>    - 반응성을 주입하는 방식은 2가지 방식이 있다
>      - 첫번째) 데이터의 수가 고정된 객체라면  빈 문자열로 미리 정의를 사용한다 <br>
>          ex) heropy: { name : "HEROPY",age :31,homepage:"",email:""}
>      - 두번째) 데이터의 수가 가변적인 객체라면, 객체를 확장시키는 object.prototype.assign를 사용한다,아래 코드와 같이 heropy객체를 통째로 변경하는 행위는 반응성이 작용하는 행위다 <br>
>          ex) this.heropy=Object.assign({},this.heropy,{homepage:"heropy.blog"})
> ```html
> <template>
>  <div id="app">
>   <ul class="heropy">
>	   <li v-for="(value,key,index) in heropy" v-bind:key="index">{{value}}</li>
>	</ul>
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>       heropy: {
>		   name : "HEROPY",
>		   age :31
>		}
>    }
>  } 
> </script>
> ```
><br>

## Vue.set과 this.$set
> Vue.set 혹은 this.$set메소드는 반응형으로 선언된 값을 업데이트하는 메소드이다,
즉 반응성이 작용되는 메소드(vue에서 래핑된메소드) 배열 렌더링에서 push,shfit. 객체 렌더링에서는 Object.assign 메소드를 사용하지 못할때 수동으로 Vue에 값이 갱신되어 반응성이 작용해달라는 메소드 이다
> - 주의사항으로는 Vue.set의 경우 Vue가 전역으로 등록되있어 Vue.set으로 사용 가능했지만
>   실무로 넘어오면 Vue가 전역으로 등록되 있는일이 거의 없다, 그러므로 this.$set사용을 권장한다
> - 배열 렌더링일때는 this.$set({타겟},{인덱스},{값})
> - 객체 렌더링일때는 this.$set({타겟},{키}},{값})
>```html
> // 배열 렌더링일때 set 사용 예시
> <template>
>  <div id="app">
>	<button v-on:click="push()">Push</button>
>   <ul class="todos">
>	  <li v-for="(todo,index) in todos" v-bind:key="index"> 
>		  {{title}}
>	 </li>
>	</ul>
>  </div>
> </template>
>
> <script>
> export default {
>  name: 'test',
>  data(){
>    return {
>       todos:[
>			{title: "아침 먹기"},
>			{title: "점심 먹기"},
>			{title: "저녁 먹기"}
>		]
>    }
>  },
> methods:{
>	 push : function(){
>		 // Vue.set 사용 
>		 Vue.set(this.todos,3.{title:"야식먹기 "})
>		 // this 사용
>		 this.$set(this.todos,3.{title:"야식먹기 "})
>	 }
>  }
>}
> </script>
>```
><br>
>
> ```html
>  // 객체 렌더링일떄
> <template>
>  <div id="app">
>   <ul class="heropy">
>	   <li v-for="(value,key,index) in heropy" v-bind:key="index">{{value}}</li>
>	</ul>
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>       heropy: {
>		   name : "HEROPY",
>		   age :31
>		}
>    }
>  } ,
>  mounted(){
>	 this.$set(this.heropy,"homepage","hello?")
>  }
> </script>
> ```
><br>