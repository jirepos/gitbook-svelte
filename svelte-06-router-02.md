# Router


## 설치
```
npm install svelte-spa-router
```

```
📁project_root
  📁src
    📁views
      📁hello
        📄Hello.svelte 
        📄HelloSub.svelte 
      📄Main.svelte
      📄List.svelte
      📄Detail.svelte
      📄NotFound.svelte
    📄App.svelte
    📄main.js 
    📄routes.js
```    

## 시작하기 

### routes.js
Route는 반드시 '/'로 시작되어야 한다. * 는 모든 경로가 해당된다. 

```jsx
import Main from './views/Main.svelte';
import List from './views/List.svelte';
import Detail from './views/Detail.svelte';
import NotFound from './views/NotFound.svelte';
import Hello from './views/hello/Hello.svelte';
// 아래는 svelte-spa-router를 사용할 때 
// http:/localhost:8080/#/list 와 같이 라우팅 경로 앞에 #이 붙어야 한다. 
// 라우팅 경로 설정 
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
  import routes from './routes.js'   // 라우트정의 파일 import 

</script>
  <!-- 여기에 routes.js에서 정의한 라우트 이름에 해당하는 컴포너트가 표시된다. -->
  <!-- routes 프러퍼티 전달-->
	<Router {routes} />
```

이제 다음과 같이 화면을 호출할 수 있다. 

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

// src/routes.js에는 다음과 같이 정의되어 있음 
// export default  {
//   "/hello": Hello, 
//   "/hello/*": Hello, 
// }
// http://localhost/#/hello와 같이 호출할 수 있고 
// http://localhost/#/hello/sub와 같이 호출할 수 있다.


// 접두사 설정 
const prefix = "/hello"; 
// 내포된 라우트 정의
const routes = {
  '/sub': HelloSub,
}

</script>
<h1>Hello</h1>
<!-- routes와 prefix를 넘겨준다.  -->
<Router {routes} {prefix} />
```


## 동적 임포트 

### routes.js
```jsx

// component를 동적으로 임포트하기 위해 
import {wrap} from 'svelte-spa-router/wrap'


import Main from './views/Main.svelte';
import Detail from './views/Detail.svelte';
import NotFound from './views/NotFound.svelte';

// Dynamic import를 사용하는 방법
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
        // minify할 것인지 여부
        compact: false, 
        format: 'es',   // 동적 임포트 사용시
        // 동적으로 컴포넌트를 로드하고 번들 파일을 분리할 때 사용한다.
        inlineDynamicImports: true, 
        name: 'app',
        // file은 동적 컴포넌트 로드일때는 사용하지 못하고 dir을 사용한다. 
        //file: 'public/build/bundle.js',
        dir: 'public/build',
        entryFileNames: '[name]-[format].js',
        chunkFileNames: '[name]-[format]-[hash].js',
    },
```    


### index.html
type=module로 설정해야 한다. 
```html
<script defer src="/build/main-es.js" type="module"></script>
```


## parameters

### Hello.svelte 
```html
<script>
import Router from 'svelte-spa-router'
import HelloSub from './HelloSub.svelte'

// src/routes.js에는 다음과 같이 정의되어 있음 
// export default  {
//   "/hello": Hello, 
//   "/hello/*": Hello, 
// }
// http://localhost/#/hello와 같이 호출할 수 있고 
// http://localhost/#/hello/sub와 같이 호출할 수 있다.


// 접두사 설정 
const prefix = "/hello"; 
// 내포된 라우트 정의
const routes = {
  '/sub': HelloSub,
  '/sub/:id': HelloSub   // parameter 정의 
}

</script>
<h1>Hello</h1>
<!-- routes와 prefix를 넘겨준다.  -->
<Router {routes} {prefix} />
```

## HelloSub.svelte
```html
<script>
export let params = {};  // 파라미터 정의 

</script>
<h1>Hello Sub</h1>
<!--파리미터 사용-->
{params.id} 
```

