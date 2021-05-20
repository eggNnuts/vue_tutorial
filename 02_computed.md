# Computed


> - 계산된 속성이라고 하며 서버 사이드로 부터 받아온 데이터를 클라이언트 단에서 가공하며 추가적으로 반응성을 부여하는 속성 
> - 해당 속성을 활용해서 데이터를 가공해서 아래와 같이 활용하면 원본데이터를 해치지 않기 때문에 원본데이터의 무결성을 지킬 수 있다.
> ```html
> // todos = > [{title:"아침 먹기"},{title:"점심 먹기"},{title:"저녁 먹기"}]
> /*  computedTodos = > [
>        {title:"아침 먹기", id : 1 , done : false},
>        {title:"점심 먹기", id : 2 , done : false },
>        {title:"저녁 먹기", id : 3 , done : false}
>    ] */
> <template>
>  <div v-for="(todo,index) in computedTodos"  v-bind:key="todo.id">
>      // 아래 체크 박스를 클릭하면 클릭한 todo의 done 상태는 true가 됨
>      <input type="check" v-model="todo.done">
>	   <span>{{todo.title}}</span>
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>       todos :[
>        {title:"아침 먹기"},
>        {title:"점심 먹기"},
>        {title:"저녁 먹기"}
>       ]
>    }
>  },
>  computed: {
>    computedTodos: function () {
>	   //todos내 각각 object에 id, done property를 추가 
>      return this.todos.map((todo,index)=>{
>	         return {
>				...todo,
>				id : index+1,
>				done: false
>            }
>      })
>    }
>  }
> }
> </script>
> ```
>
> ```   
 
# Computed의 getter 와 setter
> 일반적인 상황에서 computed는 getter의 역활을 수행한다, 그러나 computed를 getter와 setter 의 역활로 나누게 할수 있다, getter 와 setter은 아래와 같이 사용할수 있다.
> <br>
>```html
> <template>
>  <div>
>    <p>  {{reversedMessage }}</p>  => 요세하녕안
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>      message: '안녕하세요'
>    }
>  },
>  computed: {
>    reversedMessage:{
>         get : function(){
>			return this.message.split('').reverse().join('');
>		},
>		set : function(newMessage){
>			this.message = newMessage;
>		}
>    } 
>  }
> }
> </script>
> ```
><br>


# 특징

## 1. 유지보수에 용이하다 
 
>  인라인 태그내에서 서버 사이드로부터 받아온 데이터를 템플릿 문법으로 핸들링 가능하지만<br>
>   코드가 길어지면 길어질수록 유지보수가 힘들어지는 치명적인 단점이 존재한다
>   하지만 computed를 사용할 경우에 computed내 선언한 익명함수만 유지보수 하면됨으로써
>   유지보수가 용이해진다
> ```html
> // 템플릿 문법으로 안녕하세요를 뒤집기
> <template>
>  <div>
>    <p> {{this.message.split('').reverse().join('') }}</p>  => 요세하녕안
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>      message: '안녕하세요'
>    }
>   },
>  }
> }
> </script>
> ```
> ```html
> // computed를 사용 안녕하세요를 뒤집기
> <template>
>  <div>
>    <p>  {{reversedMessage }}</p>  => 요세하녕안
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>      message: '안녕하세요'
>    }
>  },
>  computed: {
>    reversedMessage: function () {
>      return this.message.split('').reverse().join('')
>    }
>  }
> }
> </script>
> ```
><br>
<br>

## 2. 캐싱
> computed는 종속 대상의 변경, 즉 computed를 활용하여 가공된 원본데이터의 상태가 변화 할때만
> 동작한다는 장점이 있다, computed의 특징중 하나인 유지보수의 용의성만 볼때에는 methods 속성 내에 함수를 정의 하여 사용 할 수 있지만 methods내 정의된 함수를 사용 할 경우에는 렌더링 될때마다 함수가 호출 되기때문에 연산이 복잡한 기능 일수록 오래 걸리게된다 
> ```html
> // computed를 사용 안녕하세요를 뒤집기
> // => 5번 호출하더라도 처음 실행될때 캐싱됬기때문에 캐싱된 데이터 사용
> <template>
>  <div>
>    <p>  {{reversedMessage }}</p>   
>    <p>  {{reversedMessage }}</p>
>    <p>  {{reversedMessage }}</p>
>    <p>  {{reversedMessage }}</p>
>    <p>  {{reversedMessage }}</p>
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>      message: '안녕하세요'
>    }
>  },
>  computed: {
>    reversedMessage: function () {
>      return this.message.split('').reverse().join('')
>    }
>  }
> }
> </script>
> ```
><br>
> 
> ``` html
> // methods를 사용 안녕하세요를 뒤집기
> // => reversedMessage함수가 반복적으로 5번 실행됨
> <template>
>  <div>
>    <p>  {{reversedMessage() }}</p>   
>    <p>  {{reversedMessage() }}</p>
>    <p>  {{reversedMessage() }}</p>
>    <p>  {{reversedMessage() }}</p>
>    <p>  {{reversedMessage() }}</p>
>  </div>
> </template>
>
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>      message: '안녕하세요'
>    }
>  },
>  methods: {
>    reversedMessage: function () {
>      return this.message.split('').reverse().join('')
>    }
>  }
> }
> </script>
> ```
><br>


## computed관련 vue 내부 로직
> 아래는 beforeCreate Hook 후에 실행되는 property injection중 computed를 초기화 하는 과정이다
>  
> ```java
>function initComputed (vm, computed) {
>    // $flow-disable-line
>    var watchers = vm._computedWatchers = Object.create(null);
>    // computed properties are just getters during SSR
>    var isSSR = isServerRendering();
>
>    for (var key in computed) {
>      var userDef = computed[key];
>	   // computed property마다 get과 set을 명시해서 사용할 수 있는대 만약 property내부에 get을 명시하지않으면 property자체가 getter가 된다 
>      var getter = typeof userDef === 'function' ? userDef : userDef.get;
>      if (getter == null) {
>        warn(
>          ("Getter is missing for computed property \"" + key + "\"."),
>          vm
>        );
>      }
>
>  	   // 서버 사이드 렌더링이 아닐 경우 computed 속성마다 내부적으로 watcher를 생성한다 
>      if (!isSSR) {
>        // create internal watcher for the computed property.
>        watchers[key] = new Watcher(
>          vm,
>          getter || noop,
>          noop,
>          computedWatcherOptions
>        );
>      }
>
>      //  vue instance에 computed property들이 vue instance또 다른속성 (data,props등 )과 property들과 동일하지 않으면  property에 computed를 정의한다 
>      if (!(key in vm)) {
>        defineComputed(vm, key, userDef);
>      } else {
>         // 해당 로직은 computed property가 data와 props와 동일하면 warning 발생시키는 로직임
>        if (key in vm.$data) {
>          warn(("The computed property \"" + key + "\" is already defined in data."), vm);
>        } else if (vm.$options.props && key in vm.$options.props) {
>          warn(("The computed property \"" + key + "\" is already defined as a prop."), vm);
>        }
>      }
>    }
>  }
>
> ```
>```java
>  function defineComputed (
>    target,
>    key,
>    userDef
>  ) {
>    /* 
>		아래 변수는 computed의 특징중 하나인 computed 종속 대상(property)의 변경전까지 연산한 결과를 캐싱 유무이다(로직을 보니 서버사이드 렌더링일 경우 computed캐싱을 하지 않는다  )
>		또한 아래 noop가 있는데 noop는 아무런 동작을 수행하지 않을려고 초기화 시킴. 
>		* 거의 첫 라인에  function noop (a, b, c) {} 요런 function으로 정의되 있음 
>	 */
>    var shouldCache = !isServerRendering();
>    
>    if (typeof userDef === 'function') {
>      sharedPropertyDefinition.get = shouldCache  ? createComputedGetter(key) : createGetterInvoker(userDef);
>        
>      sharedPropertyDefinition.set = noop;
>    } else {
>      sharedPropertyDefinition.get = userDef.get  ? shouldCache && userDef.cache !== false  ? createComputedGetter(key) : createGetterInvoker(userDef.get)  : noop;
>       
>      sharedPropertyDefinition.set = userDef.set || noop;
>    }
>    if (sharedPropertyDefinition.set === noop) {
>      sharedPropertyDefinition.set = function () {
>        warn(
>          ("Computed property \"" + key + "\" was assigned to but it has no setter."),
>          this
>        );
>      };
>    }
>    /* 
>		Object.defineProperty()는 객체에 직접 새로운 속성을 정의하거나 이미 존재하는 속성을 수정하기 위해서 사용하는 정적메서드이다 
>		
>		Object.defineProperty()의  첫번째 인자는 프로퍼티를 정의할 대상 객체,두번째 인자 새로 정의하거나 수정하려는 프로퍼티의 이름, 세번째 인자는 새로 정의하거나 수정하려는 속성을 쓰는 객체인데 
>		각각 순서대로 vue instance , computed property, getter 및 setter를 정의한 sharedPropertyDefinition을 초기화 했다
>		여기서  sharedPropertyDefinition는 아래와 같이 초기값이 정의됬는대 
>		var sharedPropertyDefinition = {
>    		enumerable: true,  객체 속성열거 즉 값을 for...in문 등의 방법으로 열거 상태가 true
>    		configurable: true,   속성의 값을 변경 여부 상태가 true
>    		get: noop,
>    		set: noop
>  		};
>   */
>    Object.defineProperty(target, key, sharedPropertyDefinition);
>  }
>```
>
>```java
>   function createComputedGetter (key) {
>    return function computedGetter () {
>      var watcher = this._computedWatchers && this._computedWatchers[key];
>      if (watcher) {
>        if (watcher.dirty) {
>          watcher.evaluate();
>        }
>        if (Dep.target) {
>          watcher.depend();
>        }
>        return watcher.value
>      }
>    }
>  }
>
>  function createGetterInvoker(fn) {
>    return function computedGetter () {
>      return fn.call(this, this)
>    }
>  }
>```