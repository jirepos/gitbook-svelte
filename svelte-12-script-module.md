# \<script context="module"\>


\<script context="module"\>를 사용하면 동일한 컴포넌트로 만들어진 인스턴스 간의 코드를 공유할 수 있다. 

context=module에서, 모든 컴포넌트가 인스턴스되었을 때 실행되는 것이 아닌 처음 script를 읽혔을 때 실행 된다.  보통의 script태그와 마찬가지로 변수를 선언 가능하다. 



```html
<script context="module">

	const elements = new Set();

  // stopAll()을 export하면 부모에서 setopAll()을 호출 할 수 있음 
	export function stopAll() {
		elements.forEach(element => {
			element.pause(); // Set에 등록된 요소들의 pause()를 호출 
		});
	}

  // 여기서는 아래 <script>에 있는 코드에는 접근이 불가능하다. 
</script>

<script>
  // 여기서는 script=module에 있는 코드에 접근 가능 
	import { onMount } from 'svelte';

	export let src;
	export let title;
	export let composer;
	export let performer;

	let audio;
	let paused = true;

	onMount(() => {
    // module에서 <script>에 접근할 수 있게 elements에 audio를 등록 
		elements.add(audio);
		return () => elements.delete(audio);
	});

	function stopOthers() {
    // 다른 audio를 중지 
		elements.forEach(element => {
			if (element !== audio) element.pause();
		});
	}
</script>

<article class:playing={!paused}>
	<h2>{title}</h2>
	<p><strong>{composer}</strong> / performed by {performer}</p>
	<audio
		bind:this={audio}
		bind:paused
		on:play={stopOthers}
		controls
		{src}
	></audio>
</article>
```

부모 컴폰넌트인 App.svelte에서 stopAll을 호출할 수 있다. 

```html
<script>
	import AudioPlayer, { stopAll } from './AudioPlayer.svelte';
</script>

<button on:click={stopAll}>
	stop all audio
</button>

<AudioPlayer/>
<AudioPlayer/>
<AudioPlayer/>
<AudioPlayer/>
<AudioPlayer/>
```
