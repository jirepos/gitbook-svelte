# Dispatch 

:dispatch를 이용해 하위 컴포넌트에서 상위 컴포넌트의 함수를 호출해 사용 할 수 있다. 

하위 컴포넌트에서 createEventDispatcher를 impor하고 createEventDispatcher 함수를 생성한다.  dispatch('이벤트명', 값)을 사용하여 이벤트를 발생시킨다. 

```html
<script>

import { createEventDispatcher } from 'svelte'

// dispatch 이벤트를 발생시키는 함수 생성 
const dispatch = createEventDispatcher()

// dispatch를 실행할 함수 정의 
const addAction = (a, b) => {
  dispatch('add', { a, b })  // dispatch 호출 
}

</script>

<h1>Dispatch Sub</h1>

<button on:click={() => addAction(10, 20)} >Add 10 + 20</button>
```

상위 컴포넌트에서 이벤트를 처리할 핸들러 함수를 정의하고 on:add={함수명}을 사용하여 이벤트를 바인딩한다. 전달된 값은 event.detail로 받을 수 있다. 파라미터가 하나이면 event.detail로 값을 가져오고, 여러 개이면 파라미터 명으로 가져온다. 

```html
<script>
  
import DispatchSub from './DispatchSub.svelte'

let a =0;
let b =0; 
const handleEvent = (event) => {
   console.log(event)
   a = event.detail.a;   // 이벤트 발생시 전달된 값은 detail에 있음.  
   b = event.detail.b; 
}

</script>

<h1>Dispatch Main</h1>

<DispatchSub  on:add={handleEvent}/>

<h2>a : { a} ,    b:  {b}</h2>
```

변수명을 정하여 값을 넘기는 방법을 사용하는 것이 좋다.  자식 컴포넌트에서 key:value 형식으로 넘긴다. 
```jsx
	function sayHello() {
		dispatch('message', {
			text: 'Hello!'
		});
	}
```
부모 컴포넌트에서 key로 값을 구한다.   
```jsx
	function handleMessage(event) {
		alert(event.detail.text);
	}
```  


중첩된 컴포넌트들에서 최하위 자식 컴포넌트의 이벤트를 최상위 부모 컴포넌트에서도 수신할 수 있다. 


