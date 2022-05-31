# Component format 

컴포넌트 포맷은 아주 간단하다.
```html
<script>
	// logic goes here
</script>

<!-- markup (zero or more items) goes here -->

<style>
	/* styles go here */
</style>
```


## \<script\>
컴포넌트 인스턴스가 생성될 때 실행될 JavaScript 블럭이다. 

### export는 component prop를 생성한다. 

초기값 설정이 되지 않으면 undefined 이다. 개발 모드(컴파일러 옵션 참조)에서 기본 초기 값이 제공되지 않고 소비자가 값을 지정하지 않으면 경고가 인쇄된다. 이 경고를 억제하려면 정의되지 않은 경우에도 기본 초기 값이 지정되었는지 확인한다. 

```html
<script>
	export let foo;

	// Values that are passed in as props
	// are immediately available
	console.log({ foo });
</script>
```

```html
<script>
	export let bar = 'optional default initial value';
	export let baz = undefined;
</script>
```

const, class, function은 컴포넌트 외부에서는 readonly이다. 



```html
<script>
	// these are readonly
	export const thisIs = 'readonly';

	export function greet(name) {
		alert(`hello ${name}!`);
	}

	// this is a prop
	export let format = n => n.toFixed(2);
</script>
```

###  Assignments are 'reactive'
컴포넌트 상태를 변경하거나  re-render를 트리거하려면 단지 로컬로 선언된 변수에 할당만 하면 된다. 

```html
<script>
	let count = 0;

	function handleClick () {
		// calling this function will trigger an
		// update if the markup references `count`
		count = count + 1;
	}
</script>
```
기본적으로 Svelte는 반응성을 assignment에 두기 때문인데, .push()와 .splice()와 같은 배열 메서드를 사용하는 것은 자동적으로 updates를 트리거하지 않는다. 



```html
<script>
	let arr = [0, 1];

	function handleClick () {
		// this method call does not trigger an update
		arr.push(2);
		// this assignment will trigger an update
		// if the markup references `arr`
		arr = arr
	}
</script>
```


### $: marks a statement as reactive

모든 최상위 문(즉, 블록이나 함수 내부가 아님)은 $: JS 레이블 구문을 접두사로 사용하여 반응형으로 만들 수 있다. 반응성 문은 의존하는 값이 변경될 때마다 구성 요소가 업데이트되기 직전에 실행된다. 

```html
<script>
	export let title;

	// this will update `document.title` whenever
	// the `title` prop changes
	$: document.title = title;

	$: {
		console.log(`multiple statements can be combined`);
		console.log(`the current title is ${title}`);
	}
</script>
```

$: 블럭 안의 직접적으로 나타나는 값들만이 reactive statement의 의존성이 된다. x가 변할 때 total이 업데이트 될 것이다. y는 아니다. 


```html
<script>
	let x = 0;
	let y = 0;
	
	function yPlusAValue(value) {
		return value + y;
	}
	
	$: total = yPlusAValue(x);  // 여기에 x가 나열이 되었기 때문에 x을 의존
</script>

Total: {total}
<button on:click={() => x++}>
	Increment X
</button>

<button on:click={() => y++}>
	Increment Y
</button>
```

선언되지 않은 변수에 대한 할당으로만 완전히 구성된 문장의 경우 Svelte가 대신하여 let 선언을 주입한다. 

```html
<script>
	export let num;

	// we don't need to declare `squared` and `cubed`
	// — Svelte does it for us
	$: squared = num * num;
	$: cubed = squared * num;
</script>
```

### Prefix stores with $ to access their values
나중에 정리한다. 



## \<script context="module"\>
context="module" 속성을 가진 \<script\> tag는 모듈이 처음 평가될 때 한번만 실행된다. 이 블록에 선언된 값은 일반 스크립트(및 구성 요소 마크업)에서 액세스할 수 있지만 그 반대는 불가능하다. 


이 블록에서 바인딩을 내보낼 수 있으며 컴파일된 모듈의 내보내기가 된다. 

기본 내보내기는 구성 요소 자체이므로 기본값을 내보낼 수 없다. 


> module 스크립트에 선언된 변수들은 반응형이 아니다. 



```html
<script context="module">
	let totalComponents = 0;

	// this allows an importer to do e.g.
	// `import Example, { alertTotal } from './Example.svelte'`
	export function alertTotal() {
		alert(totalComponents);
	}
</script>

<script>
	totalComponents += 1;
	console.log(`total number of times this component has been created: ${totalComponents}`);
</script>
```


## \<style\>
\<style\> 블럭안에 CSS는 컴포넌트에 국한된다. 

```html
<style>
	p {
		/* this will only affect <p> elements in this component */
		color: burlywood;
	}
</style>
```
전역적으로 스타일을을  selector에 적용하려면 :global(...) modifier를 사용한다. 

```html
 <style>
	:global(body) {
		/* this will apply to <body> */
		margin: 0;
	}

	div :global(strong) {
		/* this will apply to all <strong> elements, in any
			 component, that are inside <div> elements belonging
			 to this component */
		color: goldenrod;
	}

	p:global(.red) {
		/* this will apply to all <p> elements belonging to this 
			 component with a class of red, even if class="red" does
			 not initially appear in the markup, and is instead 
			 added at runtime. This is useful when the class 
			 of the element is dynamically applied, for instance 
			 when updating the element's classList property directly. */
	}
</style>
```
전역적으로 접근가능한 @keyframes를 만들려면, -global- 을 가진 keyframe 이름들을 가정할필요가 있다. 

-global- 파트는 컴파일 될 때 제거될 것이다. keyframe은 my-animation-name을 사용하여 코드 어디서건 참조된다. 

```html
<style>
	@keyframes -global-my-animation-name {...}
</style>
```

컴포넌트 마다 단 하나의 \<script\> 태그여야 한다. 


그러나 다른 요소 또는 로직 블록 내부에 포함된 스타일 태그는 가능하다. 이 경우에, 스타일 태그는 현재 DOM에 추가될 것이다. 


```html
<div>
	<style>
		/* this style tag will be inserted as-is */
		div {
			/* this will apply to all `<div>` elements in the DOM */
			color: red;
		}
	</style>
</div>
```


## References
[Component Format](https://svelte.dev/docs#component-format)     