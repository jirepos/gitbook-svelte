# 반응성 


svelte에서 변수 선언 또는 조건절 등에 $: 를 붙여 주면 반응성 코드가 된다. 



```html
<script>
let count = 0

// count가 변하면 이 코드가 동작한다 
$: doubled = count * 2

// count 값이 변하면 이 코드가 동작 
$: if(count >= 10) {
	alert('카운트가 10을 넘었습니다. ')
	count = 9
}


// 다음과 같이 하면 특정 영역을 반응성 상태로 만들 수 있다. 
$: {
	console.log( count )
	console.log( doubled )
}

function handleClick() {
	count += 1
}

```