# 컴포넌트 재귀 처리 

아래 코드를 보면  self 지시자가 보인다. 재귀적으로 자기 자신을 생성할 수 있다. 


특정 컴포넌트 내부에서 컴포넌트 자신을 재귀로 추가해야 하는 경우에 사용한다. 재귀로 컴포넌트 자신을 추가해야 하는 경우 일반적으로 \<script\> 태그 내에서 컴포넌트를 import 하여 사용하지만, Svelte에서는 \<svelte:self\> 요소를 사용하여 쉽게 구현할 수 있다.



```html
<svelte:self tree={child} />
```

```html
<script context="module">
	// retain module scoped expansion state for each tree node
	const _expansionState = {
		/* treeNodeId: expanded <boolean> */
	}
</script>
<script>
//	import { slide } from 'svelte/transition'
	export let tree
	const {label, children} = tree

	let expanded = _expansionState[label] || false
	const toggleExpansion = () => {
		expanded = _expansionState[label] = !expanded
	}
	$: arrowDown = expanded
</script>

<ul><!-- transition:slide -->
	<li>
		{#if children}
			<span on:click={toggleExpansion}>
				<span class="arrow" class:arrowDown>&#x25b6</span>
				{label}
			</span>
			{#if expanded}
				{#each children as child}
					<svelte:self tree={child} />
				{/each}
			{/if}
		{:else}
			<span>
				<span class="no-arrow"/>
				{label}
			</span>
		{/if}
	</li>
</ul>

<style>
	ul {
		margin: 0;
		list-style: none;
		padding-left: 1.2rem; 
		user-select: none;
	}
	.no-arrow { padding-left: 1.0rem; }
	.arrow {
		cursor: pointer;
		display: inline-block;
		/* transition: transform 200ms; */
	}
	.arrowDown { transform: rotate(90deg); }
</style>

```