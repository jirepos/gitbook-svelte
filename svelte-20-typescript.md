# typescript 사용


## lang 속성 사용 
script 태그의 lang 속성에 'ts'를 설헝하면 된다. 
```html
<script lang='ts'>
</script>
```

## typescript  변환 
Javascript를 사용하는 기준으로 템플릿이 작성되어 있는데, TypeScript로 변환할 수 있는 방안이 템플릿안에 제공되는데 scripts 폴더 안에 setupTypeScript.js이다.

아래와 같이 명령을 실행하면, 템플릿 구조가 TypeScript를 사용하는 것으로 변경된다.
```
node scripts/setupTypeScript.js
```
그런다음 필요한 것을 설치한다.
```
npm install
```

