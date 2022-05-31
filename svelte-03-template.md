# TEMPLATE SYNTAX

## Tags 
\<div\>와 같이 소문자는 HTML element를 나타낸다. \<Widget\> 또는 \<Namespace.Widget\>과 같은 대문자화된 태그는 component를 나타낸다. 


```html
<script>
	import Widget from './Widget.svelte';
</script>

<div>
	<Widget/>
</div>
```


## Attributes and props

기본적으로 속성은 HTML 속성과 똑같이 작동한다. 
```
<div class="foo">
	<button disabled>can't touch this</button>
</div>
```
HTML에서 값을 인용하지 않을 수 있다. 
```html
<input type=checkbox>
```
속성 값은 JavaScript 표현식을 포함할 수 있다.
```html
<a href="page/{p}">page {p}</a>
```

또는 JavaScript 표현식일 수 있다. 
```html
<button disabled={!clickable}>...</button>
```
불린 속성은 값이 truthy이면 요소에서 포함되고 값이 falsy이면 제외된다. 

다른 속성들은 값이 nullish(null or undefined)가 아니면 포함된다. 
```html
<input required={false} placeholder="This input field is not required">
<div title={null}>This div has no title attribute</div>
```

표현식은 정규 HTML에서 syntax highlighting을 일으키는 문자를 포함할 수 있다.  그래서 값을 quoting하는 것이 허용된다. 그 quotes는 값이 파싱되는 방법에 영향을 주지 않는다. 

```html
<button disabled="{number !== 42}">...</button>
```

애트리뷰트 이름과 값이 매칭될 때(name={name}), {name}으로 대체될 수 있다. 

```html
<!-- These are equivalent -->
<button disabled={disabled}>...</button>
<button {disabled}>...</button>
```
관례로, componet에 전달된 값들은 properties 또는 props로 참조된다. attribute가 아니다. 그것은 DOM의 특징이다. 

요소들과 같이, name={name}은 {name}으로 간단히 대체될 수 있다. 

```html
<Widget foo={bar} answer={42} text="hello"/>
```

Spread attributes는 많은 attributes 또는 properties를 요소 또는 컴포넌트에 전달되도록 허용한다. 

요소 또는 컴포넌트는 다수의 spread attributes를 가질 수 있다. 

```html
<Widget {...things} />
```

\$\$props references는 컴포넌트에 전달되는 모든 props이다. export를 가지고 선언되지 않은 것들을 포함한다.  보통 추천되지 않는다. 







## References
[TEMPLATE SYNTAX](https://svelte.dev/docs#template-syntax-tags)     





