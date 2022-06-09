# store - 상태관리

규모가 있는 프로젝트에서 상태를 관리하기 위해 store를 사용하는 방법을 살펴본다. 



 Store란 각자 다른 컴포넌트들이 같은 데이터를 접근하기 위해 사용하는 것으로, 애플리케이션의 규모가 커지거나 컴포넌트 간의 상호 작용이 필요할 경우 매우 유용하게 사용할 수 있다. 



 Store의 값이 수정될 때 참조하고 있는 컴포넌트에게 알려주는 구독 기능을 제공한다. 구독을 사용하는 컴포넌트는 개발자가 변경된 값을 화면에 출력하기 위해 DOM을 조작하지 않아도 자동으로 반영된다. 또한 구독 메서드를 사용하면 Svelte 내부 로직을 통해 값이 변경되기 때문에 성능 및 안전성이 높은 장점이 있다.



## store.js 

src 폴더 아래에 store 폴더를 만들고 그 아래 store.js를 만든다. 

```
📁project_root
  📁src
    📁store
      📄store.js
```  
컴포넌트를 만들고 컴포넌트에서 svelte/store로부터 writable 객체를 임포트한다. 

```jsx
import { writable } from 'svelte/store';
```

구독을 통해서 값을 가져올 수 있다. 
```jsx
let count = 0; 
// 구독을 통해서 변경된 값을 받아온다.
logCount.subscribe(value => {
  count = value;
});
```
update()를 사용하역 값을 변경할 수 있다. 
```jsx
// update를 통해서  count 값을 변경한다.
const increaseCount = () => {
  logCount.update( value => value + 1)
}
```
```html
<input type="button" value="Increase Count" on:click={increaseCount}>
```

set를 사용하여 값을 초기화 할 수 있다. 


```jsx
// set를 통해서 값을 초기화 한다 
const initCount = () => {
  logCount.set( 0); 
}
```

### writable 
읽기, 쓰기가 가능한 스토어로 Set과 Update 메서드를 지원한다. Set은 값을 초기화하는 메서드이고, Update는 값을 수정하는 메서드이다. Update의 경우 파라미터로 콜백 함수가 전달되는데, 콜백 함수의 파라미터로 스토어의 현재 값이 전달된다. 콜백 함수를 리턴하면 스토어의 값이 수정된다.

```jsx
storeVar.update((현재 스토어 값) => {
      return 업데이트할 값
})
```    



## 메모리 관리 - unsubscribe
다른 페이지로 이동하거나 애플리케이션을 종료할 때 스토어에 할당된 객체가 소멸되지 않고 메모리에 계속 저장되어 메모리 누수를 발생시킨다. 이것을 해결하기 위해 애플리케이션이 파괴될 때 스토어의 구독을 끊어 메모리를 소거시켜야 한다.

**구독 메서드를 실행하면 리턴 값으로 구독을 해지하는 함수를 리턴**한다. 리턴된 함수를 실행하면 자동으로 구독이 해지된다. 일반적으로 스토어의 구독을 해지하는 경우는 애플리케이션이 종료되는 시점인 경우가 많으므로 Svelte 컴포넌트 생명 주기의 onDestroy 내부에서 구독을 해지한다. 


 ```jsx
	const unsubscribe = count.subscribe(value => {
		countValue = value;
	});

	onDestroy(unsubscribe);
```

