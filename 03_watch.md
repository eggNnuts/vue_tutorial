# Watch
> - Watch 속성 내 기술된 대상의 상태를 감시하고 있다가, 대상의 상태가 변경되는 순간 특정한 로직을 수행하는 트리거,감시자 역활을 하는 속성
> - Watch 속성은 Computed 속성도 감시가 가능하다
> <br>
> ```html
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
>		get :function(){
>			return this.message.split('').reverse().join('');
>		},
>		set : function(newMessage){
>			this.message=newMessage;
>		}
>	 }
>  },
>  watch :{
>	  message : function(newMessage){
>		  console.log(`new message : ${newMessage}`)
>     },
>	  reversedMessage : function(newMessage){
>		  console.log(`new reversedMessage : ${newMessage}`)
>	  }
>  }
>}
> </script>
> ```
><br>
<br>

## Computed 와 Watch
> Computed와 Watch는 대상의 변경 상태를 감시 한다는 유사한 기능을 공유하고 있다.<br>
> Computed와 Watch는 상황에 따라서 올바르게 선택해야 한다.
> <br>
> 
> <strong>Computed</strong> 
> - 인스턴스의 data에 할당된 값들 사이의 종속관계를 자동으로 세팅하고자 할때, 위의 예시처럼 message에 값에따라 reversedMessage의 상태가 변경될때 사용하는게 좋다
> - 이미 정의된 계산식에 따라 결과값을 반환할 때
> <br>
> 
> <strong>Watch</strong> 
> - 특정 프로퍼티의 변경시점에 특정 액션(api call, push route)을 취하고자 할때 
> - 어떤 특정 조건에서 함수를 실행 시키기 위한 트리거로서 사용
><br>
>```html
> <script>
>export default {
>  name: 'test',
>  data(){
>    return {
>      count: -1
>    }
>  },
>  watch :{
> 	  count: function (newVal) {
>      	  if(newVal === 0) {
>      	  	alert('값이 0이 되었습니다.')
>          	this.count = 3
>      	  }
>    }
>  }
>}
> </script>
> ```
><br>
