# BootStrap 사용하기 



## 설치 
```
npm install bootstrap
```


> const bootstrap = require('bootstrap') 또는 import bootstrap from 'bootstrap'는 bootstrap 객체에 모든 Bootstrap의 플러그인을 불러옵니다. bootstrap 모듈은 자체에서 모든 플러그인을 내보냅니다. 패키지 최부모 디렉토리 아래에 있는 /js/dist/*.js 파일을 불러옴으로서 Bootstrap의 플러그인을 직접 독립적으로 불러올 수 있습니다.


[사전 컴파일된 CSS 사용하기](https://getbootstrap.kr/docs/5.1/getting-started/webpack/)      




## CSS Preprocessor 란?


HTML, CSS를 다루는 분이라면 한 번은 들어봤을 Sass, Less 등이 있습니다.
이 친구들은 CSS 전(예비)처리기 입니다.
보통 CSS Preprocessor 라고 부릅니다.

CSS가 동작하기 전에 사용하는 기능으로,
웹에서는 분명 CSS가 동작하지만 우리는 CSS의 불편함을 이런 확장 기능으로 상쇄할 수 있습니다.

### 어떻게 사용하나요?
CSS는 불편하니 일단 배제하고 우선 전처리기로 작성(코딩)합니다.

전처리기는 CSS 문법과 굉장히 유사하지만 선택자의 중첩(Nesting)이나 조건문, 반복문, 다양한 단위(Unit)의 연산 등… 표준 CSS 보다 훨씬 많은 기능을 사용해서 편리하게 작성할 수 있습니다.


### 컴파일은 어떻게 하나요?


전처리기 종류마다 방법이 조금씩 다르고 여러 방식을 제공합니다.
보통의 경우 컴파일러(Compiler)가 필요합니다.
우리는 이제 Sass(SCSS)를 알아볼 것이고 컴파일 방법에 대해서도 같이 살펴보겠습니다.


### Sass와 SCSS는 차이점은 뭔가요?


Sass(Syntactically Awesome Style Sheets)의 3버전에서 새롭게 등장한 SCSS는 CSS 구문과 완전히 호환되도록 새로운 구문을 도입해 만든 Sass의 모든 기능을 지원하는 CSS의 상위집합(Superset) 입니다.
즉, SCSS는 CSS와 거의 같은 문법으로 Sass 기능을 지원한다는 말입니다.

더 쉽고 간단한 차이는 {}(중괄호)와 ;(세미콜론)의 유무입니다.
아래의 예제를 비교해 보세요.


**Sass:**

```
.list
  width: 100px
  float: left
  li
    color: red
    background: url("./image.jpg")
    &:last-child
      margin-right: -10px
```



**SCSS:**
```
.list {
  width: 100px;
  float: left;
  li {
    color: red;
    background: url("./image.jpg");
    &:last-child {
      margin-right: -10px;
    }
  }
}
```


Sass는 선택자의 유효범위를 ‘들여쓰기’로 구분하고, SCSS는 {}로 범위를 구분합니다.
Sass 방식과 SCSS 방식 중 어떤 방식이 마음에 드세요?


[Saas 완전정복](https://heropy.blog/2018/01/31/sass/)      




### 컴파일 방법








## References
[설치](https://getbootstrap.kr/docs/5.1/getting-started/download/#npm)     


[Scss](https://tongere.hashnode.dev/svelte-adding-scss-and-bootstrap-to-your-project)     


