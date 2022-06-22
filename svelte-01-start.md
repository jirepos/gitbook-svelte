# 시작하기 

스벨트 툴킷을 사용하여 프로젝트를 생성한 다음 실행해 볼 것이고 스벨트의 구조에 대해서 살펴보겠다. 

## 설치하기 
```
npx degit sveltejs/template my-svelte-project
cd my-svelte-project
npm install
npm run dev
```

Svelte 프로젝트를 빠르게 만들 수 있는 두 가지. 
* REPL
* digit 


**REPL**    
REPL(Real Eval Print Loop):  사용자의 입력을 받아 실행하고 결과를 사용자에게 보여주는 프로그래밍 환경

> Svelte는 빌드 타임에 반응형이 결정됩니다. 이런 특징은 런타임 동안에 변경사항을 찾아야 하는 오버헤드를 줄여 속도를 개선하는 장점이 있지만, CDN 등으로 Svelte를 사용할 수 없는 단점도 있다. Svelte는 이런 단점을 보완하기 위해 자체적으로 REPL을 제공하는 것으로 보인다. 

**digit**    

Svelte는 template와 template-webpack 두 가지 템플릿 프로젝트를 제공한다. 

* template : sveltejs/template은 rollup 번들러를 사용하는 템플릿
* webpack template : sveltejs/template-webpack은 webpack을 사용하는 템플릿



## 프로젝트 구조
```
📁project_root
  📁public
    📄favicon.png
    📄global.css
    📄index.html
  📁scripts
    📄setupTypeScript.js 
  📁src
    📄App.svelte
    📄main.js 
  📄.gitignore
  📄package.json
  📄README.md
  📄rollup.config.js 
```  


## src/main.js
```jsx
import App from './App.svelte';

const app = new App({
	target: document.body,
	props: {
		name: 'world'
	}
});

export default app;
```

## src/App.svelte 
```html
<script>
	export let name;
</script>

<main>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</main>

<style>
  ...생략...
</style>
```



## public/index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset='utf-8'>
	<meta name='viewport' content='width=device-width,initial-scale=1'>

	<title>Svelte app</title>

	<link rel='icon' type='image/png' href='/favicon.png'>
	<link rel='stylesheet' href='/global.css'>
	<link rel='stylesheet' href='/build/bundle.css'>

	<script defer src='/build/bundle.js'></script>
</head>

<body>
</body>
</html>
```


## rollup.config.js
```jsx
export default {
	input: 'src/main.js',
	output: {
		sourcemap: true,
		format: 'iife',
		name: 'app',
		file: 'public/build/bundle.js'
	},
    ... 생략 ...
```




