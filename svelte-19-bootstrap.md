# BootStrap 사용하기 


## 번들러를 사용

Webpack이나 다른 번들러를 사용하여 프로젝트에 Bootstrap을 포함하는 방법을 알아본다. 

### BootStrap 설치 

```
npm install bootstrap
```

> const bootstrap = require('bootstrap') 또는 import bootstrap from 'bootstrap'는 bootstrap 객체에 모든 Bootstrap의 플러그인을 불러옵니다. bootstrap 모듈은 자체에서 모든 플러그인을 내보냅니다. 패키지 최부모 디렉토리 아래에 있는 /js/dist/*.js 파일을 불러옴으로서 Bootstrap의 플러그인을 직접 독립적으로 불러올 수 있습니다.


### JavaScript 불러오기 

앱의 진입점 (일반적으로 index.js 또는 app.js)에 다음 줄을 추가하여 Bootstrap의 JavaScript를 불러온다. 

```
// You can specify which plugins you need
import { Tooltip, Toast, Popover } from 'bootstrap';
```

또는 플러그인이 전부 필요하지 않다면 필요에 따라서 개별적으로 플러그인을 불러올 수 있다:
```
import Alert from 'bootstrap/js/dist/alert';
```



## 스타일 불러오기 
### 컴파일된 CSS 불러오기
프로젝트의 진입점에 이 줄을 추가하여 Bootstrap의 즉시 사용 가능한 CSS를 사용할 수도 있습니다:
```
import 'bootstrap/dist/css/bootstrap.min.css';
```



### 사전 컴파일된 CSS 불러오기 
먼저 _custom.scss를 직접 만들고 이를 통해 기본 제공 커스텀 변수를 재정의한다. 그런 다음에 메인 Sass 파일을 사용하여 커스텀 변수를 가져온 다음 Bootstrap을 사용한다. 


```
@import "custom";
@import "~bootstrap/scss/bootstrap";
```

[사전 컴파일된 CSS 사용하기](https://getbootstrap.kr/docs/5.1/getting-started/webpack/)      


UX팀에서 실제로 작업하는 방법은 [Saas](https://getbootstrap.kr/docs/5.1/customize/sass/)에 자세히 나와 있다. 여기를 참고한다. 













## References
[설치](https://getbootstrap.kr/docs/5.1/getting-started/download/#npm)     


[Scss](https://tongere.hashnode.dev/svelte-adding-scss-and-bootstrap-to-your-project)     


