# props

부모 컴포넌트와 자식 컴포넌트 간에 통신하는 방법에 대해서 살펴본다. 



## 부모 컴포넌트에서 값을 전달 
### 값 전달 
먼저 자식 컴포넌트에서 prop를 export한다. 
```html
<script>
// props 
export let  count;  // prop 노출,  부모 컴포넌트에서 값이 변경되면 반응한다. 

</script>

<h1>PropsLeft</h1>
<h2>{count}</h2>
```


부모 컴포넌트에서 컴포넌트 요소 생성시 { 변수 }를 사용하여 값을 전달한다. 

```html
<script>

import PropsLeft from './PropsLeft.svelte';

// count가 증가하면 PropsLeft 컴포넌트에 반응적으로 값이 전달된다. 
let count  = 100;
const increaseCount = () => { 
  count++
 }
</script>

<h1>Props Main</h1>
<!-- Props 전달-->
<PropsLeft  {count} />

<input type="button" value="Increase count" on:click={increaseCount}>
```

버튼을 클릭하여 count 값을 증가시키면 자식 컴포넌트가 반응한다. 

### 함수 전달 
부모 컴포넌트에서 자식 컴포넌트로 함수 전달도 가능하다. 

부모 컴포넌트에서 전달할 함수를 선언하고 prop로 함수를 전달한다. 
```html
<script>
import PropsContent from './PropsContent.svelte'
let count  = 100;

// 자식 컴포넌트에 전달할 함수 선언 
 const decreaseCount = () => { 
  count--
 }

</script>

<h1>Props Main</h1>

<hr>
<!--- 자식 컴포넌트에 함수 전달 -->
<PropsContent {decreaseCount}/>
```


자식컴포넌트에서 함수를 전달받을 prop를 선언한다. 

```html
<script>

// Props 선언 , 함수가 전달된다. 
export let decreaseCount; 

</script>


<h1>PropsContent</h1>

<!-- 클릭하면 전달 받은 함수를 실행-->
<input type="button" value="Decrease Count" on:click={ (event) => decreaseCount() }>
```

## Prop 이름 사용 
Prop 이름을 사용하여 값을 전달할 수도 있다. 
자식 컴포넌트에서 title이라는 prop를 하나 선언한다. 
```html
<script>
  export let title; 
</script>

<h1>PropsRight</h1>
{title}
```

부모 컴포넌트에서 title 이름을 사용하여 값을 전달한다. 
```html
<!-- prop 이름을 사용하여 값을 전달할 수도 있다.-->
<PropsRight title="Props Right222" />
```




## Spred props 
spred 연산자를 사용하여 전달할 수도 있다. 

```html
<script>
  import Info from "./Info.svelte";
  const person = {
    name: "Jane Smith",
    age: 20,
    gender: "female"
  };
</script>
<main>
  <Info {...person} />
</main>

```

순서대로 export한다. 
```html
<script>
  export let name;
  export let age;
  export let gender;
</script>
<p>{name} {age} {gender}</p>
```


spread 연산자를 사용하여 props를 전달한다. 

```html
<script>
  import Info from "./Info.svelte";
  const person = {
    name: "Jane Smith",
    age: 20,
    gender: "female"
  };
</script>
<main>
  <Info {...person} />
</main>
```
하위 컴포넌트에서는 export 없이 $$props를 사용하여 props에 접근할 수 있다. 
```html
<p>{$$props.name} {$$props.age} {$$props.gender}</p>
```



## References
[Passing Props Between Svelte Components](https://codeburst.io/passing-props-between-svelte-components-f1caab7bb32)     
