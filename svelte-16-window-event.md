# window에 이벤트 등록 

window 객체에  \<svelte:window\>를 사용하여 이벤트를 등록할 수 있다. 

```html
<script>
  let key;
  let keyCode;

  function handleKeydown(event) {
    key = event.key;
    keyCode = event.keyCode;
  }
</script>

<svelte:window on:keydown={handleKeydown}/>

<div style="text-align: center">
  {#if key}
    <kbd>{key === ' ' ? 'Space' : key}</kbd>
    <p>{keyCode}</p>
  {:else}
    <p>Focus this window and press any key</p>
  {/if}
</div>
```


## window 속성에 바인딩 
bind 를 사용하여 윈도우 속성에 바인딩할 수 있다. 


```html
<svelte:window bind:scrollY={y}/>
```


## \<svelte:body\>
window 객체에 이벤트를 등록할 수 있듯이, document.body에 이벤트를 등록할 수 있다. 


## <\svelte:head\>
\<head\>에 요소를 추가할 수 있다.

```html
<svelte:head>
  <link rel="stylesheet" href="tutorial/light-theme.css">
</svelte:head>
```









