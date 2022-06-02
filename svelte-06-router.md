# Svelte Router SPA
Svelte에서 Router를 사용하는 방법을 설명한다.  상세한 내용은 [Svelte Router SPA](https://www.npmjs.com/package/svelte-router-spa)을 참조한다. 


## Installation 

```
npm i svelte-router-spa
```

## Usage 
package.json 에서 -s 를 sirv public에 추가한다. 
```json
"start": "sirv public -s"
```


### 기초 라우팅
src 디렉터리 아래 views 디렉터리를 만들고 그 아래 Detail, List, Main 3개의 컴포넌트를 만든다.  
```
📁project_root
  📁src
    📁views
      📄Detail.svelte 
      📄List.svelte 
      📄Main.svelte 
    📄App.svelte
    📄main.js 
```    
src 디렉터리에 routes.js를 만든다.  만든 컴포넌트들을 임포트한다.  
```jsx
import Main from './views/Main.svelte';
import List from './views/List.svelte';
import Detail from './views/Detail.svelte';


const routes = [
  {
    name:"/",
    component: Main,
  },
  {
    name:"list",
    component: List,
  },
  {
    name:"detail",
    component: Detail,
  },
]

export { routes }
```

App.svelte을 다음과 같이 작성한다.
```html
<script>
	
	import { Router } from 'svelte-router-spa'
  import { routes } from './routes'
	
	import '~/my.css'
</script>

<main>
	<Router {routes} />
</main>

```

서버를 실행한다. 

* localhost:8080/ 이면 Main 컴포넌트가 표시될 것이고
* localhost:8080/list 이면, List 컴포넌트가 표시될 것이고
* localhost:8080/detail이면, Detail 컴포넌트가 표시될 것이다. 



### Nested 라우팅 
user 디렉터리를 만들고 UserLayout, UserShow 컴포넌트를 만든다. 
```
📁project_root
  📁src
    📁views
      📁user
        📄UserLayout.svelte 
        📄UserShow.svelte 
    📄App.svelte
    📄main.js 
```    

routes.js를 다음과 같이 수정한다. nestedRoutes를 사용하여 Nested route를 정의한다. 

```jsx
import UserLayout from './views/user/UserLayout.svelte'
import UserShow from './views/user/UserShow.svelte'


const routes = [
  {
    name:"/",
    component: Main,
  },
  {
    name:"user",
    component: UserLayout,
    nestedRoutes: [
      { name: "show", component: UserShow }
    ]
  },  
]

export { routes }
```

UserLayout 컴포넌트를 다음과 같이 작성한다. 

```html
<script>
  import { Route } from "svelte-router-spa";
 export let currentRoute;
 const params = {}
</script>

<h1>User</h1>
<Route {currentRoute} {params} />
```
UserShow 컴포넌트를 다음과 같이 작성한다. 


```html
<script></script>
<h1>User Show</h1>
```

* http://localhost:8080/user 이면 UserLayout 컴포넌트가 표시된다. 
* http://localhost:8080/user/show 이면, UserLaytout 컴포넌트 안에 UserShow 컴포넌트가 표시된다. 



### 파라미터 전달 
routes.js에서 show/:id와 같이 콜론과 이름을 추가한다. 
```jsx
  {
    name:"user",
    component: UserLayout,
    nestedRoutes: [
      { name: "show/:id", component: UserShow }
    ]
  },  
```  

UserShow 컴포넌트를 다음과 같이 수정한다.  currentRoute를 export하고 파라미터를 namedParams을 사용하여 id의 값을 구할 수 있다. 
```html
<script>
  export let currentRoute
</script>
<h1>User Show id: {currentRoute.namedParams.id}</h1>
```

브라우저에서 다음과 같이 호출한다. 

```
http://localhost:8080/user/show/batman
```



### default 라우팅 
component의 값을 비워두면  name='index'로 설정된 컴포넌트가 라우팅 된다. .routers.js를 다음과 같이 수정한다. 
 ```jsx
   {
    name:"user",
    component: '',
    nestedRoutes: [
      { name: 'index', component: UserLayout},
      { name: "show/:id", component: UserShow }
    ]
  },  
```
브라우저에서 다음과 같이 입력하면 UserLayout 컴포넌트가 표시될 것이다. 
```
http://localhost:8080/user
```



### redirect 
사용자 페이지에 접근했을 때 권한이 없을 때 로그인 페이지로 리다이렉트 시킨다고 가정하자. 

Login 컴포넌트를 만든다. 
```html
<script></script>
<h1>Login</h1>
```
routes.js에 Login 컴포넌트를 임포트하고 라우트를 추가한다. 
```jsx
import Login from './views/Login.svelte';


const routes = [
  {
    name:"login",
    component: Login,
  },
]
```

User 페이지에 접근하면 권한을 체크할 함수를 만든다. routes.js를 다음과 같이 수정한다. 
```jsx
function isAccessable() {
  // return true or false
  return false; 
}
```

이제 user 라우트에 rediret를 다음과 같이 추가한다.  onlyIf 속성을 살펴보면, user 페이지에 접근하면 isAccessable 함수를 실행하고 결과가 false이면 /login으로 리다이렉트한다. 
```
  {
    name:"user",
    component: '',
    onlyIf: { guard: isAccessable, redirect: '/login' },
    nestedRoutes: [
      { name: 'index', component: UserLayout},
      { name: "show/:id", component: UserShow }
    ]
  },  
```

브라우저에서 user 페이지를 호출하면 login 페이지가 표시될 것이다. 

**조건없이 리다이렉트하기**     
user에 접근할 때  조건없이 login으로 리다이렉트하도록 하려면 redirectTo를 사용한다. 
```
  {
    name:"user",
    component: '',
    redirectTo: 'login',
    nestedRoutes: [
      { name: 'index', component: UserLayout},
      { name: "show/:id", component: UserShow }
    ]
  },  
```

브라우저에서 user 페이지를 호출하면 login 페이지가 표시될 것이다. 



## API

### Router 
```jsx
import { Router } from 'svelte-router-spa'
```
이것은 라우트와 렌더링되어야 하는 라우트에 대한 정보를 담고 있기 때문에 다른 콘텐츠보다 먼저 포함되어야 하는 주요 구성 요소이다. 


## Route
```jsx
import { Route } from 'svelte-router-spa'
```
이 구성 요소는 레이아웃을 생성하는 경우에만 필요하다. 자식 구성 요소 또는 자식 레이아웃에 대한 콘텐츠 렌더링을 재귀적으로 처리한다. 필요한 만큼 중첩된 레이아웃을 가질 수 있다.

현재 경로에 대한 정보는 속성으로 수신되므로 currentRoute를 정의하고 내보내야 한다. .


currentRoute에는 현재 경로와 자식 경로에 대한 모든 정보가 있다. 

```html
<script>
  import { Route } from 'svelte-router-spa'
  import TopHeader from './top_header.svelte'
  import FooterContent from './footer_content.svelte'
  export let currentRoute

  const params = { validCheck: true }
</script>

<div class="app">
  <TopHeader />
  <section class="section">
    <Route {currentRoute} {params} />
    <p>Route params are: {currentRoute.namedParams} and {currentRoute.queryParams}</p>
  </section>
  <FooterContent />
</div>
```

**currentRoute**    
* Properties
  * name
  * component
  * layout
  * queryParams
  * namedParams
  * childRoute
  * language


다음과 같이 현재 경로에 대한 모든 정보를 얻을 수 있다.
```html
<script>
  export let currentRoute
</script>

<h1>Your name is: {currentRoute.namedParams.name}</h1>
```


### navigateTo
```jsx
import { navigateTo } from 'svelte-router-spa'
```
* Params
  * route name (Required) String A valid route to navigate to.
  * language (Optional) String A language to convert the route to.


NavigationTo를 사용하면 브라우저 URL 및 기록을 업데이트하는 앱 내부에서 경로를 프로그래밍 방식으로 탐색할 수 있다.


```html
  // Example route
  {
    name: '/setup',
    component: 'SetupComponent',
    lang: { es: 'configuracion' }
  }
```
```jsx
navigateTo('setup') // Will redirect to /setup

navigateTo('setup', 'es') // Will redirect to /configuracion
```  

## References

[Svelte Router SPA](https://www.npmjs.com/package/svelte-router-spa)    