# Svelte에서 CSS 사용 

> 그렇기 때문에 Svelte에서는 구성 요소의 \<style\>태그에 CSS를 추가할 수 있습니다. CSS를 마크업과 함께 배치 하면 개발자가 새로운 CSS를 도입하지 않고 CSS를 작성할 때 직면하는 가장 큰 문제를 해결할 수 있으며 동시에 상당히 좋은 개발 경험을 제공할 수 있습니다.       
그러나 Svelte의 스타일 처리에는 몇 가지 제한 사항이 있습니다. 구성 요소 간에 스타일을 공유하거나 앱 수준 최적화를 적용하는 것은 너무 어렵습니다. 이것들은 우리가 향후 버전에서 다룰 계획인 영역이지만, 그 동안 이러한 것들이 필요하다면 프레임워크에 구애받지 않는 CSS-in-JS 라이브러리를 사용할 수 있습니다.



## global하게 css 파일을 적용

Svelte에서 global하게 css 파일을 적용하는 방법은 public/index.html에 css 파일을 경로를 설정하면 된다. 

```html
<head>
	<link rel='stylesheet' href='/global.css'>
</head>
```

## JavaScript에서 css 파일을 import 
Svelte tookit으로 프로젝트를 생성했다면 할 필요는 없다. 이미 적용이 되어 있기 때문이다.  매뉴얼로 적용하려면 아래의 References의 "How to import CSS files to Svelte" 내용을 참고한다. 

index.html에 bundle.css 파일을 포함하고 있다. 

```html
<link rel='stylesheet' href='/build/bundle.css'>
```

rollup.config.js 파일에 css 플러그인이 적용된 것을 확인할 수 있다. 
```jsx
		// we'll extract any component CSS out into
		// a separate file - better for performance
		css({ output: 'bundle.css' }),

```

### css 생성 
project root 폴더에 static 폴더를 만들고 그 아래 my.css 파일을 만든다. 

```
📁project_root
  📁static
    📄my.css
```    
다음과 같이 간단히 css를 정의한다. 

```css
body { 
  background-color: red;
}
```
src/App.svelete 파일에 css 파일을 import한다. 
```html
<script>
	import '../static/my.css'
	export let name;
</script>
```

배경색이 빨간색으로 변경된 것을 브라우저에서 확인할 수 있다. 

> node_modules의 css를 가져돌 때 node_modules의 전체 경로를 사용할 필요는 없다. 





## References
[Using CSS-in-JS with Svelte](https://svelte.dev/blog/svelte-css-in-js)    
[How to import CSS files to Svelte](https://www.educative.io/edpresso/how-to-import-css-files-to-svelte)     