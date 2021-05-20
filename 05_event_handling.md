# Event Handling

## 이벤트 수식어 
> Vue.js의 이벤트 발생시 이벤트 핸들러 내부에서 event.preventDefault,event.stopPropagation를 사용하여 
  상위 엘리멘트에 이벤트 발생을 중지하거나, 고유 이벤트를 중지하는 행위를 할수있지만, vue에서는 개발자가 
  비지니스 로직에 신경쓸수 있도록 이벤트 수식어를 제공한다
>  - 이벤트 핸들러 내부 : v-on:click,v-on:change등 이벤트가 발생했을때 실행되는 메소드 내부
>  - event.preventDefault : 고유 이벤트를 중지- submit이벤트 발생시 form안에있는 input등을 전송을 중지하거나<br> a태그 클릭시 페이지 이동시키는걸 중지하거나
>  - event.stopPropagation : 이벤트 버블링 방지 - 자식 이벤트 발생시 , 이벤트가 상위 엘리먼트에 전달되지 않게 막아줌, 자식 엘리멘트에 영역과 부모 엘리멘트의 영역이 공유되기때문에 이벤트 버블링이 발생하게 된다
> 
> | 수식어 | 설명 | 비고 |
> |---|:---:|---:|
> | `.prevent` | 고유 이벤트의 자동 실행을 중단 시킴 | `preventDefault() 과 동일한 기능` |
> | `.stop` | 이벤트가 전파되는 것을 중단 시킴 (Bubbling 중단) | `stopPropagation() 과 동일한 기능`  |
> | `.capture` | 포착 단계에서만 이벤트를 발생시킴 | `내부 엘리먼트를 대상으로 하는 이벤트가 해당 엘리먼트에서 처리되기 전에 여기서 처리함` |
> | `.self` | 발생 단계에서만 이벤트를 발생시킴 | `event.target이 엘리먼트 자체인 경우에만 트리거를 처리, 자식 엘리먼트에서는 실행안됨,이벤트 버블링과 동일한문제임 자식엘리먼트와 부모 엘리먼트의 영역이 공유되기때문에`  |
> | `.once` | 이벤트를 한번만 실행 시킴 | `2.1.4에 새로 추가됨`  |
> | `.passive` | 기본 이벤트를 취소할 수 없게 함 | `있을지 모를 .preventDefault()를 실행 안되게 함, 2.3.0+ 이후 추가됨`  |
<br>

## 키코드 수식어
> 기존 pure javascript 및 jquery에서 input태그 등에서 키보드 이벤트 발생시(enter,page-up)등 이벤트 핸들링을 할때 키코드를 일일이 인지하여 이벤트 핸들링을 해야하는 불편함이 있었다, 그러나 vue.js에서는 이벤트 v-on:change등 이벤트에 키보드를 수식하는 예약어를 사용시 간편하게 키보드 이벤트 핸들링을 할 수 있다 
>
>- 키코드 수식어 종류
>
> |   |     |    |
> |---|:---:|---:|                        
> | `.enter` |` .tab` | `.delete` |
> | `.esc` | `.space` | `.up`  |
> | `.down` | `.left` | `.right` |
> | `.ctrl(vue 2.1.0+ added)` | `.alt(vue 2.1.0+ added)` | `.shift(vue 2.1.0+)`  |
> | `.meta(vue 2.1.0+ added)` | |   |
> - 키코드 수식어는 아래와 같이 사용할수 있다
> ```html
>  <input type="text" v-model="name" v-on:keyup.enter="search">
> ```
>
> - 또한 또한 전역 config.keyCodes 객체를 통해 사용자 지정 키 수식어 별칭을 지정할 수 있다
>  
>  ```javascript
>  // `v-on:keyup.f1`을 사용시.
>  Vue.config.keyCodes.f1 = 112
>  ```
> - vue 2.5.0 이후 버전에서는
> 정확도 향상을 위해 .exact 가 추가됨
>  ``` html
>  <!-- Alt 또는 Shift와 함께 눌린 경우에도 실행됨. -->
> <button @click.ctrl="onClick">A</button>
> 
> <!-- Ctrl 키만 눌려있을 때만 실행됨 -->
> <button @click.ctrl.exact="onCtrlClick">A</button>
>
> <!-- 아래 코드는 시스템 키가 눌리지 않은 상태인 경우에만  실행됨 -->
> <button @click.exact="onClick">A</button>
>  ```

## 마우스 버튼 수식어
> vue.js에서는 마우스 버튼도 수식어가 제공된다
> |   |     |    |
> |---|:---:|---:|                        
> | `.left` |` .right` | `.middle` |
> |          |         |            |
>
>  마우스 왼쪽 클릭 이벤트가 발생했을 때 'leftMouse' 라는 메서드를 실행하려면, 아래 예제와 같이 사용할 수 있다.
>```html
><div id="example" @mouse.left="leftMouse">
>	...
></div>
> ```




 