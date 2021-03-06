# Router


## ์ค์น
```
npm install svelte-spa-router
```

```
๐project_root
  ๐src
    ๐views
      ๐hello
        ๐Hello.svelte 
        ๐HelloSub.svelte 
      ๐Main.svelte
      ๐List.svelte
      ๐Detail.svelte
      ๐NotFound.svelte
    ๐App.svelte
    ๐main.js 
    ๐routes.js
```    

## ์์ํ๊ธฐ 

### routes.js
Route๋ ๋ฐ๋์ '/'๋ก ์์๋์ด์ผ ํ๋ค. * ๋ ๋ชจ๋  ๊ฒฝ๋ก๊ฐ ํด๋น๋๋ค. 

```jsx
import Main from './views/Main.svelte';
import List from './views/List.svelte';
import Detail from './views/Detail.svelte';
import NotFound from './views/NotFound.svelte';
import Hello from './views/hello/Hello.svelte';
// ์๋๋ svelte-spa-router๋ฅผ ์ฌ์ฉํ  ๋ 
// http:/localhost:8080/#/list ์ ๊ฐ์ด ๋ผ์ฐํ ๊ฒฝ๋ก ์์ #์ด ๋ถ์ด์ผ ํ๋ค. 
// ๋ผ์ฐํ ๊ฒฝ๋ก ์ค์  
export default  {
  // http://localhost/
  "/": Main,
  // http://localhost/#/list 
  "/list": List,
  // http://localhost/#/detail
  "/detail": Detail,
  // http://localhost/#/hello
  "/hello": Hello, 
  // http://localhost/#/hello/sub
  "/hello/*": Hello, 
  "*": NotFound,
}

```

### App.svelte
```html
<script>
  import Router from 'svelte-spa-router'
  import routes from './routes.js'   // ๋ผ์ฐํธ์ ์ ํ์ผ import 

</script>
  <!-- ์ฌ๊ธฐ์ routes.js์์ ์ ์ํ ๋ผ์ฐํธ ์ด๋ฆ์ ํด๋นํ๋ ์ปดํฌ๋ํธ๊ฐ ํ์๋๋ค. -->
  <!-- routes ํ๋ฌํผํฐ ์ ๋ฌ-->
	<Router {routes} />
```

์ด์  ๋ค์๊ณผ ๊ฐ์ด ํ๋ฉด์ ํธ์ถํ  ์ ์๋ค. 

```
http://localhost/
http://localhost/#/list 
http://localhost/#/detail
http://localhost/#/hello
http://localhost/#/hello/sub
```

### Nested Routes

#### Hello.svelte
```html
<script>
import Router from 'svelte-spa-router'
import HelloSub from './HelloSub.svelte'

// src/routes.js์๋ ๋ค์๊ณผ ๊ฐ์ด ์ ์๋์ด ์์ 
// export default  {
//   "/hello": Hello, 
//   "/hello/*": Hello, 
// }
// http://localhost/#/hello์ ๊ฐ์ด ํธ์ถํ  ์ ์๊ณ  
// http://localhost/#/hello/sub์ ๊ฐ์ด ํธ์ถํ  ์ ์๋ค.


// ์ ๋์ฌ ์ค์  
const prefix = "/hello"; 
// ๋ดํฌ๋ ๋ผ์ฐํธ ์ ์
const routes = {
  '/sub': HelloSub,
}

</script>
<h1>Hello</h1>
<!-- routes์ prefix๋ฅผ ๋๊ฒจ์ค๋ค.  -->
<Router {routes} {prefix} />
```


## ๋์  ์ํฌํธ 

### routes.js
```jsx

// component๋ฅผ ๋์ ์ผ๋ก ์ํฌํธํ๊ธฐ ์ํด 
import {wrap} from 'svelte-spa-router/wrap'


import Main from './views/Main.svelte';
import Detail from './views/Detail.svelte';
import NotFound from './views/NotFound.svelte';

// Dynamic import๋ฅผ ์ฌ์ฉํ๋ ๋ฐฉ๋ฒ
export default  {
  // http://localhost/
  "/": Main,
  // http://localhost/#/list 
  "/list": wrap({
    asyncComponent: () =>  import('./views/List.svelte')
  }),
  // http://localhost/#/detail
  "/detail": Detail,
  "/hello": wrap({
    asyncComponent: () =>  import('./views/hello/Hello.svelte')
  }),
  "/hello/*": wrap({
    asyncComponent: () =>  import('./views/hello/Hello.svelte')
  }),
  "*": NotFound,
}
```
### rollup.config.js
```jsx
    output: {
        sourcemap: true,
        //format: 'iife',
        // minifyํ  ๊ฒ์ธ์ง ์ฌ๋ถ
        compact: false, 
        format: 'es',   // ๋์  ์ํฌํธ ์ฌ์ฉ์
        // ๋์ ์ผ๋ก ์ปดํฌ๋ํธ๋ฅผ ๋ก๋ํ๊ณ  ๋ฒ๋ค ํ์ผ์ ๋ถ๋ฆฌํ  ๋ ์ฌ์ฉํ๋ค.
        inlineDynamicImports: true, 
        name: 'app',
        // file์ ๋์  ์ปดํฌ๋ํธ ๋ก๋์ผ๋๋ ์ฌ์ฉํ์ง ๋ชปํ๊ณ  dir์ ์ฌ์ฉํ๋ค. 
        //file: 'public/build/bundle.js',
        dir: 'public/build',
        entryFileNames: '[name]-[format].js',
        chunkFileNames: '[name]-[format]-[hash].js',
    },
```    


### index.html
type=module๋ก ์ค์ ํด์ผ ํ๋ค. 
```html
<script defer src="/build/main-es.js" type="module"></script>
```


## parameters

### Hello.svelte 
```html
<script>
import Router from 'svelte-spa-router'
import HelloSub from './HelloSub.svelte'

// src/routes.js์๋ ๋ค์๊ณผ ๊ฐ์ด ์ ์๋์ด ์์ 
// export default  {
//   "/hello": Hello, 
//   "/hello/*": Hello, 
// }
// http://localhost/#/hello์ ๊ฐ์ด ํธ์ถํ  ์ ์๊ณ  
// http://localhost/#/hello/sub์ ๊ฐ์ด ํธ์ถํ  ์ ์๋ค.


// ์ ๋์ฌ ์ค์  
const prefix = "/hello"; 
// ๋ดํฌ๋ ๋ผ์ฐํธ ์ ์
const routes = {
  '/sub': HelloSub,
  '/sub/:id': HelloSub   // parameter ์ ์ 
}

</script>
<h1>Hello</h1>
<!-- routes์ prefix๋ฅผ ๋๊ฒจ์ค๋ค.  -->
<Router {routes} {prefix} />
```

## HelloSub.svelte
```html
<script>
export let params = {};  // ํ๋ผ๋ฏธํฐ ์ ์ 

</script>
<h1>Hello Sub</h1>
<!--ํ๋ฆฌ๋ฏธํฐ ์ฌ์ฉ-->
{params.id} 
```

