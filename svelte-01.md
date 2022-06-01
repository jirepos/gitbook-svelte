# 시작하기 

## 설치하기 
```
npx degit sveltejs/template my-svelte-project
cd my-svelte-project
npm install
npm run dev
```

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




