# GraphQL 

fetch()  함수를 사용하여 서버에 요청하는 방법을 살펴본다.  svelte-apollo를 사용하려고 했지만 동작하지 않아서 일단 기록만 남겨 두었다. 


## fetch 함수 사용 
### 예제 1
awat를 사용한 예제이다. 
```html
<script>
	$: employee = fetch('http://localhost/graphql', {
		method: 'POST',
		headers: {
			'Content-Type': 'application/json'
		},
		body: JSON.stringify({
			query: `{
				employee {
					empId, 
					empName 
				}
			}`
		})
	})
	.then( (response) => response.json() )
</script>
<main>
		{#await employee}
			<p>...Loading</p>
		{:then employee } 
			{employee.data.employee.empName}
		{:catch error}
			<p>오류가 발생했습니다.</p>
		{/await}
</main>
<style>
</style>
```


### 예제 2 
변수가 없을 때의 쿼리 예이다. 
```html
<script>
  let employees = fetch('http://localhost/graphql', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        query: `{
          employee {
            empId, 
            empName ,
            email
          }
        }`
      })
    })
    .then( (response) => response.json() )
  let employeearr; 
  employees.then(  (data) => {
      console.log(data.data.employee);
      employeearr = data.data.employee; 
  }); 
</script>

<ul>
  {#if employeearr}
    {#each employeearr as item}
      <li>{item.empName} :  {item.email}</li>
    {/each}
  {/if}
</ul>
```


### 예제 3 
변수가 있는 경우 쿼리와 변수를 분리하여 요청한다. 
```html
<script>
   const query =`query ATN_CARDS($empId:String) {
      attendCards(empId: $empId) {
          empId,
          atndDate,
          atndYear,
          atndTime 
      }
   }`;
  const variables = { empId: 'M164' };

  let attendcards = fetch('http://localhost/graphql', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({  query, variables })
    })
    .then( (response) => response.json() )
  let attendcardarr; 
  attendcards.then(  (data) => {
      console.log(data.data.attendCards);
      attendcardarr = data.data.attendCards; 
  }); 
</script>


<ul>
  {#if attendcardarr}
    {#each attendcardarr as item}
      <li>{item.empId} :  {item.atndDate} : {  item.atndTime} </li>
    {/each}
  {/if}
</ul>
```


이거 정리

https://hasura.io/learn/graphql/svelte-apollo/intro-to-graphql/1-architecture/





## svelte-apollo 사용 
Apollo Client를 사용한 예제는 동작이 되지 않아서 일단 제외한다. 테스트를 위해 작업한 기록만 우선 남긴다. 



설치는 아래 URL 
https://www.npmjs.com/package/svelte-apollo


node.js는 최신 버전으로



client setup은 아래와 같이 
```
<script>
	import { ApolloClient } from "@apollo/client";
  import { setClient } from "svelte-apollo";
	const client = new ApolloClient({
    uri: 'http://localhost:80/graphql',
  });

  setClient(client);

</script>
```


ApolloClient에 대한 명세는 아래에서 참고 

https://www.apollographql.com/docs/react/api/core/ApolloClient 



github 공식 페이지에도 예제가 있어
https://github.com/timhall/svelte-apollo



쿼리는 

```
<script>
  import { query } from "svelte-apollo";  # query 임포트하고 
  import { SEARCH_BY_AUTHOR } from "./queries"; # 쿼리 불러오고 

  export let author;
  let search = "";

  # 쿼리 실행 : 변수 설정하고 
  const books = query(SEARCH_BY_AUTHOR, {
    variables: { author, search },
  });

```

```
<script>
  import { query } from "svelte-apollo";
  import { GET_BOOKS } from "./queries";

  const books = query(GET_BOOKS, {
    // variables, fetchPolicy, errorPolicy, and others
  });

  function reload() {
    books.refetch();
  }
</script>
```



쿼리는 gql로 작성 
grave accetn로 감싼다. 

```
  const ADD_TODO = gql`
    mutation($text: String!) {
      addTodo(text: $text) {
        id
        done
        text
      }
    }
  `;
```


### sample
아래 코드는 동작하지 않는다. 
```
<script>
	
	import { ApolloClient, InMemoryCache, gql } from "@apollo/client";
  import { setClient, query } from "svelte-apollo";
	const client = new ApolloClient({
		cache: new InMemoryCache(), 
    uri: 'http://localhost:80/graphql'
  });
  setClient(client);

	let employee  = query(gql`query myEmployee{
				employee {
					empId,
					empName 
				}
			}`); 
	
	
</script>
<main>
	{#if employee}
	  {#await employee}
		    <p>....loading</p>
		{:then data}
			{data.employee.empName }
		{:catch error}
			<p>error occured</p>

		{/await}
		
	{/if}
</main>
```


## References

https://www.npmjs.com/package/svelte-apollo-client