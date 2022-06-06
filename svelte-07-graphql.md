# GraphQL 

## fetch 함수 사용 
```html
<script>
	
	//import { ApolloClient, InMemoryCache, gql } from "@apollo/client";
  //import { setClient, query } from "svelte-apollo";
	// const client = new ApolloClient({
	// 	cache: new InMemoryCache(), 
  //   uri: 'http://localhost:80/graphql'
  // });
  // setClient(client);
	//const employee = query(EMPLOYEE_QUERY);

	let resData;
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
	// .then( (data) => {
	// 	console.log(data.data.employee.empName)
	// })
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

이거 정리

https://hasura.io/learn/graphql/svelte-apollo/intro-to-graphql/1-architecture/



## svelte-apollo 사용 



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



## References

https://www.npmjs.com/package/svelte-apollo-client