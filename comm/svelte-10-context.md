# Context API 

중첩된 컴포넌트 간에 props로 값을 전달하는데 어려움이 있을 수 있는데 이런 경우에는 context  API를 사용하여 값을 전달하거 가져올 수 있다. 


부모 컴포넌트에서 setContext(key, value)를 사용하여 값을 설정한다. 

```html
<script>

  import {setContext} from 'svelte'
  import ContextSub from './ContextSub.svelte'

  const employee = { 
    name: 'John', 
    age: 30 
  }

setContext('employee', employee)


</script>

<h1>Context Main</h1>

<ContextSub/>
```

자식 컴포넌트에서는 getContext(key)를 사용하여 값을 꺼내 온다. 
```html
<script>
  import {getContext} from 'svelte'

const employee = getContext('employee')
</script>

<h1>Context Sub</h1>
<h2>{employee.name}, { employee.age}</h2>
```